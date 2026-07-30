---
title: Automatische Erstellung der ersten LLM-App
description: Erstellen Sie auf Ihrer Website eine Adobe-LLM-App, überprüfen Sie die generierten Aktionen, stellen Sie sie bereit und testen Sie sie in einer unterstützten LLM-Plattform wie ChatGPT.
source-git-commit: f91bb73a39cc5aacf44979ee55dd0ab5f69d4c81
workflow-type: tm+mt
source-wordcount: '1272'
ht-degree: 0%

---


# Erste App automatisch erstellen {#create-first-app}

>[!IMPORTANT]
>
>[!DNL Adobe LLM Apps] befindet sich derzeit in Beta.
>
>Die hier gezeigten Funktionen, Workflows und Benutzeroberflächen stellen nicht unbedingt den endgültigen Status des Produkts dar. Um Beta beizutreten, senden Sie eine E-Mail an llm-apps-beta@adobe.com.

Die Plattform macht Ihre Website zu einer voll funktionsfähigen App. Es schlägt Aktionen vor, schreibt Handler-Code und Tests, erstellt EDS-Widgets und sendet die generierten Dateien an zwei [!DNL GitHub]-Repositorys, die Sie besitzen.

Die Generierung dauert ca. 15 Minuten. Am Ende dieses Tutorials verfügen Sie über eine bereitgestellte App, die Sie in einer unterstützten LLM-Plattform wie [!DNL ChatGPT] testen können.

**Journey:** Bestätigen Sie die Anforderungen, → zwei Repositorys → erstellen Sie die App → überprüfen Sie die generierten Aktionen, → Sie sie für das Staging bereitstellen → das Plug-in testen, → Produktionssysteme anzuschließen.

## Bevor Sie beginnen

