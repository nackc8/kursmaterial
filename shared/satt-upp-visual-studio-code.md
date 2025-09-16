# Sätt upp Visual Studio Code

Instruktionerna förutsätter och rekommenderar att ditt operativsystem är inställt på engelska.

Det står dig fritt att ändra eller hoppa över instruktionerna nedan om du vet säkert att du kan få utvecklingsmiljön att fungera på ett annat sätt som du föredrar. Om du är osäker bör du dock göra allt nedan i sin helhet.

Följ både de specifika instruktionerna för ditt operativsystem och de allmänna instruktionerna nedan.

## Windows

1. Besök [code.visualstudio.com][1].
    1. Ladda hem den senaste installationsfilen genom att välja `Download for Windows`.
2. Starta installationen.
    1. Standardinställningarna kan användas utan ändringar. Om du tycker att det verkar smidigt kan du, när möjligheten ges, välja:
        - `Add "Open with Code" action to Windows Explorer file context menu`.
        - `Add "Open with Code" action to Windows Explorer directory context menu`.
        - Var noga med att låta följande vara fortsatt valt eftersom det ofta används:
            - `Register Code as an editor for supported file types`.
            - `Add to PATH (requires shell restart)`.
    2. Avsluta installationsguiden med att starta Visual Studio Code.
    3. Stäng Visual Studio Code när du har sett att det startade.

3. Säkerställ att `code` är i din PATH.
    1. Starta `PowerShell`.
    2. Säkerställ att du kan starta Visual Studio Code genom att bara skriva kommandot `code`. Stäng Visual Studio Code när du har sett att det startade.
        - Om inte code finns i din PATH så
            - Kan du börja med att starta en ny terminal och se om det fungerar då?
            - Annars så kanske du missade att kryssa i det i steg 2 ovan? Avinstallera i så fall Visual Studio Code och installera om det.

## Ubuntu

1. Besök [code.visualstudio.com][1].
2. Ladda ner den senaste installationsfilen för Debian-baserade system genom att välja .deb (Debian, Ubuntu...).
3. Starta installationen med kommandot:

    ```bash
    sudo apt install ./code_X.XX.X-XXXXXXXXXX_amd64.deb
    ```

    `X` representerar versionsnumret på den Visual Studio Code-fil du har laddat ned.

## Alla operativsystem

