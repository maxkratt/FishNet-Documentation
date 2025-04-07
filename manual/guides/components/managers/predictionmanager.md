---
description: >-
  PredictionManager provides states, callbacks, and settings to fine tuning
  prediction for your game type.
---

# PredictionManager

## Component Settings

<details>

<summary>Client <em>are settings which only affect the client.</em></summary>

**Client Interpolation** is how many states to try and hold in a buffer before running them on clients. Larger values add resilience against network issues at the cost of running states later.

</details>

<details>

<summary>Server <em>are settings which only affect the server.</em></summary>

**Server Interpolation** is how many states to try and hold in a buffer before running them on the server. Larger values add resilience against network issues at the cost of running states later.

**Drop Excessive Replicates** will discard replicate datas received from clients and server after the cached count exceeds a certain value. When false multiple datas will be consumed per tick to clear the cache quicker.

* **Maximum Server Replicates** is shown when the above value is true. This value indicates the maximum number of replicates which the server will cache from client before dropping old ones. Generally, cached replicates will never significantly exceed **Queued Inputs**.

</details>