Schließen Sie alle [LLM Apps-Anforderungen](/help/overview/overview.md#requirements) ab, bevor Sie mit diesem Tutorial beginnen.

In diesem Tutorial wird eine LLM-App für [Frescopa Coffee](https://frescopa.coffee/) erstellt.

## Zwei leere Repositorys erstellen

Die Plattform benötigt zwei leere Repositorys. Erstellen Sie beide unter demselben [!DNL GitHub]-Konto oder derselben Organisation:

- **Handler-Repository** - speichert Aktions-Handler und Tests. Beispiel: `my-brand-llm-app`.
- **EDS-Repository** - speichert generierte Widget-Blöcke und -Stile. Beispiel: `my-brand-llm-app-eds`.

Navigieren Sie zu [github.com/new](https://github.com/new) für jedes Repository.

Initialisieren Sie keines der Repositorys mit einer README-, `.gitignore`- oder Lizenzdatei. Die Plattform bereitet die erforderliche Projektstruktur vor.

>[!TIP]
>
>Verwenden Sie Repository-Namen, die die App und den Zweck jedes Repositorys identifizieren. Dadurch sind sie im Dialogfeld zur App-Erstellung leichter zu erkennen.

## App starten

1. Öffnen Sie [Adobe LLM-](https://experience.adobe.com/#/@llmapps/llm-apps/) und wählen Sie **[!UICONTROL App erstellen]** aus.
2. Geben Sie den **[!UICONTROL LLM-App-]** und eine optionale Beschreibung ein.
3. Wählen Sie die **[!UICONTROL Analytics-Region]** aus.

   >[!IMPORTANT]
   >
   >Die Analytics-Region kann nach der Erstellung der App nicht mehr geändert werden.

4. Wählen **[!UICONTROL unter „Meine App]**&quot; die Option **[!UICONTROL Meine App automatisch erstellen]** aus.
5. Geben **[!UICONTROL in „Ihre]**&quot; Ihre Website-URL einschließlich des `https://` ein. Die Plattform analysiert diese Website, um nützliche Aktionen und repräsentative Beispielergebnisse zu ermitteln.

![LLM-App erstellen — App-Details und „Meine App erstellen“ aktiviert](/help/assets/guide-onboarding-agent/app-details-onboarding.png)

## [!DNL LLM Apps] Zugriff auf die Repositorys gewähren

Die Adobe LLM Apps [!DNL GitHub] App bietet [!DNL LLM Apps] Zugriff auf die von Ihnen ausgewählten Repositorys.

>[!NOTE]
>
>Die Verbindung einer [!DNL GitHub] ist ein einmaliges Setup. Wenn die Organisation bereits im Dialogfeld angezeigt wird, verwenden Sie **[!UICONTROL Repos auf GitHub verwalten]** anstatt sie erneut zu verbinden.

### Verbundene Organisation

Wenn die Adobe LLM Apps [!DNL GitHub] App bereits vor der Erstellung der Repositorys installiert war:

1. Die verbundene Organisation auswählen.
2. Wählen Sie **[!UICONTROL Repositorys auf GitHub verwalten]** aus.
3. Fügen Sie die beiden Repositorys zur vorhandenen [!DNL GitHub] App-Installation hinzu.
4. Kehren Sie zu [!DNL LLM Apps] zurück und aktualisieren Sie die Repository-Listen.

### Nur erstmalige Verbindung

Wenn die Organisation nicht im Dialogfeld angezeigt wird:

1. Wählen Sie **[!UICONTROL GitHub-Organisation verbinden]** aus.
2. Installieren Sie die Adobe LLM Apps [!DNL GitHub] App.
3. Wählen Sie **[!UICONTROL Nur Repositorys]** und wählen Sie die beiden Repositorys aus.
4. Kehren Sie zum Dialogfeld LLM-App erstellen zurück.

Wenn Sie die [!DNL GitHub] App nicht installieren oder aktualisieren können, wenden Sie sich an einen Organisationsadministrator.

## Repositorys auswählen

1. Wählen **[!UICONTROL unter „Textbausteinrepository]** die Organisation und das leere Handler-Repository aus.
2. Wählen Sie **[!UICONTROL EDS-Repository]** die Organisation und das leere EDS-Repository aus.

   ![Meine App erstellen — Wählen Sie die GitHub-Organisation, das Textbausteinrepository und das EDS-Repository aus](/help/assets/guide-onboarding-agent/repos-selected.png)

3. Aktivieren **[!UICONTROL unter &quot;]**&quot; die Option **[!UICONTROL Ich akzeptiere die Adobe Developer-]**.
4. Wählen Sie **[!UICONTROL App erstellen]** aus.

## EDS-Einrichtung abschließen

Wenn das ausgewählte EDS-Repository leer ist, wird es von [!DNL LLM Apps] mit dem AEM-Textbaustein initialisiert. Im Dialogfeld werden Sie dann aufgefordert, AEM Code Sync zu installieren, bevor Sie versuchen, die App erneut zu erstellen.

1. Wählen Sie in der Meldung unter dem EDS-Repository die Option **[!UICONTROL AEM Code Sync installieren]** aus.
2. Installieren Sie auf [!DNL GitHub] AEM Code Sync und gewähren Sie ihm Zugriff auf das EDS-Repository.

   Wählen Sie auf der **** AEM-Code-Synchronisierung registriert **[!UICONTROL unter „Site-]**&quot; die Option **[!UICONTROL + Benutzer hinzufügen]** und fügen Sie die E-Mail-Adresse hinzu, mit der Sie sich bei [!DNL LLM Apps] mit der Rolle **[!UICONTROL admin]** anmelden. Wählen Sie dann **[!UICONTROL Setup beenden]** unten auf der Seite aus.

   ![AEM Code Sync registriert — Fügen Sie sich als Site-Benutzer mit der Administratorrolle hinzu](/help/assets/guide-onboarding-agent/aem-code-sync-site-users-admin.png)

3. Kehren Sie zum Dialogfeld LLM-App erstellen zurück.

![LLM-App erstellen — Leeres EDS-Repository initialisiert und AEM-Code-Synchronisierung erforderlich](/help/assets/guide-onboarding-agent/install-aem-code-sync.png)

Sie müssen Administrator für die EDS-Site sein. Wenn im Dialogfeld gemeldet wird, dass Sie kein Administrator sind:

![LLM-App erstellen — EDS-Administratorzugriff erforderlich](/help/assets/guide-onboarding-agent/eds-admin-required.png)

1. Wählen Sie **[!UICONTROL AEM Live Admin öffnen]**.
2. Fügen Sie sich als Administrator für die EDS-Website hinzu, indem Sie auf die Schaltfläche **[!UICONTROL + Benutzer hinzufügen]**.

   ![LLM-App erstellen — Als EDS-Admin hinzufügen](/help/assets/guide-onboarding-agent/add-eds-admin.png)

3. Kehren Sie zu [!DNL LLM Apps] zurück, aktualisieren Sie das EDS-Repository und wählen Sie erneut **[!UICONTROL App erstellen]** aus.

Nachdem die Repository- und Administrator-Prüfungen bestanden wurden, erstellt [!DNL LLM Apps] die App und beginnt mit der Erstellung von Aktionen.

## Auf die Erstellung von Aktionen warten

Gehen Sie **[!UICONTROL Seite]** Aktionen“ von links. Die Seite Aktionen zeigt &quot;**für Ihr Gesprächserlebnis“ an** während der Agent die Website analysiert und die App generiert. Die Generierung dauert in der Regel etwa 15 Minuten. Sie können diese Seite verlassen und später zurückkehren.

![Aktionen — Empfehlungen werden generiert](/help/assets/guide-onboarding-agent/actions-generating.png)

Während der Generierung [!DNL LLM Apps]:

1. Analysiert die Website und identifiziert nützliche Kundenabsichten.
2. Erstellt Aktionsmetadaten, einschließlich Beschreibungen und Eingabeparametern.
3. Generiert einen Handler und testet für jede Aktion im Handler-Repository.
4. Erzeugt ein EDS-Widget für jede Aktion im EDS-Repository.
5. Bereitet die Aktionen zur Überprüfung vor.

Die generierten Handler verwenden zunächst Beispieldaten, die von der Website abgeleitet wurden. Sie demonstrieren das gesamte Erlebnis, stellen jedoch keine Verbindung zu Ihren Produktionssystemen her.

## Überprüfen der generierten Aktionen

Nach Abschluss der Generierung zeigt die Seite Aktionen die generierten Aktionen und Widget-Vorschauen an. Jede Aktion hat eine **[!UICONTROL KI-generierte Aktion, muss überprüft]**.

![Aktionen - generierte Aktionen, die zur Überprüfung bereit sind](/help/assets/guide-onboarding-agent/actions-ready-for-review.png)

Für jede Aktion:

1. Wählen Sie **[!UICONTROL Überprüfen]** aus.
2. Überprüfen Sie den Namen, die Beschreibung, die Parameter, die Anmerkungen, den generierten Handler und das Widget.
3. Wählen Sie **[!UICONTROL Als geprüft markieren]** aus. Dadurch werden die generierten Pull-Anforderungen zusammengeführt.
4. Kehren Sie zur Seite Aktionen zurück und wiederholen Sie den Vorgang für die verbleibenden Aktionen.

![Erzeugte Aktion - bereit, als geprüft zu markieren](/help/assets/guide-onboarding-agent/generated-action-review.png)

Wenn alle Aktionen überprüft wurden, wählen Sie **[!UICONTROL Zur App-Seite wechseln]** aus.

![Aktionen - alle generierten Aktionen überprüft](/help/assets/guide-onboarding-agent/actions-reviewed.png)

>[!NOTE]
>
>Generierter Code ist ein Ausgangspunkt, dessen Besitzer Sie sind. Sie können Aktionsmetadaten, Handler, Tests, Widget-JavaScript und Widget-Stile nach der Überprüfung ändern.

## Bereitstellen der App

1. Kehren Sie zur App-Detailseite zurück.
2. Wählen Sie **[!UICONTROL Bereitstellen]** aus.
3. Wählen Sie **[!UICONTROL Staging]** als Zielumgebung aus.
4. Wählen Sie **[!UICONTROL Bereitstellen]** aus.

![Bereitstellen - Wählen Sie die Staging-Umgebung aus](/help/assets/guide-onboarding-agent/deploy-stage.png)

Warten Sie, während [!DNL LLM Apps] die App vorbereitet, erstellt und veröffentlicht.

![Bereitstellen - Bereitstellungs-Pipeline wird ausgeführt](/help/assets/guide-onboarding-agent/deploy-running.png)

![Bereitstellen - erfolgreiche Staging-Bereitstellung](/help/assets/guide-onboarding-agent/deploy-successful.png)

Nach der Bereitstellung wird **[!UICONTROL Abschnitt „App testen]** die Staging-MCP-Server-URL angezeigt. Wählen Sie **[!UICONTROL URL kopieren]** aus.

![Anwendungsdetails - Kopieren Sie die Staging-MCP-Server-URL](/help/assets/guide-onboarding-agent/app-mcp-url.png)

## Test in [!DNL ChatGPT]

Folgen Sie [Testen in ChatGPT](/help/guides/test-in-chatgpt.md), um ein Plug-in mithilfe der Staging-MCP-Server-URL zu erstellen.

Stellen Sie eine Frage, die einer der generierten Aktionen entspricht. Überprüfen Sie, ob:

- [!DNL ChatGPT] wählt die erwartete Aktion aus.
- Das Widget wird gerendert und enthält die erwarteten Beispieldaten.
- Widget-Steuerelemente erzeugen das erwartete Folgeverhalten.
- Die Textantwort fasst das Ergebnis genau zusammen.

![ChatGPT — generierte Antwort des LLM-App-Plug-ins](/help/assets/guide-onboarding-agent/chatgpt-generated-app.png)

Sie verfügen jetzt über eine voll funktionsfähige, funktionierende End-to-End-App.

## Produktionsbereit machen

Die generierte App verwendet Beispieldaten. Vor der Verwendung mit Kunden:

1. **Systeme verbinden** - [jeden generierten Handler anpassen](/help/guides/customize-handler.md) um Beispieldaten durch Aufrufe an Ihre APIs oder Datenquellen zu ersetzen.
2. **Anmeldeinformationen schützen** - API-URLs und Anmeldeinformationen werden in der verwalteten Laufzeitkonfiguration gespeichert, nie im Quell-Code oder Widget-JavaScript.
3. **Daten validieren** - Validieren von Aktionsargumenten und API-Antworten, Hinzufügen von Anfrage-Timeouts und Rückgeben sicherer Fehlermeldungen.
4. **Widgets aktualisieren** - Halten Sie jedes Widget an den `structuredContent` seines Handlers ausgerichtet und wenden Sie dann Ihre Branding- und Barrierefreiheitsanforderungen an. Siehe [Anpassen eines generierten Widgets](/help/guides/widgets.md).
5. **Handler testen** - umfasst gültige Eingaben, ungültige Eingaben, leere Ergebnisse, API-Fehler und die vom Widget erwartete Datenform.
6. **In Staging überprüfen** - Stellen Sie jede Aktion über das Plug-in [!DNL ChatGPT] erneut bereit und testen Sie sie.
7. **In Produktion bereitstellen** - Nach erfolgreichem Staging-Test können Sie das Plug-in in der Produktion bereitstellen und mit der Produktions-MCP-Server-URL erstellen oder aktualisieren.

Informationen zum Hinzufügen einer Funktion, die von der Plattform nicht erstellt wurde, finden Sie unter [Erstellen einer neuen Aktion](/help/guides/create-action.md).

