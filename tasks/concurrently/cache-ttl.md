# Cache с TTL
Написать модуль для кеша с TTL для каждого отдельного ключа

## Описание задачи
Реализовать интерфейс для кеша с заданным TTL для элементов

У кеша должны быть реализованы следующие методы:
- `Get` возвращает элемент по его ключу
- `Set` сохраняет значение в кеше по его ключу с заданным TTL
- `Clear` очищает кеш

## Пример использования
```go
c := NewCache()
c.Set("key-1", value1, 1 * time.Second)
c.Set("key-2", value2, 2 * time.Second)

v = c.Get("key-1") // value1
v = c.Get("key-2") // value2

time.Sleep(1 * time.Second)

v = c.Get("key-1") // not found
v = c.Get("key-2") // value2

time.Sleep(1 * time.Second) 
v = c.Get("key-2") // not found
```