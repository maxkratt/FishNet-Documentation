---
description: >-
  Step-by-step instructions for installing Fish-Networking into your Unity
  project.
---

# Installing Fish-Networking

{% hint style="info" %}
If after importing the package you get a message asking to automatically update the API, select either one of the **Yes** options.
{% endhint %}

### The Unity Asset Store

Installing via the Unity Asset Store is the easiest method and ensures you get a stable release:

1. Open your Unity project.
2. Go to **Window > Asset Store** or visit the [Unity Asset Store online](https://assetstore.unity.com/).
3. Search for **FishNet**.
4. Click **Add to My Assets** and then **Open in Unity**.
5. In Unity, go to **Package Manager** (`Window > Package Manager`).
6. Select **My Assets**, find **FishNet**, and click **Download** and then **Import**.

{% hint style="success" %}
&#x20;**Recommended for most users** who want a stable release without version control setup.
{% endhint %}

***

### Unity Package File

Use this method if you have a `.unitypackage` file, for example, downloaded from [GitHub Releases](https://github.com/FirstGearGames/FishNet/releases), from the [Fish-Networking website](https://www.fish-networking.com/), or shared by a team member.

1. Download the FishNet `.unitypackage` file.
2. In Unity, go to **Assets > Import Package > Custom Package…**
3. Select the downloaded `.unitypackage` file.
4. Click **Import** to bring all the FishNet files into your project.

{% hint style="info" %}
**Best for** offline installs or specific package versions not yet on the Asset Store.
{% endhint %}

***

### Git URL (via Unity Package Manager)

This method keeps FishNet up to date via a Git repository. Ideal if you want the latest updates or to contribute to development.

1. Open your Unity project.
2. Go to **Window > Package Manager**.
3. Click the **+** button in the top-left corner.
4. Select **Add package from Git URL…**
5.  Enter the following URL:

    ```
    https://github.com/FirstGearGames/FishNet.git?path=Assets/FishNet
    ```
6. Click **Add** to install.

{% hint style="warning" %}
Ensure you have Git installed on your system and added to your system path.
{% endhint %}

***

### Fix for Errors After Importing

If you receive the following errors after importing FishNet, then make sure you don't have Netcode for GameObjects inside your project. The files it uses will clash with FishNet's files and you will need to remove it first and then import FishNet again.

```
Assets\FishNet\CodeGenerating\ILCore\ILCoreHelper.cs(18,13): error CS0246: The type or namespace name 'PostProcessorAssemblyResolver' could not be found (are you missing a using directive or an assembly reference?)
Assets\FishNet\CodeGenerating\ILCore\ILCoreHelper.cs(24,50): error CS0246: The type or namespace name 'PostProcessorReflectionImporterProvider' could not be found (are you missing a using directive or an assembly reference?)
```
