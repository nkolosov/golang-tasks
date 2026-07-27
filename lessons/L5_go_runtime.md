# Занятие 5. Go Runtime

Go Runtime. Составные части.

Go Scheduler. Garbage Collector. Memory Allocator. Stack Manager.

Go Scheduler. GMP-модель. Горутины. Внутреннее устройство горутин.

Стек и куча. Сборщик мусора. Mark & Sweep. Трехцветный алгоритм. Green Tea GC. Write Barrier.

Escape analysis.

## Go Runtime

Конкурентность в Go https://go.dev/doc/effective_go#concurrency

Пакет в SDK https://pkg.go.dev/runtime

### Подробный разбор
* Модель памяти https://habr.com/ru/articles/1023762/
* Как работает CPU и RAM. Часть 1 https://habr.com/ru/articles/1023964/
* Как работает CPU и RAM. Часть 2 https://habr.com/ru/articles/1024868/
* Как работает CPU и RAM. Часть 3 https://habr.com/ru/articles/1025292/
* Как работает CPU и RAM. Часть 4 https://habr.com/ru/articles/1026308/
* Как работает CPU и RAM. Часть 5 https://habr.com/ru/articles/1027988/
* Низкоуровненвые концепции. Atomic https://habr.com/ru/articles/1033780/
* Go Runtime. https://habr.com/ru/articles/1063194/

Внутреннее устройство Go (серия статей)
* https://www.altoros.com/blog/golang-internals-part-1-main-concepts-and-project-structure/

### Разбор от AI
* Управление памятью в куче https://deepwiki.com/golang/go/2.2-memory-management
* Управление стеком https://deepwiki.com/golang/go/2.4-stack-management

## Go Scheduler
Устройство (кратко) https://habr.com/ru/articles/804145/

Устройство планировщика https://habr.com/ru/articles/891426/

Внутреннее устройство планировщика (Видео) https://www.youtube.com/watch?v=P2Tzdg8n9hw

Как живут потоки ОС в Go https://habr.com/ru/companies/tbank/articles/868390/

Goroutine Scheduler https://golang.design/under-the-hood/en/part3concurrency/ch09sched/

Context Switching in Go https://deavinside.medium.com/the-anatomy-of-a-context-switch-in-go-a05cefc9b96e

### Разбор от AI
https://deepwiki.com/golang/go/2.1-goroutine-scheduler

## Garbage Collector
Документация 
* https://go.dev/doc/gc-guide

Модель памяти Go 
* https://go.dev/ref/mem

Полезные материалы
* Green Tea GC https://habr.com/ru/articles/1063470/
* Memory Allocator https://golang.design/under-the-hood/en/part4memory/ch12alloc/
* Stack Management https://golang.design/under-the-hood/en/part4memory/ch14stack/
* Как работает стек в Go https://blog.cloudflare.com/how-stacks-are-handled-in-go/
* Garbage Collector https://golang.design/under-the-hood/en/part4memory/ch13gc/
* Механизмы выделения памяти https://habr.com/ru/companies/ruvds/articles/442648/

Утечки памяти
* Бизнес-логика https://habr.com/ru/companies/ncloudtech/articles/675390/
* Особенности Runtime https://habr.com/ru/companies/ncloudtech/articles/676960/

### Разбор от AI
https://deepwiki.com/golang/go/2.3-garbage-collection
