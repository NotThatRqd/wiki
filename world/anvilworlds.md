---
description: >-
  How to load an Anvil world folder using AnvilLoader and more
---

## What is Anvil?

Anvil is the format vanilla Minecraft uses to save worlds. Anvil uses the `.mca` file format (minecraft anvil).
You can read more about Anvil [on the Minecraft (fandom) wiki](https://minecraft.fandom.com/wiki/Anvil_file_format).

## Loading a world using AnvilLoader

To load a world saved using the Anvil file format we can use the built in chunk loader `AnvilLoader` like so,

```java
InstanceContainer#setChunkLoader(new AnvilLoader("worlds/world"));
```

This will load the world inside of the `worlds/world` directory into the InstanceContainer.

In order to load a world, the world folder will only need the `/region` folder, as it contains the block data.

## Saving a world

To save a world we can use the `saveChunksToStorage()` function,
```java
InstanceContainer#saveChunksToStorage();
```
This will only work if you have previously loaded a world into the instance using AnvilLoader.

## Block entities

You may have noticed if you loaded a world using AnvilLoader that signs don't have their text, banners don't
have their patterns, etc. This is because they don't have a BlockHandler specifying their block entity tags.

A [block entity](https://minecraft.fandom.com/wiki/Block_entity) (a.k.a. tile entity) is extra data associated with a
block such as the text on a sign or the pattern of a banner. Let's set up a BlockHandler that will let us see
text on signs,

```java
public class SignHandler implements BlockHandler {
  @Override
  public NamespaceID getNamespaceId() {
    return NamespaceID.from(Key.key("minecraft:sign"));
  }

  @Override
  public Collection<Tag<?>> getBlockEntityTags() {
    ArrayList<Tag<?>> list = new ArrayList<>();
    list.add(Tag.Byte("GlowingText"));
    list.add(Tag.String("Color"));
    list.add(Tag.String("Text1"));
    list.add(Tag.String("Text2"));
    list.add(Tag.String("Text3"));
    list.add(Tag.String("Text4"));
    return list;
  }
}
```

`getBlockEntityTags()` is used to know which tags from a block's block entity data should be sent to the player.
You can find a list of block entity data for every block on the wiki, for example signs can be found
[here](https://minecraft.fandom.com/wiki/Sign#Block_data).

We must also register our BlockHandler like so,

```java
MinecraftServer.getBlockManager().registerHandler("minecraft:sign", () -> new SignHandler());
```

Now if you open your world your signs should appear to have text!
