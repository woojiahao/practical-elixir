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

# 🦭 Conditionals

When writing functions you may notice that the function behaviors may differ based on the state of the arguments. While [#pattern-matching](functions.md#pattern-matching "mention") and [#guard-clauses](functions.md#guard-clauses "mention") in the function declaration may help to alleviate some of these problems, some derived variables (i.e. variables that are composed of the arguments) cannot be handled through these mechanisms and as such, will require explicit control through conditions.

## case

`case` is used to compare a given value against many patterns until a matching one is found. This is most similar to `switch` statements in other languages:

```elixir
case {1, 2, 3} do
    {1, 2, 5} -> "This will not return"
    {4, 5, 6} -> "Neither will this"
    {1, x, 3} -> "This will work with any #{x}"
    _ -> "This is used as the 'default' case"
end
```

If a pattern contains a reference to a variable outside of the `case`, you need to use the pin operator `^` which "locks" in the variable at the time of use, preventing it from being re-assigned (like in the above example with `x`):

```elixir
x = 5
case {1, 2, 3} do
    {1, 2 ^x} -> "This will also try matching {1, 2, 5} and fail"
    _ -> "This will be the result"
end
```

You can also use [#guard-clauses](functions.md#guard-clauses "mention") with cases, specifying restrictions on each clause:

```elixir
case {1, 2, 3} do
    {1, 2, 5} -> "This will not return"
    {4, 5, 6} -> "Neither will this"
    {1, x, 3} when x < 3 -> "This will work with any #{x} < 3"
    _ -> "This is used as the 'default' case"
end
```

If none of the clauses match the given value, then an error is raised.

## if

`if` is relatively straightforward and is pretty much the same as the other languages:

```elixir
x = 5
if x > 3 do
    "Greater"
else
    "Lesser"
end
```

There is no explicit `elif` so if you have multiple `if` statements, you will have to nest them as such:

```elixir
if x > 3 do
    "Greater"
else
    if x < 0 do
        "Negative"
    else
        "Lesser"
    end
end
```

Like functions, you can also write the `if` statements as one-liners:

```elixir
if x > 3, do: "Greater", else: "Lesser"
```

## cond

Notice that when using `if`, the lack of an explicit `elif` causes your code to adopt an arrowhead style. This is can make your code look messy. This is where `cond` comes in. `cond` is the same as flattening the nested `if` statements:

```elixir
cond do
    x > 3 -> "Greater"
    x < 0 -> "Negative"
    true -> "Lesser"
end
```

The final `true` clause is used as the "default" case. This is because each clause in `cond` is supposed to evaluate to a boolean.

## unless

A final conditional we are introducing is the `unless` conditional which works as the opposite of `if`. The statement given to `unless` must be false for the body to run.

```elixir
unless true do
    "This will not return"
end
```

## Returning conditionals

Everything in Elixir is an expression. This means that even the conditionals are just expressions. This allows the conditionals to be assigned to variables or returned from functions:

```elixir
x = if y > 3, do: "Greater", else: "Lesser"

def foo do
    if true do
        "This is returned"
    else
        "This isn't"
    end
end
```
