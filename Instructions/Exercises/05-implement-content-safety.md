---
lab:
  title: Implementieren von Azure KI Inhaltssicherheit
---

# Implementieren von Azure KI Inhaltssicherheit

In dieser Übung stellen Sie eine Inhaltssicherheits-Ressource bereit und testen die Ressource in Azure KI Studio sowie auch im Code.

## Bereitstellen einer *Inhaltssicherheits*-Ressource

Wenn noch nicht geschehen, müssen Sie eine **Inhaltssicherheits**-Ressource in Ihrem Azure-Abonnement bereitstellen.

1. Öffnen Sie das Azure-Portal unter `https://portal.azure.com`, und melden Sie sich mit dem Microsoft-Konto an, das Ihrem Azure-Abonnement zugeordnet ist.
1. Wählen Sie **Ressource erstellen**.
1. Suchen Sie im Suchfeld nach **Inhaltssicherheit**. Wählen Sie dann in den Ergebnissen **Erstellen** unter **Azure KI Inhaltssicherheit** aus.
1. Stellen Sie die Ressource mithilfe der folgenden Einstellungen bereit:
    - **Abonnement**: *Ihr Azure-Abonnement*.
    - **Ressourcengruppe**: *Wählen oder erstellen Sie eine Ressourcengruppe*.
    - **Region**: Wählen Sie **USA, Osten** aus.
    - **Name**: *Geben Sie einen eindeutigen Namen ein*.
    - **Tarif**: Wählen Sie **F0** (*Free*) oder **S** (*Standard*) aus, falls F0 nicht verfügbar ist.
1. Wählen Sie **Überprüfen und erstellen** aus und dann **Erstellen**, um die Ressource bereitzustellen.
1. Warten Sie, bis die Bereitstellung abgeschlossen ist, und wechseln Sie dann zu der Ressource.
1. Wählen Sie **Access Control** im linken Navigationsbereich und anschließend **+ Hinzufügen** und **Rollenzuweisung hinzufügen** aus.
1. Scrollen Sie nach unten, um die Rolle **Cognitive Services-Benutzer** auszuwählen, und wählen Sie **Weiter** aus.
1. Fügen Sie Ihr Konto zu dieser Rolle hinzu, und wählen Sie dann **Überprüfen und Zuweisen** aus.
1. Wählen Sie **Ressourcenmanagement** in der linken Navigationsleiste und dann **Schlüssel und Endpunkt** aus. Lassen Sie diese Seite geöffnet, damit Sie die Schlüssel später kopieren können.

## Verwenden von Prompt Shields in Azure KI Inhaltssicherheit

In dieser Übung nutzen Sie Azure KI Studio, um Inhaltssicherheits-Prompt Shields mit zwei Beispieleingaben zu testen. Die eine simuliert eine Benutzeraufforderung, und die andere simuliert ein Dokument mit eingebettetem potenziell unsicheren Text.

