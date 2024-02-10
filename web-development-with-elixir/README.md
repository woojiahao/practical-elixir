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

# 🦅 Web development with Elixir

Web development has become a growing area that Elixir has begun to shine. This is in part thanks to the [Phoenix framework](https://www.phoenixframework.org/) created by [Chris McCord](https://chrismccord.com/) that has made web development very hassle free with Elixir.

{% hint style="info" %}
[**Heroku x Elixir**](https://elixir-lang.org/blog/2020/09/24/paas-with-elixir-at-Heroku/)\
\
Heroku was one of the companies that adopted Elixir and Phoenix for web development of one of their core analytics dashboard and internal systems. What they had noticed from using Elixir was that their systems were able to handle higher loads while maintaining a high response time.
{% endhint %}

## About Phoenix

Phoenix is a server-side framework designed for building web applications with Elixir. It was first created by Chris McCord and modeled after the [Ruby on Rails](https://rubyonrails.org/) framework.

It is a server-side framework, which means that all of the computation is performed on the server and sent to the client to render. Phoenix implements the Model-View-Controller (MVC) pattern:

<figure><img src="../.gitbook/assets/Untitled Diagram.drawio.png" alt="" width="311"><figcaption></figcaption></figure>

You can build both your frontend and backend with Phoenix. The frontend of Phoenix is written as `.heex` files which is Phoenix's template language that embeds Elixir into HTML. The backend of Phoenix uses [Plug](https://hexdocs.pm/plug/readme.html).

## What this guide covers

As mentioned in the beginning of the guide, this guide does not attempt to provide a comprehensive introduction to the Phoenix framework. There are many nuances to how Phoenix can be used and you should refer to the official documentation if you are [interested in learning more about Phoenix](https://hexdocs.pm/phoenix/up\_and\_running.html).

This guide will provide an introduction to Phoenix by getting you to build a to-do list application with some basic data persistence using SQLite3 (we are using a local database to reduce the amount of setup required to following along).

However, there will be bonus chapters that cover slightly more advanced version ideas of Phoenix such as Phoenix LiveView.

### Structure

The following is the content structure of this guide:

1. Directory structure of Phoenix projects
2. Adding your first page
3. Anatomy of Phoenix projects
4. Persisting data with Ecto
5. Building the to-do list

## Getting started

To get started with using web development using Elixir and Phoenix, we will start by installing the Phoenix CLI archive on your machine:

```
mix archive.install hex phx_new
```

Then, you can create a new Phoenix project with the necessary boilerplate:

```
mix phx.new practical_elixir_demo --database sqlite3
```

You may be prompted on whether you want to fetch the dependencies, agree to that option.

Then, navigate to the new folder:

```
cd practical_elixir_demo/
```

Then, create the database. Phoenix uses a very popular Elixir library, Ecto for performing database queries and mapping:

```
mix ecto.create
```

Finally, you can run the demo application:

```
mix phx.server
```

You should see the following show up at [http://localhost:4000](http://localhost:4000/)

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
If you want to view the full codebase without running each command, refer to [this repository instead.](https://github.com/woojiahao/practical\_elixir\_demo)
{% endhint %}
