---
description: >-
  How to load an Anvil world folder using AnvilLoader and more
---

## What is Anvil?

Anvil is the format vanilla Minecraft uses to save worlds. Anvil uses the `.mca` file format (minecraft anvil).
You can read more about Anvil [on the wiki](https://minecraft.fandom.com/wiki/Anvil_file_format).

## Loading a world using AnvilLoader

To load a world saved using the Anvil file format we can use the built in chunk loader `AnvilLoader` like so,

```java
InstanceContainer#setChunkLoader(new AnvilLoader("worlds/world"));
```

This will load the world inside of the `worlds/world` directory into the InstanceContainer.

In order to load a world, the world folder will only need the `/region` folder, as it contains the block data.
Note that tile entities (signs, chests, anything that isn't just a plain block) will not load with their
custom information (text, contents, etc.). This will be addressed below.

## Saving a world

To save a world we can use the `saveChunksToStorage()` function,
```java
InstanceContainer#saveChunksToStorage();
```
This will only work if you have previously loaded a world into the instance using AnvilLoader.
