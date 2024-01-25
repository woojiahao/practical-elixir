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

# 🦋 History of Elixir

Elixir was first developed in 2011 by Jose Valim, a former Ruby on Rails core contributor. Jose started working on Elixir after noticing that there was an increase in demand and interest in distributed and parallel computing, something that Ruby and Ruby on Rails was not able to fully handle.

Jose took inspiration from two main sources. The first was the functional programming paradigm, more specifically the [referential transparency](why-functional-programming.md#referential-transparency) principle. By ensuring immutability of state, Elixir was able to avoid an entire class of problems that plagued imperative languages like Ruby, caused by race conditions.

The second point of inspiration for Elixir was the Erlang virtual machine (and by extension the Open Telemetry Platform or OTP). Erlang was first designed to be used for telecom systems that emphasized a need for not only highly concurrent systems, but also systems that were distributed across devices/servers. This resonated with Jose as he wanted to build a language that not only supported concurrency within the machine, but to also be able to communicate seamlessly with other machines.

This eventually led to the birth of Elixir and the community has been growing since then. Elixir has seen itself used in many domains, such as [IoT,](https://nerves-project.org/) [web development,](https://www.phoenixframework.org/) and [distributed computing.](https://discord.com/blog/how-discord-scaled-elixir-to-5-000-000-concurrent-users)
