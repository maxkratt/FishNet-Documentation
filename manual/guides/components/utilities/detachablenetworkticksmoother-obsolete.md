---
description: >-
  This component is primarily used for smooth camera movement between ticks. It
  can be used with prediction or normal design.
---

# DetachableNetworkTickSmoother \[Obsolete]

The DetachableNetworkTickSmooth will detach from parents at runtime and follow the configured follow object. Generally this is placed on an empty object beneath the parent, and you will set your camera target to this.

<figure><img src="../../../../.gitbook/assets/image (21).png" alt=""><figcaption><p>For use with prediction. You may follow NetworkObjectRoot if not using prediction.</p></figcaption></figure>

<details>

<summary>Settings <em>are general settings related to this component.</em></summary>

**Attach On Stop** will re-attach this object to it's previous parent when the NetworkObject for this component has completed network stop callbacks.

</details>

<details>

<summary><strong>Smoothing</strong> <em>decides how this object is smoothed.</em></summary>

**Follow Object** is the object to follow. This is typically the graphical object for the same NetworkObject.

**Interpolation** is how long in ticks to smooth over. Depending on your game type and camera settings, typically 1 or 2 works well.

**Enable Teleport** will allow this object to teleport to Follow Object's position after a distance between tick has passed.

* **Teleport Threshold** is shown while teleporting is enabled. If the graphical object's position is this many units away from the actual position, then the graphical object will teleport to the actual position.

</details>

<details>

<summary>Synchronizing <em>determines which transform properties to update.</em></summary>

**Synchronize Position** when true will move this transforms position towards the follow object's.

**Synchronize Rotation** when true will move this transforms rotation towards the follow object's.

**Synchronize Scale** when true will move this transforms scale towards the follow object's.

</details>
