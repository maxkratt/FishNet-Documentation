---
description: >-
  Tutorial for creating a script to manually spawn your players when you call a
  method.
---

# Spawning Players Manually

You may find yourself needing to spawn your players from some custom action other than the previously mentioned options, we can write a script that has a public method you can easily call to trigger it whenever you need to.

{% stepper %}
{% step %}
### Create the Script

Create a new script in your project for your custom player spawner. We have called ours `ManualPlayerSpawner`.
{% endstep %}

{% step %}
### Add the Namespaces and Inheritance

Our script will make use of a few different FishNet namespaces and will inherit from **NetworkBehaviour** to make use of its exposed properties.

```csharp
using FishNet.Connection;
using FishNet.Managing;
using FishNet.Object;
using UnityEngine;
​
public class ManualPlayerSpawner : NetworkBehaviour
{
}
```

{% hint style="info" %}
Remember that since this class is a **NetworkBehaviour**, it should not be placed on the NetworkManager GameObject or any of its children, but rather on a Networked GameObject in the desired scene.
{% endhint %}
{% endstep %}

{% step %}
### Expose the Prefab in the Inspector

Let's add a [NetworkObject](../../../guides/features/networked-gameobjects-and-scripts/networkobjects/) variable for holding a reference to our player prefab.

```csharp
[SerializeField] private NetworkObject playerPrefab;
```
{% endstep %}

{% step %}
### Create a SpawnPlayers Method

Now we can create the method that we will call when we want to spawn in the players.

The method will first to a simple null check to make sure we don't get any null reference errors if we forget to assign the player prefab.

After that it will loop through all the connected clients and as long as they are authenticated, it will instantiate and spawn the prefab for them.

```csharp
public void SpawnPlayers()
{
    if (playerPrefab == null)
    {
        Debug.LogWarning("Player prefab is not assigned and thus cannot be spawned.");
        return;
    }

    foreach (NetworkConnection client in ServerManager.Clients.Values)
    {
        // Since the ServerManager.Clients collection contains all clients (even non-authenticated ones),
        // we need to check if they are authenticated first before spawning a player object for them.
        if (!client.IsAuthenticated)
            continue;

        NetworkObject obj = Instantiate(playerPrefab);
        Spawn(obj, client, gameObject.scene);
    }
}
```

Wonderful, with that we already have a functioning player spawner, but we can still make some improvements.
{% endstep %}

{% step %}
### Prevent Accidentally Running on Client

Let's prevent ourselves from accidentally running this method on the client side, remember, spawning should only happen by the server.

Add the `Server` attribute to the method.

```csharp
[Server]
public void SpawnPlayers()
```

This will also allow FishNet to strip out this code from client builds when using FishNet Pro's code stripping feature.
{% endstep %}

{% step %}
### Enable Use of Object Pooling

We can also make the script work with FishNet's [Object Pooling](../../../guides/features/networked-gameobjects-and-scripts/spawning/object-pooling.md) system by changing the `Instantiate` method call to one provided by FishNet. This code will work even if we don't use Object Pooling, so it's a direct improvement here.

The `NetworkManager.GetPooledInstantiated` method requires an additional argument to indicate if this is being called on the server or client. Since we are only going to call it on the server, we provide `true` to the final `asServer` parameter.

```csharp
NetworkObject obj = NetworkManager.GetPooledInstantiated(playerPrefab, asServer: true);
```
{% endstep %}

{% step %}
### Handle Starting Scenes

Now the script works well for situations where the clients are loaded into the scene by the [FishNet SceneManager](../../../guides/features/scene-management/), but what if you start the game in this scene and **don't** use the FishNet SceneManager to load it? In that case we can tell FishNet that the client has loaded this scene and should observe it. Let's add the following code after checking that the client is authenticated and before instantiating the object for him.

```csharp
// If the client isn't observing this scene, make him an observer of it.
if (!client.Scenes.Contains(gameObject.scene))
    SceneManager.AddConnectionToScene(client, gameObject.scene);
```

***

**Alternatively**, perhaps you only want clients who are already loaded in this scene to have a player spawned for them; for example if you have multiple games running in different scenes in the same server instance.&#x20;

To do that you can simply `continue` the loop instead of adding the connection to this scene, thus skipping the spawning of that player.

```csharp
// If the client isn't in this scene, ignore him.
if (!client.Scenes.Contains(gameObject.scene))
    continue;
```
{% endstep %}

{% step %}
### The Final Script

And that's all there is to it, you should now have a script like this that you can use. Add it to a game object in your desired scene and assign the player prefab to it.

{% code title="ManualPlayerSpawner.cs" lineNumbers="true" %}
```csharp
using FishNet.Connection
using FishNet.Managing;
using FishNet.Object;
using UnityEngine;

public class ManualPlayerSpawner : NetworkBehaviour
{
    [SerializeField] private NetworkObject playerPrefab;

    // The Server attribute here prevents this method from being called except on the server.
    [Server]
    public void SpawnPlayers()
    {
        if (playerPrefab == null)
        {
            Debug.LogWarning("Player prefab is not assigned and thus cannot be spawned.");
            return;
        }

        foreach (NetworkConnection client in ServerManager.Clients.Values)
        {
            // Since the ServerManager.Clients collection contains all clients (even non-authenticated ones),
            // we need to check if they are authenticated first before spawning a player object for them.
            if (!client.IsAuthenticated)
                continue;

            // If the client isn't observing this scene, make him an observer of it.
            if (!client.Scenes.Contains(gameObject.scene))
                SceneManager.AddConnectionToScene(client, gameObject.scene);

            NetworkObject obj = NetworkManager.GetPooledInstantiated(playerPrefab, asServer: true);
            Spawn(obj, client, gameObject.scene);
        }
    }
}
```
{% endcode %}
{% endstep %}
{% endstepper %}
