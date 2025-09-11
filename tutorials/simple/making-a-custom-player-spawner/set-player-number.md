---
description: >-
  Tutorial for spawning players as soon as a set number of clients have joined
  your game.
---

# Spawning Players When Set Number of Players Joined

{% stepper %}
{% step %}
### Create the Script

Create a new script in your project for your custom player spawner. We have given ours the name: `CountBasedPlayerSpawner`.
{% endstep %}

{% step %}
### Add the needed fields and namespaces

Our class will take a NetworkObject prefab which will be spawned for our players and an integer for how many players need to connect before we spawn the players.

We're also storing a reference to the NetworkManager this script will use. This is necessary as we won't be inheriting from NetworkBehaviour for this player spawner script; as a result, we will be able to place this component directly on the NetworkManager.

```csharp
using FishNet;
using FishNet.Connection;
using FishNet.Managing;
using FishNet.Managing.Server;
using FishNet.Object;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;

public class CountBasedPlayerSpawner : MonoBehaviour
{
    [SerializeField] private NetworkObject playerPrefab;
    [SerializeField] private int requiredPlayerCount;
    
    private NetworkManager networkManager;
}
```
{% endstep %}

{% step %}
### Get a reference to the NetworkManager

Let's now get a reference to the NetworkManager, we will first try to get it from this object or one of this object's parents, but if that fails we will get it from the [InstanceFinder](../../../guides/features/instancefinder-guides.md).

```csharp
private void Awake()
{
    networkManager = GetComponentInParent<NetworkManager>();
    if (networkManager == null)
        networkManager = InstanceFinder.NetworkManager;

    if (networkManager == null)
    {
        Debug.LogWarning($"CountBasedPlayerSpawner cannot work as a NetworkManager couldn't be found.");
        return;
    }
}
```
{% endstep %}

{% step %}
### Listen for clients joining

With access to the **NetworkManager**, we can now monitor when clients join the game. To achieve this, we’ll utilize the `SceneManager.OnClientLoadedStartScenes` event, which is triggered once a client finishes loading the initial scenes after connecting.

Subscribe to the event at the end of the Awake method.

{% code overflow="wrap" %}
```csharp
networkManager.SceneManager.OnClientLoadedStartScenes += OnClientLoadedStartScenes;
```
{% endcode %}

And also unsubscribe when this object is destroyed.

```csharp
private void OnDestroy()
{
    if (networkManager != null)
        networkManager.SceneManager.OnClientLoadedStartScenes -= OnClientLoadedStartScenes;
}
```
{% endstep %}

{% step %}
### Implement the OnClientLoadedStartScenes method

Now let's implement the `OnClientLoadedStartScenes` method that will do all of the player spawning for us.

This event runs for the server and client so we will exit early if it's not running on the server.

Then we'll create a list of all the clients that are currently connected to the server and authenticated. If there aren't enough clients, then we can return already.

Otherwise, for each of those authenticated clients, let's spawn a player object for them.

```csharp
private void OnClientLoadedStartScenes(NetworkConnection _, bool asServer)
{
    if (!asServer)
        return;

    List<NetworkConnection> authenticatedClients = networkManager.ServerManager.Clients.Values
        .Where(conn => conn.IsAuthenticated).ToList();

    if (authenticatedClients.Count < requiredPlayerCount) return;

    foreach (NetworkConnection client in authenticatedClients)
    {
        NetworkObject obj = Instantiate(playerPrefab);
        networkManager.ServerManager.Spawn(obj, client);
    }
}
```
{% endstep %}

{% step %}
### Enable use of object pooling

We can also make the script work with FishNet's [Object Pooling](../../../guides/features/networked-gameobjects-and-scripts/spawning/object-pooling.md) system by changing the `Instantiate` method call to one provided by FishNet. This code will work even if we don't use Object Pooling, so it's a direct improvement here.

The `NetworkManager.GetPooledInstantiated` method requires an additional argument to indicate if this is being called on the server or client. Since we are only going to call it on the server, we provide `true` to the final `asServer` parameter.

```csharp
NetworkObject obj = NetworkManager.GetPooledInstantiated(playerPrefab, asServer: tr
```
{% endstep %}

{% step %}
### Handle starting scenes

This script functions well when clients are loaded into the scene via the FishNet [SceneManager](../../../guides/features/scene-management/). However, if the game begins in this scene without using the FishNet SceneManager to load it, we need to manually inform FishNet that the client has entered the scene and should begin observing it. To handle this, insert the following code right after our call to `Spawn`.

```csharp
// If the client isn't observing this scene, make him an observer of it.
if (!client.Scenes.Contains(gameObject.scene))
    SceneManager.AddConnectionToScene(client, gameObject.scene);
```
{% endstep %}

{% step %}
### Final script

The script is now completed and ready to use. You can add it to your NetworkManager object or a different object that exists before the clients start connecting. Don't forget to assign its `playerPrefab` and `requiredPlayerCount` fields.

{% code title="CountBasedPlayerSpawner.cs" lineNumbers="true" %}
```csharp
using FishNet;
using FishNet.Connection;
using FishNet.Managing;
using FishNet.Managing.Server;
using FishNet.Object;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;

public class CountBasedPlayerSpawner : MonoBehaviour
{
    [SerializeField] private NetworkObject playerPrefab;
    [SerializeField] private int requiredPlayerCount;

    private NetworkManager networkManager;

    private void Awake()
    {
        networkManager = GetComponentInParent<NetworkManager>();
        if (networkManager == null)
            networkManager = InstanceFinder.NetworkManager;

        if (networkManager == null)
        {
            Debug.LogWarning($"CountBasedPlayerSpawner cannot work as a NetworkManager couldn't be found.");
            return;
        }

        networkManager.SceneManager.OnClientLoadedStartScenes += OnClientLoadedStartScenes;
    }

    private void OnDestroy()
    {
        if (networkManager != null)
            networkManager.SceneManager.OnClientLoadedStartScenes -= OnClientLoadedStartScenes;
    }

    private void OnClientLoadedStartScenes(NetworkConnection _, bool asServer)
    {
        if (!asServer)
            return;

        List<NetworkConnection> authenticatedClients = networkManager.ServerManager.Clients.Values
            .Where(conn => conn.IsAuthenticated).ToList();

        if (authenticatedClients.Count < requiredPlayerCount) return;

        foreach (NetworkConnection client in authenticatedClients)
        {
            NetworkObject obj = networkManager.GetPooledInstantiated(playerPrefab, asServer: true);
            networkManager.ServerManager.Spawn(obj, client);

            // If the client isn't observing this scene, make him an observer of it.
            if (!client.Scenes.Contains(gameObject.scene))
                networkManager.SceneManager.AddOwnerToDefaultScene(obj);
        }
    }
}
```
{% endcode %}
{% endstep %}
{% endstepper %}
