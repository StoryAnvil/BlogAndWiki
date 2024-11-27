# Schedule

Schedule is `/cogwheel` subcommand that used to schedule command execution after specified time (in ticks).

## Syntax

```
/cogwheel schedule <DELAY IN TICKS> "<COMMAND>"
```

As `<DELAY IN TICKS>` use number that represents how many ticks should pass before execting command

As `<COMMAND>` use just any command you can type in chat (without `/`).

## Examples

```
/cogwheel schedule 20 "say 1"
```

This will say 1 after 20 ticks.

## Differnce between vanialla schedule command

Vanilla schedule command[^sch] used to schedule functions but our schedule command schedules just single command.

[^sch]: [See minecraft wiki](https://minecraft.wiki/w/Commands/schedule)
