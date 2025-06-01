---
layout:
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# PlayerSpawner

The **Player Spawner** component can be used to spawn a game object for a client once they connect to the game.&#x20;

<figure><img src="../../.gitbook/assets/player-spawner-component.png" alt=""><figcaption></figcaption></figure>

## Component Settings <a href="#server-and-host" id="server-and-host"></a>

> **Player Prefab** is used to select the prefab this component will instantiate for clients when they connect to the server.

> **Add to Default Scene** is used to add a client to the active scene when no global scenes are specified through the [FishNet SceneManager](../../guides/features/scene-management/). This is important because FishNet doesn't force a client to start in the same scene as the server, so FishNet needs to be told if a client should observe a specific scene and receive information about the Network Objects in that scene.

> **Spawns** is a list of GameObject Transforms that the **PlayerSpawner** will use as locations to spawn the players at. It will use them in order, one after another, looping once it reaches the final one. If none are given, then it will use the prefab's base position.
