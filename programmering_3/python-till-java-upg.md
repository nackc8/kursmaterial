# Python till Java: Övningsuppgifter

## Inför övningsuppgifterna

### Datainläsning från användaren

Exempel på hur ditt program kan begära inmatning från användaren.

```java
import java.util.Scanner;

public class Example {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        System.out.print("Enter first integer: ");
        double number1 = scanner.nextDouble();
        System.out.print("Enter second integer: ");
        double number2 = scanner.nextDouble();

        //
        // Använd number1 och number2 här...
        //

        // Stäng scanner sist för att frigöra resurser. Notera att System.in
        // också stängs när scannern stängs vilket innebär att man bör göra
        // det när man är säker på att ingen mer inmatning behöver läsas.
        scanner.close();
    }
}
```

### Att göra i varje övning

#### Läs dokumentation

Slå upp klasser och metoder i [API Documentation][1]. Läs den inledande beskrivningen och exempelen. Undersök använda metoder i detalj och ögna igenom vilka fler som finns. Slå upp wrapper-klasser för primitiver som `int` och `double`.

#### Använd debugläget i utvecklingsmiljön

Vänj dig vid debugverktygen. Sätt breakpoints, stega igenom koden och kontrollera variabelvärden. [Debugverktygen i IntelliJ][2] och [Visual Studio Code][3] är till stor hjälp för att förstå och hitta fel.

### Metoder för utskrift

Läs på om följande metoder då alla tre kommer att behövas:

```java
System.out.printf(...)
System.out.println(...)
System.out.printf(...)
```

`...` ovan betyder vad som helst och innebär att du bör bekanta dig med metoderna överlag och de olika parametrar som de tar emot.

### Vad är pseudokod

Pseudokod är ett sätt att beskriva algoritmer med en förenklad syntax som liknar naturligt språk. Den används för att tydligt visa logiken bakom en algoritm utan att följa ett specifikt programmeringsspråk.

Pseudokod används ofta för att planera, visualisera och dokumentera lösningar innan de implementeras i kod.

## Övningsuppgifter

### Övning 1: Typkonvertering

Skapa ett program som:

1. Läser in ett heltal (int) från användaren med hjälp av `Scanner`.
2. Konverterar det till:
    - `double`
    - `byte`
    - `String`
3. Skriver ut de konverterade värdena.

#### Exempel

```plaintext
Enter an integer: 42
As double: 42.0
As byte: 42
As String: "42"
```

### Övning 2: Beräkningar med flyttal

Skapa ett program som:

1. Läser in två decimaltal (double) från användaren med `Scanner`.
2. Utför följande operationer och visar resultaten med två decimalers precision:
    - Addition
    - Multiplikation
    - Division

#### Exempel

```plaintext
Enter the first number: 3.456
Enter the second number: 1.234
Addition: 4.69
Multiplication: 4.27
Division: 2.80
```

### Övning 3: Störst av två tal med `if`-sats

Skapa ett program som:

1. Läser in två heltal (`int`) från användaren med hjälp av `Scanner`.
2. Använder en if-sats för att bestämma vilket av de två talen som är störst, eller om de är lika stora.
3. Skriver ut resultatet.

#### Exampel

**Exempel 1:**

```plaintext
Enter the first number: 15
Enter the second number: 20
The largest number is: 20
```

**Exempel 2:**

```plaintext
Enter the first number: 7
Enter the second number: 7
The numbers are equal.
```

### Övning 4: Störst av två tal med ternär operator

Skapa ett program som:

1. Läser in två heltal (`int`) från användaren med hjälp av **`Scanner`**.
2. Använder en **ternär operator** för att bestämma vilket av de två talen som är störst.
3. Skriver alltid ut resultatet som:

   ```plaintext
   The largest number is: <number>
   ```

**Observera:** Programmet ska **inte** kontrollera om talen är lika stora. Det ska alltid skriva ut ett av talen som störst.

#### Exempel

**Exempel 1:**

```plaintext
Enter the first number: 15
Enter the second number: 20
The largest number is: 20
```

**Exempel 2:**

```plaintext
Enter the first number: 7
Enter the second number: 7
The largest number is: 7
```

### Övning 5: Hitta temperaturer

Skapa ett program som:

1. Skapar en **array** med decimaltal (hitta rätt datatyp, tänk på att punkt används som decimalavskiljare i Java) som representerar temperaturen hos en person vid olika tidpunkter.
2. Använder en **`for-each`-loop** för att gå igenom arrayen och hitta:
   - Den **högsta temperaturen**.
   - Den **näst högsta temperaturen**.
