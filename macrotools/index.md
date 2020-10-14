
class: center, middle






# MacroTools.jl






### Julia Taiwan meetup






#### Yueh-Hua Tu






#### 2020.10.16


---






# Metaprogramming


It provides a higher point of view for programming.


--






### Program the program


--






### The language view point for programming


  * `5 + 6 * 9` => `+(5, *(6, 9))`
  * I am a human. => S. + V. + O.


--






### The evaluation view point for programming


  * `5 + 6 * 9` => `59`
  * I am a human. => `typeof(I) == Human`


---






# Expression in Julia






### Components


  * Symbol
  * Expr


--






### Symbol


```
julia> :x
```


--






### Expr


```
julia> :(1 + 2 * 3)
```


---




# Expression in Julia


```
julia> expr = :(x = 5)
:(x = 5)

julia> dump(expr)
Expr
  head: Symbol =
  args: Array{Any}((2,))
    1: Symbol x
    2: Int64 5
```


--






### Expr is itself a data structure


```
julia> expr.head
:(=)

julia> expr.args
2-element Array{Any,1}:
  :x
 5
```


---






# Execute an expression


```
5 + 6 * 9
```


--






### Abstract syntax tree


```
    (+)
   /   \
 (5)    (*)
        /  \
      (6)  (9)
```


--






### Evaluation 1


```
    (+)
   /   \
 (5)    (54)
```


--






### Evaluation 2


```
    (59)
```


---






# Execute expression in Julia


```
julia> expr = :(x = 5)
:(x = 5)

julia> x
ERROR: UndefVarError: x not defined
Stacktrace:
 [1] top-level scope
 [2] run_repl(::REPL.AbstractREPL, ::Any) at /build/julia/src/julia-1.5.2/usr/share/julia/stdlib/v1.5/REPL/src/REPL.jl:288
```


--






### Evaluation of expression


```
julia> eval(expr)
5
```


--


```
julia> x
5
```


---






# Expression-level pattern matching - @capture


```
julia> expr = :(x = 5)
:(x = 5)

julia> @capture expr name_=val_
true

julia> name
:x

julia> val
5
```


  * Remind: to add underline after a matching term.


--






### Fail to match


```
julia> expr = :(x = 5)
:(x = 5)

julia> @capture expr name_ = 7
false
```


---




# Expression-level pattern matching - @capture


```
julia> expr = :(foo(x::Bool, y::Int64))
:(foo(x::Bool, y::Int64))

julia> @capture expr fn_(arg1_::T1_, arg2_::T2_)
true

julia> T1
:Bool

julia> T2
:Int64

julia> arg1
:x

julia> fn
:foo
```


---






# Expression traversal - prewalk


```
julia> expr = :(5 + 6 * 9)
:(5 + 6 * 9)

julia> MacroTools.prewalk(x -> @show(x), expr)
x = :(5 + 6 * 9)
x = :+
x = 5
x = :(6 * 9)
x = :*
x = 6
x = 9
:(5 + 6 * 9)
```


--


```
    (+)
   /   \
 (5)    (*)
        /  \
      (6)  (9)
```


---






# Expression traversal - postwalk


```
julia> MacroTools.postwalk(x -> @show(x), expr)
x = :+
x = 5
x = :*
x = 6
x = 9
x = :(6 * 9)
x = :(5 + 6 * 9)
:(5 + 6 * 9)
```


--


```
    (+)
   /   \
 (5)    (*)
        /  \
      (6)  (9)
```


---






# Pattern matching - @match


```
julia> x = @match :(4 + 5) begin
           (x_ + y_) => (x, y)
       end
(4, 5)

julia> x
(4, 5)
```


--






### More patterns


```
julia> x = @match :(4 - 5) begin
           (x_ + y_) => (x, y)
           (x_ - 5) => x
       end
4
```


---




# Pattern matching - @match






### Default values


```
julia> x = @match :(485) begin
           (x_ + y_) => (x, y)
           (x_ - 5) => x
           _ => nothing
       end

julia> x

```


---




# Pattern matching - @match






### Cannot match types


```
julia> expr = :([1, 2, 3, 4])
:([1, 2, 3, 4])

julia> x = @match expr begin
    [] => 0
    [head_, tail_] => 1 + ?(tail)
end
```


--






### Try to mimic Haskell


```
length' :: (Num b) => [a] -> b  
length' [] = 0  
length' (_:xs) = 1 + length' xs  
```


---






# Pipe tools


```
julia> f(x) = 12x
f (generic function with 1 method)

julia> MacroTools.@> 1 f
12
```


--






### Pipe to second function


```
julia> g(x) = 3 + x
g (generic function with 1 method)

julia> MacroTools.@> 1 f g
15
```


--






### Third function with arguments


```
julia> h(x, y) = x - y
h (generic function with 1 method)

julia> MacroTools.@> 1 f g h(10)
5
```


---






# Pipe tools - toward last position


```
julia> MacroTools.@>> 1 f
12
```


--






### Second function with arguments


```
julia> MacroTools.@>> 1 f h(10)
-2
```


--






### More arguments


```
julia> k(w, x, y, z) = w + x - y * z
k (generic function with 1 method)

julia> MacroTools.@>> 1 f k(1, 2, 3)
-33
```


---


class: middle






# Thank you for attention






### References


  * [MacroTools.jl](https://github.com/FluxML/MacroTools.jl)
  * [操作表达式的必备工具：MacroTools.jl](https://zhuanlan.zhihu.com/p/41062491)

