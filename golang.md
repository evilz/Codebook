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
fmt.Println("hello world")
```
```output
hello world
```

---

## Values

Go has various value types including strings, integers, floats, booleans, etc.

```go
fmt.Println("go" + "lang") // string concatenation

fmt.Println("1+1 =", 1+1) // int addition
fmt.Println("7.0/3.0 =", 7.0/3.0) // float division

fmt.Println(true && false) // boolean AND
fmt.Println(true || false) // boolean OR
fmt.Println(!true) // boolean NOT

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

--

```go
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

```
```output
initial
1 2
true
0
apple
```

---

## Constants

Go supports constants of character, string, boolean, and numeric values.

`const` declares a constant value.

A const statement can appear anywhere a `var` statement can.

Constant expressions perform arithmetic with arbitrary precision.
A numeric constant has no type until it’s given one, such as by an explicit conversion.
A number can be given a type by using it in a context that requires one, such as a variable assignment or function call. For example, here `math.Sin` expects a `float64`.

--

```go
import "math"

const s string = "constant"

fmt.Println(s)

const n = 500000000

const d2 = 3e20 / n
fmt.Println(d2)

fmt.Println(int64(d2))

fmt.Println(math.Sin(n))

```
```output
constant
6e+11
600000000000
-0.28470407323754404
```

---

## For

`for` is Go’s only looping construct. Here are some basic types of `for` loops.

The most basic type, with a single condition.

A classic initial/condition/after `for` loop.

`for` without a condition will loop repeatedly until you `break` out of the loop or `return` from the enclosing function.

You can also `continue` to the next iteration of the loop.

We’ll see some other `for` forms later when we look at `range` statements, channels, and other data structures.

--

```go
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

---

## If/Else

Branching with ``if`` and ``else`` in Go is straight-forward.

You can have an ``if`` statement without an ``else``.

A statement can precede conditionals; any variables declared in this statement are available in all branches.

Note that you don’t need parentheses around conditions in Go, but that the braces are required.

There is **no ternary** if in Go, so you’ll need to use a full ``if`` statement even for basic conditions.

--

```go
if 7%2 == 0 {
    fmt.Println("7 is even")
} else {
    fmt.Println("7 is odd")
}

if 8%4 == 0 {
    fmt.Println("8 is divisible by 4")
}

if num := 9; num < 0 {
    fmt.Println(num, "is negative")
} else if num < 10 {
    fmt.Println(num, "has 1 digit")
} else {
    fmt.Println(num, "has multiple digits")
}

```
```output
7 is odd
8 is divisible by 4
9 has 1 digit
```

---

## Switch

Switch statements express conditionals across many branches.

You can use commas to separate multiple expressions in the same case statement. We use the optional ``default`` case in this example as well.

``switch`` without an expression is an alternate way to express if/else logic. Here we also show how the ``case`` expressions can be non-constants.

A type ``switch`` compares types instead of values. You can use this to discover the type of an interface value. In this example, the variable t will have the type corresponding to its clause.

--

```go
import "time"

i2 := 2
fmt.Print("Write ", i2, " as ")
switch i2 {
case 1:
    fmt.Println("one")
case 2:
    fmt.Println("two")
case 3:
    fmt.Println("three")
}

switch time.Now().Weekday() {
case time.Saturday, time.Sunday:
    fmt.Println("It's the weekend")
default:
    fmt.Println("It's a weekday")
}

t := time.Now()
switch {
case t.Hour() < 12:
    fmt.Println("It's before noon")
default:
    fmt.Println("It's after noon")
}

whatAmI := func(i interface{}) {
    switch t := i.(type) {
    case bool:
        fmt.Println("I'm a bool")
    case int:
        fmt.Println("I'm an int")
    default:
        fmt.Printf("Don't know type %T\n", t)
    }
}
whatAmI(true)
whatAmI(1)
whatAmI("hey")

```
```output
Write 2 as two
It's the weekend
It's after noon
I'm a bool
I'm an int
Don't know type string
```

---

## Arrays

In Go, an array is a numbered sequence of elements of a specific length.

Here we create an array a that will hold exactly 5 ``ints``. The type of elements and length are both part of the array’s type. By default an array is zero-valued, which for ``ints`` means 0s.

We can set a value at an index using the ``array[index] = value`` syntax, and get a value with ``array[index]``.

The builtin ``len`` returns the length of an array.

Use this syntax to declare and initialize an array in one line.

Array types are one-dimensional, but you can compose types to build multi-dimensional data structures.

Note that arrays appear in the form ``[v1 v2 v3 ...]`` when printed with fmt.Println.

You’ll see slices much more often than arrays in typical Go. We’ll look at slices next.

--

```go
var a2 [5]int
fmt.Println("emp:", a2)

a2[4] = 100
fmt.Println("set:", a2)
fmt.Println("get:", a2[4])

fmt.Println("len:", len(a2))

b2 := [5]int{1, 2, 3, 4, 5}
fmt.Println("dcl:", b2)

var twoD [2][3]int
for i := 0; i < 2; i++ {
    for j := 0; j < 3; j++ {
        twoD[i][j] = i + j
    }
}
fmt.Println("2d: ", twoD)

```
```output
emp: [0 0 0 0 0]
set: [0 0 0 0 100]
get: 100
len: 5
dcl: [1 2 3 4 5]
2d:  [[0 1 2] [1 2 3]]
```

---


```go
// //package main

// import (
//     "fmt"
//     "log"
//     "os"
//     "time"
// )

// func timeTrack(start time.Time, name string) {
//     elapsed := time.Since(start)
//     fmt.Printf("%s took %s", name, elapsed)
// }

// func main() {
//     defer timeTrack(time.Now(), "Total")
//     c1 := make(chan string)
//     c2 := make(chan string)

//     go func() {
//         fmt.Println("Wait for 1s")
//         time.Sleep(1 * time.Second)
//         c1 <- "one"
//     }()
//     go func() {
//         fmt.Println("Wait for 2s")
//         time.Sleep(2 * time.Second)
//         c2 <- "two"
//     }()

//     for i := 0; i < 2; i++ {
//         select {
//         case msg1 := <-c1:
//             fmt.Println("received", msg1)
//         case msg2 := <-c2:
//             fmt.Println("received", msg2)
//         }
//     }
// }
```
```output
# command-line-arguments
.\main.go:207:5: syntax error: unexpected EOF, expecting }
```
