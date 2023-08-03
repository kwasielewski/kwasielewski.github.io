@def title = "Functional style lists in Julia"
@def author = "Krzysztof Wasielewski"
@def published = "XX August 2023"
@def tags = ["blog"]

### Functional style lists in Julia

In today's post we will look at how to implement immutable, generic yet homogenous lists in Julia.

Let's start with creating a package for our lists.
Simply open Julia REPL, press `]` to enter package manager mode and type:
```julia
generate Lists
```
We'll be working with `src/Lists.jl` file.
A module with single `greet` method is already defined there.
We may delete this function, as we won't need it.
From now on all the code we'll write will be inside this module.

Now, let's move to defining an empty list:
```julia
struct Nil end
```
In Julia `struct` is used to define new data types.
By default, all fields are immutable, so we don't have to specify it explicitly.
Empty list contain no values, so it has no fields.
We can now create an empty list by calling its constructor:
```julia
Nil()
```
It may seem too verbose to use parentheses to create an empty list, but we need a way to differentiate between a type and a value.

In the next step we'll specify what a List is.
As one may know, a list is either an empty list or a node with value and another list.
Because list may take one of two forms, we'll use `Union` type.
The value inside a node may be of any type so we'll use a type parameter.
We would like to have a something like this:
```julia
List{T} = Union{Nil, Node{T}}
```
but we don't now yet what `Node` is.
So let's define it:
```julia
struct Node{T}
    value::T
    next::Union{Nil, Node{T}}
end
```
Now that we have a definition of `Node` we can finally define `List`.

With all the definitions in place we can create a list:
```julia
Node(1, Node(2, Nil()))
```
The output for this expression is:
```julia
Node{Int64}(1, Node{Int64}(2, Nil()))
```

If we try to create a list with values of different types, we'll get an error:
```julia
julia> Node(1, Node(2.0, Nil()))
ERROR: MethodError: no method matching Node(::Int64, ::Node{Float64})
```

