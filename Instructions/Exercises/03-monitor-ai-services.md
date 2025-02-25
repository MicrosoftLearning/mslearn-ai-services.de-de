---
lab:
  title: "Überwachen von Azure\_KI Services"
  module: Module 2 - Developing AI Apps with Azure AI Services
---

# Überwachen von Azure KI Services

Azure KI Services können ein entscheidender Bestandteil der gesamten Anwendungsinfrastruktur sein. Es ist wichtig, dass Sie die Aktivitäten überwachen können und auf Probleme aufmerksam gemacht werden, die möglicherweise Aufmerksamkeit erfordern.

## Klonen des Repositorys in Visual Studio Code

Sie entwickeln Ihren Code mittels Visual Studio Code. Die Codedateien für Ihre App wurden in einem GitHub-Repository bereitgestellt.

> **Tipp**: Wenn Sie das **mslearn-ai-services**-Repository bereits geklont haben, öffnen Sie es in Visual Studio-Code. Führen Sie andernfalls die folgenden Schritte aus, um es in Ihrer Entwicklungsumgebung zu klonen.

1. Starten Sie Visual Studio Code.
2. Öffnen Sie die Palette (UMSCHALT+STRG+P), und führen Sie einen **Git: Clone**-Befehl aus, um das Repository `https://github.com/MicrosoftLearning/mslearn-ai-services` in einen lokalen Ordner zu klonen (der Ordner ist beliebig).
3. Nachdem das Repository geklont wurde, öffnen Sie den Ordner in Visual Studio Code.
4. Warten Sie, während zusätzliche Dateien zur Unterstützung der C#-Codeprojekte im Repository installiert werden, falls nötig

    > **Hinweis**: Wenn Sie aufgefordert werden, erforderliche Ressourcen zum Erstellen und Debuggen hinzuzufügen, wählen Sie **Not now** (Jetzt nicht) aus.

5. Erweitern Sie den Ordner `Labfiles/03-monitor-ai-services`.

## Bereitstellen einer Azure KI Services-Ressource

Wenn Sie noch keine in Ihrem Abonnement haben, müssen Sie eine **Azure KI Services**-Ressource bereitstellen.

1. Öffnen Sie das Azure-Portal unter `https://portal.azure.com`, und melden Sie sich mit dem Microsoft-Konto an, das Ihrem Azure-Abonnement zugeordnet ist.
2. Suchen Sie in der oberen Suchleiste nach *Azure KI Services*, wählen Sie **Azure KI Services Multi-Service-Konto** und erstellen Sie eine Ressource mit den folgenden Einstellungen:
    - **Abonnement:** *Geben Sie Ihr Azure-Abonnement an.*
    - **Ressourcengruppe**: *Wählen Sie eine Ressourcengruppe aus, oder erstellen Sie eine Ressourcengruppe (wenn Sie eine gehostete Lab-Umgebung verwenden, sind Sie möglicherweise nicht berechtigt, eine neue Ressourcengruppe zu erstellen, verwenden Sie dann die bereitgestellte Ressourcengruppe).*
    - **Region**: *Wählen Sie eine beliebige verfügbare Region aus*.
    - **Name**: *Geben Sie einen eindeutigen Namen ein.*
    - **Tarif**: Standard S0.
3. Aktivieren Sie alle erforderlichen Kontrollkästchen, und erstellen Sie die Ressource.
4. Warten Sie, bis die Bereitstellung abgeschlossen ist, und zeigen Sie dann die Bereitstellungsdetails an.
5. Wenn die Ressource bereitgestellt wurde, wechseln Sie zu ihr, und zeigen Sie ihre Seite **Schlüssel und Endpunkt** an. Notieren Sie sich den Endpunkt-URI. Sie benötigen ihn später.

## Konfigurieren einer Warnung

Zu Beginn der Überwachung definieren Sie eine Warnungsregel, um Aktivitäten in Ihrer Azure KI Services-Ressource zu erkennen.

1. Wechseln Sie im Azure-Portal zu Ihrer Azure KI Services-Ressource, und rufen Sie die Seite **Warnungen** (im Abschnitt **Überwachung**) auf.
2. Wählen Sie die Dropdownliste **+ Erstellen** aus, und wählen Sie **Warnungsregel** aus.
3. Vergewissern Sie sich auf der Seite **Warnungsregel erstellen** unter **Bereich**, dass Ihre Azure KI Services-Ressource aufgeführt ist. (Schließen Sie den Bereich **Signal auswählen**, wenn dieser geöffnet ist.)
4. Wählen Sie die Registerkarte **Bedingung** aus, und wählen Sie auf dem Link **Alle Signale anzeigen** aus, um den Bereich **Signal auswählen** anzuzeigen, der auf der rechten Seite erscheint, wo Sie einen zu überwachenden Signaltyp auswählen können.
5. Scrollen Sie in der Liste **Signaltyp** nach unten zum Abschnitt **Aktivitätsprotokoll**, und wählen Sie dann **Schlüssel auflisten (Cognitive Services API-Konto)** aus. Wählen Sie anschließend **Anwenden**.
6. Überprüfen Sie die Aktivität der letzten sechs Stunden.
7. Navigieren Sie zur Registerkarte **Aktionen**. Beachten Sie, dass Sie eine *Aktionsgruppe* angeben können. Damit können Sie automatische Aktionen konfigurieren, wenn eine Warnung ausgelöst wird – z. B. das Senden einer E-Mail-Benachrichtigung. In dieser Übung werden wir das nicht vornehmen, aber es kann nützlich sein, dies in einer Produktionsumgebung durchzuführen.
8. Legen Sie auf der Registerkarte **Details** die Option **Name der Warnungsregel** auf **Key List Alert** (Schlüssellistenwarnung) fest.
9. Klicken Sie auf **Überprüfen + erstellen**. 
10. Überprüfen Sie die Konfiguration für die Warnung. Klicken Sie auf **Erstellen**, und warten Sie, bis die Warnungsregel erstellt wurde.
11. Jetzt können Sie den folgenden Befehl verwenden, um die Liste der Azure KI Services-Schlüssel abzurufen. Ersetzen Sie dabei *&lt;resourceName&gt;* durch den Namen Ihrer Azure KI Services-Ressource und *&lt;resourceGroup&gt;* durch den Namen der Ressourcengruppe, in der Sie sie erstellt haben.

    ```
    az cognitiveservices account keys list --name <resourceName> --resource-group <resourceGroup>
    ```

    Der Befehl gibt eine Liste der Schlüssel für Ihre Azure KI Services-Ressource zurück.

    > **Hinweis** Wenn Sie sich nicht bei Azure CLI angemeldet haben, müssen Sie möglicherweise `az login` ausführen, bevor der Befehl „Schlüssel auflisten“ funktioniert.

