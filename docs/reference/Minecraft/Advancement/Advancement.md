# Advancement

```java
public record Advancement(Optional<ResourceLocation> parent, Optional<DisplayInfo> display, AdvancementRewards rewards, Map<String, Criterion<?>> criteria, AdvancementRequirements requirements, boolean sendsTelemetryEvent, Optional<Component> name)
```

### Constructor
```java
public Advancement(Optional<ResourceLocation> p_299284_, Optional<DisplayInfo> p_301017_, AdvancementRewards p_286389_, Map<String, Criterion<?>> p_286635_, AdvancementRequirements p_300504_, boolean p_286478_) {
    this(p_299284_, p_301017_, p_286389_, Map.copyOf(p_286635_), p_300504_, p_286478_, p_301017_.map(Advancement::decorateName));
}
```

### decorateName
Decorates the name of the advancement based on the input display info

```java
private static Component decorateName(DisplayInfo p_300038_) {
    Component component = p_300038_.getTitle();
    ChatFormatting chatformatting = p_300038_.getFrame().getChatColor();
    Component component1 = ComponentUtils.mergeStyles(component.copy(), Style.EMPTY.withColor(chatformatting)).append("\n").append(p_300038_.getDescription());
    Component component2 = component.copy().withStyle((p_138316_) -> {
        return p_138316_.withHoverEvent(new HoverEvent(HoverEvent.Action.SHOW_TEXT, component1));
    });
    return ComponentUtils.wrapInSquareBrackets(component2).withStyle(chatformatting);
}
```

### name
Returns the name Component of the Advancement

```java
public static Component name(AdvancementHolder p_297556_) {
    return p_297556_.value().name().orElseGet(() -> {
        return Component.literal(p_297556_.id().toString());
    });
}
```

### serializeToJson
serializes the advancement to a json object to be used in game

```java
public JsonObject serializeToJson() {
    JsonObject jsonobject = new JsonObject();
    this.parent.ifPresent((p_296095_) -> {
        jsonobject.addProperty("parent", p_296095_.toString());
    });
    this.display.ifPresent((p_296097_) -> {
        jsonobject.add("display", p_296097_.serializeToJson());
    });
    jsonobject.add("rewards", this.rewards.serializeToJson());
    JsonObject jsonobject1 = new JsonObject();

    for(Map.Entry<String, Criterion<?>> entry : this.criteria.entrySet()) {
        jsonobject1.add(entry.getKey(), entry.getValue().serializeToJson());
    }

    jsonobject.add("criteria", jsonobject1);
    jsonobject.add("requirements", this.requirements.toJson());
    jsonobject.addProperty("sends_telemetry_event", this.sendsTelemetryEvent);
    return jsonobject;
}
```

### fromJson
Get an advancement from a json

```java
public static Advancement fromJson(JsonObject p_300691_, DeserializationContext p_300053_) {
    Optional<ResourceLocation> optional = p_300691_.has("parent") ? Optional.of(new ResourceLocation(GsonHelper.getAsString(p_300691_, "parent"))) : Optional.empty();
    Optional<DisplayInfo> optional1 = p_300691_.has("display") ? Optional.of(DisplayInfo.fromJson(GsonHelper.getAsJsonObject(p_300691_, "display"))) : Optional.empty();
    AdvancementRewards advancementrewards = p_300691_.has("rewards") ? AdvancementRewards.deserialize(GsonHelper.getAsJsonObject(p_300691_, "rewards")) : AdvancementRewards.EMPTY;
    Map<String, Criterion<?>> map = Criterion.criteriaFromJson(GsonHelper.getAsJsonObject(p_300691_, "criteria"), p_300053_);
    if (map.isEmpty()) {
        throw new JsonSyntaxException("Advancement criteria cannot be empty");
    } else {
        JsonArray jsonarray = GsonHelper.getAsJsonArray(p_300691_, "requirements", new JsonArray());
        AdvancementRequirements advancementrequirements;
        if (jsonarray.isEmpty()) {
            advancementrequirements = AdvancementRequirements.allOf(map.keySet());
        } else {
            advancementrequirements = AdvancementRequirements.fromJson(jsonarray, map.keySet());
        }

        boolean flag = GsonHelper.getAsBoolean(p_300691_, "sends_telemetry_event", false);
        return new Advancement(optional, optional1, advancementrewards, map, advancementrequirements, flag);
    }
}
```

### write
Writes the advancement info to a FriendlyByteBuf
```java
public void write(FriendlyByteBuf p_299393_) {
    p_299393_.writeOptional(this.parent, FriendlyByteBuf::writeResourceLocation);
    p_299393_.writeOptional(this.display, (p_296098_, p_296099_) -> {
        p_296099_.serializeToNetwork(p_296098_);
    });
    this.requirements.write(p_299393_);
    p_299393_.writeBoolean(this.sendsTelemetryEvent);
}
```

