class: center, middle

# 你有所不知的 Julia 語言特性

## Julia 到底怎樣比其他語言好？

### Yueh-Hua Tu
#### 2021.3.12

---
class: middle

Programming languages have their own purpose or design philosophy.

--

Programming language, as a tool, should solve certain range of problems, but seek to be general as possible.

--

A rising of programming language is related to their historical background.

--

Thus, programming languages have plenty of language features.

---
class: middle

# If you want to introduce a Julia to your friend, what will you tell?

---
class: middle

## Some well-known Julia features

**Dynamic type**

--

**Optional typed**

--

**High performance**

--

**Open source**

---
class: middle

## Some technical Julia features

**Strong type**

--

**Type inference**

--

**Just-in-time (JIT) compilation**

--

**Implemented using LLVM**

--

**Garbage collection**

---
## Julia

Julia come with scientific computing.

--

Julia is deisgned for scientists or domain experts.

--

Experts are not expected to familiar with computer science or computer engineering.

--

Julia should be as easy to pick up as possible.

--

Thus, Julia is for everyone!

---
## We will focus on "similar" languages today.

--

For the usage of languages, python, R and MATLAB are the languages usually used in scientific computing and numerical computing.

<img src="../../pics/py-R-mat.svg" width="70%" style="background-color:white;display:block;margin-left:auto;margin-right: auto;">

---
class: middle

### Julia feature you don't know
# Expressiveness

---
## Expressiveness of Julia

<img src="../../pics/julia-logo.svg" width="15%" style="background-color:white;display:block;margin-left:auto;margin-right: auto;">

```julia
function sort!(v::AbstractVector, lo::Integer, hi::Integer, a::QuickSortAlg, o::Ordering)
    @inbounds while lo < hi
        hi-lo <= SMALL_THRESHOLD && return sort!(v, lo, hi, SMALL_ALGORITHM, o)
        j = partition!(v, lo, hi, o)
        if j-lo < hi-j
            # recurse on the smaller chunk
            # this is necessary to preserve O(log(n))
            # stack space in the worst case (rather than O(n))
            lo < (j-1) && sort!(v, lo, j-1, a, o)
            lo = j+1
        else
            j+1 < hi && sort!(v, j+1, hi, a, o)
            hi = j-1
        end
    end
    return v
end
```

