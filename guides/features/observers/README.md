---
description: >-
  Fish-Networking features an advanced area-of-interest system that controls
  which clients receive updates about specific objects.
---

# Area of Interest (Observer System)

An **observer** is a client which can see an object and use communications for the object. You may control which clients can observe an object by using the [NetworkObserver](../../../fishnet-building-blocks/components/network-observer.md) and/or [ObserverManager](../../../fishnet-building-blocks/components/managers/observermanager/) components.

If a client is not an observer of an object, then the object will not activate, and the client will not receive network messages or callbacks for that object. Should the object be a [scene object](../../high-level-overview/terminology/miscellaneous.md#scene-object) then it will remain disabled on the client until they become an observer of it. If the object is [instantiated](../../high-level-overview/terminology/miscellaneous.md#instantiated-object), then the client will simply not instantiate the object until after becoming an observer.

The observer system is designed to work out of the box for new developers. When it comes time to customize how clients observe objects, the observer system additionally offers a large amount of flexibility, keeping in mind there are many condition types, and that you may also create your own.

FishNet comes with a NetworkManager prefab which contains the recommended minimum components to begin working on a new project. Within that prefab is the [ObserverManager](../../../fishnet-building-blocks/components/managers/observermanager/) with an included [Scene Condition](../../../fishnet-building-blocks/components/network-observer.md#component-settings). If you have not familiarized yourself with the ObserverManager and condition types, please do so now using the links above.

A common problem new developers encounter is scene network objects not being enabled for clients. This occurs when the client is not considered part of the scene where the object resides, and the scene condition is preventing that object from spawning for the client. The NetworkManager prefab contains a PlayerSpawner script which adds the player to the current scene; this makes the client an observer for objects in that scene. This requires a player object to be spawned. Should you have made your own NetworkManager object or removed the PlayerSpawner script you will need to add the client to the scene you wish the client to be an observer of.

<div data-full-width="false"><figure><img src="../../../.gitbook/assets/player-spawner-component.png" alt=""><figcaption><p>The "Add To Default Scene" option enabled on the PlayerSpawner</p></figcaption></figure></div>

When encountering such an issue you may of course also remove the ObserverManager or scene condition from the ObserverManager, but this is not recommended as objects in other scenes will attempt to spawn for clients which do not occupy such scenes. Alternatively, you may add the client to the scene where the objects reside; there's a variety of ways to accomplish this.

You can use a script such as the following on an object in the scene. But be careful not to place it on a potentially DDoL object such as the NetworkManager.

{% code expandable="true" %}
```csharp
using FishNet;
using FishNet.Connection;
using UnityEngine;

public class InitialSceneObserver : MonoBehaviour
{
    private void Start()
    {
        var networkManager = InstanceFinder.NetworkManager;
        if (networkManager != null)
            networkManager.SceneManager.OnClientLoadedStartScenes += OnClientLoadedStartScenes;
    }

    private void OnDestroy()
    {
        var networkManager = InstanceFinder.NetworkManager;
        if (networkManager != null)
            networkManager.SceneManager.OnClientLoadedStartScenes -= OnClientLoadedStartScenes;
    }

    private void OnClientLoadedStartScenes(NetworkConnection conn, bool asServer)
    {
        if (!asServer)
            return;

        if (!conn.Scenes.Contains(gameObject.scene))
            InstanceFinder.NetworkManager.SceneManager.AddConnectionToScene(conn, gameObject.scene);
    }
}
```
{% endcode %}

Under the assumption you removed the PlayerSpawner and/or are not using _SceneManager.AddOwnerToDefaultScene_ or the method above, then you must load the client into the scene using the SceneManager. Clients are only considered networked into scenes when those scenes are loaded using the SceneManager. Clients may become part of a scene by loading a scene globally or by loading a scene for a specific client (connection). See the [SceneManager](../scene-management/) section for more information on how to manage networked client scenes as well understand the difference between global and connection scenes.
