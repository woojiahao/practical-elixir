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

# 🦆 Data persistence

{% hint style="info" %}
Depending on time, this chapter may not have been covered during the recording/live talk. Fret not, we have documented the process of setting up data persistence with Phoenix in this chapter.
{% endhint %}

If you want to checkout data persistence in action, go to the `complete` branch by doing `git switch complete`.

The core essentials for data persistence with SQLite3 should have already been setup for you when you first initialized the project. Phoenix uses the widely popular Ecto database library that helps abstract the process of database connection and querying on your behalf.

## Creating a schema and migration

Phoenix comes with various handy [mix.md](../elixir-fundamentals/mix.md "mention") tasks to help with this task. Use the following command to generate the necessary database schema and migration file for the to-do table:

```bash
mix phx.gen.schema Todo todo \
  title:string \
  description:string \
  is_done:boolean \
  --binary-id
```

The command above will automatically generate a file under `lib/practical_elixir_demo/` called `todo.ex` along with a database migration file under `priv/repo/migrations`.

If you open `todo.ex`, you will see the following:

{% code title="lib/practical_elixir_demo/todo.ex" %}
```elixir
defmodule PracticalElixirDemo.Todo do
  alias PracticalElixirDemo.Repo
  use Ecto.Schema
  import Ecto.Changeset

  @primary_key {:id, :binary_id, autogenerate: true}
  @foreign_key_type :binary_id
  schema "todo" do
    field :description, :string
    field :title, :string
    field :is_done, :boolean, default: false

    timestamps(type: :utc_datetime)
  end

  @doc false
  def changeset(todo, attrs) do
    todo
    |> cast(attrs, [:title, :description, :is_done])
    |> validate_required([:title, :description, :is_done])
  end
end
```
{% endcode %}

To quickly run through what you've done so far:

1. `mix phx.gen.schema`: creates a database schema and corresponding migration file
   * Contains fields `title`, `description`, and `is_done`
   * Has auto-generated UUID from `--binary-id`
   * Table name is `todo`&#x20;
   * Elixir module name is `Todo`
2. The schema module contains:
   * `schema` declaration for the same fields we declared during `mix phx.gen.schema`
   * `changeset/2` function that provides model validation

## Running database migrations

Before running the migration for the new `todo` table, let us first delete the existing `Todo` and `TodoItem` modules (by deleting the files). These are all redundant modules. You may notice some compile errors as there were some references to the `get_items/0` function in `Todo`. You may add a dummy function to the new `Todo` module we have made:

{% code title="lib/practical_elixir_demo/todo.ex" %}
```elixir
defmodule PracticalElixirDemo.Todo do
  # ...
  
  def get_items() do
    []
  end
  
  # ...
end
```
{% endcode %}

Finally, move the `todo.ex` file created into the `todo/` folder.

Now, you can run the migration via:

```
mix ecto.migrate
```

Then, make sure you restart the local development server.

You may also need to make the following changes to the HEEx and functional component to work with the latest changes:

{% code title="lib/practical_elixir_demo_web/live/todo_live.ex" %}
```html
    <div class="flex gap-x-4 mb-4 last:mb-0 items-center">
      <%= if @item.is_done do %>
        <p>✅</p>
      <% else %>
        <p>❌</p>
      <% end %>
      <div class="w-full">
        <div class="flex justify-between items-center w-full">
          <p>
            <%= @item.title %>
          </p>

          <div>
            <%= if @item.is_done do %>
              <button
                class="bg-blue-300 px-3 py-1 font-bold rounded-md text-sm"
                type="button"
                phx-value-id={@id}
                phx-click="mark-todo"
              >
                Mark as Not Done
              </button>
            <% else %>
              <button
                class="bg-green-300 px-3 py-1 font-bold rounded-md text-sm"
                type="button"
                phx-value-id={@id}
                phx-click="mark-todo"
              >
                Mark as Done
              </button>
            <% end %>
            <button class="bg-yellow-300 px-3 py-1 font-bold rounded-md text-sm">Edit</button>
            <button class="bg-red-300 px-3 py-1 font-bold rounded-md text-sm">Delete</button>
          </div>
        </div>
        <p class="italic">
          <%= @item.description %>
        </p>
      </div>
    </div>
```
{% endcode %}

Note the use of `is_done` instead of `is_done?`

