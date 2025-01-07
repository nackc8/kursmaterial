# Byggsystem och beroendehantering i Java

## Distribuering och paketering av Java-program

### Introduktion till JAR- och WAR-filer

#### JAR-filer (Java Archive) – För paketering av applikationer och bibliotek

En JAR-fil är ett paketformat som används för att distribuera Java-program och bibliotek. Den innehåller:

- Kompilerade `.class`-filer.
- Metadata som `MANIFEST.MF`.
- Resurser som bilder eller konfigurationsfiler.

JAR-filer används ofta för:

- Återanvändbara bibliotek.
- Självständiga program med en definierad startpunkt (main-metod).
- Distribuering av programvara till servrar eller klienter.

##### Exempel: Kör en JAR-fil från terminalen

Om JAR-filen innehåller en main-metod och är körbar kan den startas med följande kommando:

```bash
java -jar filnamn.jar
```

#### WAR-filer (Web Application Archive) – För webbapplikationer

En WAR-fil är ett paketformat för webbaserade Java-applikationer. Den innehåller:

- Java-klasser och bibliotek.
- Webbresurser som HTML, CSS och JavaScript.
- Konfigurationsfiler som `web.xml`.

WAR-filer används för att distribuera applikationer till servlet-containers som Apache Tomcat eller Jetty.

### Classpath och sökvägar för Java-program

#### Vad är classpath?

Classpath är en lista med sökvägar där Java letar efter paket (kataloger) och JAR-filer som innehåller klasser och resurser som behövs för att köra ett program.

#### Ange classpath vid körning

- Classpath kan anges med flaggan `-cp` eller `-classpath` vid körning av ett program:

  ```bash
  java -cp lib/*:. mypackage.MyClass
  ```

  I detta exempel:

  - `lib/*` inkluderar alla JAR-filer i katalogen `lib`.
  - `.` anger aktuell katalog som en del av sökvägen.

- Alternativt kan classpath definieras med miljövariabeln `CLASSPATH`:

  ```bash
  export CLASSPATH=lib/*:.
  ```

  Detta påverkar alla Java-kommandon som körs i samma terminalsession.

#### Klasser utan paket

Det är också möjligt att ha en klass, t.ex. `MyClass.class`, utan något paket direkt i en katalog som finns i classpath. Om `MyClass.class` ligger i katalogen `/projekt`, kan programmet köras med:

```bash
java -cp /projekt MyClass
```

Denna metod fungerar för små projekt eller testprogram, men det rekommenderas att använda paket för bättre organisering och för att undvika namnkonflikter.

#### Standardklasser

Standardklasserna i Java (till exempel från `java.lang` och `java.util`) är alltid tillgängliga, även om du anger en egen classpath.
Om ingen classpath anges använder JVM som standard den aktuella katalogen för att leta efter klasser.

## Introduktion till byggsystem och beroendehantering

### Vad är ett byggsystem?

Ett byggsystem är ett verktyg för att automatisera kompilering, testning och paketering av kod. De två mest använda systemen är Maven och Gradle.

### Varför behövs beroendehantering?

Program innehåller ofta bibliotek och moduler från tredje part. Att manuellt ladda ner och uppdatera dessa är ineffektivt och riskfyllt. Byggsystem erbjuder lösningar för att:

- Hämta och hantera beroenden automatiskt.
- Lösa transitiva beroenden (beroenden till andra beroenden).
- Hantera versionskonflikter.

## IntelliJ IDEA Community Edition

### Inbyggt stöd för byggsystem

IntelliJ IDEA Community Edition har inbyggt stöd för byggsystem som Maven och Gradle. Det möjliggör:

- Projektkonfiguration via grafiskt gränssnitt eller direkt i byggfiler.
- Import och hantering av externa beroenden.
- Synkronisering av projektfiler och beroenden.

### Användning av Maven och Gradle i IntelliJ

- Skapa projekt direkt med Maven eller Gradle.
- Synkronisera beroenden med IDE\:t.
- Bygga och köra projekt med inbyggda verktyg och terminal.

### Hantera dependencies utan Maven eller Gradle

IntelliJ IDEA har inget eget byggsystem för beroendehantering, men kan hantera externa bibliotek utan Maven eller Gradle genom att:

- Lägga till JAR-filer manuellt:
  1. Gå till `File` > `Project Structure` > `Modules` > `Dependencies`.
  2. Klicka på `+` och välj `JARs or directories`.
  3. Lägg till den nödvändiga JAR-filen.
  4. IntelliJ uppdaterar klassvägen automatiskt.

### Begränsningar

- Ingen automatisk hantering av transitiva beroenden.
- Ingen versionshantering eller uppdateringskontroll.
- Kräver manuell hantering vid tillägg av fler beroenden.

## Maven – En deklarativ bygghantering

## Vad är Maven?

Maven är ett XML-baserat byggsystem som fokuserar på standardisering och deklaration. Det används ofta för stora och komplexa projekt.