### read
Reads the advancement from a FriendlyByteBuf
```java
public static Advancement read(FriendlyByteBuf p_300670_) {
    return new Advancement(p_300670_.readOptional(FriendlyByteBuf::readResourceLocation), p_300670_.readOptional(DisplayInfo::fromNetwork), AdvancementRewards.EMPTY, Map.of(), new AdvancementRequirements(p_300670_), p_300670_.readBoolean());
}
```

### isRoot
Returns if this is a root advancement or not
```java
public boolean isRoot() {
    return this.parent.isEmpty();
}
```

### Builder Class

#### advancement
Returns a new Advancement Builder with a telemetry event
```java
public static Advancement.Builder advancement() {
    return (new Advancement.Builder()).sendsTelemetryEvent();
}
```

#### recipeAdvancement
Returns a new Advancement Builder
```java
public static Advancement.Builder recipeAdvancement() {
    return new Advancement.Builder();
}
```

#### parent
Sets the parent of the advancement, then returns the advancement
```java
public Advancement.Builder parent(AdvancementHolder p_300513_) {
    this.parent = Optional.of(p_300513_.id());
    return this;
}
```

#### parent (Deprecated)
Sets the parent of the advancement, then returns the advancement
```java
public Advancement.Builder parent(ResourceLocation p_138397_) {
    this.parent = Optional.of(p_138397_);
    return this;
}
```

#### display (ItemStack)
Returns the display of the advancement from a new DisplayInfo using an ItemStack
```java
public Advancement.Builder display(ItemStack p_138363_, Component p_138364_, Component p_138365_, @Nullable ResourceLocation p_138366_, FrameType p_138367_, boolean p_138368_, boolean p_138369_, boolean p_138370_) {
    return this.display(new DisplayInfo(p_138363_, p_138364_, p_138365_, p_138366_, p_138367_, p_138368_, p_138369_, p_138370_));
}
```

#### display (ItemLike)
Returns the display of the advancement from a new DisplayInfo using an ItemLike class
```java
public Advancement.Builder display(ItemLike p_138372_, Component p_138373_, Component p_138374_, @Nullable ResourceLocation p_138375_, FrameType p_138376_, boolean p_138377_, boolean p_138378_, boolean p_138379_) {
    return this.display(new DisplayInfo(new ItemStack(p_138372_.asItem()), p_138373_, p_138374_, p_138375_, p_138376_, p_138377_, p_138378_, p_138379_));
}
```

#### display (DisplayInfo)
Sets the display of the advancement from DisplayInfo, then returns the advancement
```java
public Advancement.Builder display(DisplayInfo p_138359_) {
    this.display = Optional.of(p_138359_);
    return this;
}
```

#### rewards (Builder)
Returns the rewards of the advancement from a AdvancementRewards.Builder
```java
public Advancement.Builder rewards(AdvancementRewards.Builder p_138355_) {
    return this.rewards(p_138355_.build());
}
```

#### rewards
Sets the rewards from a AdvancementRewards then returns the advancement
```java
public Advancement.Builder rewards(AdvancementRewards p_138357_) {
    this.rewards = p_138357_;
    return this;
}
```

#### addCriterion
Adds a criterion from a string and Criterion<?> object, then returns the advancement
```java
public Advancement.Builder addCriterion(String p_138384_, Criterion<?> p_138385_) {
    this.criteria.put(p_138384_, p_138385_);
    return this;
}
```

#### requirements (Strategy)
Sets the requirementsStragtegy from an AdvancementRequirements.Strategy, then returns the advancement
```java
public Advancement.Builder requirements(AdvancementRequirements.Strategy p_298091_) {
    this.requirementsStrategy = p_298091_;
    return this;
}
```

#### requirements
Sets the requriements from an AdvancementRequirements, then returns the advancement
```java
public Advancement.Builder requirements(AdvancementRequirements p_300756_) {
    this.requirements = Optional.of(p_300756_);
    return this;
}
```

#### sendsTelemetryEvent
Sets sendsTelemetryEvent to true, then returns the advancement
```java
public Advancement.Builder sendsTelemetryEvent() {
    this.sendsTelemetryEvent = true;
    return this;
}
```

#### build
Builds an advancement from a resourcelocation, and the set data from the builder. Returns a new AdvanementHolder
```java
public AdvancementHolder build(ResourceLocation p_138404_) {
    Map<String, Criterion<?>> map = this.criteria.buildOrThrow();
    AdvancementRequirements advancementrequirements = this.requirements.orElseGet(() -> {
        return this.requirementsStrategy.create(map.keySet());
    });
    return new AdvancementHolder(p_138404_, new Advancement(this.parent, this.display, this.rewards, map, advancementrequirements, this.sendsTelemetryEvent));
}
```

#### save
Builds a AdvancementHolder from a String, and accepts the Advancement
```java
public AdvancementHolder save(Consumer<AdvancementHolder> p_138390_, String p_138391_) {
    AdvancementHolder advancementholder = this.build(new ResourceLocation(p_138391_));
    p_138390_.accept(advancementholder);
    return advancementholder;
}
```