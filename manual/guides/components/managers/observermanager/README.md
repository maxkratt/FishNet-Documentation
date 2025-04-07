---
description: >-
  ObserverManager assists in controlling what network objects each client may
  see.
---

# ObserverManager

ObserverManager is used to globally customize the observer system. Observer conditions within the ObserverManager will be automatically added to NetworkObjects, unless the [NetworkObserver](../../network-observer.md) component is set to ignore the manager.

## Component Settings <a href="#server-and-host" id="server-and-host"></a>

<details>

<summary>Settings <em>are general settings related to the ObserverManager.</em></summary>

**Default Conditions** are conditions which will be added to all NetworkObjects by default.

**Update Host Visibility** will hide renderers on networked objects which are hidden to clientHost. When true all networked objects will be visible to the clientHost even if these objects would normally be despawned for the client.

</details>
