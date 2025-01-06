# Python till Java

## Kommentarer

### Python

- Enradiga kommentarer skrivs med `#`.

  ```python
  # Detta är en kommentar
  ```

- Flerradiga kommentarer skrivs med trippel-citattecken, antingen `"""` eller `'''`.

  ```python
  """
  Detta är en
  flerradig kommentar
  """
  ```

### Java

- Enradiga kommentarer skrivs med `//`.

  ```java
  // Detta är en kommentar
  ```

- Flerradiga kommentarer skrivs med `/*` och avslutas med `*/`.

  ```java
  /*
   Detta är en
   flerradig kommentar
  */
  ```

- Dokumentationskommentarer skrivs med `/**` och avslutas med `*/`.

  ```java
  /**
   * Detta är en dokumentationskommentar som används för
   * att skapa API-dokumentation med Javadoc.
   */
  ```

### Skillnader

- Python använder `#` för enradiga kommentarer, medan Java använder `//`.
- Python använder trippel-citattecken för flerradiga kommentarer, medan Java använder `/* */`.
- Java har särskilda dokumentationskommentarer (`/** */`), vilket Python saknar.
- Python-kommentarer är ofta enklare och används främst för förklaringar i koden. Java-kommentarer kan också generera dokumentation med verktyg som Javadoc.

## Datatyper

### Översikt över datatyper i Python och Java

| Begrepp          | Python                   | Java Primitiva Typer                                              | Java wrapperklasser                               |
| ---------------- | ------------------------ | ----------------------------------------------------------------- | ------------------------------------------------- |
| Heltal           | `int`                    | `byte` (8-bit), `short` (16-bit), `int` (32-bit), `long` (64-bit) | `Byte`, `Short`, `Integer`, `Long`                |
| Flyttal          | `float` (64-bit)         | `float` (32-bit), `double` (64-bit)                               | `Float`, `Double`                                 |
| Dubbel Precision | Ej separat typ           | `double` (64-bit)                                                 | `Double`                                          |
| Tecken/Strängar  | Ingen `char`, bara `str` | `char` (16-bit Unicode)                                           | `Character`, `String`                             |
| Booleska värden  | `bool`                   | `boolean`                                                         | `Boolean`                                         |
| Null/None        | `None`                   | `null`                                                            | Ej tillämpligt                                    |
| Arrayer          | `list`                   | Fast storlek, t.ex. int[], String[]                               | Dynamiska alternativ som List (ej wrapperklasser) |

### Varför har Java wrapperklasser och inte Python?

- **Python** använder **dynamisk typning**, där variabelns typ bestäms under körning:

    ```python
    x = 10        # int
    x = "hello"   # str
    ```

    Variabler i Python är egentligen pekare till objekt, och alla värden behandlas som objekt i bakgrunden.

- **Java** använder **statisk typning**, där variabelns typ måste anges vid deklarationen:

    ```java
    int x = 10;        // Primitiv
    String s = "hello"; // Objekt
    ```

    Java skiljer mellan primitiva typer och objekt för att optimera prestanda och minnesanvändning.

#### Primitiva typer och objekt i Java

Java har två kategorier av datatyper:

1. **Primitiva typer** (t.ex. `int`, `double`, `boolean`) som lagras direkt i minnet och är mycket snabba.
2. **Objekt** (t.ex. `String`, `Integer`, `Boolean`) som hanteras via referenser och ger fler funktioner men är långsammare.

Eftersom Java kräver strikt typning och ibland behöver använda objekt där primitiva typer inte är tillåtna (t.ex. i samlingar som `ArrayList`), introducerades **wrapperklasser** som en brygga mellan primitiva typer och objekt.

```java
int x = 10;            // Primitiv
Integer y = Integer.valueOf(x); // Wrapperklass (objekt)
System.out.println(y.bitCount(x)); // Motsvarar Python-metoden bit_length()
```

#### Python behandlar allt som objekt och har dynamisk typning

Python saknar behovet av wrapperklasser eftersom alla datatyper redan är objekt.

```python
x = 10       # Int är ett objekt
x.bit_length()       # Metod för heltal
```

Här är `x` redan ett objekt och kan därför använda metoder direkt, till skillnad från Java där primitiva typer saknar metoder.

#### Autoboxing och unboxing i Java

För att förenkla övergången mellan primitiva typer och wrapperklasser har Java stöd för:

- **Autoboxing**: Konvertering av en primitiv typ till en wrapperklass automatiskt:

    ```java
    Integer x = 10;  // int blir Integer
    ```

