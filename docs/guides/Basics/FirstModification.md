<head>
<meta property="og:title" content="MCPReborn Wiki" />
<meta content="A resource to help others get around Minecraft's code." property="og:description" />
<meta property="og:type" content="website" />
<meta content="#43B581" data-react-helmet="true" name="theme-color" />
<meta property="og:url" content="https://archerv123456.github.io/MCPRWiki/" />
</head>

# Your First Modification

Before starting it's recommended that you have a basic knowledge of Java (classes, variables, importing, etc).

## Setup

To get started go to the **[MCPReborn Github Page](https://github.com/Hexeption/MCP-Reborn)** and download the latest version (1.20.2), and **follow** the instructions on the repo and come back here when you're done.

All done? Good! Now we can start look through Minecraft's source code! To start, search for the **MobEffects** class, here is where we will add something new.

## Mob Effect Creation

In the **MobEffects** class, you should see something like:
```java
public static final MobEffect MOVEMENT_SPEED = register("speed", (new MobEffect(MobEffectCategory.BENEFICIAL, 3402751)).addAttributeModifier(Attributes.MOVEMENT_SPEED, "91AEAA56-376B-4498-935B-2F7F68070635", (double)0.2F, AttributeModifier.Operation.MULTIPLY_TOTAL));
```

## Basic Effect

Once there we can starting adding a new effect, to get started type in:

```java
public static final MobEffect NAME = register("name", (new MobEffect(MobEffectCategory.BENEFICIAL, 3402751)));
```

This creates and registers the new effect for us to use! The ```"name"``` is the name id of the effect **MAKE SURE IT NOT SOMETHING THAT ALREADY EXISTS**, It **WILL** make a conflict if you do. Also make sure there are **no special characters** (', !, ., etc.), or **uppercase** keys.

The ```new MobEffect()``` part creates the effect itself, the ```MobEffectCategory``` within the ```new MobEffect()``` determines if its on the top row (BENEFICIAL) or bottom row (NEUTRAL/HARMFUL), the ```3402751``` is just the color in decimal.

From there you can start the game, and give yourself the effect! One thing you will notice is the ```effect.minecraft.name``` rather then ```name``` for the effect display name. You can learn how to set up resources [here](../Beginner/Resources.md).

### Attributes

Now to add attribute modifiers! 

Adding attributes is fairly simple, but will require a [UUID Generator](https://www.uuidgenerator.net) for saving to work.

To start adding the attribute you want it to change add:

```java
.addAttributeModifier(Attributes.MOVEMENT_SPEED)
```

to the end of ```(new MobEffect(MobEffectCategory.BENEFICIAL, 3402751))```.

Next you'll need to generate a new UUID (link to one above), and insert it right after the chosen attribute like so:

```java
.addAttributeModifier(Attributes.MOVEMENT_SPEED, "RANDOM UUID")
```

Now to set the modifier, simply add a double for the modifier:

```java
.addAttributeModifier(Attributes.MOVEMENT_SPEED, "RANDOM UUID", 1D)
```

Finally, select the ```AttributeModifier.Operation``` you want to use. ```ADDITION``` adds the value to the attribute, ```MULTIPLY_TOTAL``` multiplies the attribute including other modifers, and ```MULTIPLY_BASE``` multiplies the attribute, ignoring the other modifiers.

After all that you should have something that looks like:

```java
public static final MobEffect NAME = register("name", (new MobEffect(MobEffectCategory.BENEFICIAL, 3402751)).addAttributeModifier(Attributes.MOVEMENT_SPEED, "RANDOM UUID", 1D, AttributeModifier.Operation.ADDITION));
```

Now start the game and give it a test run!