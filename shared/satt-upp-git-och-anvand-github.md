# Sätt upp Git och använd GitHub

Instruktionerna förutsätter och rekommenderar att ditt operativsystem är inställt på engelska.

## Git

### Windows

1. Besök [git-scm.com][1].
    - Gå vidare till `Downloads`, följt av `Windows`, och ladda ner installationsfilen `Standalone Installer` eller `64-bit Git for Windows Setup`.
2. Starta installationen.
    - Standardinställningarna kan användas utan ändringar. Om det verkar praktiskt kan du välja `Use the Nano editor by default` när möjligheten ges.
3. Starta `PowerShell` eller `Command Prompt`.
4. Säkerställ att `git --version` svarar med att skriva ut vilken version du installerat.

### Ubuntu / Debian Linux

1. Starta ett valfritt kommandoskal som t.ex. `Bash` och installera Git med: `sudo apt update && sudo apt install git`
2. Säkerställ att `git --version` svarar med att skriva ut vilken version du installerat.

## Alla operativsystem

### Grundläggande konfiguration

Välj om du vill använda din studentmejl eller en privat e-postadress.

Konfigurera sedan Git med ditt namn och den valda e-postadressen genom att köra följande kommandon, där du ersätter texten inom citattecknen:

```bash
git config --global user.name "Ditt Namn"
git config --global user.email "din.email@exempel.com"
```

Gör därefter dessa ytterligare inställningar:

```bash
git config --global pull.rebase true
git config --global push.default current
```

[SSH-nycklar][2] möjliggör en säker anslutning till GitHub och gör det enklare att hantera kod utan att behöva ange lösenord varje gång. Följ länkarna nedan för att skapa ett GitHub-konto och konfigurera SSH så att du kan ansluta med Git.

1. [Skapa ett konto på GitHub][3]
2. [Generera en ny SSH-nyckel och lägg till den i ssh-agent][4]. Följ dokumentet från början fram till rubriken "Adding your SSH key to the ssh-agent". Gör inte det som står i den rubriken eller längre ned på sidan.
3. [Lägg till en ny SSH-nyckel till ditt GitHub-konto][5]
4. [Testa din SSH-anslutning][6]

## Lär dig använda Git

Git är mycket populärt, så det finns en mängd bra resurser för att lära sig använda det, både i form av artiklar och videor.

- **För dig som föredrar en kort introduktion**:  [Learning Git Basics][7].
- **För dig som vill fördjupa dig**: [Pro Git - Book][8] är en utförlig guide som tar upp både grunder och avancerade koncept.
- **För dig som vill lära dig via videor**: Sök efter "Git tutorial for beginners" på YouTube för videor som förklarar grunderna steg för steg.

### Vanliga Git-kommandon

Här är några av de mest använda Git-kommandona som kan hjälpa dig att komma igång:

- `git clone <URL>`: Klonar ett existerande repository till din dator.
- `git add <filnamn>`: Lägger till en fil till staging-området, så att den är redo att committas.
- `git commit -m "Meddelande"`: Skapar en commit med ett beskrivande meddelande om ändringarna.
- `git push`: Skickar dina commits till den fjärranslutna repositoryt, t.ex. GitHub.
- `git pull`: Hämtar och integrerar ändringar från fjärranslutna repositoryt till din lokala kopia.

### Git i grupparbeten

I projektet utgår du alltid från huvud-branchen `main`. Du skapar en ny branch för dina ändringar och gör dina commits där. När du är klar öppnar du en "Pull Request" på GitHub. När ändringarna är granskade och godkända mergar du tillbaka dem till `main`.

1. **Skapa en ny branch**

   ```bash
   git checkout -b minbranch
   ```

2. **Gör din förändring i koden**

3. **Lägg till och commit:a ändringarna**

   _Spara ändringarna med ett beskrivande commit-meddelande som skrivs som en uppmaning och följer [Conventional Commits][11]._

   ```bash
   git commit -m "feat: add pinky ghost"
   ```

4. **Skicka upp din branch till GitHub**

   ```bash
   git push
   ```

5. **Skapa en Pull Request (PR)**

   - Gå till GitHub och skapa en PR från din branch till `main`.

   - Be resten av gruppen om en "Approval" - minst en godkännande krävs.

6. **Granska och ge feedback**

   - Övriga gruppmedlemmar tittar på "Files changed" i PR:n.
   - De kan lämna kommentarer och godkänna PR:n.

7. **Mergea PR:n när den är godkänd**

   - Den som skapade PR:n klickar på "Merge" på GitHub. Det är så man gör på riktigt för att kunna följa sin egen förändring och vara beredd om något trots allt går fel.

8. **Städa efter merge**

   - Ta bort din branch på GitHub (GitHub har en knapp för detta).
   - Byt tillbaka till `main`, hämta senaste versionen och radera din branch lokalt:

