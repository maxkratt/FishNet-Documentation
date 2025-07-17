---
description: >-
  Overview of key server hosting terms, comparing session-based vs persistent
  architectures, and outlining the roles of relays and dedicated servers.
---

# Terminology

### Session-based versus Persistent

A session-based service, be it relay or dedicated server, will start up and end as needed. Given that these servers are on-demand(session-based) you will often pay-per-use rather than a flat rate, which can financially save you a substantial amount. Session-based services are the most common way online games are hosted and are typically used for matches and lobby based gaming, but can also be used for entire worlds with hundreds of players.

Persistent services are typically always running, no matter the number of players present. Given the servers are always present, this makes it easier to keep state of your worlds. A persistent server is the common choice for MMO-like games. While a constant world is very appealing these server types often come with an increased monetary cost.

### Dedicated Servers

Dedicated servers is a common term which implies a server authoritative game server. There are several advantages to a dedicated server, and very few cons.

**Benefits of a dedicated server:**

* **Scalability**: servers are rarely limited to a set number of players. If you need worlds which support a dozens or more players generally a dedicated server is the recommended route.
* **Security**: a dedicated server is the only way to effectively prevent player cheating, as the server becomes your game's authority and allows you to validate player information.
* **Bandwidth**: data transfer is rarely limited with dedicated servers. Relays may limit how much total data you can send, or throttle how frequently you can send data.
* **Latency:** for games which require a fast response time a dedicated server is mandatory. These servers can deploy across the world, providing the best latency and most neutrality for players.
* **Persistence**: if you want players to be able to join and leave servers, be them session-based or persistent, a dedicated server will provide a smoother experience. A player host is not required for a dedicated server.

**Drawbacks of a dedicated server:**

* Can be more expensive if a relay service is available to you free as part of where you launched your game, or other means.

### Relays

A relay is a client authoritative service, where the client simulates as the server.

**Benefits of a relay:**

* Ideal for casual games when a free relay service is available. Impractical if you have to pay for the relay service.

**Drawbacks of a relay:**

* **Scalability**: relays cannot scale. You are often limited to a number of players per session.
* **Security**: whichever player is acting as the server, is the authoritative client. Because there is not a neutral authority, cheating is very possible and often easily performed on relay servers. If cheating is a concern, a relay is likely not for you.
* **Bandwidth**: total used bandwidth is rarely restricted — or at least not to a means you will encounter. However, there are often limitations on how much data can be regularly sent. Games which need to send data rapidly, or send large chunks of data, may find themselves being throttled by the relay; this can create any number of issues.
* **Latency**: the client acting as the server is typically the determining factor for latency. A host with a weak connection will result in all players having increased latency. Players further from the host will also experience more latency.
* **Persistence**: there is no state persistence with relays — when the host ends the session everything is lost. In addition, a leaving host will often immediately terminate the game session for all connected players.
