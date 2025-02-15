# Commitkonventioner

Detta repository använder en tolkning av **Conventional Commits** som gör att förändringar kan delas upp i olika typer då själva featureserna för repot är dokumentation. Om vi strikt följde Conventional Commits så skulle allt bli `docs:` och inte vara till hjälp.

Nedan beskrivs de commit-typer som används:

## Commit-typer

### feat: (ny funktionalitet eller beskrivning)

Används när något nytt beskrivs eller läggs till, exempelvis:

- Nya avsnitt i dokumentationen.
- Nya övningar eller exempel.

**Exempel:**

```plaintext
feat(i): add example of using regex in grep
```

### fix: (korrigering av fel i innehåll eller kodexempel)

Används för att rätta till fel, t.ex.:

- Felaktiga kodexempel.
- Stavfel eller andra tydliga misstag i texten.

**Exempel:**

```plaintext
fix(i): correct syntax error in Bash script
```

### style: (formaterings- och layoutjusteringar)

Används för ändringar som endast påverkar kodstil eller formatering utan att ändra innehållets innebörd, såsom:

- Ta bort dubbla tomma rader.
- Ta bort onödiga blankrader mellan punkter i en lista.

**Exempel:**

```plaintext
style(i): remove extra blank line in markdown list
```

### chore: (uppdateringar av utvecklingsmiljön eller beroenden)

Används för ändringar i repositoryts struktur eller konfiguration som inte påverkar innehållet, t.ex.:

- Uppdatering av linters eller formatteringsverktyg.
- Uppdatering av README.md utan att ändra innehållet.

**Exempel:**

```plaintext
chore(i): add commitlint configuration
```

### docs: (mindre justeringar i dokumentation utan nya funktioner eller korrigeringar)

Används när rubriker byts ut eller omformuleras, eller vid andra justeringar som inte tillför nytt innehåll eller rättar till fel.

**Exempel:**

```plaintext
docs(i): rename section for SSH
```
