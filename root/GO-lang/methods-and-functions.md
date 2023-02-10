# ***Methods and Functions***

---

Method is just a function with a receiver argument. It has a special <mark>receiver</mark> argument.

You can declare methods with pointer receivers.

This means the receiver type has the literal syntax <mark>*T</mark> for some type <mark>T</mark>. (Also, T cannot itself be a pointer such as *int.)

<mark>Methods with pointer receivers can modify the value to which the receiver points.</mark> Since methods often need to modify their receiver, pointer receivers are more common than value receivers.

```go
type Vertex struct {
    NumberOne, NumberTwo float64
}

func (vertex Vertex) PrintValues() {
    // vertex Vertex - тут в ресивере уже копия структуры, а не указатель на нее
    fmt.Println(vertex.NumberOne, vertex.NumberTwo)
}

func (vertex *Vertex) Scale(num int) {
    // vertex *Vertex - по указателю на настоящую структуру меняем ей поля
    // если в ресивере убрать *, работа будет происходить на полях копии структуры, а не на оригинале
    vertex.NumberOne = vertex.NumberOne + float64(num)
    vertex.NumberTwo = vertex.NumberTwo + float64(num)
}

func main() {
    v := Vertex{0, 1}
    v.Scale(10)
    v.PrintValues()
}
```







### [to main page](../../README.md)