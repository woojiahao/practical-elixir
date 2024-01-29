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

# 🦀 Pattern matching

The `=` operator is known as the match operator in Elixir. This is because the implicit behavior of `=` is to perform a match between the left hand side and right hand side of the equation. This is on top of the existing assignment behavior. As a result, the following code can be interpreted as:

```elixir
iex(1)> x = 1 # This is assigning the value 1 to variable x
1
iex(2)> 1 = x # This is matching the value of variable x to 1 (checks if LHS == RHS)
1
iex(3)> 2 = x # This fails because variable x holds value of 1, so the match fails
** (MatchError) no match of right hand side value: 1
    (stdlib 5.2) erl_eval.erl:498: :erl_eval.expr/6
    iex:3: (file)
```

As a result, `=` can be used to de-structure a variable into its "parts" as it will map all matched LHS values to the found values on the RHS. This is also known as **pattern matching**.

## Tuples

For instance, you can de-structure the contents of a tuple:

```elixir
iex(3)> x = {:a, "hello", 1}
{:a, "hello", 1}
iex(4)> {atom_var, str_var, num_var} = x
{:a, "hello", 1}
iex(5)> atom_var
:a
iex(6)> str_var
"hello"
iex(7)> num_var
1
```

Notice how each value of the tuple `x` is directly matched to a variable on the LHS. Additionally, if you attempt to de-structure/pattern match a variable with mismatched "parts", you will receive an error:

```elixir
iex(8)> {atom_var, str_var, num_var, failed_var} = x
** (MatchError) no match of right hand side value: {:a, "hello", 1}
    (stdlib 5.2) erl_eval.erl:498: :erl_eval.expr/6
    iex:8: (file)
```

## Lists

You can also perform pattern matching on lists. However, since lists are of variable length (compared to tuples), you may not know the exact number of elements in a list when pattern matching, so you can use the `|` operator instead:

```elixir
iex(8)> l = [1, 2, 3, 4, 5, 6]
[1, 2, 3, 4, 5, 6]
iex(9)> [h | t] = l
[1, 2, 3, 4, 5, 6]
iex(10)> h
1
iex(11)> t
[2, 3, 4, 5, 6]
```

The `|` operator works like the `hd/1` and `tl/1` function in one.

You can optionally "ignore" a variable in pattern matching by replacing it with `_` so the part will still be matched but not assigned to any variable or compared against anything:

```elixir
iex(12)> [a | _] = l
[1, 2, 3, 4, 5, 6]
iex(13)> a
1
```

## Maps

As mentioned in [keyword-lists-and-maps.md](types/keyword-lists-and-maps.md "mention"), while keyword lists support pattern matching, their order dependent nature makes it hard to use with pattern matching. However, maps are the perfect candidate for pattern matching against "dynamic" structures:

```elixir
iex(15)> %{:a => a, "hello" => world_var, 2 => _} = d
%{2 => :b, :a => 1, "hello" => "world"}
iex(16)> a
1
iex(17)> world_var
"world"
```

## Deeply nested structures

Pattern matching also applies to any nested structures as long as you know its "parts" ahead of time:

```elixir
iex(18)> n = [:a, %{"hello" => "world", :b => %{"nested" => "value"}}]
[:a, %{:b => %{"nested" => "value"}, "hello" => "world"}]
iex(19)> [_, %{:b => %{"nested" => nested_var}}] = n
[:a, %{:b => %{"nested" => "value"}, "hello" => "world"}]
iex(20)> nested_var
"value"
```
