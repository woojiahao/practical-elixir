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

Lists in Elixir are implemented as linked lists and are denoted using square brackets:

```elixir
iex(1)> x = [1, 2, 3, 4]
[1, 2, 3, 4]
```

Since Elixir is dynamically typed, you can create lists of different types that will, by default, be treated as the an `[any()]` type.

```elixir
iex(2)> x = ["hi", 1, true, :atom]
["hi", 1, true, :atom]
```

List concatenation is achieved using `++/2` and list subtraction is achieved using `--/2`.

```elixir
iex(3)> [1, 2] ++ [4, 5]
[1, 2, 4, 5]
iex(4)> [1, 2, 3] -- [2, 3]
[1]
```

There are other built-in functions like `hd/1` and `tl/1` that return the head and remainder of the linked list (excluding the head) respectively. You can also use `length/1` to retrieve the length of the list.

```elixir
iex(5)> hd([1, 2, 3])
1
iex(6)> tl([1, 2, 3])
[2, 3]
iex(7)> hd([])
** (ArgumentError) errors were found at the given arguments:

  * 1st argument: not a nonempty list

    :erlang.hd([])
    iex:7: (file)
```

Tuples, on the other than, use curly braces instead:

```elixir
iex(7)> t = {:ok, "hello", 1}
{:ok, "hello", 1}
```

Use the `tuple_size/1` function to retrieve the length of the tuple and `elem/2` to retrieve an element of the tuple given its zero-based index.

```elixir
iex(8)> elem(t, 2)
1
iex(9)> elem(t, 1)
"hello"
iex(10)> tuple_size(t)
3
```

Tuples, unlike arrays, are of fixed size. So the size of the tuple cannot be changed (i.e. cannot add to an existing tuple).

#### Lists or tuples?

Both lists and tuples are immutable data structures so any operations on an existing list/tuple will result in a new list/tuple being created.

However, lists are represented as linked lists. Thus, any operation that will require searching through a linked list will always take $$O(n)$$ time where $$n$$ is the length of the list.&#x20;

Tuples, on the other hand, are much closer to arrays in representation: being stored contiguously in memory. Thus, most tuple operations are quick, such as getting by index. However, operations that require updating the tuple can be more costly as a new contiguous memory must be located to be assigned.

{% hint style="info" %}
Elixir has some compiler-level optimizations to reduce the overall memory usage by tuples. For instance, when modifying a tuple, the existing elements are shared between the old and new tuple.
{% endhint %}

Tuples are more commonly used as return values with fixed sizes, such as `{:ok, value}` and `{:error, error}`.

#### Comparing between lists and tuples

The equality operators, `==` and `!=` both work on lists and tuples as well.

### Keyword lists and maps

Keyword lists and maps are known as "associative data structures" in Elixir. Associative data structures are able to associate a key to a certain value. This is often known as hash maps or dictionaries in other languages. However, there are some minor differences between Elixir's associative data structures and the traditional hash map/dictionary.

#### Keyword lists

Keyword lists are a common data structure used to pass options to functions. Consider a function call with parameters to configure how the function works such as `String.split/3`:

```elixir
iex(11)> String.split("hello world  another ", " ", trim: true)
["hello", "world", "another"]
```

We are providing the additional options via a keyword list. A keyword list toward the end of a function call can omit the use of the square brackets.

Keyword lists are really just lists with 2-item tuples where the first element is the key (an [atom](elixir-fundamentals.md#atoms)) and the second is any value that corresponds to that key.

You can create keyword lists in two ways:

```elixir
iex(12)> x = [a: 0, a: 1, b: 2]
[a: 0, a: 1, b: 2]
iex(15)> [{:a, 0}, {:a, 1}, {:b, 2}]
[a: 0, a: 1, b: 2]
```

Note that keyword lists allow duplicate keys (unlike hash maps).

When accessing a keyword list, square brackets can be used (much like dictionaries in Python):

```elixir
iex(16)> x[:b]
2
```

Note that since keyword lists support duplicate keys, using square brackets on a duplicate key retrieves the first instance:

```elixir
iex(17)> x[:a]
0
```

Another key property of keyword lists is that the keys must be ordered (as defined by the developer, YOU!). This means that `[a: 1, b: 2] != [b: 2, a: 1]`. Thus, while keyword lists allow you to specify a select number of keywords for the function, it is impractical to set "expectations" on the form that these keywords lists may take.

The [`Keyword` module](https://hexdocs.pm/elixir/1.13/Keyword.html) contains more helper functions to manipulate and interact with keyword lists.

{% hint style="info" %}
**Key properties of keyword lists:**

\
1\. Keyword lists must use atoms as keys\
2\. Keyword lists are order dependent\
3\. Keyword lists support duplicate keys
{% endhint %}

#### Maps

Maps are the closer equivalent to hash maps and dictionaries in other languages. Maps essentially store key-value pairs and are defined using `%{}`:

```elixir
iex(18)> m = %{:a => 1, "hello" => "world", 2 => :c}
%{2 => :c, :a => 1, "hello" => "world"}
```

From the above example, you can see that maps are not order dependent nor do they require atoms as keys (in comparison to keyword lists).

To access elements in a map, the square bracket notation can be used (much like keyword lists):

```elixir
iex(19)> m[:a]
1
iex(20)> m[2]
:c
```

Additionally, if a key is an atom, the `.` operator can also be used (similar to how object access works in Javascript):

```elixir
iex(21)> m.a
1
```

There is also a very useful syntax for updating keys in a map via `%{map | key: new_value}`.

Similar to the `Keyword` module, the [`Map` module](https://hexdocs.pm/elixir/1.12.3/Map.html) exists to provide extra utility when working with maps.

## Functions

## Conditionals

## Recursion

## Pattern matching

## Enumerables and streams

## Modules

## Linking with other modules&#x20;
