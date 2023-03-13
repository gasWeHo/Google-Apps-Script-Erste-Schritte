## GAS-Projekte in Visual Studio Code entwickeln

Eine beliebte Alternative zum Google Apps Script (GAS) Editor stellt Visual Studio Code (VS Code) dar. Neben komfortablen Coden ist somit auch ein lokales Programm-Backup gegeben. Push & Pull Befehle ermöglichen den Code-Abgleich in beide Richtungen, also zwischen lokalem Rechner und der Google-Cloud.

Inhaltlich identische Skript-Dateien werden dabei mit unterschiedlichen Dateiendungen abgelegt:
  * .gs in GAS (Cloud)
  * .js in VS Code (lokal)

Folgende Tabelle zeigt die wesentlichen Schritte, um genau diesen Komfort zu bewerkstelligen. 

| Link / App | Bemerkung |
|:------------------ |-----------------------------------------|
| [Google Account](https://www.google.com/) | mit Google Account einloggen, ggf. zuvor Konto erstellen |
| [GAS Settings](https://script.google.com/home/usersettings) | Google Apps Script API einschalten |
| [GAS Script](https://developers.google.com/apps-script/guides/standalone?hl=de) | Eigenständigen oder gebundenen Skript-Type erstellen |
| [VS Code](https://code.visualstudio.com/download) | Download und Installation von VS Code (LTS/Installer) |
| [Node.js](https://nodejs.org/en/download/) | Download und Installation von Node.js |
| [npm & clasp](https://www.youtube.com/watch?v=lwxiEB-Mnys) | ausführliches Installations - Video |
| VS Code | lokalen Projekt-Ordner mit Unterordner für Sourcen (z.B. *src*) erstellen |
| [Terminal](https://github.com/google/clasp) | als Admin in VS Code Terminal:  *npm install -g @google/clasp* |
| [Scripts disabled?](https://www.google.com/search?q=error+Visual+Studio+terminal+script+running+power+shell&ei=TfEIZKTgF9TBlAbplJSgCw&ved=0ahUKEwjkuuyfl839AhXUIMUKHWkKBbQQ4dUDCBA&uact=5&oq=error+Visual+Studio+terminal+script+running+power+shell&gs_lcp=Cgxnd3Mtd2l6LXNlcnAQAzoFCAAQogRKBAhBGABQAFi6CGCUC2gAcAF4AIABbYgBygSSAQMzLjOYAQCgAQHAAQE&sclient=gws-wiz-serp#fpstate=ive&vld=cid:5984ff59,vid:X7mg3pJfhZQ) | im VS Code Terminal mit *clasp -v* die Version prüfen, falls Scripts disabled, die Windows Power Shell als Admin öffnen und dort<br>-> *Get-ExecutionPolicy -List* , falls Scope-Eintrag bei localMachine=Restricted<br>-> *Set-ExecutionPolicy Unrestricted* und mit *J* quittieren|
| Terminal | Clasp-Login mit *clasp login* und dabei entsprechenden Google-Account auswählen|
| Terminal | Node-Projekt initialisieren mit *npm init* , dabei alles quittieren -> package.json|
| Terminal | GAS-Projekt lokal clonen mit *clasp clone "GAS-Projekt-ID" --rootDir src*|
| Terminal | Source-Dateien von lokal in die Cloud kopieren mit *clasp push*<br>Alternativ mit *clasp push -w* , dann automatisch nach speichern in VS Code|
| Terminal | Source-Dateien von der Cloud nach lokal kopieren mit *clasp pull*|
| [Terminal](https://github.com/google/clasp/blob/master/docs/typescript.md) | Code autocomplete mit *npm i -S @types/google-apps-script*|

Tabelle 1. wesentliche Schritte um GAS-Projekte in VS Code zu entwickeln 

---






[Zurück](README.md)