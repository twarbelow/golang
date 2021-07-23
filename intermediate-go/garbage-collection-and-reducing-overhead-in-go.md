---
description: How to ease the overhead of cleaning up after yourself.
---

# Garbage Collection, and reducing overhead in Go

### Learning Goals

### Background

One of the first steps in optimizing a Go application is tweaking the garbage collection and fine-tuning Go routines and background tasks.

The developers of the language did something really clever when they build Go. The compiled binary can act differently based on operating system [environment variables that you set](https://pkg.go.dev/runtime).

### Garbage Collection in Go

When dealing with garbage collection in Go, there are two environment settings we can tune for performance. GOGC and GOMAXPROCS.

In a nutshell, there are two things you do not want to do:

* rely on Go's default settings
* manually figure out how to optimize your settings, as this can be extremely time-consuming

#### GOGC

GOGC is the key for the primary Garbage Collector, and it can be set to "off" or a numeric value with a default of 100 representing a percentage; a negative value is the same as "off". The percentage is a "ratio of freshly allocated data to live data remaining after the previous collection reaches this percentage."

GODEBUG is an environment variable you can use to see output on stderr when collection is happening, and from where.

You can also trigger garbage collection by using the "runtime" package and calling `GC()` within your code.

Your code can also configure the garbage collection percentage by using the "debug" package and calling `SetGCPercent()` and pass it an integer value. The method will return the previous setting.

The debug package also gives you access to ReadGCStats\(\) which will give you a [struct of statistics](https://cs.opensource.google/go/go/+/refs/tags/go1.16.6:src/runtime/debug/garbage.go;drc=refs%2Ftags%2Fgo1.16.6;l=14) including the time GC was last run, the number of collections, and some important "pause" stats of your garbage collection so far.

#### So What Do We Do?

Unfortunately, the best tuning is done manually right now. There are some tools like [ExpvarMon](https://github.com/divan/expvarmon) to monitor your application while it runs.

We can set the GODEBUG environment variable to show us the garbage collection tracing:

`GODEBUG="gctrace=1"`

This will output a LOT of data, showing you just how often your code is doing garbage collection. You can benchmark this programmatically where you set GOGC to different values to find something that meets your needs:

```text
for i in `seq 0 20000 100`; do
  GODEBUG="gctrace-1" GOGC=$i go run yourapp.go 2>&1 > output.gc-$i.txt
done
```

#### Taking out Your Own Garbage

An alternative approach, though less ideal, is to turn Garbage Collection off completely and manually calling the Garbage Collection. Setting the Garbage Collection this way will return the previous setting if you want to reset it later.

```text
// turn off garbage collection completely and save the previous setting:

var int oldGCsetting
oldGCsetting = debug.SetGCPercent(-1)


// trigger garbage collection yourself:
runtime.GC()


// reset garbage colelction to its previous setting:
debug.SetGCPercent(oldGCsetting)
```

### Wrap-up

Tuning garbage collection in Go is a very necessary step in our development process. It can be examined manually, which can be time-consuming, but it is extremely important.

Here are some additional articles that discuss manual tuning:

* [https://blog.cloudflare.com/go-dont-collect-my-garbage/](https://blog.cloudflare.com/go-dont-collect-my-garbage/)
* 
