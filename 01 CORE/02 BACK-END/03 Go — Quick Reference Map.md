---
title: "Go - Quick Reference Map"
type: doc-post
framework: Go
tags:
  - go
  - golang
  - backend
  - programming
  - cheatsheet
  - quick-reference
created: 2025-12-26
---
****

## 🔹 1.1 Go Fundamentals & Setup

- **History & Philosophy** : simplicity, readability, concurrency, performance
    
- **Versions** : Go 1.23+ highlights
    
- **Installation** : binaries, package managers
    
- **GOPATH vs Modules** : legacy vs modern workflow
    
- **Go Command** : `go run`, `go build`, `go test`, `go mod`, `go env`, `go get`
    
- **Workspace** : module-based layout
    
- **Module Files** : `go.mod`, `go.sum`
    
- **Environment Vars** : `GOOS`, `GOARCH`, `GOROOT`, `GOPATH`
    
- **Editors/IDEs** : VS Code, GoLand
    
- **Formatting** : `gofmt`, `goimports`
    
- **Analysis** : `go vet`, staticcheck
    
- **Proxies & Checksums** : module proxy, checksum DB
    



## 🔹 1.2 Basic Syntax & Data Types

- **Packages** : `package main`, entrypoint
    
- **Imports** : grouped imports
    
- **Variables** : `var`, short `:=`, zero values
    
- **Constants** : `const`, `iota`
    
- **Basic Types** : `bool`, `string`, `int/uint*`, `float32/64`, `complex*`
    
- **Byte & Rune** : `byte` (uint8), `rune` (int32)
    
- **Type Aliases & Conversions**
    
- **Pointers** : `*T`, `&`, `new`
    
- **Structs** : fields, anonymous fields
    
- **Arrays & Slices**
    
    - `make`, `append`
        
    - `len`, `cap`, slicing
        
- **Maps** : `make`, `delete`, `range`
    



## 🔹 1.3 Control Structures & Functions

- **Conditionals** : `if` with init
    
- **Switch** : expression, type switch, no fallthrough by default
    
- **Loops** : `for` (classic, range, while-like)
    
- **Defer** : delayed execution
    
- **Panic/Recover** : controlled crashes
    
- **Functions**
    
    - Multiple returns
        
    - Named returns
        
    - Variadic `...`
        
- **Anonymous & Closures**
    
- **Init Functions** : `init()` hooks
    



## 🔹 1.4 Methods, Interfaces & Embedding

- **Methods** : receiver types (value vs pointer)
    
- **Method Values / Expressions**
    
- **Interfaces**
    
    - Implicit satisfaction
        
    - Empty interface `interface{}`
        
- **Type Assertions** : `v, ok := x.(T)`
    
- **Type Switches**
    
- **Interface Embedding**
    
- **Struct Embedding** : field promotion
    
- **Design** : composition over inheritance
    



## 🔹 1.5 Concurrency with Goroutines & Channels

- **Goroutines** : `go f()`
    
- **Channels** : `make`, send/receive `<-`, buffered
    
- **Select** : multiplexing
    
- **sync Package**
    
    - `WaitGroup`
        
    - `Mutex`, `RWMutex`
        
    - `Once`, `Cond`
        
- **Context** : cancellation, deadlines, values
    
- **Atomic Ops** : `sync/atomic`
    
- **Patterns** : worker pools, fan-out/fan-in, pipelines
    



## 🔹 1.6 Error Handling & Logging

- **Error Type** : `error` interface
    
- **Custom Errors**
    
- **Wrapping** : `fmt.Errorf("%w")`
    
- **Inspection** : `errors.Is`, `errors.As`
    
- **Panic/Recover** : last resort
    
- **Logging**
    
    - Standard `log`
        
    - Structured `log/slog` (Go 1.21+)
        
    - Third-party : zap, zerolog
        



## 🔹 1.7 Standard Library Highlights

- **Formatting & Strings** : `fmt`, `strings`
    
- **I/O** : `io`, `bufio`
    
- **HTTP** : `net/http` (server, client, middleware)
    
- **Encoding** : `json`, `xml`, `gob`
    
- **OS & FS** : `os`, `filepath`
    
- **Time** : `time`, timers, tickers
    
- **CLI** : `flag`, cobra, viper
    
- **Testing** : `testing` (tests, benchmarks, examples)
    
- **Reflection** : `reflect`
    
- **Unsafe** : `unsafe` (rare, careful use)
    



## 🔹 1.8 Generics (Go 1.18+)

- **Type Parameters** : `[T any]`
    
- **Constraints** : `any`, `comparable`, `~T`
    
- **Generic Functions & Types**
    
- **Methods with Generics**
    
- **Type Sets** : unions & approximation
    
- **Patterns** : stacks, queues, option/result
    



## 🔹 1.9 Modules, Tooling & Dependency Management

- **Go Modules Deep Dive**
    
    - `require`, `replace`, `exclude`
        
- **MVS** : minimal version selection
    
- **Workspaces** : `go work`
    
- **Vendoring** : `go mod vendor`
    
- **Build Tags** : conditional builds
    
- **Profiling** : `pprof`
    
- **Tracing** : execution tracing
    
- **Fuzzing** : `go test -fuzz`
    



## 🔹 1.10 Advanced & Modern Features

- **Struct Tags** : `json:"id,omitempty"`
    
- **Reflection Use Cases**
    
- **Internals** : slices & maps (Go 1.21+ improvements)
    
- **Iterators** : ranges over functions (Go 1.23)
    
- **Workspace Enhancements**
    
- **Structured Logging** : `slog`
    
- **Time Enhancements**
    
- **Generics Best Practices**
    
- **Error Wrapping Improvements**
    
- **Context-Aware Patterns**
    



## 🔹 1.11 Ecosystem & Best Practices

- **Testing** : table-driven tests, subtests, coverage
    
- **Benchmarking** : `go test -bench`
    
- **Optimization** : profiling, hot paths
    
- **Style** : _Effective Go_, idioms
    
- **Dependency Hygiene**
    
- **Concurrency Patterns** : pipelines, pools
    
- **Error Conventions** : sentinel vs wrapped
    
- **Performance Tuning**
    
- **Cross-Compilation** : `GOOS/GOARCH`
    
- **Frameworks**
    
    - Web : Gin, Echo, Fiber
        
    - RPC : `grpc-go`
        



If you want, I can next:

- Add **`[[Wiki Links]]`** for each Go section (e.g., `[[Go Concurrency]]`).
    
- Merge this into a **Languages Master Index** with Python, PHP, JS, etc.
    
- Or distill it into a **one-page Go cheatsheet** for interviews and quick revision.