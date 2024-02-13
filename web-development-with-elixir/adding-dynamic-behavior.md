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

# 🐔 Adding dynamic behavior

You may notice that viewing the to-do list may be nice, but it is sorely lacking the key features of a to-do application like creating a new to-do and marking a to-do as done.

If you have dug around with the original Phoenix documentation, you will notice that there is no mention on how this can be achieved. This is because in the base version of Phoenix, handling button actions and performing such dynamic changes requires the use of JavaScript. This is not uncommon for most server-side frameworks.

> "But I don't want to use JavaScript!"\
> \- Presumably you

This is where **Phoenix LiveView** comes in. Phoenix LiveView is an "add-on" on top of base Phoenix that provides real-time server-rendered HTML.

{% hint style="danger" %}
The following content covered will require a bit of time to grasp as it is a relatively new concept introduced over this guide. Do take your time when reading and understanding.
{% endhint %}

## LiveView

Phoenix LiveView inverts the traditional request-response lifecycle that we have seen so far. Instead of sending a request to the API/server and re-rendering the front-end based on the response, LiveView first uses a regular HTTP request-response to retrieve the initial page.&#x20;

The key benefit from doing this is reducing the amount of time spent waiting for HTTP responses in a traditionally stateless application.

However, once the page is rendered, a persistent connection (via a socket) is established between client and server and this connection is used to communicate any changes/actions performed from the front-end to the back-end and vice versa.

```mermaid
sequenceDiagram
  actor User
  participant Client
  participant Server
  participant Database
  User ->> Client : request page
  Client ->> Server : request page over HTTP
  Server ->> Database : initial data request
  Database -->> Server : initial data
  Server -->> Client : initial page as HTTP response
  Client -->> User : render initial page
  note over Client,Server : all subsequent requests are done via a<br/>persistent connection between<br/>Client and Server 
```

This gives rise to the following lifecycle for LiveView (courtesy of [John Elm Labs](https://johnelmlabs.com/posts/liveview-lifecycle-flow-chart) for this diagram):

```mermaid
graph TB
    HTTP_Request["HTTP Request"]
    mount_disconnected["mount/3 Callback (Disconnected)"]
    handle_params_disconnected["handle_params/3 Callback (Disconnected)"]
    render_disconnected["render/1 Callback (Disconnected)"]
    LiveView_Connects["LiveView Connects (Stateful views are spawned)"]
    mount_connected["mount/3 Callback (Connected)"]
    handle_params_connected["handle_params/3 Callback (Connected)"]
    render_connected["render/1 Callback (Connected)"]
    Continuous_Connection["Continuous Connection"]
    handle_event["Callbacks\nhandle_event/3\nhandle_call/3\nhandle_info/2\nhandle_continue/2\nhandle_cast/2\n"]
    Reconnect["Reconnect"]
    terminate["Terminate callback - handle cleanup"]

    HTTP_Request --> mount_disconnected
    mount_disconnected --> handle_params_disconnected
    handle_params_disconnected --> render_disconnected
    render_disconnected --> LiveView_Connects
    LiveView_Connects --> mount_connected
    mount_connected --> handle_params_connected
    handle_params_connected --> render_connected
    render_connected --> Continuous_Connection
    Continuous_Connection --> handle_event
    handle_event --> render_connected
    Continuous_Connection -- "If crash or connection drop" --> Reconnect
    Reconnect --> mount_connected
    Continuous_Connection -- "Patch" --> handle_params_connected

    classDef orange fill:#f96,stroke:#333;
    class mount_connected orange;
    class Continuous_Connection orange;
    class handle_event orange;
```

It may seem like a handful but we will only be focusing on the components colored orange as they are the most fundamental ideas of LiveView.&#x20;
