---
layout:
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

# Multipass

## General

Multipass is a pass-through transport that allows multiple transports to run on the server at once. For example, you may want to run Tugboat for your desktop or mobile users, and Bayou for your web users. With Multipass a single server build can run both, joining players from both together in the same game.&#x20;

For usage see the [guide](../../transports/multipass.md#setup).

***

## Compatibility

<table data-full-width="false"><thead><tr><th width="149">System</th><th width="198">Supported? </th></tr></thead><tbody><tr><td>Windows</td><td><mark style="color:green;">Fully Supported</mark></td></tr><tr><td>MacOS</td><td><mark style="color:green;">Fully Supported</mark></td></tr><tr><td>IOS</td><td><mark style="color:green;">Fully Supported</mark></td></tr><tr><td>Android</td><td><mark style="color:green;">Fully Supported</mark></td></tr><tr><td>Linux</td><td><mark style="color:green;">Fully Supported</mark></td></tr><tr><td>Xbox</td><td><mark style="color:green;">Fully Supported</mark></td></tr><tr><td>PlayStation</td><td><mark style="color:green;">Fully Supported</mark></td></tr><tr><td>Nintendo</td><td><mark style="color:green;">Fully Supported</mark></td></tr></tbody></table>

***

## How to Install

Multipass comes with FishNet by default! No extra downloads needed.\
\
Just add Multipass to your [Transport Manager](../managers/transportmanager/) and follow the usage guide mentioned in General above.

***

## Component Settings

* **Global Server Actions: W**hile true will perform server actions such as start or stop server, on all transports.
* **Transports:** A collection of transports you wish Multipass to support.&#x20;
