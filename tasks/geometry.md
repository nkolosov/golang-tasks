# Описание задачи

Реализовать структуру Point (точка с координатами X, Y) и функцию Distance (вычисляющую расстояние между двумя точками).

Реализовать структуру Polygon (набор точек) и структуру Circle (окружность: задается Point центра и радиус)

Структуры Polygon и Circle должны соответствовать указанным ниже интерфейсам

```go
package geometry

type Figure interface {
    Area() float64
    Perimeter() float64
    Contains(point Point) bool
}

type Point interface {
    DistanceTo(other Point) float64
}
```

Весь функционал должен быть покрыт тестами

## Запуск проекта
Проект должен запускаться в консольном режиме, должны быть реализованы все сценарии

Примеры консольных команд для запуска (формат флагов, их названия и прочее могут отличаться в зависимости от деталей реализации)

Подсчет расстояния
```shell
$ /your/app --distance --point=X,Y --point=X,Y
```

Подсчет площади фигуры
```shell
$ /your/app --area [--circle=X,Y,R] [--center=X,Y --radius=R] [--polygon --point=X,Y, --point=X,Y --point=X,Y]
```

Проверка вхождения точки в фигуру
```shell
$ /your/app --contains --point=X,Y [--circle=X,Y,R] [--center=X,Y --radius=R] [--polygon --point=X,Y, --point=X,Y --point=X,Y]
```

При неправильном запуске должны отображаться корректные ошибки