[base/sort.jl](https://github.com/JuliaLang/julia/blob/bb5a0130c595bb3595713b9cd05aea403a4cd095/base/sort.jl#L597)

---
## Expressiveness of R

<img src="../../pics/R-logo.svg" width="10%" style="background-color:white;display:block;margin-left:auto;margin-right: auto;">

```r
sort.int <-
    function(x, partial = NULL, na.last = NA, decreasing = FALSE,
             method = c("auto", "shell", "quick", "radix"),
             index.return = FALSE)
{
    ## fastpass
    decreasing <- as.logical(decreasing)
    if (is.null(partial) && !index.return && is.numeric(x)) {
        ...
    }
    method <- match.arg(method)
    if (method == "auto" && is.null(partial) &&
        (is.numeric(x) || is.factor(x) || is.logical(x)) &&
        is.integer(length(x)))
        method <- "radix"
    if (method == "radix") {
        ...
    }
    else if (method == "auto" || !is.numeric(x))
        ...
```

[sort.R](https://github.com/wch/r-source/blob/ce3d35c3d06120e4e481d6eae40e745aa5ee80c5/src/library/base/R/sort.R#L78)

---
## Expressiveness of Python

<img src="../../pics/python-logo.svg" width="20%" style="background-color:white;display:block;margin-left:auto;margin-right: auto;">

Well... it is written in C.

[listobject.c](https://github.com/python/cpython/blob/ba251c2ae6654bfc8abd9d886b219698ad34ac3c/Objects/listobject.c#L1051)

---
class: middle

### Julia feature you don't know
# Integration of all spectrum of operations

---
class: middle

# What extent of granularity can one manipulate in a programming language?

--

Every language supports higher-level operations...

--

Usually, a dynamic language persuits higher-level operations.

---
## If we take a look roughly...

--

#### MATLAB

MATLAB is just a software for scientific computing and a programming language on its own.

--

#### Python

CPython is based on C. C is in the level of programming language.

--

#### R

R is based on C++. C++ is in the level of programming language.

--

#### Julia

Julia is based on LLVM. LLVM is in the level of intermediate representation (IR).

--

Julia supports lower level of operations than other languages.

---
## But the statement is too rough...

Python, R and Julia all have metaprogramming support, which means they can manipulate their own code as data.

Code is highly dynamic, because it can be changed in runtime.

---

### So, what is the minimum granularity can one manipulate?

--

#### MATLAB

MATLAB as a programming language and it is not allowed to access lower level operations than its own language.

--

#### Python

Python doesn't have a pointer in its own language while it can call C by [ctypes](https://docs.python.org/3/library/ctypes.html).

Python can manipulate the pointer to Python object by ctypes.

--

#### R

R has very limited operations in lower level and most of low-level operations rely on C++ in Rcpp.

R can manipulate the pointer to R object by [pointr package](https://cran.r-project.org/web/packages/pointr/index.html).

--

#### Julia

Julia has [the ability to access pointer](https://docs.julialang.org/en/v1/base/c/#Base.pointer) and [the ability to call C](https://docs.julialang.org/en/v1/base/c/#Base.@ccall), even [call LLVM](https://docs.julialang.org/en/v1/base/c/#Core.Intrinsics.llvmcall).

It is possible to insert a line of LLVM IR from Julia!

---
## What is the advantage to support lower level of operations?

--

Lower level of operations means more fine-grained to operate constructs in a programming language.

--

That also means that developers have more power to manipulate the elements of language.

--

#### Power of Julia

Due to the power of Julia, Julia can even change the behavior of compiler by inserting LLVM IR to compiler.

So, Julia makes it possible that highly dynamic changing the behavior of code generation to CUDA from the same source code.

--

#### Example

[JuliaGPU/CUDA.jl](https://github.com/JuliaGPU/CUDA.jl) supports generating NVPTX pseudo-assembly to CUDA driver directly by-passing CUDA C library.

Furthermore, Julia has a wrapper package [maleadt/LLVM.jl](https://github.com/maleadt/LLVM.jl) for manipulating LLVM from Julia.

---
## Why a dynamic language have to poss the ability of lower-level operations?

Low-level operations are supported by static-typed or C-like languages.

These kind of languages have the full power on low-level operations or hardware manipulations.

--

However, dynamic languages usually don't focus on these features.

--

Julia does focus on these lower-level operations, even the intermediate representation-level.

--

Julia posses more power than other languages!

---

## The spectrum of operations

<img src="../../pics/operation-spectrum.svg" width="80%" style="background-color:white;display:block;margin-left:auto;margin-right: auto;">

---
## So, what is the advantage of python?

Python has large and sound developer ecosystem. They comes from all fields, including IT industry, academic (especially scientific community) and some organisations.

Python has plenty of internet resources, which is friendly to newbies.

--

So far, they relie on the success of large projects, e.g. **TensorFlow**, **PyTorch**, which is usually supported by IT gaints (i.g. Google and Facebook).

Also, the foundation is established on the success of numerical computing infrastructure of **Numpy** and **Scipy**.

<img src="../../pics/numpy-logo.png" width="30%" style="background-color:white;display:block;margin-left:auto;margin-right: auto;">

---
class: middle

# Comparison to Rust and Go?

---
## Comparing to rising languages

<img src="../../pics/go-rust.svg" width="70%" style="background-color:white;display:block;margin-left:auto;margin-right: auto;">

Both of them are static-typed and compilation langauges, which is very different from Julia.

Although their design and language feautres are away from Julia, it is still worth for discussion.

---

<img src="../../pics/golang-logo.png" width="20%" style="background-color:white;display:block;margin-left:auto;margin-right: auto;">

Go-lang rises in the background of C is too old. There are much more new syntax or language constructs need brand new design.

Go, as a system programming language, is very close to C but avoiding disadvantages of C. The simpilcity of C's syntax is the core design philosophy. But not C++!

--

* Static type
* Strong type
* Type inference
* Open source

These are similar to Julia.

--

* Structual type

Julia takes nominal type system.

--

* Lack of support for generic programming and functional programming

Type parameters (generic programming) is introduced in Go2.

---

<img src="../../pics/rust-lang-logo.jpg" width="20%" style="background-color:white;display:block;margin-left:auto;margin-right: auto;">

Rust is designed and manintained by Mozillia. Rust aims to be a system programming language.

Rust comes with the feature of memory safety. The memory management and ownership mechanism is awesome.

--

* Static type
* Strong type
* Type inference
* Nominal type
* Open source

These are similar to Julia.

--

* Rust is multi-paradigm, including imperative, functional, generic, concurrent.

But tend to be functional

--

* Rust has no virtual machines but compilations

---
## Simplicity

#### Go

```go
package main
import "fmt"

func plus(a int, b int) int {
    return a + b
}

func plusPlus(a, b, c int) int {
    return a + b + c
}

func main() {
    res := plus(1, 2)
    fmt.Println("1+2 =", res)
    res = plusPlus(1, 2, 3)
    fmt.Println("1+2+3 =", res)
}
```

---
## Simplicity

#### Rust

```rust
fn plus(a: i32, b: i32) -> i32 {
    a + b
}

fn plusPlus(a: i32, b: i32) -> i32 {
    a + b + c
}

fn main() {
	let res = plus(1, 2);
    println!("1+2 =", res);
	let res = plusPlus(1, 2, 3);
    println!("1+2+3 =", res);
}
```

---
## Simplicity

#### Julia

```julia
plus(a::Int64, b::Int64)::Int64 = a + b

plusPlus(a::Int64, b::Int64, c::Int64)::Int64 = a + b + c

function main()
	res = plus(1, 2)
	println("1+2 =", res)
	res = plusPlus(1, 2, 3)
	println("1+2+3 =", res)
end

main()
```

---
## Memory management

#### Go

Use garbage collection.

--

#### Rust

The ownership governs over objects.

If an object reach its lifetime or no more references point to it, the object is deallocated automatically.

Rust insert `deallocate` LLVM IR to the end of a scope in compile time.

--

#### Julia

Use garbage collection.

For a highly dynamic language, it is hard to determine the lifetime of an object.

---
## Concurrency

#### Go

Well-known for its goroutines and channels.

Threads and locks.

--

#### Rust

Have coroutines and channels.

Threads and locks.

--

#### Julia

Also have Task (coroutines) and channels.

Threads and locks.

---
class: middle

### Julia feature you don't know
# Composable

---
## Composable

Composable is the key feature to Julia.

The core Julia developer Lyndon wrote an [article](https://white.ucc.asn.au/2020/02/09/whycompositionaljulia.html) for elaborate the importance of composablility.

I also wrote another [article](https://yuehhua.github.io/2020/04/11/composable-the-key-feature-in-julia/) about my thoughts and conclusions.

--

#### Examples

If you have a package for solving differential equations ([DifferentialEquations.jl](https://github.com/JuliaDiffEq/DifferentialEquations.jl)) and a package for training a neural network ([Flux.jl](https://github.com/FluxML/Flux.jl)), then you should have neural ODE.

---

<img src="../../pics/diffeq.png" width="100%" style="background-color:white;display:block;margin-left:auto;margin-right: auto;">

---

<img src="../../pics/flux.png" width="100%" style="background-color:white;display:block;margin-left:auto;margin-right: auto;">

---

<img src="../../pics/neural-ode.png" width="100%" style="background-color:white;display:block;margin-left:auto;margin-right: auto;">

---

<img src="../../pics/flux-2.png" width="100%" style="background-color:white;display:block;margin-left:auto;margin-right: auto;">

---

<img src="../../pics/graphs.png" width="100%" style="background-color:white;display:block;margin-left:auto;margin-right: auto;">

---

<img src="../../pics/geometricflux.png" width="100%" style="background-color:white;display:block;margin-left:auto;margin-right: auto;">

---

<img src="../../pics/julia-composable.png" width="100%" style="background-color:white;display:block;margin-left:auto;margin-right: auto;">

---
## Weak namespace

Composablility comes from the weak namespace.

```julia
julia> v = Vector{Float64}()
Float64[]

julia> length(v)
0
```

--

```julia
julia> using DataStructures

julia> deq = Deque{Float64}()
Deque [Float64[]]

julia> length(deq)
0
```

--

```julia
julia> methods(length, (Array, ))
# 1 method for generic function "length":
[1] length(a::Array) in Base at array.jl:219

julia> methods(length, (Deque, ))
# 1 method for generic function "length":
[1] length(q::Deque) in DataStructures at /home/yuehhua/.julia/packages/DataStructures/DLSxi/src/deque.jl:85
```

---
## Weak namespace

The intuition of namespace is to **separate** identifiers from module to module.

That way, we can define the same identifier in different namespace.

It prevents from mixing identifiers which have **different semantics in distinct contexts**.

--

However, Julia breaks the namespace and want developers to discuss with each other.

--

The supoort of `length`

* Object of `Array` type is supported by `Base`
* Object of `Deque` type is supported by `DataStructures`

--

Packages **collaboratively support** the semantics of `length` to be *"the amount of elements in the collection"*.

--

Once a new package want to support the functionality of `length`, it supports its object on its own.

--

This forces Julia developers to discuss and maintain the semantics of functions.

---
## Module

Julia developers have the right to control over the visibility of a function or a type.

The visibility is also weak.

--

```julia
module Foo

export bar;

bar() = println("this function is visible in public.")

baz() = println("this function is only visible in Foo module.")

end
```

--

```julia
using Foo

bar()

Foo.baz()
```

---
## Interface

Julia developers should **collaboratively support** the semantics of interfaces in stdlib or packages.

--

If a language posses shared semantics and well-defined interfaces, modules in Julia are **composable**.

--

#### In Python

This doesn't happened in TensorFlow and PyTorch. PyTorch cannot use the functions defined in TensorFlow.

--

#### In Julia

It is possible to collectively use Flux and MLJ.

--

#### Julia as a plugable system

Modules in Julia are plug-ins.

If you plug MLJ and Flux, Julia becomes a machine learning engine.

If you plug DataFrames, Pipe and some database utilities, Julia becomes a data pipeline.

---
class: middle

# Use it as a WHOLE!

---
class: middle

### Julia feature you don't know
# Reproducible

---
## Reproducible

Julia's packages are on Github and open source.

--

#### Package management

The [Pkg3](https://github.com/JuliaLang/Pkg.jl), as a package management, maintains the installation and package dependency.

A package management ease the process of installation of a package.

--

#### Package code quality

To enhance reproducibility, continuous integration (CI) is needed for automatic testing the developing packages.

Julia developers work hard to add new features, get tests passed and increase testing coverage.

CI ensure certain extent of code quality.

--

#### Provide libraries and binaries

To have its own library, instead of depending on the user's OS environment, [JuliaPackaging/BinaryBuilder.jl](https://github.com/JuliaPackaging/BinaryBuilder.jl) provides the ability to install library from Julia's package management system.

---
## Reproducible

#### Container

Julia's official docker is provided.

Docker ensures the OS environment where Julia executes.

---
class: middle

### Julia feature you don't know
# Compatibility of multi-paradigm

---
## Compatibility of multi-paradigm

Julia supports multi-paradigm. It combines features of imperative, functional, and object-oriented programming.

--

Currently, programming languages support these paradigms.

#### Python

* imperative
* functional
* object-oriented
* concurrent
* metaprogramming

--

#### R

* imperative
* functional
* (weak) object-oriented
* metaprogramming

---
## Compatibility of multi-paradigm

#### MATLAB

* imperative
* functional
* (weak) object-oriented

--

#### Rust

* imperative
* (strongly) functional
* (compatible) object-oriented
* concurrent
* generic programming

--

#### Go

* imperative
* (compatible) object-oriented
* concurrent
* generic programming (coming soon)

---
## Compatibility of multi-paradigm

#### Julia

* imperative
* functional
* (compatible) object-oriented
* concurrent
* generic programming
* metaprogramming

---
## OOP support

#### Types! Not classes!

```julia
julia> struct Foo
           bar
           baz::Int
           qux::Float64
       end
```

--

#### Methods and polymorphisms

Polymorphism is supported implicitly by multiple dispatch.

```julia
julia> v = Vector{Float64}()
Float64[]

julia> length(v)
0

julia> deq = Deque{Float64}()
Deque [Float64[]]

julia> length(deq)
0
```

---
## OOP support

Design patterns are avialable in Julia!

```julia
"""
This pattern can be apply to subscription behavior.
Suppose we want to subscribe for newspapers.
"""
abstract type Newspaper end
abstract type Subscriber end

function subscribe(::Subscriber, ::Newspaper) end
function unsubscribe(::Subscriber, ::Newspaper) end
function notify(::Newspaper) end
```

```julia
struct SubscriberA <: Subscriber
    name::String
end

update(a::SubscriberA, s::String) = println("$(a.name) is notified by $(s).")



struct SubscriberB <: Subscriber
    name::String
end

update(b::SubscriberB, s::String) = println("$(b.name) is notified by $(s).")
```

---
## OOP support

```julia
mutable struct AppleNews <: Newspaper
    subscribers::Vector{Subscriber}
end

AppleNews() = AppleNews(Subscriber[])

subscribe(s::Subscriber, a::AppleNews) = push!(a.subscribers, s)
notify(a::AppleNews) = foreach(x -> update(x, "AppleNews"), a.subscribers)

mutable struct BananaNews <: Newspaper
    subscribers::Set{Subscriber}
end

BananaNews() = BananaNews(Set{Subscriber}())

subscribe(s::Subscriber, b::BananaNews) = union!(b.subscribers, [s])
notify(b::BananaNews) = foreach(x -> update(x, "BananaNews"), b.subscribers)
```

[yuehhua/patterns.jl](https://github.com/yuehhua/patterns.jl)

---
## Generic programming

#### Parametric types

```julia
julia> struct Point{T}
           x::T
           y::T
       end
```

--

#### Parametric methods

```julia
myappend(v::Vector{T}, x::T) where {T} = [v..., x]
```

---
## Go will have type parameters in Go2

```go
func ReverseSlice[T any](s []T) []T {
	first := 0
	last := len(s) - 1
	for first < last {
		s[first], s[last] = s[last], s[first]
		first++
		last--
	}
	return s
}
```

Code from [go2go Playground](https://go2goplay.golang.org/p/doitUP4_1Jm)

---
## Functional support

#### First-class function and anonymous functions

```julia
julia> map(x -> x^2, [1,2,3,4,5])
5-element Array{Int64,1}:
  1
  4
  9
 16
 25
```

---
class: middle

## The Julia features you don't know

* Expressiveness
* Integration of all spectrum of operations
* Composable
* Reproducible
* Compatibility of multi-paradigm

---
class: middle

# Thank you for attention
