# VirtualBox

Detta dokument är en guide för installation av VirtualBox på en värddator med gästoperativsystemen Ubuntu Desktop och/eller Ubuntu Server. Valet av gästoperativsystem samt antalet virtuella maskiner kan variera mellan kurser, men förutom dessa val bör alla rubriker läsas i den ordning de tas upp i dokumentet.

Kontakta din utbildare om du är osäker på vilka gästoperativsystem och hur många virtuella maskiner som behövs för just din kurs.

[Aktivera processorstöd för virtualisering i UEFI]
[Ladda ner och installera VirtualBox]
[Windows]
[Ubuntu]
[Gästoperativsystem]
[Ubuntu Desktop]
[Skapa den virtuella maskinen]
[Installera Ubuntu Desktop]
[Ubuntu Server som gästoperativsystem]
[Skapa den virtuella maskinen]
[Installera Ubuntu Server]
[Nätverk]
[Exakt en virtualiserad maskin]
[Exempel]
[Flera virtualiserade maskiner]
[Vanliga fel och åtgärder]
[Boota igen från en ISO-fil trots ett fungerande operativsystem]
[UEFI/EFI]
[VirtualBox fryser efter Ubuntu Desktop-installationen]
# Aktivera processorstöd för virtualisering i UEFI {#aktivera-processorstöd-för-virtualisering-i-uefi}

Innan du börjar med installationen är det viktigt att kontrollera och aktivera virtualiseringsstödet i din dators processor:

1. Starta om din dator.

2. Under uppstarten, tryck på den specifika tangenten för att komma åt UEFI-inställningarna. Denna tangent varierar beroende på datorns tillverkare, men vanligtvis är det Del, F2, F10, eller Esc.

3. När du är inne i UEFI-menyn, navigera till fliken som heter Avancerat, CPU-konfiguration eller något liknande.

4. Leta efter en inställning som heter Virtualiseringsteknik, Intel Virtualization Technology (VT-x), eller AMD-V, beroende på din processor.

5. Akrivera funktionen eller funktionerna om de inte redan är det.

6. Spara dina ändringar och starta om datorn.

7. Dessa steg gör att VirtualBox kan köra virtuella maskiner mer effektivt genom att till fullo utnyttja sådant stöd i processorn.

# Ladda ner och installera VirtualBox {#ladda-ner-och-installera-virtualbox}

## Windows {#windows}

Följ dessa instruktioner om du vill installera VirtualBox på en Windows-dator:

