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

# GridCondition

The **Grid Condition** is similar to the [DistanceCondition](distancecondition.md) and evaluates to true if clients are within specified distance of the object. The Grid Condition is however less accurate but more performant; it is also a [Timed Condition](#user-content-fn-1)[^1]. This condition requires you to place a [HashGrid ](../../components/managers/observermanager/hashgrid.md)on or beneath your [NetworkManager](../../components/managers/network-manager.md) object.

<figure><img src="../../../.gitbook/assets/grid-observer-condition.png" alt=""><figcaption></figcaption></figure>

## Component Settings <a href="#server-and-host" id="server-and-host"></a>

> **Add Order** controls the order in which this observer condition will be evaluated on an object.
>
> This can be very useful when having observer conditions that are more computationally complex than others, as it allows you to choose the order in which they will be evaluated. Timed conditions are always evaluated after non-timed conditions.

> **Is Constant** is used to declare whether the condition's settings or data  will remain unchanged at runtime. Its purpose is to optimize performance by avoiding unnecessary updates or recalculations for conditions that do not change during execution. It is currently not implemented, but is available for future use and can already be set.

[^1]: A Timed Condition is evaluated periodically rather than only when certain conditions change.
