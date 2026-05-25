# Объединение done-каналов

Реализовать функцию, которая будет объединять один или более done-каналов в single-канал, если один из его составляющих каналов закроется, то закроется и он сам.
```go
package channel

var Or func(channels ...<-chan any) <-chan any
```

Пример использования функции

```go
package example

import (
	"channel"
	"fmt"
	"time"
)

func foo() {
	sig := func(after time.Duration) <-chan any {
		c := make(chan any)
		go func() {
			defer close(c)
			time.Sleep(after)
		}()
		return c
	}

	start := time.Now()
	<-channel.Or(
		sig(2*time.Hour),
		sig(5*time.Minute),
		sig(1*time.Second),
		sig(1*time.Hour),
		sig(1*time.Minute),
	)

	fmt.Printf("done after %v", time.Since(start)) // ~1 second
}

```
