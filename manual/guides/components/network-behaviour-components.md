# NetworkBehaviour

{% hint style="danger" %}
Adding a NetworkBehaviour derived component at run-time is not yet supported.
{% endhint %}

NetworkBehaviour(s) cannot be added to objects directly. This script must be inherited from before it may be included on an object. Inheriting from NetworkBehaviour allows access to [communications](../../general/terminology/communicating.md) and other useful datas about the network, server, and client.
