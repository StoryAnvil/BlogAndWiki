# .sam files
Files with `.sam` extension can be placed everywhere where `.sa` files are placed.

`.sam` stands for (S)tory(A)nvil (M)odule. Modules allow creating your own custom CogScript objects with their own properties!

## Creating a module
Open folder where you place your CogScript files and create file with `.sam` extension. File name must contain only lowercase english characters, numbers, underscores or dots (`a-z_.`).

## Defining constuctor
Constuctor is used to create instances of your module. (Instances are like copies of module. Module by itself just defines how its instances will behave like, what properties would they have, etc.)

Modules are required to have one constructor.

To define a constructor following syntax can be used:
```cogmodule
define (<arguments>) {
    <code>
    *return this
}
```

Replace `<arguments>` will comma-separated list of variable names to store constuctor's arguments in. Examples: `name, color`, `name`, `` (empty argument list means there are no arguments).

Replace `<code>` with any amount of lines of CogScript code that will be executed in the constructor.

Constructor example:
```cogmodule
define (name) {
    l = "Constructed module instance with name: "
    Cogwheel.log(l.append(name))
    *return this
}
```

Be sure to leave `*return this` line in the end of a constructor.

## Defining properties
*Properties are actions you can do with instances of module. For example `Cogwheel.log("test")` runs `log` property of module instance in `Cogwheel` variable with one argument: `"test"`*

Any amount of properties can be defined. But each property must have a new name.

To define a property following syntax is used:
```cogmodule
define <prop name>(<arguments>) {
    <code>
}
```

Replace `<arguments>` with comma-separated list of variable names to store properties arguments in.

Replace `<code>` with any amount of lines of CogScript code that will be executed each time this property is executed.

Replace `<prop name>` with name of this property.

Property example:
```cogmodule
define tell(text) {
    Cogwheel.log(Cogwheel.str("I'm telling: ").append(text, "!"))
}
```

You can use `*return <expression>` syntax to return any object in property.
For example:
```cogmodule
define addOne(n) {
    # This property takes CogInteger as arguments, clones it, add 1 to it and returns it
    *return n.clone().add(^1)
}
```

## This variable
Both in properties and constructors there is additional default variable named `this`.

This variable stores instance of module whose property (or constructor) is being executed.

## Storing data in module instances
All variables you create in properties and constructor are deleted after that property or constructor finished execution.

To store data in module's instance permanatly you can use `this.put(<variable name>, <object to store>)` and `this.get(<variable name>)`.

Example:
```cogscript
define (name) {
    this.put("myName", name)
    *return this
}
define whoAmI() {
    Cogwheel.log(this.get("myName"))
}
```

## Using module in CogScript scripts
First, you need to determine name of your module. Module's name always equal to module's file name without the `.sam` extension.

Then, you need to import your module:
```cogscript
*import def:<module name>
```

When module is imported its constructor object will be stored in variable named as module is, but with first letter being uppercase. (if module named `test`, variable will be named `Test`)

Now, you can create any amount of instances of this module by execution `new` property of module constructor object that was stored in variable by `*import`. This `new` properties takes arguments you defined in module's constructor.

### Full example
**test_module.sam**:
```cogmodule
define (name) {
    l = "Constructed module instance with name: "
    Cogwheel.log(l.append(name))
    *return this
}
define tell(text) {
    Cogwheel.log(Cogwheel.str("I'm telling: ").append(text, "!"))
}
```

**script.sa**:
```cogscript
*import def:test_module

instance = Test_module.new("apple")
instance.tell("Hello World!")
```

## CogInvokers with modules
You can get <a onclick="$story.to('/wiki/wiki.html?p=wiki/projects/cogwheel/specs/coginvoker.sa.json')">CogInvoker</a> that invokes module's property by adding `:` before property name. For example:
```cogscript
*import def:test_module

instance = Test_module.new("apple")
invoker = instance.:tell()
invoker.invoke("Hello World!")
```