1. Besök [VirtualBoxs hemsida](https://www.virtualbox.org/)

2. Klicka på "Downloads" och välj sedan versionen för Windows

3. Ladda ner filen och starta installationen

4. Följ installationsguiden och klicka på "Next" tills installationen är klar

## Ubuntu {#ubuntu}

Följ dessa instruktioner om du vill installera VirtualBox på en Ubuntu-dator:

1. Installera paketet virtualbox-dkms (**ladda inte hem programmet från Virtualbox hemsida**)

   1. sudo apt update

   2. sudo apt install virtualbox-dkms

# Gästoperativsystem {#gästoperativsystem}

**Observera:** Valet mellan Ubuntu Desktop och Ubuntu Server beror på din kurs. Kontakta din utbildare om du är osäker på vilket gästoperativsystem som ska installeras.

## Ubuntu Desktop {#ubuntu-desktop}

Följ dessa instruktioner om du vill installera "Ubuntu Desktop" som gästoperativsystem i VirtualBox. När du har följt dessa instruktioner behöver du även följa instruktionerna under rubriken [Nätverk]
1. Besök [Download Ubuntu Desktop](https://ubuntu.com/download/desktop)

2. Ta hem ISO-filen för den senaste Long Term Support (LTS) versionen av Ubuntu Desktop

### Skapa den virtuella maskinen {#skapa-den-virtuella-maskinen}

1. Starta VirtualBox

![][image1]

1. Välj "New" för att öppna guiden "Create Virtual Machine"

   2. "Virtual machine Name and Operating System"

![][image2]

1. Sätt ett namn i "Name"

   2. Välj den nedladdade ISO-filen i "ISO Image". Välj "Other" i rullgardinsmenyn för att få upp en fildialogruta

      3. Välj "Linux" som "Type" och "Ubuntu" som "Version" om det inte redan är förvalt

      4. Bocka i "Skip Unattended Installation" då vi vill installera själva. Det är viktigt eftersom installationen blir rätt underlig om den görs automatiskt

      5. Välj "Next"

   3. "Hardware"

![][image3]

1. Beroende av hur stort RAM-minne din dator har kan du välja följande "Base Memory" för den virtuella maskinen

   1. 32 GB → 6000 MB

      2. 16 GB → 5000 MB

         3. 08 GB → 4000 MB

      2. Beroende av hur många virtuella CPUer din dator har kan du välja följande "Processors"-antal. Välj den som är närmast din dators antal CPUer

         1. 32 → 8

         2. 16 → 4

         3. 08 → 3

         4. 04 → 1

      3. Bocka i "Enable EFI (special OSes only)" för att få ett modernt boot-system.

      4. Välj "Next"

   4. "Virtual Hard disk"

![][image4]

1. Välj mellan 20 GB och 40 GB som "Disk Size". Vi kommer inte att behöva mer än 40 GB i kursen

   2. Välj "Next"

   5. "Summary"

![][image5]

1. Kontrollera att summeringen av dina val stämmer

   2. Välj "Finish"

2. Välj "Settings" för den nya virtuella maskinen då vi vill ändra några inställningar innan vi installerar Ubuntu Desktop

![][image6]

1. "General" / "Advanced"

![][image7]

1. Välj "Bidirectional" för "Shared Clipboard"

   2. Välj "Bidirectional" för "Drag'n'Drop"

   2. "System"

      1. "Motherboard"

![][image8]

1. Ta bort bocken för "Floppy" då vi inte behöver någon sådan enhet

   2. "Processor"

![][image9]

1. Bocka i de "Extended Features" som är möjliga att aktivera. Det kan till exempel vara så att "Enable PAE/NX" är tillgängligt men inte "Enable Nested VT-x/AMD-V"

   3. "Display" / "Screen"

![][image10]

1. Välj 128 MB för "Video Memory"

   2. Bocka i "Enable 3D Acceleration" om det är tillgängligt

   4. "Network" / "Adapter 1"

![][image11]

1. Låt standardinställningen NAT vara kvar i nuläget. Det är en inställning som gör att gästoperativet kommer att få en IP-address via DHCP och har möjlighet att nå ut på Internet. Det finns en separat del av dokumentet för att sätta upp nätverket för att uppfylla diverse krav. Läs igenom och följ den delen när gästoperativsystmet är installerat och fungerar

   2. Välj "OK"

3. Välj "Start" för att starta den virtuella maskinen för att påbörja installationen

![][image12]

### Installera Ubuntu Desktop {#installera-ubuntu-desktop}

1. Boot-m	enyn

![][image13]

1. Välj "Try or Install Ubuntu"

2. Guiden "Welcome to Ubuntu" öppnas

   1. "Choose your language"

![][image14]

1. Välj "English"

   2. Välj "Next"

3. "Accessibility in Ubuntu"

![][image15]

1. Utforska alternativen och aktivera eventuellt någon eller några funktioner för att förbättra tillgängligheten

   2. Välj "Next"

4. "Select your keyboard layout"

![][image16]

1. Välj "Swedish"

   2. Välj "Swedish" i "Select your keyboard variant"

   3. Välj "Next"

5. "Connect to the Internet"

![][image17]

1. Välj "Use wired connection"

   2. Välj "Next"

6. "What do you want to do with Ubuntu?"

![][image18]

1. Välj "Install Ubuntu" om det inte redan är förvalt

   2. Välj "Next"

7. "How would you like to install Ubuntu?"

![][image19]

1. Välj "Interactive installation" om det inte redan är förvalt

   2. Välj "Next"

8. "What apps would you like to install to start with?"

![][image20]

1. Välj "Default selection" om det inte redan är förvalt

   2. Välj "Next"

9. "Install recommended proprietary software?"

![][image21]

1. Bocka i "Install third-party software for graphics and Wi-Fi hardware"

   2. Bocka i "Download and install support for additional media formats"

   3. Välj "Next"

10. "How do you want to install Ubuntu?"

![][image22]

1. Välj "Erase disk and install Ubuntu" om det inte redan är förvalt

   2. Välj "Next"

11. "Create your account"

![][image23]

1. Skriv ditt kompletta namn i "Your name"

   2. Skriv ett valfritt kort datornamn med små bokstäver och utan mellanslag i "Your computer's name". Tänk på att det här namnet ofta blir med i din Bash-prompt så det kan vara irriterande att ha ett långt namn

   3. Fältet "Your username" och de två fälten för "password" beror på var och hur du installerar Ubuntu Desktop

      1. På egen dator direkt eller virtualiserat → Helt upp till dig\!

      2. På en studentdator direkt eller virtualiserat →

         1. Username: administrator

         2. Password: Linux4Ever

   4. Välj "Require my password to login" om det inte redan är förvalt

   5. Ta bort bocken för "Use Active Directory" om det är förvalt

   6. Välj "Next"

12. "Select your timezone"

![][image24]

1. Välj "Stockholm (Stockholm, Sweden) genom att skriva det i "Location"-fältet eller genom att klicka på kartan om det inte redan är förifyllt

   2. Välj "Next"

13. "Review your choices"

![][image25]

1. Kontrollera att summeringen av dina val stämmer

   2. Välj "Install"

14. Systemet installeras\!

![][image26]

1. Förundras över allt man kan göra med Ubuntu Desktop eller ta rast i tjugo minuter. Hur lång tid installationen faktiskt tar beror på datorns prestanda och kan variera från 5 minuter till 30 minuter.

15. "Ubuntu XX.04 LTS is installed and ready to use"

![][image27]

1. Välj "Restart now"

16. Omstartstexten "Please remove the installation medium, then press ENTER"

    1. VirtualBox har lägre boot-prioritet för din cd-rom (vilket ISO-filen presenteras som) än din hårddisk så det räcker med att trycka "Enter" eller "Return"-knappen på tangentbordet

![][image28]

17. Efter omstarten

    1. Inloggningsskärmen

![][image29]

2. Välj ditt konto genom att klicka på det eller genom att trycka på "Enter" eller "Return"-tangenten på tangentbordet om det bara finns ett konto att välja på

   3. Skriv ditt lösenord

   4. Tryck "Enter" eller "Return"-tangenten på tangentbordet

18. En guide att gå igenom vid första inloggningen öppnas\!

19. "Welcome to Ubuntu XX.04 LTS\!"

![][image30]

1. Välj "Next"

20. "Ubuntu Pro"

![][image31]

1. Välj "Skip for now" om det inte redan är valt för "Enable Ubuntu Pro now, or skip this step"

   2. Välj "Next"

21. "Help improve Ubuntu"

![][image32]

1. Välj själv om du vill att systemet ska bidra med information för att förbättra Ubuntu eller ej. Om du är osäker på vad det betyder är det bäst att välja "No, don't share system data"

   2. Välj "Next"

22. "Get started with more applications"

![][image33]

1. Det är bättre att installera program senare

   2. Välj "Finish"

23. Uppdatera systemet

    1. Tryck på "Windows"/"Start"/"Meta"-tangenten för att söka efter program. Skriv "terminal" och tryck "Enter" eller "Return" på tangentbordet för att öppna ett terminalfönster med skalet Bash

    2. Kör följande kommandon för att uppdatera systemet och sedan starta om det. Om systemet redan är uppdaterat så kommer inget att uppdateras

       1. sudo apt update

       2. sudo apt full-upgrade \-y

       3. sudo reboot

## Ubuntu Server som gästoperativsystem {#ubuntu-server-som-gästoperativsystem}

Följ dessa instruktioner om du vill installera "Ubuntu Desktop" som gästoperativsystem i VirtualBox. När du har följt dessa instruktioner behöver du även följa instruktionerna under rubriken [Nätverk]
1. Besök [Get Ubuntu Server](https://ubuntu.com/download/server)

2. Ta hem ISO-filen för den senaste Long Term Support (LTS) versionen av Ubuntu Server

### Skapa den virtuella maskinen {#skapa-den-virtuella-maskinen-1}

1. Starta VirtualBox

![][image34]

1. Välj "New" för att öppna guiden "Create Virtual Machine"

   2. "Virtual machine Name and Operating System"

![][image35]

1. Sätt ett namn i "Name"

   2. Välj den nedladdade ISO-filen i "ISO Image". Välj "Other" i rullgardinsmenyn för att få upp en fildialogruta

      3. Välj "Linux" som "Type" och "Ubuntu" som "Version" om det inte redan är förvalt

      4. Bocka i "Skip Unattended Installation" då vi vill installera själva. Det är viktigt eftersom installationen blir rätt underlig om den görs automatiskt

      5. Välj "Next"

   3. "Hardware"

![][image36]

1. Välj "2048 MB" som "Base Memory"

   2. Välj "2" för "Processors"-antal

      3. Bocka i "Enable EFI (special OSes only)" för att få ett modernt boot-system.

   4. "Virtual Hard disk"

![][image37]

1. Välj 40 GB som "Disk Size". Vi kommer inte att behöva mer än det i kursen

   2. Välj "Next"

   5. "Summary"

![][image38]

6. Kontrollera att summeringen av dina val stämmer

   7. Välj "Finish"

2. Välj "Settings" för den nya virtuella maskinen då vi vill ändra några inställningar innan vi installerar Ubuntu Server

![][image39]

1. "System"

   1. "Motherboard"

![][image40]

2. Ta bort bocken för "Floppy" då vi inte behöver någon sådan enhet

   2. "Processor"

![][image41]

1. Bocka i de "Extended Features" som är möjliga att aktivera. Det kan till exempel vara så att "Enable PAE/NX" är tillgängligt men inte "Enable Nested VT-x/AMD-V"

   3. "Network" / "Adapter 1"

![][image42]

1. Låt standardinställningen NAT vara kvar i nuläget. Det är en inställning som gör att gästoperativet kommer att få en IP-address via DHCP och har möjlighet att nå ut på Internet. Det finns en separat del av dokumentet för att sätta upp nätverket för att uppfylla diverse krav. Läs igenom och följ den delen när gästoperativsystmet är installerat och fungerar

   2. Välj "OK"

3. Välj "Start" för att starta den virtuella maskinen för att påbörja installationen

![][image43]

### Installera Ubuntu Server {#installera-ubuntu-server}

1. Boot-menyn

![][image44]

1. Välj "Try or Install Ubuntu"

2. Guiden "Welcome to Ubuntu" öppnas

3. "Select your language"

![][image45]

1. Välja "English"

4. "Keyboard configuration"

![][image46]

1. Välj "Swedish" som "Layout"

   2. Välj "Swedish" som "Variant"

   3. Välj "Done"

5. "Choose the type of installation"

![][image47]

1. Bocka i "Ubuntu Server" om det inte redan är förvalt

   2. Bocka i "Search for third-party drivers"

   3. Välj "Done"

6. "Network configuration"

![][image48]

1. Kontrollera att servern har fått DHCPv4-adressen 10.0.2.15/24. Alla virtuella maskiner med inställningen NAT (lägg märke till att det skiljer sig från NAT Network som är förvirrande likt) får detta IP.

   2. Välj "Done"

7. "Proxy configuration"

![][image49]

1. Låt vara blankt då vi inte använder en proxy

   2. Välj "Done"

8. "Ubuntu archive mirror configuration"

![][image50]

1. Låt sökningen efter bäst server att installera paket ifrån slutföras

   2. Välj "Done"

9. "Guided storage configuration"

![][image51]

1. Bocka i "Custom storage layout"

   2. Välj "Done"

10. "Storage configuration" (inledningsvis)

![][image52]

1. Välj (gå med piltangenterna och tryck "Enter" eller "Return" på tangengbordet) "free space"

   2. Välj sedan "Add GPT Partititon"

11. "Adding GPT partition to ..."

![][image53]

1. Välj att använda ca 10G mindre än max för "Size"

   2. Välj "ext4" som Format om det inte är förvalt

   3. Välj "/" som "Mount" om det inte är förvalt

   4. Välj "Create"

12. "Storage configuration" (resultatet)

![][image54]

1. Kontrollera att de två mountpointsen "/" och "/boot/efi" är konfigurerade

   2. Kontrollera att det finns ca 10G "free space"

   3. Välj "Done"

13. "Confirm destructive action"

![][image55]

1. Välj "Continue" för att spara partitionstabellen

14. "Profile configuration"

![][image56]

1. Skriv ditt kompletta namn i "Your name"

   2. Skriv ett valfritt kort datornamn med små bokstäver och utan mellanslag i "Your servers name". Tänk på att det här namnet ofta blir med i din Bash-prompt så det kan vara irriterande att ha ett långt namn

   3. Fältet "Pick a username" och de två fälten för "password" beror på var och hur du installerar Ubuntu Server

      1. På egen dator direkt eller virtualiserat → Helt upp till dig\!

      2. På en studentdator direkt eller virtualiserat →

         1. Username: administrator

         2. Choose a password: Linux4Ever

   4. Välj "Done"

15. "Upgrade to Ubuntu Pro"

![][image57]

1. Bocka i "Skip for now" om det inte redan är förvalt. Att skaffa en sådan prenumeration kan vara en jättebra idé i en arbetssituation men för kursens del behöver vi inte den

   2. Välj "Continue"

16. "SSH configuration"

![][image58]

1. Ta bort bocken för "Install OpenSSH server" om den var ifylld

   2. Välj "Done"

17. "Third-party drivers"

![][image59]

1. Välj "Continue". Oftast så finns det inga tredjepartsdrivrutiner för en virtuell maskin men det kan vara annorlunda på en fysisk maskin

18. "Featured server snaps"

![][image60]

1. Bocka inte i någon mjukvara

   2. Välj "Done"

19. "Installation complete\!"

![][image61]

1. Låt installationen fortskrida och vänta tills den även har uppdaterat mjukvaran. Vi kommer att uppdatera mjukvaran vid första inloggningen också men det skadar inte med två uppdateringar

   2. Välj "Reboot Now"

20. Omstartstexten "Please remove the installation medium, then press ENTER"

![][image62]

1. VirtualBox har lägre boot-prioritet för din cd-rom (vilket ISO-filen presenteras som) än din hårddisk så det räcker med att trycka "Enter" eller "Return"-knappen på tangentbordet

21. Efter omstarten

![][image63]

1. Logga in med kontot du skapade under installationen

   2. Kör följande kommandon för att uppdatera systemet och sedan starta om det. Om systemet redan är uppdaterat så kommer inget att uppdateras

      1. sudo apt update

      2. sudo apt full-upgrade \-y

      3. sudo reboot

# Nätverk {#nätverk}

Denna del beskriver hur du konfigurerar nätverket för att ge internetåtkomst till enskilda virtuella maskiner eller möjliggöra kommunikation mellan flera. För en mer detaljerad översikt rekommenderas foruminlägget [Virtualbox Networks: In Pictures](https://forums.virtualbox.org/viewtopic.php?f=35&t=96608#p468773).

## Endast en ensam virtualiserad maskin {#endast-en-ensam-virtualiserad-maskin}

Om du behöver nätverksåtkomst till en enskild virtuell maskin kan du använda "NAT" tillsammans med "Port Forwarding" för att göra specifika portar tillgängliga. Använd portnummer 1024 eller högre på värddatorn, eftersom lägre portar kräver utökade rättigheter. Det går däremot bra att använda valfritt portnummer på gästdatorn.

Om port forwarding inte fungerar som förväntat, trots rätt portinställningar, kan du prova att byta "Adapter Type" till "Paravirtualized Network (virtio-net)" istället för "Intel PRO/1000 MT Desktop (82540EM)".

### Exempel {#exempel}

Använd "NAT" för att vidarebefordra TCP-port 1024 på värddatorn till TCP-port 22 på gästdatorn. Eftersom endast en virtuell maskin finns i ett "NAT"-nätverk så behöver du inte ange IP-adress. Att namnge reglerna är frivilligt och endast till för att lättare komma ihåg varför en port forwardas.

För att kunna ansluta med SSH från värddatorn till gästdatorn så behöver du ha OpenSSH Server installerat. Det kan du göra med `apt install openssh-server` på gästdatorn.

![][image64]

![][image65]

## Flera virtualiserade maskiner {#flera-virtualiserade-maskiner}

Du kan placera flera virtuella maskiner i samma isolerade nätverk med hjälp av adaptertypen "NAT Network" (vilket skiljer sig från enbart "NAT"). Denna nätverkstyp gör att alla virtuella maskiner i nätverket kan kommunicera fritt med varandra.

För att skapa ett nytt "NAT Network"-nätverk, navigera till *File* / *Tools* / *Network Manager* / *NAT Networks* och välj *Create*. Du kan behålla standardnamnet "NatNetwork" om du bara planerar att ha ett sådant nätverk eller ge det ett eget namn. Se till att "Enable DHCP" är ikryssat om det inte redan är förvalt.

Ett exempel följer här på hur du lägger till ett NAT Network, ändrar en virtuell maskin till att använda det, samt konfigurerar port forwarding enligt beskrivningen:

1. **Lägg till ett NAT Network** genom att välja *Create* i *Network Manager* och kontrollera att "Enable DHCP" är ikryssat.
2. **Ändra en virtuell maskin till att använda NAT Network** genom att gå till maskinens inställningar (*Settings*), välja *Network*, och under *Attached to* välja "NAT Network". Välj det nätverk du skapade, t.ex. "NatNetwork".
3. **Sätt upp port forwarding** genom att gå tillbaka till *Network Manager*, välja *NAT Networks*, och klicka på ditt nätverk för att öppna dess inställningar. Klicka på *Port Forwarding*, och lägg till följande regler för SSH-åtkomst:
   * **Port 1025 på värddatorn** till **port 22 på den första virtuella maskinen genom att ange dess Guest IP.**
   * **Port 1026 på värddatorn** till **port 22 på den andra virtuella maskinen genom att ange dess Guest IP.**
   * **Port 1027 på värddatorn** till **port 22 på den tredje virtuella maskinen genom att ange dess Guest IP.**

När detta är konfigurerat kan du ansluta till dina virtuella maskiner via SSH med följande kommandon:

`ssh -p 1025 localhost  # Ansluter till första virtuella maskinen`
`ssh -p 1026 localhost  # Ansluter till andra virtuella maskinen`
`ssh -p 1027 localhost  # Ansluter till tredje virtuella maskinen`

För ytterligare information om portbegränsningar och möjliga lösningar, se avsnittet ovan om "NAT".

![][image66]

# Vanliga fel och åtgärder {#vanliga-fel-och-åtgärder}

## Boota igen från en ISO-fil trots ett fungerande operativsystem {#boota-igen-från-en-iso-fil-trots-ett-fungerande-operativsystem}

### UEFI/EFI {#uefi/efi}

För att komma åt Boot Menu så används knappen Esc vid uppstart. Det är extremt kort tid som man har möjlighet att trycka på Esc. Bäst chans har man om man väljer Restart i menyn och sedan direkt trycker Esc några gånger. Välj Reset igen tills du lyckas och får följande skärmbild:

![][image67]

För att sedan starta från ISO filen väljer du "Boot Manager" och väljer något i stil med UEFI VBOX CD-ROM VB96007754-1673a637 genom menyn.

## VirtualBox fryser efter Ubuntu Desktop-installationen {#virtualbox-fryser-efter-ubuntu-desktop-installationen}

När du har installerat Ubuntu Desktop och trycker på "Enter" eller "Return"-tangenten på tangentbordet för att starta om maskinen så händer och VirtualBox fryser. Testa i så fall att starta om maskinen "hårt" genom att

1. Gå till "Machine" i VirtualBox-menyn

2. Välj "Reset"

