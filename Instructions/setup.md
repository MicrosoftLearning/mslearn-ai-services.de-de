---
lab:
  title: Einrichten der Laborumgebung
  module: Setup
---

# Einrichten der Laborumgebung

Diese Übungen sind für die Ausführung in einer gehosteten Laborumgebung konzipiert. Wenn Sie sie auf Ihrem eigenen Computer durchführen möchten, können Sie dazu die folgende Software installieren. Bei der Verwendung Ihrer eigenen Umgebung können unerwartete Dialoge und Verhaltensweisen auftreten. Aufgrund der Vielzahl möglicher lokaler Konfigurationen kann das Kursteam keine Unterstützung für Probleme leisten, die in Ihrer eigenen Umgebung auftreten können.

> **Hinweis:** Die folgenden Anweisungen beziehen sich auf einen Windows 11-Computer. Sie können auch Linux oder MacOS verwenden. Möglicherweise müssen Sie die Labanweisungen für Ihr gewähltes Betriebssystem anpassen.

### Basisbetriebssystem (Windows 11)

#### Windows 11

Installieren Sie Windows 11, und wenden Sie alle Updates an.

#### Microsoft Edge

Installieren von [Edge (Chromium)](https://microsoft.com/edge)

### .NET Core SDK

1. Führen Sie den Download und die Installation über https://dotnet.microsoft.com/download durch. Laden Sie das .NET Core SDK und nicht nur die Runtime herunter. Wenn Sie die Labs in diesem Kurs auf Ihrem eigenen Computer ausführen, sollten Sie .NET 7.0 installiert haben. Die Labs wurden mit .NET 7.0 getestet, doch 7.0 wird derzeit nicht mehr unterstützt. Sie können 8.0 verwenden, aber es können möglicherweise einige kleinere Probleme auftreten. Es wird dringend empfohlen, die gehostete Umgebung zu verwenden.

### C++ Redistributable

1. Laden Sie das Visual C++ Redistributable (x64)-Paket von https://aka.ms/vs/16/release/vc_redist.x64.exe herunter, und installieren Sie es.

### Node.JS

1. Laden Sie die neueste LTS-Version von https://nodejs.org/en/download/ herunter. 
2. Installieren mithilfe der Standardoptionen

### Python (und erforderliche Pakete)

1. Laden Sie Version 3.11 von https://docs.conda.io/en/latest/miniconda.html herunter. 
2. Ausführen des Setups für die Installation – **Wichtig**: Wählen Sie die Optionen aus, um Miniconda zur PATH-Variablen hinzuzufügen und um Miniconda als Python-Standardumgebung zu registrieren.
3. Öffnen Sie nach der Installation die Anaconda-Eingabeaufforderung, und geben Sie die folgenden Befehle ein, um die Pakete zu installieren: 

```
pip install flask requests python-dotenv pylint matplotlib pillow
pip install --upgrade numpy
```

### Azure CLI

1. Herunterladen von https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest 
2. Installieren mithilfe der Standardoptionen

### Git

1. Herunterladen und Installieren von https://git-scm.com/download.html unter Verwendung der Standardoptionen


### Visual Studio Code (und Erweiterungen)

1. Herunterladen von https://code.visualstudio.com/Download 
2. Installieren mithilfe der Standardoptionen 
3. Starten Sie Visual Studio Code nach der Installation, suchen Sie auf der Registerkarte **Erweiterungen** (STRG+UMSCHALT+X) nach den folgenden Erweiterungen von Microsoft, und installieren Sie sie:
    - Python
    - C#
    - Azure-Funktionen
    - PowerShell