```bash
   git checkout main
   git pull
   git branch -d minbranch
```

9. **Repetera**

   - Gör en ny branch för nästa förändring och följ samma process igen.

#### Git-konflikter

En Git-konflikt uppstår när två personer ändrar samma rad i en fil och Git inte vet vilken version som ska gälla. Det händer ofta vid git pull eller merge när ändringarna krockar och måste lösas manuellt.

Exempel:

- Person A ändrar en rad i index.html, commitar och pushar.
- Person B har samtidigt ändrat samma rad.
- Person B gör git pull och får en konflikt eftersom Git inte kan välja vilken version som ska behållas.

##### Hur ser konfliktmarkeringarna ut?

När en konflikt uppstår lägger Git in så kallade konfliktmarkeringar direkt i koden. Filen kan till exempel se ut så här:

  ```txt
  <<<<<<< HEAD
  <h1>Hej från Person B</h1>
  =======
  <h1>Hej från Person A</h1>
  >>>>>>> main
  ```

- `<<<<<<< HEAD` visar innehållet från din lokala version (den som du just nu jobbar på i din branch).
- `=======` är avgränsaren mellan de två konflikterande versionerna.
- `>>>>>>> main` (eller namnet på den andra branchen) visar ändringen som finns i den andra versionen (i det här fallet `main`).

Du behöver manuellt gå in och bestämma vad som ska stå kvar i filen, och därefter ta bort dessa konfliktmarkeringar innan du kan fortsätta med din merge eller pull.

##### Steg för steg: Så här hanterar du en konflikt lokalt

1. **Uppdatera din lokala kopia**

   Använd kommandot `git pull` för att hämta och försöka merga de senaste ändringarna från exempelvis `origin/main` in i din nuvarande branch.

   ```bash
   git pull
   ```

   Om konflikter hittas kommer Git att meddela vilka filer som är drabbade.

2. **Leta reda på konfliktmarkeringarna**

   Öppna de filer som Git pekar ut och leta efter markeringarna `<<<<<<<`, `=======` och `>>>>>>>`.

3. **Redigera filen**

   Bestäm vilken kod som ska behållas. Du kan välja din egen version, den andra personens version eller en kombination. Ta därefter bort konfliktmarkeringarna så att filen blir “ren”.

4. **Lägg till (stage) filen**
   När du är nöjd med din lösning och filen är fri från konfliktmarkeringar, lägger du till filen för commit:

   ```bash
   git add <filnamn>
   ```

   eller alla uppdaterade filer på en gång:

   ```bash
   git add .
   ```

5. **Skapa en commit**

   Gör en commit som beskriver att du har löst konflikten. Det går bra att behålla det autogenererade commit-meddelandet:

   ```bash
   git commit
   ```

6. **Pusha ändringarna**
   Skicka sedan upp dina ändringar till GitHub:

   ```bash
   git push
   ```

Efter detta bör din merge vara slutförd, förutsatt att inga fler konflikter uppstår.

##### Att lösa konflikter på GitHub

Ibland vill man lösa konflikter direkt på GitHubs webbgränssnitt - till exempel i ett pull request. När GitHub upptäcker en konflikt i en pull request visas ofta en knapp med texten “Resolve conflicts” eller liknande.

1. **Öppna pull requesten**

   Gå till fliken “Pull Requests” i ditt GitHub-repo och öppna den aktuella pull requesten.

2. **Klicka på “Resolve conflicts”**

   GitHub visar då filen eller filerna med konfliktmarkeringar, ungefär som du skulle sett dem lokalt.

3. **Redigera filen direkt i webbläsaren**

   Precis som lokalt väljer du vilken version av koden du vill behålla, eller om du vill kombinera båda. Ta bort konfliktmarkeringarna och se till att filens innehåll är korrekt.

4. **Markera konflikten som löst**

   När du är klar klickar du på knappen för att markera konflikten som löst och bekräftar sedan förändringarna.

5. **Genomför (merga) pull requesten**
   Nu kan du avsluta pull requesten med en merge, förutsatt att alla andra eventuella kontroller eller godkännanden är uppfyllda.

Om du har många eller svåra konflikter är det ofta enklare att lösa dem lokalt på din dator. Då har du din vanliga editor och verktyg som gör det lättare att se och fixa konfliktmarkeringarna.

#### Varför måste konflikter lösas?

Git klarar oftast att slå ihop ändringar automatiskt, så länge ni inte ändrat samma rad. Men om två ändringar krockar på samma ställe kan Git inte välja åt er. Då måste ni själva bestämma vad som ska behållas eller hur ändringarna ska kombineras.

## Valfri fördjupning

### Hantera och ångra ändringar

#### Användning av git revert för att ångra ändringar

