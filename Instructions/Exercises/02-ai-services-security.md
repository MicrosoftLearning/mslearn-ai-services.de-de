---
lab:
  title: Verwalten der Sicherheit von Azure KI Services
  module: Module 2 - Developing AI Apps with Azure AI Services
---

# Verwalten der Sicherheit von Azure KI Services

Sicherheit ist ein wichtiger Aspekt für jede Anwendung, und als Entwickler sollten Sie sicherstellen, dass der Zugriff auf Ressourcen wie Azure KI Services nur denjenigen vorbehalten ist, die ihn benötigen.

Der Zugriff auf Azure KI Services wird in der Regel über Authentifizierungsschlüssel gesteuert, die bei der erstmaligen Erstellung einer Azure KI Services-Ressource generiert werden.

## Klonen des Repositorys in Visual Studio Code

Sie entwickeln Ihren Code mittels Visual Studio Code. Die Codedateien für Ihre App wurden in einem GitHub-Repository bereitgestellt.

> **Tipp**: Wenn Sie das Repository **mslearn-ai-services** kürzlich geklont haben, öffnen Sie es in Visual Studio-Code. Führen Sie andernfalls die folgenden Schritte aus, um es in Ihrer Entwicklungsumgebung zu klonen.

1. Starten Sie Visual Studio Code.
2. Öffnen Sie die Palette (UMSCHALT+STRG+P), und führen Sie einen **Git: Clone**-Befehl aus, um das Repository `https://github.com/MicrosoftLearning/mslearn-ai-services` in einen lokalen Ordner zu klonen (der Ordner ist beliebig).
3. Nachdem das Repository geklont wurde, öffnen Sie den Ordner in Visual Studio Code.
4. Warten Sie, während zusätzliche Dateien zur Unterstützung der C#-Codeprojekte im Repository installiert werden, falls nötig

    > **Hinweis**: Wenn Sie aufgefordert werden, erforderliche Ressourcen zum Erstellen und Debuggen hinzuzufügen, wählen Sie **Not now** (Jetzt nicht) aus.

5. Erweitern Sie den Ordner `Labfiles/02-ai-services-security`.

Code sowohl für C# als auch Python wurde bereitgestellt. Erweitern Sie den Ordner Ihrer bevorzugten Sprache.

## Bereitstellen einer Azure KI Services-Ressource

Wenn Sie noch keine in Ihrem Abonnement haben, müssen Sie eine **Azure KI Services**-Ressource bereitstellen.

1. Öffnen Sie das Azure-Portal unter `https://portal.azure.com`, und melden Sie sich mit dem Microsoft-Konto an, das Ihrem Azure-Abonnement zugeordnet ist.
2. Suchen Sie in der oberen Suchleiste nach *Azure KI Services*, wählen Sie **Azure KI Services Multi-Service-Konto** und erstellen Sie eine Ressource mit den folgenden Einstellungen:
    - **Abonnement:** *Geben Sie Ihr Azure-Abonnement an.*
    - **Ressourcengruppe**: *Wählen Sie eine Ressourcengruppe aus, oder erstellen Sie eine Ressourcengruppe (wenn Sie eine gehostete Lab-Umgebung verwenden, sind Sie möglicherweise nicht berechtigt, eine neue Ressourcengruppe zu erstellen, verwenden Sie dann die bereitgestellte Ressourcengruppe).*
    - **Region**: *Wählen Sie eine beliebige verfügbare Region aus*.
    - **Name**: *Geben Sie einen eindeutigen Namen ein.*
    - **Tarif**: Standard S0.
3. Aktivieren Sie die erforderlichen Kontrollkästchen, und erstellen Sie die Ressource.
4. Warten Sie, bis die Bereitstellung abgeschlossen ist, und zeigen Sie dann die Bereitstellungsdetails an.

## Verwalten von Authentifizierungsschlüsseln

Beim Erstellen Ihrer Azure KI Services-Ressource wurden zwei Authentifizierungsschlüssel generiert. Sie können diese im Azure-Portal oder über die Azure-Befehlszeilenschnittstelle (CLI) verwalten. 

