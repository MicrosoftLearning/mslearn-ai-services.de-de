---
lab:
  title: Erste Schritte mit Azure KI Services
  module: Module 2 - Developing AI Apps with Azure AI Services
---

# Erste Schritte mit Azure KI Services

In dieser Übung machen Sie die ersten Schritte mit Azure KI Services, indem Sie eine **Azure KI Services**-Ressource in Ihrem Azure-Abonnement erstellen und diese von einer Clientanwendung aus verwenden. Ziel der Übung ist es nicht, Fachwissen über einen bestimmten Dienst zu erwerben, sondern sich mit einem allgemeinen Verfahren für die Bereitstellung von und die Arbeit mit Azure KI-Diensten als Entwickler vertraut zu machen.

## Klonen des Repositorys in Visual Studio Code

Sie entwickeln Ihren Code mittels Visual Studio Code. Die Codedateien für Ihre App wurden in einem GitHub-Repository bereitgestellt.

> **Tipp**: Wenn Sie das **mslearn-ai-services**-Repository bereits geklont haben, öffnen Sie es in Visual Studio-Code. Führen Sie andernfalls die folgenden Schritte aus, um es in Ihrer Entwicklungsumgebung zu klonen.

1. Starten Sie Visual Studio Code.
2. Öffnen Sie die Palette (UMSCHALT+STRG+P), und führen Sie einen **Git: Clone**-Befehl aus, um das Repository `https://github.com/MicrosoftLearning/mslearn-ai-services` in einen lokalen Ordner zu klonen (der Ordner ist beliebig).
3. Nachdem das Repository geklont wurde, öffnen Sie den Ordner in Visual Studio Code.
4. Warten Sie, während zusätzliche Dateien zur Unterstützung der C#-Codeprojekte im Repository installiert werden, falls nötig

    > **Hinweis**: Wenn Sie aufgefordert werden, erforderliche Ressourcen zum Erstellen und Debuggen hinzuzufügen, wählen Sie **Not now** (Jetzt nicht) aus.

5. Erweitern Sie den Ordner `Labfiles/01-use-azure-ai-services`.

Code sowohl für C# als auch Python wurde bereitgestellt. Erweitern Sie den Ordner Ihrer bevorzugten Sprache.

## Bereitstellen einer Azure KI Services-Ressource

Azure KI Services sind cloudbasierte Dienste, die KI-Funktionen kapseln, die Sie in Ihre Anwendungen integrieren können. Sie können einzelne Azure KI Services-Ressourcen für bestimmte APIs bereitstellen (z. B. **Language** oder **Vision**). Sie können auch eine einzelne **Azure KI Services**-Ressource bereitstellen, die über einen einzelnen Endpunkt und Schlüssel Zugriff auf mehrere Azure KI Services-APIs bietet. In diesem Fall verwenden Sie eine einzelne **Azure KI Services**-Ressource.

1. Öffnen Sie das Azure-Portal unter `https://portal.azure.com`, und melden Sie sich mit dem Microsoft-Konto an, das Ihrem Azure-Abonnement zugeordnet ist.
2. Suchen Sie in der oberen Suchleiste nach *Azure KI Services*, wählen Sie **Azure KI Services** aus und erstellen Sie eine Azure KI Services Multi-Service-Kontoressource mit den folgenden Einstellungen:
    - **Abonnement:** *Geben Sie Ihr Azure-Abonnement an.*
    - **Ressourcengruppe**: *Wählen Sie eine Ressourcengruppe aus, oder erstellen Sie eine Ressourcengruppe (wenn Sie eine gehostete Lab-Umgebung verwenden, sind Sie möglicherweise nicht berechtigt, eine neue Ressourcengruppe zu erstellen, verwenden Sie dann die bereitgestellte Ressourcengruppe).*
    - **Region**: *Wählen Sie eine beliebige verfügbare Region aus*.
    - **Name**: *Geben Sie einen eindeutigen Namen ein.*
    - **Tarif**: Standard S0.
