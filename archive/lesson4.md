# Golang - основы 4. Рекурсия, указатели
## Рекурсия
С вызовом других функций мы уже сталкивались, а что будет, если вызвать функцию внутри её самой?<br>
**Рекурсивная функция** - это функция, в записи которой содержится она сама.
Пример рекурсивной функции в математике (функция возвращает `n`-ное число Фибоначчи):<br>
$F(0) = 0$<br>
$F(1) = 1$<br>
$F(n) = F(n - 1) + F(n - 2)$<br>
Такую же функцию можно построить и в программировании:
```golang
func fib(n int) int {
    if n == 0 || n == 1 {
        return n
    }
    return fib(n - 1) + fib(n - 2)
}
```
При вызове этой рекурсивной функции каждый раз будет вызываться еще парочка, полный ход выполнения можно проследить на картинке:
![fibonacci](https://github.com/papashik/elena/assets/96551531/d5985c71-98ef-4ee6-8faf-e782d6ea9acb)
Компилятору абсолютно все равно, где находится вызываемая функция, 
как мы помним, функции - это просто инструкции процессора, размещенные в оперативной памяти последовательно.
И от того, что находится в специальном регистре - счетчике команд (_IP - Instruction Pointer_) - зависит то,
какая инструкция будет выполняться следующей. Поэтому технически рекурсивный вызов функции ничем не отличается от обычного.
### Смысл рекурсии (украдено с [Практикума](https://practicum.yandex.ru/blog/rekursiya-v-programmirovanii/))
Рекурсивные функции нужны, когда требуется выполнить последовательность из одинаковых действий.
И иногда их проще описать рекурсивно, чем итеративно (с помощью цикла). 
Например, рекурсивный алгоритм обхода связного графа выглядит так (запускается из любой вершины графа):<br>
1. Пометить данную вершину как пройденную
2. Если дочерних вершин у данной нет, прервать выполнение функции
3. Иначе - вызвать функцию обхода для всех дочерних вершин
![Обход графа](https://prog-cpp.ru/wp-content/uploads/width.gif)

Если можно эффективно разделить задачу на маленькие подзадачи (как в следующих примерах),
то рекурсивные алгоритмы выглядят понятно и работают быстро:
### Пример - рекурсивное нахождение факториала
Математика:<br>
$0! = 1$ <br>
$n! = n * (n - 1)!$<br>
Информатика:
```golang
func fact(n int) int {
    if e == 0 {
        return 1 // завершение рекурсии
    }
    return n * fact(n - 1) // основная рекурсия
}
```
### Ещё пример - рекурсивное возведение числа в степень
Математика:<br>
$a^0 = 1$<br>
$a^{-e} = \frac{1}{a^{e}}$<br>
$a^e = a * a^{e-1}$<br>
Информатика:
```golang
func power(a float64, e int) float64 {
    if e == 0 {
        return 1 // завершение рекурсии
    } else if e < 0 {
        return 1 / power(a, -e) // расчет отрицательных степеней
    }
    return a * power(a, e - 1) // основная рекурсия
}
```
При построении рекурсивного алгоритма нужно четко выделить **условие завершения рекурсии** (получение пустого массива, числа 0, числа 1 и т.д.), так построить основную рекурсию будет легче.<br>
Вторая часть рекурсивного алгоритма - **шаг рекурсии**. Он частенько бывает разным в зависимости от проверки какого-либо условия (например, `return 1 + rec(...)` или просто `return rec(...)` - зависит от того, нужно ли увеличивать какой-то конечный результат).
## Указатели
Каждая переменная хранится в оперативной памяти компьютера. Оперативная память поделена на сегменты размером в 1 байт, и каждый сегмент имеет свой, уникальный, адрес. Соответственно, и каждая переменная в ней имеет свой адрес (обычно пишется в шестнадцатеричной СС)- например 0B5A67C. Адрес каждой переменной можно получить с помощью унарной операции `&`.
```golang
// Содержимое функции main()
a := 57
fmt.Println(&a) // выведет что-то по типу 0x4536537485, в зависимости от того, по какому адресу компилятор решит разместить эту переменную

arr := []int{1, 2, 3}
fmt.Println(&arr) // тоже выведет что-то по типу 0x453653748, адрес есть у переменной любого типа (среза, массива, числа, строки и т.д.)
fmt.Println(&arr[1]) // и у элемента среза тоже
```
Адреса (переменных) тоже можно хранить в переменных.
Переменные, хранящие адреса других переменных, называются **указателями**. Они имеют тип `*<тип оригинальной переменной>`, например, указатель на целое число имеет тип `*int`, указатель на срез строк имеет тип `*[]string`, указатель на указатель на дробное число имеет тип `**float64`(да, их ровно так же можно записать в память и ссылаться на них).<br>
Операция чтения или записи по адресу указателя называется **разыменованием указателя**. Она записывается вот так: `*`.
```golang
// Содержимое функции main()
a := 179
b := &a // в переменной `b` хранится адрес переменной `a`
        // так как `a` имеет тип `int`, то `b` имеет тип `*int`
fmt.Println(b) // выведет что-то по типу 0x4536537485 - значение указателя - адрес переменной `a`

fmt.Println(*b) // разыменование указателя - чтение значения по адресу, хранящемся в переменной `b` - выведет `179`
*b = 57 // разыменование указателя - запись значения по адресу, хранящемся в переменной `b`
fmt.Println(*b) // разыменование указателя - на этот раз выведет `57`
```
### Пример - функция, меняющая значение переменной по ее адресу
```golang
package main
import "fmt"

func change(p *int) {
    *p = 57 // вне зависимости от переданного адреса целого числа, по нему запишется число 57
}

func main() {
    var a int = 81
    pointer := &a // в переменной `pointer` лежит адрес переменной `a`
    fmt.Println(a, "лежит в памяти по адресу", pointer) // выведет `81 лежит в памяти по адресу 0x123456`
    change(pointer)
    fmt.Println(a, "лежит в памяти по адресу", pointer) // выведет `57 лежит в памяти по адресу 0x123456`
        // значение переменной `pointer` (что то же самое, что и адрес переменной `a`)
        // не изменилось (остался тот же самый `0x123456`), изменилось лишь число, лежащее в памяти по этому адресу
}
```
### Пример - функция, меняющая значения двух переменных местами, используя их адреса
```golang
package main
import "fmt"

func swap(p1, p2 *int) { // принимает два адреса целых чисел
    temp := *p1 // разыменуем первый указатель и положим значение в `temp`, теперь `temp` имеет тип `int` и `temp = 5`
    *p1 = *p2 	// разыменуем второй указатель и положим по адресу, содержащемуся в первом указателе,
                // теперь обе исходные переменные (`a` и `b` в функции main) равны 7
    *p2 = temp  // разыменуем второй указатель для записи в него значения переменной `temp` (равна 5)

                // в итоге по адресу `p1` (адрес переменной `a` в функции main) лежит число 7,
                // а по адресу `p2` (адрес переменной `b` в функции main) лежит число 5
}

func main() {
    var a, b int
    a, b = 5, 7
    swap(&a, &b) // меняем значения местами, передавая в качестве параметров адреса переменных
    fmt.Println(a, b) // выведет `7 5`
}
``` 
### Пример - функция, принимающая 3 указателя на целые числа и меняющая их на среднее арифметическое
```golang
package main
import "fmt"

func mean(p1, p2, p3 *int) { // принимает три адреса целых чисел
    res := (*p1 + *p2 + *p3) / 3
    *p1 = res
    *p2 = res
    *p3 = res
}

func main() {
    var a, b, c int
    a, b, c = 5, 7, 6
    mean(&a, &b, &c) // вызов функции, используя адреса локальных переменных
    fmt.Println(a, b, c) // выведет `6 6 6`
}
``` 