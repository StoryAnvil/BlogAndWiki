# Switch statement
Switch statement allows comparing value of varible and running code according to it.

## Usage example
```
actor = Cogwheel.getTaggedNPC("npc1")
actor.name("Test NPC")
actor.chat("Hello! How can I help you?")
response = actor.dialogChoices("Who are you?", "Why are you here?", "What time is it?")

*switch response
^0 -> who
^1 -> why
^2 -> time

@marker who
actor.chat("I am test NPC")
@stop

@marker why
actor.chat("Sadly, I do not know")
@stop

@marker time
actor.chat("It is day probably")
@stop
```

