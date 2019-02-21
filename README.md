# LightStructArrays

[![Build Status](https://travis-ci.org/KristofferC/LightStructArrays.jl.svg?branch=master)](https://travis-ci.org/piever/LightStructArrays.jl)
[![codecov.io](http://codecov.io/github/KristofferC/LightStructArrays.jl/coverage.svg?branch=master)](http://codecov.io/github/piever/LightStructArrays.jl?branch=master)

This package introduces the type `StructArray` which is an `AbstractArray` whose elements are `struct` (for example `NamedTuples`,  or `ComplexF64`, or a custom user defined `struct`). While a `StructArray` iterates `structs`, the layout is column based (meaning each field of the `struct` is stored in a seprate `Array`).

`Base.getproperty` or the dot syntax can be used to access columns, whereas rows can be accessed with `getindex`.

The package is largely inspired from the `Columns` type in [IndexedTables](https://github.com/JuliaComputing/IndexedTables.jl)

## Example usage to store complex numbers

```julia
julia> using LightStructArrays, Random

julia> Random.seed!(4);

julia> s = StructArray{ComplexF64}((rand(2,2), rand(2,2)))
2×2 StructArray{Complex{Float64},2,NamedTuple{(:re, :im),Tuple{Array{Float64,2},Array{Float64,2}}}}:
 0.680079+0.625239im   0.92407+0.267358im
 0.874437+0.737254im  0.929336+0.804478im

julia> s[1, 1]
0.680079235935741 + 0.6252391193298537im

julia> s.re
2×2 Array{Float64,2}:
 0.680079  0.92407
 0.874437  0.929336
```

Note that the same approach can be used directly from an `Array` of complex numbers:

```julia
julia> StructArray([1+im, 3-2im])
2-element StructArray{Complex{Int64},1,NamedTuple{(:re, :im),Tuple{Array{Int64,1},Array{Int64,1}}}}:
 1 + 1im
 3 - 2im
```

## Example usage to store a data table

```julia
julia> t = StructArray((a = [1, 2], b = ["x", "y"]))
2-element StructArray{NamedTuple{(:a, :b),Tuple{Int64,String}},1,NamedTuple{(:a, :b),Tuple{Array{Int64,1},Array{String,1}}}}:
 (a = 1, b = "x")
 (a = 2, b = "y")

julia> t[1]
(a = 1, b = "x")

julia> t.a
2-element Array{Int64,1}:
 1
 2

julia> push!(t, (a = 3, b = "z"))
3-element StructArray{NamedTuple{(:a, :b),Tuple{Int64,String}},1,NamedTuple{(:a, :b),Tuple{Array{Int64,1},Array{String,1}}}}:
 (a = 1, b = "x")
 (a = 2, b = "y")
 (a = 3, b = "z")
```
