# Scene Events

{% embed url="https://www.youtube.com/watch?index=1&list=PLkx8oFug638qBthd3n_F98zAtOdI8o_Gb&v=c_kdZELyJcQ" %}
Scene Events Video Guide
{% endembed %}

## General

There are a variety of events within SceneManager to help with your development. The order in which events invoke are reliable.

This diagram represents which order events can be expected to run. You may also find this diagram within your Fish-Networking import under Example/All/SceneManager.

![SceneManager Event Order](<../../../.gitbook/assets/SceneManager Event Diagram.png>)

{% hint style="info" %}
Notice that client and server process queues and events exactly the same. OnClientPresence change is a special exception, as discussed below.
{% endhint %}

## Events

### OnClientLoadedStartScenes

This event calls every time a client loads the starting scenes for the first time. The start scenes would be any global scenes you have set, or if no global scenes are set then the scene you are entering play into. The PlayerSpawner.cs example demonstrates usage of the **OnClientLoadedStartScenes** event. Included is the connecion, or client which loaded the scenes, and a boolean indicating true if the callback is on the server side or false if on the client side.

The NetworkConnection class also has a similar event **OnLoadedStartScenes** if you'd like to know when only a specific connection has loaded the start scenes.

### OnQueueStart, OnQueueEnd

**OnQueueStart** and **OnQueueEnd** are called when a scene change queue occurs. **Start** will only call if a scene has succesfully begun to load or unload. The queue may process any number of scene events. For example: if a scene is told to unload while a load is still in progress, then the unload will be placed in the queue. **OnQueueEnd** will invoke after both the load and unload have completed.

### OnLoadStart, OnUnloadStart

**OnLoadStart** and **OnUnloadStart** occur before a queue entry is processed. These will call for every entry in a queue. For example: if you call SceneManager.LoadGlobalScenes() twice in a row, **OnQueueStart** will invoke once, and OnLoadStart will invoke twice. **OnLoadStart** will be called when a scene load begins, and **OnUnloadStart** when an unload begins.

Both the Load and Unload events contain a structure which has a field labeled _QueueData._ _QueueData_ holds information about the queue entry. The provided data is exposed for your convenience.

### OnLoadPercentChange

Only available when loading scenes is the **OnLoadPercentChange** event. This event contains information about what percentage of your scenes have loaded for the queue entry. Like the Start events, _QueueData_ is provided. You will also be supplied a float _Percent_, indicating the total progress of your scene load. This can be useful to show loading screens, or perform a variety of initialization tasks.

### OnLoadEnd

The **OnLoadEnd** event is called after all scenes for the queue entry have been loaded. This event will only invoke after the scenes have fully loaded, and after the active scene has been set if applicable. Like the **OnLoadStart** event _QueueData_ is provided. Additionally, _LoadedScenes_ and _SkippedSceneNames. LoadedScenes_ provides Scene references to which scenes were loaded. _SkippedSceneNames_ contains strings of scenes which were not loaded; this generally occurs if the scene is already loaded.

{% hint style="warning" %}
You may want to spawn objects for players once a  scene is loaded, but it's best to do that once the specific client has started observing the scene to prevent race conditions. Thus it's better to use one of the OnClientPresenceChange events or OnSpawnServer from a NetworkBehaviour in the scene.
{% endhint %}

### OnUnloadEnd

After scene unloading has occurred **OnUnloadEnd** is invoked. This event contains _QueueData_, and _UnloadedSceneHandles_. As before _QueueData_ contains information about the queue entry. _UnloadSceneHandles_ is a collection occupied with the handles of which scenes were unloaded.

### OnClientPresenceChangeStart, OnClientPresenceChangeEnd

These two events are only available on the server and indicate when a client is being added to a scene, or being removed from a scene. Both events are only invoked after the client has fully loaded or unloaded the scene. The **Start** variant is invoked before the [observer](../../../fishnet-building-blocks/components/network-observer.md) status has been updated for the client, and **End** after being updated. To clarify, if a client joins a scene **OnClientPresentStart** will call before the client has visibility of any networked content in the scene, and **OnClientPresentEnd** after the client has gained visibility.

Both events contain the same structure which has the following information: _Scene_, _Connection,_ and _Added._ _Scene_ is which scene the client is being added or removed from. _Connection_ is the NetworkConnection for the client. Lastly, _Added_ will be true if being added to the scene, or false is being removed.
