---
description: ClientManager provides settings unique to clients.
---

# ClientManager

## Component Settings <a href="#server-and-host" id="server-and-host"></a>

<details>

<summary>Settings <em>are general settings related to the ClientManager.</em></summary>

**Remote Server Timeout** decides if the client should disconnect when the server seems unresponsive. This feature can be set to disabled, work in development and releases, or only releases.

* **Timeout** is how long the server must be unresponsive before they are kicked.

**Change Frame Rate** while true will change the frame rate limitation when acting as server only.

* **Frame Rate** is the frame rate to use while only the client is active. When both server and client are active the higher of the two frame rates will be used.&#x20;

</details>
