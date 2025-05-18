# Studentguide till GitHub Classroom

Instruktionerna förutsätter och rekommenderar att ditt operativsystem är inställt på engelska.

[GitHub Classroom][1] är ett verktyg som utbildare använder för att skapa och hantera studentuppgifter.

Följ först instruktionerna i [Sätt upp Git och använd GitHub][2] om du inte redan har gjort det.

## Termer

Dessa termer är viktiga och används i guiden:

- **Repo**: En katalog vars innehåll versionshanteras av Git, vanligtvis på GitHub.
- **Branch**: En serie ändringar i form av "commits".
- **Commit**: En förändring i en eller flera filer, beskriven med ett meddelande.
- **Push**: Att ladda upp lokala commits till GitHub.
- **Pull Request (PR)**: En förfrågan att infoga ändringar från en gren till en annan.
- **Merge**: Att föra in förändringar från en gren till en annan.
- **Feedback**: En särskild PR som skapas för varje uppgift och används för kommunikation med utbildaren. **Den ska aldrig mergas!**

## Arbetsflöde för varje uppgift

1. **Acceptera uppgiften**: Använd länken från utbildaren för att acceptera uppgiften. För gruppuppgifter skapar en person från gruppen ett gruppnamn som övriga kan ansluta till.

2. **Ladda ned repo**: När uppgiften är accepterad skapas ett repo som kan klonas till datorn med `git clone <repourl>`. Gå till uppgiftens repo på GitHub, klicka på knappen "Code", välj "SSH" och kopiera URL som börjar med `git@`.

3. **Arbeta med uppgiften**: Använd `git add .`, `git commit`, och `git push` för att synkronisera ändringar.

4. **Grupper**: Grupper bör aktivera "branch protection" för att förhindra direkta pushes till "main". Ändringar görs i separata brancher och mergas till "main" via en PR, där gruppmedlemmarna kan kommentera och godkänna.

5. **Slutföra uppgiften**: Skriv en kommentar i "Feedback"-PR och tagga utbildaren med `@kc8se` för att meddela att uppgiften är klar.

## När en uppgift uppdateras

Det är ganska vanligt att en uppgift uppdateras för att förtydligas eller rätta till ett fel.

När detta sker skapas en "Pull Request" med ett namn i stil med "GitHub Classroom: Sync Assignment" i varje students uppgiftsrepo på GitHub. Klicka på PR:en och välj sedan "Merge Pull Request", följt av "Confirm Merge".

När det är gjort behöver du köra `git pull` i ditt lokala uppgiftsrepo för att hämta uppdateringen.

Håll utkik efter uppdateringar – det är varje students ansvar att genomföra dessa steg. När en uppdatering har publicerats är det den versionen som gäller och ska följas.

## Feedback och rättning

- **Manuell rättning**: Vid manuell rättning ändrar utbildaren statusen i "Feedback" till "Accepted" om uppgiften godkänns, eller till "Change Request" om ändringar behövs. Skriv en kommentar i "Feedback"-PR och tagga utbildaren med `@kc8se` för att meddela att uppgiften är klar.

- **Automatiska tester**: Uppgifter som innehåller katalogen `.basetests` har automatiska tester. Sådana uppgifter är godkända om testerna passerar och ingen status sätts då i "Feedback".

- **Manuell rättning och tester**: Om uppgiften både har automatiska tester och kräver manuell rättning så anges det **tydligt** i uppgiftsbeskrivningen. För sådana uppgifter väntar utbildaren med rättning tills testerna passerar.

### Muntlig redovisning

Flera studenter kommer att få redovisa sin uppgift muntligt. Studenter som väljs för muntlig redovisning underrättas via mejl, vilket kan skickas både före och efter att uppgiften godkänts.

Om du får en kallelse till muntlig redovisning är uppgiften inte klar förrän redovisningen genomförts, oavsett om den redan har godkänts i "Feedback" eller passerat automatiska tester.

### Detaljerad feedback

Feedback ges på ditt initiativ!

Skriv kommentarer via GitHubs gränssnitt i PR:en "Feedback" och ställ gärna frågor som:

