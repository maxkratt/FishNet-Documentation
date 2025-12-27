---
description: >-
  Tutorial for allowing your players to choose a character object before
  spawning it.
---

# Spawning Selected Player

Quite a few games allow their players to choose a character before playing the game, we'll go over one way to achieve that here. You will be able to put the script in your game scene and then have your clients call the `SpawnPlayer` [ServerRpc](../../../guides/features/network-communication/remote-procedure-calls.md#serverrpc) method to have the server spawn their chosen character.

We will also have a list of allowed player prefabs to prevent your clients from spawning any network object prefab in your game, such as NPCs, projectiles, items, etc.

{% stepper %}
{% step %}
### Create the script

Create a new script in your project for your custom player spawner. We have called ours `SelectablePlayerSpawner`.
{% endstep %}

{% step %}
### Add the namespaces and inheritance

Our script will make use of a few different FishNet namespaces and will inherit from **NetworkBehaviour** to make use of its exposed properties.

```csharp
using FishNet.Connection;
using FishNet.Managing;
using FishNet.Object;
using System.Linq;
using UnityEngine;
​
public class SelectablePlayerSpawner : NetworkBehaviour
{
}
```

{% hint style="info" %}
Remember that since this class is a **NetworkBehaviour**, it should not be placed on the NetworkManager GameObject or any of its children, but rather on a Networked GameObject in the desired scene.
{% endhint %}
{% endstep %}

{% step %}
### Create an array of permitted prefabs

Let's add a [NetworkObject](../../../guides/features/networked-gameobjects-and-scripts/networkobjects/) array for holding all the permitted prefabs your players can choose from. We'll populate this array inside the Unity inspector.

```csharp
[SerializeField] private NetworkObject[] _playerPrefabs;
```
{% endstep %}

{% step %}
### Create a SpawnPlayer method

Now we can create the method that our players will be able to call when they want to spawn in their chosen player object. It will be a **ServerRpc** because it will be called on the client side and run the body of the method on the server side. We also need to disable the ownership requirement of the ServerRpc, so that any client can call it.

We'll give the method two parameters, a NetworkObject parameter for the players to fill in their chosen player character, and a [NetworkConnection](../../../guides/features/server-and-client-identification/networkconnections.md) parameter which we'll use to know who sent the RPC.

```csharp
[ServerRpc(RequireOwnership = false)]
public void SpawnPlayer(NetworkObject _playerPrefab, NetworkConnection sender = null)
```

{% hint style="info" %}
Be sure to make the **NetworkConnection** parameter the final parameter in the method and defaulted to null, this will allow FishNet to fill it in with the NetworkConnection of the sender automatically for us.
{% endhint %}

