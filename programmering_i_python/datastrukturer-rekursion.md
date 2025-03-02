# Datastrukturer och rekursion: Övningsuppgifter

I dessa övningar kommer du att öva på att använda datastrukturerna list, dict, duple, set. Du kommer även att gå djupare vad gäller hantering av lisor, funktioner och rekursion..

## Inför övningsuppgifterna

### Att göra i varje övning

#### Läs dokumentation

Läs dokumentationen för de delar av språket som du använder. Dessa är:

- [Inbyggda funktioner][1], de funktioner som är inbyggda och finns tillgängliga från början utan att behöva göra `import`.
- [Inbyggda datatyper][2] som `int` och `str`.
- [Moduler i standardbiblioteket][3], det är de saker du använder som kräver att du importerar dem med `import` innan.

#### Använd debugläget i utvecklingsmiljön

Vänj dig vid debugverktyget. Sätt breakpoints, stega igenom koden och kontrollera variabelvärden. [Visual Studio Code][4] är till stor hjälp för att förstå och hitta fel.

## Övningsuppgifter

### Övning 1: Dublettfilter med set

**Uppgift:**

1. Skriv en funktion `remove_duplicates(lst)` som tar en lista av strängar och/eller heltal.
2. Funktionen ska returnera en ny lista där alla dubletter är borttagna.
3. Använd gärna ett `set` för att förenkla filtreringen.

**Tips:**

- Tänk på ordningen – då den spelar roll kan du inte bara använda konvertera listan till `set` och tillbaka.

### Övning 2: Teckenfrekvens i en sträng

**Uppgift:**

1. Skriv en funktion `char_count(s)` som tar en sträng `s`.
2. Returnera en ordbok (dictionary) där nyckeln är ett tecken och värdet är antalet förekomster av tecknet.

**Exempel på körning:**

```python
>>> char_count("abca")
{"a": 2, "b": 1, "c": 1}
```

**Tips:**

- Iterera över varje tecken i strängen och uppdatera ordboken.
- Använd `dict.get(key, 0)` för att enklare öka en räknare.

### Övning 3: Frekvens och total (tuple) + procentuell förekomst

Denna övning kombinerar två steg:

1. **Frekvens och total antal tecken**: Skriv en funktion `char_count_with_total(s)` som:
   - Använder samma logik som i `char_count(s)`.
   - Returnerar en tuple `(frekvens_dict, totalt_antal_tecken)`.

2. **b) Beräkna procentuell förekomst**: Skapa en funktion `char_frequency(s)` som:
   - Anropar `char_count_with_total(s)` och packar upp tuple-värdena.
   - Räknar ut procentuella förekomster för varje tecken (t.ex. med list comprehension).
   - Returnerar resultatet i valfri struktur (t.ex. en lista av `(tecken, procent)`).

### Övning 4: Tom

Med avsikt lämnad tom.

### Övning 5: De vanligaste tecknen

**Uppgift:**

1. Skriv en funktion `most_common_chars(s, n)` som tar en sträng `s` och ett heltal `n`.
2. Hämta frekvenserna (t.ex. via din tidigare `char_count`-funktion).
3. Sortera utifrån hur ofta varje tecken förekommer.
4. Returnera de `n` vanligaste tecknen i en lista.

**Tips:**

- Använd `.items()` på din ordbok för att skapa en lista av `(tecken, frekvens)`.
- `sorted(..., key=lambda x: x[1], reverse=True)` kan hjälpa till att sortera i fallande ordning.
- Använd slicing för att plocka ut de `n` första.

### Övning 6: Balanserade parenteser

**Uppgift:**

1. Skriv en funktion `is_balanced(s)` som kollar om parenteser av typerna `()`, `{}`, `[]` är balanserade i strängen `s`.
2. Använd en lista som stack för att pusha på öppnande parenteser och poppa när du möter en stängande.

**Exempel på körning:**

```python
>>> is_balanced("()[]{}")
True
>>> is_balanced("(]")
False
```

### Övning 7: Räkna total längd rekursivt

**Uppgift:**

1. Skriv en funktion `nested_length(lst)` som tar en nästlad lista (listor i listor) med siffror eller andra element.
2. Räkna den totala mängden element genom rekursion.
3. Returnera summan av alla element, oavsett nivå.

**Exempel:**

```python
nested_length([1, 2, 3, [5, 6, [8, 9, 10]]])
# Ska returnera 10 (totalt antal element).
```

### Övning 8: Tom

Med avsikt lämnad tom.

