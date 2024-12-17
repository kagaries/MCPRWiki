# Making Your First Entity

This point of the guide will focus on creating your own entity. This will teach you how to make the entity class to finishing the rendering and spawning it in-game.

First we'll need to create a entity class for us to use. For this tutorial we'll create a zombie clone with modified stats to be used. To get started, navigate to the ``net.minecraft.world.entity.monster`` package, this is where we will create our class. Now let's create a class named ``ZombieClone``, and have it extend the ``Zombie`` class. It should now look like:

```java
public class ZombieClone extends Zombie
```

You'll most likely notice the class has an error on it, for now hover over the error and auto-create the constructor matching the super:

```java
public ZombieClone(EntityType<? extends Zombie> p_34271_, Level p_34272_) {
    super(p_34271_, p_34272_);
}
```

Next we will head to the ``EntityType`` class to register this entity, when registering it you should have sonething that looks like:

```java
public static final EntityType<ZombieClone> ZOMBIE_CLONE = register("zombie_clone", Builder.<ZombieClone>of(ZombieClone::new, MobCategory.MONSTER).sized(0.6F, 1.95F).clientTrackingRange(8));
```

Now make your way back to the ``ZombieClone`` class and change the constructor ``EntityType<? extends Zombie>`` to be ``EntityType<? extends ZombieClone>`` instead. It should end up looking like this:

```java
public ZombieClone(EntityType<? extends ZombieClone> p_34271_, Level p_34272_) {
    super(p_34271_, p_34272_);
}
```

We now have a proper entity class! Here we can start adding and overriding functions, for now we will just override the ``getExperienceReward()`` function:

```java
@Override
public int getExperienceReward() {
    this.xpReward *= 10;

    return super.getExperienceReward();
}
```

Now we will have to define the attributes of the mob, so let's head over to the ``DefaultAttributes`` and add em there. All you'll need to add to the ``SUPPLIERS`` variable is:

```java
.put(EntityType.ZOMBIE_CLONE, ZombieClone.createAttributes().build())
```

This will create the attributes for our entity. Now we can go in-game and summon it! As it spawns the entity you'll notice a little thing that happens, the game crashes. This is because the game tries to render the entity without it having an actual renderer, and gets angry. The next part of this tutorial guide will teach you how to make a renderer for your mob!