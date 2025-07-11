# CogMaster
CogMaster object is stored in varible called `Cogwheel` in all scripts by default.

In this article we will list all CogMaster methods avalible.

## log method
Log method prints its first argument to minecraft logs

For example, code below will output `Test` in minecraft logs:
```
Cogwheel.log("Test")
```

For example, code below will output value of varible named test:
```
Cogwheel.log(test)
```

## getTaggedNPC moethod
This method searches for NPC by its [minecraft tag](https://minecraft.wiki/w/Commands/tag).

For example, code below will save npc tagged `npc1` in varible named `actor`:
```
actor = Cogwheel.getTaggedNPC("npc1")
```

## str method
Syntax below is invalid:
```
varible = "Text"
```

Strings can be created by wrapping text in quotes (`"`) only in method arguments.

For this reason `Cogwheel.str` exists. This method takes one string as argument and returns it.

For example invalid syntax above can be fixed using code below:
```
varible = Cogwheel.str("Text")
```

Calling `Cogwheel.str` is only required when string is not argument of another method, so code below:
```
t = Cogwheel.str("Test")
Cogwheel.log(t)
```
is idenical to code below:
```
Cogwheel.log("Test")
```

**⚠️ Methods cannot be called as method arguments, so code below is invalid:**
```
Cogwheel.log(Cogwheel.str("Test"))
```
It is required to save method's result to a varible before using it in method

## int method
Integers can be created kinda like strings but instead of wrapping text with quotes you need to add `^` before number.
Remember you can create integer objects this way only in method arguments. So code below is invalid:
```
age = ^24
```

You can fix code above using this code:
```
age = Cogwheel.int(^24)
```

## true and false methods
Booleans can be created by `Cogwheel.true()` and `Cogwheel.false()` methods

## getLevel method
`Cogwheel.getLevel()` method returns <a onclick="$story.to('/wiki/wiki.html?p=wiki/projects/cogwheel/level')">level</a> object.