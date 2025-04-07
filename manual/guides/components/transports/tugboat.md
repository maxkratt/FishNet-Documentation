---
coverY: 0
layout:
  cover:
    visible: false
    size: full
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# Tugboat

## General

Tugboat uses LiteNetLib to support reliable and unreliable messages. This is the default transport for Fish-Networking.

***

## Compatibility <a href="#server-and-host" id="server-and-host"></a>

<table data-full-width="false"><thead><tr><th width="149">System</th><th width="198">Supported? </th></tr></thead><tbody><tr><td>Windows</td><td><mark style="color:green;">Fully Supported</mark></td></tr><tr><td>MacOS</td><td><mark style="color:green;">Fully Supported</mark></td></tr><tr><td>IOS</td><td><mark style="color:green;">Fully Supported</mark></td></tr><tr><td>Android</td><td><mark style="color:green;">Fully Supported</mark></td></tr><tr><td>Linux</td><td><mark style="color:green;">Fully Supported</mark></td></tr><tr><td>Xbox</td><td><mark style="color:green;">Fully Supported</mark></td></tr><tr><td>PlayStation</td><td><mark style="color:green;">Fully Supported</mark></td></tr><tr><td>Nintendo</td><td><mark style="color:green;">Fully Supported</mark></td></tr></tbody></table>

***

## How to Install

Tugboat is the default transport of FishNet! Which means it comes with FishNet by default when Importing FishNet from your favorite source.

The [Transport Manager](../managers/transportmanager/) will automatically make this the default transport at runtime, if you did not add any other Transport to the Network Manager Game Object.

***

## Component Settings <a href="#server-and-host" id="server-and-host"></a>

<details>

<summary>Settings <em>are general settings for Tugboat.</em></summary>

**Dont Route** forces sockets to send data directly to the network adapter interface without routing through other services such as routers. This is often only needed when working with multiple network adapters.

</details>

<details>

<summary>Channels <em>are settings related to channels, such as reliable and unreliable.</em></summary>

**Unreliable MTU** is the largest size an unreliable packet may be. When a single outbound data exceeds this value it is sent reliably. Smaller unreliable datas will be automatically split over multiple unreliable sends.

</details>

<details>

<summary>Server <em>are settings used by the server.</em></summary>

**IPv4 Bind Address** is which address to bind the server to. If set and IPv4 is not available this will cause a socket error upon server start.

**Enable IPv6** enables IPv6 binding when available. In some cases you may want this disabled if you have an IPv6 interface you do not want used.

* **IPv6 Bind Address** is which address to bind the server to. If set and IPv6 is not available this will cause a socket error upon server start.

**Port** is which port the server will listen on. This is also the port clients will connect to. In some instances you may need to change this at runtime using TransportManager.SetPort().

**Maximum Clients** is the maximum active clients allowed before the transport begins to deny connections.

</details>

<details>

<summary>Client <em>are settings used by the client.</em></summary>

**Client Address** is which address to connect to. This is typically your server address. Localhost is set as default for local testing.

</details>
