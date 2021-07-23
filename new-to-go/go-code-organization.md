---
description: What does a typical Go project look like? Modules? Packages?
---

# Go Code Organization

### Learning Goals

* packages versus modules

### Background

Structuring a Go project takes careful planning to properly follow the Single-Responsibility Principle.

### Content

You can develop Go in two primary ways:

* a full application
* a library of code for another application to use

Every application is made up of, and possibly uses, packages.

#### What's the "main" difference between a package and an application?

If it doesn't have a "main" function, it's a package for others to consume.

Typically, consumable packages to add to other packages will use a package name declaration that is something other than "main", like `package "stream"`

#### Folder Layout

A typical Go application will have files in the root folder of the source code, where you'll usually find a "main.go" file. \(this makes it very easy to location the "main" function that gets everything started\) This level of your application will generally use `package "main"` to declare itself.

You might also see files in that folder that end in "\_test.go" which are test files.

If you want to split up your application into more manageable sizes, you can move methods into other files, but they must also be part of the same package as other source files in the same folder.

#### Subfolders are for more specific things, and are their own packages

If you find you want even more granularity in your application, you can move code into sub-folders. This will start to incorporate Single Responsibility Principle \(SRP\) where packages in sub-folders become more specific about their role in your application.

#### Example of Good Layout and SRP

* root folder, `main.go`:

```text
package "main"

import (
    "logging"
    "user_input"
)

func main() {
    user_name := user_input.GetUserName()
    logging.WriteData(user_name)
}
```

* subfolder `user_input`, `user_input.go`:

```text
package "user_input"

import "fmt"

func GetUserName() string {
    var userInput string
    fmt.Println("what is your name? ")
    fmt.Scanln(&userInput)
}
```

* subfolder `logging`, `logging.go`:

```text
package "logging"

func WriteData(input string) {
    f, err := os.OpenFile("output.log", os.O_APPEND|os.O_WRONLY|os.O_CREATE, 0600)
    if err != nil {
        panic(err)
    }
    
    defer f.Close()
    
    if _, err = f.WriteString(text); err != nil {
        panic(err)
    }
}
```

This layout will allow us to extract the input\_input and logging packages as their own standalone Go modules, and use them in other projects.



### Wrap-Up

Every application is a "package", which may use other packages. The other packages can be embedded subfolders, or they can be imported from



