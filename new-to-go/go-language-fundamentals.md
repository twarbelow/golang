---
description: >-
  If you're coming from another programming language, here are some things
  you'll need to know about Go programming.
---

# Go Language Fundamentals

### Learning Goals

* the basics of the Go language

### Background

At Stream, we recognize that not everybody has spent time learning Go before applying for a job, or may not be as familar with the language as other developers. We have written this content to get you up to speed quickly on the important things you need to get started.

### Content

Some basics we'll cover here:

* Go is a compiled language; you compile your source code once and it'll run without the need for an interpreter like Python or JavaScript.
* The structure of the language will feel a bit backwards compared to C++/Java, in that data types are declared after parameters

#### Go is a Compiled Language

We have covered this in The Co Compiler page. You write your code, you compile it, you ship the binary. No extra interpreter is needed, and your production environment does not need to download additional third-party libraries

#### Go At a Glance

Go has several beginner-level "gotchas" and "quirks" that will trip you up, and we'll cover those in another article, but here are some basics to get started:

* curly braces cannot be on a line by themselves after function declarations, if statements, for loops, etc.
* data types are declared after, not before, eg `var myGreeting string`
* there are a LOT of data types you can use, and lots of choice for numeric types
* there's no "public" or "private" declarations, any method in a package with an uppercase first letter \(eg `Println`\) is exported as "public" where any method that starts with a lowercase first letter \(eg `myPrivateMethod`\) is not exported and only usable within the scope of the package itself
* you can have several files in a folder to separate your code, but only one can have a "main" function, and it will usually be called "main.go"; all other files in that same folder need to be part of the same "package", or be a test file
* test files are named after the package of that folder, but ends in `"_test"`; for example if you have `package "main"` in your code, your tests would be `package "main_test"`
* You can declare variables on-the-go using `:=` and Go will do its best to declare the correct type, but it can only be used within functions \(eg, `a := 5`\)



### Wrap-Up





