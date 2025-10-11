---
description: Using the DefaultScene component to automatically manage simple scene setups.
---

# Automatic Online and Offline Scenes

If you're looking for a simple way to manage scene transitions when your network starts or stops, FishNet has an easy way to manage it without any custom code. The [**DefaultScene**](../../../../fishnet-building-blocks/components/utilities/defaultscene.md) component is a ready-made solution that automatically loads your designated **online scene** when the network starts, and your **offline scene** when the network shuts down.

This is perfect for developers who want a plug-and-play setup without writing custom scene logic.

### Setup instructions

To get started:

1. Add the `DefaultScene` component to your **NetworkManager** GameObject.
2. In the Inspector, assign your **Offline Scene** and **Online Scene**.
3. Make sure the two scenes are different — using the same scene for both will prevent the component from functioning properly.

{% hint style="success" %}
Read more about the component here: [defaultscene.md](../../../../fishnet-building-blocks/components/utilities/defaultscene.md "mention")
{% endhint %}
