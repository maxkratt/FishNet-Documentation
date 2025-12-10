---
description: How to setup a player camera for your multiplayer setup.
---

# Setting Up a Camera

When it comes to multiplayer, a player's camera setup can seem quite tricky to understand. In reality it's often quite simple, only one[^1] camera should exist for your local player.&#x20;

The most reliable method is to create a camera prefab and instantiate it locally when the player's character spawns. Alternatively, you can place a camera in the scene ahead of time and have the player’s object "claim" it upon spawning.

Things get a bit more nuanced when working with [Cinemachine](https://unity.com/features/cinemachine). We'll briefly explore how to manage this setup effectively.

<table data-view="cards"><thead><tr><th></th><th data-hidden data-card-target data-type="content-ref"></th><th data-hidden data-card-cover data-type="files"></th></tr></thead><tbody><tr><td>Instantiating a Local Camera</td><td><a href="instantiating-a-local-camera.md">instantiating-a-local-camera.md</a></td><td><a href="../../../.gitbook/assets/instantiated-camera-card.png">instantiated-camera-card.png</a></td></tr><tr><td>Using the Scene Camera</td><td><a href="using-the-scene-camera.md">using-the-scene-camera.md</a></td><td><a href="../../../.gitbook/assets/scene-camera-card.png">scene-camera-card.png</a></td></tr><tr><td>Basic Setup with Cinemachine</td><td><a href="basic-setup-with-cinemachine.md">basic-setup-with-cinemachine.md</a></td><td><a href="../../../.gitbook/assets/cinemachine-camera-card.png">cinemachine-camera-card.png</a></td></tr></tbody></table>

[^1]: This is excluding cameras used for minimaps, rear view mirrors, etc.
