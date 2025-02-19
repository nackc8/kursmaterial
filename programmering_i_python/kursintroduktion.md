# Kort introduktion till Python

## Grundläggande Datatyper

För att kontrollera typen av en variabel, använd funktionen `type()`.

```python
x = 42
print(type(x))  # Output: <class 'int'>

y = 3.14
print(type(y))  # Output: <class 'float'>

z = "Hello"
print(type(z))  # Output: <class 'str'>
```

### Numeriska typer

- **int**: Heltal (t.ex. `42`, `-5`). Python `int` har obegränsad precision, begränsad endast av tillgängligt minne.
- **float**: Decimaltal (t.ex. `3.14`, `-0.1`). Python `float` följer IEEE 754-standarden för dubbelprecision flyttalsrepresentation och lagras med en teckenbit, en exponent och en mantissa. Värdet beräknas enligt formeln:

  \[ (-1)^s \times m \times 2^e \]

  där \( s \) är teckenbiten, \( m \) är mantissan (fraktionen) och \( e \) är exponenten.

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
  numbers = range(5)  # Motsvarar [0, 1, 2, 3, 4]
  ```

## Konvertering mellan datatyper

- Använd inbyggda funktioner för att konvertera typer.

  ```python
  age = "25"  # str
  age_int = int(age)  # Konvertera till int
  pi = 3.14
  pi_str = str(pi)  # Konvertera till sträng
  ```

- Konvertera mellan listor och intervall.

  ```python
  numbers = list(range(5))  # [0, 1, 2, 3, 4]
  ```

## Läsa inmatning från användaren

- Använd `input()` för att ta emot inmatning.

  ```python
  name = input("Ange ditt namn: ")
  print("Hello, " + name + "!")
  ```

- Konvertera inmatningsvärden till andra typer.

  ```python
  age = int(input("Ange din ålder: "))
  print("Om 10 år kommer du att vara", age + 10)
  ```

## Villkorssatser: `if`-satser

- Python betraktar icke-nollvärden och icke-tomma strängar och samlingar som "sanna" värden.

  ```python
  if 42:
      print("Detta är sant!")
  ```

- Använda `if`, `elif` och `else` för att fatta beslut.

  ```python
  age = int(input("Ange din ålder: "))
  if age < 18:
      print("Du är minderårig.")
  elif age < 65:
      print("Du är vuxen.")
  else:
      print("Du är pensionär.")
  ```

## Loopar: `for` och `while`

- **`for`-loop**: Itererar över en sekvens.

  ```python
  for i in range(5):
      print(i)
  ```

- **`while`-loop**: Upprepas medan ett villkor är `True`.

  ```python
  x = 5
  while x > 0:
      print(x)
      x -= 1
  ```

- **Använda `break` och `continue`**:

  ```python
  for i in range(10):
      if i == 5:
          break  # Stoppar loopen vid 5
      if i % 2 == 0:
          continue  # Hoppar över jämna tal
      print(i)
  ```

## Funktioner: Definiera med positionella argument

- Definiera en enkel funktion.

  ```python
  def greet(name, age):
      print(f"Hello {name}, you are {age} years old!")
  ```

- Anropa funktionen.

  ```python
  greet("Alice", 30)
  ```

## Importera moduler

- Importera en modul från standardbiblioteket.

  ```python
  import math
  print(math.sqrt(16))
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
