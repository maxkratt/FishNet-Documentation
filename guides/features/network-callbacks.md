---
description: >-
  You can take advantage of numerous available events to stay informed about the
  current state of the network.
---

# Network State Events

{% hint style="info" %}
This is not a comprehensive list of available events, but merely some of the most commonly used ones. To view all the available ones check out the [API pages](https://fish-networking.com/FishNet/api/api/index.html).
{% endhint %}

## Server Side Events

### [**OnAuthenticationResult**](https://fish-networking.com/FishNet/api/api/FishNet.Managing.Server.ServerManager.html#FishNet_Managing_Server_ServerManager_OnAuthenticationResult)

| **Name:**       | <mark style="color:green;">`ServerManager.OnAuthenticationResult`</mark>                                                                                                               |
| --------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Namespace:**  | <mark style="color:blue;">`FishNet.Managing.Server`</mark>                                                                                                                             |
| **Parameters:** | [<mark style="color:orange;">NetworkConnection</mark>](https://fish-networking.com/FishNet/api/api/FishNet.Connection.NetworkConnection.html)<mark style="color:orange;">, bool</mark> |

This event is called once a client has either been authenticated or failed to authenticate. The parameters are the relevant [NetworkConnection](server-and-client-identification/networkconnections.md), and a Boolean representing if that client successfully authenticated or not. This event can be very useful to use to store the NetworkConnection of a client that has successfully connected to the server or to send some message to the newly connected client.

### [**OnServerConnectionState**](https://fish-networking.com/FishNet/api/api/FishNet.Managing.Server.ServerManager.html#FishNet_Managing_Server_ServerManager_OnServerConnectionState)

| **Name:**       | <mark style="color:green;">`ServerManager.OnServerConnectionState`</mark>                                                                                       |
| --------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Namespace:**  | <mark style="color:blue;">`FishNet.Managing.Server`</mark>                                                                                                      |
| **Parameters:** | [<mark style="color:orange;">ServerConnectionStateArgs</mark>](https://fish-networking.com/FishNet/api/api/FishNet.Transporting.ServerConnectionStateArgs.html) |

This event is called when the server's [state ](#user-content-fn-1)[^1]changes. In other words, immediately when the server starts or stops running. This event is very useful for performing network actions on the server such as loading initial scenes or spawning initial objects.

### [**OnRemoteConnectionState**](https://fish-networking.com/FishNet/api/api/FishNet.Managing.Server.ServerManager.html#FishNet_Managing_Server_ServerManager_OnRemoteConnectionState)

| **Name:**       | <mark style="color:green;">`ServerManager.OnRemoteConnectionState`</mark>                                                                                                                                                                                                                                                                         |
| --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Namespace:**  | <mark style="color:blue;">`FishNet.Managing.Server`</mark>                                                                                                                                                                                                                                                                                        |
| **Parameters:** | [<mark style="color:orange;">NetworkConnection</mark>](https://fish-networking.com/FishNet/api/api/FishNet.Connection.NetworkConnection.html)<mark style="color:orange;">,</mark> [<mark style="color:orange;">RemoteConnectionStateArgs</mark>](https://fish-networking.com/FishNet/api/api/FishNet.Transporting.RemoteConnectionStateArgs.html) |

This event is called when a client's [state ](#user-content-fn-1)[^1]changes with the server, in other words, immediately when a client connects or disconnects from the server. This event is very useful for detecting when a client has disconnected as well as for authentication scripts to handle sending data or checking if the server is full before authenticating a connection.

***

## Client Side Events

### [**OnAuthenticated**](https://fish-networking.com/FishNet/api/api/FishNet.Managing.Client.ClientManager.html#FishNet_Managing_Client_ClientManager_OnAuthenticated)

| **Name:**      | <mark style="color:green;">`ClientManager.OnAuthenticated`</mark> |
| -------------- | ----------------------------------------------------------------- |
| **Namespace:** | <mark style="color:blue;">`FishNet.Managing.Client`</mark>        |

This event is called when the local client has successfully been authenticated with the FishNet server. The client will now have a Client ID and be added to the [ServerManager.Clients](https://fish-networking.com/FishNet/api/api/FishNet.Managing.Server.ServerManager.html#FishNet_Managing_Server_ServerManager_Clients) and [ClientManager.Clients](https://fish-networking.com/FishNet/api/api/FishNet.Managing.Client.ClientManager.html#FishNet_Managing_Client_ClientManager_Clients) collections. This is a good event to use to know when your client is fully connected to the server and ready to play the game.

### [**OnClientConnectionState**](https://fish-networking.com/FishNet/api/api/FishNet.Managing.Client.ClientManager.html#FishNet_Managing_Client_ClientManager_OnClientConnectionState)

| **Name:**       | <mark style="color:green;">`ClientManager.OnClientConnectionState`</mark>                                                                                       |
| --------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Namespace:**  | <mark style="color:blue;">`FishNet.Managing.Client`</mark>                                                                                                      |
| **Parameters:** | [<mark style="color:orange;">ClientConnectionStateArgs</mark>](https://fish-networking.com/FishNet/api/api/FishNet.Transporting.ClientConnectionStateArgs.html) |

This event is called when the local client's [state ](#user-content-fn-1)[^1]changes. In other words, immediately when the client makes or contact with the Fish-Networking server or is disconnected from it. This event is useful for detecting when you are disconnected from the server and when you initially connect. Authentication scripts will often use this event to send initial data to the server so that the server can authenticate the client.

{% hint style="warning" %}
When this event is invoked the client hasn't yet been added to the [ClientManager.Clients](https://fish-networking.com/FishNet/api/api/FishNet.Managing.Client.ClientManager.html#FishNet_Managing_Client_ClientManager_Clients) collection.
{% endhint %}

### [**OnRemoteConnectionState**](https://fish-networking.com/FishNet/api/api/FishNet.Managing.Client.ClientManager.html#FishNet_Managing_Client_ClientManager_OnRemoteConnectionState)

| **Name:**       | <mark style="color:green;">`ClientManager.OnRemoteConnectionState`</mark>                                                                                       |
| --------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Namespace:**  | <mark style="color:blue;">`FishNet.Managing.Client`</mark>                                                                                                      |
| **Parameters:** | [<mark style="color:orange;">RemoteConnectionStateArgs</mark>](https://fish-networking.com/FishNet/api/api/FishNet.Transporting.RemoteConnectionStateArgs.html) |

This event is called when a remote[^2] client's [state ](#user-content-fn-1)[^1]changes. This event is useful for detecting when another player has connected or disconnected from the game.

{% hint style="info" %}
This is only available when using [ServerManager.ShareIds](https://fish-networking.com/FishNet/api/api/FishNet.Managing.Server.ServerManager.html#FishNet_Managing_Server_ServerManager_ShareIds).
{% endhint %}

***

## Shared Events

### [**OnClientLoadedStartScenes**](https://fish-networking.com/FishNet/api/api/FishNet.Managing.Scened.SceneManager.html#FishNet_Managing_Scened_SceneManager_OnClientLoadedStartScenes)

| **Name:**       | <mark style="color:green;">`SceneManager.OnClientLoadedStartScenes`</mark>                                                                                                                                                |
| --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Namespace:**  | <mark style="color:blue;">`FishNet.Managing.Scened`</mark>                                                                                                                                                                |
| **Parameters:** | [<mark style="color:orange;">NetworkConnection</mark>](https://fish-networking.com/FishNet/api/api/FishNet.Connection.NetworkConnection.html)<mark style="color:orange;">,</mark> <mark style="color:orange;">bool</mark> |

Called when a client loads the initial scenes after connecting, for example any global networked scenes. \
Since this event will be called on the client who loaded the start scenes as well as the server, there is available a Boolean parameter which will be true if the event was called as the server. This can be used on the host player to prevent logic in this method from running twice. This will invoke even if the SceneManager is not used when the client completes fully connecting to the server.

### [**OnPreTick**](https://fish-networking.com/FishNet/api/api/FishNet.Managing.Timing.TimeManager.html#FishNet_Managing_Timing_TimeManager_OnPreTick)

| **Name:**      | <mark style="color:green;">`TimeManager.OnPreTick`</mark>  |
| -------------- | ---------------------------------------------------------- |
| **Namespace:** | <mark style="color:blue;">`FishNet.Managing.Timing`</mark> |

This event is called right before a network tick occurs, as well as before data is read.

### [**OnTick**](https://fish-networking.com/FishNet/api/api/FishNet.Managing.Timing.TimeManager.html#FishNet_Managing_Timing_TimeManager_OnTick)

| **Name:**      | <mark style="color:green;">`TimeManager.OnTick`</mark>     |
| -------------- | ---------------------------------------------------------- |
| **Namespace:** | <mark style="color:blue;">`FishNet.Managing.Timing`</mark> |

This event is called when a network tick occurs. This can be useful for sending data to the server at a set rate to prevent sending data too frequently and flooding the connection.

### [**OnPostTick**](https://fish-networking.com/FishNet/api/api/FishNet.Managing.Timing.TimeManager.html#FishNet_Managing_Timing_TimeManager_OnPostTick)

| **Name:**      | <mark style="color:green;">`TimeManager.OnPostTick`</mark> |
| -------------- | ---------------------------------------------------------- |
| **Namespace:** | <mark style="color:blue;">`FishNet.Managing.Timing`</mark> |

This event is called just after a network tick occurs; physics would have already been simulated if using [PhysicsMode.TimeManager](../../fishnet-building-blocks/components/managers/time-manager.md).

[^1]: The available states are **Started** and **Stopped**

[^2]: This means some client other than the local client itself.
