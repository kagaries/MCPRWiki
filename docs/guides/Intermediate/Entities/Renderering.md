# Renderering

As stated in the previous guide ([Basics](./Basics.md)), entities do not automagically know how to render themselves. You will need to create your own renderer file so you can render your own mob.

## Creating Your First Renderer

 Navigate yourself over to the ``net.minecraft.client.renderer.entity`` package, this is where we will start on our rendering class. Let's create the ``ZombieCloneRenderer`` class to get started, since we are just making a clone of the zombie, let's have it extend the ``AbstractZombieRenderer`` with ``<ZombieClone, ZombieModel<ZombieClone>>``. After that you should have a class that looks like:

```java
@OnlyIn(Dist.CLIENT)
public class ZombieCloneRenderer extends AbstractZombieRenderer<ZombieClone, ZombieModel<ZombieClone>>{
    public ZombieCloneRenderer(EntityRendererProvider.Context p_174456_) {
        this(p_174456_, ModelLayers.ZOMBIE, ModelLayers.ZOMBIE_INNER_ARMOR, ModelLayers.ZOMBIE_OUTER_ARMOR);
    }

    public ZombieCloneRenderer(EntityRendererProvider.Context p_174458_, ModelLayerLocation p_174459_, ModelLayerLocation p_174460_, ModelLayerLocation p_174461_) {
        super(p_174458_, new ZombieModel<>(p_174458_.bakeLayer(p_174459_)), new ZombieModel<>(p_174458_.bakeLayer(p_174460_)), new ZombieModel<>(p_174458_.bakeLayer(p_174461_)));
    }
}
```

This will tell the game _how_ to render our mob, and when we register it will stop the crashing from earlier.

Now that we have created our renderer, we need to go to the ``EntityRenderers`` class. In here you should scroll down until you see:

```java
static {
    register(EntityType.ALLAY, AllayRenderer::new);
    register(EntityType.AREA_EFFECT_CLOUD, NoopRenderer::new);
    register(EntityType.ARMOR_STAND, ArmorStandRenderer::new);
    register(EntityType.ARROW, TippableArrowRenderer::new);
    register(EntityType.AXOLOTL, AxolotlRenderer::new);
    register(EntityType.BAT, BatRenderer::new);
    register(EntityType.BEE, BeeRenderer::new);
    register(EntityType.BLAZE, BlazeRenderer::new);
    register(EntityType.BLOCK_DISPLAY, DisplayRenderer.BlockDisplayRenderer::new);
    ...
}
```

Under the ``register(EntityType.ZOMBIE, ZombieRenderer::new);`` we'll add the line:

```java
register(EntityType.ZOMBIE_CLONE, ZombieCloneRenderer::new);
```

You can now go in-game and your mob will now work!