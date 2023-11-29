# Классная работа №7. Связные списки

## 1. Простейшие операции
Написать:
1. функцию `InitLinkedList(value int) *Node`, инициирующую связный список (с англ. linked list - связанный список) - создает  вершину со значением value и возвращает указатель на неё;
2. метод `(root *Node) Print()`, печатающий **весь** связный список (все значения в одной строке);
3. метод `(root *Node) Length() int`, возвращающий длину связного списка. Для этого нужно пройти по всем вершинам связанного списка и посчитать их;
4. метод `(root *Node) Last() *Node`, возвращающий указатель на последний элемент связного списка;
5. метод `(root *Node) Get(n int) *Node`, возвращающий указатель на `n`-ный элемент (нумеруются с нуля) связного списка  (либо `nil`, если такого нет);
6. метод `(root *Node) Find(value int) *Node`, возвращающий указатель на вершину со значением `value` (либо `nil`, если такой нет).


## 2. Добавление элементов
Обратите внимание, эти методы ничего не возвращают! После их выполнения структура, лежащая по адресу `root` может измениться! (например, после выполнения `AddBeforeRoot`)

Написать:
1. метод `(root *Node) AddBeforeRoot(node *Node)`, вставляющий элемент в список перед корнем;
2. метод `(root *Node) AddAfterRoot(node *Node)`, вставляющий элемент в список после корня;
3. метод `(root *Node) Add(node *Node, n int) *Node`, вставляющий элемент в список на `n`-ное место (нумеруются с нуля).


Пример использования:
```golang
// Содержимое функции main()
root := InitLinkedList(10)
root.next = &Node{9, nil}

root.Print() // выведет `10 9`
fmt.Println(root.Length()) // выведет `2`

fmt.Println(root.Last()) // выведет что-то типа `&Node{9, <nil>}`

// следующие две строки эквивалентны следующей записи: `root.next.next = &Node{8, nil}`
root.next.next = new(Node)
root.next.next.value = 8

fmt.Println(root.Get(2).value) // выведет `8`

found := root.Find(5)
if found != nil {
    fmt.Println(found.value} // не сработает
}

found = root.Find(8)
if found != nil {
    fmt.Println(found.value} // выведет 8
}

// Добавление элементов
root.AddBeforeRoot(11)
root.Print() // выведет `11 10 9 8`
fmt.Println(root.value) // выведет `11`

root.AddAfterRoot(0)
root.Print() // выведет `11 0 10 9 8`

root.Add(&Node{7, nil}, 5)
root.Print() // выведет `11 0 10 9 8 7`

root.Add(&Node{12, nil}, 0)
root.Print() // выведет `12 11 0 10 9 8 7`
fmt.Println(root.Length()) // выведет `7`
```

Полный вывод (чтобы сравнить со своим):
```
10 9
2
&Node{9, <nil>}
8
8
11 10 9 8
11
11 0 10 9 8
11 0 10 9 8 7
12 11 0 10 9 8 7
7
```
