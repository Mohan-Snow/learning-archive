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

В ***switch*** не обязательно писать кс break так как это реализованно по дефолту(хотя это возможно сделать).
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

* В поле свич не обязательно указывать переменную, по которой будет работать свич
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
### [to main page](../../README.md)