1. Wählen Sie eine Methode zum Abrufen von Authentifizierungsschlüsseln und Endpunkten aus: 

    **Verwendung des Azure-Portals**: Gehen Sie im Azure-Portal zu Ihrer Azure KI-Services-Ressource und rufen Sie die Seite **Schlüssel und Endpunkt** auf. Diese Seite enthält die Informationen, die Sie benötigen, um eine Verbindung zu Ihrer Ressource herzustellen und sie aus den von Ihnen entwickelten Anwendungen zu nutzen. Dies betrifft insbesondere:
    - Ein HTTP-*Endpunkt*, an den Clientanwendungen Anforderungen senden können.
    - Zwei *Schlüssel*, die für die Authentifizierung verwendet werden können (Clientanwendungen können einen der beiden Schlüssel verwenden. Eine gängige Methode besteht darin, einen für die Entwicklung und einen anderen für die Produktion zu verwenden. Sie können den Entwicklungsschlüssel einfach neu generieren, nachdem die Entwickler ihre Arbeit beendet haben, um weiteren Zugriff zu verhindern).
    - Der *Standort*, an dem die Ressource gehostet wird. Dies ist für Anforderungen an einige (aber nicht alle) APIs erforderlich.

    **Verwendung der Befehlszeile**: Alternativ können Sie den folgenden Befehl verwenden, um die Liste der Azure KI Services-Schlüssel abzurufen. Öffnen Sie in Visual Studio Code ein neues Terminal. Fügen Sie dann den folgenden Befehl ein und ersetzen Sie *&lt;resourceName&gt;* durch den Namen Ihrer Azure KI Services-Ressource und *&lt;resourceGroup&gt;* durch den Namen der Ressourcengruppe, in der Sie sie erstellt haben.

    ```
    az cognitiveservices account keys list --name <resourceName> --resource-group <resourceGroup>
    ```

    Der Befehl gibt eine Liste der Schlüssel für Ihre Azure KI Services-Ressource zurück – es gibt zwei Schlüssel, **key1** und **key2**.

    > **Tipp**: Wenn Sie Azure CLI noch nicht authentifiziert haben, führen Sie zunächst `az login` aus und melden Sie sich bei Ihrem Konto an.

2. Um Ihren Azure KI-Dienst zu testen, können Sie **curl** verwenden – ein Befehlszeilentool für HTTP-Anfragen. Öffnen Sie im Ordner **02-ai-cognitive-security** die Datei **rest-test.cmd**, und bearbeiten Sie den darin enthaltenen Befehl **curl** (unten angezeigt). Ersetzen Sie dabei *&lt;yourEndpoint&gt;* und *&lt;yourKey&gt;* durch Ihren Endpunkt-URI und den Schlüssel **Key1**, um die API „Text analysieren“ in Ihrer Azure KI Services-Ressource zu verwenden.

    ```bash
    curl -X POST "<yourEndpoint>/language/:analyze-text?api-version=2023-04-01" -H "Content-Type: application/json" -H "Ocp-Apim-Subscription-Key: <your-key>" --data-ascii "{'analysisInput':{'documents':[{'id':1,'text':'hello'}]}, 'kind': 'LanguageDetection'}"
    ```

3. Speichern Sie die Änderungen. Navigieren Sie im Terminal zum Ordner „02-ai-services-security“. (**Hinweis**: Klicken Sie dazu mit der rechten Maustaste auf den Ordner „02-ai-services-security“ in Ihrem Explorer und wählen Sie *In integriertem Terminal öffnen* aus.) Führen Sie dann den folgenden Befehl aus:

    ```
    ./rest-test.cmd
    ```

Der Befehl gibt ein JSON-Dokument zurück, das Informationen über die in den Eingabedaten erkannte Sprache enthält (dies sollte Englisch sein).

4. Wenn ein Schlüssel kompromittiert wird oder die Entwickler, die ihn besitzen, keinen Zugriff mehr benötigen, können Sie ihn im Portal oder über die Azure CLI neu generieren. Führen Sie den folgenden Befehl aus, um Ihren Schlüssel **key1** neu zu generieren (wobei Sie *&lt;resourceName&gt;* und *&lt;resourceGroup&gt;* für Ihre Ressource ersetzen).

    ```
    az cognitiveservices account keys regenerate --name <resourceName> --resource-group <resourceGroup> --key-name key1
    ```

Sie erhalten die Liste der Schlüssel für Ihre Azure KI Services-Ressource. Beachten Sie, dass sich **key1** seit dem letzten Abruf geändert hat.

