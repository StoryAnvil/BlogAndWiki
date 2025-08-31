# ⚠️ THIS WIKI PAGE IS DEPERACTED! INFORMATION HERE CAN BE OUTDATED!
# Level object
Level object represents minecraft world.

Level can be acquired by `Cogwheel.getLevel()` method.

In this article we will list all level methods avalible.

## runCommand method
Level's `runCommand` method allows running commands like in command blocks.

For example to run `/give @a minecraft:apple 64` command you can use syntax below:
```
level = Cogwheel.getLevel()
level.runCommand("/give @a minecraft:apple 64")
```

## getPlayers method
`getPlayers` method returns <a onclick="$story.to('/wiki/wiki.html?p=wiki/projects/cogwheel/array')">list</a> of <a onclick="$story.to('/wiki/wiki.html?p=wiki/projects/cogwheel/player')">players</a> in the world.