3. Skriver ut båda temperaturerna.

**Notera:**

Precisionen hos `float` är tillräcklig för denna typ av problem. Temperaturmätningar är inte så exakta att de kräver högre precision, och människans kroppstemperatur varierar dessutom inom ett mycket begränsat område. Därför är `float` ett bra val för denna uppgift.

#### Exampel

Följande temperaturdata:

- 37,0
- 37,8
- 39,2
- 38,5
- 36,9

Ska ge utskriften:

```plaintext
Highest temperature: 39.2
Second highest temperature: 38.5
```

### Övning 6: Dubblat värde med undantagshantering

Skapa ett program som:

1. Läser in ett heltal (`int`) från användaren med hjälp av **`Scanner`**.
2. Dubblar värdet och skriver ut resultatet.
3. Hanterar ogiltig inmatning, såsom bokstäver, genom att fånga ett **unchecked exception** (`NumberFormatException`).
4. Fortsätter fråga användaren tills en giltig inmatning ges.

Testa programmet med både giltig och ogiltig inmatning.

#### Exempel

```plaintext
Ange ett heltal: abc
Ogiltig inmatning. Försök igen.
Ange ett heltal: 10
Det dubbla värdet är: 20
```

### Övning 7: Bubble sort med do-while-loop

Skapa ett program som:

1. Skapar en array av heltal (`int[]`) som heter `numbers` med minst 10 osorterade värden.
2. Skriver ut den osorterade arrayen.
3. Använder en `do-while`-loop för att sortera arrayen med **bubble sort**:
   - Deklarera en variabel `isModified` som sätts till `false` i början av loopen.
   - Använd en inre for-loop för att gå igenom elementen i arrayen, förutom det sista elementet.
   - Jämför varje par av intilliggande värden och byt plats om det första är större än det andra.
   - Sätt `isModified` till `true` om ett byte sker.
4. Fortsätt loopen tills arrayen är sorterad.
5. Skriv ut den sorterade arrayen.

**Pseudokod:**

Denna pseudokod beskriver samma sak som punktlistan ovan. Den tillhandahålls för att visa programmets struktur på ett alternativt sätt:

```java
skriv ut alla talen i den osorterade arrayen numbers
{ // start do-while
  isModified = false
  för alla index i numbers utom det sista {
    om numbers[index] > numbers[index + 1] {
      byt plats på numbers[index] och numbers[index + 1]
      isModified = true
    }
  }
} medan isModified är true
skriv ut alla talen i den sorterade arrayen numbers
```

#### Exampel

```plaintext
Unsorted array: [5, 3, 8, 6, 2, 7, 4, 1, 9, 0]
Sorted array: [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
```

### Övning 8: Frågesport

Samla ihop grupper och ha en liten tävling. Välj ett av de fyra alternativen på varje fråga. Endast ett av de fyra alternativen är mest rätt, välj det.

