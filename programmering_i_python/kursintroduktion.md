# Kort introduktion till Python

## Grundläggande Datatyper

För att kontrollera typen av en variabel, använd funktionen `type()`. Du kan även titta på objektets identitet med funktionen `id()`.

```python
x = 42
print(type(x))  # Check the type of x
print(id(x))    # Check the unique id of x

y = 3.14
print(type(y))  # Check the type of y
print(id(y))    # Check the unique id of y

z = "Hello"
print(type(z))  # Check the type of z
print(id(z))    # Check the unique id of z
```

### Numeriska typer

- **int**: Heltal (t.ex. `42`, `-5`). Python `int` har obegränsad precision, begränsad endast av tillgängligt minne.
- **float**: Decimaltal (t.ex. `3.14`, `-0.1`). Python `float` följer IEEE 754-standarden för dubbelprecision flyttalsrepresentation och lagras med en teckenbit, en exponent och en mantissa. Värdet beräknas enligt formeln:

  ```math
  (-1)^s \times m \times 2^e
  ```

  där $(s)$ är teckenbiten, $m$ är mantissan (fraktionen) och $e$ är exponenten.

### Booleska typer

- **bool**: Representerar `True` eller `False`.

  ```python
  is_active = True
  is_logged_in = False
  ```

### Texttyp

- **str**: En sekvens av tecken (t.ex. `'Hello, World!'`).

  ```python
  name = "Alice"
  greeting = 'Hello'
  ```

### Sekvenstyper

- **list**: En samling av objekt (förändringsbar, ordnad).

  ```python
  numbers = [1, 2, 3, 4, 5]
  words = ["apple", "banana", "cherry"]
  ```

- **range**: Representerar en sekvens av nummer.

  ```python
  numbers_range = range(5)  # similar to [0, 1, 2, 3, 4]
  ```

## Konvertering mellan datatyper

- Använd inbyggda funktioner för att konvertera typer.

  ```python
  age_str = "25"      # str
  age_int = int(age_str)  # Convert to int

  pi = 3.14
  pi_str = str(pi)     # Convert float to string
  ```

- Konvertera en range till en lista.

  ```python
  numbers_list = list(range(5))  # [0, 1, 2, 3, 4]
  ```

## Läsa inmatning från användaren

- Använd `input()` för att ta emot inmatning.

  ```python
  user_name = input("Ange ditt namn: ")
  print("Hello, " + user_name + "!")
  ```

- Konvertera inmatningsvärden till andra typer.

  ```python
  user_age = int(input("Ange din ålder: "))
  print("Om 10 år kommer du att vara", user_age + 10)
  ```

## Villkorssatser: `if`-satser

- Python betraktar icke-nollvärden, icke-tomma strängar och icke-tomma samlingar som "sanna" (truthy) värden. Detta fenomen kallas för "truthiness".

  ```python
  if 42:  # 42 är truthy
      print("Detta är sant eftersom 42 inte är 0 eller False!")
  ```

- Använda `if`, `elif` och `else` för att fatta beslut.

  ```python
  user_age = int(input("Ange din ålder: "))
  if user_age < 18:
      print("Du är minderårig.")
  elif user_age < 65:
      print("Du är vuxen.")
  else:
      print("Du är pensionär.")
  ```

## Loopar: `for` och `while`

- **`for`-loop** med `range()`: Itererar över en sekvens av nummer.

  ```python
  for i in range(5):
      # i kommer att anta värdena 0, 1, 2, 3, 4
      print(i)  # Print i
  ```

- **`for`-loop** över en lista:

  ```python
  fruit_list = ["apple", "banana", "cherry"]
  for fruit in fruit_list:
      print(fruit)  # Print each fruit
  ```

- **`while`-loop**: Upprepas medan ett villkor är `True`.

  ```python
  counter = 5
  while counter > 0:
      print(counter)  # Print counter
      counter -= 1
  ```

- **Använda `break` och `continue`**:

  ```python
  for i in range(10):
      if i == 5:
          break  # Stop the loop at 5
      if i % 2 == 0:
          continue  # Skip even numbers
      print(i)  # Print only odd numbers until 5
  ```

## Funktioner: Definiera med positionella argument

- Definiera en enkel funktion.

  ```python
  def greet(person_name, person_age):
      # Print a greeting message
      print(f"Hello {person_name}, you are {person_age} years old!")
  ```

- Anropa funktionen.

  ```python
  greet("Alice", 30)  # Call greet with arguments "Alice" and 30
  ```

## Importera moduler

- Importera en modul från standardbiblioteket.

  ```python
  import math
  print(math.sqrt(16))  # Print the square root of 16
  ```

- Importera en annan fil från samma katalog.

  Antag att en fil `utils.py` innehåller:

  ```python
  def say_hello():
      print("Hello from utils!")
  ```

  Du kan importera den i en annan fil:

  ```python
  import utils
  utils.say_hello()
  ```

## Kopiering av värden och referenser

I Python är alla variabler i grunden referenser till objekt. Men när det gäller immutabla typer (t.ex. tal som `int` och `float`, strängar och boolska värden) känns det ofta som att tilldelningen kopierar "värdet". För muterbara objekt (t.ex. listor, dictionaries) är det tydligare att variabler pekar på samma objekt i minnet.

- **Tal och andra immutabla objekt (copy by value)**

  Även om Python internt använder referenser, beter sig en tilldelning av t.ex. heltal som om värdet kopieras. Eftersom `int` är immutabel (kan inte ändras efter skapandet) så märks ingen skillnad om man har två variabler med samma värde.

  ```python
  num_a = 10
  num_b = num_a  # Feels like a copy of the value
  print("num_a:", num_a, "id:", id(num_a))
  print("num_b:", num_b, "id:", id(num_b))

  # Ändra num_a
  num_a = 20
  # num_b är oförändrad
  print("num_a:", num_a, "id:", id(num_a))
  print("num_b:", num_b, "id:", id(num_b))
  ```

  I exemplet ovan ser du att `num_b` inte påverkas om du ändrar `num_a`. Detta liknar "copy by value".

- **Listor och andra muterbara objekt (copy by reference)**

  När du tilldelar en lista till en annan variabel pekar båda variablerna på samma underliggande lista. Ändrar du i listan via den ena variabeln märks det via den andra.

  ```python
  my_list = [1, 2, 3]
  another_list = my_list  # Reference to the same list
  print("my_list id:", id(my_list))
  print("another_list id:", id(another_list))

  my_list.append(4)
  print("my_list:", my_list)          # [1, 2, 3, 4]
  print("another_list:", another_list)  # [1, 2, 3, 4] (same object)
  ```

  Här är det tydligt att `my_list` och `another_list` pekar på samma objekt i minnet. Detta kallas ofta "copy by reference".

Sammanfattningsvis:

- Immutabla typer (t.ex. `int`, `float`, `bool`, `str`) beter sig som om de kopieras med värde (copy by value).
- Muterbara typer (t.ex. `list`, `dict`) beter sig som om de kopieras med referens (copy by reference).

I praktiken används alltid referenser i Python, men uppdelningen mellan immutabla och muterbara objekt gör att det ibland upplevs som olika typer av kopiering.
