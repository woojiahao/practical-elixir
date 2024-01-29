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

# 🐮 Modules

## What are modules?

Modules are a way for developers to "package" like functions. For simplicity sake, you can think of modules as static classes/singletons that are initialized once when your program starts. There are other nuances behind how modules really work in Elixir but that is the fundamental behavior that you should think about at this stage.

## Your first module

To define a new module, you can use the `defmodule` [macro](https://hexdocs.pm/elixir/macros.html). This guide will not cover what macros are, but you can think of them as defining keywords with some behavior. Then, within the body of the module, you can define functions using the `def` macro. For this demonstration, we will continue using `.exs` (or Elixir scripts):

```elixir
defmodule Math do
    def add(a, b) do
        a + b
    end
    
    def subtract(a, b) do
        a - b
    end
end

IO.puts(Math.add(5, 3))
```

Then, when you run this script, you will see `8` printed.

```
λ ~/Projects/practical-elixir-demo/ main* elixir modules.exs 
8
```

## Compiling files with modules

If you are dealing with `.ex` files (as you normally would in larger projects), you can expect to compile these modules via the `elixirc` command. Then, running the `iex` command again with the resulting bytecode in the same folder will cause the `Math` module to be loaded.

```
λ ~/Projects/practical-elixir-demo/ main* elixirc math.ex
λ ~/Projects/practical-elixir-demo/ main* iex
iex(1)> Math.subtract(5, 3)
2
```

Normally, you should not have to manually compile Elixir files (mainly because it becomes unwieldy). Elixir comes with a built-in build tool called [mix.md](mix.md "mention") that we will discuss later on.
