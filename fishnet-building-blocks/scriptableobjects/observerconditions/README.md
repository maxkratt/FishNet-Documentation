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

# ObserverConditions

**ObserverConditions** in Fish-Networking define specific conditions under which clients receive updates about a [NetworkObject's](../../../guides/features/networked-gameobjects-and-scripts/networkobjects.md) state. These conditions help optimize network traffic by ensuring that only relevant updates are transmitted, as well as prevent client's from cheating by only sending relevant data to individual clients.

<figure><img src="../../../.gitbook/assets/aoi-system.svg" alt=""><figcaption><p>Example of a player only being able to observe objects withing a specific distance</p></figcaption></figure>
