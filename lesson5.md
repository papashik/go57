# Golang - основы 5. Структуры
Предположим, мы пишем социальную сеть на Go.<br>
Нам понадобится много функций для обработки данных пользователей - для регистрации, добавления друзей,
отправки сообщений и файлов, замены фотографии профиля и так далее.
Для подобных действий функциям нужны данные пользователя - им нужно знать, кому и куда что добавить,
что поменять, от чьего имени отправить сообщение и т.д.<br> 
Так как хранить данные пользователей в глобальных переменных неразумно (почему?), сигнатуры наших функций будут увеличиваться
со скоростью света, обрастая разными подробностями о пользователе. 
К примеру, функция, каким-то образом обрабатывающая данные пользователя, должна знать: его уникальный идентификатор, имя, фамилию,
страну, город, дату рождения, фотографию его профиля, количество друзей, количество подписчиков, онлайн ли он сейчас,
когда последний раз был онлайн и еще много-много информации 
(пример - какие данные есть у [пользователя ВК](https://dev.vk.com/ru/reference/objects/user)):
```golang
// Наша функция могла бы выглядеть так:
func DoSomething(user_id, friends_count, followers_count int, name, surname, country, city string, birth_date, was_online date, is_online bool, profile_photo photo) {
    ...
}
```
Такие функции плохи тем, что работать с ними тяжело, нужно помнить порядок передаваемых параметров, в каких функциях есть
дополнительные аргументы, а в каких нет, да и уж больно их объявление длинное - даже не помещается в строчку на GitHub. Что же делать?
## Структуры
**Структура** - это составной тип данных, значение которого представляет собой последовательность именованных полей разных типов.<br>
Давайте заведем отдельную переменную под целого пользователя, которая будет хранить все его данные - своеобразный "ящик"
с другими переменными (переменные, хранящиеся внутри структуры, называются **полями структуры**). 
И тогда в функции, работающие с данными пользователя, можно будет передавать только одну переменную!

```golang
// Объявления полей в простейшем случае аналогичны объявлениям переменных, но не начинаются с ключевого слова var.
struct {
    имена_полей тип_полей
    имена_полей тип_полей
    ...
}
```
### Пример - использование безымянной структуры
```golang
package main
import "fmt"

func main() {
    var my_user struct { // объявление переменной типа `структура`
                    name, surname string
                    id int
                }
    my_user.name = "Alexander" // обращение к полям структуры для записи
    my_user.surname = "Kudimov"
    my_user.id = 1
    fmt.Printf("User: %s %s\nId: %d", my_user.name, my_user.surname, my_user.id) // обращение к полям структуры для чтения
    // выведет:
    // `User: Alexander Kudimov`
    // `Id: 1`
}
```
## Именованные структуры
Чтобы дать структуре имя, нужно объявить пользовательский именованный тип 
(об этом мы будем говорить подробнее, а пока просто запомните синтаксис).
### Пример - структура с именем
```golang
package main
import "fmt"

type User struct { // объявление пользовательского типа структуры
    name, surname string
    id, friends_count int
}

func main() {
    var pavel User // создание структуры, в этот момент все поля инициализирутся нулевыми значениями
    pavel.name = "Pavel"
    pavel.surname = "Yakubov"
    pavel.id = 31415
    PrintUser(pavel)
}

func PrintUser(user User) { // принимает один параметр типа "User"
    fmt.Printf("User: %s %s, Id: %d, Friends: %d\n", user.name, user.surname, user.id, user.friends_count)
    // выведет `User: Pavel Yakubov, Id: 31415, Friends: 0` - при создании структуры поля инициализируются нулевыми значениями
}
```
## Структурные литералы
**Литерал** – это изображение некоторого значения (целого числа, числа с плавающей точкой, строки и т.п.) в исходном тексте программы.
Примеры литералов: `57` (числовой литерал), `"pavel"` (строковой литерал), `false` (булевый литерал), 
`0,43` (литерал с плавающей точкой), `[]int{1, 2, 3}` (массивовый литерал) и т.д. <br>
Для структур тоже существуют составные литералы, похожие на массивовые литералы.
Они записываются так:
```golang
тип_структуры { значения_полей }
```
или так
```golang
тип_структуры { поле: значение, ..., поле: значение }
```
### Пример - несколько структур и структурные литералы
```golang
package main
import "fmt"

type Moment struct { // структурный тип, обозначающий момент времени
    year, month, day, hour, minute, second, millisecond int
}

type Message struct { // структурный тип, обозначающий сообщение (например, в социальной сети)
    from_id, to_id int           // от кого и кому
    text string                  // текст сообщения
    time Moment                  // время отправки
}

// функция отправляет сообщение (в нашем случае - в консоль)
func SendMessage(mes Message) {
    fmt.Println(mes)
}

func main() {
    var mes1 Message
    mes1.from_id = 123
    mes1.to_id = 456
    mes1.text = "Привет!"
    mes1.time = Moment{2023, 10, 4, 9, 0, 0, 0}
    SendMessage(mes1) // выведет `{123 456 Привет! {2023 10 4 9 0 0 0}}`

    // при использовании литерала с именами полей необязательно заполнять их все
    // (остальные заполнятся нулевыми значениями):
    time2 := Moment{year: 2023, month: 10, day: 5, hour: 10} // minute = 0, second = 0, millisecond = 0
    mes2 := Message{from_id: 456, to_id: 123, text: "И тебе привет!", time: time2}
    SendMessage(mes2) // выведет `{456 123 И тебе привет! {2023 10 5 10 0 0 0}}`


    // структурный литерал можно использовать даже с безымянной структурой:
    my_point := struct{ x, y int } {15, 35}
    fmt.Printf("X = %d, Y = %d\n", my_point.x, my_point.y) // выведет `X = 15, Y = 35`
}
```
Важный момент - обращение к полям через объект структуры или через указатель на структуру выглядит одинаково (операция разыменования сопряжена с обращением к полю):
```golang
// Содержимое main()
var coords struct { x, y int }
coords.x = 10
pointer := &coords
pointer.y = 20
fmt.Println(coords) // выведет `{10 20}`
```
## Структурные методы
С любым пользовательским именованным типом (или с указателем на пользовательский именованный тип) - в частности, с именованной структурой - можно связать множество специальных функций, называемых _методами_.<br>
**Метод** – это глобальная функция, имеющая параметр-приёмник. Она объявляется как
```golang
func (приёмник) имя сигнатура блок
```
**Приёмник** – это специальный параметр, объявляемый как
```golang
имя_параметра пользовательский_именованный_тип
```
либо
```golang
имя_параметра *пользовательский_именованный_тип
```
### Пример - объявление методов структуры User
```golang
type User struct {
    id int
    name, surname string
    friends []*User
}

// параметр-приёмник - объект типа `User`
func (u User) GetFullName() string { return u.name + " " + u.surname }

// параметр-приёмник - объект типа `*User`
func (u *User) AddFriend(friend *User) { u.friends = append(u.friends, friend) }
```
С типом `User` связан только метод `GetFullName()`.
С указателем на `User` связан метод `AddFriend()`, а также метод `GetFullName()`. 
Дело в том, что набор методов, связанный с некоторым именованным пользовательским типом `T`, автоматически переходит и на тип `*T`.

Для вызова методов предусмотрен особый синтаксис:
```параметр_приёмник.имя_метода(пар1, ..., парM)```
Набор методов для типа параметра-приёмника должен содержать вызываемый метод.

**В чем разница** между методами, связанными с `*User` и `User`?<br>
Для методов, имеющих параметром-приёмником объект типа `User`, в момент вызова в имя параметра (в данном случае - `u`) **копируется значение** оригинального (вызывающего метод) объекта, а для методов, имеющих параметром-приёмником объект типа `*User`, в момент вызова в имя параметра **копируется адрес** оригинального объекта. Следовательно, оригинальный объект можно изменять только через методы, имеющие параметром-приёмником указатель на какой-либо тип.
### Пример - работа с типом User
```golang
package main
import "fmt"

type User struct {
    id int
    name, surname string
    friends []*User
}

func (u User) GetFullName() string { return u.name + " " + u.surname }

func (u User) GetFriendsNames() []string {
    names := make([]string, len(u.friends))
    for i := range u.friends {
        names[i] = u.friends[i].GetFullName()
    }
    return names
}

func (u *User) AddFriend(friend *User) { u.friends = append(u.friends, friend) }

func main() {
    pavel := User{id: 123, name: "Pavel"}
    alex := User{456, "Alexander", "Pushkin", []*User{}}
    rulet := User{id: 789, name: "Rulet", surname: "Makovyy"}
    fmt.Println(rulet.GetFullName()) // выведет `Rulet Makovyy`

    pavel.AddFriend(&alex)
    pavel.AddFriend(&rulet)

    fmt.Println(pavel.GetFriendsNames()) // выведет `[Alexander Pushkin Rulet Makovyy]`
}
```