---
description: >-
  SceneLookupData is how the server determines to load clients into a new
  instance of a Scene, or load a client into a scene that the server already has
  loaded.
---

# SceneLookupData

## General

{% hint style="warning" %}
When Creating [**SceneLoadData**](sceneloaddata.md) or [**SceneUnloadData**](sceneunloaddata.md) there are provided constructors that automatically create SceneLookupData for you. Most likely you will be using these constructors and will not be creating a separate SceneLookupData.
{% endhint %}

When you specify a scene by reference, or handle, the SceneManager will prefer to lookup that scene using the scene handle. This is important information when [**Scene Stacking**](../scene-stacking.md). Looking up a scene by handle will place connections in the scene specified, but when using scene names, the server will create a new scene instance for the specified connections and place them into that scene. The described behavior only applies when loading scenes over multiple Load calls; such as if you call LoadConnectionScene twice, each with it's own connection.

## Default Values

```csharp
        //SceneLookupData Default values
        SceneLookupData slud = new SceneLookupData()
        { 
            //If Handle is greater than 0 then it will ignore Name and use Handle
            //to look up the scene.
            Handle = 0,
            //If Handle is set to 0, then Name is used to lookup the scene instead.
            Name = null 
        }; 
```

See[ **Loading Scenes**](../loading-scenes/) and [**Unloading Scenes**](../unloading-scenes.md) for implementation.
