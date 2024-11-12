# Advancement Node

```java
public class AdvancementNode
```

### Constructor
```java
@VisibleForTesting
public AdvancementNode(AdvancementHolder p_300583_, @Nullable AdvancementNode p_299774_) {
    this.holder = p_300583_;
    this.parent = p_299774_;
}
```

### advancement
```java
public Advancement advancement() {
    return this.holder.value();
}
```

### holder
```java
public AdvancementHolder holder() {
    return this.holder;
}
```

### parent
```java
@Nullable
public AdvancementNode parent() {
    return this.parent;
}
```

### root
```java
public AdvancementNode root() {
    return getRoot(this);
}
```

### getRoot
```java
public static AdvancementNode getRoot(AdvancementNode p_300357_) {
    AdvancementNode advancementnode = p_300357_;

    while(true) {
        AdvancementNode advancementnode1 = advancementnode.parent();
        if (advancementnode1 == null) {
            return advancementnode;
        }

        advancementnode = advancementnode1;
    }
}
```

### children
```java
public Iterable<AdvancementNode> children() {
    return this.children;
}
```

### addChild
```java
@VisibleForTesting
public void addChild(AdvancementNode p_298204_) {
    this.children.add(p_298204_);
}
```

### equals
```java
public boolean equals(Object p_297253_) {
    if (this == p_297253_) {
        return true;
    } else {
        if (p_297253_ instanceof AdvancementNode) {
            AdvancementNode advancementnode = (AdvancementNode)p_297253_;
            if (this.holder.equals(advancementnode.holder)) {
               return true;
            }
        }

        return false;
    }
}
```

### hashCode
```java
public int hashCode() {
    return this.holder.hashCode();
}
```

### toString
```java
public String toString() {
    return this.holder.id().toString();
}
```