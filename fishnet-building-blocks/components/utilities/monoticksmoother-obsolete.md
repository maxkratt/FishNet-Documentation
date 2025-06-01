---
description: >-
  MonoTickSmoother smooths a child graphical object over frames between each
  network tick. This component can be used on non-networked objects.
---

# MonoTickSmoother \[Obsolete]

The main difference between NetworkTickSmoother and MonoTickSmoother is that MonoTickSmoother does not need to be placed on a networked object, and initializes by default using InstanceFinder; NetworkTickSmoother initializes using NetworkBehaviour callbacks.

<details>

<summary>Settings are general settings related to this component.</summary>

**Use Instance Finder** is whether or not to use the InstanceFinder to find the TimeManager. When this is false you will need to specify which TimeManager to use by calling SetTimeManager. This can be useful in cases where you have multiple NetworkManagers and therefore multiple TimeManagers and you want to specify which one to use.

**Graphical Object** is the object which holds the graphics you want to smooth. The graphics should be a child of the game object with this component.

* **Enable Teleport** will allow the graphical object to teleport to it's actual position – also known as the root position – if the position changes are drastic. Ideally you will not need this setting, but it's an available option should you desire to use it.
  * **Teleport Threshold** is shown while teleporting is enabled. If the graphical object's position is this many units away from the actual position, then the graphical object will teleport to the actual position.

</details>