3. Aktivieren Sie die erforderlichen Kontrollkästchen, und erstellen Sie die Ressource.
4. Warten Sie, bis die Bereitstellung abgeschlossen ist, und zeigen Sie dann die Bereitstellungsdetails an.
5. Wechseln Sie zur Ressource, und zeigen Sie die Seite **Schlüssel und Endpunkt** an. Diese Seite enthält die Informationen, die Sie benötigen, um eine Verbindung zu Ihrer Ressource herzustellen und sie aus den von Ihnen entwickelten Anwendungen zu nutzen. Dies betrifft insbesondere:
    - Ein HTTP-*Endpunkt*, an den Clientanwendungen Anforderungen senden können.
    - Zwei *Schlüssel*, die für die Authentifizierung verwendet werden können (Clientanwendungen können beide Schlüssel zur Authentifizierung verwenden).
    - Der *Standort*, an dem die Ressource gehostet wird. Dies ist für Anforderungen an einige (aber nicht alle) APIs erforderlich.

## Verwenden einer REST-Schnittstelle

Die Azure KI Services-APIs sind REST-basiert, d. h. Sie können sie nutzen, indem Sie JSON-Anforderungen über HTTP übermitteln. In diesem Beispiel lernen Sie eine Konsolenanwendung kennen, die die **Language**-REST-API zur Spracherkennung verwendet. Das Grundprinzip ist jedoch für alle von der Azure KI Services-Ressource unterstützten APIs gleich.

> **Hinweis**: In dieser Übung können Sie wahlweise die REST-API von **C#** oder **Python** verwenden. Führen Sie in den folgenden Schritten die entsprechenden Aktionen für Ihre bevorzugte Sprache aus.

1. Erweitern Sie in Visual Studio Code den Ordner **C-Sharp** oder **Python** abhängig von Ihren Spracheinstellungen.
2. Zeigen Sie den Inhalt des Ordners **rest-client** an, und beachten Sie, dass er eine Datei für Konfigurationseinstellungen enthält:

    - **C#** : appsettings.json
    - **Python**: .env

    Öffnen Sie die Konfigurationsdatei und aktualisieren Sie die darin enthaltenen Konfigurationswerte, um den **Endpunkt** und einen **Schlüssel** für die Authentifizierung Ihrer Azure KI Services-Ressource hinzuzufügen. Speichern Sie die Änderungen.

3. Beachten Sie, dass der Ordner **rest-client** eine Codedatei für die Clientanwendung enthält:

    - **C#** : Program.cs
    - **Python**: rest-client.py

    Öffnen Sie die Codedatei, und überprüfen Sie den darin enthaltenen Code, wobei Sie sich die folgenden Details notieren:
    - Verschiedene Namespaces werden importiert, um die HTTP-Kommunikation zu ermöglichen.
    - Der Code in der Funktion **Main** ruft den Endpunkt und den Schlüssel für Ihre Azure KI Services-Ressource ab. Diese werden verwendet, um REST-Anforderungen an den Textanalysedienst zu senden.
    - Das Programm akzeptiert Benutzereingaben und verwendet die Funktion **GetLanguage**, um die REST-Spracherkennungs-API für Textanalyse für Ihren Azure KI-Dienstendpunkt aufzurufen und die Sprache des eingegebenen Texts zu ermitteln.
    - Die an die API gesendete Anforderung besteht aus einem JSON-Objekt, das die Eingabedaten enthält. In diesem Fall eine Sammlung von **document**-Objekten, von denen jedes eine **ID** und **Text** aufweist.
    - Der Schlüssel für Ihren Dienst ist im Anforderungsheader enthalten, um Ihre Clientanwendung zu authentifizieren.
    - Die Antwort des Diensts ist ein JSON-Objekt, das die Clientanwendung analysieren kann.

4. Klicken Sie mit der rechten Maustaste auf den Ordner **rest-client**, wählen Sie *In integriertem Terminal öffnen* aus, und führen Sie den folgenden Befehl aus:

    **C#**

    ```
    dotnet run
    ```

    **Python**

    ```
    pip install python-dotenv
    python rest-client.py
    ```

5. Wenn Sie dazu aufgefordert werden, geben Sie etwas Text ein und überprüfen Sie die Sprache, die der Dienst erkennt und die in der JSON-Antwort zurückgegeben wird. Geben Sie beispielsweise „Hello“, „Bonjour“ und „Gracias“ ein.
6. Wenn Sie mit dem Testen der Anwendung fertig sind, geben Sie „quit“ ein, um das Programm zu beenden.

