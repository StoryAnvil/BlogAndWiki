# Creating addons
First, you should add Cogwheel Engine as gradle dependency. You can use one of maven repos listed <a onclick="$story.to('/wiki/wiki.html?p=wiki/projects/cogwheel/addons/maven')">here</a>

## Cogwheel Engine Infrustructure
All objects accessible in CogScript are instances of `com.storyanvil.cogwheel.infrustructure.CogProprtyManager`.

All `CogPropertyManager`'s have three main methods:
- `boolean hasOwnProperty(String name)` - Returns `true` if object has property with provided name
- `@Nullable CogPropertyManager getProperty(String name, ArgumentData args, DispatchedScript script)` - Actually runs property
- `boolean equalsTo(CogPropertyManager o)` - Returns `true` if provided `CogPropertyManager` equals to other one

### hasOwnProperty and getProperty
Let's see some examples of this methods work.

Imagine we have `CogPropertyManager` for redstone lamp. In this example redstone lamp will have only two methods: `on()` - turns on the lamp and `off()` - turns off the lamp.

In this case code for this redstone lamp will look kinda like this:
```java
public class CogRedstoneLamp implements CogPropertyManager {
    private BlockPos lampPos;
    public CogRedstoneLamp(BlockPos lampPos) {
        this.lampPos = lampPos;
    }

    @Override
    public boolean hasOwnProperty(String name) {
        return name.equals("on") || name.equals("off");
    }

    @Override
    public @Nullable CogPropertyManager getProperty(String name, ArgumentData args, DispatchedScript script) {
        if (name.equals("on")) {
            // turn the lamp on
        } else if (name.equals("off")) {
            // turn the lamp off
        }
        return null;
    }

    @Override
    public boolean equalsTo(CogPropertyManager o) {
        if (o instanceof CogRedstoneLamp other) {
            return other.lampPos == this.lampPos;
        }
        return false;
    }
}
```

And lets look at CogScript code where we use this new `CogRedstoneLamp` class.

```
# Get instance of CogRedstoneLamp somehow and store it in varible called lamp
lamp.on()
```

When CogScript parser will reach `lamp.on()` line it will check what is stored in `lamp` varible by running `DispatchedScript#get`.

In this case we will expect it to be some instance of `CogRedstoneLamp`.

Then parser will see `.on()` meaning property of that `CogRedstoneLamp` named `on` needs to be called.

To call property parser will call `CogRedstoneLamp#hasOwnProperty` passing name of called property.
If `CogRedstoneLamp#hasOwnProperty` returns `false` parser will raise error because CogScript code tries to call property that does not exist.

But if `CogRedstoneLamp#hasOwnProperty` returns `true` parser will call `CogRedstoneLamp#getProperty` passing `on` as name.


So, basically you can think of `CogPropertyManager`'s how hashmaps, `CogPropertyManagers#hasOwnProperty` is kinda like hashmap's `containsKey` method and `CogPropertyManager#getProperty` is like hashmap's `get` method.
First we check does it contain key we need and if it does we get it.

For `CogPropertyManager`'s with lots of methods its actually recommeneded to use hashmaps for method checking. So lets rewrite code of `CogRedstoneLamp` to use a hashmap:
```java
import com.storyanvil.cogwheel.infrustructure.cog.PropertyHandler;
import com.storyanvil.cogwheel.infrustructure.CogPropertyManager;

public class CogRedstoneLamp implements CogPropertyManager {
    private static HashMap<String, PropertyHandler> methods = null;

    private BlockPos lampPos;
    public CogRedstoneLamp(BlockPos lampPos) {
        this.lampPos = lampPos;
    }

    @Override
    public boolean hasOwnProperty(String name) {
        if (methods == null) registerProperties();
        return methods.containsKey(name);
    }

    @Override
    public @Nullable CogPropertyManager getProperty(String name, ArgumentData args, DispatchedScript script) {
        return methods.get(name).handle(name, args, script, this);
    }

    @Override
    public boolean equalsTo(CogPropertyManager o) {
        if (o instanceof CogRedstoneLamp other) {
            return other.lampPos == this.lampPos;
        }
        return false;
    }

    private static void registerProperties() {
        methods = new HashMap<>();
        methods.put("on", (name, args, script, o) -> {
            // turn the lamp on
        });
        methods.put("off", (name, args, script, o) -> {
            // turn the lamp off
        });
    }
}
```

