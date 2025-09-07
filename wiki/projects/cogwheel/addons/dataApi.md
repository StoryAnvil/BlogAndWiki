# DataAPI
CE's DataAPI can be used by CE's addons for data serialization (encoding) and deserialization (decoding) data.

We will refer to `net.minecraft.network.FriendlyByteBuf` class as FBB in this wiki page.

#### WARNING! UTILL COGWHEEL ENGINE V2.11.0 DATAAPI IS EXPERIMENTAL, SO BREAKING CHANGES CAN OCCUR

## StoryCodecs
StoryCodec is one of most important classes of CE's DataApi. This class defines how some type of data can be encoded and decoded.

![(StoryCodec schema)](https://raw.githubusercontent.com/StoryAnvil/BlogAndWiki/master/wiki/projects/cogwheel/addons/storycodec_schema.png)

As visible in scheme above `StoryCodec<T>` can encode and decode classes of type: `T`.

`StoryCodec<T>` consists of two methods:
- Encoder that takes instance of `T` and FBB. This method encodes provided instance of `T` to provided FBB.
- Decoder that takes FBB and returns instance of `T`. This methods decodes and creates instance of `T` from provided FBB.

`StoryCodec<T>` is actually a record (records are immutable classes in java) that holds `BiConsumer<T, FriendlyByteBuf> encoder, Function<FriendlyByteBuf, T> decoder`.

### Example of StoryCodec
Let's see example of StoryCodec implementation for Boolean.
```java
public static final StoryCodec<Boolean> BOOLEAN_CODEC = new StoryCodec((value, buf) -> {
    buf.writeBoolean(value);
}, buf -> {
    return buf.readBoolean();
});
```

This is really simple example of StoryCodec that uses FBB builtin methods to encode and decode booleans.

### Builtin StoryCodecs
CE provideds big amount of ready-to-use StoryCodec instances for wide range of types. You can find them in [`StoryCodecs` class](https://github.com/StoryAnvil/Cogwheel-Engine/blob/master/src/main/java/com/storyanvil/cogwheel/data/StoryCodecs.java)

### Compound StoryCodecs
Compound StoryCodecs are StoryCodecs that join multiple StoryCodes into one.

For example we want to create StoryCodec for following class:
```java
public class ExampleClass {
    private String name;
    private int age;
    private boolean isAdmin;

    public ExampleClass(String name, int age, boolean isAdmin) {
        this.name = name;
        this.age = age;
        this.isAdmin = isAdmin;
    }
    public ExampleClass() {
        this.name = null;
        this.age = 0;
        this.isAdmin = false;
    }

    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
    public int getAge() {
        return age;
    }
    public void setAge(int age) {
        this.age = age;
    }
    public boolean isAdmin() {
        return isAdmin;
    }
    public void setAdmin(boolean admin) {
        isAdmin = admin;
    }
}
```
To do that we need to write our own `StoryCodec<ExampleClass>` or we can use compound StoryCodec. Let's write our StoryCodec for this first:
```java
public class ExampleClass {
    public static final StoryCodec<ExampleClass> CODEC = new StoryCodec<>((exampleClass, buf) -> {
        // Write length of name string
        int nameLength = exampleClass.getName().length();
        buf.writeInt(nameLength);
        // Write each character of name string
        for (int i = 0; i < nameLength; i++) {
            buf.writeByte(exampleClass.getName().charAt(i));
        }
        // Write age integer
        buf.writeInt(exampleClass.getAge());
        // Write admin boolean
        buf.writeBoolean(exampleClass.isAdmin());
    }, buf -> {
        // Create instance of class we want to decode
        ExampleClass instance = new ExampleClass();
        // Read length of name string
        int nameLength = buf.readInt();
        // Create StringBuilder to build name string from individual characters
        StringBuilder sb = new StringBuilder();
        // Read each character and append it to StringBuilder
        for (int i = 0; i < nameLength; i++) {
            sb.append((char) buf.readByte());
        }
        // Set name string to built StringBuilder
        instance.setName(sb.toString());
        // Read and set age integer
        instance.setAge(buf.readInt());
        // Read and set admin boolean
        instance.setAdmin(buf.readBoolean());
        return instance;
    });

    private String name;
    private int age;
    private boolean isAdmin;

    // Constructors, getters and setters omitted
}
```
This way is really complicated and requires repeating same code over and over. Implenting StoryCodec for object that just stores String, Integer and Boolean took big enough amount of code. Now imaging StoryCodec for class with like 10 fields. This is way too unnecessarily hard! To fix this we can use compound StoryCodec:
```java
public class ExampleClass {
    public static final StoryCodec<ExampleClass> CODEC = StoryCodecBuilder.build(
            StoryCodecBuilder.Prop(ExampleClass::getName, StoryCodecs.STRING),
            StoryCodecBuilder.Prop(ExampleClass::getAge, StoryCodecs.INTEGER),
            StoryCodecBuilder.Prop(ExampleClass::isAdmin, StoryCodecs.BOOLEAN),
            ExampleClass::new
    );

    private String name;
    private int age;
    private boolean isAdmin;

    // Constructors, getters and setters omitted
}
```
That's way cleaner and simpler. But let's dig deeper and learn how does compound StoryCodecs work.

`StoryCodecBuilder#Prop` method creates `StoryCodecBuild.P<R,Y>` of Y type. `StoryCodecBuild.P<R,Y>` store two things:
- `java.util.function.Function<R,Y>` that explains how to get value assigned to this property from instance of `R` type
- `StoryCodec<Y>` that explains how to encode and decode that individual property.

Basically, `StoryCodecBuild.P<R,Y>` represents `Y` type field of `R` type. For each property you want to encode and decode in compound StoryCodec you need to have mathing `StoryCodecBuild.P<R,Y>`.

`StoryCodecBuilder#Prop` method takes `java.util.function.Function<R,Y>` and `StoryCodec<Y>` to create instance of `StoryCodecBuild.P<R,Y>`.

For example:
```java
StoryCodecBuilder.Prop(ExampleClass::getName, StoryCodecs.STRING)
```
Tells Cogwheel Engine that there is field in `ExampleClass` that can be accessed with `ExampleClass#getName` method and can be encoded and decoded with `StoryCodecs.STRING` StoryCodec.


`StoryCodecBuilder#build` method takes some amount (0-15) `StoryCodecBuild.P<R,Y>`'s and method that takes decoded value of each property and creates class encoded by compound StoryCodec.

Let's see some examples of `StoryCodecBuilder#build` overload's arguments:
```java
public static <T0,T1,T2,R> StoryCodec<R> build(P<R,T0> firstProperty, P<R,T1> secondProperty, P<R,T2> thirdProperty, Function3<T0,T1,T2,R> factory) { /* internal code */}
```
There is schema of encodeing and decoding performing by compound StoryCodec created by method right above
![Compound StoryCodec shema](https://raw.githubusercontent.com/StoryAnvil/BlogAndWiki/master/wiki/projects/cogwheel/addons/comp1.png)
![Compound StoryCodec shema](https://raw.githubusercontent.com/StoryAnvil/BlogAndWiki/master/wiki/projects/cogwheel/addons/comp2.png)
