# Creating Cogwheel Engine addons
1. Create normal mod as you usually normally do using forge's mdk
2. Install [Geckolib 4](https://github.com/bernie-g/geckolib/wiki/Installation-(Geckolib4))
3. Add Cogwheel Engine as gradle dependency as shown <a onclick="$story.to('/wiki/wiki.html?p=wiki/projects/cogwheel/addons/maven')">here</a>
4. Add Cogwheel Engine as dependency in your `resources/META-INF/mods.toml` file:
```toml
[[dependencies.${mod_id}]]
    modId="storyanvil_cogwheel"
    mandatory=true
    # This version range declares supported Cogwheel Engine versions
    versionRange="[1.0.0,)"
    ordering="BEFORE"
    side="BOTH"
```
5. Done! Your mod is now a Cogwheel Engine addon. You can access Cogwheel Engine's classes and your mod will require Cogwheel Engine to run (btw, you don't need to install Cogwheel Engine in dev environment. It will be installed just by adding it as gradle dependency)

## API Deprecation Policy
It is recommended to use [`CogwheelAPI` class](https://github.com/StoryAnvil/Cogwheel-Engine/blob/master/src/main/java/com/storyanvil/cogwheel/api/CogwheelAPI.java) for all interactions with Cogwheel Engine if `Cogwheel API` has needed method.

Methods that addons can use safely marked with annotations defined in [`Api` class](https://github.com/StoryAnvil/Cogwheel-Engine/blob/master/src/main/java/com/storyanvil/cogwheel/api/Api.java). You can read what this annotations mean below in this wiki page.

If method in not annotated by any of `Api` class annotations, you should assume this method is unsafe.

### @Api.Stable
`@Api.Stable` annotation means annotated method can be safely using with any versions of Cogwheel Engine starting from version specified in `Api.Stable#since()` ending with next major release.

For example:
```java
// Annotaion below means exampleMethod can be safely used with Cogwheel Engine versions from 2.4.0 to 3.0.0
@Api.Stable(since = "2.4.0")
public static void exampleMethod() {
    // some really usefull code
}
```

### @Api.Experimental
`@Api.Experimental` annotations is same to `@Api.Stable` but it still leaves small chance for this method to be removed.

Usage of Experimental methods is not recommended. Consider using stable methods if there is any.

For example:
```java
// Annotaion below means exampleMethod can be mostly safely used with Cogwheel Engine versions from 2.4.0 to 3.0.0
@Api.Experimental(since = "2.4.0")
public static void exampleMethod() {
    // some really usefull code
}
```

## @Api.Internal
`@Api.Internal` annotaion means annotated method is internal and should not be used outside of Cogwheel Engine.

For example:
```java
// Annotaion below means exampleMethod can not be safely used outside of Cogwheel Engine
@Api.Internal
public static void exampleMethod() {
    // some really usefull code
}
```

## @Api.MixinsNotAllowed
`@Api.MixinsNotAllowed` annotaion means you should avoid adding mixins to this method because this can cause issues.

Also `@Api.MixinsNotAllowed` has `where()` method that usually specifies where you can mixin safely to achive results you probably wanted.

## @Api.MixinIntoHere
`@Api.MixinIntoHere` annotaion means you welcomed to mixin into annotated method if needed.


## What's next?
Reminder, we will refer to Cogwheel Engine as CE in other wiki pages.

You can check <a onclick="$story.to('/wiki/wiki.html?p=wiki/projects/cogwheel/home')">Wiki's home page</a> to see other information about making Cogwheel Engine addons.
