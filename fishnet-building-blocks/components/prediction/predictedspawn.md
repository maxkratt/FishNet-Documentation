# PredictedSpawn

Adding this component to a NetworkObject will allow you to adjust predicted spawning settings for the object. To enable this feature you must also enable predicted spawning within the [ServerManager](../managers/server-manager.md).

The PredictedSpawn component offers several virtual methods to override to customize and validate predicted spawns.

## Component Settings

**Allow Spawning** allows clients to predicted spawn the object.

**Allow Despawning** allows clients to predicted despawn the object.

{% hint style="info" %}
You can implement WritePayload and ReadPayload in your classes which inherit NetworkBehaviour to send data with spawn messages. This can even be used for predicted spawns to the server, and clients!
{% endhint %}
