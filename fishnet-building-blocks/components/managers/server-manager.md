---
description: >-
  ServerManager handles validation of clients and a variety of settings only
  applicable to the server.
---

# ServerManager

## Component Settings <a href="#server-and-host" id="server-and-host"></a>

<details>

<summary>Settings <em>are general settings related to the ServerManager.</em></summary>

**SyncType Rate** is the default send rate for SyncTypes. 0f will send updates every tick. This value can be overridden for individual SyncTypes.

**Spawn Packing** determin es how well transform properties will be packed when being sent in a spawn message. If transforms spawn marginally off reducing the packing may help.

**Change Frame Rate** while true will change the frame rate limitation when acting as server only.

* **Frame Rate** is the frame rate to use while only the server is active. When both server and client are active the higher of the two frame rates will be used.

**Start On Headless** will automatically start the server when executing server builds.

</details>

<details>

<summary>Connections <em>are settings related to each client connection.</em></summary>

**Remote Client Timeout** decides if the server should disconnect clients which seem unresponsive. This feature can be set to disabled, work in development and releases, or only releases.

* **Timeout** is how long the client must be unresponsive before they are kicked.

**Share Ids** while true, enables clients to be aware of other clients in game and objects owned by other clients. Objects owned by other clients are only known if they are available to the local client, such as through the observer system. Client Ids are not sensitive information but leaving this option enable will use additional bandwidth.

</details>

<details>

<summary>Security <em>are settings related to server security.</em></summary>

**Authenticator** is where you specify which [authenticator](../authenticator.md) to use. When left empty clients may join the server without specialized authentication.

**Allow Predicted Spawning** lets prefabs and scene objects be setup to use [Predicted Spawning](../../../guides/features/networked-gameobjects-and-scripts/spawning/predicted-spawning.md). You can use this setting to enable or disable predicted spawning without having to change the settings for every object. This value may also be set at runtime. If changing at runtime be certain to also change on the client; otherwise they could be kicked for trying to use a disabled feature.

* **Reserved Object Ids** is the number of ObjectIds to reserve per client for predicted spawning. Clients will start out with the specified number of Ids and receive new ones as the server validates their predicted spawning requests. For example: if this value was set to 15 and a client with a 100ms ping sent 3 predicted spawns in one tick then they would have only 12 predicted spawns left to use until the server responded with 3 new Ids, which would be approximately 50ms later.

</details>
