---
description: There are a variety of ways to send communications between server and clients.
---

# Communicating

Below are several terms you will become familiar with when wanting to send data.

## [SyncTypes](../../features/network-communication/synchronizing/) <a href="#server-and-host" id="server-and-host"></a>

{% tabs %}
{% tab title="Description" %}
SyncTypes reside on objects and are data driven variables or collections. SyncTypes send at adjustable intervals. When a SyncType is modified on an object the changes are automatically sent from the server to clients. Clients will receive the changes locally on the same object. A great example is a health variable. You may update the health variable as a player takes damage, and the new values will be sent to clients.
{% endtab %}

{% tab title="Example" %}
This basic example shows a simple integer variable that will automatically be synchronized to all clients whenever the server changes it.

```csharp
using FishNet.Object;
using FishNet.Object.Synchronizing;

public class Player : NetworkBehaviour
{
    readonly SyncVar<int> _health = new SyncVar<int>(100);

    public void StepOnLego()
    {
        if (!IsServerInitialized)
            return;

        _health. Value -= 10;
    }
}
```
{% endtab %}
{% endtabs %}

## [Remote Procedure Calls](../../features/network-communication/remote-procedure-calls.md) <a href="#server-and-host" id="server-and-host"></a>

{% tabs %}
{% tab title="Description" %}
[Remote Procedure Calls](#user-content-fn-1)[^1] are another object bound communication type. While SyncTypes are used to synchronize variables, RPCs allow you to run logic on server and clients. RPCs are not limited to an interval like states, RPCs can be sent immediately on the next network tick or as soon as possible. They are required to be within a NetworkBehaviour class.
{% endtab %}

{% tab title="Example" %}
This is a simple example that shows a [Server Remote Procedure Call](#user-content-fn-2)[^2]. The method should be called from a client and the body will not execute locally, instead, the arguments will be sent across the network and the method body with those arguments will run remotely on the server.

```csharp
using FishNet.Object;

public class Player : NetworkBehaviour
{
    [ServerRpc]
    void UpdatePlayerName(string newName)
    {
        print($"Player {OwnerId} has updated their name to {newname}");
    }
}
```
{% endtab %}
{% endtabs %}

## [Broadcasts](../../features/network-communication/broadcasts.md) <a href="#server-and-host" id="server-and-host"></a>

{% tabs %}
{% tab title="Description" %}
Unlike states, broadcasts are not object bound. Broadcasts can be used for any number of tasks but are more commonly preferred for communicating data groups between server and clients. Broadcasts can be received and sent from anywhere in your code and do not need to be within a NetworkBehaviour class as RPCs do.
{% endtab %}

{% tab title="Example" %}
This example shows how a client could send a broadcast to the server.

```csharp
using FishNet;
using UnityEngine;

public class ChatSystem : MonoBehaviour
{
    public void SendChatMessage(string text)
    {
        ChatBroadcast msg = new ChatBroadcast()
        {
            Message = text,
            FontColor = Color.white
        };

        InstanceFinder.ClientManager.Broadcast(msg);
    }
}
```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
For all of the above communication methods, two channels are supported, Reliable and Unreliable. Data sent reliably is guaranteed to arrive and be processed in the order it was sent. Data sent unreliably uses less bandwidth, but can occasionally arrive out of order, or not at all.
{% endhint %}

{% hint style="info" %}
Some features of Fish-Networking internally use eventual consistency; an example of this are unreliable SyncVars or the NetworkTransform component. These features use the unreliable channel to send datas to consume less bandwidth and provide better performance, but you can still use them knowing that even if data is dropped or arrives out of order the features will eventually resolve the desynchronizations automatically.
{% endhint %}

[^1]: Often called RPCs

[^2]: These are commonly called ServerRPCs
