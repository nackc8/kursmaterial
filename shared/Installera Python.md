# Installera Python

Instruktionerna förutsätter och rekommenderar att ditt operativsystem är inställt på engelska.

## Windows

1. Besök [python.org](https://www.python.org/)
    - Ladda hem den senaste installationsfilen som du hittar i `Downloads`. För kursens räkning krävs version 3.10.x som lägst.
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
3. Ändra Windows-inställningar så att den inte försöker installera Python när man skriver vissa kommandon eftersom du vill använda det du har installerat
    - Starta `Settings`
    - Välj `Apps` / `Advanced app settings` / `App execution aliases`
    - Ändra till `Off` för
        - `App Installer`: `python.exe`
        - `App Installer`: `python3.exe`
4. Kontrollera installationen
    - Starta `PowerShell` eller, för ett sämre kommandoskal, `Command Prompt`
    - Säkerställ att Python kan startas med kommandot `py` eller `python` genom att köra följande kommandon och se att de svarar med att skriva ut vilken version du installerat.
        - `py --version`
        - `python --version`
    - Säkerställ att inställningen du gjorde ovan, som gör att Windows inte erbjuder sig att installera Python från Windows Store, fungerar. Kör kommandot `python3` och verifiera att du får ett felmeddelande som svar. I Windows startar man alltså Python genom `py` eller `python` och inte `python3`. Det skiljer sig mellan operativsystemen.
### Ubuntu/Debian Linux

1. De flesta Linux-distributioner har Python förinstallerat.
2. Säkerställ att Python är installerat och kan startas med kommandot `python3` genom att köra följande kommando och få en versionssträng som svar.
        - `python3 --version`
