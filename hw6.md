# ДЗ 6. Двумерные массивы
В этом ДЗ продолжаем тренировать операции над многомерными массивами и начинаем разрабатывать библиотеку для работы над матрицами.

**Транспонированная матрица** $A^{T}$ -  матрица, полученная из исходной матрицы $A$ заменой строк на столбцы.
Например, для матрицы 

$$ A = {\left\lbrack \matrix{1 & 2 & 3 \cr 4 & 5 & 6} \right\rbrack} $$

транспонированной будет являться

$$ A^{T} = {\left\lbrack \matrix{1 & 4 \cr 2 & 5 \cr 3 & 6} \right\rbrack} $$

## Структура и методы
Создать:
1. тип структуры матрицы `Matrix`, содержащую длину матрицы `Length int`, ширину матрицы `Width int` и саму матрицу `Data [][]int`;
2. функцию для создания матрицы `func CreateMatrix(n, m int) Matrix` - создает матрицу `n X m`, инициализирует её поля и возвращает в качестве результата;
3. метод `func (m Matrix) Print()` - печатает матрицу в консоль по строкам;
4. метод `func (m Matrix) IsSquare() bool` - возвращает, является ли матрица квадратной;
5. метод `func (m Matrix) MainDiagonal() []int` - возвращает главную диагональ матрицы;
6. метод `func (m Matrix) IsDiagonal() bool` - возвращает, является ли матрица диагональной (с англ. Diagonal matrix - диагональная матрица);
7. метод `func (m Matrix) IsIdentity() bool` - возвращает, является ли матрица единичной (с англ. Identity matrix - единичная матрица);
8. метод `func (m Matrix) DelRow(i int) Matrix` - возвращает **новую** матрицу, являющуюся копией исходной, но без строки `i` (нумеруются с нуля);
9. метод `func (m Matrix) DelColumn(j int) Matrix` - возвращает **новую** матрицу, являющуюся копией исходной, но без столбца `j` (нумеруются с нуля);
10. метод `func (m *Matrix) ResetThis()` - обнуляет исходную матрицу. Обратите внимание, только этот метод объявлен с указателем на матрицу `(m *Matrix)` в качестве параметра-приёмника - так как только он меняет исходную матрицу. Остальные методы, которые не меняют исходный объект, объявлены с самим объектом `(m Matrix)` в качестве параметра-приёмника;
11. метод `func (m Matrix) Transpose() Matrix` - возвращает матрицу, которая является транспонированной копией исходной;
12. метод `func (m Matrix) Rotate() Matrix` - возвращает матрицу, которая является копией исходной матрицы, повернутой на 90° по часовой стрелке.

Примеры использования:
```golang
// Содержимое функции main()
matr := CreateMatrix(3, 3)
fmt.Println(matr.IsSquare()) // true
fmt.Println(matr.IsDiagonal()) // true
fmt.Println(matr.IsIdentity()) // false

for i := 0; i < 3; i++ {
    matr.Data[i][i] = 1
}
fmt.Println(matr.IsIdentity()) // true

matr = matr.DelRow(2)
fmt.Println(matr.IsSquare()) // false
fmt.Println(matr.IsDiagonal()) // true
fmt.Println(matr.IsIdentity()) // false
matr.Print() // выведет `1 0 0`
             //         `0 1 0`
fmt.Println(matr.Length, matr.Width) // выведет `2 3`

matr = matr.Rotate()
matr.Print() // выведет `0 1`
             //         `1 0`
             //         `0 0`
fmt.Println(matr.Length, matr.Width) // выведет `3 2`

matr = matr.Transpose()
matr.Print() // выведет `0 1 0`
             //         `1 0 0`

matr = matr.DelColumn(2)
fmt.Println(matr.IsSquare()) // true
fmt.Println(matr.IsDiagonal()) // false
fmt.Println(matr.IsIdentity()) // false

matr = matr.Rotate()
matr.Print() // выведет `1 0`
             //         `0 1`
fmt.Println(matr.IsIdentity()) // true
fmt.Println(matr.MainDiagonal()) // выведет `[1 1]`

matr.ResetThis()
matr.Print() // выведет `0 0`
             //         `0 0`
```
