# Manifests (.sam)
Manifests allow listining to events using CogScript.

To create a manifest open your `.minecraft/config/cog/` folder then create file with (`.sam` extension).

Then you can subscribe to events by writing lines of code with following syntax:
```
<EVENT NAME> -> <SCRIPT NAME>
```

For example to make script named `place.sa` when player placed a block:
```
BLOCK_PLACED -> place.sa
```

**IMPORTANT:** You can have only one script bound to event. Binding new script to event will just overwrite the old one. To unbind script from event type `unset` in place of script name in manifest.

To run manifest you can use `@storyanvil dispatch-script` command. You can find list of avalible events <a onclick="$story.to('/wiki/wiki.html?p=wiki/projects/cogwheel/specs/events.sa.json')">here</a>

## Writing scripts
When script was dispathced by manifest event listener you can use `Cogwheel.getEvent()` method do get <a onclick="$story.to('/wiki/wiki.html?p=wiki/projects/cogwheel/specs/event_callback.sa.json')">event callback</a>.

Event callback object is really special as it can contain different methods for different events.