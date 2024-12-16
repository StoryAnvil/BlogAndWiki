<fromDevsToDevs></fromDevsToDevs>

# Getting started

1. <a onclick="$story.to('/download.html?id=shaftapi')">Download datapack</a>
2. Create datapack
   Just create datapack normally but don't use regular `#minecraft:tick` and `#minecraft:load` tags
3. Create following tags
   `<datapack>/data/minecraft/tags/function/shaft_check.json` (equivalent to `#minecraft:load`), `<datapack>/data/minecraft/tags/function/shaft_install.json` and `<datapack>/data/minecraft/tags/function/shaft_tick.json` (equivalent to `#minecraft:tick`)
4. Create 3 functions in your datapack for reseting (equivalent to `#minecraft:load`), tick and install

Add tick function to `#minecraft:shaft_tick` tagput code you want to execute each tick

Add install function to `#minecraft:shaft_install` tag and put following code:

```mcfunction
scoreboard players set <uniqe id for you datapack> shaft <version of your datapack>
```

Add reseting function to `#minecraft:shaft_check` tag and put code you want to execute on reload
Also here you can require other datapacks by adding following line in reseting function. You can depend on datapack only if it uses ShaftAPI too.

```mcfunction
function storyanvil:shaft/require {id:"<uniqe id for datapack you want to depend on>", version:"<range of datapack versions to depend on>"}
```
