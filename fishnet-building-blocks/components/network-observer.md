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

# NetworkObserver

The **NetworkObserver** uses conditions to determine if a client qualifies to be an observer of an object; any number of conditions may be used and a client will be considered an observer of the object if he passes all active conditions. The NetworkObserver component can be used to override the [ObserverManager](managers/observermanager/), or add additional conditions onto the object to which the NetworkObserver is added.

## Conditions <a href="#conditions" id="conditions"></a>

There are [several included conditions](../scriptableobjects/observerconditions/), which may be used together. ObserverCondition may also be inherited from [to make your own conditions](../../guides/features/observers/custom-conditions.md). When all conditions are true, the object will become visible to the client.

Each condition must be created as a scriptable object, and dropped into the Network Observer component.

## Component Settings <a href="#component-settings" id="component-settings"></a>

**Override Type** is used to change how the NetworkObserver component uses the ObserverManager settings. _Add Missing_ will add any conditions from the ObserverManager which are not already on the NetworkObserver. _UseManager_ replaces conditions with those from the manager. _Ignore Manager_ will keep the NetworkObserver conditions, ignoring the ObserverManager entirely.

**Update Host Visibility** will change the visibility of renderers for clientHost when server objects are not visible to the client. If you wish to enable and disable other aspects during a visibility change consider using the NetworkObject.OnHostVisibilityUpdated event.

**Observer Conditions** are which conditions to use.
