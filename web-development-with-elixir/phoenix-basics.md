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

# 🦇 Getting to-dos

To first render the to-dos, let's first create a new endpoint to handle this "retrieval" request (i.e. `GET` request) and add a new page to render this content.

## Anatomy of the router

Phoenix uses a router to map HTTP routes to actions (functions) handled by a controller (from the MVC pattern). You define both the front-end and back-end routes via the router.

New routes are added to the `lib/practical_elixir_demo_web/router.ex` file. If you open this file, you will see the following:

{% code title="lib/practical_elixir_demo_web/router.ex" %}
```elixir
defmodule PracticalElixirDemoWeb.Router do
  use PracticalElixirDemoWeb, :router

  pipeline :browser do
    plug :accepts, ["html"]
    plug :fetch_session
    plug :fetch_live_flash
    plug :put_root_layout, html: {PracticalElixirDemoWeb.Layouts, :root}
    plug :protect_from_forgery
    plug :put_secure_browser_headers
  end

  pipeline :api do
    plug :accepts, ["json"]
  end

  scope "/", PracticalElixirDemoWeb do
    pipe_through :browser

    get "/", PageController, :home
  end

  # Other scopes may use custom stacks.
  # scope "/api", PracticalElixirDemoWeb do
  #   pipe_through :api
  # end

  # Enable LiveDashboard and Swoosh mailbox preview in development
  if Application.compile_env(:practical_elixir_demo, :dev_routes) do
    # If you want to use the LiveDashboard in production, you should put
    # it behind authentication and allow only admins to access it.
    # If your application does not have an admins-only section yet,
    # you can use Plug.BasicAuth to set up some basic authentication
    # as long as you are also using SSL (which you should anyway).
    import Phoenix.LiveDashboard.Router

    scope "/dev" do
      pipe_through :browser

      live_dashboard "/dashboard", metrics: PracticalElixirDemoWeb.Telemetry
      forward "/mailbox", Plug.Swoosh.MailboxPreview
    end
  end
end
```
{% endcode %}

### use

```elixir
  use PracticalElixirDemoWeb, :router
```

This is "similar" to inheritance in OOP (but not really) where macros are used to initialize several predefined behaviors for the current router module to behave like a router.

### Pipelines

```elixir
  pipeline :browser do
    plug :accepts, ["html"]
    plug :fetch_session
    plug :fetch_live_flash
    plug :put_root_layout, html: {PracticalElixirDemoWeb.Layouts, :root}
    plug :protect_from_forgery
    plug :put_secure_browser_headers
  end

  pipeline :api do
    plug :accepts, ["json"]
  end
```

Pipelines are comprised of defined "plugs" (this is the name of the library) which defines some kind of behavior. Any requests that are routed through a pipeline will have to pass through these checks before they reach the controller's action. These are useful when you want to modify the request or perform validation on the request before it is even executed.

### Scopes

```elixir
  scope "/", PracticalElixirDemoWeb do
    pipe_through :browser

    get "/", PageController, :home
  end
```

`scope` defines the parent route name and the module that would contain the controllers with the actions.

In the above example, we define the parent route to be `/` and the parent module that contains the controllers as the `PracticalElixirDemoWeb` module. You are free to use a different module to store these controllers but this guide will follow the convention and define the controllers under the `lib/practical_elixir_demo_web/` folder.

## Retrieving the to-do list

To familiarize yourself with the router structure, we can define an API endpoint that returns the dummy list of to-do items.

{% hint style="warning" %}
This guide will not actually require any API endpoints as we would be able to retrieve all of the information directly through the back-end (since this is an umbrella project). However, in projects that have separate repositories for the front-end and back-end, you would use these API endpoints to query for the information!
{% endhint %}

Add the following lines to the router:

```elixir
  scope "/api", PracticalElixirDemoWeb do
    pipe_through :api

    get "/todo", TodoController, :get_todo_list
  end
```

And then add a file under `lib/practical_elixir_demo_web/controllers/` called `TodoController.ex`. Then add the following to the new controller:

```elixir
defmodule PracticalElixirDemoWeb.TodoController do
  use PracticalElixirDemoWeb, :controller

  def get_todo_list(conn, _params) do
    json(conn, PracticalElixirDemo.Todo.get_items())
  end
end
```

We've basically done the following:

1. Create a new route `GET /api/todo`
2. Setup the router to call `TodoController.get_todo_list/0` when the route is called
3. `get_todo_list/0` calls the function to get all to-do items `Todo.get_items/0`&#x20;
4. Returns the resulting to-do list as a JSON using the `json/2` function

Thus, when you run the web application again, you can call this endpoint using a program like cURL or just using the URL in the browser:

```
curl localhost:4000/api/todo
```

```json
[{"description":null,"title":"Finish homework","is_done?":false},{"description":"Ideally somewhere that is cheap and quiet","title":"Find accommodation","is_done?":false},{"description":null,"title":"Take out the rubbish","is_done?":true}]%         
```

As you can see, the newly created endpoint returns the dummy list of to-dos we have.
