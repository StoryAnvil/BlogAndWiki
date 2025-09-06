# Config
Cogwheel Engine's config is located in `.minecraft/config/cog/config-main.json`.
Config file is reloaded when world is loaded or when `/@storyanvil dispatch-script def:config-main.json` chat command was executed.

Cogwheel Engine's config file uses JSON format (`//` comments are not allowed in real json configs. They will be used on this wiki page to tell what config properties do).
```json
{
    // Shows link to wiki
    "wiki": "https://storyanvil.github.io/wiki/wiki.html?p=wiki/projects/cogwheel/config",

    // Disabled ALL Cogwheel Engine script when set to true
    "disableAllScripts": false,

    // Disables BELT Protocol when set to true
    "disableBelt": false,

    // Disables NPC talking animation when set to false
    "npcTalkingAnimation": true,

    // Array of strings. Each string is treated like resource location. CogScript LineHandlers with listed resource locations will be disabled
    // Leave empty if you don't know what this does. Changing this can break CogScript
    "disableLineHandles": [],

    // Enables object monitor when set to true
    // Leave empty if you don't know what this does. Only used for Cogwheel Engine development and debugging
    "monitorEnabled": false,

    // This can be used by CogScript scripts to tell if they need to debug information.
    // By default does nothing. Setting this to true will make Cogwheel.isDebugging() return true
    "devEnvironment": true
}
```