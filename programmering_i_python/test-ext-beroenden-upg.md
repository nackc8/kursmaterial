# Test och externa beroenden

I dessa övningar tränar du på att använda verktyg för automatiserad testning och arbetssättet Test-Driven Development. Du övar också på att använda externa beroenden med `pip` samt virtuella, isolerade Python-miljöer med `venv`.

## Inför övningsuppgifterna

### Att göra i varje övning

#### Använd Test-Driven-Development (TDD)

Försök att använda TDD i så många övningar som möjligt. Även där det inte uttryckligen efterfrågas kan testning vara till nytta.

Att använda TDD innebär att följa Red-Green-Refactor-stegen:

- Skriv bara ett test i taget innan du implementerar ny funktionalitet.
- Kör testerna ofta och se till att allt fungerar innan du går vidare.
- När alla tester passerar kan du refaktorera för att förbättra koden.

#### Alternera mellan testverktyg

De flesta övningar kan dra nytta av testning. Även om du inte följer TDD strikt kan du skriva tester efter eget behov.

Försök att variera mellan `doctest`, `unittest` och `pytest` i övningar där inget specifikt verktyg anges. På så sätt lär du dig alla verktygen och får en bättre förståelse för när de passar bäst.

#### Läs dokumentation

Läs dokumentationen för de delar av språket som du använder. Dessa är:

- [Inbyggda funktioner][1], de funktioner som är inbyggda och finns tillgängliga från början utan att behöva göra `import`.
- [Inbyggda datatyper][2] som `int` och `str`.
- [Moduler i standardbiblioteket][3], det är de saker du använder som kräver att du importerar dem med `import` innan.

#### Använd debugläget i utvecklingsmiljön

Vänj dig vid debugverktyget. Sätt breakpoints, stega igenom koden och kontrollera variabelvärden. [Visual Studio Code][4] är till stor hjälp för att förstå och hitta fel.

## Övningsuppgifter

### Uppgift 1: Faker - generera fejkade profiler

Faker låter dig generera fejkad användardata som namn, e-postadresser, adresser och lorem ipsum-text.

- Beroende: `faker`
- Installera: `pip install faker`
- Dokumentation: [faker.readthedocs.io][5]

Skapa ett program som genererar 5 fejkade användarprofiler med namn, e-postadresser och adresser. Skriv ut varje profil i ett läsbart format.

#### Tips

Använd `faker.Faker()` och metoder som `.name()`, `.email()` och `.address()`.

### Övning 2: Hundålder till människoår (med `doctest`)

- Skriv en funktion `dog_age_to_human_years(age)` som konverterar en hunds ålder till människoår.
- De första två åren av en hunds liv motsvarar 10,5 människoår vardera.
- Därefter motsvarar varje hundår 4 människoår.
- Använd `doctest` för att testa funktionen.

#### Exempel

```plaintext
dog_age_to_human_years(1)  ->  10.5
dog_age_to_human_years(2)  ->  21.0
dog_age_to_human_years(3)  ->  25.0
```

#### Tips

Hantera specialfallet `age = 0`.

```python
def dog_age_to_human_years(age):
    """
    Konverterar en hunds ålder till människoår.

    >>> dog_age_to_human_years(1)
    10.5
    >>> dog_age_to_human_years(2)
    21.0
    >>> dog_age_to_human_years(3)
    25.0
    """
    # Implementera din lösning här

if __name__ == "__main__":
    import doctest
    doctest.testmod()
```

### Övning 3: Rövarspråket (med `unittest`)

- Skriv en funktion `to_robber_language(text)` som omvandlar en sträng till rövarspråket.
- Alla konsonanter ska dubbleras och ett "o" läggs in mellan dem.
- Exempel: `"hej"` blir `"hohejoj"`.

#### Exempel

```plaintext
to_robber_language("hej")     ->  "hohejoj"
to_robber_language("abc")     ->  "aoboc"
to_robber_language("Python")  ->  "PoPythothonon"
```

#### Tips

En konsonant är en bokstav som inte är en vokal (`a, e, i, o, u, y, å, ä, ö`).

