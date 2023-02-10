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

Functions with a **pointer argument** must take a **pointer**:
```go
var v Vertex
ScaleFunc(v, 5)  // Compile error!
ScaleFunc(&v, 5) // OK

func ScaleFunc(v *Vertex, f float64) {
	v.X = v.X * f
	v.Y = v.Y * f
}
```
The equivalent thing happens in the reverse direction.

Functions that take a **value argument** must take a **value** of that specific type:

```go
var v Vertex
fmt.Println(AbsFunc(v))  // OK
fmt.Println(AbsFunc(&v)) // Compile error!

func AbsFunc(v Vertex) float64 {
	return math.Sqrt(v.X*v.X + v.Y*v.Y)
}
```

while methods with **value receivers** take either a **value** or a **pointer** as the receiver when they are called:

```go
var v Vertex
fmt.Println(v.Abs()) // OK
p := &v
fmt.Println(p.Abs()) // OK

func (v Vertex) Abs() float64 {
	return math.Sqrt(v.X*v.X + v.Y*v.Y)
}
```






### [to main page](../../README.md)