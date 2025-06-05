---
lab:
  title: Verwenden von Azure KI Services-Container
  module: Module 2 - Developing AI Apps with Azure AI Services
---

# Verwenden von Azure KI Services-Container

Die Verwendung von in Azure gehosteten Azure KI Services ermöglicht es Anwendungsentwicklern, sich auf die Infrastruktur für ihren eigenen Code zu konzentrieren, während sie von skalierbaren Diensten profitieren, die von Microsoft verwaltet werden. In vielen Szenarien benötigen Organisationen jedoch mehr Kontrolle über ihre Dienstinfrastruktur und die Daten, die zwischen Diensten übergeben werden.

Viele der Azure KI Services-APIs können in einem *Container* verpackt und bereitgestellt werden, sodass Organisationen die Azure KI Services in ihrer eigenen Infrastruktur hosten können, z. B. in lokalen Docker-Servern, Azure Container-Instanzen oder Azure Kubernetes Services-Clustern. Containerisierte Azure KI-Dienste müssen mit einem Azure-basierten Azure KI Services-Konto kommunizieren, um die Abrechnung zu unterstützen. Die Anwendungsdaten werden jedoch nicht an den Back-End-Dienst weitergegeben, und Organisationen haben eine größere Kontrolle über die Bereitstellungskonfiguration ihrer Container, sodass benutzerdefinierte Lösungen für Authentifizierung, Skalierbarkeit und andere Überlegungen möglich sind.

> **Hinweis:** Es wird derzeit ein Problem untersucht, das verursacht, dass Container bei einigen Benutzer*innen nicht ordnungsgemäß bereitgestellt werden und Aufrufe dieser Container fehlschlagen. Updates zu diesem Lab erfolgen, sobald das Problem behoben wurde.

## Klonen des Repositorys in Visual Studio Code

Sie entwickeln Ihren Code mittels Visual Studio Code. Die Codedateien für Ihre App wurden in einem GitHub-Repository bereitgestellt.

> **Tipp**: Wenn Sie das **mslearn-ai-services**-Repository bereits geklont haben, öffnen Sie es in Visual Studio-Code. Führen Sie andernfalls die folgenden Schritte aus, um es in Ihrer Entwicklungsumgebung zu klonen.

1. Starten Sie Visual Studio Code.
2. Öffnen Sie die Palette (UMSCHALT+STRG+P), und führen Sie einen **Git: Clone**-Befehl aus, um das Repository `https://github.com/MicrosoftLearning/mslearn-ai-services` in einen lokalen Ordner zu klonen (der Ordner ist beliebig).
3. Nachdem das Repository geklont wurde, öffnen Sie den Ordner in Visual Studio Code.
4. Warten Sie, während zusätzliche Dateien zur Unterstützung der C#-Codeprojekte im Repository installiert werden, falls nötig

    > **Hinweis**: Wenn Sie aufgefordert werden, erforderliche Ressourcen zum Erstellen und Debuggen hinzuzufügen, wählen Sie **Not now** (Jetzt nicht) aus.

5. Erweitern Sie den Ordner `Labfiles/04-use-a-container`.

## Bereitstellen einer Azure KI Services-Ressource

Wenn Sie noch keine in Ihrem Abonnement haben, müssen Sie eine **Azure KI Services**-Ressource bereitstellen.

1. Öffnen Sie das Azure-Portal unter `https://portal.azure.com`, und melden Sie sich mit dem Microsoft-Konto an, das Ihrem Azure-Abonnement zugeordnet ist.
2. Suchen Sie in der oberen Suchleiste nach *Azure KI Services*, wählen Sie **Azure KI Services** aus und erstellen Sie eine Azure KI Services Multi-Service-Kontoressource mit den folgenden Einstellungen:
    - **Abonnement:** *Geben Sie Ihr Azure-Abonnement an.*
    - **Ressourcengruppe**: *Wählen Sie eine Ressourcengruppe aus, oder erstellen Sie eine Ressourcengruppe (wenn Sie eine gehostete Lab-Umgebung verwenden, sind Sie möglicherweise nicht berechtigt, eine neue Ressourcengruppe zu erstellen, verwenden Sie dann die bereitgestellte Ressourcengruppe).*
    - **Region**: *Wählen Sie eine beliebige verfügbare Region aus*.
    - **Name**: *Geben Sie einen eindeutigen Namen ein.*
    - **Tarif**: Standard S0.
3. Aktivieren Sie die erforderlichen Kontrollkästchen, und erstellen Sie die Ressource.
4. Warten Sie, bis die Bereitstellung abgeschlossen ist, und zeigen Sie dann die Bereitstellungsdetails an.
5. Wenn die Ressource bereitgestellt wurde, wechseln Sie zu ihr, und zeigen Sie ihre Seite **Schlüssel und Endpunkt** an. Sie benötigen den Endpunkt und einen der Schlüssel von dieser Seite im nächsten Verfahren.

## Bereitstellen und Ausführen eines Stimmungsanalyse-Containers

