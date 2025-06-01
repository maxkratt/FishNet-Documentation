---
description: >-
  An easy tool to assist with spawning specific objects as soon as the server
  starts.
---

# ServerSpawner

The **ServerSpawner** component is a helpful tool that you can use for instantiating and _optionally_ spawning objects as soon as the FishNet server successfully starts running. This is very often used to instantiate [Global Network Objects](#user-content-fn-1)[^1], which need to be instantiated at run-time instead of existing inside a scene directly.

<figure><img src="../../.gitbook/assets/server-spawner-component.png" alt=""><figcaption></figcaption></figure>

## Component Settings <a href="#server-and-host" id="server-and-host"></a>

> **Automatically Spawn** dictates whether the component will call **Spawn** on the objects after it instantiated them.
>
> You can disable this if you want to manually control when these objects are spawned.

> **Network Objects** is a list of the Network Objects the component will instantiate once the server starts. The order of this list is respected when instantiating the objects, and the component will attempt to use [FishNet's object pooling](../../guides/features/networked-gameobjects-and-scripts/spawning/object-pooling.md) for the objects if they are added to the an object pool.

[^1]: These are NetworkObjects that are added to the DontDestroyOnLoad scene and visible to all clients.
