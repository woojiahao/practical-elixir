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

# 🐳 Recursion

While Elixir has a `for` keyword for comprehension, it does not work quite the same as traditional for loops. In fact, Elixir does not have any loop constructs. Thus, all iterative looping has to be done recursively (it is possible to do it through `Enum.reduce/3` but that's out of the scope of this guide).

## Tail-call optimization

Traditionally, recursive calls accrue additional stack frames as the computer has to maintain the state of previous function calls. This causes an increase in memory usage as the stack frames are added till the final base case is met.

However, Elixir uses tail-call optimization. Simply put, the computer only needs to use 1 stack frame to handle all recursive calls so long as the return statement of the function is a recursive call. This means that recursion in Elixir does not incur the same memory cost as other languages.

## Using pattern matching and guard clauses

Recursion is where the powers of pattern matching and guard clauses in functions shine. These mechanisms allow us to design recursive solutions in an incredibly concise manner, moving all base cases to pattern matching or guard clauses where possible.

## Common patterns

This guide will not cover the fundamentals of recursive thinking (you can read [this article for more context](https://en.wikipedia.org/wiki/Recursion)). Instead, it tries to demonstrate how common iterative loops are converted to their recursive counterparts. For demonstration purposes, I will be using Python and Elixir:

### Mapping

{% tabs %}
{% tab title="Python" %}
```python
x = [1, 2, 3]
for i in range(len(n)):
    x[i] *= 2 # doubles all elements in array
print(x)
```
{% endtab %}

{% tab title="Elixir" %}
```elixir
def double_map([], res), do: res
def double_map([xi | rest], res), do: double_map(rest, res ++ [xi * 2])
```
{% endtab %}
{% endtabs %}

### Side effect in loop

{% tabs %}
{% tab title="Python" %}
```python
x = [1, 2, 3]
for i in range(len(x)):
    print(x[i])
```
{% endtab %}

{% tab title="Elixir" %}
```elixir
def print_elements([]), do: nil
def print_elements([xi | rest]) do
    IO.puts(xi)
    print_elements(rest)
end
```
{% endtab %}
{% endtabs %}

### Reducing

{% tabs %}
{% tab title="Python" %}
```python
x = [1, 2, 3, 4]
s = 0
for i in range(len(x)):
    s += x[i]
print(s)
```
{% endtab %}

{% tab title="Elixir" %}
```elixir
def sum([], acc), do: acc
def sum([xi | rest], acc), do: sum(rest, acc + xi)
```
{% endtab %}
{% endtabs %}

### Filtering

{% tabs %}
{% tab title="Python" %}
```python
x = [1, 2, 3, 4, 5]
filtered = []
for i in range(len(x)):
    if x[i] & 1 == 1:
        filtered.append(x[i])
```
{% endtab %}

{% tab title="Elixir" %}
```elixir
def filter([], acc), do: acc
def filter([xi | rest], acc) when Integer.is_even(xi), do: filter(rest, acc)
def filter([xi | rest], acc), do: filter(rest, acc ++ [xi])
```
{% endtab %}
{% endtabs %}

## Using predefined methods

For the common patterns above, the `Enum` module provides functions that encompass the behavior we wish to encode. These will be discussed in the upcoming section.
