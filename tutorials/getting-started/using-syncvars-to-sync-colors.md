---
description: Synchronizing color with synchronized variables!
---

# Using SyncVars to Sync Colors

We've got quite a few things synchronized now, but how might you go about synchronizing a specific variable in one of your scripts? SyncVars[^1] are one answer! A [SyncVar](../../guides/features/network-communication/synchronizing/syncvar.md) is a FishNet generic type that can be used within [NetworkBehaviours](../../guides/features/networked-gameobjects-and-scripts/network-behaviour-guides.md) that automatically synchronize their value from the server to all clients.

Let's liven up the colors in our game and synchronize them with SyncVars.

{% stepper %}
{% step %}
### **Creating a script to sync color**

Create a new script called `SyncMaterialColor`. This will be used to synchronize the **Material** color of our **MeshRenderers**.

{% code title="SyncMaterialColor.cs" lineNumbers="true" %}
```csharp
using FishNet.Object;
using FishNet.Object.Synchronizing;
using UnityEngine;

[RequireComponent(typeof(MeshRenderer))]
public class SyncMaterialColor : NetworkBehaviour
{
    public readonly SyncVar<Color> Color = new SyncVar<Color>();

    private void Awake()
    {
        Color.OnChange += OnColorChanged;
    }

    private void OnColorChanged(Color previous, Color next, bool asServer)
    {
        GetComponent<MeshRenderer>().material.color = Color.Value;
    }
}
```
{% endcode %}

This simple script has a `Color` variable of the type `SyncVar<Color>`. It needs be set to `readonly`, but don't worry, we can still get and set its `Value`.

{% hint style="info" %}
Setting your **SyncVar** as `readonly` will cause it to be hidden from the Unity Inspector. You can read about a work-around for this [here](../../guides/features/network-communication/synchronizing/customizing-behavior.md).
{% endhint %}

In `Awake` we subscribe to the SyncVar's `OnChange` event. This event is very useful as it will be invoked as soon as the SyncVar changes, even on a client when the server changes it and it's synced.

`OnColorChanged` is our method that we subscribed to the OnChange event. This method simply gets the MeshRenderer and sets its material color to the SyncVar's Color Value, thus updating the visuals to match on all devices.

{% hint style="success" %}
You may want to read more about **SyncVars** and the other available **SyncTypes** that FishNet has, such as **SyncLists.** You can find those pages [here](../../guides/features/network-communication/synchronizing/).
{% endhint %}
{% endstep %}

{% step %}
### **Add the script component**

Now add your newly created script to your **Cube Prefab**. The script won't currently do anything unless we change the `Color` **SyncVar** in it, so let's do that next.

<figure><img src="../../.gitbook/assets/cube-with-sync-color.png" alt=""><figcaption><p>The Sync Material Color component added</p></figcaption></figure>
{% endstep %}

{% step %}
### **Give the cubes random colors**

Let's give the cubes some color now as soon as we instantiate them.

Reopen the `PlayerCubeCreator.cs` script and make the following addition after the call to `Instantiate` and before the call to `Spawn`:

```csharp
obj.GetComponent<SyncMaterialColor>().Color.Value = Random.ColorHSV();
```

Here is the complete script with the change made:

{% tabs %}
{% tab title="Old Input System" %}
<pre class="language-csharp" data-title="PlayerCubeCreator.cs" data-line-numbers><code class="lang-csharp">using FishNet.Object;
using UnityEngine;

public class PlayerCubeCreator : NetworkBehaviour
{
    public NetworkObject CubePrefab;

    void Update()
    {
        // Only the local player object should perform these actions.
        if (!IsOwner)
            return;

        if (Input.GetButtonDown("Fire1"))
            SpawnCube();
    }

    // We are using a ServerRpc here because the Server needs to do all network object spawning.
    [ServerRpc]
    private void SpawnCube()
    {
        NetworkObject obj = Instantiate(CubePrefab, transform.position, Quaternion.identity);
<strong>        obj.GetComponent&#x3C;SyncMaterialColor>().Color.Value = Random.ColorHSV();
</strong>        Spawn(obj); // NetworkBehaviour shortcut for ServerManager.Spawn(obj);
    }
}
</code></pre>
{% endtab %}

{% tab title="New Input System" %}
<pre class="language-csharp" data-title="PlayerCubeCreator.cs" data-line-numbers><code class="lang-csharp">using FishNet.Object;
using UnityEngine;
using UnityEngine.InputSystem;

public class PlayerCubeCreator : NetworkBehaviour
{
    public NetworkObject CubePrefab;

    public override void OnStartClient()
    {
        if (IsOwner)
            GetComponent&#x3C;PlayerInput>().enabled = true;
    }

    public void OnAttack(InputValue value)
    {
        if (value.isPressed)
            SpawnCube();
    }

    // We are using a ServerRpc here because the Server needs to do all network object spawning.
    [ServerRpc]
    private void SpawnCube()
    {
        NetworkObject obj = Instantiate(CubePrefab, transform.position, Quaternion.identity);
<strong>        obj.GetComponent&#x3C;SyncMaterialColor>().Color.Value = Random.ColorHSV();
</strong>        Spawn(obj); // NetworkBehaviour shortcut for ServerManager.Spawn(obj);
    }
}
</code></pre>
{% endtab %}
{% endtabs %}

This line of code gets the **SyncMaterialColor** component and sets the SyncVar Value to a random color as soon as it's instantiated. Once Spawn is called on the next line, FishNet will spawn this object for all clients with the color SyncVar state synced.
{% endstep %}

{% step %}
### **Test the synchronized cubes**

Now all you need to do is run your game again and see if the cubes spawn with a random color and if that color is synchronized across the network.

<figure><img src="../../.gitbook/assets/synced-cube-colors.gif" alt=""><figcaption><p>Cube color synchronized!</p></figcaption></figure>
{% endstep %}
{% endstepper %}

{% hint style="info" %}
Download the project files with these completed steps here, or explore the repository:

<a href="https://github.com/maxkratt/fish-networking-getting-started/releases/download/using-syncvars-to-sync-colors/using-syncvars-to-sync-colors.unitypackage" class="button primary" data-icon="down-to-line">Source Files</a> <a href="https://github.com/maxkratt/fish-networking-getting-started/tree/syncing-colors" class="button secondary" data-icon="github">Repository</a>
{% endhint %}

[^1]: synchronized variables
