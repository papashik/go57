# Зачётное задание 1
## Вариант 1 - представление больших чисел с помощью массивов
Тип целого числа имеет ограниченный размер, поэтому не подходит для хранения (очень) больших чисел.<br> 
Пример переполнения числа:
```golang
var a int64 = 9223372036854775807   // int64 - чтобы размер был определён точно
fmt.Println(a + 1)                  // выведет -9223372036854775808
```
Нужно реализовать свой тип данных для работы с такими числами.

### Задание
Создать тип `Big`, под капотом имеющий массив цифр числа (если размер массива фиксирован, то размер - минимум 20 разрядов), для него функции:
- `CreateBig(num int64) Big` - создаёт `Big`-число из обычного - можно не создавать, но с ним удобнее тестировать
- `(b Big) Add(another Big) Big` - возвращает сумму двух чисел
- `(b Big) Sub(another Big) Big` - возвращает разность двух чисел (гарантируется, что меньшее вычитается из большего)
- `(b Big) String()` - возвращает строковую запись числа для нормального вывода в консоль

Пример использования:
```golang
var a, b Big = CreateBig(9223372036854775807), CreateBig(1)

// складываем два числа и вызываем у результата метод String() для перевода в человекочитаемый вид
fmt.Println(a.Add(b).String())    // выведет 9223372036854775808 - переполнения нет

var c, d Big = CreateBig(101), CreateBig(2)
fmt.Println(c.Sub(d).String())    // выведет 99
```
Совет: храните число в обратном порядке - так будет проще складывать от младшего разряда

