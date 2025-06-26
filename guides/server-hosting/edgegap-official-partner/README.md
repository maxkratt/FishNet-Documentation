---
cover: ../../../.gitbook/assets/edgegap-cover.png
coverY: 0
---

# Other Hosting Options

In addition to PlayFlow there are other hosting options available. Here are a few that we are aware of.

{% hint style="warning" %}
We strongly recommend reviewing our [Server Hosting](../) page for terminology as well the Pros/Cons of services mentioned on this page.
{% endhint %}

This information is updated to the best of our knowledge. If you are interested in a service listed below — to better understand their platform — please visit the provided links to their official website.

## Dedicated Servers

These hosts offer session-based servers, as well persistent servers. Relay services may be available as well.

[Hathora](https://hathora.dev/)

* Pay-per-use pricing.
* Session-based servers available.
* Persistent servers available.
* Relay servers available.
* Match Making / Lobbies.

[Edgegap](https://edgegap.com/)

* Pay-per-use pricing.
* Session-based servers available.
* Persistent servers available.
* Relay servers available.
* Match Making / Lobbies.

[Amazon Web Services (AWS)](https://aws.amazon.com/)

AWS is a bit more difficult to setup and manage and is primarily recommended to experienced developers.

* Pay-per-use pricing — a little more complex.
* Session-based servers available.
* Persistent servers available.
* Relay servers available.
* Match Making / Lobbies.

We have a community guide for AWS [here](getting-started-with-aws.md)!

## Relays Only

These are relay only services.

[Epic Online Services (EOS)](https://onlineservices.epicgames.com/en-US/services)

EOS provides a number of features for free. The API and documentation is a bit more complex, which may be difficult to beginners. A vetted third-party [EOS transport](../../../fishnet-building-blocks/transports/fishyeos-epic-online-services.md) is available for FishNet, which may help with some of those hurdles.

A cross-platform friends list is possible with EOS but at the cost of complexity; eg: you will not be able to directly access your Steam friends via the EOS platform. With this in mind, match making with friends may be more difficult.

* Free to all developers.
* Session-based relays.
* \*Match Making / Lobbies.
* \*Friends list.

[Steam Relay](https://partner.steamgames.com/doc/features/multiplayer/steamdatagramrelay)

Steam relays are offered free when you launch your game on steam; there is a percentage of sales, as well initial fee when using Steam.

We have an official FishNet transport for Steam, and there are third-party Unity assets available as well to help utilize Steam Relays. A popular asset is [Toolkit for Steamworks Foundation by Heathen Engineering](https://github.com/heathen-engineering/Toolkit-for-Steamworks-Foundation).

* Free for games launched using Steam.
* Session-based relays.
* Match Making / Lobbies.
* Friends list (Steam).