### Övning 9: Summan av element i en nästlad lista

**Uppgift:**

1. Utgå från `nested_length(lst)`, men döp om funktionen till `nested_sum(lst)`.
2. Addera siffrorna rekursivt istället för att bara räkna dem.
3. Returnera totalsumman.

**Exempel:**

```python
nested_sum([1, 2, 3, [5, 6, [8, 9, 10]]])
# Ska returnera 44.
```

### Övning 10: Skillnad mellan två listor

**Uppgift:**

1. Skriv en funktion `list_difference(a, b)` som tar två listor `a` och `b`.
2. Returnera en ny lista med de element som finns i `a` men inte i `b`.

**Tips:**

- Använd gärna list comprehension eller `set` för att underlätta.

### Övning 11: Simulera uniq

**Uppgift:**

1. Skriv en funktion `uniq(lst)` som tar en lista och returnerar en ny lista.
2. Om listan innehåller **sammanhängande** dubbletter (t.ex. `["a", "a", "b", "b", "b", "c"]`) ska dessa dubbletter ersättas med en enda instans i resultatet.
3. I det angivna exemplet blir alltså resultatet `["a", "b", "c"]`.

### Övning 12: Tom

Med avsikt lämnad tom.

### Övning 13: Sortering med sökord

**Uppgift:**

1. Skriv en funktion `sort_with_keyword(lst, keyword)` som tar en lista av strängar och ett sökord.
2. De strängar som innehåller sökordet ska hamna först i listan vid sortering.
3. Övriga kommer efter.

**Tips:**

- Du kan använda en `key`-funktion vid sorteringen som kollar om sökordet finns i strängen eller inte.
- T.ex. `key=lambda x: 0 if keyword in x else 1`.

### Övning 14: Simulera tr

**Uppgift:**

1. Skriv en funktion `translate(s, mapping)` som tar en sträng och en ordbok (mapping) där nyckel är ett tecken och värde är ersättningen.
2. Returnera en ny sträng där varje tecken är utbytt enligt ordboken.
3. Om ett tecken inte finns i `mapping` kan du låta det vara oförändrat eller ta bort det – du väljer.

**Exempel:**

```python
mapping = {"a": "X", "b": ""}
translate("abcabc", mapping)
# Exempelretur: "XcXc" (om "b" tas bort)
```

### Övning 15: Filtrera strängar efter sökord

**Uppgift:**

1. Skriv en funktion `filter_strings(lst, keyword)` som tar en lista av strängar och ett sökord.
2. Returnera en ny lista med bara de strängar som innehåller sökordet.

**Tips:**

- En enkel list comprehension räcker ofta:

  ```python
  [s for s in lst if keyword in s]
  ```

### Övning 16: Tom

Med avsikt lämnad tom.

### Övning 17: Nästlade list comprehensions

**Uppgift:**

1. Skapa en lista av tuples där varje tuple är `(x, y, x*y)`.
2. `x` ska komma från `[1, 2, 3]` och `y` från `[4, 5, 6]`.
3. Använd nästlad list comprehension.

**Exempel på önskat resultat** (ordning kan variera):

```python
[(1,4,4), (1,5,5), (1,6,6), (2,4,8), (2,5,10), (2,6,12), (3,4,12), ...]
```

### Övning 18: Rekursion – förändra nästlad lista

**Uppgift:**

1. Skriv en rekursiv funktion `increment_nested(lst, inc)` som tar en nästlad lista och ett heltal `inc`.
2. Funktionen ska öka varje heltalselement i listan med `inc`, oavsett hur djupt nästlade de är.
3. Returnera den modifierade listan.

**Exempel:**

```python
increment_nested([1, [2, [3, 4]]], 1)
# Ska returnera [2, [3, [4, 5]]].
```

### Övning 19: Rekursion – med högre ordningens funktion

**Uppgift:**

1. Utöka `increment_nested` till en mer flexibel funktion `modify_nested(lst, modfn)` där `modfn` är en funktion som tar ett tal och returnerar ett nytt tal.
2. Om inget `modfn` skickas i>n kan du välja en default-funktion, t.ex. +1.
3. Se till att alla heltal i listan transformeras av `modfn`.

### Övning 20: Tom

Med avsikt lämnad tom.

[1]: https://docs.python.org/3.11/library/functions.html
[2]: https://docs.python.org/3.11/library/stdtypes.html
[3]: https://docs.python.org/3.11/library/index.html
[4]: https://code.visualstudio.com/Docs/editor/debugging
