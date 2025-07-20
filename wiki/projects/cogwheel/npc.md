# ⚠️ THIS WIKI PAGE IS DEPERACTED! INFORMATION HERE CAN BE OUTDATED!
# NPC object
NPC object represents npc.

In this article we will list all npc methods avalible.

## setName method
this method allows changing NPC's name.

For example, code below will set name of npc stored in actor varible to `Cool Guy`:
```
actor.setName("Cool Guy")
```

## getName method
this method returns NPC's name as string

## setSkin method
this method allows changing NPC's skin.

For example, code below will set skin of npc stored in actor varible to `denisjava`:
```
actor.setSkin("denisjava")
```

Texture of NPC's skin is taken from `storyanvil_cogwheel:textures/entity/npc/<SKIN NAME>` [resource location](https://minecraft.wiki/w/Resource_location)

## getSkin method
this method returns NPC's skin as string

## chat method
This method makes NPC chat provided string.

For example, code below will make NPC in varible named `actor` say `Hello World!`:
```
actor.chat("Hello World")
```

## pathfind method
This method will make npc go to provided coordinates.

For example, code below will make NPC in varible named `actor` go to `17 66 96` coordinates:
```
actor.pathfind(^17, ^66, ^96)
```

## dialogChoices method
This method allows player to choice some dialog response.

Method returns number of choosen dialog.

For example, code below will prompt players to choose one of reponses:
```
actor.dialogChoices("Who are you?", "Why are you here?", "What time is it?")
```

You can use <a onclick="$story.to('/wiki/wiki.html?p=wiki/projects/cogwheel/switch')">switch</a> statement to reply to players according to their reposnse.