### Livscykel och faser i Maven

Mavens livscykel är en sekvens av steg som körs i en fördefinierad ordning för att automatisera byggprocessen. Varje fas har ett specifikt ansvar, och när en fas körs utförs automatiskt alla föregående faser i ordningen:

1. **validate**: Verifierar att projektet är korrekt och redo att byggas.
2. **compile**: Kompilerar källkoden till bytecode i `.class`-filer.
3. **test**: Kör enhetstester med en lämplig test-ramverk som JUnit.
4. **package**: Paketerar den kompilerade koden och resurser i en JAR- eller WAR-fil.
5. **verify**: Kör integrationstester och verifierar att paketet fungerar som förväntat.
6. **install**: Installerar paketet i den lokala Maven-cachen för att kunna användas som beroende i andra lokala projekt.
7. **deploy**: Distribuerar paketet till ett fjärrbibliotek för delning och produktion.

### Exempel på att köra en fas

För att köra en specifik fas, till exempel `package`, använd kommandot:

```bash
mvn package
```

Detta kommando kör automatiskt alla föregående faser fram till och med `package`.

### Struktur i ett Maven-projekt

- Standardiserad mappstruktur:
  - `src/main/java` för källkod.
  - `src/test/java` för tester.
- Konfiguration via `pom.xml`:

```xml
<dependencies>
    <dependency>
        <groupId>com.google.code.gson</groupId>
        <artifactId>gson</artifactId>
        <version>2.8.9</version>
    </dependency>
</dependencies>
```

### Bygg och körning med Maven

### Förklaring av kommandoradens delar

Exempelkommando:

```bash
mvn archetype:generate \
  -DgroupId=com.example \
   -DartifactId=projekt \
   -DarchetypeArtifactId=maven-archetype-quickstart \
   -DinteractiveMode=false
```

Förklaring:

- `mvn` – Startar Maven-kommandot.
- `archetype:generate` – Använder en arketyp-plugin för att skapa ett nytt projekt.
- `-DgroupId=com.example` – Anger grupp-ID (paketnamn) för projektet.
- `-DartifactId=projekt` – Anger artefakt-ID, som blir projektets namn.
- `-DarchetypeArtifactId=maven-archetype-quickstart` – Specificerar en fördefinierad mall (quickstart) för projektstrukturen.
- `-DinteractiveMode=false` – Stänger av interaktivt läge så att allt genereras automatiskt.

### Skapa en standardiserad mappstruktur

För att skapa en standardiserad mappstruktur med Maven, använd följande kommando:

```bash
mvn archetype:generate \
  -DgroupId=com.example \
  -DartifactId=projekt \
  -DarchetypeArtifactId=maven-archetype-quickstart \
  -DinteractiveMode=false
```

Detta skapar ett projekt med den typiska Maven-strukturen inklusive:

- `pom.xml` – Huvudkonfigurationsfilen för projektet.
- `src/main/java` – För källkod.
- `src/main/resources` – För resurser.
- `src/test/java` – För testkod.
- `src/test/resources` – För testresurser.

## Gradle – Ett modernt och flexibelt alternativ

### Vad är Gradle?

Gradle använder Groovy eller Kotlin DSL för att skapa flexibla byggskript. Det är känt för snabbhet och anpassningsbarhet.

### Struktur i ett Gradle-projekt

- Standardiserad mappstruktur liknande Maven.
- Konfiguration via `build.gradle`:

```groovy
dependencies {
    implementation 'com.google.code.gson:gson:2.8.9'
}
```

### Bygg och körning med Gradle

- Kommandon:
  - `gradle build` – Bygg projektet.
  - `gradle test` – Kör tester.
  - `gradle run` – Kör applikationen.

## Jämförelse mellan Maven och Gradle

| Funktion            | Maven                 | Gradle                |
| ------------------- | --------------------- | --------------------- |
| Konfigurationsspråk | XML                   | Groovy/Kotlin DSL     |
| Lärandekurva        | Enklare för nybörjare | Mer avancerad syntax  |
| Prestanda           | Långsammare           | Snabbare byggtider    |
| Flexibilitet        | Begränsad             | Hög anpassningsbarhet |
| Community-stöd      | Mycket stort          | Växande community     |

## Praktiska exempel och övningar

### Exempelprojekt

- **Maven**: Skapa ett projekt som läser JSON-data med biblioteket Gson.
- **Gradle**: Skapa ett projekt som genererar XML-filer med biblioteket Jackson.

### Hantera beroenden och versioner

- Lägg till beroenden.
- Uppdatera versioner och hantera konflikter.

### Testautomation och plugins

- Kör JUnit-tester.
- Lägg till plugins för rapportering.

## Sammanfattning och tips

- Använd Maven för enklare projekt och Gradle för mer flexibla lösningar.
- Håll beroenden uppdaterade för att undvika säkerhetsproblem.
- Utforska CI/CD-integrationer för automatiska bygg- och testprocesser.

## Bilagor och resurser

### Referenser
