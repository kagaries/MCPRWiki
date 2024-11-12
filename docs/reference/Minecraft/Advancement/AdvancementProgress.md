# Advancement Progress

```java
public class AdvancementProgress implements Comparable<AdvancementProgress>
```

### Constructor
```java
private AdvancementProgress(Map<String, CriterionProgress> p_144358_) {
    this.criteria = p_144358_;
}
```

### Constructer (Public)
```java
public AdvancementProgress() {
    this.criteria = Maps.newHashMap();
}
```

### update
```java
public void update(AdvancementRequirements p_300626_) {
    Set<String> set = p_300626_.names();
    this.criteria.entrySet().removeIf((p_8203_) -> {
        return !set.contains(p_8203_.getKey());
    });

    for(String s : set) {
        this.criteria.putIfAbsent(s, new CriterionProgress());
    }

    this.requirements = p_300626_;
}
```

### isDone
```java
public boolean isDone() {
    return this.requirements.test(this::isCriterionDone);
}
```

### hasProgress
```java
public boolean hasProgress() {
    for(CriterionProgress criterionprogress : this.criteria.values()) {
        if (criterionprogress.isDone()) {
            return true;
        }
    }

    return false;
}
```

### grantProgress
```java
public boolean grantProgress(String p_8197_) {
    CriterionProgress criterionprogress = this.criteria.get(p_8197_);
    if (criterionprogress != null && !criterionprogress.isDone()) {
        criterionprogress.grant();
        return true;
    } else {
        return false;
    }
}
```

### revokeProgress
```java
public boolean revokeProgress(String p_8210_) {
    CriterionProgress criterionprogress = this.criteria.get(p_8210_);
    if (criterionprogress != null && criterionprogress.isDone()) {
        criterionprogress.revoke();
        return true;
    } else {
        return false;
    }
}
```

### toString
```java
public String toString() {
    return "AdvancementProgress{criteria=" + this.criteria + ", requirements=" + this.requirements + "}";
}
```

### serializeToNetwork
```java
public void serializeToNetwork(FriendlyByteBuf p_8205_) {
    p_8205_.writeMap(this.criteria, FriendlyByteBuf::writeUtf, (p_144360_, p_144361_) -> {
        p_144361_.serializeToNetwork(p_144360_);
    });
}
```

### fromNetwork
```java
public static AdvancementProgress fromNetwork(FriendlyByteBuf p_8212_) {
    Map<String, CriterionProgress> map = p_8212_.readMap(FriendlyByteBuf::readUtf, CriterionProgress::fromNetwork);
    return new AdvancementProgress(map);
}
```

### getCriterion
```java
@Nullable
public CriterionProgress getCriterion(String p_8215_) {
    return this.criteria.get(p_8215_);
}
```

### isCriterionDone
```java
private boolean isCriterionDone(String p_301316_) {
    CriterionProgress criterionprogress = this.getCriterion(p_301316_);
    return criterionprogress != null && criterionprogress.isDone();
}
```

### getPercent
```java
public float getPercent() {
    if (this.criteria.isEmpty()) {
        return 0.0F;
    } else {
        float f = (float)this.requirements.size();
        float f1 = (float)this.countCompletedRequirements();
        return f1 / f;
    }
}
```

### getProgressText
```java
@Nullable
public Component getProgressText() {
    if (this.criteria.isEmpty()) {
        return null;
    } else {
        int i = this.requirements.size();
        if (i <= 1) {
            return null;
        } else {
            int j = this.countCompletedRequirements();
            return Component.translatable("advancements.progress", j, i);
        }
    }
}
```

### countCompletedRequirements
```java
private int countCompletedRequirements() {
    return this.requirements.count(this::isCriterionDone);
}
```

### getRemainingCriteria
```java
public Iterable<String> getRemainingCriteria() {
    List<String> list = Lists.newArrayList();

    for(Map.Entry<String, CriterionProgress> entry : this.criteria.entrySet()) {
         f (!entry.getValue().isDone()) {
            list.add(entry.getKey());
        }
    }

    return list;
}
```

### getCompletedCriteria
```java
public Iterable<String> getCompletedCriteria() {
    List<String> list = Lists.newArrayList();

    for(Map.Entry<String, CriterionProgress> entry : this.criteria.entrySet()) {
        if (entry.getValue().isDone()) {
            list.add(entry.getKey());
        }
    }

    return list;
}
```

### getFirstProgressDate
```java
@Nullable
public Instant getFirstProgressDate() {
    return this.criteria.values().stream().map(CriterionProgress::getObtained).filter(Objects::nonNull).min(Comparator.naturalOrder()).orElse((Instant)null);
}

public int compareTo(AdvancementProgress p_8195_) {
    Instant instant = this.getFirstProgressDate();
    Instant instant1 = p_8195_.getFirstProgressDate();
    if (instant == null && instant1 != null) {
        return 1;
    } else if (instant != null && instant1 == null) {
        return -1;
    } else {
        return instant == null && instant1 == null ? 0 : instant.compareTo(instant1);
    }
}
```