Viele häufig verwendete Azure KI Services-APIs sind in Containerimages verfügbar. Eine vollständige Liste finden Sie in der [Dokumentation zu Azure KI Services](https://learn.microsoft.com/en-us/azure/ai-services/cognitive-services-container-support#containers-in-azure-ai-services). In dieser Übung verwenden Sie das Containerimage für die *Stimmungsanalyse*-API der Textanalyse, aber die Prinzipien sind für alle verfügbaren Images identisch.

1. Wählen Sie im Azure-Portal auf der Seite **Start** die Schaltfläche **&#65291;Ressource erstellen** aus, suchen Sie nach *Container Instances*, und erstellen Sie eine **Container Instances**-Ressource mit den folgenden Einstellungen:

    - **Grundlagen**:
        - **Abonnement:** *Geben Sie Ihr Azure-Abonnement an.*
        - **Ressourcengruppe**: *Wählen Sie die Ressourcengruppe aus, die Ihre Azure KI Services-Ressource enthält.*
        - **Containername**: *Geben Sie einen eindeutigen Namen ein.*
        - **Region**: *Wählen Sie eine beliebige verfügbare Region aus*.
        - **Verfügbarkeitszonen:** Keine
        - **SKU**: Standard
        - **Imagequelle:** Andere Registrierung
        - **Imagetyp**: Öffentlich
        - **Image**: `mcr.microsoft.com/azure-cognitive-services/textanalytics/sentiment:latest`
        - **Betriebssystemtyp**: Linux
        - **Größe**: 1 vCPU, 8 GB Arbeitsspeicher
    - **Netzwerk**:
        - **Netzwerktyp**: Öffentlich
        - **DNS-Namensbezeichnung**: *Geben Sie einen eindeutigen Namen für den Containerendpunkt ein.*
        - **Ports**: *Ändern Sie den TCP-Port von 80 in **5000**.*
    - **Erweitert**:
        - **Neustartrichtlinie**: Bei Ausfall
        - **Environment variables** (Umgebungsvariablen):

            | Als sicher markieren | Schlüssel | Wert |
            | -------------- | --- | ----- |
            | Ja | `ApiKey` | *Jeder der Schlüssel für Ihre Azure KI Services-Ressource* |
            | Ja | `Billing` | *Der Endpunkt-URI für Ihre Azure KI Services-Ressource* |
            | Nein | `Eula` | `accept` |

        - **Außerkraftsetzung von Befehl**: [ ]
        - **Schlüsselverwaltung**: Von Microsoft verwaltete Schlüssel (MMK)
    - **Tags**:
        - *Fügen Sie keine Tags hinzu.*

2. Klicken Sie auf **Überprüfen + erstellen** und dann auf **Erstellen**. Warten Sie, bis die Bereitstellung abgeschlossen ist, und wechseln Sie dann zur bereitgestellten Ressource.
    > **Hinweis** Bitte beachten Sie, dass die Bereitstellung eines Azure KI-Containers auf Azure Container Instances in der Regel 5-10 Minuten dauert (Bereitstellung), bevor diese einsatzbereit sind.
3. Beachten Sie die folgenden Eigenschaften Ihrer Containerinstanzressource auf ihrer Seite **Übersicht**:
    - **Status**: Dieser sollte *Wird ausgeführt* lauten.
    - **IP-Adresse**: Dies ist die öffentliche IP-Adresse, die Sie für den Zugriff auf Ihre Containerinstanzen verwenden können.
    - **FQDN**: Dies ist der *vollqualifizierte Domänenname* der Container Instances-Ressource. Sie können diesen anstelle der IP-Adresse verwenden, um auf die Containerinstanzen zuzugreifen.

    > **Hinweis**: In dieser Übung haben Sie das Azure KI Services-Containerimage für die Stimmungsanalyse in einer Azure Container Instances (ACI)-Ressource bereitgestellt. Sie können einen ähnlichen Ansatz verwenden, um es auf einem *[Docker](https://www.docker.com/products/docker-desktop)*-Host auf Ihrem eigenen Computer oder im eigenen Netzwerk bereitzustellen, indem Sie den folgenden Befehl (in einer einzigen Zeile) ausführen, um den Stimmungsanalyse-Container in Ihrer lokalen Docker-Instanz bereitzustellen. Ersetzen Sie dabei *&lt;yourEndpoint&gt;* und *&lt;yourKey&gt;* durch Ihren Endpunkt-URI und einen der Schlüssel für Ihre Azure KI Services-Ressource.
    > Der Befehl sucht nach dem Image auf Ihrem lokalen Computer, und wenn er ihn dort nicht findet, wird er aus der Imageregistrierung *mcr.microsoft.com* abgerufen und in Ihrer Docker-Instanz bereitgestellt. Nach Abschluss der Bereitstellung startet der Container und lauscht auf eingehende Anforderungen an Port 5000.

    ```
    docker run --rm -it -p 5000:5000 --memory 8g --cpus 1 mcr.microsoft.com/azure-cognitive-services/textanalytics/sentiment:latest Eula=accept Billing=<yourEndpoint> ApiKey=<yourKey>
    ```

## Verwenden des Containers

1. Öffnen Sie in Ihrem Editor **rest-test.cmd**, und bearbeiten Sie den darin enthaltenen Befehl **curl** (unten angezeigt). Ersetzen Sie dabei *&lt;your_ACI_IP_address_or_FQDN&gt;* durch die IP-Adresse oder den FQDN für Ihren Container.

    ```
    curl -X POST "http://<your_ACI_IP_address_or_FQDN>:5000/text/analytics/v3.1/sentiment" -H "Content-Type: application/json" --data-ascii "{'documents':[{'id':1,'text':'The performance was amazing! The sound could have been clearer.'},{'id':2,'text':'The food and service were unacceptable. While the host was nice, the waiter was rude and food was cold.'}]}"
    ```

2. Speichern Sie Ihre Änderungen im Skript, indem Sie **STRG+S** drücken. Beachten Sie, dass Sie weder den Endpunkt noch den Schlüssel von Azure KI Services angeben müssen. Die Anforderung wird vom containerisierten Dienst verarbeitet. Der Container kommuniziert wiederum regelmäßig mit dem Dienst in Azure, um die Nutzung für die Abrechnung zu melden, sendet aber keine Anforderungsdaten.
3. Geben Sie den folgenden Befehl ein, um das Skript auszuführen:

    ```
    ./rest-test.cmd
    ```

4. Überprüfen Sie, ob der Befehl ein JSON-Dokument mit Informationen über die in den beiden Eingabedokumenten erkannte Stimmung zurückgibt (die positiv und negativ sein sollte, in dieser Reihenfolge).

## Problembehandlung

> **Hinweis:**  
> Wenn Ihr Container im Azure-Portal als „Ausgeführt“ angezeigt wird, bedeutet dies **nicht zwangsläufig**, dass er bereit ist, Anforderungen zu verarbeiten. Beim Versuch `curl` zu verwenden, erhalten Sie möglicherweise Fehlermeldungen, wie die folgenden:
>
> ```text
> curl: (52) Empty reply from server
> curl: (56) Recv failure: Connection reset by peer
> ```
>
> Dies kann bedeuten, dass der Container nicht mit den richtigen Parametern oder der richtigen Umgebung gestartet wurde oder dass Modelldateien oder der Internetzugang für den Modell-Download fehlen.

### Alternative: Bereitstellen des Containers mithilfe der Azure-CLI

Sie können den folgenden Befehl ausprobieren (ersetzen Sie die Endpunkte `rg-name`, `container-name`, `ApiKey` und `Billing` durch Ihre Werte):

```bash
az container create \
  --resource-group <rg-name> \
  --name <container-name> \
  --image mcr.microsoft.com/azure-cognitive-services/sentiment \
  --ports 5000 \
  --dns-name-label ai102sentimentdemo \
  --environment-variables Eula=accept \
  --secure-environment-variables ApiKey=<your_api_key> Billing=<your_billing_endpoint> \
  --cpu 2 --memory 4 \
  --os-type Linux
```
Nach der Bereitstellung können Sie den Endpunkt testen (ersetzen Sie <ACI_IP> entsprechend):
```bash
curl -X POST "http://<ACI_IP>:5000/text/analytics/v3.0/sentiment?model-version=latest" \
-H "Content-Type: application/json" \
-d '{
  "documents": [
    {
      "id": "1-en",
      "language": "en",
      "text": "The performance was amazing! The sound could have been clearer."
    },
    {
      "id": "2-en",
      "language": "en",
      "text": "The food and service were unacceptable. While the host was nice, the waiter was rude and food was cold."
    }
  ]
}'
```
**Probieren Sie es aus**:

    If v3.1 endpoints do not work, try using v3.0 or v3.1-preview.1 with above cmd.

    Always check the /status endpoint and container logs for model loading messages (look for: Model loaded from /input/TextAnalytics/v3.x/Sentiment).

    If you still encounter issues, running the container locally with Docker may help to diagnose local vs. cloud issues.

## Bereinigen

Wenn Sie die Experimente mit Ihrer Containerinstanz abgeschlossen haben, sollten Sie sie löschen.

1. Öffnen Sie im Azure-Portal die Ressourcengruppe, in der Sie Ihre Ressourcen für diese Übung erstellt haben.
2. Wählen Sie die Containerinstanzressource aus, und löschen Sie sie.

## Bereinigen von Ressourcen

Wenn Sie die in diesem Lab erstellten Azure-Ressourcen nicht für andere Trainingmodule verwenden, können Sie sie löschen, um weitere Gebühren zu vermeiden.

1. Öffnen Sie das Azure-Portal unter `https://portal.azure.com`, und suchen Sie in der oberen Suchleiste nach den Ressourcen, die Sie in diesem Lab erstellt haben.

2. Wählen Sie auf der Ressourcenseite **Löschen** aus, und folgen Sie den Anweisungen zum Löschen der Ressource. Alternativ können Sie die gesamte Ressourcengruppe löschen, um alle Ressourcen gleichzeitig zu bereinigen.

## Weitere Informationen

Weitere Informationen zur Containerisierung von Azure KI Services finden Sie in der [Dokumentation zu Azure KI Services-Containern](https://learn.microsoft.com/azure/ai-services/cognitive-services-container-support).
