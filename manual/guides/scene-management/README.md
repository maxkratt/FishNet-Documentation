---
description: >-
  Fish-Networking comes with a powerful scene manager tool that enables you to
  synchronize networked scenes with minimal effort.
---

# Scene Management

## General

The [**SceneManager**](../components/managers/scenemanager.md) Component has many scene features to support your multiplayer needs. The links below will take you to the different guides for each feature that the [**SceneManager**](../components/managers/scenemanager.md) has to offer.

Visit the [**API**](https://firstgeargames.com/FishNet/api/api/FishNet.Managing.Scened.SceneManager.html) to see the public items exposed to the user in the SceneManager.

## Sub Pages

### [Scene Events](scene-events.md)

"Scene Events" are the Invoked Events that happen along the loading and unloading method.

### [Scene Data](scene-data/)

The Data Classes that are needed for the the various features to function.

### [Loading Scenes](loading-scenes.md)

Information on how to "load" scenes and the options available to the user while loading.

### [Unloading Scenes](unloading-scenes.md)

Information on how to "unload" scenes and the options available to the user while Unloading.

### [Scene Stacking](scene-stacking.md)

"Scene Stacking" is the ability for server or host to load multiple instances of a scene at once, usually with different observers in each scene.

### [Scene Caching](scene-caching.md)

"Scene Caching" is the ability for the Server to keep a scene loaded when either all clients have unloaded that scene, or stopped observing that scene.

### [Scene Visibility](scene-visibility.md)

"Scene Visibility" guide offers details of using the "Scene Condition" with the [**ObserverManager**](../components/managers/observermanager/), and how to manage Observers in a Scene.

### [Persisting NetworkObjects](persisting-networkobjects.md)

"Persisting NetworkObjects" is the ability to keep a network objects state when loading and unloading scenes.

### [Custom Scene Processors](custom-scene-processors/)

Fishnet has the ability for users to create their own Custom Scene Processor for loading and unloading scenes. Using Addressables for example, would need a Custom Scene Processor created.