12. Wechseln Sie zurück zu dem Browser, in dem das Azure-Portal geöffnet ist, und aktualisieren Sie die **Warnungsseite**. In der Tabelle sollte die Warnung **Sev 4** aufgeführt sein (andernfalls warten Sie bis zu fünf Minuten und aktualisieren Sie sie erneut).
13. Wählen Sie die Warnung aus, um ihre Details anzuzeigen.

## Visualisieren einer Metrik

Sie können nicht nur Warnungen definieren, sondern auch Metriken für Ihre Azure KI Services-Ressource anzeigen, um deren Auslastung zu überwachen.

1. Wählen Sie im Azure-Portal auf der Seite für Ihre Azure KI Services-Ressource die Option **Metriken** (im Abschnitt **Überwachung**) aus.
2. Wenn kein Diagramm vorhanden ist, wählen Sie **+ Neues Diagramm** aus. Überprüfen Sie dann in der Liste **Metrik** die möglichen Metriken, die Sie visualisieren können, und wählen Sie **Aufrufe gesamt** aus.
3. Wählen Sie in der Liste **Aggregation** die Option **Anzahl** aus.  Dadurch können Sie die Gesamtzahl der Aufrufe Ihrer Azure KI-Dienstressource überwachen, was nützlich ist, um festzustellen, wie stark der Dienst über einen bestimmten Zeitraum genutzt wird.
4. Verwenden Sie **curl** – ein Befehlszeilentool für HTTP-Anfragen – um einige Anfragen an Ihren Azure KI-Dienst zu erstellen. Öffnen Sie in Ihrem Editor **rest-test.cmd** und bearbeiten Sie den darin enthaltenen Befehl **curl** (unten angezeigt). Ersetzen Sie dabei *&lt;yourEndpoint&gt;* und *&lt;yourKey&gt;* durch Ihren Endpunkt-URI und den Schlüssel **Key1**, um die Textanalyse-API in Ihrer Azure KI Services-Ressource zu verwenden.

    ```
    curl -X POST "<your-endpoint>/language/:analyze-text?api-version=2023-04-01" -H "Content-Type: application/json" -H "Ocp-Apim-Subscription-Key: <your-key>" --data-ascii "{'analysisInput':{'documents':[{'id':1,'text':'hello'}]}, 'kind': 'LanguageDetection'}"
    ```

5. Speichern Sie Ihre Änderungen, und führen Sie dann den folgenden Befehl aus:

    ```
    ./rest-test.cmd
    ```

    Der Befehl gibt ein JSON-Dokument zurück, das Informationen über die in den Eingabedaten erkannte Sprache enthält (dies sollte Englisch sein).

6. Führen Sie den Befehl **rest-test** mehrmals aus, um eine Aufrufaktivität zu generieren (Sie können die Taste **^** verwenden, um vorherige Befehle zu durchlaufen).
7. Kehren Sie zur Seite **Metriken** im Azure-Portal zurück, und aktualisieren Sie das Diagramm **Anrufe gesamt**. Es kann ein paar Minuten dauern, bis die Aufrufe, die Sie mit *curl* durchgeführt haben, im Diagramm angezeigt werden. Aktualisieren Sie das Diagramm so lange, bis es diese Aufrufe enthält.

## Bereinigen von Ressourcen

Wenn Sie die in diesem Lab erstellten Azure-Ressourcen nicht für andere Trainingmodule verwenden, können Sie sie löschen, um weitere Gebühren zu vermeiden.

1. Öffnen Sie das Azure-Portal unter `https://portal.azure.com`, und suchen Sie in der oberen Suchleiste nach den Ressourcen, die Sie in diesem Lab erstellt haben.

2. Wählen Sie auf der Ressourcenseite **Löschen** aus, und folgen Sie den Anweisungen zum Löschen der Ressource. Alternativ können Sie die gesamte Ressourcengruppe löschen, um alle Ressourcen gleichzeitig zu bereinigen.

## Weitere Informationen

Eine der Optionen für die Überwachung von Azure KI Services ist die Verwendung der *Diagnoseprotokollierung*. Sobald die Diagnoseprotokollierung aktiviert ist, erfasst sie umfangreiche Informationen über die Auslastung Ihrer Azure KI Services-Ressource und kann ein nützliches Tool zum Überwachen und Debuggen sein. Nach dem Einrichten der Diagnoseprotokollierung kann es über eine Stunde dauern, bis Informationen generiert werden. Aus diesem Grund werden wir in dieser Übung nicht darauf eingehen. Weitere Informationen dazu finden Sie in der [Dokumentation zu Azure KI Services](https://docs.microsoft.com/azure/ai-services/diagnostic-logging).