- **Unboxing**: Konvertering av en wrapperklass tillbaka till en primitiv typ:

    ```java
    int y = x;  // Integer blir int
    ```

### Detaljerade exempel

#### Heltal (integer)

##### Python

```python
x = 10          # int
y = 2_000_000   # Stora tal stöds direkt
```

##### Java

```java
byte a = 127;             // 8-bit
short b = 32000;          // 16-bit
int x = 10;               // 32-bit
long y = 2_000_000_000L;  // 64-bit
Integer z = 2000000;      // Wrapperklass
```

##### Skillnad

- Python stöder stora heltal direkt utan att behöva en annan typ.
- Java har specifika typer beroende på storlek och använder wrapperklasser för flexibilitet i samlingar eller när objekt krävs.

#### Flyttal och dubbel precision

##### Python

```python
x = 3.14      # float (64-bit)
y = 1.2e-10   # Vetenskaplig notation
```

##### Java

```java
float x = 3.14f;     // 32-bit
float y = 1.2e-10f;  // Vetenskaplig notation

double z = 3.14;     // 64-bit
```

##### Skillnad

- Python använder alltid 64-bit för flyttal (motsvarar Java's double).
- Java skiljer mellan `float` (32-bit) och `double` (64-bit) för att optimera prestanda och precision.
- I Java måste ett `float`-värde ha ett 'f' eller 'F' suffix. De är exakt samma sak, men gemenen 'f' används oftast av konvention. Gemena bokstaven 'f' är vanligast och används av konvention, medan 'F' kan användas för tydlighet om det behövs.

#### Tecken och strängar

##### Python

```python
flag = True    # eller False
```

##### Java

```java
boolean flag = true;     // Primitiv
Boolean valid = false;   // Wrapperklass
```

##### Skillnad

- Python har bara `bool`, som egentligen är en underklass till `int`.
- Java skiljer mellan en primitiv `boolean` och en wrapperklass `Boolean`.

#### `null`-värden

##### Python

```python
x = None
```

##### Java

```java
String s = null;   // Kan bara användas för objekt
int n = 0;         // Primitiv kan inte vara null
```

##### Skillnad

- Python använder `None` som ett universellt null-värde för alla typer.
- Java använder `null` enbart för objekt, medan primitiva typer alltid måste ha ett giltigt värde.

### Arrayer (endast Java)

#### Python

Python har ingen direkt motsvarighet till arrayer, men listor är det närmaste alternativet. Listor är flexibla och används för att lagra en mängd element vars ordning har betydelse.

**Enkel lista:**

```python
numbers = [1, 2, 3, 4, 5]
names = ["Alice", "Bob"]
print(numbers[0])  # Utmatning: 1
```

**Listor med flera dimensioner:**

```python
matrix = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
]
print(matrix[1][2])  # Utmatning: 6
```

**Tom lista med storlek:**

```python
data = [0] * 5  # Skapar en lista med 5 element
```

#### Java

Java använder arrayer för att lagra en mängd element vars ordning har betydelse och där alla element är av samma typ med en fast storlek. Java har också dynamiska samlingstyper, såsom `List`.

**Enkel array:**

```java
int[] numbers = {1, 2, 3, 4, 5};
String[] names = {"Alice", "Bob"};
System.out.println(numbers[0]); // Utmatning: 1
```

**Multidimensionell array:**

```java
int[][] matrix = {
    {1, 2, 3},
    {4, 5, 6},
    {7, 8, 9}
};
System.out.println(matrix[1][2]); // Utmatning: 6
```

**Tom array med storlek:**

```java
int[] data = new int[5]; // Skapar en array med plats för 5 element
```

### Skillnader mellan Python och Java

- Python-listor är dynamiska och kan innehålla element av olika typer.
- Java-arrayer har fast storlek och kan endast innehålla element av samma typ.
- Python har ingen direkt motsvarighet till arrayer, men listor används ofta på samma sätt.

## Villkorssatser och kodblock

### `if`-satser

#### Python

```python
x = 10
if x > 5:
    print("Större än 5")
elif x == 5:
    print("Lika med 5")
else:
    print("Mindre än 5")
```

#### Java

```java
int x = 10;
if (x > 5) {
    System.out.println("Större än 5");
} else if (x == 5) {
    System.out.println("Lika med 5");
} else {
    System.out.println("Mindre än 5");
}
```

#### Skillnad

- Python använder indrag för att markera kodblock, medan Java använder klammerparenteser `{}`.
- I Java måste parenteser `( )` omsluta villkoret.

### Ternära operatorer

#### Python

```python
x = 10
y = "Större än 5" if x > 5 else "5 eller mindre"
print(y)
```

#### Java

```java
int x = 10;
String y = (x > 5) ? "Större än 5" : "5 eller mindre";
System.out.println(y);
```

#### Skillnad

- Java använder `? :` som syntax för ternära operatorer, medan Python använder `if ... else`.
- Python tillåter fler uttryck i den ternära operationen än Java.

### Scope och risk för överskrivning

#### Python

I Python har block som definieras av indrag (t.ex. if-satser, loopar) inte ett eget scope. Variabler som skapas inuti dessa block är fortfarande tillgängliga utanför blocket.

```python
x = 5  # Variabel deklarerad utanför blocket

if True:
    x = 10  # Skriver över den tidigare variabeln x

print(x)  # Utmatning: 10
```

Här har variabeln `x` som deklarerades utanför blocket blivit skriven över av en ny tilldelning inuti blocket. Det kan vara lätt att av misstag ändra ett värde utan att inse det, speciellt i större kodbaser.

#### Java

I Java har block som definieras av `{}` sitt eget scope. Variabler som deklareras inuti ett block är inte tillgängliga utanför blocket.

```java
int x = 5; // Deklarerad utanför blocket

if (true) {
    int y = 10; // Ny variabel i blocket
}

System.out.println(y); // Fel: y är inte tillgänglig utanför blocket
```

**Felmeddelande:**

```plaintext
error: cannot find symbol
```

**Exempel på omdeklaration:**

```java
int x = 5; // Deklarerad utanför blocket

if (true) {
    int x = 10; // Fel: x kan inte deklareras igen i samma scope
}
```

**Felmeddelande:**

```plaintext
error: variable x is already defined
```

Java minskar risken för oavsiktliga fel genom att strikt begränsa scope för variabler inom block.

## Loopar

### Itererbara objekt

#### Python

Python kan iterera över alla objekt som är "itererbara". Ett objekt anses vara itererbart om det implementerar metoden `__iter__()` eller `__getitem__()`. Detta inkluderar listor, tupler, strängar, sets och dictionaries.

**Exempel:**

```python
# Lista
for x in [1, 2, 3]:
    print(x)

# Sträng
for char in 'abc':
    print(char)

# Dictionary
for key, value in {'a': 1, 'b': 2}.items():
    print(key, value)
```

#### Java

I Java används for-each-loopar för att iterera över samlingar och arrays, men endast objekt som implementerar gränssnittet `Iterable` kan användas direkt i sådana loopar.

```java
// Array
int[] numbers = {1, 2, 3};
for (int x : numbers) {
    System.out.println(x);
}

// Lista
List<String> list = Arrays.asList("a", "b", "c");
for (String s : list) {
    System.out.println(s);
}
```

#### Skillnad

- Python kan iterera över alla itererbara objekt utan extra import.
- Java kräver att samlingar implementerar gränssnittet `Iterable` eller använder en array.

### `for`-loopar

#### Python

```python
# Iterera över en lista
for x in [1, 2, 3]:
    print(x)

# Iterera över ett intervall
for i in range(1, 4):
    print(i)
```

#### Java

```java
// Iterera med en räknare
for (int i = 1; i < 4; i++) {
    System.out.println(i);
}
```

#### Skillnad

- Python använder "in" för att iterera över sekvenser, medan Java använder en for-each-loop för samlingar och en klassisk for-loop för räknare.

### `while`-loopar

#### Python

```python
i = 1
while i <= 3:
    print(i)
    i += 1
```

#### Java

```java
int i = 1;
while (i <= 3) {
    System.out.println(i);
    i++;
}
```

#### Skillnad

- Båda språken använder samma struktur för while-loopar, men Java kräver parenteser runt villkoret och klammerparenteser runt blocket.

### `do-while`-loopar (endast Java)

#### Java

```java
int i = 1;
do {
    System.out.println(i);
    i++;
} while (i <= 3);
```

#### Skillnad

- Python har ingen do-while-loop; denna loop körs minst en gång i Java innan villkoret testas.

### `continue` och `break`

`continue` hoppar över resten av koden i en iteration och går vidare till nästa. `break` avslutar loopen direkt.

De fungerar i alla typer av loopar, inklusive `for`-loopar, `while`-loopar och `do-while`-loopar (endast Java).

#### Python

```python
for i in range(1, 6):
    if i == 3:
        continue  # Hoppar över 3
    if i == 5:
        break  # Avslutar loopen vid 5
    print(i)
```

#### Java

```java
for (int i = 1; i <= 5; i++) {
    if (i == 3) {
        continue; // Hoppar över 3
    }
    if (i == 5) {
        break; // Avslutar loopen vid 5
    }
    System.out.println(i);
}
```

#### Skillnad

- Syntaxen för `continue` och `break` är likadan i båda språken.
- Java kräver parenteser och klammerparenteser, medan Python använder indrag.

## Typsystem

Ett typsystem styr hur datatyper hanteras, deklareras och konverteras i ett programmeringsspråk.

### Explicita och implicita typkonverteringar

#### Python

Python är dynamiskt typat, vilket innebär att variabler inte är bundna till en specifik typ och att typkonverteringar kan ske med funktioner (explicit) eller automatiskt (implicit).

**Explicit konvertering:**

```python
x = 10       # int
y = float(x) # Explicit konvertering av ett befintligt värde till float
print(y)     # Utmatning: 10.0
```

**Implicit konvertering:**

```python
x = 10       # int
y = 2.5      # float
z = x + y    # Automatisk konvertering till float
print(z)     # Utmatning: 12.5
```

#### Java

Java är statiskt typat, vilket innebär att variabler är bundna till en specifik typ.

**Syntax för explicit typkonvertering:**

Formen för en explicit typkonvertering i Java är:

```java
(targetType) value
```

Där `targetType` är den typ du vill konvertera till, och `value` är det värde som ska konverteras.

**Exempel på explicita typkonverteringar:**

```java
double a = 10.5;
int b = (int) a; // Konvertering från double till int
System.out.println(b); // Utmatning: 10

long x = 100000L;
short y = (short) x; // Konvertering från long till short
System.out.println(y); // Utmatning: 34464 (kan leda till dataförlust)

float f = 3.14f;
int i = (int) f; // Konvertering från float till int
System.out.println(i); // Utmatning: 3
```

##### Narrowing och Widening

**Narrowing (explicit konvertering):**

Narrowing används när en större datatyp konverteras till en mindre datatyp, och det kan leda till förlust av data eller precision. Detta kräver explicit typkonvertering:

```java
double value = 9.99;
int result = (int) value; // Explicit konvertering från double till int
System.out.println(result); // Utmatning: 9
```

**Widening (implicit konvertering):**

Widening sker automatiskt när en mindre datatyp konverteras till en större datatyp, eftersom ingen information kan gå förlorad:

```java
int num = 42;
double dbl = num; // Automatisk konvertering från int till double
System.out.println(dbl); // Utmatning: 42.0
```

##### Explicit typkonvertering med objekt

I Java kan objekt också konverteras mellan typer, men de måste vara relaterade genom arv eller implementering av ett gränssnitt (eng. interface).

Anta att vi har två klasser: `Bicycle` och `MountainBike`, där `MountainBike` ärver från `Bicycle`.

**Uppåtriktad konvertering (upcasting):**

```java
MountainBike mb = new MountainBike();
Bicycle b = mb; // Uppåtriktad konvertering sker automatiskt
System.out.println(b);
```

Eftersom `MountainBike` är en underklass till `Bicycle` kan en referens till `MountainBike` implicit konverteras till en referens av typen `Bicycle`. Detta kallas upcasting och är alltid säkert.

**Nedåtriktad konvertering (downcasting):**

```java
Bicycle b = new MountainBike();
MountainBike mb = (MountainBike) b; // Nedåtriktad konvertering måste vara explicit
System.out.println(mb);
```

Vid nedåtriktad konvertering (downcasting) krävs en explicit typkonvertering eftersom det inte är garanterat att objektet faktiskt är av den specifika typen. Detta kan resultera i en `ClassCastException` om konverteringen är ogiltig.

### Operatorer som kan ändra typ

| Operator                                    | Python                                                      | Java                                                  |
| ------------------------------------------- | ----------------------------------------------------------- | ----------------------------------------------------- |
| Addition `+`                                | Kan kombinera strängar och tal (med konvertering)           | Kräver explicita konverteringar för olika typer       |
| Subtraktion `-`                             | Bibehåller typ, men kan ge `float` om en operand är `float` | Kräver explicita konverteringar för olika typer       |
| Multiplikation `*`                          | Kan skapa strängrepetition om en operand är sträng          | Kräver explicita konverteringar för olika typer       |
| Division `/`                                | Konverterar till `float` automatiskt                        | Returnerar `int` om båda är `int`, annars `double`    |
| Golvdivision `//`                           | Returnerar alltid `int`                                     | Ingen motsvarighet                                    |
| Modulus `%`                                 | Resultatet har samma typ som operanderna                    | Kräver explicita konverteringar för olika typer       |
| Exponentiering `**`                         | Kan resultera i `float`                                     | Använder `Math.pow()` som returnerar `double`         |
| Bitoperatorer `&`, &#124;,`^`,`~`,`<<`,`>>` | Arbetar på bitnivå och returnerar heltal                    | Kräver explicita konverteringar för att hantera typer |

### Vad räknas som sant och falskt?

#### Python

Python följer principen att värden betraktas som "falsy" om de är tomma eller 0, annars är de "truthy".

**Exempel på falska värden:**

```python
False
0
0.0
''  # Tom sträng
[]  # Tom lista
{}  # Tom dictionary
None
```

**Exempel på sanna värden:**

```python
True
1
0.1
"text"
[1, 2, 3]
{"key": "value"}
```

#### Java

Java använder striktare regler. Endast `boolean`-typen kan användas direkt i villkorssatser. Andra typer måste uttryckligen jämföras eller konverteras.

```java
boolean flag = true;
if (flag) {
    System.out.println("flag är true");
}

int x = 0;
if (x != 0) { // Måste uttryckligen jämföra värdet
    System.out.println("x är inte 0");
}
```

## Klasser

Klasser används i både Python och Java för att skapa objekt och definiera deras egenskaper (attribut) och beteenden (metoder). De möjliggör objektorienterad programmering och underlättar återanvändning av kod.

### Skapa en klass

#### Python

```python
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age

    def greet(self, greeting="Hej"):
        return f"{greeting}, jag heter {self.name} och är {self.age} år gammal."

p = Person("Anna", 30)
print(p.greet())
print(p.greet("Hallå"))
```

#### Java

```java
class Person {
    String name;
    int age;

    Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    String greet() {
        return greet("Hej");
    }

    String greet(String greeting) {
        return greeting + ", jag heter " + name + " och är " + age + " år gammal.";
    }
}

public class Main {
    public static void main(String[] args) {
        Person p = new Person("Anna", 30);
        System.out.println(p.greet());
        System.out.println(p.greet("Hallå"));
    }
}
```

#### Skillnad

- Python har valfria parametrar (default-argument), medan Java inte har detta. Java simulerar detta med överlagrade metoder, som visas i exemplen ovan.
- Python använder `self` för att referera till instansen, medan Java använder `this`.
- Java kräver att varje klass har en specifik typdeklaration för variabler, medan Python är dynamiskt typat.
- Java behöver en huvudmetod (`main`) för att köra programmet, medan Python kan exekvera kod direkt.

### Inkapsling

Inkapsling används för att skydda data och kontrollera åtkomst till attribut och metoder i en klass.

#### Python

```python
class BankAccount:
    def __init__(self, balance):
        self._balance = balance   # Skyddat attribut (konvention)
        self.__private_balance = balance  # Privat attribut (namnmangling)

    def deposit(self, amount):
        if amount > 0:
            self.__private_balance += amount

    def get_balance(self):
        return self.__private_balance

account = BankAccount(1000)
account.deposit(500)
print(account.get_balance())
```

#### Java

```java
class BankAccount {
    private double balance; // Privat attribut

    BankAccount(double balance) {
        this.balance = balance;
    }

    void deposit(double amount) {
        if (amount > 0) {
            balance += amount;
        }
    }

    double getBalance() {
        return balance;
    }
}

public class Main {
    public static void main(String[] args) {
        BankAccount account = new BankAccount(1000);
        account.deposit(500);
        System.out.println(account.getBalance());
    }
}
```

#### Skillnad

- Python använder ett enkelt understreck `_` som konvention för skyddade attribut och dubbla understreck `__` för pseudo-privata attribut genom **namnmangling**.
- Java använder nyckelordet `private` för strikt kontroll över åtkomst.
- Java tillåter direkt typkontroll och kompileringsfel om typerna inte stämmer, medan Python kontrollerar vid körning.

### Vad är namnmangling?

Python stöder **namnmangling** för att förhindra oavsiktlig åtkomst till pseudo-privata attribut. När ett attribut börjar med dubbla understreck (`__`), ändrar Python internt namnet på attributet för att inkludera klassnamnet. Detta gör attributet svårare att komma åt utifrån klassen.

```python
class Example:
    def __init__(self):
        self.__hidden = "intern angelägenhet"
        obj = Example()
#print(obj.__hidden)  # Ger fel
print(obj._Example__hidden)  # Fungerar med namnmangling
```

**Resultat:**

```plaintext
intern angelägenhet
```

**Observera:** Namnmangling är inte ett fullständigt skydd, utan en mekanism för att undvika konflikter med underklasser. Java har en striktare kontroll via `private`.

### Arv

Arv används för att skapa en ny klass (underklass) baserad på en befintlig klass (överklass), vilket gör det möjligt att återanvända kod.

#### Python

```python
class Animal:
    def make_sound(self):
        return "Ljud"

class Dog(Animal):
    def make_sound(self):
        return "Voff"

d = Dog()
print(d.make_sound())  # Utmatning: Voff
```

#### Java

```java
class Animal {
    String makeSound() {
        return "Ljud";
    }
}

class Dog extends Animal {
    @Override
    String makeSound() {
        return "Voff";
    }
}

public class Main {
    public static void main(String[] args) {
        Dog d = new Dog();
        System.out.println(d.makeSound()); // Utmatning: Voff
    }
}
```

### Polymorfism (kort exempel)

#### Python

```python
animals = [Animal(), Dog()]
for animal in animals:
    print(animal.make_sound())
```

#### Java

```java
Animal[] animals = {new Animal(), new Dog()};
for (Animal animal : animals) {
    System.out.println(animal.makeSound());
}
```

- Python stöder multipelt arv, medan Java endast tillåter enkel arv.
- Java kräver nyckelordet `extends` för arv och `@Override` för att åsidosätta metoder.
- Python är mer dynamiskt och kontrollerar vid körning, medan Java gör statiska kontroller vid kompilering.

## Strukturera program

### Översikt

Tabellen nedan ger en sammanfattning av centrala komponenter som används för att strukturera program i Python och Java. Den beskriver deras syfte och hur de implementeras i respektive språk.

| **Komponent**          | **Beskrivning**                                                                                             | **Python**                                                                                                                                                                                                      | **Java**                                                                                                                                                                                                  |
| ---------------------- | ----------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Variabeltillgänglighet | En variabel definierad i ett kodblock, som en if-sats, är endast tillgänglig inom blocket där den skapades. | Skapas automatiskt för funktioner och klasser.                                                                                                                                                                  | Skapas automatiskt för funktioner och klasser.                                                                                                                                                            |
| Kodfil                 | En fil som innehåller kod.                                                                                  | Kallas modul. En modul är en fil med ändelsen `.py` som kan innehålla klasser, funktioner och variabler.                                                                                                        | Kallas kompileringsenhet (_compilation unit_). En kompileringsenhet kan innehålla flera klasser, men endast en `public`-klass per fil.                                                                    |
| Paket                  | Mappar för att organisera kodfiler. Varje mapp kan innehålla fler undermappar och kodfiler.                 | I Python så är ett paket en mapp som innehåller moduler och kan organiseras hierarkiskt. Ett paket kan innehålla en `__init__.py`-fil för att köra kod vid importering, men detta är valfritt sedan Python 3.3. | I Java så är paket en samling mappar som följer en strikt struktur och måste matcha deklarationerna i koden. Paket används för att organisera klasser och gränssnitt samt för att undvika namnkonflikter. |
| Imports                | Mekanism för att inkludera kod från andra filer eller mappar.                                               | Dynamiska och flexibla (kan importera delar av moduler och importera olika moduler under körning).                                                                                                              | Statiska och strikt strukturerade (kräver exakta paket och klasser vid kompilering).                                                                                                                      |
| Åtkomstmodifierare     | Regler som styr synlighet och åtkomst till klasser, metoder och variabler.                                  | Konventioner för privat, skyddad och offentlig.                                                                                                                                                                 | Strikta regler med `public`, `protected`, `private`.                                                                                                                                                      |

### Variabeltillgänglighet

Variabeltillgänglighet (scope) definierar det område i koden där en variabel är giltig och kan användas. Detta är viktigt för att undvika namnkonflikter och säkerställa att variabler bara är tillgängliga där de behövs.

#### Python

- Variabler deklareras automatiskt som lokala inom funktioner och kodblock.
- Globala variabler måste anges med nyckelordet `global` för att kunna modifieras inom funktioner.

```python
def my_function():
    x = 10  # Lokal variabel
    print(x)

x = 5  # Global variabel
my_function()
print(x)

if True:
    y = 20
print(y)  # Fungerar, y är fortfarande tillgänglig här

for i in range(1):
    z = 30
#print(z)  # Ger fel eftersom z är utanför sitt block
```

#### Java

- Variabler i Java har tydligt definierade scopes baserat på var de deklareras.
- Instansvariabler är bundna till objektet och är tillgängliga så länge objektet existerar.

```java
public class Example {
    int x = 5; // Instansvariabel

    public void myMethod() {
        int y = 10; // Lokal variabel
        System.out.println(y);
    }

    public void testScope() {
        if (true) {
            int a = 15;
            System.out.println(a); // Fungerar här
        }
        // System.out.println(a); // Ger fel: a är utanför sitt scope

        for (int i = 0; i < 1; i++) {
            int b = 25;
            System.out.println(b); // Fungerar här
        }
        // System.out.println(b); // Ger fel: b är utanför sitt scope
    }
}
```

#### Likheter

- Både Python och Java har blockscope för variabler deklarerade inom kodblock (t.ex. loopar och if-satser).
- Variabler definierade inuti ett block är endast tillgängliga inom det blocket.

#### Skillnader

- Python låter scopes skapas dynamiskt vid körning, medan Java kräver att scopes deklareras explicit vid kompilering.
- Java hanterar statisk typkontroll för variabler, vilket saknas i Python.

### Kodfil

En kodfil är en fil som innehåller programkod och används för att organisera funktioner, klasser och logik.

#### Python

- En kodfil i Python kallas en **modul** och har filändelsen `.py`.
- Moduler kan innehålla klasser, funktioner och variabler.

```python
# my_module.py
class Person:
    def __init__(self, name):
        self.name = name

    def greet(self):
        return f"Hej, jag heter {self.name}!"
```

#### Java

- En kodfil i Java kallas en **kompileringsenhet** (_compilation unit_).
- Varje fil kan innehålla flera klasser, men endast en klass kan vara `public` och måste matcha filnamnet.

```java
// Person.java
public class Person {
    private String name;

    public Person(String name) {
        this.name = name;
    }

    public String greet() {
        return "Hej, jag heter " + name + "!";
    }
}
```

#### Likheter

- Både Python och Java använder kodfiler för att strukturera program och återanvända kod.
- Kodfiler kan definiera flera klasser och funktioner.

#### Skillnader

- Python är mer flexibel och kräver inte att filnamn matchar klassnamn.
- Java kräver att en `public`-klass matchar filnamnet och kontrollerar strikt strukturen vid kompilering.

### Paket

Ett paket är en mapp som används för att organisera kodfiler och skapa en hierarkisk struktur för programmet.

#### Python

- I Python är ett paket en mapp som innehåller en eller flera moduler.
- Paket kan innehålla en valfri `__init__.py`-fil för att köra kod vid importering, men detta är inte längre ett krav sedan Python 3.3.

```python
my_package/
    __init__.py
    module1.py
    module2.py

from my_package import module1
module1.some_function()
```

#### Java

- I Java är ett paket en samling mappar som följer en strikt struktur där mapparna måste matcha paketdeklarationerna i koden.
- Paket börjar ofta med ett domännamn baklänges, till exempel com.example för att undvika namnkonflikter, och fortsätter med undermappar för att spegla programmets struktur.

```java
com/example/
    Main.java

package com.example;

public class Main {
    public static void main(String[] args) {
        System.out.println("Hej från ett paket!");
    }
}
```

### Imports

Imports används för att inkludera kod från andra filer eller paket och återanvända funktioner, klasser och variabler.

#### Python

- Python stödjer flexibla och dynamiska imports.
- Moduler kan importeras helt eller delvis beroende på behov.
- Dynamiska imports möjliggör att moduler laddas under körning med hjälp av `__import__()`-funktionen.

```python
import math
print(math.sqrt(16))

from math import sqrt
print(sqrt(16))

modulnamn = 'math'
modul = __import__(modulnamn)
print(modul.sqrt(16))
```

#### Java

- Imports i Java är statiska och strikt strukturerade.
- Alla klasser och paket måste specificeras exakt vid kompilering.
- Klasser i java.lang-paketet importeras automatiskt och kräver ingen explicit import.

```java
import java.util.ArrayList;

public class Example {
    public static void main(String[] args) {
        ArrayList<String> list = new ArrayList<>();
        list.add("Hej");
        System.out.println(list.get(0));
    }
}
```

#### Likheter

- Både Python och Java använder imports för att inkludera kod från andra filer och paket.
- Imports möjliggör återanvändning av kod och modularisering.

#### Skillnader

- Python erbjuder dynamiska imports, vilket möjliggör att moduler laddas under körning.
- Java kräver statiska imports som måste specificeras exakt under kompilering.

### Åtkomstmodifierare

Åtkomstmodifierare styr synligheten och åtkomsten till klasser, metoder och variabler. De används för att kontrollera hur olika delar av programmet kan interagera med varandra och skydda data från obehörig åtkomst.

#### Python

- Python saknar strikt kontroll av åtkomstmodifierare men följer konventioner:
  - `**public**`: Alla attribut och metoder är offentliga som standard och kan nås var som helst.
  - `**_protected**`: Ett understreck markerar att attributet är skyddat och bör behandlas som privat inom klassen och dess underklasser.
  - `**__private**`: Dubbla understreck aktiverar namnmangling för att göra attributet svårare att nå utanför klassen.

```python
class Example:
    public_var = 10
    _protected_var = 20
    __private_var = 30

    def __init__(self):
        self.__private_var = 40
```

#### Java

- Java använder striktare åtkomstmodifierare:
  - `**public**`: Synlig överallt.
  - `**protected**`: Synlig inom samma paket och i underklasser, även om de ligger i andra paket.
  - `**private**`: Endast synlig inom samma klass.
  - **(Standard, inget nyckelord)**: Synlig endast inom samma paket (kallas package protected).

```java
public class Example {
    public int publicVar = 10;
    protected int protectedVar = 20;
    private int privateVar = 30;

    public Example() {
        privateVar = 40; // Endast tillgänglig inom samma klass
    }
}
```

#### Likheter

- Både Python och Java möjliggör kapsling (encapsulation) av data genom åtkomstkontroller.
- Attribut och metoder kan döljas från andra delar av programmet för att förhindra oavsiktliga ändringar.

#### Skillnader

- Python förlitar sig på konventioner för att markera attribut som privata eller skyddade.
- Java tillämpar striktare regler genom kompilatorn för att säkerställa åtkomstkontroll.
- Java stöder nivåer för paketbaserad synlighet med standardmodifieraren (package protected), vilket inte finns i Python.

## Bytekod och kompilering

Att förstå hur bytekod och kompilering fungerar är viktigt för att kunna analysera och optimera prestanda i program skrivna i Python och Java. Den här sektionen beskriver processen från källkod till körbar kod i båda språken.

### Vad är bytekod?

Bytekod är en mellannivårepresentation av koden som kan exekveras av en virtuell maskin (VM). Den fungerar som ett steg mellan källkod och maskinkod.

- **Python**: Källkoden kompileras till bytekod (`.pyc`-filer) som exekveras av Python Virtual Machine (PVM).
- **Java**: Källkoden kompileras till bytekod (`.class`-filer) som körs i Java Virtual Machine (JVM).

### Kompileringsprocessen

#### Python

1. Källkod sparas i filer med filändelsen `.py`.
2. Vid körning kompileras källkoden till bytekod och sparas i `.pyc`-filer i katalogen `__pycache__`.
3. Bytekoden tolkas och exekveras av Python Virtual Machine (PVM).

```python
print("Hej världen")
```

Vid första exekveringen skapas en motsvarande `.pyc`-fil som kan användas för snabbare laddning vid senare exekveringar.

**Exempel på bytekod för Python:**

```plaintext
  1           0 LOAD_GLOBAL              0 (print)
              2 LOAD_CONST               1 ('Hej världen')
              4 CALL_FUNCTION            1
              6 POP_TOP
              8 LOAD_CONST               0 (None)
             10 RETURN_VALUE
```

#### Java

1. Källkod sparas i filer med filändelsen `.java`.
2. Kompilatorn `javac` kompilerar koden till bytekod i `.class`-filer.
3. Bytekoden exekveras av Java Virtual Machine (JVM).

Exempel:

```java
public class Main {
    public static void main(String[] args) {
        System.out.println("Hej världen");
    }
}
```

Efter kompilering med `javac Main.java` skapas filen `Main.class`, som kan exekveras med `java Main`.

**Exempel på bytekod för Java:**

```plaintext
public static void main(java.lang.String[]);
  Code:
     0: getstatic     #2                  // Field java/lang/System.out:Ljava/io/PrintStream;
     3: ldc           #3                  // String Hej världen
     5: invokevirtual #4                  // Method java/io/PrintStream.println:(Ljava/lang/String;)V
     8: return
```

### Likheter

- Båda språken använder bytekod som ett mellansteg innan exekvering.
- Virtual Machines (PVM och JVM) hanterar exekveringen.

### Skillnader

- Python kompilerar koden vid körning (Just-In-Time, JIT), medan Java kompileras i förväg till bytekod.
- Java kräver en tydlig kompilering innan programmet kan exekveras, medan Python kan exekvera direkt från källkoden.

### Optimeringar

#### Python

- Python använder caching av kompilerad bytekod i `__pycache__` för snabbare exekvering.
- Just-In-Time-kompilatorer (t.ex. PyPy) kan användas för bättre prestanda.

#### Java

- Just-In-Time-kompilering i JVM optimerar prestanda under exekvering.
- Verktyg som `javac` och profilerare kan användas för att analysera och optimera kod.
