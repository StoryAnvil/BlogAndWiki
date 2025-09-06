# Environments
You might noticed `def:` prefix in most ways of executing scripts.

When executing script (using `Cogwheel.dispatchScriptGlobal()` or `/@storyanvil dispatch-script` command) you need to provide environment name before script name.

Environment name separated from script name with `:` (`<environment name>:<script name>` syntax).
Most of the time you will use `def` environment, but you should know there are other ones.

Environments are used to isloate scripts from each other when this is needed.
Usually environments are used by Cogwheel Engine to isloate CogScript code created by Cogwheel Engine addons and libraries.

Scripts can't interact with <a onclick="$story.to('/wiki/wiki.html?p=wiki/projects/cogwheel/specs/manifest.sa.json')">manifests</a> and <a onclick="$story.to('/wiki/wiki.html?p=wiki/projects/cogwheel/specs/storylevel.sa.json')">StoryLevel's put and get properties</a> of other environments.

## `def` environment (default environment)
All scripts in `.minecraft/config/cog` folder are default environment scripts.

There is no way to call scripts from that folder in a different environment and there isn't much of point to it.
