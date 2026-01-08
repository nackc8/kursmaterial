# Objektorienterad programmering: Övningsuppgifter

Målet med övningarna är att träna de viktigaste OO-idéerna i Java med så lite “ramverk” som möjligt.

## Inför övningsuppgifterna

### Att göra i varje övning

- Läs felmeddelanden noga och fixa en sak i taget.
- Kör programmet ofta och skriv ut mellanresultat.
- Använd debugläge. Sätt breakpoints och kontrollera fältvärden före/efter metodanrop.

### Kodstil (enkel tumregel)

- Fält ska nästan alltid vara `private`.
- Undvik “setter på allt”. Lägg hellre metoder som gör en tydlig sak.
- Validera input tidigt med guard clauses och kasta `IllegalArgumentException`.

## Övningsuppgifter

### Övning 1: Inkapsling och invariants (HP får aldrig bli negativt)

Skapa en klass `Health` som kapslar in en spelares liv.

**Krav:**

- `Health` ska ha ett privat fält `hp`.
- Konstruktor tar `int initialHp`.
- Metoder:
  - `void takeDamage(int amount)`
  - `void heal(int amount)`
  - `int currentHp()`
  - `boolean isAlive()`
- Invariant: `hp` ska alltid vara `>= 0`.
- `amount` måste vara `> 0` annars `IllegalArgumentException`.

**Exempel:**

- `Health(10)`
- `takeDamage(3)` → 7
- `takeDamage(100)` → 0 (inte negativt)
- `heal(5)` → 5

### Övning 2: Interface + polymorfism (Explosion som funkar för flera typer)

Skapa ett interface `Damageable` och tre typer som implementerar det.

**Krav:**

- `Damageable`:
  - `void takeDamage(int amount)`
  - `boolean isAlive()`
- Implementera:
  - `Player` (har `Health`)
  - `Enemy` (har `Health`)
  - `Crate` (har t.ex. `durability` som går ned till 0)
- Skriv en metod:
  - `static void explosion(List<Damageable> targets, int damage)`
  - Den ska loopa och anropa `takeDamage(damage)` på alla.

**Bonus:**

Skriv ut vilka som fortfarande “lever” efter explosionen.

### Övning 3: Komposition + delegation (Player har en Weapon)

Träna “has-a” och att vidarebefordra arbete.

**Krav:**

- Skapa ett interface `Weapon`:
  - `int damage()`
  - `String name()`
- Implementera två vapen:
  - `Sword` (t.ex. damage 7)
  - `Bow` (t.ex. damage 4)
- Uppdatera `Player` så att den har ett privat fält `Weapon weapon`.
- `Player.attack(Damageable target)` ska delegera skadan:
  - hämta skada via `weapon.damage()`
  - anropa `target.takeDamage(...)`

**Valfritt:**

Lägg till `equip(Weapon w)` för att byta vapen.

### Övning 4: Arv (Character som basklass)

Träna “är-en” och återanvändning av gemensam kod.

**Krav:**

- Skapa en basklass `Character` med:
  - fält för `name` och `Health`
  - metod `String name()`
  - metod `boolean isAlive()`
  - metod `void takeDamage(int amount)`
- Skapa subklasser:
  - `Hero extends Character`
  - `Monster extends Character`
- Lägg till `int baseDamage()` i `Character` och override i minst en subklass.
- Skriv kod som håller en `List<Character>` med både `Hero` och `Monster` och summerar deras `baseDamage()`.

**Reflektion (1–2 meningar i kommentar):**

Är `Hero` verkligen en `Character` och varför?

### Övning 5: Abstraktion med interface (Spara spelet)

Träna “stabilt API” och att dölja detaljer.

**Krav:**

- Skapa ett interface `SaveStorage`:
  - `void save(String slot, String data)`
  - `String load(String slot)`
- Implementera två varianter:
  - `InMemorySaveStorage` (använd `HashMap<String,String>`)
  - `FileSaveStorage` (spara till filer med `Files.writeString` / `Files.readString`)
- Skapa en klass `Game` som tar `SaveStorage` i konstruktorn och har metoder:
  - `void saveGame(String slot)`
  - `void loadGame(String slot)`
- `Game` ska inte veta om lagringen är i minne eller fil.

