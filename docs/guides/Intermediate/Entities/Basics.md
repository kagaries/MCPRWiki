# Basics

This guide will go over the basic things you'll need for creating your own mobs moving foward.

## Entity Classes

Entity Logic is contained within classes. These classes usually extend a different **LivingEntity** class:

```java
public abstract class Mob extends LivingEntity implements Targeting
```
_This is the Mob class which the PathfinderMob uses_

Entities will usually extend something like the 'Monster' class or the 'Animal' class. These have many of the important things for the entitiy to use (like Pathfinding, important methods, and other things of the such). These classes are used in the EntityType class to register the entity to be able to be used later in a Renderer or spawning.

## EntityType (Entity Registering)

When you create an entity class, the game doesn't automagically know that entity exists. The way the game is able to know that entity exists and use that entity in spawning, rendering, etc. is through **Registering**. Registering an entity is fairly simple, you'll need to know a couple of things before creating yours though. 

First, you'll want to have an entity class prepared, this is required since we need to create a new one when registering:

```java
public static final EntityType<Piglin> PIGLIN = register("piglin", EntityType.Builder.of(Piglin::new));
```

Second, We'll need to know what kind of entity this is so we can put it into the correct MobCategory:

```java
public static final EntityType<Piglin> PIGLIN = register("piglin", EntityType.Builder.of(Piglin::new, MobCategory.MONSTER));
```

Finally we'll want to know the dimensions of our entity so we can have a correctly sized hitbox when using .sized(x, y), and the client tracking range we want to use (8 is fine for most mobs). Putting all of that together looks about something like this: 

```java
public static final EntityType<Piglin> PIGLIN = register("piglin", EntityType.Builder.of(Piglin::new, MobCategory.MONSTER).sized(0.6F, 1.95F).clientTrackingRange(8));
```

This will again allow the mob to be used elsewhere.

## Renderering

Entities do not automatically have a way to render from just their class. This is where Renderers come in to allow us to render our entities. Renderers are located in the client package of net.minecraft and contains all of the rendering of literally everything. Later in the Entity Guides we will go over them, but they need a guide to themselves.

## Conclusion

Put simply, Entities exist when you actually register them, they aren't too hard, but we'll learn how to make different kinds of entities later.