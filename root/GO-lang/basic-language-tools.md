## Циклы (Loop)

---

Обычный цикл ***for***

```go
for i := 0; i < 10; i++ {
    <running code>
}
```

Цикл ***for*** аналог ***while*** цикла в других языках

```go
for i < 10 {
    <running code>
    i++
}
```

Вечный цикл
```go
for {
    <running code>
}
```
### Лэйблы (*Labels*)

Используются в том случае, если есть необходимость при каком-либо условии выйти
не только из внутреннего, но и из внешнего цикла.

* Использование ***goto*** это антипаттерн

```go
loop:
    for {
        switch {
        casex > 100:
            // break
            break loop
        default:
            x += x
        }
    }
    fmt.Println(x)
}
```

---

## Условия (***if*** conditions)

---

```go
if err != nil {
    <running code>
} else {
    <running code>
}
```

В блоке ***if*** можно инициализировать перменные

```go
if i := math.Pow(a, b); i > number {
    return i
}
```

---

## Switch cases

---

В ***switch*** также можно инициализировать переменные
Например: используем из стандартного пакета го.

В ***switch*** необязательно писать кс break так как это реализованно по дефолту(хотя это возможно сделать).
Можно явно указать, используя кс ***fallthrough***, в каком кейсе ***switch***
будет проваливаться в следующий по порядку кейс.

```go
switch os := runtime.GOOS; os {
    case "darwin": 
        fmt.Println("OS X.")
    case "linux": 
        fmt.Println("OS Linux.")
    default: 
        // freebsd, openbsd
        // plan9, windows...
        fmt.Printf("%s. \n", os)
}
```

* В поле ***switch*** необязательно указывать переменную, по которой будет работать ***switch***
```go
t := time.Now()
switch {
    case t.Hour() < 12: 
        fmt.Println("Good Morning")
    case t.Hour() < 17: 
        fmt.Println("Good Afternoon")
    default: 
        fmt.Printf("Good evening")
}
```

---

## Отложенные вызовы (***Defer***)

---

Это функциональность в **go** для использования **отложенных вызовов** функций
Все, что указанно в области ***defer*** будет выполнено после return текущей функции (в которой вызывается функция с defer)

```go
func main() {
    defer fmt.Println("world")
    
    fmt.Println("hello")
    // return
}
```
> hello  
> world

Может использоваться для очистки ресурсов.

Например: 
* сделали вызов клиента (call http)
* прочитали боди респонса (его надо закрыть)
* написать defer res.Body.Close() -> defer закроет и почистит все ресурсы

```go
func main() {
	fmt.Println("START")

	deferCall := func() {
		defer fmt.Println("DEFER")
	}

	deferCall()

	fmt.Println("FINISH")
}
```

> START   
> DEFER   
> FINISH  

Использование defer в цикле будет исполняться в обратном порядке. То есть, если мы вызываем Println с индексом 1 2 3, 
в консоль выведется 3 2 1

* Все вычисления происходят на момент объявления **defer**! (если переменная, использованная в defer, 
  поменяет свое значение ниже defer, после исполнения функции она будет иметь значение на момент указанной в defer)

---

## Указатели (Pointers)

---

Используются для указания на область памяти, разыменовывания, изменения значений.

Объявляем переменную **p**
```go
var p *int
```
Указываем переменной **p** на область памяти, где лежит значение **i**
```go
i := 1
p &i
```
Меняем значение **i** с помощью указателя **p**
```go
p* = 2
```
Разыменовываем поинтер (обращаемся по адресу(0xc0001...), который хранится в указателе **p**)
```go
i = 3
fmt.Println(*p) -> 3
```

---

## Структуры (***Structs***) или создание кастомных типов

---

Название структуры и/или его поля могут начинаться с lowercase, либо с uppercase. 
Как и с названиями функций это будет расцениваться как модификатор доступа.

```go
type MyType struct {
  A int
  B int
}

func main() {
  fmt.Println(MyType{1, 2})
}
```

Обращение к полям структуры происходит через точку.

```go
myType := MyType{1, 2}
myType.A = 3
```

Можно создавать указатели на кастомные типы.

```go
myType := MyType{1, 2}
p := &myType                // fmt.Println(p) -> console output gonna be &{1, 2}
p.X = 4
```

### Struct Literals
Явная и неявная инициализация поля в структуре.

```go
var (
  myType1 = MyType{A: 1}        // B:0 -> B is implicitly set to 0
  myType2 = MyType{}            // A:0, B:0 -> A,B are implicitly set to 0
  myType3 = MyType{A: 3, B: 4}  // A:3, B:4 -> A,B are explicitly set to 3,4
)
```

---

## Массивы (***Arrays***)

---

Индексированная последовательность определенного типа.  
Размер ***фиксирован***!

```go
var arr [2]string
arr[0] = "Hello"
arr[1] = "World"
```

С предустановленными значениями
```go
numbers := [4]int{0, 1, 2, 3}   // fmt.Println(numbers) -> console output gonna be [0 1 2 3]
```

---


### [to main page](../../README.md)