### Övning 6: Kontrakt och exceptions (Inventory)

Träna preconditions och “fail early”.

**Krav:**

- Skapa en klass `Inventory` som håller en `Map<String, Integer>` av item → antal.
- Metoder:
  - `void add(String itemId, int quantity)`
  - `void remove(String itemId, int quantity)`
  - `int countOf(String itemId)`
- Preconditions:
  - `itemId` får inte vara null
  - `quantity` måste vara `> 0`
- Om man försöker ta bort fler än man har ska du kasta `IllegalStateException`.

### (Valfri) Övning 7: equals/hashCode (Position som value object)

**Krav:**

- Skapa en `record Position(int x, int y)`.
- Lägg `Position` i en `HashSet<Position>` och visa att `contains(new Position(...))` fungerar.

## Lösningsförslag

> Obs: Detta är exempel-lösningar. Det finns flera rimliga varianter.

### Lösning 1: Health

```java
public final class Health {
    private int hp;

    public Health(int initialHp) {
        if (initialHp < 0) throw new IllegalArgumentException("initialHp must be >= 0");
        this.hp = initialHp;
    }

    public void takeDamage(int amount) {
        if (amount <= 0) throw new IllegalArgumentException("amount must be > 0");
        hp = Math.max(0, hp - amount);
    }

    public void heal(int amount) {
        if (amount <= 0) throw new IllegalArgumentException("amount must be > 0");
        hp += amount;
    }

    public int currentHp() { return hp; }

    public boolean isAlive() { return hp > 0; }
}
```

### Lösning 2: Damageable + explosion

```java
public interface Damageable {
    void takeDamage(int amount);
    boolean isAlive();
}
```

```java
public final class Player implements Damageable {
    private final Health health;

    public Player(int hp) { this.health = new Health(hp); }

    @Override public void takeDamage(int amount) { health.takeDamage(amount); }

    @Override public boolean isAlive() { return health.isAlive(); }
}
```

```java
public final class Enemy implements Damageable {
    private final Health health;

    public Enemy(int hp) { this.health = new Health(hp); }

    @Override public void takeDamage(int amount) { health.takeDamage(amount); }

    @Override public boolean isAlive() { return health.isAlive(); }
}
```

```java
public final class Crate implements Damageable {
    private int durability;

    public Crate(int durability) {
        if (durability < 0) throw new IllegalArgumentException("durability must be >= 0");
        this.durability = durability;
    }

    @Override public void takeDamage(int amount) {
        if (amount <= 0) throw new IllegalArgumentException("amount must be > 0");
        durability = Math.max(0, durability - amount);
    }

    @Override public boolean isAlive() { return durability > 0; }
}
```

```java
import java.util.List;

public final class Effects {
    public static void explosion(List<Damageable> targets, int damage) {
        if (damage <= 0) throw new IllegalArgumentException("damage must be > 0");
        for (Damageable t : targets) {
            t.takeDamage(damage);
        }
    }
}
```

### Lösning 3: Weapon + Player.attack (komposition + delegation)

```java
public interface Weapon {
    int damage();
    String name();
}

public final class Sword implements Weapon {
    @Override public int damage() { return 7; }
    @Override public String name() { return "Sword"; }
}

public final class Bow implements Weapon {
    @Override public int damage() { return 4; }
    @Override public String name() { return "Bow"; }
}
```

En enkel variant är att uppdatera `Player` och lägga till ett vapen + `attack`:

```java
import java.util.Objects;

public final class Player implements Damageable {
    private final Health health;
    private Weapon weapon;

    public Player(int hp, Weapon weapon) {
        this.health = new Health(hp);
        this.weapon = Objects.requireNonNull(weapon);
    }

    public void equip(Weapon weapon) {
        this.weapon = Objects.requireNonNull(weapon);
    }

    public void attack(Damageable target) {
        Objects.requireNonNull(target);
        target.takeDamage(weapon.damage()); // delegation: skadan bestäms av vapnet
    }

    @Override public void takeDamage(int amount) { health.takeDamage(amount); }
    @Override public boolean isAlive() { return health.isAlive(); }
}
```

### Lösning 4: Arv (Character -> Hero/Monster)

