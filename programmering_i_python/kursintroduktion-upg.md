# Kort introduktion till Python: Övningsuppgifter

## Inför övningsuppgifterna

### Datainläsning från användaren

Exempel på hur ditt program kan begära inmatning från användaren:

```python
name = input("Enter your name: ")
age = int(input("Enter your age: "))

# Använd variablerna name och age här...
```

### Att göra i varje övning

#### Läs dokumentation

Läs dokumentationen för de delar av språket som du använder. Dessa är:

- [Inbyggda funktioner][1], de funktioner som är inbyggda och finns tillgängliga från början utan att behöva göra `import`.
- [Inbyggda datatyper][2] som `int` och `str`.
- [Moduler i standardbiblioteket][3], det är de saker du använder som kräver att du importerar dem med `import` innan.

#### Använd debugläget i utvecklingsmiljön

Vänj dig vid debugverktyget. Sätt breakpoints, stega igenom koden och kontrollera variabelvärden. [Visual Studio Code][4] är till stor hjälp för att förstå och hitta fel.

### Funktion för utskrift

Funktionen `print` används för att skriva ut saker på skärmen. Den kan användas på sitt enklaste sätt med ett argument:

```python
print("Hello world")
```

Eller så kan man skriva flera argument med komma emellan:

```python
print("Hej", name)
```

## Övningsuppgifter

### Övning 1: Typkonvertering

1. Läs in ett heltal (int) från användaren med hjälp av `input`.
2. Behåll det inmatade som en sträng i en variabel, men skapa två nya variabler där du konverterar värdet till:
    - `int`
    - `float`
3. Skriv ut de konverterade värdena.

#### Exempel

```plaintext
Enter an integer: 42
As int: 42
As float: 42.0
As string: "42"
```

### Övning 2: Beräkningar med flyttal

1. Läs in två decimaltal (float) med `input`.
2. Utför följande operationer och visa resultaten:
    - Addition
    - Multiplikation
    - Division

#### Exempel

Exakt utskrift kan skilja något beroende på hur Python formaterar flyttal.

```plaintext
Enter the first number: 3.456
Enter the second number: 1.234
Addition: 4.69
Multiplication: 4.27
Division: 2.80
```

### Övning 3: Störst av två tal med `if`-sats

1. Läs in två heltal från användaren med hjälp av `input`.
2. Använd en `if`-sats (gärna `if`-`elif`-`else`) för att bestämma vilket av de två talen som är störst, eller om de är lika stora.
3. Skriv ut resultatet.

#### Exempel

**Exempel 1:**

```plaintext
Enter the first number: 15
Enter the second number: 20
The largest number is: 20
```

**Exempel 2:**

```plaintext
Enter the first number: 7
Enter the second number: 7
The numbers are equal.
```

### Övning 4: Hitta temperaturer

1. Skapa en **lista** med decimaltal (float) som representerar temperaturen hos en person vid olika tidpunkter. Listans temperaturer är “hårdkodade”, d.v.s. skrivna direkt i källkoden.
2. Använd en **`for`-loop** för att gå igenom listan och hitta:
   - Den **högsta temperaturen**.
   - Den **näst högsta temperaturen**.
3. Skriv ut båda temperaturerna.

#### Exempel

Följande temperaturdata:

- 37.0
- 37.8
- 39.2
- 38.5
- 36.9

Ska ge utskriften:

```plaintext
Highest temperature: 39.2
Second highest temperature: 38.5
```

### Övning 5: Summa av inmatade heltal

1. Skriv ett program som ber användaren mata in **fem** heltal.
2. Spara dessa heltal i en lista.
3. Räkna ihop summan av de fem heltalen.
4. Skriv ut summan.

#### Exempel

```plaintext
Enter an integer (1/5): 10
Enter an integer (2/5): 5
Enter an integer (3/5): 3
Enter an integer (4/5): 12
Enter an integer (5/5): 0
The sum of the numbers is: 30
```

### Övning 6: Summering med `range()`

