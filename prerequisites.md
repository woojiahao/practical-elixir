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

# 🐷 Prerequisites

## Technical prerequisites

To follow along with this guide, please ensure that you have the following setup on your machine.

1. Install Elixir locally, for more instructions, refer to [this guide](https://elixir-lang.org/install.html)
2. Install SQLite locally (it should be preinstalled for MacOS and Linux users), for Windows users, refer to [this guide](https://www.tutorialspoint.com/sqlite/sqlite\_installation.htm)&#x20;
3. Clone the demo repository for the code snippets

```
git clone https://github.com/woojiahao/practical_elixir_demo
```

```
cd practical_elixir_demo/
```

```
mix setup
```

### Using the demo repository

The demo repository contains four key branches:

1. `main`: contains the code for base Phoenix
2. `liveview-base`: contains the code for migrating from base Phoenix to Phoenix LiveView without any further additions
3. `liveview`: contains the completed code for Phoenix LiveView (including creating to-dos and marking to-dos as done/not done)
4. `complete`: contains the fully completed code with Phoenix LiveView and data persistence with SQLite3

To view the code on each branch, use the following commands:

```
git fetch
```

```
git switch main/liveview-base/liveview/complete
```

## Slides

You can find the slides for this workshop [here](https://github.com/woojiahao/talks/blob/main/20240215-practical-functional-programming/slides.pdf).

## Other prerequisites

Aside from these technical requirements, this guide assumes that you have some programming fundamentals (i.e. loops, basic understanding of recursion, variables, statements, etc.). More on these concepts will be introduced but it will be useful to have some fundamentals.
