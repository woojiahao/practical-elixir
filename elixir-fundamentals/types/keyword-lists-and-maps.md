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

# 🦉 Keyword lists and maps

Keyword lists and maps are known as "associative data structures" in Elixir. Associative data structures are able to associate a key to a certain value. This is often known as hash maps or dictionaries in other languages. However, there are some minor differences between Elixir's associative data structures and the traditional hash map/dictionary.

## Keyword lists

Keyword lists are a common data structure used to pass options to functions. Consider a function call with parameters to configure how the function works such as `String.split/3`:

```elixir
iex(11)> String.split("hello world  another ", " ", trim: true)
["hello", "world", "another"]
```

We are providing the additional options via a keyword list. A keyword list toward the end of a function call can omit the use of the square brackets.

Keyword lists are really just lists with 2-item tuples where the first element is the key (an [atom](keyword-lists-and-maps.md#atoms)) and the second is any value that corresponds to that key.

You can create keyword lists in two ways:

```elixir
iex(12)> x = [a: 0, a: 1, b: 2]
[a: 0, a: 1, b: 2]
iex(15)> [{:a, 0}, {:a, 1}, {:b, 2}]
[a: 0, a: 1, b: 2]
```

Note that keyword lists allow duplicate keys (unlike hash maps).

### Accessing elements

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

### Comparing keyword lists

Another key property of keyword lists is that the keys must be ordered (as defined by the developer, YOU!). This means that `[a: 1, b: 2] != [b: 2, a: 1]`. Thus, while keyword lists allow you to specify a select number of keywords for the function, it is impractical to set "expectations" on the form that these keywords lists may take.

The [`Keyword` module](https://hexdocs.pm/elixir/1.13/Keyword.html) contains more helper functions to manipulate and interact with keyword lists.

{% hint style="info" %}
**Key properties of keyword lists:**

\
1\. Keyword lists must use atoms as keys\
2\. Keyword lists are order dependent\
3\. Keyword lists support duplicate keys
{% endhint %}

Given the order dependence of keyword lists, it is not recommended to use [pattern-matching.md](../pattern-matching.md "mention") with it. More on this will be covered below.

## Maps

Maps are the closer equivalent to hash maps and dictionaries in other languages. Maps essentially store key-value pairs and are defined using `%{}`:

```elixir
iex(18)> m = %{:a => 1, "hello" => "world", 2 => :c}
%{2 => :c, :a => 1, "hello" => "world"}
```

From the above example, you can see that maps are not order dependent nor do they require atoms as keys (in comparison to keyword lists).

### Accessing elements

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

Maps are the perfect structure to use with [pattern-matching.md](../pattern-matching.md "mention") as the fields are not order dependent. More on this will be covered below.
