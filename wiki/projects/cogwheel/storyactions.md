# StoryActions
StoryActions used to queue certain tasks that can take time.
NPCs use StoryActions to execute their commands like dialogs, skins, animtaions and others.

## Terminology
`StAc` is short name for StoryAction

`StAcs` is short name for multiple amount of StoryActions

`Instant StoryActions` are StoryActions that always take 1 tick excactly to execute

## How StoryActions work?
All StAcs are owned by some StoryAction Queue. StAc can't be owned by mutiple amount of StoryAction Queues. NPCs are one of types of StoryAction Queues.

Each StoryAction Queue executes StAcs one-by-one every tick. If StAc is not instant it will be executed during multiple ticks.

For example if NPC was given following StoryActions:
- Pathfind to `(x=15,y=0,z=34)`
- Send `"Hello"` to chat

It will first pathfind to `(x=15,y=0,z=34)` and only when NPC finished pathfinding it will chat `"Hello"`.

All StoryActions take at least one tick. Multiple StoryActions cannot be executed in same tick by singular StoryAction Queue.

Multiple StoryAction Queues (for example multiple NPCs) always execute StoryActions independently (Any amount of StoryAction can be executed if each of them is owned by different StoryAction Queue). For example: this means if one NPC is pathfining to somewhere and it is not done, other NPCs can still execute StoryActions anyways.

## Labels
When StoryAction finishes executing it emits(broadcasts) completion signal. This signal can be used if StoryAction has label.
Labels can be set by <a onclick="$story.to('/wiki/wiki.html?p=wiki/projects/cogwheel/specs/storyaction.sa.json')">StoryAction CogScript object</a>

For example this two scripts were executed same time:
```cogscript
npcA = Cogwheel.getTaggedNPC("a")
# We will assume NPC is far away from that location and will take some time to go there
npcA.pathfind(^15, ^0, ^34)
```

```cogscript
npcB = Cogwheel.getTaggedNPC("b")
# We will assume NPC is far away from that location and will take some time to go there
npcB.pathfind(^20, ^0, ^34)
```
In this scenario both NPCs will start pathfinding. Also CogSript properties that add StoryActions does not make script wait utill StoryAction finished, so both script above can be merged and will do exactly the same:
```cogscript
npcA = Cogwheel.getTaggedNPC("a")
npcB = Cogwheel.getTaggedNPC("b")
# We will assume both NPCs are far away from thier pathfind locations and will take some time to go there
npcA.pathfind(^15, ^0, ^34)
npcB.pathfind(^20, ^0, ^34)
```
But what if we want `npcB` start pathfinding only after `npcA` finished its pathfinding StoryAction?
To solve this StoryAction labels come in handy.

Let's give `npcA`'s StoryAction a label:
```cogscript
npcA = Cogwheel.getTaggedNPC("a")
npcB = Cogwheel.getTaggedNPC("b")
# We will assume both NPCs are far away from thier pathfind locations and will take some time to go there
npcA.pathfind(^15, ^0, ^34).setLabel("npcA StoryAction")
npcB.pathfind(^20, ^0, ^34)
```
For first glance, nothing will change, but from Cogwheel Engine's perspective `npcA`'s pathfinding StoryAction will emit completion singal with `npcA StoryAction` label when `npcA` finished pathfinding.
We can use that by giving `npcB` StoryAction that waits utill completion singal with needed label is emitted:
```cogscript
npcA = Cogwheel.getTaggedNPC("a")
npcB = Cogwheel.getTaggedNPC("b")
# We will assume both NPCs are far away from thier pathfind locations and will take some time to go there
npcA.pathfind(^15, ^0, ^34).setLabel("npcA StoryAction")
npcB.waitForLabel("npcA StoryAction")
npcB.pathfind(^20, ^0, ^34)
```
Code above will make `npcB` have two StoryActions:
- Wait for completion signal with `npcA StoryAction` label
- Pathfind to `(x=20,y=0,z=34)`

`npcB`'s first StoryAction (label waiting one) will take any amount of ticks before completion signal with needed label was emited.
Because StoryAction Queues (NPCs are StoryAction Queues) can only execute one StoryAction at a time this will cause `npcB` to do nothing but wait for `npcA` to finish walking. Also none of properties in code above actually make script wait, so if we will have any code after:
```cogscript
npcA = Cogwheel.getTaggedNPC("a")
npcB = Cogwheel.getTaggedNPC("b")
# We will assume both NPCs are far away from thier pathfind locations and will take some time to go there
npcA.pathfind(^15, ^0, ^34).setLabel("npcA StoryAction")
npcB.waitForLabel("npcA StoryAction")
npcB.pathfind(^20, ^0, ^34)

Cogwheel.log("Code after StoryActions")
```
This code will be executed without waiting for any of StoryActions to complete. So example above will log `Code after StoryActions` even if NPCs didn't finish pathfinding.

## Blocking
StoryActions have property named `blocking`. This property makes script wait utill this StoryAction finished. So we can fix example mentioned above to actually log `Code after StoryActions` only when `npcB` finished pathfinding.

```cogscript
npcA = Cogwheel.getTaggedNPC("a")
npcB = Cogwheel.getTaggedNPC("b")
# We will assume both NPCs are far away from thier pathfind locations and will take some time to go there
npcA.pathfind(^15, ^0, ^34).setLabel("npcA StoryAction")
npcB.waitForLabel("npcA StoryAction")
npcB.pathfind(^20, ^0, ^34).blocking()

Cogwheel.log("Code after StoryActions")
```

## Then property
Also StoryActions have property named `then` that takes one <a onclick="$story.to('/wiki/wiki.html?p=wiki/projects/cogwheel/specs/coginvoker.sa.json')">CogInvoker</a> as arguments.
This property makes StoryAction execute that invoker when its done executing.