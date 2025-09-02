# Advanced Scripting
This wiki pages talks about more advanced CogScript syntax you can use for easier development

## IF Statements
If statements allow running code only **if** some requirement is met. For example if two CogStrings are equal.

**Syntax:**
```
if (<condition>) {
    # Code to execute if condition met
}
```

Condition is any **expression** that returns a [CogBool](https://storyanvil.github.io/wiki/wiki.html?p=wiki/projects/cogwheel/specs/cogbool.sa.json)

For example code below will make NPC say `Hello World` only if string in variable `check` is `Allow`
```
# Get npc by tag "test"
npc = Cogwheel.getTaggedNPC("test")

# Put value Allow in check variable
check = "Allow"

if (check.equals("Allow")) {
    npc.chat("Hello World")
}
```
If you run code above, NPC will say `Hello World` but if you will change variable `check` to have anything other then `"Allow"` CogString, nothing will happen.

## Using IF Statements for dialogs
```
# Get NPC with tag "a"
npc = Cogwheel.getTaggedNPC("a")

# Give NPC a name
npc.setName("Test NPC")

# Ask players to choose a dialog option
response = npc.dialogChoices("Hi!", "Who are you?")

# If player responded "Hi!"
if (response.equals(^0)) {
    npc.chat("Hello!")

    # Ask another option
    response = npc.dialogChoices("zero", "one")
    if (response.equals(^0)) {
        npc.setSkin("You said zero")
        # Set response to -1, so "if (response.equals(^1)) {" won't run
        response = ^-1
    }

    if (response.equals(^1)) {
        npc.chat("You said one")
    }
    response = ^-1
}
if (response.equals(^1)) {
    npc.chat("I am NPC!")
}
```

## Switch statements
**SWITCH STATEMENTS WERE REMOVED IN FAVOUR OF IF STATEMENTS.**

# What's next?
You can <a onclick="$story.to('/wiki/wiki.html?p=wiki/projects/cogwheel/specs/cogmaster.sa.json')">Check what you can do with CogMaster object (stored in default variable named `Cogwheel`)</a>