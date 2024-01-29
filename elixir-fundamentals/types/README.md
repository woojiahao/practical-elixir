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

# 🐝 Types

As mentioned in [why-functional-programming.md](../../why-functional-programming.md "mention"), one of functional programming's draws is the type system. This is no exception in Elixir.&#x20;

Elixir is a dynamically typed language. This means that rather than declaring the type of variables during initialization like in languages like Java (`int x = 0;`), you simply initialize the variable with the appropriate value `x = 0` and the compiler is able to recognize the appropriate type for the variable.

Elixir relies on tools like the [dialyzer](https://github.com/jeremyjh/dialyxir) to provide type checking using type annotations.

Unlike other dynamic languages like Python, Elixir's use of [referential transparency](../../why-functional-programming.md#referential-transparency) means that the value of a variable cannot be changed once it has been initialized so all references to that variable before any re-assignments will not be affected by a re-assignment.

## Reading function signatures

There will be points in the guide where I refer to a function via the following notation: `function_name/arity`. For instance, `or/2`.

The arity of a function refers to the number of arguments that the function receives. For more information about the function, you can use `h function_name/arity` in IEx to get the help documentation.
