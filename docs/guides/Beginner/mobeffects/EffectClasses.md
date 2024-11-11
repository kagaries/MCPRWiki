# Effect Classes

In this guide, you'll learn to make make custom class effect like ``RegenerationMobEffect``.

## Creating the Class

To start go to the effect folder in world, and create a class named ``TestMobEffect``. First we need to have the class extend MobEffect:

```java
public class TestMobEffect extends MobEffect
```

Now it'll start erroring, we'll need to have a constructor for it to stop being mad, if you're using IntelliJ you can hover over it and it'll do it for you, but you can manually add it:

```java
protected TestMobEffect(MobEffectCategory p_19451_, int p_19452_) {
    super(p_19451_, p_19452_);
}
```

Nice! Now we can start adding the methods used for healing the player. First add:

```java
public void applyEffectTick(LivingEntity p_301282_, int p_300945_) {
    super.applyEffectTick(p_301282_, p_300945_);
}
```

This is where the effect will tick its healing, to get started adding that start with:

```java
public void applyEffectTick(LivingEntity p_301282_, int p_300945_) {
    super.applyEffectTick(p_301282_, p_300945_);
    if (p_301282_.getHealth() < p_301282_.getMaxHealth()) {
    
    }
}
```

This will prevent us from trying to heal at or above the entity's max health. Next we'll just need to add:

```java
public void applyEffectTick(LivingEntity entity, int p_300945_) {
    super.applyEffectTick(p_301282_, p_300945_);
    if (p_301282_.getHealth() < p_301282_.getMaxHealth()) {
        p_301282_.heal(2.0F);
    }
}
```

And any entity with this effect will now health a full heart of health! But how fast will it do that? It'll do that every tick since we haven't add one thing yet, let's add that now:

```java
public boolean shouldApplyEffectTickThisTick(int tickCount, int amplifier) {

}
```

This is what will determine if it should heal this tick or not. We can start by adding:

```java
public boolean shouldApplyEffectTickThisTick(int tickCount, int amplifier) {
    int i = 35 >> amplifier;
}
```

This will determine the ticks inbetween the effect being applied. Now we add the conditional:

```java
public boolean shouldApplyEffectTickThisTick(int tickCount, int amplifier) {
    int i = 35 >> amplifier;
    if (i > 0) {
        return tickCount % i == 0;
    } else {
        return true;
    }
}
```

Now that we have the effect class created, we can create an effect with it!

## Creating the Effect

If you followed the [Getting Started](../../Basics/FirstModification.md) guide, you'll know how you created an effect in there, we'll just create a basic effect like:

```java
public static final MobEffect NAME = register("name", (new MobEffect(MobEffectCategory.BENEFICIAL, 3402751)));
```

Now that we have this we'll want to change the ``(new MobEffect())`` to ``(new TestMobEffect())`` keeping all the values inside the same, so it should look like:

```java
public static final MobEffect NAME = register("name", (new TestMobEffect(MobEffectCategory.BENEFICIAL, 3402751)));
```

You can now give yourself the effect and start healing!

## Closing

Creating effect classes is very simple, later we'll take about how effects can mess with your vision and that kind of stuff.