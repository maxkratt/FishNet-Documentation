# Upgrading To Fish-Networking

Moving to Fish-Networking will vary depending on which previous networking solution you are coming from. With other solutions changing their API so regularly it may be difficult to keep transition information up to date but we will try our best.&#x20;

If you encounter any questions we welcome you on our Discord at [https://discord.gg/NqzSEqR](https://discord.gg/NqzSEqR), where our helpful community can get you started.

## Leaving Mirror Networking

More information will be added as it becomes available.&#x20;

{% hint style="warning" %}
Always backup before making large changes. Having another copy of your project to reference during the upgrade is a great idea!
{% endhint %}

{% hint style="info" %}
Majority of these changes can be done by using quick replace within your IDE!
{% endhint %}

**Mirror example conversion video:**

[**https://www.youtube.com/watch?v=ZhEMh5RjEjs**](https://www.youtube.com/watch?v=ZhEMh5RjEjs)

**Upgrade tool:**

An upgrade tool is provided to help transition from Mirror to Fish-Net. The tool will replace several Mirror components with the like ones for Fish-Net. Note that while the components will be replaced the settings of your components will not be preserved. For best results keep a copy of your original project for reference until switching over.

Currently supported components are:

* NetworkIdentity
* NetworkProximityChecker
* NetworkTransform/Child
* NetworkAnimator
* NetworkSceneChecker

Additional components from FirstGearGames Mirror Assets/Projects are supported:

* FlexNetworkTransform/Child
* FastProximityChecker
* FlexNetworkAnimator
* FlexSceneChecker

{% hint style="info" %}
The upgrade tool requires scripting defines to function. These defines should be removed after running the upgrade tool.

MIRROR - for Mirror components.

FGG\_ASSETS - for FirstGearGames assets.

FGG\_PROJECTS - for FirstGearGames projects.
{% endhint %}

Before running the upgrade tool you will likely need to remove a few files. If using FirstGearGames Mirror assets or projects, delete any of the demo folders included for those assets and projects.

In addition, any scripts requiring component NetworkIdentity will need to have the declaring line removed.

```csharp
//Replace the following with an empty string.
[RequireComponent(typeof(NetworkIdentity))]

//The hotkey in Visual Studio is Ctrl + Shift + H.
```

**Component changes:**

| Mirror              | Fish-Networking                                                                                                                                                         | Notes                              |
| ------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------- |
| Interest Management | <p><a href="../guides/components/managers/observermanager/">ObserverManager</a><br>see also: <a href="../guides/components/network-observer.md">NetworkObserver</a></p> | _Both support multiple conditions_ |
| NetworkIdentity     | NetworkObject                                                                                                                                                           |                                    |

{% hint style="info" %}
Changing out components can be a tedious task. Save time by using Window -> Fish-Networking -> Upgrading -> From Mirror -> Replace Components. This will swap out most components for you.

Mirror and Fish-Networking must be within your project to run the operation mentioned. Afterwards you may remove Mirror.
{% endhint %}

**NetworkBehaviour property renames:**

{% hint style="warning" %}
Each IsServer, IsClient, etc option has two varieties: Initialized and Started.

Initialized means the object has been marked as ready for network use by the side (eg: client or server). Started means the client or server socket is connected, but does not mean the object has been initialized for network use yet.

When converting from Mirror you will most likely want to use Initialized.
{% endhint %}

| Mirror             | Fish-Networking         | Notes                                         |
| ------------------ | ----------------------- | --------------------------------------------- |
| hasAuthority       | IsOwner                 | _isLocalPlayer removed, use IsOwner instead._ |
| isClient           | IsClientInitialized     |                                               |
| isClientOnly       | IsClientOnlyInitialized |                                               |
| isServer           | IsServerInitialized     |                                               |
| isServerOnly       | IsServerOnlyInitialized |                                               |
| connectionToClient | Owner                   | _Can be used on client._                      |
| connectionToServer | LocalConnection         |                                               |
| netId              | ObjectId                |                                               |

**NetworkBehaviour callbacks:** callbacks such as OnStartClient for the most part are the same in Fish-Networking. Some of the callbacks do provide additional information which is not available within Mirror's callbacks. There are also several new callbacks to use. For a complete guide on using Fish-Networking callbacks click [here](https://fish-networking.gitbook.io/docs/manual/guides/network-behaviour-guides#callbacks).

**Remote Procedure Call renames:** see [Remote Procedure Calls](../guides/remote-procedure-calls.md) for enhanced features and usage related to RPCs.

| Mirror       | Fish-Networking | Notes                                         |
| ------------ | --------------- | --------------------------------------------- |
| \[Command]   | \[ServerRpc]    |                                               |
| \[TargetRpc] | \[TargetRpc]    | _First parameter must be a NetworkConnection_ |
| \[ClientRpc] | \[ObserversRpc] |                                               |

{% hint style="info" %}
RPCs within Fish-Networking has many advantages over Mirror; see [Remote Procedure Calls](../guides/remote-procedure-calls.md) for more information.
{% endhint %}

Mirror uses several singletons while Fish-Networking uses an anti-singleton design. There are still easy ways to access similar features. All of the same access can be returned from the NetworkObject component, as well by using [InstanceFinder](../guides/instancefinder-guides.md).

**Manager renames:** all of these may also be found within the NetworkManager.

| Mirror         | Fish-Networking  | Notes                   |
| -------------- | ---------------- | ----------------------- |
| NetworkManager | NetworkManager   | _Unchanged._            |
| NetworkServer  | ServerManager    |                         |
| NetworkClient  | ClientManager    |                         |
|                | TransportManager | _Fish-Networking only._ |
|                | TimeManager      | _Fish-Networking only._ |
|                | SceneManager     | _Fish-Networking only._ |

**Switching from Mirror.NetworkManager:** the NetworkManager in Mirror features several overrides for state changes, such as when clients connect, disconnect, server start or stop, scene changes, and more.&#x20;

Fish-Networking instead uses sealed managers with events. To mimic the NetworkManager of Mirror create a new MonoBehaviour and place it on your NetworkManager object. Instead of using overrides subscribe to your needed events from each appropriate Fish-Networking manager.

```
//Mirror example.
//Called when a remote client connects.
public override OnServerConnect(NetworkConnectionToClient conn) {}

//FishNet example.
//Called when a remote client state changes.
networkManager.ServerManager.OnRemoteConnectionState += YourCallback;
```



| Mirror                                                                                                                                                         | Fish-Networking                                                                                                                                                                |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| <p>ServerChangeScene </p><p>OnServerChangeScene </p><p>OnServerSceneChanged </p><p>ClientChangeScene</p><p>OnClientChangeScene </p><p>OnClientSceneChanged</p> | <p>SceneManager.OnLoadEnd</p><p>SceneManager.OnClientPresenceChangeEnd</p><p></p><p>See <a href="../guides/scene-management/">SceneManager</a> guide for more information.</p> |
| OnServerReady                                                                                                                                                  | SceneManager.OnClientLoadedStartScenes                                                                                                                                         |
| <p>OnServerConnect</p><p>OnServerDisconnect</p>                                                                                                                | Servermanager.OnRemoteConnectionState                                                                                                                                          |
| <p>OnServerAddPlayer</p><p>OnClientNotReady</p><p>OnStartHost</p><p>OnStopHost</p>                                                                             | Not needed.                                                                                                                                                                    |
| <p>OnStartServer</p><p>OnStopServer</p><p>OnServerError</p>                                                                                                    | ServerManager.OnServerConnectionState                                                                                                                                          |
| <p>OnStartClient</p><p>OnStopClient</p><p>OnClientConnect</p><p>OnClientDisconnect</p><p>OnClientError</p>                                                     | ClientManager.OnClientConnectionState                                                                                                                                          |
