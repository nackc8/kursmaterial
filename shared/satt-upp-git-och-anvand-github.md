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
2. [Generera en ny SSH-nyckel och lägg till den i ssh-agent][4]
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

## Valfri fördjupning

### Hantera och ångra ändringar i Git-historiken

Du kan bara ändra Git-historiken i ett repository om ingen annan har hämtat den med `git pull`. Eftersom Git är distribuerat orsakar det stora problem om du ändrar historiken som andra redan bygger på.

Det är oftast okej att göra ändringar på din egen branch som inte har mergats. Men efter en merge bör du aldrig göra det. Använd då istället `git revert` och läs mer om hur det fungerar.

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

#### Ändra den senaste committen

Det finns tillfällen då du kanske vill justera den senaste committen, antingen för att korrigera ett meddelande eller för att lägga till fler ändringar. Här är två sätt att göra detta. **Observera att dessa kommandon ändrar Git-historiken**. Se till att läsa sektionens huvudrubrik igen innan du använder dem.

##### Justera den senaste committen med `git commit --amend`

Om du vill lägga till ändringar till den senaste committen eller ändra commit-meddelandet kan du använda `git commit --amend`. Detta kommando öppnar en redigerare där du kan ändra meddelandet, och eventuella ändringar som har stagats (`git add`) inkluderas i den senaste committen.

**Exempel:**

```bash
# Lägg till filer att inkludera i den senaste committen
git add <filnamn>

# Ändra den senaste committen
git commit --amend
```

Det här kommandot skapar en ny commit som ersätter den senaste, vilket innebär att commit-historiken uppdateras. Använd detta endast om den ändrade committen inte har pushats till en delad remote repository.

##### Ångra den senaste committen med `git reset --soft`

Om du vill ångra den senaste committen men behålla filändringarna i staging-området kan du använda `git reset --soft`. Detta är användbart om du vill dela upp eller ändra innehållet i den senaste committen.

**Exempel:**

```bash
git reset --soft HEAD~1
```

`git reset --soft` flyttar tillbaka `HEAD` till den senaste committen utan att ändra de faktiska filerna i ditt arbetsområde. Dina ändringar förblir staged, så att du kan göra en ny commit med justerat innehåll eller meddelande.

Om du vill unstagea filändringar efter att ha använt `git reset --soft` kan du köra:

```bash
git reset
```

Detta flyttar filerna från staging-området tillbaka till ditt arbetsområde, så att de inte längre är stagade men behåller ändringarna.

#### Ändra historiken med en interaktiv rebase

Interaktiv rebase (`git rebase -i`) är ett kraftfullt verktyg för att ändra commit-historiken. **Observera att detta ändrar Git-historiken**. Se till att läsa sektionens huvudrubrik igen innan du använder det. Med interaktiv rebase kan du:

- **Ändra commit-meddelanden** så att de ser ut som du vill.
- **Slå ihop commits** för att städa upp historiken och göra den mer sammanhängande.
- **Ändra innehållet i en commit** genom att dela upp, redigera eller lägga till nya ändringar.

**Exempel på interaktiv rebase:**

```bash
git rebase -i HEAD~3
```

Detta öppnar en redigerare där du kan välja hur de senaste tre commitarna ska hanteras. Du kan till exempel ändra `pick` till `edit` för att redigera en commit, eller till `squash` för att slå ihop den med föregående commit.

##### Verktyg för interaktiv rebase

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