1. Skriv ett program som summerar alla heltal från och med 1 till och med 10 med hjälp av en `for`-loop och `range()`.
2. Skriv ut summan.

#### Tips

range(1, 11)` ger talen 1, 2, 3, …, 10.

#### Exempel

```plaintext
The sum of 1 through 10 is: 55
```

### Övning 7: Nedräkning med `while`

1. Be användaren mata in ett startvärde (heltal).
2. Använd en `while`-loop för att räkna nedåt från detta startvärde tills du når 0.
3. Skriv ut varje värde under nedräkningen.

#### Exempel

```plaintext
Enter a start value: 5
5
4
3
2
1
0
```

### Övning 8: Filtrera udda tal med `continue`

1. Be användaren mata in ett heltal `n`.
2. Använd en `for`-loop från 1 till `n` (inklusivt).
3. Om talet är **jämnt**, hoppa över utskriften av talet med hjälp av `continue`.
4. Om talet är **udda**, skriv ut det.

#### Exempel

```plaintext
Enter a number: 10
1
3
5
7
9
```

#### Tips

`num % 2` ger talet `1` om variabeln `num` är udda.

### Övning 9: Muterbara och icke-muterbara objekt

I Python används alltid referenser vid tilldelning av variabler. Det som avgör om en ändring påverkar den ursprungliga variabeln eller ej beror på om objektet är **muterbart** (kan ändras) eller **icke-muterbart** (kan inte ändras).

Utforska detta genom att:

1. Skapa en lista `original_list` med några värden, t.ex. `[1, 2, 3]`.
2. Tilldela denna lista till en ny variabel `copy_list = original_list`.
3. Lägg till ett nytt värde i `original_list`.
4. Skriv ut båda listorna och notera vad som händer.

    **Fråga**: Varför ändras båda listorna?

5. Gör nu en **äkta kopia** av listan med `copy_list = original_list.copy()`.
6. Lägg till ett nytt värde i `original_list` igen och skriv ut båda listorna.

    **Fråga**: Vad är skillnaden den här gången?

#### Exempel

```python
# Icke-muterbar datatyp (sträng)
original_string = "Hej"
copy_string = original_string
original_string += " världen"

print("Original string:", original_string)  # "Hej världen"
print("Copy string:", copy_string)          # "Hej" (förändrades ej!)

# Muterbar datatyp (lista)
original_list = [1, 2, 3]
copy_list = original_list
original_list.append(4)

print("Original list:", original_list)  # [1, 2, 3, 4]
print("Copy list:", copy_list)          # [1, 2, 3, 4] (förändrades också!)
```

### Övning 10: Skapa en lista med tal över medelvärdet

1. Skapa en lista `in_lst` med ett antal tal (heltal eller decimaltal) som du väljer själv.
2. Räkna ut medelvärdet (average) av talen i listan.
   - Du kan använda funktionen `sum(...)` för att räkna ihop värdena och `len(...)` för att få antalet värden i listan.
   - Medelvärdet blir då `sum(in_lst) / len(in_lst)`.
3. Skapa en ny lista `out_lst` som bara innehåller de tal i `in_lst` som är **större** än medelvärdet.
4. Skriv ut den nya listan `out_lst`.

#### Exempel

```plaintext
in_lst = [5, 10, 3, 15, 2]
Average = 7.0
out_lst = [10, 15]
```

### Övning 11: Gissa talet (slumpmässigt tal)

1. Använd `import random` och generera ett slumpmässigt tal mellan 1 och 100. Detta är det hemliga talet.
2. Be sedan användaren gissa talet.
3. Om gissningen är för låg, skriv ut ett meddelande om att det hemliga talet är högre.
4. Om gissningen är för hög, skriv ut ett meddelande om att det hemliga talet är lägre.
5. Om gissningen är korrekt, skriv ut ett grattis-meddelande och avsluta programmet.

Du kan välja att låta användaren gissa flera gånger i rad med en loop tills rätt gissning sker.

#### Tips

För att kunna välja ett slumpmässigt heltal i Python kan du använda standardbiblioteket `random`. Exempel:

```python
import random

