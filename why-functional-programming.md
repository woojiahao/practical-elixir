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

# 🐸 Why functional programming?

According to [Wikipedia](https://en.wikipedia.org/wiki/Functional\_programming):&#x20;

> ...functional programming is a programming paradigm where programs are constructed by applying and composing functions.
>
> ...functions are treated as first-class citizens...

## Defining functional programming

To some of you, that might seem like a bunch of technical jargon. To explain each component of the definition:

1. Functions: best to think of it as a machine that receives inputs and (sometimes) produces outputs
2. Programming paradigm: a way of writing code that also influences how you think/approach problems programmatically
3. Applying and composing functions: functional programming uses functions as basic building blocks and you can build programs from these fundamental units of work
4. First-class citizens: the core idea of functional programming is to treat functions as the fundamental building blocks so you can pass functions around to other functions (more on this later on)

Essentially, you can think of functional programming as a an alternative way of thinking when approaching problems that uses functions as the fundamental building block of systems (rather than objects in object-oriented programming)!

## Tackling misconceptions

Functional programming can seem intimidating at first because of the misconception that to work with functional programming, you will have to understand concepts like [monads](https://en.wikipedia.org/wiki/Monad\_\(functional\_programming\)) and [functors](https://en.wikipedia.org/wiki/Functor\_\(functional\_programming\)).&#x20;

While you will eventually be exposed to such concepts, you do not need to worry about them starting out. If you have ever used the `map` function in your favorite programming language, you have applied (some form of) functional programming!

Elixir (in my opinion) provides an incredibly gentle introduction to functional programming while providing the necessary tools and knowledge to appreciate and understand other (traditionally more intimidating) functional programming languages.

## What makes functional programming amazing?

While there are many concepts that branch from functional programming, functional programming finds its roots in some of these key principles:

### First-class functions

This concept allow you to pass functions around as though they are normal arguments.&#x20;

This allows you to compose very complex functions and enable concepts like [partial application](https://en.wikipedia.org/wiki/Partial\_application) and [currying](https://en.wikipedia.org/wiki/Currying) (both of which are not concepts you will interact with if you're working on web development).

### Pure functions

Functional programming emphasizes the use of pure functions, which are functions that are free of "side effects" (i.e. no unintended changes happen to variables/state/files outside of the scope of the function).&#x20;

This allows you to think of such functions as black boxes that receive some input while producing some output in a deterministic manner. For instance, a function $$f(x) = x * 2 + 1$$ is always going to produce the value $$5$$ when an input of $$x = 2$$ is given. By creating pure functions, you are ensured that when $$f(x)$$ is executed, the state of the program will always remain the same (apart from the output of the function).

The key reason why pure functions are amazing is the guarantee that the state of the program changes in a deterministic and predictable manner. This helps to reduce the "what-ifs" when calling functions.

### Recursion

Rather than relying on iterative `for` or `while` loops, functional programming encourages the use of recursion to perform iteration. This is further enhanced by the use of [tail recursion](https://en.wikipedia.org/wiki/Tail\_recursion) where recursive calls at the end of functions do not take up additional space on the stack.

Although that might seem like a mouthful of mumbo jumbo, the key takeaway about this is that recursion in most functional programming languages have an equivalent performance for using recursion to its iterative counterparts, hence having no performance penalty!

Another bonus of using recursion is that it trains your ability to look at problems from a different perspective as it breaks your preconceived notions of iteration (which is an amazing thing!)

### Referential transparency

Often referred to as "immutability", functional programming emphasizes the use of immutability. This means that the value of variables cannot be changed after initialization. When a variable is initialized again, it is effectively a different copy of the variable being initialized.

For instance:

```
def fn(x) do
    x = 10
end

x = 6
fn(x)
```

The above code snippet does not re-assign the original `x` to be `10` when `fn` is called. Instead, `x` is treated as an entirely new variable when it is re-assigned.

Referential transparency is very useful when dealing with concurrent code (this is why Elixir was built to follow the functional programming paradigm actually!) and when you want to guarantee function purity.

### Type system

A big component of functional programming languages is their type system which essentially defines a set of rules that govern how/what a specific "type" can contain. A very practical example of why types are useful is the use of type aliases.

For instance, let's say you are building a shape module and you are dealing with multiple shapes that have a `float width` field. Rather than always declaring this `width` field as a `float` (which can be error prone in the event where you choose to change `width` to be an `int` or `double`), you can declare a type alias to the float type, such as `@type width :: float` so next time, you can use `width w` and if you change the type `width` to use an `int` instead, you only need to do it once where the type alias was declared!

There are many other benefits to a powerful type system but those will not be covered in this guide.
