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

# 🦎 Functions

Functions are the core building blocks of Elixir. If you had tried the example above about modules, you would have tried creating your very first function. Unlike functions in Python and Javascript, functions cannot be declared at the top-level, outside of a module. If a function does not have a module, then it will be impossible to reference.

Functions are created using the `def` macro, where the format is as follows: `def <function_name>(<function_parameters>) do ... end`.&#x20;

## Return values

The return value of a function is automatically set as the final statement in the function body (i.e. no explicit `return` is necessary):

```elixir
def format_person(name, age, school) do
    "#{name} is #{age} years old and attends #{school}"
end
```

## Impure functions

Unlike stricter functional programming languages, Elixir does allow side effects (like printing or database editing) in a function:

```elixir
def format_person(name, age, school) do
    IO.puts("Hello world!!")
    "#{name} is #{age} years old and attends #{school}"
end
```

However, it is recommended to keep side effects to a minimum.

## Quality of life

Elixir also provides minor quality of life features when writing functions. Some of which include:

1. Omitting the parentheses when there are no parameters
2. Removing the use of `end` when there is only the return statement

```elixir
def foo, do: 5
```

## Method overloading

Similar to other languages, Elixir also supports method overloading (i.e. declaring the same method name with differing parameters). For instance:

```elixir
def foo(a, b), do: a + b
def foo(a), do: a
def foo, do: nil
```

## Default arguments

Elixir functions also supports default arguments:

```elixir
def minus(a, b \\ 0) do
    a - b
end
```

The function above receives an optional argument `b` that defaults to `0` if left unspecified. If `b` is omitted, `minus` just returns the original argument. Otherwise, it performs the subtraction operation.

## Pattern matching

Function parameters can also be pattern matched, allowing you to create really concise function definitions without the need for any other constructs. For instance, look at the implementation of the Fibonacci sequence using pattern matching:

$$
f(n) = \begin{cases}
1, n = 0\\
1, n = 1\\
f(n-1) + f(n - 2)
\end{cases}
$$

```elixir
def fib(0), do: 1
def fib(1), do: 1
def fib(n), do: fib(n - 1) + fib(n - 2)
```

Pattern matching in functions makes it so we can easily express such recurrence relations with minimal effort and in a way that makes sense semantically.

You can combine the pattern matching shown in previous examples as well! For instance, you can pattern match the parts of a map and use them within the function without any further access methods:

```elixir
def foo([_, %{:b => %{"nested" => nested_var}}]) do
    "Nested var was " <> nested_var 
end
```

If there are multiple function declarations with different pattern matching parameters, Elixir will try each of them until it finds a match. If no match is found, then an exception is raised. You can create a "base function" to handle such cases. These functions tend to be the last in the function list:

```elixir
def fib(0), do: 1
def fib(1), do: 1
def fib(n): do: fib(n - 1) + fib(n - 2)
def fib(_), do: nil # NOTE: this won't ever be called, this is for illustrations
```

{% hint style="info" %}
Try arranging your pattern matching functions in decreasing order of strictness (i.e. the most specific cases should be declared first)
{% endhint %}

## Guard clauses

Another way to validate the arguments of a function before the function body is to use guard clauses which are added after the parameter list, following the `when` keyword. This is useful if you wish to combine various pattern matching clauses, you have to check the types of the arguments, or when you need to validate the arguments against one another:

```elixir
def fib(n) when n < 0, do: nil
def fib(n) when n == 0 or n == 1, do: 1
def fib(n), do: fib(n - 1) + fib(n - 2)

def abs_minus(a, b) when a < b, do: b - a
def abs_minus(a, b), do: a - b
```

There are certain limitations to using guard clauses, one of which includes not being able to use custom functions in the guard clause (this is due to the nature of how guard clauses and functions are compiled). You can refer to this [website for more information](https://kapeli.com/cheat\_sheets/Elixir\_Guards.docset/Contents/Resources/Documents/index) about guard clauses.

## Anonymous functions

Anonymous functions are functions declared without using the `def` macro. They allow you to pass functions around as parameters or return values (you can do that even with regular functions too!).

You can declare anonymous functions with `fn <parameter list> -> <function body> end`:

```elixir
pow_two = fn x -> x * x end
pow_two.(4) # returns 16
```

You can also make the anonymous function multi-line by adding a newline after `->` (unlike Python's lambdas).&#x20;

Notice that you call the anonymous function using `.()` rather than just `()`, this helps make it clear that you are calling an anonymous function as you may have overriden an existing function.

## Closures

Anonymous functions have access to the variables that are in scope when the function is defined. This is also known as a closure.

```elixir
def foo do
    x = 42
    bar = fn -> x * 2 end
    bar.() # returns 84
end
```

## Currying

Closures are particularly useful when you are trying to curry functions. Currying is a functional programming technique that takes functions that accept multiple parameters and transforms them into functions that take only one parameter each.&#x20;

Closures allow each of the nested functions to reference the variables from outside of its current scope (i.e. the innermost function can reference `a` and `b`):

```elixir
def foo(a, b, c) do
    a * b + c
end

def curry_foo(a) do
    fn b ->
        fn c ->
            a * b + c
    end
end

foo(1, 2, 3) # returns 5
curry_foo(1).(2).(3) # returns 5
```

Currying is useful for creating partial applications of functions. For instance, let's say I would like to apply the last operation (`* c`) with different values but preserve the values of `a = 1` and `b = 2` from the initial application, I can do so by applying the curried function twice (not three times) and then saving the partially applied function as a variable:

```elixir
partial = curry_foo(1).(2)
partial.(3) # returns 5
partial.(6) # returns 8 instead
partial.(10) # returns 12
```

So, rather than having to type `foo(1, 2, 3)` and then `foo(1, 2, 6)` and then `foo(1, 2, 10)`, I only have to call `partial.(x)` with the different variables.
