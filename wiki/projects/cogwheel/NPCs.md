<fromDevsToDevs></fromDevsToDevs>

# NPC

> NPC is entity added by **StoryAnvil cogwheel[^cogwheel]**

## Spawning NPCs

To spawn npc use _`/summon storyanvil_cogwheel:npc`_ command. There is no npc spawn egg.

## Changing NPCs skin

You can change npc skin by running `/cogwheel npc set skin <SKIN NAME> <NPC ENTITY>`

Skin name is part of resource location[^rl]: `storyanvil_cogwheel:textures/entity/<SKIN NAME>.png`.

After typing this command npc skin should change. If skin's resource location doesn't exists just some white&pink squires would apear.

To add custom skins resource packs[^rp] could be used.

## NPC Actions

**NPC Actions** used to control your npcs.

Actions executed in order they were added.

To add action use:

```
/cogwheel npc queue add <NPC ENTITY> <ACTION DATA>
```

As _<NPC ENTITY>_ use any selector (for example: @e[type=storyanvil_cogwheel:npc,limit=1]) that leads obly to single storyanvil_cogwheel:npc.

As _<ACTION DATA>_ use NBT data (for example: {type:CHAT,msg:"hi!"}). Read below to know how to write this nbt.

Action data nbt must have `type` tag with one of value below.

- CHAT
- GO
- FUN

This value changes what npc will do when this action being exectued.

### CHAT

This action type requires another nbt tag called `msg`.
This tag should contain localization key or just any string that will be sent to chat when action is fired.

#### Examples:

```
/cogwheel npc queue add <NPC ENTITY> {type:CHAT,msg:"Any text here"}
```

---

```
/cogwheel npc queue add <NPC ENTITY> {type:CHAT,msg:"story.storyanvil.npc_greets_player"}
```

### FUN

This action type requires another nbt tag called `fname`.
This tag should contain function resource location that will be exectuted when action if fired.

#### Examples:

```
/cogwheel npc queue add <NPC ENTITY> {type:FUN,fname:"storyanvil:test"}
```

---

Functions being executed by running `/function <FUNCTION NAME PROVIDED IN ACTION NBT>` so its possible to use macroses[^macro]

```
/cogwheel npc queue add <NPC ENTITY> {type:FUN,fname:"storyanvil:test {key_1:"Example String", key_2:10}"}
```

### GO

This action type requires another three nbt tags called `x`, `y` and `z`.
And makes NPC pathfind to specified location when action is fired.

#### Examples:

```
/cogwheel npc queue add <NPC ENTITY> {type:GO,x:10.5,y:82,z:26.5}
```

[^cogwheel]: StoryAnvil cogwheel is mod created by StoryAnvil team. [See more](https://storyanvil.github.io/page?wiki=projects/cogwheel/home)
[^macro]: [See minecraft wiki](<https://minecraft.wiki/w/Function_(Java_Edition)#Macros>)
[^rl]: [See minecraft wiki](https://minecraft.wiki/w/Resource_location)
[^rp]: [See minecraft wiki](https://minecraft.wiki/w/Resource_pack)
