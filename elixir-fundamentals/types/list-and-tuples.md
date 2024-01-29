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

# 🪱 List and tuples

## Lists

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

### Common list operations

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

## Tuples

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

## Lists or tuples?

Both lists and tuples are immutable data structures so any operations on an existing list/tuple will result in a new list/tuple being created.

However, lists are represented as linked lists. Thus, any operation that will require searching through a linked list will always take $$O(n)$$ time where $$n$$ is the length of the list.&#x20;

Tuples, on the other hand, are much closer to arrays in representation: being stored contiguously in memory. Thus, most tuple operations are quick, such as getting by index. However, operations that require updating the tuple can be more costly as a new contiguous memory must be located to be assigned.

{% hint style="info" %}
Elixir has some compiler-level optimizations to reduce the overall memory usage by tuples. For instance, when modifying a tuple, the existing elements are shared between the old and new tuple.
{% endhint %}

Tuples are more commonly used as return values with fixed sizes, such as `{:ok, value}` and `{:error, error}`.

## Comparing lists and tuples

The equality operators, `==` and `!=` both work on lists and tuples as well.
