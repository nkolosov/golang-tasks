# Cache Middleware с Write-Behind и TTL
Необходимо реализовать middleware для кэширования данных, которые хранятся в базе данных (БД). 
Кэш работает в памяти приложения и служит прослойкой между бизнес-логикой и БД. 
Данные в кэше считаются "рабочими" (основными), а БД выступает как постоянное хранилище для восстановления.

```go
package cache

import "context"

type Cache[T any] interface {
    // Get возвращает значение по ключу
    // Если ключ не найден в кэше или TTL истек - загружает из БД
    // Блокирует вызов, пока данные загружаются (single flight)
    Get(ctx context.Context, key string) (T, error)
    
    // Set обновляет значение в кэше и помечает его как "грязный" для записи в БД
    // Запись в БД происходит асинхронно через batch writer
    Set(ctx context.Context, key string, value T) error
    
    // Delete удаляет ключ из кэша
    // Должен также удалить из БД (синхронно или асинхронно)
    Delete(ctx context.Context, key string) error
	
	// Close корректно завершает работу кеша и приводит систему к консистентному состоянию
	Close() error
}
```

## TTL (Time-To-Live) механика
- Каждая запись в кэше имеет свой TTL
- При истечении TTL запись считается "просроченной"
- При попытке получить просроченную запись - нужно обновить её из БД
- Реализовать механизм фоновой очистки просроченных записей (lazy expiration + background cleaner)
- TTL можно задавать для всех записей глобально или индивидуально (на выбор)

## Write-Behind стратегия
- `Set()` обновляет данные в кэше мгновенно
- Запись в БД происходит асинхронно с накоплением изменений
- Реализовать batch writer, который:
  - Накапливает изменения (upsert/delete) в буфере
  - Отправляет батч в БД при выполнении любого из условий:
    - Накопилось N изменений (например, 100)
    - Прошло T времени (например, 5 секунд)
    - Если БД недоступна - батч должен сохраниться и повторяться с экспоненциальной задержкой

## Graceful Shutdown
При получении сигнала завершения работы:
- Останавливаем принятие новых запросов
- Ждем завершения всех текущих операций Get/Set
- Принудительно сбрасываем все "грязные" записи из кэша в БД (даже если батч не полный)
- Убеждаемся, что все данные записались, прежде чем завершить приложение

Для этого реализовать метод `Close() error`

## Single Flight для предотвращения "набегания"
Если несколько горутин одновременно запрашивают отсутствующий в кэше ключ:
- Только одна горутина делает запрос к БД
- Остальные ждут результат этой горутины

Это предотвращает лишние запросы к БД (cache stampede)

## Консистентность
Обновления через Set должны быть видны всем читателям сразу (после записи в кэш)

# Дополнительная информация
```go
package cache

import "context"

type Database[T any] interface {
    // BatchUpsert массово обновляет или вставляет записи
    // Возвращает список ключей, которые не удалось обновить
    BatchUpsert(ctx context.Context, entries map[string]T) (failedKeys []string, err error)
    
    // BatchDelete массово удаляет записи
    BatchDelete(ctx context.Context, keys []string) (failedKeys []string, err error)
    
    // Get загружает одну запись из БД
    Get(ctx context.Context, key string) (T, error)
    
    // Close закрывает соединение с БД
    Close() error
}
```

Пример использования
```go
package main

import (
	"context"
	"os/signal"
)

func main() {
    db := NewPostgresDB("postgres://localhost:5432/mydb")
    
    cache := NewWriteBehindCache(Config{
        DB:            db,
        DefaultTTL:    5 * time.Minute,
        BatchSize:     100,
        FlushInterval: 5 * time.Second,
        MaxRetries:    3,
    })
    
    ctx := context.Background()
    
    // Первый запрос - загрузится из БД
    value, err := cache.Get(ctx, "user:123")
    
    // Обновление - пишем в кэш, в БД позже
    err = cache.Set(ctx, "user:123", newValue)
    
    // Graceful shutdown
    sigChan := make(chan os.Signal, 1)
    signal.Notify(sigChan, syscall.SIGINT, syscall.SIGTERM)
    <-sigChan
    
    if err := cache.Close(); err != nil {
        log.Fatal("Failed to flush cache:", err)
    }
}
```