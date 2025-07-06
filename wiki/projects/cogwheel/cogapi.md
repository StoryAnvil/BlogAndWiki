# Cogwheel API
Cogwheel API is bunch of ready to use commands you can use in your CogScript code.

## log method
Reference:
```
log(<log message>)
```

Log method can be used to write text in minecraft logs.

For example following code will type `Hello from logs!` in minecraft logs:
```
log(Hello from logs!)
```

## findTaggedNPC method
Reference:
```
findTaggedNPC(<varible name>=<npc tag>)
```

findTaggedNPC method allows searching for NPCs by tag. You can give NPC tag using [/tag command](https://minecraft.wiki/w/Commands/tag). After runnign this method will save tagged NPC in provided varible.

For example following code will put NPC with tag `npc1` in `actor` varible:
```
findTaggedNPC(actor=npc1)
```

## getLevel method
Reference:
```
getLevel(<varible name>)
```

getLevel method allows getting minecraft world in varible

For example following code will put minecraft world in varible named `level`:
```
getLevel(level)
```

## chat method
Reference:
```
chat(<varible name>:<message>)
```

chat method allows sending messages to chat. Specified varible must contain a StoryChatter.
StoryChatter is any NPC or minecraft world.

For example following code will send message `Hello, World!` as minecraft world:
```
getLevel(level)
chat(level:Hello, World!)
```
And following code will send message `I am NPC` as NPC with tag `npc1`:
```
findTaggedNPC(a=npc1)
chat(a:I am NPC)
```

When message sent by NPC name of that NPC will be added to start of the message.

## name method
Reference:
```
name(<varible name>:<name>)
```

name method allows setting name of any StoryNameHolder.
Currently StoryNameHolder is any NPC.

For example following code will set name of NPC tagged `npc1` to `Cool Guy` then make it send chat message:
```
findTaggedNPC(a=npc1)
name(a:Cool Guy)
chat(a:I am NPC)
```

## skin method
Reference:
```
skin(<varible name>:<skin name>)
```

name method allows setting skin of any StorySkinHolder.
Skin texture resource loaction is `storyanvil_cogwheel:textures/entity/npc/<skin name>`
Currently StorySkinHolder is any NPC.

For example following code will set skin texture of NPC tagged `npc1` to `storyanvil_cogwheel:textures/entity/npc/denisjava`:
```
findTaggedNPC(a=npc1)
skin(a:denisjava)
```

## pathfind method
Reference:
```
pathfind(<varible name>:<x> <y> <z>)
```

pathfind method makes provided StoryNavigator go to specified location.
Currently StoryNavigator is any NPC.

For example following code will make NPC tagged `npc1` go to 18 65 91 coordinates:
```
findTaggedNPC(a=npc1)
pathfind(a:18 65 91)
```