Now let's write the body of the method. First, we'll check if the client who's called it already has a player object, we don't want them to have multiple. We'll check if the NetworkConnection's [**FirstObject**](https://fish-networking.com/FishNet/api/api/FishNet.Connection.NetworkConnection.html#FishNet_Connection_NetworkConnection_FirstObject) property is null; this property will contain the first network object owned by a client, which will usually be their player object. We can also set this ourselves with [NetworkConnection.SetFirstObject](https://fish-networking.com/FishNet/api/api/FishNet.Connection.NetworkConnection.html#FishNet_Connection_NetworkConnection_SetFirstObject_FishNet_Object_NetworkObject_).

After that, we'll ensure the player can only spawn one of our permitted prefabs. If they try to spawn some other ones, we'll kick them for trying to cheat.

```csharp
{
    if (sender.FirstObject != null)
    {
        Debug.LogWarning($"Client {sender.ClientId} already has a player object; not spawning another.");
        return;
    }

    if (!_playerPrefabs.Contains(_playerPrefab))
    {
        Debug.LogWarning("Invalid player prefab selected, cannot spawn.");
        // You don't have to kick the player, but there isn't any good reason 
        // they should be trying to spawn a non-permitted prefab.
        sender.Kick(FishNet.Managing.Server.KickReason.ExploitAttempt);
        return;
    }

    NetworkObject obj = Instantiate(_playerPrefab);
    Spawn(obj, sender, gameObject.scene);
}
```

Lastly we instantiate the prefab and then spawn it in the correct scene, while also giving the client ownership of it.

Now we have a functioning selectable player spawner, but we can still make some improvements.
{% endstep %}

{% step %}
### Enable use of object pooling

We can also make the script work with FishNet's [Object Pooling](../../../guides/features/networked-gameobjects-and-scripts/spawning/object-pooling.md) system by changing the `Instantiate` method call to one provided by FishNet. This code will work even if we don't use Object Pooling, so it's a direct improvement here.

The `NetworkManager.GetPooledInstantiated` method requires an additional argument to indicate if this is being called on the server or client. Since we are only going to call it on the server, we provide `true` to the final `asServer` parameter.

```csharp
    NetworkObject obj = NetworkManager.GetPooledInstantiated(_playerPrefab, asServer: true);
```
{% endstep %}

{% step %}
### Handle starting scenes

Now the script works well for situations where the clients are loaded into the scene by the [FishNet SceneManager](../../../guides/features/scene-management/), but what if you start the game in this scene and **don't** use the FishNet SceneManager to load it? In that case we can tell FishNet that the client has loaded this scene and should observe it. Let's add the following three methods to handle this.

In [OnStartServer](../../../guides/features/networked-gameobjects-and-scripts/network-behaviour-guides.md#onstartserver) we subscribe to the [SceneManager.OnClientLoadedStartScenes](https://fish-networking.com/FishNet/api/api/FishNet.Managing.Scened.SceneManager.html#FishNet_Managing_Scened_SceneManager_OnClientLoadedStartScenes) event and in [OnStopServer](../../../guides/features/networked-gameobjects-and-scripts/network-behaviour-guides.md#onstopserver) we simply unsubscribe from it.

OnClientLoadedStartScenes is our own method that will be invoked by the event and will tell FishNet that the client should begin observing this object's scene if he isn't already.

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

private void OnClientLoadedStartScenes(NetworkConnection conn, bool asServer)
{
    if (asServer && !conn.Scenes.Contains(gameObject.scene))
        SceneManager.AddConnectionToScene(conn, gameObject.scene);
}
```
{% endstep %}

{% step %}
### The final script

And that's all there is to it, you should now have a script like this that you can use. Add it to a game object in your desired scene and assign the player prefab to it.

{% code title="ManualPlayerSpawner.cs" lineNumbers="true" %}
```csharp
using FishNet.Connection;
using FishNet.Managing;
using FishNet.Object;
using System.Linq;
using UnityEngine;

public class SelectablePlayerSpawner : NetworkBehaviour
{
    [SerializeField] private NetworkObject[] _playerPrefabs;

    public override void OnStartServer()
    {
        SceneManager.OnClientLoadedStartScenes += OnClientLoadedStartScenes;
    }

    public override void OnStopServer()
    {
        if (SceneManager != null)
            SceneManager.OnClientLoadedStartScenes -= OnClientLoadedStartScenes;
    }

    private void OnClientLoadedStartScenes(NetworkConnection conn, bool asServer)
    {
        if (asServer && !conn.Scenes.Contains(gameObject.scene))
            SceneManager.AddConnectionToScene(conn, gameObject.scene);
    }

    [ServerRpc(RequireOwnership = false)]
    public void SpawnPlayer(NetworkObject _playerPrefab, NetworkConnection sender = null)
    {
        if (sender.FirstObject != null)
        {
            Debug.LogWarning($"Client {sender.ClientId} already has a player object; not spawning another.");
            return;
        }

        if (!_playerPrefabs.Contains(_playerPrefab))
        {
            Debug.LogWarning("Invalid player prefab selected, cannot spawn.");
            // You don't have to kick the player, but there isn't any good reason 
            // they should be trying to spawn a non-permitted prefab.
            sender.Kick(FishNet.Managing.Server.KickReason.ExploitAttempt);
            return;
        }

        NetworkObject obj = NetworkManager.GetPooledInstantiated(_playerPrefab, asServer: true);
        Spawn(obj, sender, gameObject.scene);
    }
}
```
{% endcode %}
{% endstep %}
{% endstepper %}
