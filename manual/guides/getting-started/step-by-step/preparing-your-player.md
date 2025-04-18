---
description: Spawn an object over the network to represent each client's player.
---

# Preparing Your Player

**GUIDE TODO — show images**

* Make sure dev is not in play mode.
* Make a capsule scene object.
* Add NetworkObject component. Explain networkObject is required to make an object usable over the network. Networkbehaviours will be covered later, do not mention them yet.&#x20;
* Add two spawn point transforms on the map, and add them to the PlayerSpawner.
* Explain that if spawn points are not specified, the PlayerSpawner will place them using the prefab transform properties.&#x20;
* Place prefab on player spawner.
* Press play on editor and see player spawn. Optionally start a second editor or build and see two players spawn, one for each client. Inform dev they might see 'server port already in use error', explain why, and that it can be safely ignored.
