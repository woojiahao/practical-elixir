---
layout:
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# 🐖 Enumerables

Many of the common recursive patterns are provided as functions from the `Enum` module. These are functions that are often chained with one another and reduces the code duplication necessary in your codebase. These patterns often involve operating on "enumerables" (such as lists, tuples, and maps) and produce some result.

## Enum functions

The [`Enum` module](https://hexdocs.pm/elixir/1.12/Enum.html) in Elixir is rather extensive and provides many utility functions that would otherwise require dedicated recursive functions. We will highlight just a few notable ones that you will use most often:

1. `Enum.all?`: returns `true` iff the entire enumerable is `true` or satisfies a given condition based on a given `fun`
2. `Enum.any?`: returns `true` iff any element in the enumerable is `true` or satisfies a given condition based on a given `fun`
3. `Enum.at`: returns the element at a given `index` with a default value if `index` is out of bounds (`nil` by default)
4. `Enum.filter`: returns the filtered enumerable after applying a given predicate
5. `Enum.map`: returns the mapped enumerable after applying a given transformation function
6. `Enum.flat_map`: returns the mapped enumerable after applying a given transformation function and flattens any first-level nested enumerables
7. `Enum.sort`: returns the sorted enumerable

There are many more functions that the `Enum` module provides. Feel free to read the documentation for more information.

## Function chaining

You may notice that applying `Enum` functions (or any function for that matter) often requires chaining, where you pass the output of one function call as the input to another till the final output is produced.

While you can nest these function calls as such:

```elixir
Enum.map(Enum.filter(1..10, fn x -> Integer.is_odd(x) end), fn x -> x * 2 end)
```

It becomes very messy once you have more than two nested function calls. Instead, you can use the pipe operator (`|>`) to perform function chaining (the equivalent of the above example):

```elixir
1..10
|> Enum.filter(fn x -> Integer.is_odd(x) end)
|> Enum.map(fn x -> x * 2 end)
```

Using the pipe operator helps to tidy up the code and reduce the clutter when performing function chaining.

## Streams

`Enum` functions perform computation eagerly, i.e. the transformation function is called for every element of the enumerable immediately in a `map` call. However, this can be incredibly costly as the enumerable might be an infinite stream or just a very large list with the transformation being extremely costly.

Thus, Elixir supports lazy computation of commonly used `Enum` functions through the `Stream` module. This guide will not cover streams in-depth as it is not used in this guide, but it is good to give the [documentation of `Stream`](https://hexdocs.pm/elixir/Stream.html) a read.