1. Öffnen Sie auf einer anderen Browserregisterkarte die Inhaltssicherheits-Seite von [Azure KI Studio](https://ai.azure.com/explore/contentsafety), und melden Sie sich an.
1. Wählen Sie unter **Moderater Textinhalt** die Option **Ausprobieren** aus.
1. Wählen Sie auf der Seite **Moderater Textinhalt** unter **Azure KI Services** die zuvor erstellte Inhaltssicherheitsressource aus.
1. Wählen Sie **Mehrere Risikokategorien in einem Satz** aus. Überprüfen Sie den Dokumenttext auf potenzielle Probleme.
1. Wählen Sie **Test ausführen** aus, und sehen Sie sich die Ergebnisse an.
1. Ändern Sie optional die Schwellenwerte, und wählen Sie erneut **Test ausführen** aus.
1. Wählen Sie in der linken Navigationsleiste **Geschützte Materialerkennung für Text** aus.
1. Wählen Sie **Geschützte Songtexte** aus, und beachten Sie, dass es sich hierbei um die Texte eines veröffentlichten Liedes handelt.
1. Wählen Sie **Test ausführen** aus, und sehen Sie sich die Ergebnisse an.
1. Wählen Sie in der linken Navigationsleiste **Bildinhalt moderieren** aus.
1. Wählen Sie **Selbstverletzungsinhalte** aus.
1. Beachten Sie, dass alle Bilder in KI Studio standardmäßig verschwommen sind. Beachten Sie auch, dass der sexuelle Inhalt in den Beispielen sehr mild ist.
1. Wählen Sie **Test ausführen** aus, und sehen Sie sich die Ergebnisse an.
1. Wählen Sie in der linken Navigationsleiste **Prompt Shields** aus.
1. Wählen Sie auf der **Prompt Shields-Seite** unter **Azure KI Services** die zuvor erstellte Inhaltssicherheitsressource aus.
1. Wählen Sie **Eingabeaufforderungs- und Dokumentangriffsinhalt** aus. Überprüfen Sie den Benutzeraufforderungs- und Dokumenttext auf mögliche Probleme.
1. Klicken Sie auf **Test ausführen**.
1. Überprüfen Sie unter **Anzeigen der Ergebnisse**, ob Jailbreak-Angriffe sowohl in der Benutzeraufforderung als auch im Dokument erkannt wurden.

    > [!TIP]
    > Für alle Beispiele in KI Studio ist Code verfügbar.

1. Wählen Sie unter **Nächste Schritte** unter **Anzeigen des Codes** die Option **Code anzeigen** aus. Das Fenster **Beispielcode** wird angezeigt.
1. Wählen Sie mithilfe des Abwärtspfeils entweder Python oder C# und dann **Kopieren** aus, um den Beispielcode in die Zwischenablage zu kopieren.
1. Schließen Sie den Bildschirm **Beispielcode**.

### Konfigurieren der Anwendung

Sie erstellen nun eine Anwendung in C# oder Python.

#### C#

##### Voraussetzungen

* [Visual Studio Code](https://code.visualstudio.com/) auf einer der [unterstützten Plattformen](https://code.visualstudio.com/docs/supporting/requirements#_platforms)
* [.NET 8](https://dotnet.microsoft.com/en-us/download/dotnet/8.0) ist das Zielframework für diese Übung.
* [C#-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp) für Visual Studio Code

##### Einrichten

Führen Sie die folgenden Schritte aus, um Visual Studio Code für die Übung vorzubereiten.

1. Starten Sie Visual Studio Code, klicken Sie in der Explorer-Ansicht auf **.NET-Projekt erstellen**, und wählen Sie **Konsolen-App** aus.
1. Wählen Sie auf Ihrem Computer einen Ordner aus, und geben Sie dem Projekt einen Namen. Wählen Sie **Projekt erstellen** aus, und bestätigen Sie die Warnmeldung.
1. Erweitern Sie im Explorer-Bereich den Projektmappen-Explorer, und wählen Sie **Program.cs** aus.
1. Erstellen Sie das Projekt, und führen Sie es aus, indem Sie **Ausführen** -> **Ohne Debuggen ausführen** auswählen. 
1. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das C#-Projekt, und wählen Sie **NuGet-Paket hinzufügen** aus.
1. Suchen Sie nach **Azure.AI.TextAnalytics**, und wählen Sie die neueste Version aus.
1. Suchen Sie nach einem zweiten NuGet-Paket: **Microsoft.Extensions.Configuration.Json 8.0.0**. Die Projektdatei sollte jetzt zwei NuGet-Pakete auflisten.

##### Code hinzufügen

1. Fügen Sie den zuvor kopierten Beispielcode unter dem Abschnitt **ItemGroup** ein.
1. Scrollen Sie nach unten zu *Ersetzen durch Ihr eigenes Abonnement _Schlüssel und Endpunkt*.
1. Kopieren Sie im Azure-Portal auf der Seite „Schlüssel und Endpunkt“ einen der Schlüssel (1 oder 2). Ersetzen Sie **<your_subscription_key>** durch diesen Wert.
1. Kopieren Sie im Azure-Portal auf der Seite „Schlüssel und Endpunkt“ den Endpunkt. Fügen Sie diesen Wert in Ihren Code ein, um **<your_endpoint_value>** zu ersetzen.
1. Kopieren Sie in **Azure KI Studio** den **Benutzeraufforderungs**-Wert. Fügen Sie dies in Ihren Code ein, um **<test_user_prompt>** zu ersetzen.
1. Scrollen Sie zu **<this_is_a_document_source>** nach unten, und löschen Sie diese Codezeile.
1. Kopieren Sie in **Azure KI Studio** den **Dokument**-Wert.
1. Scrollen Sie nach unten zu **<this_is_another_document_source>**, und fügen Sie Ihren Dokumentwert ein.
1. Wählen Sie **Ausführen** -> **Ausführen ohne Debuggen** aus, und überprüfen Sie, ob ein Angriff erkannt wurde. 

#### Python

##### Voraussetzungen

* [Visual Studio Code](https://code.visualstudio.com/) auf einer der [unterstützten Plattformen](https://code.visualstudio.com/docs/supporting/requirements#_platforms)

* Die [Python-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-python.python) für Visual Studio Code ist installiert.

* Das [Anforderungsmodul](https://pypi.org/project/requests/) ist installiert.

1. Erstellen Sie eine neue Python-Datei mit einer **.py**-Erweiterung, und geben Sie ihr einen geeigneten Namen.
1. Fügen Sie den Beispielcode ein, den Sie zuvor kopiert haben.
1. Scrollen Sie nach unten zum Abschnitt *Ersetzen durch Ihr eigenes Abonnement _Schlüssel und Endpunkt*.
1. Kopieren Sie im Azure-Portal auf der Seite „Schlüssel und Endpunkt“ einen der Schlüssel (1 oder 2). Ersetzen Sie **<your_subscription_key>** durch diesen Wert.
1. Kopieren Sie im Azure-Portal auf der Seite „Schlüssel und Endpunkt“ den Endpunkt. Fügen Sie diesen Wert in Ihren Code ein, um **<your_endpoint_value>** zu ersetzen.
1. Kopieren Sie in **Azure KI Studio** den **Benutzeraufforderungs**-Wert. Fügen Sie dies in Ihren Code ein, um **<test_user_prompt>** zu ersetzen.
1. Scrollen Sie zu **<this_is_a_document_source>** nach unten, und löschen Sie diese Codezeile.
1. Kopieren Sie in **Azure KI Studio** den **Dokument**-Wert.
1. Scrollen Sie nach unten zu **<this_is_another_document_source>**, und fügen Sie Ihren Dokumentwert ein.
1. Führen Sie aus dem integrierten Terminal für Ihre Datei das Programm aus, z. B.:

    - `.\prompt-shield.py`

1. Überprüfen Sie, ob ein Angriff erkannt wird.
1. Optional können Sie mit verschiedenen Testinhalten und Dokumentwerten experimentieren.
