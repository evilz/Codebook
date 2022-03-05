---
theme: sky
highlightTheme: a11y-dark
---

![](https://upload.wikimedia.org/wikipedia/commons/thumb/0/05/Go_Logo_Blue.svg/512px-Go_Logo_Blue.svg.png?20191207190041)

---

## Import some built-in package
```go
// some import
import (
    "fmt"
    "log"
    "os"
)
```

---


## Hello World

Our first program will print the classic “hello world” message. Here’s the full source code.
It use the `fmt`package and function `Println`.
```go
func main() {
    fmt.Println("hello world")
}
```
```output
hello world
```

---

## Values

Go has various value types including strings, integers, floats, booleans, etc.

```go
func main() {

    fmt.Println("go" + "lang")

    fmt.Println("1+1 =", 1+1)
    fmt.Println("7.0/3.0 =", 7.0/3.0)

    fmt.Println(true && false)
    fmt.Println(true || false)
    fmt.Println(!true)
}
```
```output
golang
1+1 = 2
7.0/3.0 = 2.3333333333333335
false
true
false
```

---

## Variables

In Go, variables are explicitly declared and used by the compiler to e.g. check type-correctness of function calls.

`var` declares 1 or more variables.

You can declare multiple variables at once.
Go will **infer** the type of initialized variables.
Variables declared without a corresponding initialization are **zero-valued**. For example, the zero value for an int is 0.

The `:=` syntax is shorthand for declaring and initializing a variable, e.g. for `var f string = "apple"` in this case.

```go
func main() {

    var a = "initial"
    fmt.Println(a)

    var b, c int = 1, 2
    fmt.Println(b, c)

    var d = true
    fmt.Println(d)

    var e int
    fmt.Println(e)

    f := "apple"
    fmt.Println(f)
}
```
```output
initial
1 2
true
0
apple
```

## Constants

Go supports constants of character, string, boolean, and numeric values.

`const` declares a constant value.

A const statement can appear anywhere a `var` statement can.

Constant expressions perform arithmetic with arbitrary precision.
A numeric constant has no type until it’s given one, such as by an explicit conversion.
A number can be given a type by using it in a context that requires one, such as a variable assignment or function call. For example, here `math.Sin` expects a `float64`.
```go
import "math"

const s string = "constant"

func main() {
    fmt.Println(s)

    const n = 500000000

    const d2 = 3e20 / n
    fmt.Println(d2)

    fmt.Println(int64(d2))

    fmt.Println(math.Sin(n))
}
```
```output
constant
6e+11
600000000000
-0.28470407323754404
```

## For

`for` is Go’s only looping construct. Here are some basic types of `for` loops.

The most basic type, with a single condition.

A classic initial/condition/after `for` loop.

`for` without a condition will loop repeatedly until you `break` out of the loop or `return` from the enclosing function.

You can also `continue` to the next iteration of the loop.

We’ll see some other `for` forms later when we look at `range` statements, channels, and other data structures.
```go
func main() {

    i := 1
    for i <= 3 {
        fmt.Println(i)
        i = i + 1
    }

    for j := 7; j <= 9; j++ {
        fmt.Println(j)
    }

    for {
        fmt.Println("loop")
        break
    }

    for n := 0; n <= 5; n++ {
        if n%2 == 0 {
            continue
        }
        fmt.Println(n)
    }
}
```
```output
1
2
3
7
8
9
loop
1
3
5
```
```go
//package main

import (
    "fmt"
    "log"
    "os"
    "time"
)

func timeTrack(start time.Time, name string) {
    elapsed := time.Since(start)
    fmt.Printf("%s took %s", name, elapsed)
}

func main() {
    defer timeTrack(time.Now(), "Total")
    c1 := make(chan string)
    c2 := make(chan string)

    go func() {
        fmt.Println("Wait for 1s")
        time.Sleep(1 * time.Second)
        c1 <- "one"
    }()
    go func() {
        fmt.Println("Wait for 2s")
        time.Sleep(2 * time.Second)
        c2 <- "two"
    }()

    for i := 0; i < 2; i++ {
        select {
        case msg1 := <-c1:
            fmt.Println("received", msg1)
        case msg2 := <-c2:
            fmt.Println("received", msg2)
        }
    }
}
```
```output
Wait for 2s
Wait for 1s
received one
received two
Total took 2.0148855s
```
