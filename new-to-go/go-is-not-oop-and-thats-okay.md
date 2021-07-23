---
description: >-
  The Go language is not object-oriented, but many principles of OOP, and good
  programming practices can be implemented and followed.
---

# Go is not OOP, and that's okay

### Learning Goals

* Discovering how we can mimic OOP design in Go

### Background

Getting started with the Go programming language can tricky when coming from other programming languages. Like other programming languages, Go also implements different/uncommon names for some primitive data types. If you are getting started with Go and coming from an Object-Oriented Programming \(PP{\) background, you might find Go quirky.

As a preface to our Go Gotchas article, we wanted to provide a brief introduction to how Go is NOT an object-oriented language. The good news is that we can still implement several OOP principles. This will help avoid thinking about a "feature" of the language as a "gotcha" in our later article.

Go was invented by Google in 2007, and by 2019 has become a top language for developers. It continues to mature as a language, and keeping up with changes is an interesting challenge.

Stream switched from Python to Go in 2017, which we have covered in other blog articles over the years.

* [Why we switched from Python to Go](https://getstream.io/blog/switched-python-go/)
* Switching from Python to Go, [Part 1](https://dzone.com/articles/why-we-switched-from-python-to-go) and [Part 2](https://dzone.com/articles/why-we-switched-from-python-to-go-part-2)

### Content

#### What Go IS and IS NOT

To get started, simply:

Go is:

* a compiled language, [many times faster than Python](https://benchmarksgame-team.pages.debian.net/benchmarksgame/fastest/go-python3.html) but [slower than C++](https://benchmarksgame-team.pages.debian.net/benchmarksgame/fastest/go-gpp.html), and has [wins/losses compared to Java](https://benchmarksgame-team.pages.debian.net/benchmarksgame/fastest/go.html)
  * able to compile and run on multiple CPU architectures
* needs an OS to launch and execute its binary files, cannot run on bare metal
* benefits strongly from many [SOLID](https://en.wikipedia.org/wiki/SOLID) design patterns
  * Single Responsibility Principle \(SRP\)
  * Open-Closed Principle \(open for extension, closed for modification\)
  * Interface segregation \(many specific interfaces are better than fewer general-purpose interfaces\)
  * [Dependency inversion](https://itnext.io/using-dependency-inversion-in-go-31d8bf9b3760) \(high-level logic should not depend on low-level implementations\)

Go is NOT:

* object-oriented, though most principles can still apply:
  * Abstraction
  * Encapsulation
  * Polymorphism
* able to use inheritance patterns
  * if you really need to, you can embed structs within structs, but this is uncommon

Let's Dive Deeper into the non-OOP nature of Go!

**Single-Responsibility Modules**

Like Python, Go benefits from **Single-Responsibility Principle** \(SRP\). Ideally, a module is responsible for one thing. As the file system gets deeper, we want those modules to be hyper-specific. Modules higher in the file structure are more general in nature and consume \(use\) the more specific modules deeper in the structure.

For example, let's imagine we have a single Go module that takes in some user input, does some data processing, and logs the data. It would be better to split the files into an abstracted/encapsulated structure such as:

* our main executable code file, which takes user input
  * a module that processes data passed into it
  * a module that logs data passed into it

Now, our main package can use dependency injection to pass data into each of the other modules to do their work. And we can reuse those other specific modules in other projects as well.

**Polymorphism**

Developers can implement interfaces in Go, a set of method signatures. When a custom data type is created, it can implement an interface that you create, allowing for polymorphism to take place.

As a working example, let's imagine that we have an interface to find numbers in a string:

```text
// interface is defined here
type NumberFinder interface {
	FindNumbers() []rune
}
```

Next, we can make a custom string type that takes in a home address and we want to extract just the numbers in the address.

```text
type HomeAddress string

func (ha HomeAddress) FindNumbers() []rune {
	var digits []rune
	for _, ch := range ha {
		if uint8(ch) >= uint8('0') && uint8(ch) <= uint8('9') {
			digits = append(digits, ch)
		}
	}
	return digits
}
```

Our `HomeAddress` string now implements the interface for `FindNumbers`

```text
func main() {
  // the string from which we want to extract numbers
	address := HomeAddress("123 Main Street")
	
	// our interface
	var nf NumberFinder
	
	// this next line is valid syntax ONLY IF our HomeAddress
	// implements the FindNumbers method
	nf = address
	
	fmt.Printf("numbers found in the string: %c", nf.FindNumbers())
}
```

[Run this in the Go Playground](https://play.golang.org/p/5lBIBqJWln_I)

Unlike other languages that use terminology to explicitly state that it "implements" an interface, Go is more subtle. As long as you implement all of the methods declared in the interface, you have achieved polymorphism.

### Wrap-Up

For those coming from other object-oriented programming languages, Go can seem strange at first. We can still follow many patterns we use in OOP when developing our code in Go.

