
# Listor, loopar och funktioner: Övningsuppgifter

I dessa övningar kommer du att öva på att hantera listor, funktioner, typkonvertering och rekursiva anrop.

## Inför övningsuppgifterna

### Bekanta dig med `enumerate`, `len`, `in` och slicing

#### `enumerate()`

Tillåter dig att iterera över t.ex. en lista och samtidigt få index:

```python
frukter = ["äpple", "banan", "körsbär"]
for index, frukt in enumerate(frukter):
      print(f"Index {index}: {frukt}")
```

#### `len(...)`

Returnerar antalet element i en sekvens (lista, sträng, etc.):

```python
frukter = ["äpple", "banan", "körsbär"]
print(len(frukter))  # Output: 3
```

#### `in`-operatorn

Används för att kontrollera om ett element finns i en sekvens:

```python
if "banan" in frukter:
      print("Banan finns i listan!")
```

#### Slicing

Ger dig möjlighet att extrahera delar av listor eller strängar:

```python
lst = [0, 1, 2, 3, 4, 5]
print(lst[0])    # Första elementet
print(lst[2:])   # Från index 2 till slutet
print(lst[:4])   # Från början till (ej inklusive) index 4
print(lst[::2])  # Varannat element
print(lst[::-1]) # Omvänd lista
```

### Att göra i varje övning

#### Läs dokumentation

Läs dokumentationen för de delar av språket som du använder. Dessa är:

- [Inbyggda funktioner][1], de funktioner som är inbyggda och finns tillgängliga från början utan att behöva göra `import`.
- [Inbyggda datatyper][2] som `int` och `str`.
- [Moduler i standardbiblioteket][3], det är de saker du använder som kräver att du importerar dem med `import` innan.

#### Använd debugläget i utvecklingsmiljön

Vänj dig vid debugverktyget. Sätt breakpoints, stega igenom koden och kontrollera variabelvärden. [Visual Studio Code][4] är till stor hjälp för att förstå och hitta fel.

## Övningsuppgifter

### Övning 1: Scrolltext

1. Skapa ett program som tar en sträng från användaren och rullar (scrollar) den från höger till vänster i terminalen.
2. Låt programmet göra en “förflyttning” av texten varje gång en kort stund har passerat (använd `time.sleep` eller liknande).
3. Se till att texten “följer med” hela vägen tills den har lämnat skärmen.

#### Exempel

```plaintext
Ange en text att scrolla: HELLO WORLD
```

Texten `HELLO WORLD` *glider* över skärmen.

#### Tips

- Du kan behöva rensa terminalen regelbundet för att skapa illusionen av rullande text (t.ex. med `os.system('cls')` på Windows eller `os.system('clear')` på Linux/macOS).
- Experimentera med olika tidsintervall för att få en fin effekt.

### Övning 2: Scrolltext med funktion

1. Skapa en funktion, t.ex. `def scroll_text(text: str, speed: float): ...`, som tar en sträng och ett hastighetsvärde.
2. Låt funktionen stå för all logik kring hur texten scrollas.
3. I din `main.py` (eller motsvarande) anropa funktionen med olika parametrar för att se hur det påverkar “scrollen”.

#### Exempel

```plaintext
Ange text: Python rocks!
Ange hastighet (sekunder mellan flytt): 0.2
```

Därefter scrollas texen `Python rocks!` över skärmen, tecken för tecken.

#### Tips

- Tänk på hur du lättast kan manipulera texten. Du kan t.ex. använda slicing för att plocka fram en teckensekvens i taget.

### Övning 3: Fizz Buzz

1. Skriv ett program som loopar genom talen 1 till och med ett tal `n`.
2. Om talet är jämnt delbart med 3, skriv ut **“Fizz”**.
3. Om talet är jämnt delbart med 5, skriv ut **“Buzz”**.
4. Om talet är jämnt delbart med både 3 och 5, skriv ut **“FizzBuzz”**.
5. Annars skriv ut talet.

#### Exempel

```plaintext
Ange ett tal: 15
1
2
Fizz
4
Buzz
Fizz
7
8
Fizz
Buzz
11
Fizz
13
14
FizzBuzz
```

#### Tips

- Se upp med ordningen när du kontrollerar delbarhet med 3 och 5 samtidigt.
- Ett vanligt misstag är att inte först kolla om talet är delbart med 15 innan du kollar 3 eller 5 separat.

### Övning 4: Funktion till en funktion till en funktion

