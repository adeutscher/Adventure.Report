# Spawning

## Portals

Portals were the first objects defined in the World database to be manifested into the game world, so they for now set the standard on how such resources should be structured.

1. Each map is outfitted with a "entry point" prefab object containing a `ZoneEntryPoint` component.
2. The `LoadMapGameEvent` is the kick-off event sent when the `ZoneEntryPoint.Awake` method on the server is about to finish.
3. Any number of different loaders should subscribe to `LoadMapGameEvent`. Since these loaders need to poll the World API and could involve delays, they should run in the background in order be handled by an `async` method.
4. The loaders pull information on each instance of a Portal for the current map from the world API.
5. For each Portal, send a `SpawnPortalGameEvent`
6. A `SpawnPortalGameEvent` is handled by a `PortalSpawner`. In order to have knowledge of which scene it's working in, the `PortalSpawner` is a component that is part of the entry point prefab. Since spawning a portal involves instantiating objects in the scene, this handler must be run in the foreground.

![image](./diagrams/png/20-portal-spawning.png)