# Планировщик задач

## Описание задачи
Реализовать планировщик, который выполняет задачи в заданное время.
Задачи могут быть однократными и периодическими.

```go
package scheduler

import (
	"context"
	"time"
	"github.com/google/uuid"
)

type Task struct {
	ID        uuid.UUID
	ExecuteAt time.Time     // для однократных
	Interval  time.Duration // для периодических (0 - однократная)
	Command   func(ctx context.Context) error
	Recurring bool
}

type Scheduler interface {
    ScheduleAt(Task, time.Time) // выполнить в определенное время
    ScheduleAfter(Task, time.Duration) // выполнить через N времени
    ScheduleEvery(Task, time.Duration) // периодическое выполнение
    Cancel(uuid.UUID) // отменить задачу
    GetPendingTasks() []Task // получить список ожидающих задач
    Stop() // остановить планировщик (дождаться выполнения текущих)
}
```
