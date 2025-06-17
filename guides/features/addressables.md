---
description: How to manage prefab and scene addressables with FishNet.
---

# Addressables

Both addressable scenes and prefabs work over the network.

## Scene Addressables

Scene addressables utilize Fish-Networking's [Custom Scene Processors](scene-management/custom-scene-processors/). With a few overrides you can implement addressable scenes using [this guide](scene-management/custom-scene-processors/addressables.md).

## Prefab Addressables

To work reliably each addressables package must have a unique ushort Id, between 1 and 65535. **Never** use 0 as the Id, as the preconfigured SpawnablePrefabs use this Id. You may assign your addressable Ids however you like, for instance using a dictionary that tracks your addressable names with Ids.

Registering addressable prefabs with Fish-Networking is easy once each package has been given an Id.

The code below shows one way of loading and unloading addressable prefabs for the network.

```csharp
/// <summary>
/// Reference to your NetworkManager.
/// </summary>
private NetworkManager _networkManager => InstanceFinder.NetworkManager;
/// <summary>
/// Used to load and unload addressables in async.
/// </summary>
private AsyncOperationHandle<IList<GameObject>> _asyncHandle;

/// <summary>
/// Loads an addressables package by string.
/// You can load whichever way you prefer, this is merely an example.
/// </summary>
public IEnumerator LoadAddressables(string addressablesPackage)
{
    /* FishNet uses an Id to identify addressable packages
     * over the network. You can set the Id to whatever, however
     * you like. A very simple way however is to use our GetStableHash
     * helper methods to return a unique key for the package name.
     * This does require the package names to be unique. */
    ushort id = addressablesPackage.GetStableHash16();

    /* GetPrefabObjects will return the prefab
     * collection to use for Id. Passing in true
     * will create the collection if needed. */
    SinglePrefabObjects spawnablePrefabs = (SinglePrefabObjects)_networkManager.GetPrefabObjects<SinglePrefabObjects>(id, true);

    /* Get a cache to store networkObject references in from our helper object pool.
     * This is not required, you can make a new list if you like. But if you
     * prefer to prevent allocations FishNet has the really helpful CollectionCaches
     * and ObjectCaches, as well Resettable versions of each. */
    List<NetworkObject> cache = CollectionCaches<NetworkObject>.RetrieveList();

    /* Load addressables normally. If the object is a NetworkObject prefab
     * then add it to our cache! */
    _asyncHandle = Addressables.LoadAssetsAsync<GameObject>(addressablesPackage, addressable =>
    {
        NetworkObject nob = addressable.GetComponent<NetworkObject>();
        if (nob != null)
            cache.Add(nob);
    });
    yield return _asyncHandle;
    
    /* Add the cached references to spawnablePrefabs. You could skip
     * caching entirely and just add them as they are read in our LoadAssetsAsync loop
     * but this saves more performance by adding them all at once. */
    spawnablePrefabs.AddObjects(cache);

    //Optionally(obviously, do it) store the collection cache for use later. We really don't like garbage!
    CollectionCaches<NetworkObject>.Store(cache);
}

/// <summary>
/// Loads an addressables package by string.
/// </summary>
public void UnoadAddressables(string addressablesPackage)
{
    //Get the Id the same was as we did for loading.
    ushort id = addressablesPackage.GetStableHash16();

    /* Once again get the prefab collection for the Id and
     * clear it so that there are no references of the objects
     * in memory. */
    SinglePrefabObjects spawnablePrefabs = (SinglePrefabObjects)_networkManager.GetPrefabObjects<SinglePrefabObjects>(id, true);
    spawnablePrefabs.Clear();
    //You may now release your addressables!
    Addressables.Release(_asyncHandle);
}
```

{% hint style="warning" %}
When adding addressables on your client be sure to do so before the server will send spawn messages for them.
{% endhint %}
