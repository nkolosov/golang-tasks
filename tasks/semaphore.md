# Cемафор (N-mutex)

Реализовать несколько (не менее трёх) вариантов семафора с общим интерфейсом и тесты для них
```go
package semaphore

import "context"

type Semaphore interface {
	Acquire(context.Context, int64) error
	TryAcquire(int64) bool
	Release(int64)
}
```