5. Führen Sie den Befehl **rest-test** mit dem alten Schlüssel erneut aus (Sie können den Pfeil **^** auf Ihrer Tastatur verwenden, um die vorherigen Befehle zu durchblättern) und überprüfen Sie, dass jetzt ein Fehler auftritt.
6. Bearbeiten Sie den Befehl *curl* in **rest-test.cmd**, indem Sie den Schlüssel durch den neuen **key1**-Wert ersetzen, und speichern Sie die Änderungen. Führen Sie dann den Befehl **rest-test** erneut aus und überprüfen Sie, ob er erfolgreich ausgeführt wurde.

> **Tipp**: In dieser Übung haben Sie die vollständigen Namen der Azure CLI-Parameter verwendet, z. B. **--resource-group**.  Sie können auch kürzere Alternativen wie **-g** verwenden, um Ihre Befehle weniger ausführlich zu gestalten (aber etwas schwieriger zu verstehen).  Die [Azure KI Services-CLI-Befehlsreferenz](https://docs.microsoft.com/cli/azure/cognitiveservices?view=azure-cli-latest) listet die Parameteroptionen für jeden Azure KI Services-CLI-Befehl auf.

## Sicherer Schlüsselzugriff mit Azure Key Vault

Sie können Anwendungen entwickeln, die Azure KI Services nutzen, indem Sie einen Schlüssel zur Authentifizierung verwenden. Das bedeutet jedoch, dass der Anwendungscode in der Lage sein muss, den Schlüssel zu erhalten. Eine Möglichkeit besteht darin, den Schlüssel in einer Umgebungsvariablen oder einer Konfigurationsdatei zu speichern, in der die Anwendung bereitgestellt wird. Bei diesem Ansatz bleibt der Schlüssel jedoch anfällig für nicht autorisierten Zugriff. Ein besserer Ansatz bei der Entwicklung von Anwendungen in Azure besteht darin, den Schlüssel sicher in Azure Key Vault zu speichern und den Zugriff auf den Schlüssel über eine *verwaltete Identität* bereitzustellen (mit anderen Worten, ein Benutzerkonto, das von der Anwendung selbst verwendet wird).

### Erstellen eines Schlüsseltresors und Hinzufügen eines Geheimnisses

Zunächst müssen Sie einen Schlüsseltresor erstellen und ein *Geheimnis* für den Azure KI Services-Schlüssel hinzufügen.

1. Notieren Sie sich den **key1**-Wert für Ihre Azure KI Services-Ressource (oder kopieren Sie ihn in die Zwischenablage).
2. Wählen Sie im Azure-Portal auf der Seite **Start** die Schaltfläche **&#65291;Ressource erstellen** aus, suchen Sie nach *Key Vault*, und erstellen Sie eine **Key Vault**-Ressource mit den folgenden Einstellungen:

    - Registerkarte **Grundlagen**
        - **Abonnement:** *Geben Sie Ihr Azure-Abonnement an.*
        - **Ressourcengruppe**: *Die gleiche Ressourcengruppe wie Ihre Azure KI Services-Ressource*
        - **Name des Schlüsseltresors**: *Geben Sie einen eindeutigen Namen ein*.
        - **Region**: *Die gleiche Region wie Ihre Azure KI Services-Ressource*
        - **Tarif**: Standard.
    
    - Registerkarte **Zugriffskonfiguration**
        -  **Berechtigungsmodell**: Tresorzugriffsrichtlinie
        - Scrollen Sie nach unten zum Abschnitt **Zugriffsrichtlinien**, und wählen Sie Ihren Benutzer/Ihre Benutzerin über das Kontrollkästchen auf der linken Seite aus. Wählen Sie dann **Überprüfen+ erstellen** und dann **Erstellen** aus, um Ihre Ressource zu erstellen.
     
3. Warten Sie, bis die Bereitstellung abgeschlossen ist, und wechseln Sie dann zu Ihrer Key Vault-Ressource.
4. Klicken Sie im linken Navigationsbereich auf **Geheimnisse** (im Abschnitt „Objekte“).
5. Wählen Sie **+ Generieren/Importieren** aus, und fügen Sie ein neues Geheimnis mit den folgenden Einstellungen hinzu:
    - **Uploadoptionen**: Manuell.
    - **Name:** AI-Services-Key *(es ist wichtig, dass dies genau übereinstimmt, da Sie später Code ausführen werden, der das Geheimnis basierend auf diesem Namen abruft)*
    - **Geheimer Wert**: *Ihr Azure KI Services-Schlüssel **key1***
6. Klicken Sie auf **Erstellen**.

### Erstellen eines Dienstprinzipals

Für den Zugriff auf das Geheimnis im Schlüsseltresor muss Ihre Anwendung einen Dienstprinzipal verwenden, der Zugriff auf das Geheimnis hat. Sie verwenden die Azure-Befehlszeilenschnittstelle (CLI), um den Dienstprinzipal zu erstellen, die entsprechende Objekt-ID zu ermitteln und den Zugriff auf das Geheimnis in Azure Key Vault zu gewähren.

1. Führen Sie den folgenden Azure CLI-Befehl aus, und ersetzen Sie *&lt;spName*&gt; durch einen eindeutigen geeigneten Namen für eine Anwendungsidentität (z. B. *ai-app* mit Ihren am Ende angefügten Initialen. Der Name muss innerhalb Ihres Mandanten eindeutig sein). Ersetzen Sie außerdem *&lt;subscriptionId&gt;* und *&lt;resourceGroup&gt;* durch die korrekten Werte für Ihre Abonnement-ID und die Ressourcengruppe, die Ihre Azure KI Services- und Key Vault-Ressourcen enthält:

    > **Tipp**: Wenn Sie sich bei Ihrer Abonnement-ID nicht sicher sind, verwenden Sie den Befehl **az account show**, um Ihre Abonnementinformationen abzurufen – die Abonnement-ID ist das **id**-Attribut in der Ausgabe. Wenn eine Fehlermeldung erscheint, dass das Objekt bereits existiert, wählen Sie bitte einen anderen eindeutigen Namen aus.

    ```
    az ad sp create-for-rbac -n "api://<spName>" --role owner --scopes subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>
    ```

Die Ausgabe dieses Befehls enthält Informationen über Ihren neuen Dienstprinzipal. Es sollte in etwa wie folgt aussehen:

    ```
    {
        "appId": "abcd12345efghi67890jklmn",
        "displayName": "api://ai-app-",
        "password": "1a2b3c4d5e6f7g8h9i0j",
        "tenant": "1234abcd5678fghi90jklm"
    }
    ```

Notieren Sie sich die Werte für **appId**, **Kennwort** und **Mandant** – Sie werden sie später benötigen (Wenn Sie dieses Terminal schließen, können Sie das Kennwort nicht mehr abrufen. Daher ist es wichtig, die Werte jetzt zu notieren. Sie können die Ausgabe in eine neue Textdatei auf Ihrem lokalen Computer einfügen, um sicherzustellen, dass Sie die benötigten Werte später wiederfinden!)

2. Führen Sie zum Abrufen der **Objekt-ID** Ihres Dienstprinzipals den folgenden Azure CLI-Befehl aus, um *&lt;appId&gt;* durch den Wert der App-ID Ihres Dienstprinzipals zu ersetzen.

    ```
    az ad sp show --id <appId>
    ```

3. Kopieren Sie den `id`-Wert in der JSON, die als Antwort zurückgegeben wird. 
3. Um Ihrem neuen Dienstprinzipal die Berechtigung für den Zugriff auf Geheimnisse in Ihrem Key Vault zu erteilen, führen Sie den folgenden Azure CLI-Befehl aus. Ersetzen Sie dabei *&lt;keyVaultName&gt;* durch den Namen Ihrer Azure Key Vault-Ressource und *&lt;objectId&gt;* durch den Wert der ID Ihres Dienstprinzipals, die Sie soeben kopiert haben.

    ```
    az keyvault set-policy -n <keyVaultName> --object-id <objectId> --secret-permissions get list
    ```

### Verwenden des Dienstprinzipals in einer Anwendung

Jetzt können Sie die Dienstprinzipalidentität in einer Anwendung verwenden, sodass diese auf den geheimen Azure KI Services-Schlüssel in Ihrem Schlüsseltresor zugreifen und ihn für die Verbindung mit Ihrer Azure KI Services-Ressource verwenden kann.

> **Hinweis**: In dieser Übung werden wir die Anmeldedaten des Dienstprinzipals in der Anwendungskonfiguration speichern und sie zur Authentifizierung einer **ClientSecretCredential**-Identität in Ihrem Anwendungscode verwenden. Für Entwicklung und Testen ist dies in Ordnung, aber in einer echten Produktionsanwendung würde ein Administrator der Anwendung eine *verwaltete Identität* zuweisen, sodass sie die Identität des Dienstprinzipals für den Zugriff auf Ressourcen verwendet, ohne das Kennwort zwischenzuspeichern oder zu speichern.

1. Wechseln Sie in Ihrem Terminal je nach Ihrer Spracheinstellung zum Ordner **C-Sharp** oder **Python**, indem Sie `cd C-Sharp` oder `cd Python` ausführen. Führen Sie dann `cd keyvault_client` aus, um zum App-Ordner zu navigieren.
2. Installieren Sie dann die Pakete, die Sie für Azure Key Vault und die Textanalyse-API in Ihrer Azure KI Services-Ressource verwenden müssen, indem Sie den entsprechenden Befehl für Ihre Spracheinstellung ausführen:

    **C#**

    ```
    dotnet add package Azure.AI.TextAnalytics --version 5.3.0
    dotnet add package Azure.Identity --version 1.12.0
    dotnet add package Azure.Security.KeyVault.Secrets --version 4.6.0
    ```

    **Python**

    ```
    pip install azure-ai-textanalytics==5.3.0
    pip install azure-identity==1.17.1
    pip install azure-keyvault-secrets==4.8.0
    ```

3. Zeigen Sie den Inhalt des Ordners **keyvault-client** an, und beachten Sie, dass er eine Datei für Konfigurationseinstellungen enthält:
    - **C#** : appsettings.json
    - **Python**: .env

    Öffnen Sie die Konfigurationsdatei, und aktualisieren Sie die darin enthaltenen Konfigurationswerte, um die folgenden Einstellungen wiederzugeben:
    
    - Der **Endpunkt** für Ihre Azure KI Services-Ressource
    - Der Name Ihrer **Azure Key Vault**-Ressource.
    - Der **Mandant** für Ihren Dienstprinzipal.
    - Die **appId** (Anwendungs-ID) für Ihren Dienstprinzipal.
    - Das **Kennwort** für Ihren Dienstprinzipal.

     Speichern Sie Ihre Änderungen, indem Sie **STRG+S** drücken.
4. Beachten Sie, dass der Ordner **keyvault-client** eine Codedatei für die Clientanwendung enthält:

    - **C#** : Program.cs
    - **Python**: keyvault-client.py

    Öffnen Sie die Codedatei, und überprüfen Sie den darin enthaltenen Code, wobei Sie sich die folgenden Details notieren:
    - Der Namespace für das von Ihnen installierte SDK wird importiert.
    - Der Code in der Funktion **Main** ruft die Konfigurationseinstellungen der Anwendung ab und verwendet dann die Anmeldeinformationen des Dienstprinzipals, um den Azure KI Services-Schlüssel aus dem Schlüsseltresor zu erhalten.
    - Die Funktion **GetLanguage** verwendet das SDK, um einen Client für den Dienst zu erstellen, und verwendet dann den Client, um die Sprache des eingegebenen Texts zu erkennen.
5. Geben Sie den folgenden Befehl ein, um das Programm auszuführen:

    **C#**

    ```
    dotnet run
    ```

    **Python**

    ```
    python keyvault-client.py
    ```

6. Wenn Sie dazu aufgefordert werden, geben Sie einen Text ein und überprüfen Sie die Sprache, die vom Dienst erkannt wird. Geben Sie beispielsweise „Hello“, „Bonjour“ und „Gracias“ ein.
7. Wenn Sie mit dem Testen der Anwendung fertig sind, geben Sie „quit“ ein, um das Programm zu beenden.

## Bereinigen von Ressourcen

Wenn Sie die in diesem Lab erstellten Azure-Ressourcen nicht für andere Trainingmodule verwenden, können Sie sie löschen, um weitere Gebühren zu vermeiden.

1. Öffnen Sie das Azure-Portal unter `https://portal.azure.com`, und suchen Sie in der oberen Suchleiste nach den Ressourcen, die Sie in diesem Lab erstellt haben.

2. Wählen Sie auf der Ressourcenseite **Löschen** aus, und folgen Sie den Anweisungen zum Löschen der Ressource. Alternativ können Sie die gesamte Ressourcengruppe löschen, um alle Ressourcen gleichzeitig zu bereinigen.

## Weitere Informationen

Weitere Informationen zur Sicherung von Azure KI Services finden Sie in der [Dokumentation zur Sicherheit von Azure KI Services](https://docs.microsoft.com/azure/ai-services/security-features).
