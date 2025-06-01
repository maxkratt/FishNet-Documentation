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

Multipass is a pass-through transport that allows multiple transports to run on the server at once. For example, you may want to run Tugboat for your desktop or mobile users, and Bayou for your web users. With Multipass a single server build can run both, joining players from both together in the same game.

For usage see the [guide](../../guides/features/transports/multipass.md#setup).

***

## Compatibility

<table data-full-width="false"><thead><tr><th width="149">System</th><th width="198">Supported?</th></tr></thead><tbody><tr><td>All Platforms</td><td><mark style="color:green;">Fully Supported</mark></td></tr></tbody></table>

***

## How to Install

Multipass comes with FishNet by default! No extra downloads needed.\
\
Just add Multipass to your [Transport Manager](../components/managers/transportmanager/) and follow the usage guide mentioned in General above.

***

## Component Settings

* **Global Server Actions: W**hile true will perform server actions such as start or stop server, on all transports.
* **Transports:** A collection of transports you wish Multipass to support.