- "Ser du ett bättre sätt att lösa detta?"
- "Vilka för- och nackdelar har det här tillvägagångssättet?"
- "Den här loopen går inte igenom alla element – har du något tips?"

Om du vill ha övergripande feedback på hela lösningen kan du skriva ett meddelande som inte är knutet till en rad och be om det.

Då går utbildaren igenom uppgiften och kommenterar:

- Vad som är särskilt bra
- Möjliga förbättringar
- Alternativa lösningar (som inte nödvändigtvis är bättre)
- Saker att tänka på framöver

Tagga alltid utbildaren med `@kc8se` när du kommenterat, så att din feedback uppmärksammas.

### Kortfattad feedback

Utbildarens första kommentarer är mycket korta och fokuserar på vad som behöver åtgärdas. Till exempel används:

- Oftast ordet "Godkänt" vid godkännande av en manuellt rättad uppgift utan någon mer text eller feedback.
- Stödord som "stderr" för att markera att utskriften bör gå till "stderr" istället för "stdout" i en Bash-uppgift där det stått att felmeddelanden ska skrivas till stderr, men inlämningen skriver meddelandet till "stdout".

Nackdelar med denna korthet är att:

- Feedbacken kan uppfattas som oengagerad eller otrevlig trots att det verkligen inte är avsikten.
- Balansen mellan korthet och tydlighet kan ibland hamna fel och göra feedbacken svårare att förstå. Be gärna om en utförligare förklaring!
- Feedbacken fokuserar enbart på vad som behöver förbättras. Därför innehåller den oftast inga kommentarer om vad som var särskilt bra.

Fördelarna med detta upplägg är att det sparar mycket tid och gör feedbacken tydligare för dem som endast vill veta vad som behöver åtgärdas.

För den som vill ha mer detaljerad feedback finns det absolut möjlighet att få det enligt avsnittet "Detaljerad feedback". Utbildaren tycker att sådant engagemang är kul att se och ger gärna mer feedback till de som vill ha det!

### Automatiska tester

Alla uppgifter med automatiska tester har katalogen `.basetests` i repots rot.

Det är enkelt att köra testerna på din egen dator. Om du står i repots rot med ett kommandoskal kan testerna köras genom:

- Windows: `.basetests\run`
- Linux/MacOS: `.basetests/run`

Om testerna passerar visas en grön bock på repots huvudsida. Om något är fel visas ett rött kryss.

Det krävs tyvärr några steg för att se detaljer kring vad som gått fel på GitHub.

För att se mer detaljer om körningen av tester:

1. Gå till "Actions"-fliken.
2. Välj "Autograding tests" på vänster sida.
3. Testkörningarna visas nu, med den senaste överst. Status för körningarna syns genom en ikon till vänster om varje rad.
4. Välj den körning du är intresserad av (oftast den senaste).
5. Välj "run-autograding-tests".
6. Körningens steg visas, där "Autograding Reporter" är i fokus. Röda kryss indikerar steg som misslyckats.
7. Klicka på det steg du vill ha mer information om och läs texten.

### Hjälp och stöd

Om du behöver hjälp med en uppgift, skriv i "Feedback" och tagga utbildaren med `@kc8se`. Det går smidigare att få hjälp om du inkluderar information som:

- Hur du har tänkt hittills
- Din tolkning av uppgiften
- Vad du behöver hjälp med
- Vad du har prövat och resultaten (beskriv gärna eventuella felmeddelanden eller oväntade resultat)

### Gör backup på dina repos

Under kursens gång kan ditt repo tas bort från GitHub på grund av ett tekniskt eller administrativt fel. Det är ditt ansvar att behålla en backup på alla dina repos.

Tänk på att studentdatorerna inte är en lämplig plats för din backup eftersom de installeras om med jämna mellanrum.

Efter kursens slut kommer ditt repo att tas bort från GitHub. Det kan ske redan dagen efter kursens sista lektion, men även ske långt efter att kursens avslutats.

[1]: https://classroom.github.com/
[2]: https://github.com/nackc8/kursmaterial/blob/main/shared/satt-upp-git-och-anvand-github.md
