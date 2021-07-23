---
description: >-
  For more advanced Go developers, here are some of our favorite tips and
  concerns.
---

# Advanced Quirks and Gotchas

### Learning Goals

* "quirk" -- things about the Go language that are unique, and parallels to other languages
* "gotcha" -- common pitfall that Go developers will find themselves in which can cause errors or unexpected behavior

### Background

Similar to our beginner-level article, these quirks and gotchas will give you some insight into some common problem areas you might run into when working in Go.

{% page-ref page="../new-to-go/beginner-quirks-and-gotchas.md" %}

### Quirks and Gotchas

#### Gotcha: the := operator can redeclare a variable of the same name in a different scope

Some examples:

```text
func myFunction() {

  // Go will infer a data type for "a"
  a := 3.14
  fmt.Println(a) // would print 3.14
  
  // redeclaring "a"
  a, b := 6, 7
  fmt.Println(a) // would print 6
  
  {
    // this is called a "shadow variable" and redclares "a"
    a := "hello"
    fmt.Println(a) // would print "hello"
    
    // this declaration gets garbage-collected when this block ends
  }
  
  fmt.Println(a) // would print "6" from line 8
```

#### 

#### Quirk: functions parameters and return data types can share a common data type \(listed at the end\), but only if each of the parameters is named

```text
func myFunction(param1, param2 int) (return1, return2 string) {
  // param1 and param2 will both be integers
  
  // return1 and return 2 will both be strings
}
```

The benefit of naming the return value\(s\) is that it/they are pre-declared so you don't need to explicitly create them within your function.

You can do this over and over again too:

```text
func something(param1, param2 string, param3, param4 int) {
  // param1 and param2 are strings
  // param3 and param4 are integers
}
```

#### 

#### Quirk: uninitialized primitives are given default values

```text
// all numeric types are 0 by default, examples:
var myIntegerValue int // 0 by default
var myFloatValue float64 // 0 by default

var myBoolean bool // false by default

var myString string // "" by default
```



#### Quirk: Clean up after yourself with "deferred" methods

When you use the defer keyword in Go, it queues your work to happen once your current method finishes. This is helpful if you want to manually free up memory or log some data.

If you queue up multiple defer statements, they execute in the reverse order you queued them.

```text
defer fmt.Println("this gets called third")
defer fmt.Println("this gets called second")
defer fmt.Println("this gets called first")
```

#### 

#### Gotcha: defer evaluates arguments at queue time, not later

defer can also take arguments, but the arguments are handled when the defer is queued, not later when the code is called.

```text
func doSomething() {
  a int = 5
  b int = 6

  defer fmt.Println(x + y)

  a = 10
  b = 20
}

// the Println would print 11, not 30
```



### Wrap-Up

If you have any tips or insight into the Go programming to share, please share them with us!

{% page-ref page="../contributing.md" %}



