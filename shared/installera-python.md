# Installera Python

Instruktionerna förutsätter och rekommenderar att ditt operativsystem är inställt på engelska.

## MacOS

1. Besök [python.org][1]
    - Ladda hem den senaste installationsfilen som du hittar i `Downloads`. För kursens räkning krävs version 3.11.x som lägst.
2. Starta installationen.

## Ubuntu/Debian Linux

1. De flesta Linux-distributioner har Python förinstallerat.
2. Kontrollera installationen
   - Säkerställ att Python är installerat och kan startas med kommandot `python3` genom att köra följande kommando och få en versionssträng som svar via `python3 --version`
   - Säkerställ att modulen `pip` finns tillgänglig genom att skriva ut dess version via `python -m pip --version`.

## Windows

1. Besök [python.org][1]
    - Ladda hem den senaste installationsfilen som du hittar i `Downloads`. För kursens räkning krävs version 3.11.x som lägst.
2. Starta installationen
    - Välj
        - `Use admin privileges when installing py.exe`
        - `Add python.exe to PATH`
    - Välj `Install Now`
    - Godkänn förfrågan om att få göra förändringar på datorn.
    - Steget `Setup was successful` visas
        - Välj `Disable path length limit`
        - Godkänn förfrågan om att få göra förändringar på datorn.
    - Välj `Close`
3. Vissa versioner av Windows behöver en konfigurationsändring så att de inte försöker installera Python när man skriver vissa kommandon eftersom du vill använda det du har installerat.
    - Starta `Settings`
    - Välj `Apps` / `Advanced app settings` / `App execution aliases`
    - Ändra till `Off` för nedanstående om de finns
        - `App Installer`: `python.exe`
        - `App Installer`: `python3.exe`
4. Kontrollera installationen
    - Starta `PowerShell` eller, för ett sämre kommandoskal, `Command Prompt`
    - Säkerställ att Python kan startas med kommandot `py` eller `python` genom att köra följande kommandon och se att de svarar med att skriva ut vilken version du installerat.
        - `py --version`
        - `python --version`
    - Säkerställ att modulen `pip` finns tillgänglig genom att skriva ut dess version via `python -m pip --version`.
    - Säkerställ att inställningen du gjorde ovan, som gör att Windows inte erbjuder sig att installera Python från Windows Store, fungerar. Kör kommandot `python3` och verifiera att du får ett felmeddelande som svar. I Windows startar man alltså Python genom `py` eller `python` och inte `python3`. Det skiljer sig mellan operativsystemen.

[1]: https://www.python.org/
