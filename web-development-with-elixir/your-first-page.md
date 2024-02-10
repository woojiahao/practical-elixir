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

# 🦤 Your first page

Phoenix (not LiveView, just regular Phoenix) relies on the typical HTTP request lifecycle that many other web applications use:

```mermaid
sequenceDiagram
    actor User
    participant Client
    participant Server
    participant Database
    User ->> Client : perform action
    Client ->> Server : HTTP request
    Server ->> Database : fetch information
    Database -->> Server : information
    Server -->> Client : HTTP response
    Client -->> User : render result of action
```

