# .sad files (avaliable starting from Cogwheel Engine 2.7.0)
Files with `.sad` extension can be created anywhere files with `.sa` can. `.sad` files are called (S)tory(A)nvil (D)ialogs.

StoryAnvil Dialogs allow writing NPCs dialogs faster and easier then in CogScript.

## Creating StoryAnvil Dialog
You just need to create a text file with `.sad` extension in folder where you create your `.sa` files. StoryAnvil Dialogs file names must only contain lowercase english characters, numbers, underscores or dots (`a-z_.`).

To run StoryAnvil Dialog you can use any method you use for `.sa` files (for example (for `dialog.sad` filename): `/@storyanvil dispatch-script def:dialog.sad`).

## Writing StoryAnvil Dialogs
Each line of code in StoryAnvil Dialog files must follow one of the following syntaxes:

Execution CogScript code:
```
!<CogScript code>
```

NPC dialog line:
```
@<npc variable name>:<FORMATTED dialog text>
```

NPC question:
```
@<npc variable name>?<FORMATTED question text>
 + <FORMATTED response 1>
  <any amount of StoryAnvil Dialog code>
 + <FORMATTED response 2 (any amount of responses is allowed)>
  <any amount of StoryAnvil Dialog code>
```

Minecraft command:
```
/<command>
```

## Going deeper
Let's talk about each allowed type of syntax and how to use them.

### Execution CogScript code
CogScript in StoryAnvil Dialogs can be used to add some code that will be executed during dialog or to get NPC for dialogs.

As you may noticed any actions with NPC in dialogs require `<npc variable name>`. To use them you can get and store NPC in variable using `Execution CogScript code` syntax. For example:
```cogdialog
!actor = Cogwheel.getTaggedNPC("actorTag")
```
Then you can use this NPC if different StoryAnvil Dialog lines, for example:
```
@actor:Hello!
```

Any CogScript code is allowed to be used here. Only exception is IF statements.

CODE BELOW IS INVALID BECAUSE COGSCRIPT IF STATEMENTS ARE NOT ALLOWED IN StoryAnvil Dialogs!
```cogdialog
!if (<any condition here>) {
!    Cogwheel.log("Test")
!}
```

### NPC dialog line
This syntax allows to make NPC say something.

```cogdialog
!actor = Cogwheel.getTaggedNPC("actorTag")
@actor:Hello!
```
In example above we get NPC first, then make it say `Hello!`

### Minecraft command
Minecraft command syntax allows running any chat command (including modded ones) from StoryAnvil Dialogs.

For example:
```cogdialog
/say 1
```

### NPC question
NPC Question syntax allows making NPC ask player something and then act accordingly to answer.

```cogdialog
!actor = Cogwheel.getTaggedNPC("actorTag")
@actor?Do you want to go there?
 + Yes
  @actor:Okay. Let's go!
  /tp @a 14 5 497
 + No
  @actor:Then we need to find another way out
```
Example code above will get npc with tag `actorTag` and store it in `actor` variable.

Then NPC will ask player `Do you want to go there?` with options to answer `Yes` or `No`.

Then if player answered `Yes`, NPC will say `Okay. Let's go!` and `tp @a 14 5 497` command will be executed.

But if player answered `No`, NPC will say `Then we need to find another way out`.


Nesting question is also possible
```cogdialog
!actor = Cogwheel.getTaggedNPC("actorTag")
@actor?Do you want to go there?
 + Yes
  @actor:Okay. Let's go!
  /tp @a 14 5 497
 + No
  @actor:Then we need to find another way out
  @actor?Do you have any ideas?
  + No
   @actor:Then, let's return home
   /tp @a 844 5 497
  + We can go through the cave
   @actor:Okay, let's go to the cave
   !Cogwheel.log("Going to the cave!")
   /tp @a 844 45 497
  + Kill me
   /kill @a
```

## FORMATTED options
You may noticed `FORMATTED` in some places in StoryAnvil Dialog syntaxes. This means CogScript code can be used here.

To use CogScript code in `FORMATTED` arguments, you need to write `${<CogScript code>}`

For example:
```cogdialog
!actor = Cogwheel.getTaggedNPC("actorTag")
@actor:Hello! I am ${actor.getName()}! Nice to meet you.
```

## Changing time of dialogs
You can change time of `NPC dialog line`s by adding `${ticks.set(^<amount of ticks dialog line will take>)}` in dialog text.

For example:
```cogdialog
@actor:${ticks.set(^200)}Really long!
```
