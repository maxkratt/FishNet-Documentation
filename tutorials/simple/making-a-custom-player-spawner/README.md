---
description: >-
  Step-by-step instructions for how to write a custom player spawner for various
  scenarios.
---

# Making a Custom Player Spawner

In many games, you'll often need to spawn a **GameObject** to represent a player. This can happen at various points during gameplay—such as when a player first connects, once a specific number of players have joined, after a particular scene finishes loading, or when the host initiates it with a button press.&#x20;

FishNet includes a default [PlayerSpawner](../../../fishnet-building-blocks/components/playerspawner.md) component designed primarily as a quick-start tool and reference example. It automatically spawns a player object for each client upon successful connection and authentication. It also has the option to define specific player spawn points, and if multiple spawn points are provided, the component will rotate through them sequentially.

If this functionality is sufficient for your game, then you can go ahead and use the default one, if not, let's look at how to implement our own to handle different use cases.

<table data-card-size="large" data-view="cards"><thead><tr><th></th><th></th><th></th><th data-type="rating" data-max="3"></th><th data-hidden data-card-target data-type="content-ref"></th><th data-hidden></th></tr></thead><tbody><tr><td><strong>Spawning Players When Connected</strong></td><td>The default PlayerSpawner handles this for you. So there's nothing more to do besides adding it to your game and assigning a prefab to it.</td><td>Difficulty</td><td>1</td><td><a href="../../../fishnet-building-blocks/components/playerspawner.md">playerspawner.md</a></td><td>Difficulty</td></tr><tr><td><strong>Spawning Players Manually</strong></td><td>Tutorial for creating a script to manually spawn your players when you call a method.</td><td>Difficulty</td><td>2</td><td><a href="manually.md">manually.md</a></td><td>Difficulty</td></tr><tr><td><strong>Spawning Players when Set Number of Players Joined</strong></td><td>Tutorial for spawning players as soon as a set number of clients have joined your game.</td><td>Difficulty</td><td>3</td><td><a href="set-player-number.md">set-player-number.md</a></td><td></td></tr><tr><td><strong>Spawning Players on Scene Load</strong></td><td>Tutorial for spawning players as soon as they are loaded by FishNet into a specific scene.</td><td>Difficulty</td><td>3</td><td><a href="on-scene-load.md">on-scene-load.md</a></td><td>Difficulty</td></tr></tbody></table>
