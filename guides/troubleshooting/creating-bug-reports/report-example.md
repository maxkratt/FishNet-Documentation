---
description: This is an example of what a created bug report might look like.
---

# Report Example

Below is a bug report for a SyncVar which is not sending to clients. You do not need to include headers as demonstrated; they are only shown here so it's clear what information is being displayed.

{% hint style="info" %}
If troubleshooting was not performed or applicable fields are missing in the bug report there is a fair chance the submitter would be asked to update their issue, and until then the issue would be suspended.
{% endhint %}

> **Unity version:**
>
> * Unity 2021.3.10f.
>
> **FishNet Version:**
>
> * FishNet 4.3.1
>
> **Discord Troubleshot Link/What I've tried:**
>
> * https://discord.com/channels/123456
>
> **Description:**
>
> When I set a SyncVar on the server non-host clients do not get the update. Callbacks are not working either.
>
> **Replication:**
>
> This can be reproduced in two editors.
>
> * Start 1st editor as clientHost or server.
> * In a player NetworkBehaviour script update an int SyncVar in Awake, wrapped in InstanceFinder.IsServerStarted checks.
> * Add
> * Start 2nd editor as client only.
> * Have editor 1 spawn the player object for 2nd editor.
>
> **Expectation:**
>
> * Both the 1st and 2nd editor get callbacks.
> * The SyncVar value should synchronize to the 2nd editor.

At a glance this would suggest everything is working as intended. The server is not active on the 2nd editor (client only) so the InstanceFinder.IsServerStarted check is failing, and the value is never applied locally. The 1st editor is not sending the value because SyncTypes set in Awake are assumed applied in server and client since Awake always fires regardless of network state.

Although this is covered in the [SyncType documentation ](../../features/network-communication/synchronizing/)the behavior could definitely be somewhat of a gotcha. If it's possible the developers troubleshooting steps wouldn't have caught this gotcha, we would likely give the answer above and ask the developer to close the issue if it were now resolved.
