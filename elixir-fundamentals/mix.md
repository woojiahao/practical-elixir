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

# 🦁 Mix

Mix is Elixir's built-in build tool. Build tools are useful for managing the structure of a project, handling the build process, downloading external packages, and even create custom scripts to handle parts of your project for you. Other build tools include `npm` in Javascript and Gradle in Java.

## Getting started

Mix is a relatively opinionated build tool. This means that Mix provides a project structure for you. This is particularly useful for people starting out so they do not need to think too much about how to structure their projects (compared to languages like Go that have a package management tool but no fixed project structure).

To create a project with Mix, you will use the `mix new` command (should already be installed when you [installed Elixir](../prerequisites.md)):

```
mix new kv --module KV
```

Mix will create a new folder `kv/` with the following structure:

```
kv/
├── .formatter.exs
├── .gitignore
├── README.md
├── lib
│   └── kv.ex
├── mix.exs
└── test
    ├── kv_test.exs
    └── test_helper.exs
```

## Project structure

The default project structure includes basic files like `README.md` and `.gitignore` and additional Elixir specific files and folders.

### lib/

The `lib/` folder contains the code that you will write. Notice that when you created the project, you specified the module name as `KV`. This should be the root module of your project.

The initial file `lib/kv.ex` will be the root file that contains the `KV` module and any related functions under this module. If you are creating any sub-modules, you will create a folder `lib/kv/` and add the folders/files there.

```
lib/
├── kv
│   └── utils
│       └── utils.ex
└── kv.ex
```

Where `utils.ex` looks like this:

```elixir
defmodule KV.Utils do
  def foo do
    :foo
  end
end
```

### test/

Mix uses [ExUnit](https://hexdocs.pm/ex\_unit/ExUnit.html) (Elixir's testing framework) to perform unit testing. These unit tests are stored under the `test/` folder and the folder follows the same structure as the `lib/` folder. Test files are appended with `_test` and run as `.exs` files instead of `.ex`.&#x20;

You can run the tests via

```
mix test
```

This guide will not go into the full details of unit testing in Elixir.

### mix.exs

This is the equivalent of `package.json` or `build.gradle` where it contains the specific instructions and packages to include for the project.

```elixir
defmodule KV.MixProject do
  use Mix.Project

  def project do
    [
      app: :kv,
      version: "0.1.0",
      elixir: "~> 1.16",
      start_permanent: Mix.env() == :prod,
      deps: deps()
    ]
  end

  # Run "mix help compile.app" to learn about applications.
  def application do
    [
      extra_applications: [:logger]
    ]
  end

  # Run "mix help deps" to learn about dependencies.
  defp deps do
    [
      # {:dep_from_hexpm, "~> 0.3.0"},
      # {:dep_from_git, git: "https://github.com/elixir-lang/my_dep.git", tag: "0.1.0"}
    ]
  end
end
```

For now, the key function you should be concerned about is `deps` which specifies the dependencies of the project. The packages of the project are specified as a list of tuple.

## Compiling the project

Previously in [#compiling-files-with-modules](modules.md#compiling-files-with-modules "mention"), you will notice that it is quite tedious to compile each file yourself and it can be quite messy with the `.beam` files scattered across the folder.

Mix fixes this by helping us with the compilation process:

```
mix compile
```

Then, to run IEx with the compiled files, you can use:

```
iex -S mix
```

## Creating a long running application

You may not want your application to only be runnable from IEx. If you wish to create a long running application, you will have to use the [`Application` module](https://hexdocs.pm/elixir/1.13/Application.html).

First, create a file `application.ex` under `lib/kv/` and add the following contents to the file:

```elixir
defmodule KV.Application do
  use Application

  def start(_type, _args) do
    IO.puts("Hello world!")
    children = []
    Supervisor.start_link(children, strategy: :one_for_one)
  end
end

```

Then, go to `mix.exs` and add the following line to the array returned by the `application` function:

```elixir
def application do
  [
    # ...
    mod: {KV.Application, []}
  ]
end
```

This allows Mix to know to run the `start/2` behavior under `KV.Application` module when you use:

```
mix run
```

You should see `Hello World` printed as specified by `start/2`.
