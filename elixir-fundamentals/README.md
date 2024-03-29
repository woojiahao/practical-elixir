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

To try out Elixir, you can use the interactive shell (IEx). This should be automatically installed when you have [installed Elixir](../prerequisites.md). Go to your terminal and type

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

For this guide, you can assume that all code snippets are run in IEx unless otherwise specified.
