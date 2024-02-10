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

# 🐦 Directory structure

If you had run the getting started instructions from the previous page, you would see the following directory structure for your project:

```
.
├── .formatter.exs
├── .gitignore
├── README.md
├── _build
├── assets
│   ├── css
│   ├── js
│   ├── tailwind.config.js
│   └── vendor
├── config
│   ├── config.exs
│   ├── dev.exs
│   ├── prod.exs
│   ├── runtime.exs
│   └── test.exs
├── lib
│   ├── practical_elixir_demo
│   ├── practical_elixir_demo.ex
│   ├── practical_elixir_demo_web
│   └── practical_elixir_demo_web.ex
├── mix.exs
├── mix.lock
├── practical_elixir_demo_dev.db
├── practical_elixir_demo_dev.db-shm
├── practical_elixir_demo_dev.db-wal
├── priv
│   ├── gettext
│   ├── repo
│   └── static
└── test
    ├── practical_elixir_demo_web
    ├── support
    └── test_helper.exs
```

Phoenix builds on top of the original folder structure that was discussed under the Mix section: [#project-structure](../elixir-fundamentals/mix.md#project-structure "mention"). For more information about Phoenix's directory structure, refer to the [official guide.](https://hexdocs.pm/phoenix/directory\_structure.html)

1. `_build`: contains the compilation artifacts
2. `assets`: stores front-end assets like JavaScript and CSS; static assets like files go to `priv/static`
3. `config`: common directory used to store project configurations (more information about order of config reading below)
4. `deps`: contains all Mix dependencies listed under `mix.exs` under `deps/0`
5. `lib`: umbrella project for main application source code where `lib/practical_elixir_demo` holds the back-end logic (model) while `lib/practical_elixir_demo_web` holds the front-end logic (view and controller)
6. `priv`: all resources that are necessary in production but not a part of source code like database scripts, static resources, etc.
7. `test`: contains unit tests for application, similar structure as `lib`

The specific structure of each folder in `lib/` will be discussed when we actually modify those files.

The file that end with `*.db-*` are the SQLite3 files that maintain the database.

## Config order

There are several configuration files that are used in Phoenix (or any Mix project for that matter) and in their read order:

1. `config.exs`: base configurations declared for the project, these are read during compile time
2. `dev.exs`/`prod.exs`/`test.exs`: environment specific configurations that run only when the project is run in a given environment (during compile time as well)
3. `runtime.exs`: configurations that are run during runtime

`runtime.exs` are the preferred method for loading environment variables from environment files like `.env` while `config.exs` are preferred for environment variables that may not be secrets and are readily accessible during compile time.

For more information about configuration files in Elixir, please refer to the official [Elixir documentation for the `Config` module](https://hexdocs.pm/elixir/main/Config.html).
