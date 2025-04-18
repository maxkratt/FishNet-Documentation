---
description: >-
  Understanding how to use ownership, as well how it affects clients and server
  is essential for any project.
---

# Ownership

{% hint style="warning" %}
This article is being improved and in result will be moved to a new location soon.
{% endhint %}

## Related Articles

* [Terminology](../../general/terminology/)
* [Spawning and Despawning](../spawning/)
* [Ownership](../ownership/)
* [Using Ownership To Read Values](../ownership/using-ownership-to-read-values.md)

## Checking Ownership

Ownership checks can be used a variety of tasks. Here are some very common uses:

* **Checking if a client can control an object, such as move the object:** see Ownership article.
* Seeing who owns an object to check player values, such as name or score: see Using Ownership to Read Values article.
* Making sure only owner calls ServerRpcs.

## Gaining and Transferring Ownership

&#x20;Ownership can be given or removed from clients, or transferred to a new client. Only the server can assign and transfer ownership. Ownership is typically granted when spawning an object; see related articles.&#x20;

When using our included PlayerSpawner script, found on the NetworkManager prefab, ownership is automatically given to client which the 'player object' is being spawned for.

At some point you may wish the client to assume immediate ownership of an object, before the server can transfer ownership. Examples: if you want to control a turret in the scene without delay, or take-over a NetworkTransform object and begin moving it immediately. To do so you can utilize the place the [PredictedOwner](../components/prediction/predictedowner.md) component on the NetworkObject, and utilize it's API. The PredictedOwner component can be inherited to customize it's logic.