Om du vill ångra en tidigare commit kan du använda `git revert`. Detta skapar en ny commit som motsvarar en omvändning av den angivna commiten, utan att ändra historiken.

```bash
git revert <commit-hash>
```

Till exempel, om du vill ångra commiten med hash `abc123`, skriver du:

```bash
git revert abc123
```

Detta gör att den ursprungliga commiten ångras men finns kvar i historiken, vilket är viktigt för att bibehålla en konsekvent och spårbar historik.

#### Ändra Git-historiken

Du kan bara ändra Git-historiken i ett repository om ingen annan har hämtat den med `git pull`. Eftersom Git är distribuerat orsakar det stora problem om du ändrar historiken som andra redan bygger på.

Det är oftast okej att göra ändringar på din egen branch som inte har mergats. Men efter en merge bör du aldrig göra det. Använd då istället `git revert` som nämndes ovan och läs mer om hur det fungerar.

#### Ändra den senaste committen

Det finns tillfällen då du kanske vill justera den senaste committen, antingen för att korrigera ett meddelande eller för att lägga till fler ändringar. Här är två sätt att göra detta. **Observera att dessa kommandon ändrar Git-historiken**. Se till att läsa sektionens huvudrubrik igen innan du använder dem.

##### Justera den senaste committen med `git commit --amend`

Om du vill lägga till ändringar till den senaste committen eller ändra commit-meddelandet kan du använda `git commit --amend`. Detta kommando öppnar en redigerare där du kan ändra meddelandet, och eventuella ändringar som har stagats (`git add`) inkluderas i den senaste committen.

###### Exempel

```bash
# Lägg till filer att inkludera i den senaste committen
git add <filnamn>

# Ändra den senaste committen
git commit --amend
```

Det här kommandot skapar en ny commit som ersätter den senaste, vilket innebär att commit-historiken uppdateras. Använd detta endast om den ändrade committen inte har pushats till en delad remote repository.

##### Ångra den senaste committen med `git reset --soft`

Om du vill ångra den senaste committen men behålla filändringarna i staging-området kan du använda `git reset --soft`. Detta är användbart om du vill dela upp eller ändra innehållet i den senaste committen.

###### Exempel

```bash
git reset --soft HEAD~1
```

`git reset --soft` flyttar tillbaka `HEAD` till den senaste committen utan att ändra de faktiska filerna i ditt arbetsområde. Dina ändringar förblir staged, så att du kan göra en ny commit med justerat innehåll eller meddelande.

Om du vill unstagea filändringar efter att ha använt `git reset --soft` kan du köra:

```bash
git reset
```

Detta flyttar filerna från staging-området tillbaka till ditt arbetsområde, så att de inte längre är stagade men behåller ändringarna.

##### Ändra historiken med en interaktiv rebase

Interaktiv rebase (`git rebase -i`) är ett kraftfullt verktyg för att ändra commit-historiken. **Observera att detta ändrar Git-historiken**. Se till att läsa sektionens huvudrubrik igen innan du använder det. Med interaktiv rebase kan du:

- **Ändra commit-meddelanden** så att de ser ut som du vill.
- **Slå ihop commits** för att städa upp historiken och göra den mer sammanhängande.
- **Ändra innehållet i en commit** genom att dela upp, redigera eller lägga till nya ändringar.

###### Exempel

```bash
git rebase -i HEAD~3
```

Detta öppnar en redigerare där du kan välja hur de senaste tre commitarna ska hanteras. Du kan till exempel ändra `pick` till `edit` för att redigera en commit, eller till `squash` för att slå ihop den med föregående commit.

###### Verktyg för interaktiv rebase

Även om du kan arbeta med interaktiv rebase direkt i terminalen, kan det vara värt att titta på  [Git Interactive Rebase Tool][9]. Detta verktyg har ett mer användarvänligt gränssnitt och underlättar hanteringen av rebases. Det går utmärkt att arbeta med interaktiva rebases utan detta verktyg, men det är ett mycket bra verktyg som förtjänar att nämnas.

### Länkar

- Git
  - [Pro Git - Book][8]
- GitHub
  - [Get started][10]

[1]: https://git-scm.com/
[2]: https://docs.github.com/en/authentication/connecting-to-github-with-ssh/about-ssh
[3]: https://docs.github.com/en/get-started/start-your-journey/creating-an-account-on-github
[4]: https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent
[5]: https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account
[6]: https://docs.github.com/en/authentication/connecting-to-github-with-ssh/testing-your-ssh-connection
[7]: https://github.com/git-guides#learning-git-basics
[8]: https://git-scm.com/book/en/v2
[9]: https://github.com/MitMaro/git-interactive-rebase-tool
[10]: https://docs.github.com/en/get-started
[11]: https://www.conventionalcommits.org/
