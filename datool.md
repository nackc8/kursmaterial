# `datool` följer upp de flesta individuella krav i projektarbetet

I ert repo finns ett verktyg som heter `datool`. Det räknar automatiskt (via
Git + GitHub) fram hur varje person har bidragit till projektet.

Det viktigaste:

- `datool`-statistiken används för att avgöra de krav som verktyget
  kan mäta.
- För samarbete måste ni använda `Co-authored-by:` i commits. Om ni glömmer
  det räknas arbetet som ensamarbete av den som gjorde commiten.

## Hur `datool` mäter

`datool` tittar på:

- **Rader kod** som finns kvar i projektets nuvarande version (via `git blame`).
- **Commits** som ändrar filer som matchar valt filurval (endast
  icke-tomma ändringar).
- **Pull requests** som innehåller sådana ändringar.
- **Samarbete** i commits via `Co-authored-by:`.

### Filurval med glob-mönster

Vilka filer som räknas styrs av glob-mönster som utbildaren ställt in. Det
påverkar alla siffror i tabellen: bara filer som matchar *Include* och
**inte** matchar *Exclude* tas med.

- `**` betyder “valfritt antal katalognivåer”.
- `*.ext` betyder “alla filer med ändelsen `.ext`.

Exempel:

```plaintext
Include: **/*.java
Exclude: **/src/test/**, **/generated/**
```

Och om projektet även innehåller Python:

```plaintext
Include: **/*.java, **/*.py
Exclude: **/venv/**, **/__pycache__/**, **/generated/**
```

### Viktiga begränsningar

- Endast ändringar i filer som matchar valt filurval räknas (och PR:er som
  innehåller sådana ändringar).
- Vi räknar bara **icke-tomma** ändringar (blankrader och mellanslag räknas
  inte).

## Så kopplas `datool`s tabell till individuella krav

`datool` visar en tabell med kolumner som ungefär betyder:

| Kolumn        | Betyder                                                                           |
| ------------- | --------------------------------------------------------------------------------- |
| **C.Alone**   | Commits du gjort själv (utan co-authors) som innehåller ändringar i räknade filer |
| **C.Collab**  | Commits där du är author **eller** korrekt nämnd som co-author                    |
| **PR.Alone**  | Pull requests du skapat utan co-authors i commits                                 |
| **PR.Collab** | Pull requests du skapat där minst en commit har co-authors                        |
| **PR.Appr**   | Pull requests du har godkänt (review/approval)                                    |
| **L.Alone**   | Rader kod som just nu räknas som dina från ensam-commits                          |
| **L.Collab**  | Rader kod som just nu räknas till dig och tillammans med co-authors               |

### Vad verktyget kan avgöra

`datool` kan visa underlag för individuella krav som till exempel:

- Att du gjort ett visst antal commits som ändrar räknade filer (titta
  på **C.Alone + C.Collab**).
- Att du skapat ett visst antal pull requests med räknade ändringar (titta
  på **PR.Alone + PR.Collab**).
- Att du godkänt minst ett visst antal pull requests (titta på **PR.Appr**).
- Att du har ett visst antal icke-tomma rader kod som finns kvar i
  slutversionen (titta på **L.Alone + L.Collab**).

Utöver detta kan varje kurs/projekt även ha andra krav som kräver ett
mänskligt avgörande. Alla krav tillsammans måste uppfyllas.

Med andra ord så måste verktyget indikera att de maskinellt avgörbara kraven
är uppfyllda. Dessutom så måste andra krav som godkänns av utbildaren
uppfyllas.

## Det är normalt att antal rader du bidragit med minskar över tid

`datool` räknar rader som **finns kvar i projektets nuvarande version**.

Det betyder:

- Om du skriver 200 rader idag och gruppen senare refaktorerar, flyttar eller
  tar bort kod, så kan din siffra minska.
- Det är **helt naturligt** i ett riktigt utvecklingsarbete.

Det är den slutgiltiga versionen av projektet som ni lämnar in som används
för att se om kraven uppfyllts. Om en student klarat kravet på antal bedragna
rader tidigare under projektet men inte i dess slutgiltiga version, så har
denne **inte** klarat kravet.

## Samarbete redovisas endast genom `Co-authored-by:`

När ni jobbar tillsammans (parprogrammering, gemensam lösning osv) ska
commiten innehålla `Co-authored-by:` för alla som deltog men inte gjorde
själva `git commit`.

Exempel (korrekt format):

```plaintext
feat: add high score list

Co-authored-by: Förnamn Efternamn <person@example.com>
Co-authored-by: Förnamn Efternamn <annan@example.com>
```

Om ni glömmer detta:

- Då räknas commiten och raderna bara till den som gjorde commiten.
- Inga undantag görs i en sådan situation.

## Var ser jag statistiken på GitHub?

Workflowet heter **Participation Stats** och körs för:

- push till `main`
- pull requests
- manuell körning (workflow_dispatch)

Det finns ett jobb som heter **stats** och ett steg som heter **Run `datool`**.

### För `main`

1. Gå till repot på GitHub.
2. Klicka på fliken **Actions**.
3. Klicka på workflowet **Participation Stats**.
4. Öppna den senaste körningen.
5. Öppna fliken **Summary**.

   - Där finns en sektion som heter **Participation Stats** med hela
     utmatningen.
6. Om du hellre vill se loggarna: öppna jobbet **stats** och expandera
   steget **Run**.

### För en pull request