All Cogwheel Engine's built-in `CogPropertyManager`s are using utility class called `EasyPropManager` for this.

**`EasyPropManager` SHOULD NOT BE USED IN ANY ADDONS. IT IS INTERNAL COGWHEEL API THAT SHOULD ONLY BE USED IN COGWHEEL's CODE AS `EasyPropManager` DOES NOT IMPLEMENT ANY NAMESPACING**

## Actually doing stuff related to minecraft in CogPropertyManagers
In examples above actual code related to minecraft was omitted and that is for a reason. CogWheel Engine runs in seperate thread pool from minecraft meaning
you should not access minecraft classes directly. For doing so you should use `CogwheelExecutor` class.

For example lets actually make `on` and `off` methods of `CogRedstoneLamp` work:
```java
private static void registerProperties() {
    methods = new HashMap<>();
    methods.put("on", (name, args, script, o) -> {
        CogRedstoneLamp lamp = (CogRedstoneLamp) o;
        BlockPos lampPos = lamp.lampPos;
        // Use CogwheelExecutor#scheduleTickEvent to run code in minecraft's server thread
        CogwheelExecutor.scheduleTickEvent(event -> {
            ServerLevel level = (ServerLevel) event.level;
            BlockState state = Blocks.REDSTONE_LAMP.defaultBlockState();
            state.setValue(RedstoneLampBlock.LIT, true);
            level.setBlock(lampPos, state, 2);
        });
        return null;
    });
    methods.put("off", (name, args, script, o) -> {
        CogRedstoneLamp lamp = (CogRedstoneLamp) o;
        BlockPos lampPos = lamp.lampPos;
        CogwheelExecutor.scheduleTickEvent(event -> {
            ServerLevel level = (ServerLevel) event.level;
            BlockState state = Blocks.REDSTONE_LAMP.defaultBlockState();
            state.setValue(RedstoneLampBlock.LIT, false);
            level.setBlock(lampPos, state, 2);
        });
        return null;
    });
}
```

### How to return data from Mineraft thread
Like in example above we sometimes need to use minecraft threads. But when we use `CogwheelExecutor#scheduleTickEvent` we can't return values.

To know how to return values from minecraft thread to Cogwheel thread let's see how CogScript's minecraft level object gets player list:
```java
manager.reg("getPlayers", (name, args, script, o) -> {
    // do not return anything in the PropertyHandler.
    throw new PreventSubCalling(new SubCallPostPrevention() {
        @Override
        public void prevent(String variable) {
            // When CogScript parser is informed so that method requires other thread's data to return we schedule our code on miencraft server thread
            CogwheelExecutor.scheduleTickEvent(event -> {
                // Create array for players
                ArrayList<CogPlayer> players = new ArrayList<>();

                // Get all players add put them in the array
                for (ServerPlayer player : ((ServerLevel) event.level).players()) {
                    players.add(new CogPlayer(new WeakReference<>(player)));
                }

                // Put return value of our property to script using provided varible name
                script.put(variable, new CogArray<>(players));

                // Let the script know we finished by scheduling parser back in Cogwheel thread
                CogwheelExecutor.schedule(script::lineDispatcher);
            });
        }
    });
});
```

## Other CogPropertyManager's methods
We talked about `equalsTo`, `hasOwnProperty` and `getProperty` methods before but `CogPropertyManager` has other methods that are not required to be implemented.

This methods are:
- `convertToString` - this method in equivalent on java's `toString` method but for CogScript usage
- `convertToCogString` - this method should returns same string as `convertToString` but as `CogScript` object


## Registring default varibles
Some objects like <a onclick="$story.to('/wiki/wiki.html?p=wiki/projects/cogwheel/cogmaster')">CogMaster</a> are needed for all scripts to function because they are
the only way to get other objects.

To register your `CogPropertyManager` to be a default varible in all CogScript's use `com.storyanvil.cogwheel.registry.CogwheelRegistries#register` method in your mods initializer:
```java
@Mod(YourMod.MODID)
public class YourMod
{
    public static final String MODID = "example_mod";

    public YourMod(FMLJavaModLoadingContext context)
    {
        IEventBus modEventBus = context.getModEventBus();
        // ....
        CogwheelRegistries.register("YourDefaultVaribleName", script -> new YourPropertyManager());
    }
}
```