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

# 🐣 Viewing to-do list

## Creating a new route

Similar to how we have created the `GET /api/todo` endpoint, we will first create the route in `router.ex`. This time, add it to the existing `scope "/"` that was given.

```elixir
  scope "/", PracticalElixirDemoWeb do
    pipe_through :browser

    get "/", PageController, :home
    
    # Add this
    get "/todo", PageController, :todo
  end
```

## Creating the associated controller action

Once done, we need to add the associated controller action `todo` in the `PageController`. We will disable the default layout as we want to style the web page ourselves.

```elixir
defmodule PracticalElixirDemoWeb.PageController do
  use PracticalElixirDemoWeb, :controller
  
  # ...

  def todo(conn, _params) do
    render(conn, :todo, layout: false)
  end
end

```

## Introducing HEEx (HTML + EEx)

Finally, add a file to `lib/practical_elixir_demo_web/controllers/page_html/` titled `todo.html.heex`. Note that the filename must correspond to the atom specified in the `render/2` function call, i.e. `todo`.

{% hint style="info" %}
Note that this differs from the original method proposed by the Phoenix framework (which involves using the `~H` sigil to render the HTML. This is the alternative approach given and is the method that is recommended.
{% endhint %}

Now that we have the `todo.html.heex` file, we can design the to-do list view of our application.

HEEx is a templating language designed with work with embedding Elixir in HTML. It is very similar to how Vue.js and Django handles embedding (where you can write Javascript/Python inside HTML).

