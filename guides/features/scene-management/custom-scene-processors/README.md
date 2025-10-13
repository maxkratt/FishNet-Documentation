---
description: >-
  You can create a custom scene processor to handle how a scene is
  loaded/unloaded.
---

# Custom Scene Processors

FishNet's SceneManager uses Unity's methods under the hood, you can override its functionality to add or change how it functions. This can be useful when using addressables or for making custom loading screens.

To start, simply create a script that inherits from the [DefaultSceneProcessor](https://fish-networking.com/FishNet/api/api/FishNet.Managing.Scened.DefaultSceneProcessor.html), add it to your network manager object, and then in the [SceneManager](../../../../fishnet-building-blocks/components/managers/scenemanager.md) component on the network manager, assign it as the [Scene Processor](../../../../fishnet-building-blocks/components/managers/scenemanager.md#scene-processor).

{% hint style="success" %}
You are encouraged to check out its full API [here](https://fish-networking.com/FishNet/api/api/FishNet.Managing.Scened.DefaultSceneProcessor.html).
{% endhint %}

### Addressables

You will need to make a custom scene processor if you want your game to make use of addressable networked scenes. Follow [this guide](addressables.md) to learn a little more.

### Loading Screen

You can use a custom scene processor to easily hook up your loading screen into FishNet's networked scene loading. There is a complete tutorial for that [here](../../../../tutorials/simple/making-a-loading-screen.md).
