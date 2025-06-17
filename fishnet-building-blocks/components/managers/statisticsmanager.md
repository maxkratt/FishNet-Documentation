---
description: >-
  The StatisticManager provides statics about Fish-Networking for a variety of
  tasks, including monitoring network traffic.
---

# StatisticsManager

## Description <a href="#server-and-host" id="server-and-host"></a>

The **Statistics Manager** component is responsible for tracking and reporting network traffic statistics for your multiplayer game. Its **Network Traffic** can be used to gain basic information about how much network traffic your game is using. These values must be enabled for the [BandwidthDisplay](../utilities/bandwidthdisplay.md) component to function.

{% hint style="success" %}
Check out its API page for more specific methods and events [here](https://firstgeargames.com/FishNet/api/api/FishNet.Managing.Statistic.StatisticsManager.html).
{% endhint %}

## Settings <a href="#server-and-host" id="server-and-host"></a>

<div align="left"><figure><img src="../../../.gitbook/assets/statistics-manager-component.png" alt=""><figcaption><p>Default Settings</p></figcaption></figure></div>

### :gear:  **Update Interval**

> This is how often network traffic related operations occur, such as invoking update events.

### :gear:  **Update Client**

> This will invoke client traffic updates when enabled.

### :gear:  **Update Server**

> This will invoke server traffic updates when enabled.
