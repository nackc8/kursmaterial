# Byggsystem och beroendehantering i Java

## Maven

### Standardstruktur i Maven-projekt

Maven använder en konvention över konfiguration (convention over configuration) för att förenkla projektuppsättningen. Genom att följa en standardiserad struktur minimeras behovet av specialkonfigurationer och verktyg kan enkelt förstå projektets layout. Här är en detaljerad genomgång:

- **`src/main/java`**: Innehåller projektets källkod organiserad i paket. Detta är den primära platsen för programlogik och funktionalitet.
- **`src/main/resources`**: Används för resurser som konfigurationsfiler (t.ex. `application.properties`), bilder och statiska filer som HTML och CSS.
- **`src/test/java`**: Innehåller testkod för enhetstester och integrationstester. Den följer samma paketstruktur som källkoden.
- **`src/test/resources`**: Används för testresurser som konfigurationsfiler och mockdata som används i tester.
- **`target/`**: Genereras av Maven under byggprocessen och innehåller kompilerade klasser, JAR- eller WAR-filer samt testresultat. Eftersom denna mapp återskapas automatiskt bör den uteslutas från versionskontroll.
- **`pom.xml`**: Projektets hjärta som definierar beroenden (t.ex. `org.springframework.boot:spring-boot-starter-web`) och plugins (t.ex. `maven-compiler-plugin` för att specificera Java-version). Den används också för att definiera projektets namn, version och byggfaser.

### `pom.xml`: Projektets konfigurationsfil

#### Maven Compiler Plugin

`maven-compiler-plugin` är ett plugin som ansvarar för att kompilera Java-källkoden till bytecode. Det låter dig specificera Java-versionen som används för att kompilera koden och säkerställer kompatibilitet med rätt JDK-version.

I exemplet nedan:
```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-compiler-plugin</artifactId>
    <version>3.8.1</version>
    <configuration>
        <source>23</source>
        <target>23</target>
    </configuration>
</plugin>
```

- **`<groupId>` och `<artifactId>`** identifierar pluginet.
- **`<version>`** specificerar vilken version av pluginet som används.
- **`<configuration>`** innehåller inställningar,

Detta säkerställer att koden kompileras korrekt enligt Java 23 och är kompatibel med den versionen.

`pom.xml` (Project Object Model) är hjärtat i ett Maven-projekt. Den:
- Beskriver projektets metadata såsom grupp-ID, artefakt-ID och version.
- Definierar beroenden som behövs för projektet.
- Anger plugins och deras konfigurationer.
- Hanterar byggfaser och rapportering.

Exempel på en komplett `pom.xml`:

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.example</groupId>
    <artifactId>projekt</artifactId>
    <version>1.0-SNAPSHOT</version>

    <dependencies>
        <dependency>
            <groupId>com.google.code.gson</groupId>
            <artifactId>gson</artifactId>
            <version>2.8.9</version>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.8.1</version>
                <configuration>
                    <source>23</source>
                    <target>23</target>
                </configuration>
            </plugin>
        </plugins>
    </build>
</project>
```

### Hitta beroenden att använda

För att hitta beroenden kan du använda tjänster som:
- **Maven Central Repository**: https://search.maven.org
- **MVNRepository**: https://mvnrepository.com

Sök på bibliotekets namn, t.ex. "Gson", och välj versionen du vill använda. Kopiera `<dependency>`-blocket och lägg det i `pom.xml`.

När beroendet är tillagt hämtar Maven det automatiskt från fjärrlagret nästa gång du bygger projektet.

Maven följer en standardiserad mappstruktur som gör det enklare att förstå och underhålla projekt. Strukturen är utformad för att separera källkod, testkod och resurser. Här är en detaljerad genomgång:

- **`src/main/java`**: Innehåller projektets källkod organiserad i paket. Detta är den primära platsen för programlogik och funktionalitet.
- **`src/main/resources`**: Används för resurser som konfigurationsfiler, egenskapsfiler och bilder som behövs vid körning.
- **`src/test/java`**: Innehåller testkod för enhetstester och integrationstester. Den följer samma paketstruktur som källkoden.
- **`src/test/resources`**: Används för testresurser som konfigurationsfiler och mockdata som används i tester.
- **`target/`**: Skapas av Maven vid byggprocessen och innehåller kompilerade klasser, JAR- eller WAR-filer samt testresultat.
- **`pom.xml`**: Projektets konfigurationsfil som definierar beroenden, plugins och bygginstruktioner.

Exempel på beroenden i `pom.xml`:

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

### Att köra faser

För att köra en specifik fas, till exempel `package`, använd kommandot:

```bash
mvn package
```

Detta kommando kör automatiskt alla föregående faser fram till och med `package`.



### Maven-archetype: Skapa projektmallar

En **archetype** i Maven är en mall som används för att snabbt skapa nya projekt med en standardiserad struktur. Archetypes förenklar skapandet av nya projekt genom att generera den nödvändiga mappstrukturen, konfigurationsfiler som `pom.xml` och exempelklasser för källkod och tester.

Exempel: Kommandot nedan skapar ett nytt projekt baserat på arketypen `maven-archetype-quickstart`, som är en enkel mall för Java-projekt:
```bash
mvn archetype:generate \
  -DgroupId=com.example \
  -DartifactId=projekt \
  -DarchetypeArtifactId=maven-archetype-quickstart \
  -DinteractiveMode=false
```

Denna mall innehåller:
- En standardiserad mappstruktur.
- En fördefinierad `pom.xml`-fil.
- Exempelkod för en Java-applikation.

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

- `mvn`: Startar Maven-kommandot.
- `archetype:generate`: Använder en arketyp-plugin för att skapa ett nytt projekt.
- `-DgroupId=com.example`: Anger grupp-ID (paketnamn) för projektet.
- `-DartifactId=projekt`: Anger artefakt-ID, som blir projektets namn.
- `-DarchetypeArtifactId=maven-archetype-quickstart`: Specificerar en fördefinierad mall (quickstart) för projektstrukturen.
- `-DinteractiveMode=false`: Stänger av interaktivt läge så att allt genereras automatiskt.
