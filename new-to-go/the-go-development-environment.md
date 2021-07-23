---
description: 'How to install Go, which editor we prefer, and how to get started.'
---

# The Go Development Environment

### Learning Goals

* learn how to install the Go programming language on your MacBook
* how to test that things are working as expected

### Background

We need to get you up and running quickly so you can be a productive member of the team. Here are the instructions to install the tools we would like you to use.

### Content

#### Installing Go

The easiest way to get started is using Homebrew:

* `brew install go`

If homebrew fails to install the Go package, you will need to ask for assistance.

#### Testing that Everything is Working

**Test the Environment**

If your environment is set up correctly so far, you should be able to successfully run this command in your terminal:

```text
go get github.com/GetStream/stream-go2/v5
```

You should see output like this:

```text
go: downloading github.com/GetStream/stream-go2 v1.14.0
go: downloading github.com/GetStream/stream-go2/v5 v5.7.1
go: downloading github.com/GetStream/stream-go2 v3.2.1+incompatible
go: downloading github.com/fatih/structs v1.1.0
go: downloading github.com/mitchellh/mapstructure v1.2.2
go: downloading github.com/dgrijalva/jwt-go v3.2.0+incompatible
go: downloading github.com/dgrijalva/jwt-go v1.0.2
```

If you get an error of some sort, please ask someone on the team to help you diagnose the error.

**Test the Compiler**

Create a new folder, and within that folder, create a file called `main.go` and paste this content inside that file:

```text
// main .go
package "main"

import "fmt"

func main() {
  fmt.Println("Hello World!")
}
```

In your terminal, run this:

```text
go run main.go
```

If you see your "Hello World!" string, then Go successfully compiled your code. If you got an error, please ask someone on the team to help you diagnose the error.

#### Editor of Choice

You are welcome to use any editor you like here at Stream. Many of our team use [Jetbrains Goland](https://www.jetbrains.com/go/). Ask your team lead about getting a license for the IDE. The integrated debugger is a great tool for learning Go by setting breakpoints and seeing what's going on with your code while it runs.

### Wrap-Up

If you got this far, your system should be all set to start building Go applications.

If you are a Stream employee, please see our private documentation on finishing the setup of your development environment for Stream-specific application development.

