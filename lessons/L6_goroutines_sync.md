# Занятие 6. Синхронизация горутин.

Каналы. Внутреннее устройство каналов. Аксиомы каналов.

Инструменты синхронизации. Пакет `sync`. Пакет `/x/sync`. Пакет `synctest`

Мультиплексирование (`select`).

Контексты. Пакет `context`.

Мьютексы, семафоры, atomic. Гонки данных. Взаимные блокировки. Race Detector.

Паттерны синхронизации worker pool, fan-in, fan-out.

## Каналы
Каналы и select https://golang.design/under-the-hood/en/part3concurrency/ch10chan/

## Инструменты синхронизации
Atomics https://habr.com/ru/articles/1033780/

Инструменты синхронизации https://golang.design/under-the-hood/en/part3concurrency/ch11sync/

### Разбор от AI
https://deepwiki.com/golang/go/2.7-concurrency-primitives-and-synctest

## Проблемы
Работа с памятью в конкурентной среде
* https://habr.com/ru/companies/oleg-bunin/articles/1014080/

### Race Detector
Документация

* https://go.dev/blog/race-detector
* https://go.dev/doc/articles/race_detector

## Паттерны синхронизации
Интерактивный учебник
* https://www.concurrency.rocks/
