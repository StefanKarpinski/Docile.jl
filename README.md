# Docile

[![Build Status](https://travis-ci.org/MichaelHatherly/Docile.jl.svg?branch=master)](https://travis-ci.org/MichaelHatherly/Docile.jl)
[![Build status](https://ci.appveyor.com/api/projects/status/ttlbaxp6pgknfru5/branch/master)](https://ci.appveyor.com/project/MichaelHatherly/docile-jl/branch/master)
[![Coverage Status](https://coveralls.io/repos/MichaelHatherly/Docile.jl/badge.png)](https://coveralls.io/r/MichaelHatherly/Docile.jl)

*Docile* is a [Julia](www.julialang.org) package documentation system
that provides a docstring macro, `@doc`, for documenting arbitrary
Julia objects and associating metadata with them.

## Install

*Docile* is in `METADATA` and can be installed via `Pkg.add("Docile")`.

## Usage

### Viewing

```julia
using Docile
query("Examples")      # full text search of all documentation
query(Docile)          # the docs associated with the module itself
query(query)           # docs for generic function `query` and all it's methods
@query doctest(Docile) # behaves similarly to `Base.@which`, but returns documentation
@query @query          # searching for macros
```

### Documenting

*Docile* can document:

* functions
* globals
* macros
* methods
* modules
* types

The syntax of `@doc` is the same for all cases.

**Example:**

```julia
module PackageName

using Docile
@docstrings # Call before any `@doc` uses. Creates module's `__METADATA__` object.

@doc """
Markdown formatted text appears here...

""" {
    # metadata section
    :section => "Main section",
    :tags    => ["foo", "bar", "baz"]
    # ... other (Symbol => Any) pairs
    } ->
function myfunc(x, y)
    # ...
end

@doc "A short docstring." ->
foo(x) = x

end

```

A `->` is required between the docstring/metadata and the object being
documented. It **must** appear on the same line as the
docstring/metadata.

If no metadata is required for an object then the metadata section can
be left out.

External files containing documentation can be linked to by adding a
`:file => "path"` to the metadata section of the `@doc` macro. The text
section of the macro, `""" ... """`, is ignored in this case and can be
left out. The file path is taken to be relative to the source file. This
README file is linked into the module documentation using:

```julia
@doc { :file => "../README.md" } -> Docile

```

The `@doc` macro requires at least a docstring or metadata section. The
docstring section always appears first if both are provided. Bare
`@doc`s are not permitted:

```julia
@doc -> illegal(x) = x

```

A `tex_mstr` string macro is provided to avoid having to escape LaTeX
syntax in docstrings. Using standard multiline strings allows for
interpolating data into the string from the surrounding module in the
usual way.

Code generated via loops and `@eval` can also be documented. See the
file `test/loop-generated-docs.jl` for an example of this.

### Doctests

`doctest(PACKAGE_NAME)` can be used to run all the docstring code blocks
in a module documented by *Docile*.

Only fenced or indented code blocks are run through `doctest`. Inline
code with single backticks isn't run. Placing an extra blank line at the
end of a code block tells `doctest` to skip the block.

## Feedback

Any thoughts, requests, or PRs are welcome.

## Issues list

Some issues and discussion related to package documentation:

* [#762](https://github.com/JuliaLang/julia/issues/762)
* [#1619](https://github.com/JuliaLang/julia/pull/1619)
* [#3407](https://github.com/JuliaLang/julia/issues/3407)
* [#3988](https://github.com/JuliaLang/julia/issues/3988)
* [#4579](https://github.com/JuliaLang/julia/issues/4579)
* [#5200](https://github.com/JuliaLang/julia/issues/5200)
* and elsewhere on the mailing lists.
