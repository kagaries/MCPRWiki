# Advancement Requirements

```java
public record AdvancementRequirements(String[][] requirements)
```

### Constructor

```java
public AdvancementRequirements(FriendlyByteBuf friendlyByteBuf) {
    this(read(friendlyByteBuf));
}
```

### read

```java
private static String[][] read(FriendlyByteBuf friendlyByteBuf) {
    String[][] astring = new String[friendlyByteBuf.readVarInt()][];

    for(int i = 0; i < astring.length; ++i) {
        astring[i] = new String[friendlyByteBuf.readVarInt()];

        for(int j = 0; j < astring[i].length; ++j) {
            astring[i][j] = friendlyByteBuf.readUtf();
        }
    }

    return astring;
}
```