1. Skapa tre funktioner: `funktion_a`, `funktion_b` och `funktion_c`.
2. `funktion_a` anropar `funktion_b`, och `funktion_b` anropar `funktion_c`.
3. Fundera på vad varje funktion ska göra – det kan vara något enkelt, som att bearbeta text, räkna ut något eller skriva ut en hälsning i tre steg.

#### Exempel

```python
>>> main()
Hej från funktion_a
Hej från funktion_b
Hej från funktion_c
```

#### Tips

- Du kan låta varje funktion returnera en sträng eller ett tal till nästa funktion, eller låta varje funktion skriva ut sitt eget budskap.

### Övning 5: Standard-argument (eng. Default)

1. Skapa en funktion med ett eller flera standard-argument, t.ex. `def greet(name, greeting="Hej"): ...`.
2. Testa att anropa funktionen både med och utan det valfria argumentet.

#### Exempel

```python
>>> greet("Anna")
Hej, Anna!
>>> greet("Oskar", greeting="Hallå")
Hallå, Oskar!
```

#### Tips

- Var försiktig när du använder muterbara objekt som default-argument (t.ex. listor eller ordböcker). Det kan ge oväntade resultat om du ändrar dem inuti funktionen.

### Övning 6: Positionella argument

1. Skapa en funktion som tar tre positionella argument, t.ex. `def concat(a, b, c): ...`.
2. Anropa den med värden i olika ordning och observera vad som händer om du blandar ihop ordningen.

#### Exempel

```plaintext
Ange tre strängar: "Hej" "kära" "Python-älskare"
Ihopslagningen blev: "Hej kära Python-älskare"
```

#### Tips

- Positionella argument är bundna till ordningen du skickar in dem i anropet. `concat("Hej", "kära", "Python-älskare")` är **inte** nödvändigtvis detsamma som `concat("Python-älskare", "kära", "Hej")`.

### Övning 7: Nyckelordsargument

1. Skapa en funktion som tar flera parametrar, t.ex. `def create_user(first_name, last_name, age): ...`.
2. Anropa funktionen **med nyckelordsargument**, t.ex. `create_user(age=30, last_name="Svensson", first_name="Anna")`.
3. Skriv ut resultatet på ett snyggt sätt.

#### Exempel

```plaintext
Ange first_name, last_name, age: Anna Svensson 30
Create user: first_name=Anna, last_name=Svensson, age=30
```

#### Tips

- Nyckelordsargument kan göra koden mer läsbar. Se även hur du kan kombinera positionella och nyckelordsargument.

### Övning 8: Använd en f"sträng"

1. Skapa en funktion som tar ett par variabler (t.ex. namn, ålder, stad).
2. Skriv ut en meningsfull text med hjälp av en **f-sträng** (`f"..."`) där variablerna bakas in.

#### Exempel

```plaintext
Ange ditt namn: Lisa
Ange din ålder: 22
Ange stad: Göteborg
"Hej, jag heter Lisa, är 22 år gammal och bor i Göteborg."
```

#### Tips

f-strängar låter dig formatera variabler mycket smidigt, men du kan även använda vanliga strängformat eller `format()`-metoden för liknande resultat.

### Övning 9: Ta bort vokaler från en sträng