| **Fråga** | **Frågetext**                                                                   | **Alt 1**                              | **Alt 2**                      | **Alt 3**                                      | **Alt 4**                                   |
| --------- | ------------------------------------------------------------------------------- | -------------------------------------- | ------------------------------ | ---------------------------------------------- | ------------------------------------------- |
| 1         | Vad kallas det när en mindre datatyp konverteras till en större datatyp?        | Narrowing                              | Widening                       | Downcasting                                    | Explicit konvertering                       |
| 2         | Vilken metod används för att skriva ut text på samma rad utan radbrytning?      | `System.out.println`                   | `System.out.print`             | `System.out.printf`                            | `System.in.read`                            |
| 3         | Vad är en primitiv datatyp?                                                     | En klass som omsluter ett värde        | En typ med direkt värdelagring | En typ som använder objekt istället för värden | En datatyp som endast kan användas i listor |
| 4         | Vilken term beskriver konverteringen från en wrapperklass till en primitiv typ? | Boxing                                 | Autoboxing                     | Unboxing                                       | Casting                                     |
| 5         | Hur många bitar använder datatypen `long`?                                      | 8                                      | 16                             | 32                                             | 64                                          |
| 6         | Vilken av följande är en wrapperklass?                                          | `boolean`                              | `Boolean`                      | `int`                                          | `array av int`                              |
| 7         | Vad gör den ternära operatorn?                                                  | Utför en enkel if-sats                 | Loopar igenom en lista         | Utför en matematisk operation                  | Skapar en metod                             |
| 8         | Vilken loop används för att iterera genom alla element i en array?              | For-loop                               | While-loop                     | For-each-loop                                  | Oändlig loop                                |
| 9         | Hur startar man en oändlig loop?                                                | `for (int i = 0; i < 10; i++) { ... }` | `while(true) { ... }`          | `do { ... } while(i < 5);`                     | `for (int i = 1; i <= 100; i++) { ... }`    |
| 10        | Vad gör `continue` i en loop?                                                   | Avslutar loopen direkt                 | Hoppar till nästa iteration    | Startar en ny loop                             | Avslutar programmet                         |
| 11        | Hur markerar man en kommentar i Java?                                           | `/* kommentar */`                      | `// kommentar`                 | `/** kommentar */`                             | Alla ovanstående                            |
| 12        | Vad kallas det när en variabel ändrar typ automatiskt?                          | Explicit typkonvertering               | Implicit typkonvertering       | Casting                                        | Autoboxing                                  |
| 13        | Vilket begrepp beskriver att variabler är bundna till en specifik typ?          | Dynamiskt typat                        | Statiskt typat                 | Typkonvertering                                | Wrapperklass                                |
| 14        | Vad kallas det när en subklass refereras som sin superklass?                    | Upcasting                              | Downcasting                    | Widening                                       | Narrowing                                   |
| 15        | Vilken metod krävs för att köra igång ett Java-program?                         | `main`                                 | `run`                          | `start`                                        | `execute`                                   |
| 16        | Vad är ett paket i Java?                                                        | En grupp av relaterade klasser         | En fil för kommentarer         | En datatyp                                     | En metod                                    |
| 17        | Vad är en kompileringsenhet?                                                    | En fil                                 | En metod                       | En variabel                                    | Ett paket                                   |
| 18        | Vilken modifierare gör en metod synlig endast inom samma klass?                 | `public`                               | `protected`                    | `private`                                      | `default`                                   |
| 19        | Vad innebär det att något är synligt inom samma paket och i underklasser?       | `public`                               | `protected`                    | `private`                                      | `default`                                   |
| 20        | Vilken åtkomstmodifierare används om ingen modifierare anges?                   | `public`                               | `protected`                    | `private`                                      | Paket-synlighet                             |

## Lösningsförslag

### Övning 1: Typkonvertering

```java
import java.util.Scanner;

public class Conversion {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        System.out.print("Enter an integer: "); // Read input from user
        int number = scanner.nextInt();

        // Convert to different types
        double asDouble = number; // Implicit conversion
        byte asByte = (byte) number; // Explicit conversion
        String asString = Integer.toString(number); // Convert to String

        // Print results
        System.out.println("As double: " + asDouble);
        System.out.println("As byte: " + asByte);
        System.out.println("As String: \"" + asString + "\"");

        // Close the scanner at the end to release resources
        // Note: Closing the scanner also closes System.in, so it cannot be reused afterward.
        scanner.close();
    }
}
```

### Övning 2: Beräkningar med flyttal

```java
import java.util.Scanner;

public class FloatingPointCalculations {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        System.out.print("Enter the first number: "); // Read first number
        double number1 = scanner.nextDouble();
        System.out.print("Enter the second number: "); // Read second number
        double number2 = scanner.nextDouble();

        // Perform calculations
        System.out.printf("Addition: %.2f%n", number1 + number2);
        System.out.printf("Multiplication: %.2f%n", number1 * number2);
        System.out.printf("Division: %.2f%n", number1 / number2);

        // Close the scanner at the end to release resources
        // Note: Closing the scanner also closes System.in, so it cannot be reused afterward.
        scanner.close();
    }
}
```

### Övning 3: Störst av två tal med `if`-sats

```java
import java.util.Scanner;

public class LargestNumber {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        // Read input from user
        System.out.print("Enter the first number: ");
        int number1 = scanner.nextInt();
        System.out.print("Enter the second number: ");
        int number2 = scanner.nextInt();

        // Determine the largest number or if they are equal
        if (number1 > number2) {
            System.out.println("The largest number is: " + number1);
        } else if (number2 > number1) {
            System.out.println("The largest number is: " + number2);
        } else {
            System.out.println("The numbers are equal.");
        }

        scanner.close(); // Close the scanner
    }
}
```

### Övning 4: Störst av två tal med ternär operator

