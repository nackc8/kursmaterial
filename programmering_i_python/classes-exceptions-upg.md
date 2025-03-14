# Classes och Exceptions: Övningsuppgifter

I de här övningarna får du träna på att skapa klasser och hantera undantag.

## Inför övningsuppgifterna

### Bekanta dig med Turtle-grafik

De sista övningarna använder `turtle`-grafik. Leta på `turtle`-modulen i standardbibliotekets dokumentation. Läs sedan gärna dokumentationen innan du börjar.

### Läs om TypeError och ValueError

Läs om skillnaden mellan undantagen `TypeError` och `ValueError`. Det är viktigt att förstå dem för att kunna kasta (`raise`) rätt undantag vid fel och ärva från rätt undantag när du skapar egna undantagsklasser.

### Att göra i varje övning

#### Läs dokumentation

Läs dokumentationen för de delar av språket som du använder. Dessa är:

- [Inbyggda funktioner][1], de funktioner som är inbyggda och finns tillgängliga från början utan att behöva göra `import`.
- [Inbyggda datatyper][2] som `int` och `str`.
- [Moduler i standardbiblioteket][3], det är de saker du använder som kräver att du importerar dem med `import` innan.

#### Använd debugläget i utvecklingsmiljön

Vänj dig vid debugverktyget. Sätt breakpoints, stega igenom koden och kontrollera variabelvärden. [Visual Studio Code][4] är till stor hjälp för att förstå och hitta fel.

## Övningsuppgifter

### Övning 1: Personklass

1. Skapa en klass `Person` med attributen `name` och `age`.
2. Instansiera och skriv ut ett objekt.
3. Lägg till metoden `greet(other_name=None)` som skriver ut:
   - `"Hej {other_name}! Mitt namn är {name}!"` om `other_name` är angivet.
   - Annars `"Hej främling! Mitt namn är {name}!"`.

#### Exempel

```python
p = Person("Alice", 25)
p.greet()       # Hej främling! Mitt namn är Alice!
p.greet("Bob")  # Hej Bob! Mitt namn är Alice!
```

#### Tips

- Använd `self.name` i metoden `greet()`.
- Testa med både angivet och icke-angivet `other_name`.

### Övning 2: Lösenordshanterare

1. Skapa en klass `PasswordManager` som lagrar lösenord i en ordbok (`dict`).
2. Implementera en metod `add_password(name, password)`.
3. Skapa en egen undantagsklass `DuplicatePasswordError` (ärver från `ValueError`). Kasta det om ett lösenord med samma namn redan finns.

#### Exempel

```python
pm = PasswordManager()
pm.add_password("email", "secure123")
pm.add_password("bank", "myp@ssword")
pm.add_password("email", "newpass")  # Kastar DuplicatePasswordError
```

#### Tips

Kontrollera om `name` redan finns innan du lägger till.

### Övning 3: Tidsvalidering

1. Skapa en klass `Time` med attributen `hours` och `minutes`.
2. Kasta `ValueError` om `hours` inte är 0–23 eller `minutes` inte är 0–59.
3. Kasta `TypeError` om `hours` eller `minutes` inte är heltal.

#### Exempel

```python
t1 = Time(14, 30)     # Giltig tid
t2 = Time(25, 10)     # Kastar ValueError
t3 = Time(25, "Boll") # Kastar TypeError
```

#### Tips

- Gör valideringen i `__init__`.
- Skapa flera `Time`-objekt för att testa.

### Övning 4: Tom

Med avsikt lämnad tom.

### Övning 5: Produktklassen `Product`

1. Skapa en klass `Product` med attributen `name` och `price`.
2. Implementera `__str__()` så att en tydlig sträng returneras.

#### Exempel

```python
p = Product("Laptop", 9999)
print(p)  # Produkt: Laptop, Pris: 9999 kr
```

#### Tips

Använd `f"{self.name}, {self.price}"` eller liknande i `__str__()`.

### Övning 6: Shoppingvagnsklassen `ShoppingCart`