```python
import unittest

def to_robber_language(text):
    # Implementera din lösning här
    pass

class TestRobberLanguage(unittest.TestCase):
    def test_translation(self):
        self.assertEqual(to_robber_language("hej"), "hohejoj")
        self.assertEqual(to_robber_language("abc"), "aoboc")
        self.assertEqual(to_robber_language("Python"), "PoPythothonon")

if __name__ == "__main__":
    unittest.main()
```

### Uppgift 4: Requests - hämta webbdata

Requests låter dig skicka HTTP-förfrågningar till webbplatser och API:er för att hämta data.

- Beroende: `requests`
- Installera: `pip install requests`
- Dokumentation: [requests.readthedocs.io][6]

Ladda ner och visa innehållet på `https://example.com`.

#### Tips

Använd `requests.get(url).text`.

### Övning 5: Primtal i ett intervall (med `pytest`)

- Skriv en funktion `prime_numbers_in_range(start, end)` som returnerar en lista med alla primtal mellan `start` och `end`.
- Ett primtal är ett heltal större än 1 som endast är delbart med 1 och sig självt.

#### Exempel

```plaintext
prime_numbers_in_range(10, 20)  ->  [11, 13, 17, 19]
prime_numbers_in_range(1, 10)   ->  [2, 3, 5, 7]
```

#### Tips

Använd en loop för att testa om ett tal är primtal.

```python
import pytest

def prime_numbers_in_range(start, end):
    # Implementera din lösning här
    pass

def test_prime_numbers():
    assert prime_numbers_in_range(10, 20) == [11, 13, 17, 19]
    assert prime_numbers_in_range(1, 10) == [2, 3, 5, 7]

if __name__ == "__main__":
    pytest.main()
```

### Uppgift 6: Pyfiglet - ASCII textbanner

Pyfiglet konverterar text till ASCII-konst med olika typsnitt.

- Beroende: `pyfiglet`
- Installera: `pip install pyfiglet`
- Dokumentation: [github.com/pwaller/pyfiglet][7]

Skriv ut "Welcome to Python!" som ASCII-konst.

#### Tips

Använd `pyfiglet.figlet_format("Welcome to Python!")`.

### Övning 7: Basketpoäng (med `doctest`)

- Skriv en funktion `basketball_score(points_2, points_3)` som räknar totalpoängen i en basketmatch.
- `points_2` är antalet gjorda tvåpoängare.
- `points_3` är antalet gjorda trepoängare.

#### Exempel

```plaintext
basketball_score(5, 3)   ->  19
basketball_score(10, 0)  ->  20
```

#### Tips

Formeln är `2 * points_2 + 3 * points_3`.

```python
def basketball_score(points_2, points_3):
    """
    Beräknar totalpoängen i en basketmatch.

    >>> basketball_score(5, 3)
    19
    >>> basketball_score(10, 0)
    20
    """
    # Implementera din lösning här

if __name__ == "__main__":
    import doctest
    doctest.testmod()
```

### Uppgift 8: Colorama - terminaltext i färger

Colorama låter dig lägga till färg i terminalutskrifter.

- Beroende: `colorama`
- Installera: `pip install colorama`
- Dokumentation: [pypi.org/project/colorama][8]

Simulera ett trafikljus genom att skriva ut "STOP" (röd), "READY" (gul) och "GO" (grön).

#### Tips

Använd `colorama.Fore.RED`, `Fore.YELLOW` och `Fore.GREEN`.

### Övning 9: Summan av jämna tal (med `doctest`)

- Skriv en funktion `sum_even_numbers(numbers)` som tar en lista av tal och returnerar summan av alla jämna tal.
- Dela gärna upp i två filer (t.ex. `sum_even_numbers.py` för funktionen och `main.py` för att anropa och testa med `doctest`), eller skriv allt i en fil om du föredrar det.
- Använd TDD: Skriv först ett `doctest` som misslyckas, implementera sedan koden så att testet klarar sig.

#### Exempel

```plaintext
sum_even_numbers([1, 2, 3, 4, 5, 6])  ->  12
sum_even_numbers([10, 15, 20, 25])    ->  30
sum_even_numbers([1, 3, 5])           ->   0
```

#### Tips

Ett tal är jämnt om `tal % 2 == 0`.

```python
def sum_even_numbers(numbers):
    """
    Returnerar summan av jämna tal i listan.

    >>> sum_even_numbers([1, 2, 3, 4, 5, 6])
    12
    >>> sum_even_numbers([10, 15, 20, 25])
    30
    >>> sum_even_numbers([1, 3, 5])
    0
    """
    # Implementera din lösning här

if __name__ == "__main__":
    import doctest
    doctest.testmod()
```

