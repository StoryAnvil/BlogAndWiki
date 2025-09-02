# Setting up Cogwheel Engine
## Installing Cogwheel Engine and it's dependencies 
First choose minecraft version supported by Cogwheel Engine. 
Here is list of them: ![cogwheel engine versions](https://img.shields.io/modrinth/game-versions/cogwheel-engine?label=%20)

Then you need to download cogwheel engine for this Minecraft version [here](https://modrinth.com/mod/cogwheel-engine). After that install [geckolib](https://modrinth.com/mod/geckolib) and [Cogwheel Engine Service Provider](https://modrinth.com/mod/cogwheel-engine-service) for your minecraft version.

## Short names
We will refer to Cogwheel Engine as CE for short. And we will call Cogwheel Engine Service Provider as CESP for short.

By config folder we mean `.minecraft/config`

Scripts folder is `<config folder>/cog`

Libraries folder is `<config folder>/cog-libs`

CogScript is programming language used by CE.

## What was installed?
CE is mod that runs CogScripts and allows creating your Story-based modpacks, mods, maps etc.

CESP is library mod that allows making other minecraft mods by coding in CogScripts

Geckolib is used for custom NPC models, textures and animations

# Writing in CogScript
## Create script file
1. Open scripts folder (`<config folder>/cog`)
2. Create file with .sa extension. *(PS: .SA stands for (S)tory(A)nvil))*
3. Make sure your file named only using lowercase english letters and numbers
4. Open file with any text editor of your choice.
5. Now you can write any CogScript code you like in this file. You can use this example code that prints `Hello World` to minecraft logs
```
Cogwheel.log("Hello World")
```
6. Now you can run script by using `/@storyanvil dispatch-script def:<script file name including extension>`

## Basic syntax
All CogScript code usually consists of **expressions**. They start with name of variable you want to interact with:
```
<variable name>
```
This expression will just return value of this variable.

Then you can add any amount of **property getters**:
```
<variable name>.<property name>(<args>)
               ^^^^^^^^^^^^^^^^^^^^^^^^
                This is property getter
```
For example, example code mentioned above uses property getter with property name `log` and `"Hello world"` arguments from variable named `Cogwheel`:
```
Cogwheel.log     ("Hello world")
^^^^^^^^ ^^^      ^^^^^^^^^^^^^
Variable Property Arguments
```
This kind of expression will return result of provided property getter.

You can stack as much property getters as you want. This will cause them to chain call each others return values:
```
Cogwheel.true().not()
```
Code above will get `true` property from variable named `Cogwheel` and **then** get property `not` from return on `Cogwheel`'s `true` property.

## Arguments
Now we will look into **property getter's** arguments.

Arguments can be empty (for example: `Cogwheel.true()`).

Or arguements can contain any amount of **expressions** separated by commas (`,`).
For example:
```
Cogwheel.randomInt(^1, ^10)
```
Code above will get `randomInt` property from `Cogwheel` variable with two arguments: `^1` and `^10`.

*PS: You can any amount of arguments (even zero or one) in property getters*

Each argument (separated by `,`) is a **expression**, so you can do stuff like this:
```
Cogwheel.log(Cogwheel.true())
```
Code below will run `Cogwheel.true()` expression first then save its return value and run `Cogwheel.log(<a>)` with `<a>` replaced with return value of first expression (`Cogwheel.true()`)

## Primitive expressions
We talked about **expressions** start from variable name, but this is not always the case.

**Expression** can start and end with quotes (`"`) to create a [CogString](https://storyanvil.github.io/wiki/wiki.html?p=wiki/projects/cogwheel/specs/cogstring.sa.json). *CogString represents any amount of characters*

For example **expression** below creates amd returns CogString that represents `Hello World`:
```
"Hello World"
```

Also **expression** can start with `^` character. This kind of **expression** can be used to create [CogIntegers](https://storyanvil.github.io/wiki/wiki.html?p=wiki/projects/cogwheel/specs/coginteger.sa.json). *CogIntegers represent whole numbers like 1, 0, -28, etc.*

For example **expression** below creates and returns CogInteger that represents -432:
```
^-432
```

## Creating variables
*PS: Variables can contain any kind of object like CogInteger, CogString etc.*

You might noticed `Cogwheel` used as variable in above examples. All scripts contain default variable named `Cogwheel` that contains [CogMaster](https://storyanvil.github.io/wiki/wiki.html?p=wiki/projects/cogwheel/specs/cogmaster.sa.json) object.

You can create your own variables by adding `<variable name> = ` (*PS: this is called variable setter*) to start of __any__ **expression**. For example to create variable names `world` that contains return value of `Cogwheel.getLevel()` expression you can use expression below:
```
world = Cogwheel.getLevel()
```

As mentioned above you can add variable setter to any expression anywhere, so code:
```
a = ^28
Cogwheel.log(a)
Cogwheel.scheduleThis(a)
```
can be replaced with:
```
Cogwheel.log(a = ^28)
Cogwheel.scheduleThis(a)
```

## Comments
You can add `#` to start of any **line of code** (NOT EXPRESSION) to make CE skip this line:
```
# This line will not be compiled by CE
```

## Chain Calling Prevented
Some properties marked as `Chain Calling Prevented`. This properties can't be stacked.
For example `Cogwheel.getTaggedNPC("e")` property is marked like this.

This mark means you can't do:
```
Cogwheel.log(Cogwheel.getTaggedNPC("e"))
```
And
```
Cogwheel.getTaggedNPC("e").getName()
```
But you can store result of `getTaggedNPC` in variable and then use it

## Init script
Script with file name `init.sa` is always executed when server starts (or world is loaded in singleplayer)

# What's next?
You can learn more <a onclick="$story.to('/wiki/wiki.html?p=wiki/projects/cogwheel/adv')">Advanced Scripting Techniques</a> or <a onclick="$story.to('/wiki/wiki.html?p=wiki/projects/cogwheel/specs/cogmaster.sa.json')">Check what you can do with CogMaster object (stored in default variable named `Cogwheel`)</a>