## Вариант 2 - доработка квадрата
Для выполнения этого задания необходимо [скачать](https://go.dev/dl/) и установить Go.<br>
Использовать компилятор Go нужно в командной строке - [**моя справка**](https://github.com/papashik/go57/blob/main/lesson8_cmd.md) по часто используемым командам.<br>
Небольшая [внешняя справка](https://python-teach.ru/osnovi-programmirovanija/komandnaya-stroka-dlya-nachinayushhih-programmistov/?ysclid=lqdw6zx352755609181) по командной строке.<br>

Самое главное - [ссылка](https://onlinegdb.com/cjxq0JHrZ) на код движущегося вправо квадрата, который вы будете дорабатывать.<br>
Для первичного запуска кода нужно:
1. установить Go (проверить, что он установлен можно, набрав `go version` в командной строке);
2. создать файл у вас в системе, содержащий код квадрата. Запомнить, где он находится;
3. с помощью командной строки перейти в директорию, содержащую этот файл;
4. инициировать модуль и установить необходимые библиотеки (как - см. в часто используемых командах);
5. скомпилировать и запустить файл - `go run imya_moego_faila.go`.

Должно появиться окошко с движущимся вправо квадратом.

### Ebiten
`Ebiten` - это пакет (библиотека) для создания игр на Go.<br>
[Официальный сайт](https://ebitengine.org) пакета `ebiten`.<br>
**Рекомендуется!** - [Документация](https://pkg.go.dev/github.com/hajimehoshi/ebiten/v2) на него на офиц. сайте языка Go.

Краткая [презентация по Ebiten](https://github.com/papashik/go57/files/13728923/Ebiten.pptx).

Всего в `ebiten` есть три жизненно важные вещи: структура `Game`, метод `(g *Game) Update()` и метод `(g *Game) Draw(screen *ebiten.Image)`:
1. `Game` содержит **всё сохраняемое состояние игры** - все объекты, существующие в игре и все то, что должно вырисовываться на экране - в примере у нас это одна структура `Square`. Если вы хотите хранить состояние нескольких квадратов - создайте поле типа `[]Square`
2. метод `(g *Game) Update()` вызывается программой 60 раз в секунду - в нём мы изменяем внутреннее (смысловое, логическое) состояние игры - проверяем, нажаты ли клавиши/кнопки мыши или достиг ли какой-то объект края поля и т.д. В нём в зависимости от каких-то условий можно изменять значения полей структуры `Game` (объекты игры). Иногда в нём размещают методы `Update()` для всех объектов игры для исключения путаницы
3. метод `(g *Game) Draw(screen *ebiten.Image)` тоже вызывается программой 60 раз в секунду (сразу после `Update()`). Он отвечает за "рисование" всех объектов игры на экран так, как нужно вам. Он имеет параметр типа `*ebiten.Image` (на этих объектах строится вся графика `ebiten`) - это "изображение" экрана, на котором можно размещать изображения других объектов. В примере можно увидеть поле `image` у структуры `Square` - именно это изображение мы размещаем на изображении `screen`.

В функции `main()` никаких обновлений **не производится!** Мы просто инициируем игру начальными параметрами и запускаем её с помощью `RunGame()`. Все изменения происходят в `Update` и `Draw`.

### Задание
1. Сымитировать поведение квадрата так, как будто он находится в гравитационном поле с постоянным ускорением, а внизу экрана стенка, от которой он отталкивается.
![bouncing_square](https://github.com/papashik/go57/assets/96551531/615466b0-adce-4c0a-8188-30c6d443dc92)

Рекомендации: заведите глобальную переменную для хранения значения ускорения свободного падения, а у квадрата дополнительное поле, содержащее его скорость. Каждый такт скорость каждого объекта будет изменяться согласно значению ускорения, а координаты - сообразно его скорости.

2. Разрешить пользователю добавлять квадраты в игру по нажатию мыши. По желанию, создавайте их случайного цвета, чтобы у зрителя полилась кровь из глаз при просмотре.
![creating_squares](https://github.com/papashik/go57/assets/96551531/bc278a72-91cd-4061-a334-94791125d3fb)

Рекомендации: для отслеживания нажатия воспользуйтесь функцией `IsMouseButtonPressed` из пакета ebiten или `IsMouseButtonJustPressed` из пакета [inpututil](https://pkg.go.dev/github.com/hajimehoshi/ebiten/v2/inpututil). Отслеживать нужно внутри функции `Update`, а не `Draw`! Для понимания, где находится курсор мыши можно воспользоваться функцией `ebiten.CursorPosition()`.

## Вариант 3 - командная строка для работы с файлами
Для выполнения этого задания необходимо [скачать](https://go.dev/dl/) и установить Go (см. предыдущий вариант).<br>
С помощью пакета `os` и конструкции `switch-case` в этом задании вы реализуете эмулятор простейшей командной строки для работы с файлами на вашем компьютере.

[**Моя справка**](https://github.com/papashik/go57/blob/main/lesson8_cmd.md) по часто используемым командам командной строки.<br>
[Справка по конструкции switch-case](https://metanit.com/go/tutorial/2.9.php?ysclid=lqf4rjzo9681772986).<br>
[Документация](https://pkg.go.dev/os) по пакету `os`.<br>
**Рекомендуется!** - Хорошие статьи по [созданию и открытию](https://metanit.com/go/tutorial/8.2.php) файлов и [чтению и записи](https://metanit.com/go/tutorial/8.3.php) в файлы.

### Задание
При запуске программы на компьютере её рабочей директорией становится та, в которой вы её запустили, и она (программа) может так же, как и вы, работать с файлами в этой директории - создавать, изменять и т.д. Вы должны сделать её интерфейс схожим с командной строкой - организовать бесконечный ввод и выполнение команд, указанных ниже.<br>
[**Простой пример**](https://onlinegdb.com/dEweMNgmk) командной строки с 3 командами - можете запустить прямо там и посмотреть, как она устроена.

С помощью бесконечного цикла `for { ... }` организуйте бесконечный ввод команд (по желанию, предваряйте его знаком `$` или `>` как в настоящих консолях). Внутри цикла нужно считать команду и затем выполнить действия, предписанные ей (выбор варианта команды организовать с помощью `switch`).

|Команда|Описание|
|-|-|
|`exit`|Завершает программу|
|`create my_file`|Создаёт файл `my_file`. Вместо `my_file` разрешается ввести любое слово - программа должна уметь создавать файлы с любым именем (введённое `create pavel.txt` создаёт файл с именем `pavel.txt`, введённое `create andrey` создаёт файл с именем `andrey`)|
|`read my_file`|Открывает файл `my_file` и выводит его содержимое. Вместо `my_file` разрешается ввести любое слово - программа должна уметь открывать файлы с любым именем. Если запрошенного файла не существует - вывести ошибку|
|`write main.go`|Добавление данных в файл `main.go`. Вместо `main.go` разрешается ввести любое слово - программа должна уметь добавлять данные в файлы с любым именем. Если запрошенного файла не существует - вывести ошибку. После ввода этой команды предлагается ввести еще одну строку, которая и будет записана в файл|
|Любая_другая_введённая_команда|Вывести ошибку `Unknown command`|

Сделайте программу максимально устойчивой к любому пользовательскому вводу - она не должна завершаться при вводе неизвестного слова, отсутствии аргумента у команды и т.д. В идеале все ошибки должны быть обработаны - пользователь должен знать, что он сделал не так при том или ином вводе.

## Вариант 4 - перевод в СС
**Входные данные:** число в десятичной системе счисления и основание системы счисления (от 2 до 16) для перевода.
**Выходные данные:** это число в заданной системе счисления (буквы выводятся заглавными). 
|Ввод|Вывод|
|-|-|
|`5757 16`|`167D`|
