# Getting started with Cogwheel Engine
1. Download `StoryAnvil's Cogwheel Engine` from modrinth
2. Create folder `cog` in your `.minecraft/config` folder

## Writing CogScripts
CogScripts are core of Cogwheel Engine. CogScripts are written in CogScript programming language.

To create CogScript you need to create file in `.minecraft/config/cog` folder with `.sa` extension.

to run CogScripts in game you need to run `@storyanvil dispatch-script` command in chat.

### Example
Let's test Cogwheel Engine by creating script that writes `Hello, World!` in chat.

1. Create file `.minecraft/config/cog/hello_world.sa`.
2. Paste following code in it:
```
getLevel(level)
chat(level:Hello, World!)
```
3. Type `/@storyanvil dispatch-script hello_world.sa` in chat
4. You will see `Hello, World!` in chat!

## Leaving comments in CogScripts
When writing big scripts you probably want to include some comments in your code so you can leave some notes there.

To do that you can type new line that starts with `#`. After the `#` you can type anything you want, this line of code will be ignored by Cogwheel Engine.

For example:
```
# This is a comment
```

## Spawning NPCs
To spawn NPC you can use `/summon storyanvil_cogwheel:npc` command.

#### To know how to use NPCs and do other cool stuff read about <a onclick="$story.to('/wiki/wiki.html?p=wiki/projects/cogwheel/cogapi')">Cogwheel API</a>