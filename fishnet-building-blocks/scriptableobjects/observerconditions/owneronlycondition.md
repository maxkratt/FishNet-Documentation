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

# OwnerOnlyCondition

The **Owner Only Condition** evaluates to true when a player owns the object. An owner condition will make an object only visible to its owner. If there is no owner, the object will not be visible to any clients. This can be very useful for sharing information with only the owner client, for example inventory data or a player's hand of cards, which shouldn't be shared with other clients to prevent cheating and wasted bandwidth.

<figure><img src="../../../.gitbook/assets/owner-only-observer-condition.png" alt=""><figcaption></figcaption></figure>

## Component Settings <a href="#server-and-host" id="server-and-host"></a>

> **Add Order** controls the order in which this observer condition will be evaluated on an object.
>
> This can be very useful when having observer conditions that are more computationally complex than others, as it allows you to choose the order in which they will be evaluated. Timed conditions are always evaluated after non-timed conditions.

> **Is Constant** is used to declare whether the condition's settings or data  will remain unchanged at runtime. Its purpose is to optimize performance by avoiding unnecessary updates or recalculations for conditions that do not change during execution. It is currently not implemented, but is available for future use and can already be set.
