---
description: >-
  The OfflineTickSmoother initializes it's settings using InstanceFinder and can
  be placed beneath objects which are not networked.
---

# OfflineTickSmoother

## Component Settings

<details>

<summary><strong>Misc settings are general settings which do not fit into a category</strong></summary>

**Automatically Initialize** when true will configure this smoother in Awake using InstanceFinder to listen to network callbacks. When false, you must manually call the Initialize method on this component.

</details>

{% hint style="success" %}
Since a non-networked object cannot be owned there is only one set of smoothing settings, compared to NetworkTickSmoother which can have unique settings if Owner or Spectator.
{% endhint %}

The remaining settings and uses on this component are exactly the same as the NetworkTickSmoother. Please review the [NetworkTickSmoother](networkticksmoother.md) documentation for further explanation of usage.
