# Filhantering och skript med argparse: Övningsuppgifter

I dessa övningar kommer du att öva på att skapa egna skript och tolka skriptets argument. Du kommer även att träna på att hantera filer och olika filformat.

## Inför övningsuppgifterna

### Att göra i varje övning

#### Körbara skript

Ställ in din dator så att de skript du skapar kan köras direkt utan att behöva skriva python eller python3 innan.

- I Windows måste .py-filer vara associerade med `python.exe`.
- På MacOS och Linux behöver filen ha körrättigheter samt en shebang: `#!/usr/bin/env python3`.

#### Tolka skriptets argument

Om skriptet är enkelt kan du tolka sys.argv direkt. I de flesta fall blir koden dock tydligare och bättre med argparse. Gör "Argparse Tutorial" och använd modulens dokumentation flitigt.

För skript som har optioner (som `--something`) och även tar filnamn bör `--` användas som avskiljare mellan optioner och filnamn. Indikatorn `--` markerar  att det som följer efter den endast är filnamn. Detta förhindrar att en fil med ett namn som liknar en option (t.ex. `--something`) tolkas felaktigt som en option.

#### Läs dokumentation

Läs dokumentationen för de delar av språket som du använder. Dessa är:

- [Inbyggda funktioner][1], de funktioner som är inbyggda och finns tillgängliga från början utan att behöva göra `import`.
- [Inbyggda datatyper][2] som `int` och `str`.
- [Moduler i standardbiblioteket][3], det är de saker du använder som kräver att du importerar dem med `import` innan.

#### Använd debugläget i utvecklingsmiljön

Vänj dig vid debugverktyget. Sätt breakpoints, stega igenom koden och kontrollera variabelvärden. [Visual Studio Code][4] är till stor hjälp för att förstå och hitta fel.

## Övningsuppgifter

### Övning 1: echo

**Uppgift:**

1. Skriv ett eget kommando `echo` i valfritt språk (t.ex. Python eller C) som tar emot argument från kommandoraden och skriver ut dem.
2. Lägg till stöd för optionerna:
   - `-n`: Skriv inte ut något radmatningstecken i slutet.
   - `-e`: Tolka specialsekvenser:
     - `\\` → `\`
     - `\n` → radmatning
     - `\t` → tab-tecken

**Exempel på körning:**

```bash
$ echo.py Hej Linuxvärlden
Hej Linuxvärlden

$ echo.py -n Hej
Hej$

$ echo.py -e "Hej\nVärlden"
Hej
Världen
```

**Tips:**

- Använd t.ex. `sys.argv` i Python för att läsa in argumenten.
- För att tolka sekvenser som `\n` kan du behöva ersätta dem i strängen innan utskrift.

### Övning 2: head

**Uppgift:**

1. Skriv ett eget kommando `head` med synopsisen `head [OPTION]... [FILE]...`.
2. Som standard ska det skriva ut de **10 första raderna** i varje fil som skickas in.
3. Stöd för optionerna:
   - **-n, --lines=[-]NUM**
     - Utan minustecken: skriv **NUM** första raderna i stället för de första 10.
     - Med ett inledande `-`: skriv **alla utom** de sista **NUM** raderna.
   - **-v, --verbose**
     - Skriv alltid en header som talar om vilket filnamn som bearbetas.

**Exempel på körning:**

```bash
$ head.py fil1.txt
... (de 10 första raderna) ...

$ head.py -n 5 fil1.txt
... (de 5 första raderna) ...

$ head.py --lines=-3 fil1.txt
... (alla utom de sista 3 raderna) ...

$ head.py -v fil1.txt fil2.txt
==> fil1.txt <==
... 10 första rader ...

==> fil2.txt <==
... 10 första rader ...
```

**Tips:**

- Använd filhantering (`with open(...) as f:` i Python eller motsvarande) för att läsa in rad för rad.
- Tänk på att hålla ordning på filnamn när du skriver ut headers (när `-v` är satt).

### Övning 3: Tom

Med avsikt lämnad tom.

### Övning 4: tail

**Uppgift:**

1. Skriv ett kommando `tail` som fungerar på motsvarande sätt som `head`, men som istället skriver ut de **sista** raderna i filen.
2. Ha stöd för samma optioner som i `head`:
   - **-n, --lines=[-]NUM**
   - **-v, --verbose**

**Exempel på körning:**

```bash
$ tail.py fil1.txt
... (de 10 sista raderna) ...

$ tail.py -n 5 fil1.txt
... (de 5 sista raderna) ...

$ tail.py --lines=-3 fil1.txt
... (alla utom de första 3 raderna) ...

$ tail.py -v fil1.txt fil2.txt
==> fil1.txt <==
... 10 sista rader ...

==> fil2.txt <==
... 10 sista rader ...
```

**Tips:**

- Ett enkelt sätt att hantera sista rader kan vara att läsa in allt och sedan ta ut önskat antal rader från slutet.
- Tänk på att även här skriva ut headers vid behov.

### Övning 5: find

**Uppgift:**

1. Skriv ett kommando `find` som söker igenom underkataloger efter filer eller kataloger.
2. Använd gärna modulen `glob` (i Python) eller liknande funktioner i andra språk för att hitta matchande filer och mappar.
3. Stöd för optioner (alla är valfria):
   - **-i, --in SOKORD**: Visa endast träffar där fil- eller katalognamnet **innehåller** `SOKORD`.
   - **-b, --begin SOKORD**: Visa endast träffar där namnet **börjar** med `SOKORD`.
   - **-e, --end SOKORD**: Visa endast träffar där namnet **slutar** med `SOKORD`.
   - **-f, --file**: Visa endast träffar som är **filer**.
   - **-d, --directory**: Visa endast träffar som är **kataloger**.
     *OBS: `-f` och `-d` är “mutually exclusive”, så de kan inte anges samtidigt.*

**Exempel på körning:**

```bash
$ find.py
.             # Skriver ut allt i nuvarande mapp och underkataloger

$ find.py -i test
./tests
./src/test_main.py

$ find.py -b data -f
./data.txt
./data_config.json
```

**Tips:**

- För att skilja på filer och kataloger kan du använda `os.path.isfile()` och `os.path.isdir()` (Python) eller motsvarande i andra språk.
- Kontrollera att inte både `-f` och `-d` används samtidigt (avbryt med ett felmeddelande eller välj logik för prioritet).'

### Övning 6: Tom

Med avsikt lämnad tom.

[1]: https://docs.python.org/3.11/library/functions.html
[2]: https://docs.python.org/3.11/library/stdtypes.html
[3]: https://docs.python.org/3.11/library/index.html
[4]: https://code.visualstudio.com/Docs/editor/debugging
