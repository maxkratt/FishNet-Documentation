---
description: >-
  Scene Caching is the ability for the Server to keep a scene loaded when either
  all clients have unloaded that scene, or stopped observing that scene.
---

# Scene Caching

## General

When loading a scene you can specify in the SceneLoadData whether to AutomaticallyUnload the scene when all Clients unload or leave the scene as an observer.\
\
When Unloading the Scene you can override the AutomaticallyUnload options by setting the ServerUnloadMode.&#x20;

## Examples

```csharp
//When Loading Scenes.
SceneLoadData sld = new SceneLoadData("MainScene");
sld.Options.AutomaticallyUnload = false;

//When Manually Unloading Scenes.
//Whatever you set the ServerUnloadMode to here will override the AutomaticallyUnload
//setting you used when loading the scene earlier.
SceneUnloadData sud = new SceneUnloadData("MainScene");
sud.Options.Mode = ServerUnloadMode.KeepUnused;
```

## Host Behaviour

In situations where the Hosts Server needs to keep a scene loaded, but the Hosts Client was requested to unload that scene. Instead of unloading, the Hosts Client will be removed from the scene using the observer system updating the Clients [**Scene Visibility**](broken-reference). \
\
As Host, the Server and Client share the same instance of loaded Scenes and Game Objects. If it were to actually unload a scene from the Host Client, it would also unload on the server.

{% hint style="danger" %}
A **Scene Condition** must be set in the **ObserverManager** for you to utilize the ability to have the server keep a scene loaded, and the hosts client not see the objects in that scene.&#x20;
{% endhint %}

{% hint style="warning" %}
This usually means that every GameObject with a mesh will have to have a NetworkObject attached if you want the Host Client not to visualize the scene.
{% endhint %}
