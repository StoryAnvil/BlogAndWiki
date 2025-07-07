# Cogwheel API
Cogwheel API is bunch of ready to use commands you can use in your CogScript code.

## log method
Reference:
```
Cogwheel.log(<log message>)
```

Log method can be used to write text in minecraft logs.

For example following code will type `Hello from logs!` in minecraft logs:
```
Cogwheel.log(Hello from logs!)
```

## findTaggedNPC method
Reference:
```
Cogwheel.findTaggedNPC(<varible name>=<npc tag>)
```

findTaggedNPC method allows searching for NPCs by tag. You can give NPC tag using [/tag command](https://minecraft.wiki/w/Commands/tag). After runnign this method will save tagged NPC in provided varible.

For example following code will put NPC with tag `npc1` in `actor` varible:
```
Cogwheel.findTaggedNPC(actor=npc1)
```

## getLevel method
Reference:
```
Cogwheel.getLevel(<varible name>)
```

getLevel method allows getting minecraft world in varible

For example following code will put minecraft world in varible named `level`:
```
Cogwheel.getLevel(level)
```

## chat method
Reference:
```
Cogwheel.chat(<varible name>:<message>)
```

chat method allows sending messages to chat. Specified varible must contain a StoryChatter.
StoryChatter is any NPC or minecraft world.

For example following code will send message `Hello, World!` as minecraft world:
```
Cogwheel.getLevel(level)
Cogwheel.chat(level:Hello, World!)
```
And following code will send message `I am NPC` as NPC with tag `npc1`:
```
Cogwheel.findTaggedNPC(a=npc1)
Cogwheel.chat(a:I am NPC)
```

When message sent by NPC name of that NPC will be added to start of the message.

## name method
Reference:
```
Cogwheel.name(<varible name>:<name>)
```

name method allows setting name of any StoryNameHolder.
Currently StoryNameHolder is any NPC.

For example following code will set name of NPC tagged `npc1` to `Cool Guy` then make it send chat message:
```
Cogwheel.findTaggedNPC(a=npc1)
Cogwheel.name(a:Cool Guy)
Cogwheel.chat(a:I am NPC)
```

## skin method
Reference:
```
Cogwheel.skin(<varible name>:<skin name>)
```

name method allows setting skin of any StorySkinHolder.
Skin texture resource loaction is `storyanvil_cogwheel:textures/entity/npc/<skin name>`
Currently StorySkinHolder is any NPC.

For example following code will set skin texture of NPC tagged `npc1` to `storyanvil_cogwheel:textures/entity/npc/denisjava`:
```
Cogwheel.findTaggedNPC(a=npc1)
Cogwheel.skin(a:denisjava)
```

## pathfind method
Reference:
```
Cogwheel.pathfind(<varible name>:<x> <y> <z>)
```

pathfind method makes provided StoryNavigator go to specified location.
Currently StoryNavigator is any NPC.

For example following code will make NPC tagged `npc1` go to 18 65 91 coordinates:
```
Cogwheel.findTaggedNPC(a=npc1)
Cogwheel.pathfind(a:18 65 91)
```

## wait method
Reference:
```
Cogwheel.wait(<varible name>:<label name>)
```
Varible must store any StoryActionQueue which is minecraft world or any NPC. This command makes StoryActionQueue wait for command with provided label to end.

For example:
```
Cogwheel.findTaggedNPC(a=npc1)
Cogwheel.findTaggedNPC(b=npc2)

path:::Cogwheel.pathfind(a:15 24 -4)
Cogwheel.wait(b:path)
Cogwheel.chat(b:Npc1 finished walking!)
```

Code above will make npc tagged `npc1` pathfind and go to `15 24 -4` coordinates.
**AFTER** npc1 finished walking npc2 will chat `Npc1 finished walking!`

## suspendScript method
Reference:
```
Cogwheel.suspendScript(<label name>)
```

suspendScript method makes script wait for label execution.

For example:
```
Cogwheel.findTaggedNPC(a=npc1)
Cogwheel.findTaggedNPC(b=npc2)

path:::Cogwheel.pathfind(a:15 24 -4)
Cogwheel.suspendScript(path)

Cogwheel.chat(b:Npc1 finished walking!)
Cogwheel.chat(a:I finished walking!)
```
This script will make npc tagged `npc1` pathfind and go to `15 24 -4` coordinates.
Then script execution will be paused to wait for npc1 to go to position when label is finished.
So npc1 and npc2 will chat thier messages only **AFTER** npc1 finished pathfinding and walking.

## dispatchScript method
Reference:
```
Cogwheel.dispatchScript(<script name>)
```

dispatchScript method allows dispathcing scripts for other scripts.
This is idenical to calling `/@storyanvil dispatch-script <script name>` in chat.

For example code below will dispatch(run) script named `hello_world.sa`:
```
Cogwheel.dispatchScript(hello_world.sa)
```

## dataDump method
Reference:
```
Cogwheel.dataDump()
```

dataDump method dumps all script data including but not limiting to varibles, script name etc. to minecraft logs.

This method should only be used for debuging.