secret_number = random.randint(1, 100)  # Slumptal mellan 1 och 100
```

### Övning 12: Black Jack

Förenklat Black Jack (21):

1. Importera `random`.
2. Låt datorn “dra” två slumpmässiga kortvärden (t.ex. 1–11). Spara summan i en variabel, t.ex. `dealer_sum`.
3. Låt användaren (spelaren) också “dra” två kortvärden (1–11). Spara i `player_sum`.
4. Fråga spelaren om de vill dra ytterligare ett kort (“hit”).
   - Om ja, dra ytterligare ett slumpmässigt tal och lägg till i `player_sum`.
   - Upprepa tills spelaren väljer att sluta eller summan överskrider 21.
5. Dealern kan du låta vara som den är (eller själv ge dealern en enkel logik: dra kort tills `dealer_sum` är över 17).
6. Avgör vem som vinner:
   - Om spelaren har mer än 21 förlorar spelaren automatiskt.
   - Annars jämför du `player_sum` och `dealer_sum`. Den som är närmast 21 vinner.

Detta är en väldigt förenklad variant av Black Jack. Du kan bygga ut med mer logik om du vill.

### Övning 13: Dela upp ett tidigare program i funktioner och separata filer

I tidigare övningar har du skrivit all kod i en och samma fil. Nu ska du ta ett av de tidigare programmen (t.ex. gissa talet eller nåt annat du skrivit) och dela upp det i flera **funktioner** och **flera filer**.

#### Förslag

1. Skapa en ny fil, t.ex. `game_logic.py`, där du lägger en eller flera funktioner, t.ex.:

   ```python
   def generate_secret_number():
       import random
       return random.randint(1, 100)

   def check_guess(secret, guess):
       if guess < secret:
           return "Too low"
       elif guess > secret:
           return "Too high"
       else:
           return "Correct"
   ```

2. Skapa en annan fil, t.ex. `main.py`, där du:
   - Importerar dina funktioner från `game_logic.py`.
   - Skriver koden som anropar funktionerna och hanterar inmatning/utmatning.
   - Exempel:

     ```python
     import game_logic

     secret_number = game_logic.generate_secret_number()
     while True:
         user_guess = int(input("Guess a number: "))
         result = game_logic.check_guess(secret_number, user_guess)
         print(result)
         if result == "Correct":
             break
     ```

3. Kör `main.py` och se att programmet fungerar.

Det här är första steget för att förstå hur du kan strukturera större projekt.

### Övning 14: Strukturera om ett annat projekt i flera filer

Gör samma sak en gång till, fast välj **ett annat** program du skrivit i en tidigare övning (t.ex. Black Jack från övning 12 eller temperaturprogrammet från övning 4). Skapa minst två filer:

1. En fil för huvudprogrammet (t.ex. `main.py`).
2. En fil där du lägger flera funktioner som sköter huvudlogiken (t.ex. `temp_logic.py`, `card_logic.py` eller något annat passande namn).

Exempel för temperaturprogrammet (om du vill strukturera just det):

- `temp_logic.py` innehåller funktioner för att beräkna högsta och näst högsta temperatur, etc.
- `main.py` innehåller koden för att:
  1. Skapa (eller läsa) en lista med temperaturer.
  2. Anropa funktionerna i `temp_logic.py`.
  3. Skriva ut resultatet.

#### Tips

Om du har två filer, t.ex. `main.py` och `game_logic.py`, kan du köra programmet genom att anropa:

```bash
python main.py
```

Ibland kan du behöva se till att båda filerna ligger i samma katalog, eller använda `import` med mer avancerade sökvägar, men för den här övningen räcker det oftast att de ligger i samma katalog.

[1]: https://docs.python.org/3.13/library/functions.html
[2]: https://docs.python.org/3.13/library/stdtypes.html
[3]: https://docs.python.org/3.13/library/index.html
[4]: https://code.visualstudio.com/Docs/editor/debugging