### Uppgift 10: Pillow - ändra storlek på en bild

Pillow är ett bibliotek för bildbehandling som låter dig redigera och manipulera bilder.

- Beroende: `pillow`
- Installera: `pip install pillow`
- Dokumentation: [pillow.readthedocs.io/en/stable][9]

Ändra storlek på en bild till 200x200 pixlar.

#### Tips

Använd `Image.open("your_image.jpg").resize((200, 200))`.

### Övning 11: Vänd en sträng (med `unittest`)

- Skriv en funktion `reverse_string(text)` som returnerar den omvända versionen av en given sträng.
- Använd `unittest` för att testa funktionen.
- Följ TDD-processen: skriv först ett test som ska misslyckas, implementera sedan koden.

#### Exempel

```plaintext
reverse_string("hello")   ->  "olleh"
reverse_string("Python")  ->  "nohtyP"
reverse_string("")        ->  ""
```

#### Tips

Du kan använda strängars slice-funktion: `text[::-1]`.

```python
import unittest

def reverse_string(text):
    # Implementera din lösning här
    pass

class TestReverseString(unittest.TestCase):
    def test_reverse_string(self):
        self.assertEqual(reverse_string("hello"), "olleh")
        self.assertEqual(reverse_string("Python"), "nohtyP")
        self.assertEqual(reverse_string(""), "")

if __name__ == "__main__":
    unittest.main()
```

### Uppgift 12: Emoji - konvertera ord till emojis

Emoji gör det möjligt att använda och manipulera emojis i Python.

- Beroende: `emoji`
- Installera: `pip install emoji`
- Dokumentation: [emoji.readthedocs.io][10]

Be användaren skriva en mening och ersätt ord som "happy" och "sad" med motsvarande emojis.

#### Tips

Använd `emoji.replace_emoji()` eller en ordbok för att koppla ord till emojis.

### Övning 13: Beräkna fakultet (med `pytest`)

- Skriv en funktion `factorial(n)` som returnerar `n!`.
- Kom ihåg att `factorial(0) = 1` och i övrigt är `factorial(n) = n * (n-1) * (n-2) * ... * 1`.
- Använd `pytest` för att testa funktionen med TDD.

#### Exempel

```plaintext
factorial(0)  ->  1
factorial(1)  ->  1
factorial(5)  ->  120
factorial(6)  ->  720
```

#### Tips

- Tänk på specialfallen `0` och `1`.
- Du kan lösa det rekursivt eller med en loop.

```python
import pytest

def factorial(n):
    # Implementera din lösning här
    pass

def test_factorial():
    assert factorial(0) == 1
    assert factorial(1) == 1
    assert factorial(5) == 120
    assert factorial(6) == 720

if __name__ == "__main__":
    pytest.main()
```

### Uppgift 14: PyWhatKit - handskriven text

PyWhatKit har funktioner för att skicka WhatsApp-meddelanden, konvertera text till handskrift och mer.

- Beroende: `pywhatkit`
- Installera: `pip install pywhatkit`
- Dokumentation: [github.com/Ankit404butfound/PyWhatKit][11]

Konvertera användarens textinmatning till en handskriftsbild.

#### Tips

Använd `pywhatkit.text_to_handwriting()`.

### Övning 15: Räkna vokaler i en sträng (med `doctest`)

- Skriv en funktion `count_vowels(text)` som räknar antalet svenska vokaler i en given sträng.
- De svenska vokalerna är: `a, e, i, o, u, y, å, ä, ö` (både stora och små).
- Använd `doctest` och TDD: skriv först testfallen i docstring, implementera sedan stegvis.

#### Exempel

```plaintext
count_vowels("hello")         ->  2
count_vowels("Python")        ->  2
count_vowels("AEIOUYÅÄÖ")     ->  9
count_vowels("xyz")           ->  0
```

#### Tips

- Tänk på både versaler och gemener.
- En enkel lösning är att konvertera strängen till gemener och sedan räkna vilka tecken som finns i en lista eller set av vokaler.

