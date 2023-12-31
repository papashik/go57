# Golang - основы. Структуры (ДЗ №5)
ДЗ присылается одним файлом с расширением `.go`, в котором есть `package main`, `import "fmt"` и все функции и методы, указанные ниже. Программа **должна быть протестирована** как минимум на примере использования (есть ниже)! Если программа падает или выдает неправильные ответы на примере использования - буду сразу отклонять.
## 1. Вектора
Реализуйте библиотеку для работы над векторами в пространстве.<br>
Почитать про них можно где угодно, мне понравилось [здесь](https://ru.onlinemschool.com/math/library/vector/vector-definition/) (много разделов про все нужные темы).<br>
Создать:
1. тип структуры пространственной точки `Point`, содержащий три поля - три координаты точки: `x, y, z float64`;
2. тип структуры вектора `Vector`, содержащий три поля - три координаты вектора: `x, y, z float64`;
3. функцию `func CreateVector(a, b Point) Vector` - создаёт и возвращает вектор $\overrightarrow{ab}$ из переданных в качестве параметров точек $a$ и $b$;
4. метод `func (p Point) Add(v Vector) Point`, возвращающий точку, являющуюся результатом прибавления вектора `v` к исходной точке `p`;
5. метод `func (v Vector) Length() float64`, возвращающий длину вектора;
6. метод `func (v Vector) IsCollinearTo(other Vector) bool`, возвращающий, коллинеарны ли вектора `v` и `other`;
7. метод `func (v Vector) Multiply(a float64) Vector`, умножающий вектор `v` на число `a` и возвращающий результирующий вектор;
8. метод `func (v Vector) Add(other Vector) Vector`, возвращающий вектор, являющийся результатом сложения векторов `v` и `other`;
9. метод `func (v Vector) GetAngleTo(other Vector) float64`, возвращающий угол между векторами `v` и `other` (в радианах);
10. метод `func (v Vector) ScalarProductWith(other Vector) float64`, возвращающий результат скалярного произведения `v` и `other`;
11. метод `func (v Vector) VectorProductWith(other Vector) Vector`, возвращающий результат векторного произведения `v` и `other`;
12. *(задача на пятерку) функцию `func IsPolygon(arr []Vector) bool`, возвращающую, образует ли переданный массив векторов `arr` замкнутую цепь.

Пример использования:
```golang
// Содержимое функции main()
var first Vector
first.x, first.y, first.z = 1, 1, 1
fmt.Println(first.Length()) // ~1.7

second := CreateVector(Point{0, 1, 1}, Point{0, 2, 1})
fmt.Println(second) // {0, 1, 0}
second = second.Multiply(2) // теперь second = {0 2 0}
fmt.Println(second.Length()) // 2

fmt.Println(first.IsCollinearTo(second)) // false

third := second.Add(Vector{2, 0, 2}) 
fmt.Println(first.IsCollinearTo(third)) // true

fmt.Println(first.GetAngleTo(third)) // 0
fmt.Println(first.GetAngleTo(first.Multiply(-1))) // ~3.14

fmt.Println(first.ScalarProductWith(second)) // 2

fourth := first.VectorProductWith(second) // {-2 0 2}
fmt.Println(fourth.Length()) // ~2.8
```
