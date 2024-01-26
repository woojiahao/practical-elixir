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

# 🐙 Elixir fundamentals

This guide aims to provide you with the fundamentals to start using Elixir with Phoenix for web development. It is by no means an exhaustive guide on the fundamentals of Elixir. For more in-depth information about Elixir, refer to the [official documentation.](https://hexdocs.pm/elixir/introduction.html)

{% hint style="info" %}
This guide assumes that you have fundamental programming knowledge of concepts like integers/floats/booleans, conditionals, and loops.
{% endhint %}

## Getting started

To try out Elixir, you can use the interactive shell (IEx). This should be automatically installed when you have [installed Elixir](prerequisites.md). Go to your terminal and type

```
iex
```

You should see a prompt like this:

```
λ ~/ iex
Erlang/OTP 26 [erts-14.2.1] [source] [64-bit] [smp:8:8] [ds:8:8:10] [async-threads:1] [jit] [dtrace]

Interactive Elixir (1.16.0) - press Ctrl+C to exit (type h() ENTER for help)
iex(1)> 
```

Then, you can try running each commands in this environment.&#x20;

Alternatively, you can also create a `.exs` file and run it via `elixir <filename>.exs`.

## Types

As mentioned in [why-functional-programming.md](why-functional-programming.md "mention"), one of functional programming's draws is the type system. This is no exception in Elixir.&#x20;

Elixir is a dynamically typed language. This means that rather than declaring the type of variables during initialization like in languages like Java (`int x = 0;`), you simply initialize the variable with the appropriate value `x = 0` and the compiler is able to recognize the appropriate type for the variable.

Elixir relies on tools like the [dialyzer](https://github.com/jeremyjh/dialyxir) to provide type checking using type annotations.

Unlike other dynamic languages like Python, Elixir's use of [referential transparency](why-functional-programming.md#referential-transparency) means that the value of a variable cannot be changed once it has been initialized so all references to that variable before any re-assignments will not be affected by a re-assignment.

### Reading types

There will be points in the guide where I refer to a function via the following notation: `function_name/arity`. For instance, `or/2`.

The arity of a function refers to the number of arguments that the function receives. For more information about the function, you can use `h function_name/arity` in IEx to get the help documentation.

### Basic types

Elixir supports the basic types like:

1. Integer
2. Float
3. Boolean
4. Atom
5. String

```elixir
x = 1        # integer
x = 0.5      # float
x = true     # boolean
x = :atom    # atom
x = "elixir" # string
```

#### Arithmetic

To perform arithmetic on numbers, you can use the following operators:

1. `+`: addition
2. `*`: multiplication
3. `/`: floating point division
4. `-`: subtraction
5. `div(dividend, divisor)`: integer division, truncates the floating point (if any)
6. `rem(dividend, divisor)`: remainder of division (similar to `%` in other languages)

```elixir
iex(1)> 1 + 1
2
iex(2)> 2 * 3
6
iex(3)> 3 / 2
1.5
iex(4)> 3 - 2
1
iex(5)> div(3, 2)
1
iex(6)> rem(3, 2)
1
```

There are also some notable utility functions like:

1. `round(number)`: rounds a number to the closest integer
2. `trunc(number)`: retrieves only the integer part of a float
3. `is_integer(value)`: returns if given `value` is an integer (floats do not count as integers)
4. `is_float(value)`
5. `is_number(value)`

```elixir
iex(7)> round(5.7)
6
iex(8)> round(5.3)
5
iex(9)> trunc(5.5)
5
iex(10)> is_integer(5)
true
iex(11)> is_integer(5.5)
false
```

There are also other modules from Erlang like [`:math`](https://www.google.com/search?client=firefox-b-d\&q=%3Amath+erlang) that provide additional utility functions such as `pow(x, y)` and `sqrt(x)`.

#### Booleans

Elixir supports `true` and `false` as boolean types (much like many other languages):

```elixir
iex(13)> true
true
iex(14)> false
false
iex(15)> false == true
false
iex(16)> false == false
true
```

Boolean operators are also supported such as `or/2`, `and/2`, and `not/1`:

```elixir
iex(17)> true and true
true
iex(18)> true and false
false
iex(19)> false or true
true
iex(20)> not true
false
```

These operators are [short circuit operators](https://en.wikipedia.org/wiki/Short-circuit\_evaluation). This means that the right hand side is executed iff the left hand side is insufficient to determine the result:

```elixir
iex(21)> false and raise("This error will never be raised")
false
iex(22)> true or raise("This error willl never be raised")
true
```

#### nil values

`nil` in Elixir represents the absence of a value (similar to `null` in other languages). `nil` and `false` are both considered "falsy" values (i.e. evaluates to `false`) and all other values are considered "truthy".

To operate with booleans and `nil` values, additional operators are supported: `||/2`, `&&/2`, `!/1` that correspond to and, or, and not.

```elixir
iex(23)> !nil
true
```

#### Atoms

An atom is a constant whose value is its own name. They are globally unique so `:apple == :apple`. The most common use case for atoms is to signal the return value of a function such as `:ok` or `:error`.

```elixir
iex(24)> x = :apple
:apple
iex(25)> x == :apple
true
```

#### Strings

Much like other languages like Java, strings are denoted using double quotes.

```elixir
iex(26)> str = "Hello world!"
"Hello world!"
```

String concatenation is achieved using `<>/2`:

```elixir
iex(27)> str <> " said the computer"
"Hello world! said the computer"
```

String interpolation (i.e. string templates/string formatting) is achieved by adding `#{}` into the string with the variable/expressions going within the curly braces:

```elixir
iex(31)> "The computer said '#{str}'"
"The computer said 'Hello world!'"
```

{% hint style="info" %}
All values are converted to a string in string interpolation
{% endhint %}

You can print to the console using `IO.puts/1` like `System.out.println()`.

To retrieve the length of a string, use the `String.length/1` function. The [`String` module](https://hexdocs.pm/elixir/1.12/String.html) contains many more string manipulation functions.

#### Structural comparison

You can compare between two values using `==`, `!=`, `<=`, `>=`, `<`, and `>` operators.

```elixir
iex(32)> 1 == 1
true
iex(33)> "a" == "a"
true
iex(34)> 1 != 2
true
iex(35)> 1 < 2
true
iex(36)> 1 == 1.0
true
```

Notice that although we were comparing an integer to a float, the result is still true. This can be mitigated by using the strict comparison operators, `===` and `!==`:

```elixir
iex(37)> 1 === 1.0
false
```

### Lists and tuples

### Keyword lists and maps

## Functions

## Conditionals

## Recursion

## Pattern matching

## Enumerables and streams

## Modules

## Linking with other modules&#x20;
