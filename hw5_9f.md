# Golang - основы. Структуры (ДЗ №5)
## 1. Вектора
Реализуйте библиотеку для работы над векторами в пространстве.<br>
Почитать про них можно где угодно, мне понравилось [здесь](https://ru.onlinemschool.com/math/library/vector/vector-definition/) (много разделов про все нужные темы).<br>
Создать:
1. тип структуры пространственной точки `Point`, содержащий три поля - три координаты точки: `x, y, z float64`;
2. тип структуры вектора `Vector`, содержащий три поля - три координаты вектора: `x, y, z float64`;
3. функцию `func CreateVector(a, b Point) Vector` - создаёт и возвращает вектор $\overrightarrow{ab}$ из переданных в качестве параметров точек $a$ и $b$;
4. метод `func (v Vector) Length() float64`, возвращающий длину вектора;
5. метод `func (v Vector) IsCollinearTo(other Vector) bool`, возвращающий, коллинеарны ли вектора `v` и `other`;
6. метод `func (v Vector) Multiply(a float64) Vector`, умножающий вектор `v` на число `a` и возвращающий результирующий вектор;
7. метод `func (v Vector) Add(other Vector) Vector`, возвращающий вектор, являющийся результатом сложения векторов `v` и `other`;
8. метод `func (v Vector) GetAngleTo(other Vector) float64`, возвращающий угол между векторами `v` и `other` (в радианах);
9. метод `func (v Vector) ScalarProductWith(other Vector) float64`, возвращающий результат скалярного произведения `v` и `other`;
10. метод `func (v Vector) VectorProductWith(other Vector) Vector`, возвращающий результат векторного произведения `v` и `other`.

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
fmt.Println(fourth.Length()) // 8
```
