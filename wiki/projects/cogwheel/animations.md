# How to use custom animation on NPCs
Cogwheel Engine allows adding custom animations for NPCs.

## Creating animations
In this tutorial we will use [Blockbench](https://blockbench.net) for creating animations.

1. Install [Geckolib Animation Utils](https://www.blockbench.net/plugins/animation_utils) plugin
2. Download Cogwheel Engine [NPC model](https://github.com/StoryAnvil/Cogwheel-Engine/blob/master/src/main/resources/assets/storyanvil_cogwheel/geo/entity/npc.geo.json) *Press ctrl-shift-S on github to download the file*
3. Open downloaded model in blockbench by pressing `ctrl-O` in blockbench and selecting model you downloaded
4. Click `Animate` in right top corner of you screen
5. Here you can animate the NPC, **DO NOT EDIT THE MODEL**. You can watch [this video](https://www.youtube.com/watch?v=3srLEdFTgVI) if you need help or [ask question on github](https://github.com/orgs/StoryAnvil/discussions/categories/questions)
6. After you done animating press on save icon next to `Unsaved` section in animations section
7. Save you animations json file in any texturepack in modpack you have cogwheel engine in. Animations json file should be in `assets/<namespace>/animations` folder

## Running animations
Before you can run animations you need to register json file in CogScript.
```py
# if your animations json file in resource pack is assets/test/animations/custom_anims.json
Cogwheel.addAnimationSource("test:animations/custom_anims")

# then get npc you want to animate
npc = Cogwheel.getTaggedNPC("animatedNpc")

# start the animation by name
# first argument is animation name and second is amount of ticks for animations
npc.animation("animation.model.test", ^20)
```

## Problems
If you have any problems, you can [ask questions on github](https://github.com/orgs/StoryAnvil/discussions/categories/questions)