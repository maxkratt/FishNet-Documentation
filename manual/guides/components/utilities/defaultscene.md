# DefaultScene

This component will automatically change between online and offline scenes depending on server and client connective state. When a connection is started the online scene is loaded, and when disconnected offline is loaded. This component must be placed on or beneath your NetworkManager.

## Component Settings

**Enable Global Scenes** will load the scenes as global when enabled, or as connection when not. For more information on the differences see [Scene Loading](../../scene-management/loading-scenes.md).

**Start In Offline** will load the server and clients into the offline scene when the game starts.

**Offline Scene** is the scene to load when offline.

**Online Scene** is the scene to load when online.

**Replace Scenes** can be set to replace all loaded scenes, or just online scenes with the Online or Offline scene. For more information see loading [Scene Loading - replacing scenes](../../scene-management/loading-scenes.md).