1. Skapa en klass `ShoppingCart`.
2. Använd en ordbok där nyckeln är ett `Product`-objekt och värdet är antalet.
3. Implementera:
   - `add_item(product, quantity)`
   - `remove_item(product)`
   - `view_cart()`
4. Kasta ett undantag om man tar bort en produkt som inte finns i kundvagnen.

#### Exempel

```python
cart = ShoppingCart()
p1 = Product("Laptop", 9999)
p2 = Product("Mus", 499)

cart.add_item(p1, 1)
cart.add_item(p2, 2)
cart.view_cart()
cart.remove_item(p1)
cart.remove_item(p1)  # Kastar undantag
```

#### Tips

Kontrollera om produkten finns innan du tar bort den.

### Övning 7: Miniräknare

1. Skapa en klass `Calculator` för enkla räkneoperationer (addition, subtraktion, multiplikation, division).
2. Endast en operation i taget på det aktuella talet.
3. Lägg till en metod `reset()` som återställer värdet till 0.

#### Exempel

```python
calc = Calculator()
calc.add(5)       # 5
calc.multiply(2)  # 10
calc.divide(0)    # Kastar ZeroDivisionError
```

#### Tips

- Hantera division med 0 genom att kasta `ZeroDivisionError`.
- Använd en instansvariabel för det aktuella värdet.

### Övning 8: Tom

Med avsikt lämnad tom.

### Övning 9: Timerklass

1. Skapa en klass `Timer` med metoderna `start()`, `stop()`, `clear()` och `elapsed_time()`.
2. Kasta ett undantag om `elapsed_time()` anropas innan 10 sekunder gått sedan `start()`.
3. Gör ett textbaserat gränssnitt där användaren kan:
   - Se alla timers och deras status.
   - Lägga till, ta bort eller ändra en timer.

#### Exempel

```python
t = Timer()
t.start()
time.sleep(5)
print(t.elapsed_time())  # Kastar undantag
```

#### Tips

- Använd `time.time()` för att mäta tiden.
- Hantera flera timers genom en lista eller ordbok.

### Övning 10: Figurritare med `Turtle`

1. Skapa en klass `ShapeDrawer` för att rita figurer med `Turtle`.
2. Stöd minst fyrkanter och cirklar.
3. Kasta `ValueError` för ogiltiga dimensioner.

#### Exempel

```python
drawer = ShapeDrawer()
drawer.draw_square(50)
drawer.draw_circle(-20)  # Kastar ValueError
```

#### Tips

- Använd `turtle.Turtle()` för att rita.
- Validera dimensioner innan du ritar.

### Övning 11: Färgbytande Turtle

1. Skapa en klass `ColorTurtle` som ärver från `Turtle`.
2. Lägg till en metod `change_color(color)` och kasta `ValueError` för ogiltiga färger.
3. Låt `ColorTurtle` rita former medan färger slumpas eller anges av användaren.

#### Exempel

```python
t = ColorTurtle()
t.change_color("red")
t.draw_square(50)
t.change_color("unknown")  # Kastar ValueError
```

#### Tips

- Använd `turtle.color()` för att ändra färg.
- Kontrollera färgsträngarna innan du ändrar dem.

### Övning 12: Tom

Med avsikt lämnad tom.

### Övning 13: Mönstergenerator med felhantering

1. Skapa en klass `PatternGenerator` som ritar mönster (t.ex. spiraler, stjärnor).
2. Använd en ordbok för att koppla mönstertyper till ritfunktioner.
3. Kasta `ValueError` för ogiltiga mönstertyper eller repetitionsantal.
4. Tillåt användaren att definiera egna mönster.

#### Exempel

```python
p = PatternGenerator()
p.draw("spiral", 5)
p.draw("unknown", 10)  # Kastar ValueError
```

#### Tips

- Använd en ordbok som mappar mönstertyper till funktioner.
- Validera indata innan ritning.

[1]: https://docs.python.org/3.11/library/functions.html
[2]: https://docs.python.org/3.11/library/stdtypes.html
[3]: https://docs.python.org/3.11/library/index.html
[4]: https://code.visualstudio.com/Docs/editor/debugging
