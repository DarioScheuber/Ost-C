AVRworkspace_multiroot
======================

Diese Vorlage zeigt eine Multi-Root-Arbeitsumgebung fuer VS Code.

Start:
1. In VS Code die Datei C.code-workspace oeffnen.
2. Der Ordner 00_AVR_Project_Template erscheint als eigener Root im Explorer.
3. Weitere AVR-Projekte oder Referenzprojekte koennen als zusaetzliche Roots
   in die Workspace-Datei aufgenommen werden.
4. Beim ersten Oeffnen fragt VS Code eventuell, ob dem Workspace vertraut wird.
   Diese Abfrage ist normal, weil Tasks und Action Buttons Terminalbefehle
   ausfuehren koennen.

Neues AVR-Projekt anlegen:
Der Eintrag C (Workspace) ist nur der Multi-Root-Container und kein echter
Ordner. Deshalb kann dort nicht direkt eingefuegt werden.
1. Den Ordner 00_AVR_Project_Template im Windows File Explorer kopieren.
2. Die Kopie im Windows File Explorer passend umbenennen, zum Beispiel
   01_Blink_LED.
3. In VS Code: File -> Add Folder to Workspace... auswaehlen.
4. Den neuen Projektordner, zum Beispiel 01_Blink_LED, hinzufuegen.
5. Die Workspace-Datei speichern: File -> Save Workspace.

Copy/Paste direkt im VS-Code-Explorer funktioniert nur innerhalb eines
angezeigten echten Ordners, nicht auf dem Multi-Root-Container C (Workspace).

Danach erscheint das neue Projekt als eigener Root im Explorer. Die Dateien
.vscode/c_cpp_properties.json, .vscode/tasks.json und .vscode/settings.json
wurden mitkopiert und funktionieren weiterhin, weil sie ${workspaceFolder}
verwenden. Das Projekt muss deshalb nach dem Kopieren normalerweise nicht
weiter angepasst werden.

Wichtig fuer Build, Compile, Flash und Clean:
Die Action Buttons wechseln vom Ordner der gerade aktiven Datei eine Ebene nach
oben und fuehren dort make aus. Sie sind deshalb fuer Dateien in app/, src/ und
includes/ gedacht. Soll also 01_Blink_LED gebaut werden, muss zuerst eine Datei
aus 01_Blink_LED/app, 01_Blink_LED/src oder 01_Blink_LED/includes geoeffnet oder
aktiv sein.

Wichtig:
Die Datei C.code-workspace wird im VS-Code-Explorer
nicht automatisch angezeigt. Sichtbar sind nur die Ordner, die in der
Workspace-Datei unter "folders" eingetragen sind. Deshalb gibt es diesen
Ordner _WorkspaceInfo als sichtbaren Informations-Root.

Vorteil:
- Jedes AVR-Projekt kann seine eigene .vscode-Konfiguration besitzen.
- IntelliSense sucht pro Projekt nur in includes/, ext_includes/ und im
  WinAVR-Systempfad.
- Andere Projekte bleiben im Explorer sichtbar, ohne dass deren Header
  versehentlich bevorzugt werden.
- Die Action Buttons arbeiten mit dem Projektordner der gerade aktiven Datei.
  Ist eine Datei aus Projekt A geoeffnet, wird Projekt A gebaut. Ist eine Datei
  aus Projekt B geoeffnet, wird Projekt B gebaut.

Projektstruktur eines AVR-Projekts:
- app/          : Applikation oder Testprogramm mit main.c
- src/          : wiederverwendbare C-Dateien fuer eine spaetere Library
- includes/     : eigene oeffentliche Header
- ext_includes/ : Header externer Bibliotheken
- ext_libs/     : externe statische Bibliotheken

Hinweis zu den Action Buttons:
Die Buttons Compile, Build, Flash und Clean sollten nur verwendet werden, wenn
gerade eine Datei aus app/, src/ oder includes/ eines AVR-Projekts aktiv ist.
Ist stattdessen dieser README, das Makefile selbst oder eine Datei aus einem
reinen Referenzordner aktiv, stimmt der automatisch gewaehlte Pfad nicht.

Hinweis zu Workspace Trust:
Dass VS Code beim ersten Verwenden von Tasks oder Action Buttons nach Vertrauen
fragt, ist ein Sicherheitsmechanismus und sollte nicht in der Vorlage
abgeschaltet werden. Auf vorbereiteten Kursrechnern kann der verwendete
Vorlagen- oder Kursordner einmalig als vertrauenswuerdig markiert werden.

Hinweis zu IntelliSense:
Wenn ein Projektordner kopiert oder umbenannt wird, bleibt seine IntelliSense-
Konfiguration gueltig, solange die .vscode-Konfiguration im Projektordner liegt
und nur ${workspaceFolder} verwendet.

Coding-Regeln fuer Claude Code und GitHub Copilot:
Die Regeln liegen zentral im Workspace-Ordner C/ (nicht in den Projekten):
- .claude/skills/avr-c-coding/SKILL.md   : Code-Guidelines (Hauptquelle)
- .claude/skills/avr-c-coding/hardware.md: Pinbelegung Board + Datenblatt-Kapitel
- CLAUDE.md                              : Claude Code laedt die Regeln automatisch
- .github/instructions/avr-c.instructions.md : Copilot-Verweis auf den Skill
Copilot findet die Dateien ueber die Einstellungen chat.agentSkillsLocations und
chat.instructionsFilesLocations in C.code-workspace. Der Pfad "../" wird vom
ERSTEN Ordner der Workspace-Datei aus aufgeloest, deshalb muss "doc" als erster
Ordner unter "folders" stehen bleiben.