```java
import java.util.Scanner;

public class LargestNumberTernary {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        // Läs in två tal från användaren
        System.out.print("Enter the first number: ");
        int number1 = scanner.nextInt();
        System.out.print("Enter the second number: ");
        int number2 = scanner.nextInt();

        // Använd ternär operator för att bestämma det största talet
        int largest = (number1 > number2) ? number1 : number2;

        // Skriv ut resultatet
        System.out.println("The largest number is: " + largest);

        scanner.close(); // Stäng scannern
    }
}
```

### Övning 5: Hitta temperaturer

```java
public class TemperatureAnalysis {
    public static void main(String[] args) {
        // Create an array of temperatures
        float[] temperatures = {37.0f, 38.5f, 37.8f, 39.2f, 36.9f};

        // Initialize variables
        float highest = Float.MIN_VALUE;
        float secondHighest = Float.MIN_VALUE;

        // Find highest and second highest temperature
        for (float temp : temperatures) {
            if (temp > highest) {
                secondHighest = highest;
                highest = temp;
            } else if (temp > secondHighest) {
                secondHighest = temp;
            }
        }

        // Print results
        System.out.println("Highest temperature: " + highest);
        System.out.println("Second highest temperature: " + secondHighest);
    }
}
```

### Övning 6: Dubblat värde med undantagshantering

```java
import java.util.Scanner;

public class ExceptionExample {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int number = 0;
        boolean validInput = false;

        while (!validInput) {
            System.out.print("Ange ett heltal: ");
            try {
                number = Integer.parseInt(scanner.nextLine());
                validInput = true;
            } catch (NumberFormatException e) {
                System.out.println("Ogiltig inmatning. Försök igen.");
            }
        }

        System.out.println("Det dubbla värdet är: " + (number * 2));
    }
}
```

### Övning 7: Bubble sort med do-while-loop

```java
public class BubbleSort {
    public static void main(String[] args) {
        int[] numbers = {5, 3, 8, 6, 2, 7, 4, 1, 9, 0};

        System.out.print("Unsorted array: ");
        for (int num : numbers) {
            System.out.print(num + " ");
        }
        System.out.println();

        boolean isModified;
        do {
            isModified = false;
            for (int i = 0; i < numbers.length - 1; i++) {
                if (numbers[i] > numbers[i + 1]) {
                    int temp = numbers[i];
                    numbers[i] = numbers[i + 1];
                    numbers[i + 1] = temp;
                    isModified = true;
                }
            }
        } while (isModified);

        System.out.print("Sorted array: ");
        for (int num : numbers) {
            System.out.print(num + " ");
        }
    }
}
```

### Övning 8: Frågesport

Endast ett av de fyra alternativen är mest rätt och räknas som rätt svar. Tomma rutor markerar felaktiga svarsalternativ.

| **Fråga** | **Rätt svar**                  | **Alt 1** | **Alt 2** | **Alt 3** | **Alt 4** |
| --------- | ------------------------------ | --------- | --------- | --------- | --------- |
| 1         | Widening                       |           | X         |           |           |
| 2         | `System.out.print`             |           | X         |           |           |
| 3         | En typ med direkt värdelagring |           | X         |           |           |
| 4         | Unboxing                       |           |           | X         |           |
| 5         | 64                             |           |           |           | X         |
| 6         | `Boolean`                      |           | X         |           |           |
| 7         | Utför en enkel if-sats         | X         |           |           |           |
| 8         | For-each-loop                  |           |           | X         |           |
| 9         | `while(true) { ... }`          |           | X         |           |           |
| 10        | Hoppar till nästa iteration    |           | X         |           |           |
| 11        | Alla ovanstående               |           |           |           | X         |
| 12        | Implicit typkonvertering       |           | X         |           |           |
| 13        | Statiskt typat                 |           | X         |           |           |
| 14        | Upcasting                      | X         |           |           |           |
| 15        | `main`                         | X         |           |           |           |
| 16        | En grupp av relaterade klasser | X         |           |           |           |
| 17        | En fil                         | X         |           |           |           |
| 18        | `private`                      |           |           | X         |           |
| 19        | `protected`                    |           | X         |           |           |
| 20        | Paket-synlighet                |           |           |           | X         |

[1]: https://docs.oracle.com/en/java/javase/25/docs/api/overview-summary.html
[2]: https://www.jetbrains.com/help/idea/debugging-your-first-java-application.html
[3]: https://code.visualstudio.com/Docs/editor/debugging