## Verwenden eines SDK

Sie können Code schreiben, der die Azure KI Services-REST-APIs direkt nutzt, aber es gibt Software Development Kits (SDKs) für viele beliebte Programmiersprachen, darunter Microsoft C#, Python, Java und Node.js. Die Verwendung eines SDK kann die Entwicklung von Anwendungen, die Azure KI-Dienste nutzen, erheblich vereinfachen.

1. Erweitern Sie in Visual Studio Code den Ordner **sdk-client** unter dem Ordner **C-Sharp** oder **Python**, abhängig von Ihren Spracheinstellungen. Führen Sie dann `cd ../sdk-client` aus, um in den relevanten **sdk-Client**-Ordner zu wechseln.

2. Installieren Sie das Textanalyse-SDK-Paket, indem Sie den entsprechenden Befehl für Ihre bevorzugte Sprache ausführen:

    **C#**

    ```
    dotnet add package Azure.AI.TextAnalytics --version 5.3.0
    ```

    **Python**

    ```
    pip install azure-ai-textanalytics==5.3.0
    ```

3. Zeigen Sie den Inhalt des Ordners **sdk-client** an, und beachten Sie, dass er eine Datei für Konfigurationseinstellungen enthält:

    - **C#** : appsettings.json
    - **Python**: .env

    Öffnen Sie die Konfigurationsdatei und aktualisieren Sie die darin enthaltenen Konfigurationswerte, um den **Endpunkt** und einen **Schlüssel** für die Authentifizierung Ihrer Azure KI Services-Ressource hinzuzufügen. Speichern Sie die Änderungen.
    
4. Beachten Sie, dass der Ordner **sdk-client** eine Codedatei für die Clientanwendung enthält:

    - **C#** : Program.cs
    - **Python**: sdk-client.py

    Öffnen Sie die Codedatei, und überprüfen Sie den darin enthaltenen Code, wobei Sie sich die folgenden Details notieren:
    - Der Namespace für das von Ihnen installierte SDK wird importiert.
    - Der Code in der Funktion **Main** ruft den Endpunkt und den Schlüssel für Ihre Azure KI Services-Ressource ab. Diese werden mit dem SDK verwendet, um einen Client für den Textanalysedienst zu erstellen.
    - Die Funktion **GetLanguage** verwendet das SDK, um einen Client für den Dienst zu erstellen, und verwendet dann den Client, um die Sprache des eingegebenen Texts zu erkennen.

5. Kehren Sie zum Terminal zurück, stellen Sie sicher, dass Sie sich im Ordner **sdk-client** befinden, und geben Sie den folgenden Befehl ein, um das Programm auszuführen:

    **C#**

    ```
    dotnet run
    ```

    **Python**

    ```
    python sdk-client.py
    ```

6. Wenn Sie dazu aufgefordert werden, geben Sie einen Text ein und überprüfen Sie die Sprache, die vom Dienst erkannt wird. Geben Sie beispielsweise „Goodbye“, „Au revoir“ und „Hasta la vista“ ein.
7. Wenn Sie mit dem Testen der Anwendung fertig sind, geben Sie „quit“ ein, um das Programm zu beenden.

> **Hinweis**: Einige Sprachen, die Unicode-Zeichensätze erfordern, werden in dieser einfachen Konsolenanwendung möglicherweise nicht erkannt.

## Bereinigen von Ressourcen

Wenn Sie die in diesem Lab erstellten Azure-Ressourcen nicht für andere Trainingmodule verwenden, können Sie sie löschen, um weitere Gebühren zu vermeiden.

1. Öffnen Sie das Azure-Portal unter `https://portal.azure.com`, und suchen Sie in der oberen Suchleiste nach den Ressourcen, die Sie in diesem Lab erstellt haben.

2. Wählen Sie auf der Ressourcenseite **Löschen** aus, und folgen Sie den Anweisungen zum Löschen der Ressource. Alternativ können Sie die gesamte Ressourcengruppe löschen, um alle Ressourcen gleichzeitig zu bereinigen.

## Weitere Informationen

Weitere Informationen zu Azure-KI-Diensten finden Sie in der [Dokumentation zu Azure KI Services](https://docs.microsoft.com/azure/ai-services/what-are-ai-services).