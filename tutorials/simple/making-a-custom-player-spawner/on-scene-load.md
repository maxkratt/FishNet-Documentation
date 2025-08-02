---
description: >-
  Tutorial for spawning players as soon as they are loaded by FishNet into a
  specific scene.
---

# Spawning Players on Scene Load

To spawn a player when they load into a scene, let's write a `ScenePlayerSpawner.cs` script that we can add to our desired scene to handle this.

{% stepper %}
{% step %}
### Create the Script

Create a new script in your project and name it something like `ScenePlayerSpawner.cs`.
{% endstep %}

{% step %}
### Make the Script a NetworkBehaviour

Instead of being a regular [MonoBehaviour](https://docs.unity3d.com/ScriptReference/MonoBehaviour.html), we'll make our class inherit from [NetworkBehaviour](../../../guides/features/networked-gameobjects-and-scripts/network-behaviour-guides.md), this will enable us to use the **NetworkBehaviour** callbacks and properties.

{% code fullWidth="true" %}
```csharp
using FishNet.Connection;
using FishNet.Managing;
using FishNet.Object;
using UnityEngine;

public class ScenePlayerSpawner : NetworkBehaviour
```
{% endcode %}

{% hint style="info" %}
Remember that since this class is a **NetworkBehaviour**, it should not be placed on the NetworkManager GameObject or any of its children, but rather on a Networked GameObject in the desired scene.
{% endhint %}
{% endstep %}

{% step %}
### Expose the Prefab in the Inspector

Add a [NetworkObject](../../../guides/features/networked-gameobjects-and-scripts/networkobjects/) variable for holding a reference to our player prefab.

```csharp
[SerializeField] private NetworkObject playerPrefab;
```
{% endstep %}

{% step %}
### Override the OnSpawnServer Method

Let's utilize the NetworkBehaviour's [OnSpawnServer](../../../guides/features/networked-gameobjects-and-scripts/network-behaviour-guides.md#onspawnserver) method to trigger the player spawning for us. This **OnSpawnServer** method gets called by FishNet on the server as soon as the object is being spawned for a client, thus we know he is in the scene and ready to receive a player object.

{% code overflow="wrap" %}
```csharp
public override void OnSpawnServer(NetworkConnection connection)
{
    NetworkObject obj = Instantiate(playerPrefab);
    Spawn(obj, connection, gameObject.scene);
}
```
{% endcode %}

This code simply instantiates the given prefab and then tells FishNet to spawn that object on the network (the `Spawn` method here is directly available because we inherited from **NetworkBehaviour**).
{% endstep %}

{% step %}
### Use FishNet's Object Pooling

We can optionally make the script work with FishNet's [Object Pooling](../../../guides/features/networked-gameobjects-and-scripts/spawning/object-pooling.md) system by changing the `Instantiate` method call to one provided by FishNet. This code will work even if we don't use Object Pooling, so it's a direct improvement here.

The `NetworkManager.GetPooledInstantiated` method requires an additional argument to indicate if this is being called on the server or client. Since we are only going to call it on the server, we provide `true` to the final `asServer` parameter.

{% code overflow="wrap" %}
```csharp
NetworkObject obj = NetworkManager.GetPooledInstantiated(playerPrefab, asServer: true);
```
{% endcode %}
{% endstep %}

{% step %}
### Final Script

The script is now finished and you can add it to a game object in your desired scene and assign the player prefab to it.

{% code title="ScenePlayerSpawner.cs" lineNumbers="true" %}
```csharp
using FishNet.Connection;
using FishNet.Managing;
using FishNet.Object;
using UnityEngine;

public class ScenePlayerSpawner : NetworkBehaviour
{
    [SerializeField] private NetworkObject playerPrefab;

    // This method runs on the server when the client is about to spawn this object.
    // Since the player is about to spawn this object, we know he is in this scene.
    public override void OnSpawnServer(NetworkConnection connection)
    {
        NetworkObject obj = NetworkManager.GetPooledInstantiated(playerPrefab, asServer: true);
        Spawn(obj, connection, gameObject.scene);
    }
}
```
{% endcode %}
{% endstep %}
{% endstepper %}