1. Öppna pull requesten.
2. Scrolla till **Checks**.
3. Klicka **Details** för checken som hör till workflowet **Participation
   Stats** (jobbet **stats**).
4. Öppna fliken **Summary** för körningen.

   - Där finns en sektion som heter **Participation Stats** med hela
     utmatningen.
5. Alternativt: öppna loggarna för jobbet **stats** och expandera
   steget **Run**.

Exempel på utmatning:

```plaintext
$ datool --github .

Include: **/src/main/java/**/*.java
Exclude: **/src/test/**, **/generated/**, **/legacy/**

Student summary:
  C=Commits, L=Lines, Alone=without co-authors, Collab=with co-authors
ID  Name                 PR.Alone   PR.Collab   PR.Appr   C.Alone   C.Collab   L.Alone   L.Collab
-------------------------------------------------------------------------------------------------
1   Elin Bergström             2           1         1         4          2        61          9
2   Amir Hassan                1           1         2         2          1        33         22
3   Sofia Novak                0           2         1         1          3        14         48
4   Lucas Ferreira             3           0         1         5          0        92          0
5   Mei Chen                   0           1         3         0          2         0         55

File details:
  A=Alone (no co-authors), C=Collab (with co-authors), number=line count
   1        2        3        4        5     File                                    Commit
---------------------------------------------------------/-/-------------------------------

src/main/java/com/example/pomofx/
                                       A8    MainApp.java                          8e4c9a1b
                                      A12    AppLauncher.java                      8e4c9a1b

src/main/java/com/example/pomofx/timer/
  A4               A31               C2     PomodoroEngine.java                    4b1d7f3c
                                                                                   0c9e21aa
                                                                                   f2a8d0e1

src/main/java/com/example/pomofx/ui/
A19,C1  A1,C1      C1       C1     A3,C1   TimerController.java                    91a7c3d2
                                                                                   8e4c9a1b
                                                                                   6f2b10c9
                                                                                   2aa6d4f0
                                                                                   5d7c19e4
 A13                                  A2    SettingsController.java                4b1d7f3c
                                                                                   2aa6d4f0
                                                                                   5d7c19e4

src/main/java/com/example/pomofx/settings/
                                       A9    SettingsStore.java                    6f2b10c9
                                                                                   0c9e21aa

src/main/java/com/example/pomofx/audio/
                                      A10    SoundPlayer.java                      f2a8d0e1

src/main/java/com/example/pomofx/util/
                                       A7    TimeFormatters.java                   a1b9e6d7
                                      A11    Validation.java                       2aa6d4f0

Commits:
Hash       Date         Message
-------------------------------------------------------------------------------------------------
5d7c19e4   2026-01-17   feat(ui): add quick start presets
2aa6d4f0   2026-01-16   fix: validate custom durations
6f2b10c9   2026-01-16   feat(settings): persist theme and notifications
f2a8d0e1   2026-01-16   feat(audio): add end-of-session sound
0c9e21aa   2026-01-15   refactor(timer): simplify state transitions
91a7c3d2   2026-01-15   feat(ui): initial timer screen
a1b9e6d7   2026-01-14   chore: add time formatting helpers
4b1d7f3c   2026-01-13   feat(timer): add PomodoroEngine
8e4c9a1b   2026-01-13   chore: create JavaFX app skeleton
```

## `.students.json` – viktigt om namn/e-post blir fel

`datool` måste kunna koppla commits och GitHub till rätt person. Det görs
via filen `.students.json` i repot.

- Om du råkar committa med “fel namn” eller “fel e-post” (t.ex. annan
  dator, annan IDE, annan Git-konfiguration), så kommer du kunna se att
  statistiken blir fel.
- Då behöver du lägga till det alternativa namnet/e-posten
  i `.students.json`.

Exempel med `other_names` och `other_emails`:

```json
{
  "students": [
    {
      "id": "1",
      "name": "Ditt Namn",
      "email": "din.epost@example.com",
      "github_username": "DittGitHubNamn",
      "other_names": [
        "Annat Namn"
      ],
      "other_emails": [
        "othermail@outlook.com"
      ]
    }
  ]
}
```

Ni kan också:

- Lägga till fler e-postadresser/användarnamn om någon byter dator eller har
  flera konton.

Tips för att se vilka e-postadresser som faktiskt finns i historiken:

```bash
git log --format='%ae' | sort -u
```

## Köra `datool` lokalt på din dator

Det går bra att köra lokalt för att snabbt se din status.

### Förutsättningar

- Du behöver ha **Python 3** installerat.

  - Anteckning: kommandot kan heta `python` eller `python3` beroende på system.
- Du behöver ha kommandot `git` installerat och tillgängligt i din `PATH`.
- Om du vill hämta **GitHub-statistik** (PR:er m.m.) behöver du även
  **GitHub CLI**-kommandot `gh` i din `PATH` och vara inloggad.

  - Länk: [https://cli.github.com/](https://cli.github.com/)

```bash
cd /sokvag/till/ert-repo
python .github/scripts/datool`.py .
```

Med PR-statistik:

```bash
export GH_TOKEN=$(gh auth token)
python .github/scripts/`datool`.py --github .
```

## Sammanfattning

- `datool` räknar individuella bidrag utifrån Git + GitHub och utifrån det
  filurval som används vid körning.
- Använd alltid `Co-authored-by:` när ni samarbetar.
- Håll `.students.json` uppdaterad om någon råkar committa med
  annat namn/e-post.
- Kom ihåg att rader (L.*) kan minska under projektet – det är normalt.
