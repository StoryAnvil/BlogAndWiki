# Manifests
Manifest object is always avaliable in default variable named `MANIFEST`.

## Events
Manifests can be used to subscribe to events (block breaking for example).

To do that <a onclick="$story.to('/wiki/wiki.html?p=wiki/projects/cogwheel/specs/manifest.sa.json')">Manifest</a>'s subscribeEvent property can be used. It takes two arguments: event name and script name to dispatch when event fired (event fired means event got triggered by something).

You can subscribe scripts to events anytime, but most common time to do it is `init.sa` script.

For example in `init.sa` script:
```cogscript
MANIFEST.subscribeEvent("cogwheel_engine:block/placed", "placed.sa")
```

In `placed.sa`:
```cogscript
callback = Cogwheel.getEvent()
Cogwheel.log(callback.block())
```

Scripts above will log name of placed block every time when block was placed by a player.

Properties of object returned by `Cogwheel.getEvent()` are different for every event.

## List of events

<div class="cw-global"><table class="cw-table">
    <thead>
        <tr><th>Event type (first argument of MANIFEST.subscribeEvent)</th><th>Cogwheel.getEvent() properties</th></tr>
    </thead>
    <tr><th><code>cogwheel_engine:block/placed</code><br>Called when block was placed by a player</th><th><pre style="padding:0px;"><code class="language-cogscript">
 Cogwheel.getEvent().block() = Resource location of placed block
 Cogwheel.getEvent().player() = Player who placed block
 Cogwheel.getEvent().x() = X coordinate of block
 Cogwheel.getEvent().y() = Y coordinate of block
 Cogwheel.getEvent().z() = Z coordinate of block
    </code></pre></th></tr>
    <tr><th><code>cogwheel_engine:block/broken</code><br>Called when block was broken by a player</th><th><pre style="padding:0px;"><code class="language-cogscript">
 Cogwheel.getEvent().block() = Resource location of broken block
 Cogwheel.getEvent().player() = Player who broken block
 Cogwheel.getEvent().x() = X coordinate of block
 Cogwheel.getEvent().y() = Y coordinate of block
 Cogwheel.getEvent().z() = Z coordinate of block
    </code></pre></th></tr>
    <tr><th><code>cogwheel_engine:block/use</code><br>Called when block was right clicked by a player</th><th><pre style="padding:0px;"><code class="language-cogscript">
 Cogwheel.getEvent().player() = Player who used block
 Cogwheel.getEvent().x() = X coordinate of block
 Cogwheel.getEvent().y() = Y coordinate of block
 Cogwheel.getEvent().z() = Z coordinate of block
    </code></pre></th></tr>
    <tr><th><code>cogwheel_engine:entity/use</code><br>Called when entity was right clicked by a player</th><th><pre style="padding:0px;"><code class="language-cogscript">
 Cogwheel.getEvent().player() = Player who used entity
 Cogwheel.getEvent().entity() = Entity which was right clicked
 Cogwheel.getEvent().x() = X coordinate of entity
 Cogwheel.getEvent().y() = Y coordinate of entity
 Cogwheel.getEvent().z() = Z coordinate of entity
    </code></pre></th></tr>
    <tr><th><code>cogwheel_engine:player/respawn</code><br>Called when player was respawned</th><th><pre style="padding:0px;"><code class="language-cogscript">
 Cogwheel.getEvent().player() = Player who was respawned
 Cogwheel.getEvent().endConquered() = true if player was respawned because they jumped in end portal
    </code></pre></th></tr>
    <tr><th><code>cogwheel_engine:entity/attack</code><br>Called when entity was attacked by a player</th><th><pre style="padding:0px;"><code class="language-cogscript">
 Cogwheel.getEvent().player() = Player who attacked
 Cogwheel.getEvent().entity() = Entity which was attacked
    </code></pre></th></tr>
    <tr><th><code>cogwheel_engine:belt/message</code><br>Called when belt message was received</th><th><pre style="padding:0px;"><code class="language-cogscript">
 Cogwheel.getEvent().msg() = Message
    </code></pre></th></tr>
</table></div>