1. Be användaren om en valfri sträng.
2. Ta bort alla vokaler (‘a`, ‘e`, ‘i`, ‘o`, ‘u`, ‘y` – och eventuellt de svenska ‘å`, ‘ä`, ‘ö` om du vill).
3. Skriv ut resultatet.

#### Exempel

```plaintext
Ange en sträng: Jag älskar Python
Resultat: Jg lskr Pythn
```

#### Tips

- Använd t.ex. en lista eller sträng som innehåller alla vokaler.
- Du kan använda `replace()` eller en **list comprehension** för att filtrera bort oönskade tecken.

### Övning 10: Antal kamelkaraktärer i en given sträng

1. Ta emot en sträng från användaren.
2. Räkna hur många “kameltecken” (versaler, A-Z) som finns i strängen.
3. Skriv ut hur många det är.

#### Exempel

```plaintext
Ange en sträng: HejJagHeterPython
Antal versaler: 4
```

Versaler är: H, J, H, P

#### Tips

- Du kan t.ex. loopa igenom varje tecken och kolla om `char.isupper()` är `True`.
- Var noga med att bara räkna bokstäver A-Z, inte andra tecken.

### Övning 11: Siffersumma med ihopslagning

1. Be användaren ange en blandad sträng, t.ex. `abc123xyz`.
2. Dela upp strängen i dess siffervärden (digits) och tecken (letters).
3. Räkna ut siffersumman (1+2+3 = 6 i exemplet).
4. Skriv också ut bokstäverna ihopsatta för sig (i exemplet `abcxyz`).

#### Exempel

```plaintext
Ange en blandad sträng: a1b2c3
Siffersumma: 6
Bokstäver ihop: abc
```

#### Tips

- Det kan vara smidigt att göra en iteration över varje tecken och avgöra om `char.isdigit()` eller `char.isalpha()`.
- Fundera på vad som ska hända om du får specialtecken.

### Övning 12: Byt `0` mot `5` i en `int`

1. Ta emot ett heltal från användaren.
2. Ersätt alla `0` med `5`.
3. Returnera eller skriv ut resultatet som ett heltal.

#### Exempel

```plaintext
Ange ett tal: 1020340
Nytt tal: 1525345
```

#### Tips

- Konvertera till en `str` → byt ut i strängen → konvertera tillbaka till `int`.
- Glöm inte att hantera fallet om talet är exakt `0` från början.

### Övning 13: Censurfunktion

1. Skapa en funktion `censor(text: str, banned_words: list[str]) -> str`.
2. Den ska ersätta alla förekomster av ord i `banned_words` med ett valfritt censur-ord, t.ex. `***`.
3. Testa funktionen med både korta och lite längre meningar.

#### Exempel

```plaintext
Ange text: Jag gillar att programmera i Python
Ange ord att censurera (kommaseparerade): gillar,Python
Resultat: Jag *** att programmera i ***
```

#### Tips

- Var försiktig med hur du söker och ersätter ord. Tänk på skillnader mellan “Python” och “Pytho” (undvik att delmatchningar blir fel).
- Du kan använda `replace()`, men då måste du se till att du bara censurerar hela ord. Eventuellt behöver du splitta texten i en lista av ord först.

### Övning 14: Funktion för att byta alla `0` med `5`

Skriv en `funktion` med samma funktionalitet som i Övning 12.

Dela upp koden i två filer:

- `replace_zeros.py` (innehåller funktionen, som tar in och returnerar en `int`)
- `main.py` (använder funktionen och skriver ut dess returvärde)

Använd `import` för att anropa funktionen i huvudfilen.

#### Exempel

```plaintext
Ange ett tal: 1020340
Resultat: 1525345
```

#### Tips

- Tänk på att du måste göra en stränghantering för att kunna byta ut tecken.
- Du kan därefter behöva konvertera tillbaka till en integer innan du returnerar resultatet.
- Se till att ta höjd för specialfall, t.ex. om talet är 0 från början.

### Övning 15: Fibonaccis talserie med loop

Skriv en funktion `fibonacci(n: int) -> list[int]` som returnerar en **lista** med de `n` första Fibonacci-talen.

Dela upp koden i två filer, exempelvis:

- `fibonacci.py` (innehåller funktionen)
- `main.py` (använder funktionen)
- Använd `import` för att använda funktionen i din huvudfil (`main.py`).

#### Exempel

```plaintext
Ange hur många Fibonacci-tal som ska genereras: 7
Fibonacci-serie: [0, 1, 1, 2, 3, 5, 8]
```

#### Tips

- Du kan börja din serie med `[0, 1]` och sedan lägga till nya tal i en loop.
- Tänk på vad som händer om `n` är väldigt litet (t.ex. 0 eller 1). Hur hanterar du sådana fall?

Ge inte upp om det tar tid att få loopen rätt – tänk på att varje nytt Fibonacci-tal är summan av de två föregående.

### Övning 16: Fibonaccis talserie med rekursion

Gör övning 15 först för att bekanta dig med fibonaccis talserie.

Skriv en **rekursiv** funktion `fibonacci_recursive(n: int) -> int` som returnerar det `n`-te Fibonacci-talet (inte en lista, utan **ett** tal).

Dela även här upp koden i två filer:

- `fibonacci_rec.py` (innehåller funktionen)
- `main.py` (använder funktionen)
- Använd `import` för att kunna anropa funktionen från `main.py`.

#### Tips

- Kom ihåg att rekursion kräver ett eller flera **basfall**. Fundera på vad som händer när `n` är 0 eller 1.
- Ett rekursiv anropsträd kan snabbt växa sig stort om `n` blir stort, så var försiktig med allt för höga värden.
- Testa med små värden först och använd gärna `print`-satser eller debugger för att se hur funktionen körs.

[1]: https://docs.python.org/3.11/library/functions.html
[2]: https://docs.python.org/3.11/library/stdtypes.html
[3]: https://docs.python.org/3.11/library/index.html
[4]: https://code.visualstudio.com/Docs/editor/debugging