1. Konfigurera utvecklingsmiljön.
    1. Starta Visual Studio Code på valfritt sätt.
    2. `Welcome`-fliken visas och innehåller en lista med saker som nya användare kan vilja göra för att anpassa och lära sig utvecklingsmiljön. Det är viktigt att du går igenom hela listan vid första möjliga tillfälle utanför lektionstid.
    3. Välj `Terminal` / `New Terminal` för att starta ett kommandoskal. Kopiera och klistra in följande kommandorader och tryck på Enter/Return. Raderna installerar tillägg till Visual Studio Code. Varje rad ska skriva ut att tillägget installerades korrekt, och när alla rader körts ska du stå vid en tom kommandoprompt.

        ```bash
        code --install-extension almenon.arepl
        code --install-extension charliermarsh.ruff
        code --install-extension esbenp.prettier-vscode
        code --install-extension foxundermoon.shell-format
        code --install-extension humao.rest-client
        code --install-extension ms-azuretools.vscode-docker
        code --install-extension ms-python.python
        code --install-extension ms-vscode.powershell
        code --install-extension ms-vscode-remote.remote-ssh
        code --install-extension ms-vsliveshare.vsliveshare
        code --install-extension ritwickdey.LiveServer
        code --install-extension shd101wyy.markdown-preview-enhanced
        code --install-extension timonwong.shellcheck
        code --install-extension yzhang.markdown-all-in-one
        ```

    4. Välj `Extensions`-knappen till vänster vars ikon är en grupp fyrkanter och veckla ut `INSTALLED`. Där kan du se de tillägg som du installerade i förra steget. Det bör vara fler tillägg än antalet rader ovan då några följer med från början och vissa tillägg kräver andra tillägg för att fungera.
    5. Välj `View` / `Command Palette...` och välj `Preferences: Open User Settings (JSON)` för att se och ändra utvecklingsmiljöns inställningar. Om du vet att du har gjort egna inställningar som du vill behålla behöver du pussla ihop dem. Annars kan du ta bort allt som står där nu så att det blir tomt och sedan kopiera allt nedan. Då har du en bra grundkonfiguration som du senare kan justera efter tycke och smak.

    ```json
    {"[dockercompose]":{"editor.defaultFormatter":"esbenp.prettier-vscode"},"[javascript]":{"editor.defaultFormatter":"esbenp.prettier-vscode"},"[javascriptreact]":{"editor.defaultFormatter":"esbenp.prettier-vscode"},"[json]":{"editor.defaultFormatter":"esbenp.prettier-vscode"},"[jsonc]":{"editor.defaultFormatter":"esbenp.prettier-vscode"},"[jsonl]":{"editor.defaultFormatter":"esbenp.prettier-vscode"},"[markdown]":{"editor.wordWrap":"bounded","editor.wordWrapColumn":80,"editor.tabSize":4,"editor.defaultFormatter":"yzhang.markdown-all-in-one"},"[powershell]":{"editor.defaultFormatter":"ms-vscode.powershell"},"[python]":{"editor.codeActionsOnSave":{"source.organizeImports.ruff":"always"},"editor.defaultFormatter":"charliermarsh.ruff"},"[shellscript]":{"editor.defaultFormatter":"foxundermoon.shell-format","editor.insertSpaces":false},"[typescript]":{"editor.defaultFormatter":"esbenp.prettier-vscode"},"[typescriptreact]":{"editor.defaultFormatter":"esbenp.prettier-vscode"},"[yaml]":{"editor.defaultFormatter":"esbenp.prettier-vscode"},"editor.acceptSuggestionOnEnter":"off","editor.bracketPairColorization.independentColorPoolPerBracketType":true,"editor.cursorStyle":"block","editor.defaultFormatter":"foxundermoon.shell-format","editor.formatOnSave":true,"editor.inlineSuggest.enabled":false,"editor.minimap.enabled":false,"explorer.confirmDelete":false,"explorer.confirmDragAndDrop":false,"explorer.excludeGitIgnore":false,"files.autoSave":"onFocusChange","files.insertFinalNewline":true,"files.trimTrailingWhitespace":true,"git.autofetch":true,"git.confirmSync":false,"git.enableSmartCommit":true,"git.openRepositoryInParentFolders":"never","github.copilot.enable":{"*":false},"json.format.enable":false}
    ```

    6. Nu är din utvecklingsmiljö redo att användas! Det är ditt eget ansvar att bekanta dig med hur den fungerar. Om du råkat stänga `Welcome`-fliken kan du öppna den igen med `Help` / `Welcome`.

### Valfri fördjupning

Efter att ha gått igenom `Help` / `Welcome`:

1. Utforska [marknadsplatsen för Visual Studio Code][2]-tillägg! Det finns garanterat fler tillägg som kan vara nyttiga, kul eller som anpassar utvecklingsmiljön efter dina preferenser.
2. Installera ett typsnitt med fast bredd från [Google Fonts][3] som du tycker om och ställ in utvecklingsmiljön att använda det för programkod. Några populära typsnitt för programmering är exempelvis `Source Code Pro`, `IBM Plex Mono` och `JetBrains Mono`.
3. Om du använder mer än en dator så kan det vara smidigt att [synkronisera inställningarna][4].

[1]: https://code.visualstudio.com/
[2]: https://marketplace.visualstudio.com/VSCode
[3]: https://fonts.google.com/?preview.size=16&classification=Monospace&sort=popularity
[4]: https://code.visualstudio.com/docs/editor/settings-sync