{% code title="lib/practical_elixir_demo_web/live/todo_live.html.heex" %}
```html
<div class="w-[40%] mx-auto my-8">
  <h1 class="font-bold text-3xl my-8 py-4 px-4 bg-slate-100">Todo List</h1>
  <form phx-submit="add-todo" class="flex justify-between items-center mb-8">
    <input type="text" placeholder="New task" name="task-name" class="rounded-md w-full" />
    <button type="submit" class="px-4 py-2 bg-green-100 rounded-md ml-2">
      Add
    </button>
  </form>
  <%= for item <- @todo_list do %>
    <.todo_item item={item} id={item.id} />
  <% end %>
</div>
```
{% endcode %}

Since `todo_list` should now contain the database schema objects that already have a built-in `id` field (random UUID), we can use that as the `id` instead of the relative position.

If you refresh your page, you should see that there are no to-do items in the to-do list. This is perfectly normal as your database table is still empty.

## Retrieving all to-do items

Let's first populate the behavior of the dummy `get_items/0` function we made earlier. We can use the `PracticalElixirDemo.Repo` module to provide some helper functions to easily do that:

{% code title="lib/practical_elixir_demo/todo/todo.ex" %}
```elixir
defmodule PracticalElixirDemo.Todo do
  alias PracticalElixirDemo.Repo

  # ...

  def get_items() do
    Repo.all(__MODULE__)
  end
  
  # ...
end

```
{% endcode %}

`Repo.all(__MODULE__)` is the same as `Repo.all(PracticalElixirDemo.Todo)` and all it does is perform a `SELECT * FROM todo;` for us and map the results into `PracticalElixirDemo.Todo` structs that we can use in the front-end.

## Creating to-do items

Then, to create a new to-do item, we can continue using the helper functions from `Repo` and use the `Repo.insert/2` function:

{% code title="lib/practical_elixir_demo/todo/todo.ex" %}
```elixir
defmodule PracticalElixirDemo.Todo do
  alias PracticalElixirDemo.Repo

  # ...

  def create_todo(title, description \\ nil) do
    Repo.insert(%__MODULE__{
      title: title,
      description: description,
      is_done: false
    })
  end
  
  # ...
end

```
{% endcode %}

Similar to `Repo.all/1`, `Repo.insert/2` performs an `INSERT INTO todo VALUES (...)` query on your behalf.

We can replace the `add-todo` event in our LiveView controller with a call to this `create_todo/2` function:

{% code title="lib/practical_elixir_demo_web/live/todo_live.ex" %}
```elixir
  def handle_event("add-todo", %{"task-name" => task_name}, socket) do
    Todo.create_todo(task_name)
    {:noreply, assign(socket, todo_list: Todo.get_items())}
  end
```
{% endcode %}

## Marking items as done/not done

The final piece of behavior we are migrating is the mark as done/not done functionality. In this case, we will collapse the behavior into a single `mark-todo` event that both the buttons will use (be sure to update the functional component's `phx-click` binding):

{% code title="lib/practical_elixir_demo_web/live/todo_live.ex" %}
```elixir
  def handle_event("mark-todo", %{"id" => task_id}, socket) do
    Todo.mark_todo(task_id)
    {:noreply, assign(socket, todo_list: Todo.get_items())}
  end
```
{% endcode %}

This way, we leave the toggling behavior to the `Todo` model:

```elixir
defmodule PracticalElixirDemo.Todo do
  alias PracticalElixirDemo.Repo

  # ...

  def mark_todo(id) do
    todo = Repo.one!(from t in __MODULE__, where: t.id == ^id)
    updated_todo = Ecto.Changeset.change(todo, is_done: !todo.is_done)
    Repo.update!(updated_todo)
  end
  
  # ...
end

```

We first retrieve the matching to-do based on the given `id`, and then, we toggle the `is_done` field using `Ecto.Changeset.change/2` function and then perform the update using `Repo.update!/1`.

{% hint style="info" %}
The `!` that follow `one` and `update` are to indicate functions that raise an exception when an entry is not found or cannot be updated respectively. They have counterparts that return an `:error` state instead, but we have opted to avoid using them this time.
{% endhint %}

## Voilà :tada:

If you restart your application now, you can play around with the to-do list and you will notice that the to-do items are persisted even after refreshing the page.

This also concludes this guide on practical functional programming with Elixir and Phoenix! As mentioned earlier, the complete code for the application built for this guide is found on the `complete` branch of [this repository.](https://github.com/woojiahao/practical\_elixir\_demo) If you are interested in learning more about web development with Phoenix, please refer to the [resources.md](../resources.md "mention") for the recommended readings/resources to follow!

All the best in your Elixir journey! :D&#x20;
