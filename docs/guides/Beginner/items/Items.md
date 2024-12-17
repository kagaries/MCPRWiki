<head>
<meta property="og:title" content="MCPReborn Wiki" />
<meta content="A resource to help others get around Minecraft's code." property="og:description" />
<meta property="og:type" content="website" />
<meta content="#43B581" data-react-helmet="true" name="theme-color" />
<meta property="og:url" content="https://archerv123456.github.io/MCPRWiki/" />
</head>

# Basic Items

This guide will teach you (almost) all about how basic items work, function, and how to make your own!

## How do Items Work?

Items in Minecraft: Java Edition are registered in the **Items** class, and can be different based on the item type you are going for.

They can be split into different catagories:

Normal Items, Blocks, Tools (Sword, Pickaxes, etc.), and Special Items

### Normal Items

These are just generic items created with the **Item** class, mostly crafting items with no other purpose.

### Blocks

The physical items for the blocks, registered differently from other items using **registerBlock**, rather then **registerItem**.

### Tools

Tools have a seperate class for each tool type, but are registered like normal items

### Special Items

These items are created with a unqine class rather then a tool or the **Item** class, they usually have special functionality compared to normal items.

###

So what can you do with all this? You can create your own item of course, but before that, let's look at how some items function.

## Items and How They Function

The **Item** class comes with many functions for when you want an item to do something.

For example, the **PotionItem** class uses the use, useOn, and finishUsingItem functions for its functionality as a potion:

This is used when an item is finished being used when holding down the RMB for the use duration.
```java
public ItemStack finishUsingItem(ItemStack p_42984_, Level p_42985_, LivingEntity p_42986_)
```
This is used when using it on a block, entity, or other object in the world.
```java
public InteractionResult useOn(UseOnContext p_220235_)
```
This is used when clicking RMB.
```java
public InteractionResultHolder<ItemStack> use(Level p_42993_, Player p_42994_, InteractionHand p_42995_)
```

Every item from Ink Sacs to End Crystals all use these functions based on what thier needed for.

Items also use **Implements** for even more functionality, like armor with the **Equipable** interface:

```java
public class ArmorItem extends Item implements Equipable
```

###

Now that you have a general idea on how items function, we can now look at creating some!

## Creating An Item

To start, go to the **Items** class, this is where we'll add the item.

The first step to creating the item is creating the variable it will be registered from, which can be done like:

```java
public static final Item TEST = registerItem();
```

You will notice that the registerItem has an error, you'll need to add both the id of the item, and the item class itself, adding those should make this look like:

```java
public static final Item TEST = registerItem("test", new Item());
```

Getting closer, but the ```new Item``` is now erroring, this is because it needs a ```new Item.Properties()``` inside of it. Adding that, it should end up like:

```java
public static final Item TEST = registerItem("test", new Item(new Item.Properties()));
```

Great! Now we need to set up the items model data, you'll need to have followed the Resources or have a resources folder already set up for this.

To get to where we need to add the model def, it should be at ```resources/assets/minecraft/models/item```, this is where we will create the json.

When creating the json file, make sure the name of it is the exact same as the item id, Minecraft will ignore the json file if its not exact. For our test item, we'll use the ```acacia_boat.json``` as a template for use with our item. Inside the file it should look something close to:

```json
{
  "parent": "minecraft:item/generated",
  "textures": {
    "layer0": "minecraft:item/acacia_boat"
  }
}
```

the "parent" determines the way the item is displayed in the players hand, and the "textures" is where the the textures are defined. For now just keep the texture "layer0" the same, and change the file name to the id of your item. If all goes well, the item should now correctly work!

To test it, go ingame and use ``/give @s minecraft:ITEM_ID``

### Tools

Tools start off almost the same as a normal item:

```java
public static final Item TEST = registerItem("test", new Item());
```

But with a twist. The ```new Item()``` is going to be replaced by one of the tool classes.

For this example, we'll use the **SwordItem** class.

When replacing the ```new Item()``` with ```new SwordItem()```, you'll notice there are now 4 params rather then 1. The first 3 determine what the tier of ore it is, the base damage, and the attack speed, the 4th one is just ```new Item.Properties()``` again.

First on what we need to add is the tier of item, this determines added damage, durability, mining speed, and most of the other stuff about the stats about an item. For our item, I'll use ```Tiers.GOLD```, which should be:

```java
public static final Item TEST = registerItem("test", new SwordItem(Tiers.GOLD))
```

Next is the added damage, this is just an interger which gets added to the damage of the chosen item tier, adding it should look like:

```java
public static final Item TEST = registerItem("test", new SwordItem(Tiers.GOLD, 2))
```

Good! Now for the attack speed. This determines how fast the item charges, the lower the value, the slower the charge. For this it should just be:

```java
public static final Item TEST = registerItem("test", new SwordItem(Tiers.GOLD, 2, -1.5f))
```

The item will now attack 1.5 slower then fists.

Now just add ```new Item.Properties()``` and the item will be finished:

```java
public static final Item TEST = registerItem("test", new SwordItem(Tiers.GOLD, 2, -1.5f, new Item.Properties()))
```

Just follow how to create the item model above (with tools its recommended to use ``"minecraft:item/handheld"`` rather then generated) and now it should work!

## Tiers

As said above, tiers are a enumeration that determine the stats of an item. Now you get to learn how to create your own!

### Creating a Tier

Starting off, head to the **Tiers** class and there you'll see some tiers already defined. A tier is made up of mining level, durability, mining speed, damage, enchantment value, and repair ingredient.

Let's start off but defining the name of the tier we want:

```java
NETHERITE(4, 2031, 9.0F, 4.0F, 15, () -> {
  return Ingredient.of(Items.NETHERITE_INGOT);
}),
EMERALD();
```

Now with that we can start defining the values the items given the tier will use, first we start off with the mining level:

```java
EMERALD(3);
```

Next is the durability, we can just set it to 1024:

```java
EMERALD(3, 1024);
```

Now for the mining speed:

```java
EMERALD(3, 1024, 8.0F);
```

The mining speed is a float, make sure you don't forget that.

For the attack damage, we can just have 10.0F:

```java
EMERALD(3, 1024, 8.0F, 10.0F);
```
Now for the final 2 things, the second to last is the enchantmentValue:

```java
EMERALD(3, 1024, 8.0F, 10.0F, 23);
```

This just determines the chance for better enchants, and the cost of them.

Finally, we set the repair material, this is just the material that is used when wanting to repair it in an anvil.

```java
EMERALD(3, 1024, 8.0F, 10.0F, 23, () -> {
  return Ingredient.of(Items.EMERALD);
});
```

To use it, just replace ``Tiers.GOLD`` with ``Tiers.EMERALD``.

## Custom Textures

If you followed the [Resources](../Resources.md) guide from before, all you'll need to do to add a texture is go to ```assets/minecraft/textures/items``` and add the texture there, after that, just replace ```"layer0": "minecraft:item/acacia_boat"``` with ```"layer0": "minecraft:item/TEXTURE_NAME"``` and it should work.

## Closing

Good job! You now know how to add fairly simple items to Minecraft.