```java
import java.util.Objects;

public class Character {
    private final String name;
    private final Health health;

    public Character(String name, int hp) {
        this.name = Objects.requireNonNull(name);
        this.health = new Health(hp);
    }

    public String name() { return name; }
    public boolean isAlive() { return health.isAlive(); }
    public void takeDamage(int amount) { health.takeDamage(amount); }

    public int baseDamage() { return 3; }
}
```

```java
public final class Hero extends Character {
    public Hero(String name, int hp) { super(name, hp); }
    @Override public int baseDamage() { return 5; }
}

public final class Monster extends Character {
    public Monster(String name, int hp) { super(name, hp); }
    @Override public int baseDamage() { return 4; }
}
```

```java
import java.util.List;

public final class DemoInheritance {
    public static void main(String[] args) {
        List<Character> party = List.of(
                new Hero("Pekka", 20),
                new Monster("Slime", 10)
        );

        int total = party.stream().mapToInt(Character::baseDamage).sum();
        System.out.println(total);
    }
}
```

### Lösning 5: SaveStorage (abstraktion)

```java
public interface SaveStorage {
    void save(String slot, String data);
    String load(String slot);
}
```

```java
import java.util.HashMap;
import java.util.Map;
import java.util.Objects;

public final class InMemorySaveStorage implements SaveStorage {
    private final Map<String, String> saves = new HashMap<>();

    @Override public void save(String slot, String data) {
        saves.put(Objects.requireNonNull(slot), Objects.requireNonNull(data));
    }

    @Override public String load(String slot) {
        return saves.get(Objects.requireNonNull(slot));
    }
}
```

```java
import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Path;
import java.util.Objects;

public final class FileSaveStorage implements SaveStorage {
    private final Path baseDir;

    public FileSaveStorage(Path baseDir) {
        this.baseDir = Objects.requireNonNull(baseDir);
    }

    @Override public void save(String slot, String data) {
        try {
            Files.createDirectories(baseDir);
            Files.writeString(baseDir.resolve(Objects.requireNonNull(slot) + ".txt"),
                    Objects.requireNonNull(data));
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }

    @Override public String load(String slot) {
        try {
            return Files.readString(baseDir.resolve(Objects.requireNonNull(slot) + ".txt"));
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }
}
```

```java
import java.util.Objects;

public final class Game {
    private final SaveStorage storage;
    private int level = 1;
    private int gold = 0;

    public Game(SaveStorage storage) {
        this.storage = Objects.requireNonNull(storage);
    }

    public void saveGame(String slot) {
        storage.save(slot, level + "," + gold);
    }

    public void loadGame(String slot) {
        String data = storage.load(slot);
        if (data == null) throw new IllegalStateException("no save in slot " + slot);

        String[] parts = data.split(",");
        level = Integer.parseInt(parts[0]);
        gold  = Integer.parseInt(parts[1]);
    }
}
```

### Lösning 6: Inventory (kontrakt + exceptions)

```java
import java.util.HashMap;
import java.util.Map;
import java.util.Objects;

public final class Inventory {
    private final Map<String, Integer> items = new HashMap<>();

    public void add(String itemId, int quantity) {
        Objects.requireNonNull(itemId);
        if (quantity <= 0) throw new IllegalArgumentException("quantity must be > 0");
        items.merge(itemId, quantity, Integer::sum);
    }

    public void remove(String itemId, int quantity) {
        Objects.requireNonNull(itemId);
        if (quantity <= 0) throw new IllegalArgumentException("quantity must be > 0");

        int current = items.getOrDefault(itemId, 0);
        if (current < quantity) throw new IllegalStateException("not enough items");

        int left = current - quantity;
        if (left == 0) items.remove(itemId);
        else items.put(itemId, left);
    }

    public int countOf(String itemId) {
        Objects.requireNonNull(itemId);
        return items.getOrDefault(itemId, 0);
    }
}
```

### (Valfri) Lösning 7: Position (record ger equals/hashCode)

```java
import java.util.HashSet;

public record Position(int x, int y) {
    public static void main(String[] args) {
        var set = new HashSet<Position>();
        set.add(new Position(1, 2));

        System.out.println(set.contains(new Position(1, 2))); // true
    }
}
```