```python
def count_vowels(text):
    """
    Räknar antalet svenska vokaler i strängen. Vokalerna är a, e, i, o, u, y, å, ä, ö.

    >>> count_vowels("hello")
    2
    >>> count_vowels("Python")
    2
    >>> count_vowels("AEIOUYÅÄÖ")
    9
    >>> count_vowels("xyz")
    0
    """
    # Implementera din lösning här

if __name__ == "__main__":
    import doctest
    doctest.testmod()
```

### Uppgift 16: Rich - visa en laddningsindikator

Rich är ett bibliotek för att skriva ut vackert formaterad text, tabeller och syntax-markerad kod i terminalen.

- Beroende: `rich`
- Installera: `pip install rich`
- Dokumentation: [rich.readthedocs.io/en/stable][12]

Simulera en laddningsindikator.

#### Tips

Använd `rich.progress.Progress()`.

### Uppgift 17: Art - ASCII konstgenerator

Art låter dig skriva ut cool ASCII-konst och stiliserad text.

- Beroende: `art`
- Installera: `pip install art`
- Dokumentation: [github.com/sepandhaghighi/art][13]

Skriv ut "Python" i en snygg ASCII-stil.

#### Tips

Använd `art.text2art("Python")`.

### Uppgift 18: Faker - generera fejkade blogginlägg

Generera ett fejkblogginlägg med en slumpmässig titel och innehåll.

Beroende: `faker` (se Uppgift 1)

#### Tips

Använd `.sentence()` för titeln och `.paragraph(nb_sentences=5)` för innehållet.

### Uppgift 19: Requests - hämta ett slumpmässigt skämt

Hämta ett skämt från en skämt-API och visa det.

Beroende: `requests` (se Uppgift 2)

#### Tips

Använd `requests.get("https://official-joke-api.appspot.com/random_joke").json()`.

### Uppgift 20: Pyfiglet - visa användarinmatning i ASCII

Be användaren skriva en text och visa den som ASCII-konst.

Beroende: `pyfiglet` (se Uppgift 3)

#### Tips

Använd `input()` och `pyfiglet.figlet_format()`.

### Uppgift 21: Colorama - skriv ut en färgad hälsning

Skriv ut "Hello, Python!" i grönt.

Beroende: `colorama` (se Uppgift 4)

#### Tips

Använd `colorama.Fore.GREEN`.

### Uppgift 22: Pillow - visa en bild

Öppna en bildfil och visa den.

Beroende: `pillow` (se Uppgift 5)

#### Tips

Använd `Image.open("your_image.jpg").show()`.

### Uppgift 23: Emoji - skriv ut ett roligt emoji-meddelande

Skriv ett skript som skriver ut ett hälsningsmeddelande med emojis.

Beroende: `emoji` (se Uppgift 6)

#### Tips

Använd `emoji.emojize(":earth_globe_europe-africa:")`.

### Uppgift 24: PyWhatKit - skicka ett WhatsApp-meddelande

Skicka ett schemalagt meddelande via WhatsApp.

Beroende: `pywhatkit` (se Uppgift 7)

#### Tips

Använd `pywhatkit.sendwhatmsg()` (kräver WhatsApp Web).

### Uppgift 25: Rich - visa en formaterad tabell

Visa en tabell med tre kolumner: Namn, Ålder och Stad.

Beroende: `rich` (se Uppgift 8)

#### Tips

Använd `rich.table.Table()`.

### Uppgift 26: Art - generera slumpmässig ASCII-konst

Skriv ut en slumpmässig ASCII-bild.

Beroende: `art` (se Uppgift 9)

#### Tips

Använd `art.art("random")`.

[1]: https://docs.python.org/3.11/library/functions.html
[2]: https://docs.python.org/3.11/library/stdtypes.html
[3]: https://docs.python.org/3.11/library/index.html
[4]: https://code.visualstudio.com/Docs/editor/debugging
[5]: https://faker.readthedocs.io/en/master/
[6]: https://requests.readthedocs.io/en/latest/
[7]: https://github.com/pwaller/pyfiglet
[8]: https://pypi.org/project/colorama/
[9]: https://pillow.readthedocs.io/en/stable/
[10]: https://emoji.readthedocs.io/en/latest/
[11]: https://github.com/Ankit404butfound/PyWhatKit
[12]: https://rich.readthedocs.io/en/stable/
[13]: https://github.com/sepandhaghighi/art
