---
description: >-
  Tutorial for spawning players as soon as they are loaded by FishNet into a
  specific scene.
---

# Spawning Players on Scene Load

To spawn a player when they load into a scene, let's write a `ScenePlayerSpawner.cs` script that we can add to our desired scene to handle this.

## Basic version

{% stepper %}
{% step %}
### Create the script

Create a new script in your project and name it something like `ScenePlayerSpawner.cs`.
{% endstep %}

{% step %}
### Make the script a NetworkBehaviour

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
### Expose the prefab in the inspector

Add a [NetworkObject](../../../guides/features/networked-gameobjects-and-scripts/networkobjects/) variable for holding a reference to our player prefab.

```csharp
[SerializeField] private NetworkObject _playerPrefab;
```
{% endstep %}

{% step %}
### Override the OnSpawnServer method

Let's utilize the NetworkBehaviour's [OnSpawnServer](../../../guides/features/networked-gameobjects-and-scripts/network-behaviour-guides.md#onspawnserver) method to trigger the player spawning for us. This **OnSpawnServer** method gets called by FishNet on the server as soon as the object is being spawned for a client, thus we know he is in the scene and ready to receive a player object.

{% code overflow="wrap" %}
```csharp
public override void OnSpawnServer(NetworkConnection connection)
{
    NetworkObject obj = Instantiate(_playerPrefab);
    Spawn(obj, connection, gameObject.scene);
}
```
{% endcode %}

This code simply instantiates the given prefab and then tells FishNet to spawn that object on the network (the `Spawn` method here is directly available because we inherited from **NetworkBehaviour**).
{% endstep %}

{% step %}
### Use FishNet's object pooling

We can optionally make the script work with FishNet's [Object Pooling](../../../guides/features/networked-gameobjects-and-scripts/spawning/object-pooling.md) system by changing the `Instantiate` method call to one provided by FishNet. This code will work even if we don't use Object Pooling, so it's a direct improvement here.

The `NetworkManager.GetPooledInstantiated` method requires an additional argument to indicate if this is being called on the server or client. Since we are only going to call it on the server, we provide `true` to the final `asServer` parameter.

{% code overflow="wrap" %}
```csharp
NetworkObject obj = NetworkManager.GetPooledInstantiated(_playerPrefab, asServer: true);
```
{% endcode %}
{% endstep %}

{% step %}
### Basic final script

The script can now be added to a game object in your desired scene and assigned a player prefab to it. As long as the client is loaded into this scene after connecting, he should get a player object spawned for him.

{% code title="ScenePlayerSpawner.cs" lineNumbers="true" %}
```csharp
using FishNet.Connection;
using FishNet.Managing;
using FishNet.Object;
using UnityEngine;

public class ScenePlayerSpawner : NetworkBehaviour
{
    [SerializeField] private NetworkObject _playerPrefab;

    // This method runs on the server when the client is about to spawn this object.
    // Since the player is about to spawn this object, we know he is in this scene.
    public override void OnSpawnServer(NetworkConnection connection)
    {
        NetworkObject obj = NetworkManager.GetPooledInstantiated(_playerPrefab, asServer: true);
        Spawn(obj, connection, gameObject.scene);
    }
}
```
{% endcode %}
{% endstep %}
{% endstepper %}

{% hint style="warning" %}
This will work great as long as the scene is not one of the starting scenes. If it is, then FishNet may complain that we are trying to spawn an object for the client before all the starting scenes have been loaded, to fix that, check out the section below.
{% endhint %}

***

## Handling starting scenes

If this scene happens to be a scene that's loaded before or as soon as the client connects, then the previous script will cause a warning. To fix this we need to only spawn the player **after** the client has loaded all starting scenes.

{% stepper %}
{% step %}
### Listening for the event

Let's start solving this by adding the override methods `OnStartServer` and `OnStopServer`. We will use these to subscribe to the [SceneManager](../../../fishnet-building-blocks/components/managers/scenemanager.md)'s [OnClientLoadedStartScenes ](../../../guides/features/network-callbacks.md#shared-events)event.

```csharp
public override void OnStartServer()
{
    SceneManager.OnClientLoadedStartScenes += OnClientLoadedStartScenes;
}

public override void OnStopServer()
{
    if (SceneManager != null)
        SceneManager.OnClientLoadedStartScenes -= OnClientLoadedStartScenes;
}
```

These methods will run on the server side as soon as this [NetworkObject](../../../guides/features/networked-gameobjects-and-scripts/networkobjects/) is spawned and despawned. We use them to subscribe and unsubscribe to the event as shown above.
{% endstep %}

{% step %}
### Handling the event

Now we'll write the code that will spawn the player when this event is triggered, as long as he is actually in the scene (remember this event will be triggered whenever the player has loaded all starting scenes, which may or not include this one at all).

```csharp
private void OnClientLoadedStartScenes(NetworkConnection connection, bool asServer)
{
    // Since this method is called when the client has loaded all start scenes,
    // we need to check if the client is actually in this scene before spawning the player.
    // We use Observers.Contains to see if this connection is observing this object, and
    // thus in this scene. We could have alternatively checked if the connection has this
    // scene loaded like so: connection.Scenes.Contains(gameObject.scene)
    if (asServer && Observers.Contains(connection))
        SpawnPlayer(connection);
}
```

You will notice we are calling a `SpawnPlayer` method here, we'll create that shortly.
{% endstep %}

{% step %}
### Adjusting the OnSpawnServer method

Now we will adjust to the `OnSpawnServer` method to make sure it only spawns a player if he's loaded the starting scenes.

```csharp
// This method runs on the server when the client is about to spawn this object.
// Since the player is about to spawn this object, we know he is in this scene.
// BUT he may not have loaded all start scenes yet, so we check that.
public override void OnSpawnServer(NetworkConnection connection)
{
    if (connection.LoadedStartScenes(true))
        SpawnPlayer(connection);
}
```

Checking the `NetworkConnection.LoadedStartScenes` method will return, as expected, whether or not that client has loaded the starting scenes. It takes a Boolean parameter for whether this check is happening on the server or client side. We'll pass true as this will always happen on the server for us.
{% endstep %}

{% step %}
### The SpawnPlayer method

Finally, we moved the actual player spawning logic into its own method called **SpawnPlayer**.

```csharp
private void SpawnPlayer(NetworkConnection connection)
{
    NetworkObject obj = NetworkManager.GetPooledInstantiated(_playerPrefab, asServer: true);
    Spawn(obj, connection, gameObject.scene);
}
```

This is all pretty self-explanatory, but you may be wondering why this method is called twice; once in **OnSpawnServer** and once in **OnClientLoadedStartScenes**. Couldn't this cause two players to be spawned for a single player when he loads into our scene?

It shouldn't, because in our **OnSpawnServer** method we first checked whether the client had loaded the starting scenes before we allowed it to spawn a player, this prevents it from spawning a player if **OnSpawnServer** runs before **OnClientLoadedStartScenes.**&#x20;

Then in **OnClientLoadedStartScenes** we checked that the client was observing this object before spawning a player, this prevents it from spawning a player if **OnClientLoadedStartScenes** runs before **OnSpawnServer.**

The result of this is that whichever happens later will be the one to spawn the player, and FishNet won't have any issues with starting scenes or later loading scenes.
{% endstep %}

{% step %}
### Final script

The script is now finished and you can add it to a game object in your desired scene and assign the player prefab to it.

{% code title="ScenePlayerSpawner.cs" lineNumbers="true" %}
```csharp
using FishNet.Connection;
using FishNet.Managing;
using FishNet.Object;
using UnityEngine;

public class ScenePlayerSpawner : NetworkBehaviour
{
    [SerializeField] private NetworkObject _playerPrefab;

    public override void OnStartServer()
    {
        SceneManager.OnClientLoadedStartScenes += OnClientLoadedStartScenes;
    }

    public override void OnStopServer()
    {
        if (SceneManager != null)
            SceneManager.OnClientLoadedStartScenes -= OnClientLoadedStartScenes;
    }

    private void OnClientLoadedStartScenes(NetworkConnection connection, bool asServer)
    {
        // Since this method is called when the client has loaded all start scenes,
        // we need to check if the client is actually in this scene before spawning the player.
        // We use Observers.Contains to see if this connection is observing this object, and
        // thus in this scene. We could have alternatively checked if the connection has this
        // scene loaded like so: connection.Scenes.Contains(gameObject.scene)
        if (asServer && Observers.Contains(connection))
            SpawnPlayer(connection);
    }

    // This method runs on the server when the client is about to spawn this object.
    // Since the player is about to spawn this object, we know he is in this scene.
    // BUT he may not have loaded all start scenes yet, so we check that.
    public override void OnSpawnServer(NetworkConnection connection)
    {
        if (connection.LoadedStartScenes(true))
            SpawnPlayer(connection);
    }

    private void SpawnPlayer(NetworkConnection connection)
    {
        NetworkObject obj = NetworkManager.GetPooledInstantiated(_playerPrefab, asServer: true);
        Spawn(obj, connection, gameObject.scene);
    }
}
```
{% endcode %}
{% endstep %}
{% endstepper %}
