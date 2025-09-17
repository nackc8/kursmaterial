# Sätt upp Tailscale för att ansluta till din studentdator

Instruktionerna förutsätter och rekommenderar att ditt operativsystem är inställt på engelska.

## Tailscale

Tailscale är ett smidigt sätt att nå en dator via internet som annars inte går att ansluta till. De erbjuder en [generös gratisnivå][1] med upp till 100 enheter. För mer information om [vad Tailscale erbjuder][2] och [hur det fungerar tekniskt][3], se deras webbplats.

### Kom igång

1. Besök [tailscale.com][4].
2. Välj `Get Started` och skapa ett konto. Använd en privat mejl då SSO via Nackademin inte verkar fungera med tjänsten.
3. Gå till fliken `Machines`, välj `Add Device / Client Device`, och följ den enkla installationsguiden för ditt operativsystem.
4. Klienten möjliggör kommunikation mellan datorer i ditt Tailscale-nätverk. Observera att nätverket är privat och att ingen utomstående kan ansluta som standard.

Upprepa de sista stegen för alla enheter som ska kommunicera med varandra. Om du vill ansluta till din studentdator från din privata dator är dessa två en utmärkt start.

Ett tips är att du även kan installera klienten på virtuella maskiner för direkt åtkomst, vilket kan vara väldigt smidigt!

## SSH

### Instruktioner för datorerna som ska ta emot anslutningar

Gör det möjligt att ansluta via protokollet SSH genom att sätta upp OpenSSH Server. I dess enklaste form kan du köra dessa kommandon:

```bash
sudo apt install openssh-server
sudo ufw allow OpenSSH
sudo systemctl mask sleep.target suspend.target hibernate.target hybrid-sleep.target
```

Efter det så kan du ansluta till (standard för SSH) port `22`. Om du vill använda nyckelfiler för att ansluta, härda konfigurationen eller liknande så rekommenderas en webbsökning.

Det sista kommandot `systemctl mask ...` slår av energisparlägena eftersom de kan göra så att datorn inte kan ta emot nätverksanslutningar.

### Instruktioner för datorerna som ska ansluta mot en annan

Anslut mot IP-adresser inom ditt Tailscale-nätverk vilka börjar på `100.x.x.x` (de förändras inte). Kör `tailscale status` i en terminal eller besök Tailscales webbplats för att se IP-nummer och värdnamn i ditt nätverk.

#### Terminal

Här beskrivs hur du kan ansluta till en dator via **SSH** över Tailscale-nätverket.

- **Windows:** SSH-klientkommandot `ssh` om det redan finns installerat eller [Putty][5].
- **Linux:** SSH-klientkommandot `ssh` från OpenSSH Client är oftast redan installerat.
- **macOS:** SSH-klientkommandot `ssh` är även här förinstallerat.

##### Exempel: ansluta med `ssh`

1. **Öppna en terminal:**

   - I Windows, starta **PowerShell**.
   - I Linux och macOS, öppna en vanlig terminal.

2. **Hitta Tailscale-adressen för din dator:**

   - Om du har `tailscale`-kommandot installerat, kör:

     ```bash
     tailscale status
     ```

   - Annars kan du se IP-adressen i Tailscales webbgränssnitt.

   Adressen börjar alltid med `100.` (till exempel `100.2.3.4`).

3. **Kör `ssh` för att ansluta:**
   Anta att Tailscale-adressen är `100.2.3.4` och att kontot på datorn heter `administrator`. Då skriver du:

   ```bash
   ssh administrator@100.2.3.4
   ```

4. **Logga in:**
   Du blir ombedd att skriva lösenordet för kontot (till exempel `Linux4Ever`, eller det du själv valt).

5. **Arbeta på datorn:**
   Nu kan du köra kommandon och göra övningsuppgifter direkt via SSH.

6. **Koppla ner när du är klar:**

   ```bash
   exit
   ```

#### Visual Studio Code

Börja med att följa instruktionerna i dokumentet "Sätt upp Visual Studio Code" om du inte redan gjort det. Instruktionerna under den här rubriken är desamma oavsett operativsystem.

##### Lägg till en dator att ansluta mot

1. Välj "Remote Explorer" i panelen längs till vänster som i övrigt innehåller "Explorer" och "Search".

   - Om du inte hittar det behöver du installera tillägget: [Remote - SSH][6].

2. Välj "+" (New Remote) som visas när du för muspekaren över ordet "SSH" under "Remotes".
3. Fyll i "Enter SSH Connection Command"-fältet med `ssh kontonamn@100.x.x.x`, där kontonamnet och IP-adressen ersätts med lämpliga värden för datorn du ska ansluta emot.
4. Tryck bara **Enter** vid fältet "Select SSH configuration file to update" för att välja standardvalet.
5. Ignorera "Host added!" dialogrutan som visas en stund nere till höger.

##### Anslut mot en tillagd dator

1. Välj "Remote Explorer" i panelen längs till vänster (som i övrigt innehåller "Explorer" och "Search").
2. Välj "→" (Connect in Current Window\...) som visas när du för muspekaren över det tillagda datornamnet eller IP-numret under "Remotes".
3. Välj "Continue" om du får en fråga om fingeravtryck ("fingerprint").
4. Skriv in lösenordet för kontot du ansluter mot när fältet "Enter password for [kontonamn@100.x.x.x](mailto:kontonamn@100.x.x.x)" visas.
5. Om anslutningen lyckades ska en blå ikon med text liknande "SSH: 100.x.x.x" visas längst ned till vänster.

[1]: https://tailscale.com/pricing
[2]: https://tailscale.com/why-tailscale
[3]: https://tailscale.com/blog/how-tailscale-works
[4]: https://tailscale.com/
[5]: https://putty.org/
[6]: https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-ssh
