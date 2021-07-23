---
description: Things to know as a new developer in Go
---

# Beginner Quirks and Gotchas

### Learning Goals

* "quirk" -- things about the Go language that are unique, and parallels to other languages
* "gotcha" -- common pitfall that Go developers will find themselves in which can cause errors or unexpected behavior

### Background

When you're learning a new programming language you may often ask "why does it do THAT?" and this content will help to demystify some of the entry-level nuances and problem areas of Go. These are some of our favorites to get you up and running quickly.

### Quirks and Gotchas

#### Quirk: opening braces cannot start on a new line

Here are some examples of how this is invalid:

```text
func foo()
{
  ...
}

if (...)
{
}
```

Instead, the opening `{` marker must be on the preceding line:

```text
func foo() {
  ...
}

if (...) {
}
```

There's a loophole in this, though, you can have a standlone block of code, which maintains its own scope of variables declared, by using a set of curly braces to outline that code:

```text
func foo() {
  ...

  {
    // standalone code in a block
  }
  
  ...
}
```



#### Gotcha: you cannot have unused variables or imported packages

If you declare it, you must use it.

```text
package main

func main() {
  var myUnusedVariable
}

// this will cause a compiler error that myUnusedVariable
// is "declared but not used"
```

Likewise, if you import a package that you do not use, the Go compiler will give a similar error:

```text
package main

import "math"

func main() {
  a := 5
}

// this will cause a compiler error:
//   imported and not used: "math"
```

#### 

#### Quirk: ... but unused variables are okay if they are globally scoped

```text
package main

// declaring a variable here that remains unused is totally okay
var myUnusedVariable

func main() {
  a:= 5
}
```

#### 

#### Gotcha: you can declare variables on the fly, but must use :

In many high level languages, you can create variables on the go like this:

```text
a = 5
```

In Go, you have to declare variables using the var keyword and data type, or you must use `:=` for the assignment which will do its best to auto-detect the type.

```text
var a int
a = 5

// or

a := 5
```

In our experience, declaring the variable with a type that you have chosen will lead to fewer problems later.

The type inference from Go will depend a lot on the precision of the value you're storing. For example, `i := 42` will become an int \(whose size depends on your compiled architecture\), `i := 3.14` will become a float64, and `i := 0.5i` will become a complex128

#### 

#### Gotcha: the := operator can only be used within functions

Globally-scoped initialized variables cannot use the := operator, you can only use that within the scope of a function.

#### 

#### Quirk: swapping variable values in one line

One of our favorite Python tricks is also in Go:

```text
a := 5
b := 6

a, b = b, a

// b is now 5 and a is now 6
```

#### Quirk: data types are declared "after" the names

```text
var myIntegerValue int

func myFunction(param1 string, param2 float) bool {
  // parameter 1 is a string
  // parameter 2 is a float
  // the function will return a boolean
}
```

#### 

#### Quirk: functions can return multiple values, but the data types have to be in parentheses

```text
func myFunction() (bool, uint32) {
  // the function will return a boolean as its first value
  // and then an unsigned 32-bit integer
}
```



#### Quirk: there are a LOT of numeric types

**Integers**

| basic type name | bits |
| :--- | :--- |
| int | depends on architecture, minimum of 32 bits |
| int8 / uint8 | 8 bits, -127 to 127, or 0 to 255 |
| byte | alias for uint8 |
| int16 / uint16 | 16 bits, -32767 to 32767 or 0 to 65535 |
| int32 / uint32 | 32 bits, -2B to +2B or 0 to 4B \(approx\) |
| rune | alias for unit32, used for Unicode |
| int64 / uint64 | 64 bit, -9Q to +9 Q, or 0 to 18 Quintillion \(approx\) |

**Floats**

* float32, float64, representing their 32-bit and 64-bit values, respectively

**Complex Numbers**

* complex64, complex128, representing their 64-bit and 128-bit values, respectively

#### Gotcha: "int" depends on your architecture; it's best to be explicit

"int" is not an alias for int32 or uint32 or int64 or uint64. It is its own distinct data type and will be a minimum of 32 bits in size.

You can read more about integer types in the [Go Documentation](https://golang.org/pkg/builtin/#int).



#### Quirk: Go does not have try/catch for exceptions like other languages

You can emulate exception catching using panic\(\) and recover\(\), but it's very bad practice to do so.



#### Gotcha: Somebody, somewhere, needs to handle that error

It's very common in Go to have code return either data OR an error like this:

```text
data, err := getData()
if err != nil {
  // handle the error
}
```

Unfortunately, Go also allows us to ignore return values using an underscore, and sometimes you'll see code written like this:

```text
data, _ := getData()
```

This syntax is okay, but it ignores handling an error. Commonly you might also see errors being passed back, effectively making the error handling "someone else's job":

```text
func methodOne(a int) (int, err) {
  if (a < 5) {
    return a, errors.New("a is less than 5")
  }
  
  return a+5, nil
}

func methodTwo(b int) (int, err) {
  b, err = methodOne(b)
  if err != nil {
    return b, err
  }
  return b, nil
}

func methodThree(c int) (int, err) {
  c, err = methodTwo(c)
  if err != nil {
    return c, err
  }
  return c, nil
}

// ... you get the idea
```

At some point, someone should handle the fact that an integer value is less than 5 instead of delaying the error handling until much much later in the process.



#### Quirk: Go lets you handle errors in a single line of code

You can write code this way:

```text
value := myMethod()
if value < 5 {
  // do something
}
```

Or you can streamline your code like this:

```text
if value := myMethod(); value < 5 {
  // do something
  // value is only scoped inside this block
}

// "value" as a variable does not exist afterward!

// you can also do this in an if/else block:

if value := myMethod(); value < 5 {
  // do something
  // value is only scoped inside this block
} else if value == 5 {
  // do something else
} else {
  // do something else still
}
```





### Wrap-up

Go is a little bit strange sometimes. Hope this helped you find a few key things to started on the programming language quickly!

