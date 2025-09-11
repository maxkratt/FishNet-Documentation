# Modifying Conditions

Several conditions may be modified at run-time. What can be modified for each condition may vary. I encourage you to view the [API](https://fish-networking.com/FishNet/api/api/FishNet.Component.Observing.html) to see what each condition exposes.

To change properties on a condition you must access the condition through the [NetworkObserver](../../../fishnet-building-blocks/components/network-observer.md) component.

```csharp
// Below is an example of modifying the distance requirement
// on a DistanceCondition. This line of code can be called from
// any NetworkBehaviour. You may also use nbReference.NetworkObserver...
base.NetworkObserver.GetObserverCondition<DistanceCondition>().MaximumDistance = 10f;
```

All conditions can be enabled or disabled. When a condition is disabled it's requirements are ignored, as if the condition does not exist. This can be useful for temporarily disabling condition requirements on objects.

```csharp
// The OwnerOnlyCondition is returned and disabled.
// This allows all players to see the object, rather than just the owner.
ObserverCondition c = base.NetworkObject.NetworkObserver.GetObserverCondition<OwnerOnlyCondition>();
c.SetIsEnabled(false);
// Even though we are returning ObserverCondition type, it could be casted to
// OwnerOnlyCondition.
```
