---
title: Einrichten der Infrastruktur
description: Erstellen Sie eine Adobe-LLM-App mit der von Adobe verwalteten Infrastruktur oder stellen Sie zwei leere GitHub-Repositorys für die Initialisierung der Plattform bereit.
source-git-commit: 14d94a68dabddd2f2ef2cef65c51d31645d4a958
workflow-type: tm+mt
source-wordcount: '1337'
ht-degree: 0%
---

# Einrichten der Infrastruktur {#create-first-app}

>[!IMPORTANT]
>
>[!DNL Adobe LLM Apps] befindet sich derzeit in Beta.
>
>Die hier gezeigten Funktionen, Workflows und Benutzeroberflächen stellen nicht unbedingt den endgültigen Status des Produkts dar. Um Beta beizutreten, senden Sie eine E-Mail an llm-apps-beta@adobe.com.

Infrastruktur ist die technische Grundlage für Ihre App: die Repositorys, die den Code speichern, und die Services, die die Aktionshandler ausführen und die Widgets für die Benutzer bereitstellen.

Wie in der [Übersicht](/help/overview/overview.md#key-concepts) erläutert, führt ein **Aktionshandler** eine Aufgabe aus, z. B. das Suchen von Produkten, während ein **Widget** das Ergebnis als interaktives Erlebnis in der Konversation darstellt.

[!DNL LLM Apps] speichert Handler- und Widget-Quell-Code in zwei separaten [!DNL GitHub]:

- **Aktions-Repository** - enthält die Server-seitigen Handler und ihre Tests.
- **Widget-Repository** - Enthält den Code und die Stile für die interaktiven Widgets.

Beispielsweise ruft ein Produkt-Such-Handler übereinstimmende Produkte ab und zeigt diese als Produktkarten an. Die Repositorys speichern diesen Code. Adobe I/O Runtime führt die bereitgestellten Handler aus, und Edge Delivery Services stellt die Widgets bereit.

Der Setup-Assistent bereitet diese Repositorys vor und verbindet die Services, die Ihre App benötigt. Es wird empfohlen, mit **Adobe Managed Infrastructure** zu beginnen, was der schnellste Einstieg ist. Adobe erstellt und hostet die Repositorys für Ihre Aktionshandler und Widgets. Sie müssen Repositorys nicht selbst einrichten.

Geben Sie alternativ zwei leere [!DNL GitHub]-Repositorys in Ihrem eigenen Konto oder Ihrer Organisation an. Die Plattform initialisiert sie mit den erforderlichen Projektdateien. Es ist nicht erforderlich, Aktion oder Widget-Code im Voraus vorzubereiten.

**In diesem Handbuch:** Erfahren Sie, wie Sie mit dem Einrichtungsassistenten eine Infrastrukturoption auswählen und Ihre App erstellen. Fahren Sie nach dem Setup mit [Aktionen erstellen](/help/guides/creating-actions.md) fort.

Beim Erstellen der App wird **nicht** Aktionserstellung gestartet. Starten Sie die Aktionserstellung getrennt von der App-Seite.

## Öffnen des Assistenten zum Erstellen einer App

Öffnen Sie [Adobe LLM Apps](https://experience.adobe.com/llm-apps). Wählen Sie auf der Startseite die Option **[!UICONTROL Erste Schritte]** aus. Wenn Sie bereits eine App geöffnet haben, verwenden Sie den App-Selektor und wählen Sie **[!UICONTROL Neue App erstellen]**.

Der **[!UICONTROL Create Your LLM App]**-Assistent beginnt mit **[!UICONTROL Erste Schritte]**. Wählen Sie aus, wie die technische Umgebung eingerichtet werden soll.

![Erstellen Sie Ihre LLM-App - wählen Sie zwischen einer von Adobe verwalteten Infrastruktur und der Verbindung Ihrer eigenen Repositorys](/help/assets/guide-infrastructure/setup-choice.png)

| Infrastrukturoption | Im Assistenten auswählen | Was es bedeutet |
|---|---|---|
| **Verwaltete Adobe-Infrastruktur** | **[!UICONTROL Übernehmen wir das]** | Adobe erstellt und verwaltet die Repositorys und verbundenen Services für Ihre App. Code-Zugriff ist auf Anfrage verfügbar. |
| **Ihre eigenen Repositorys** | **[!UICONTROL Ich werde es selbst einrichten]** | Sie stellen zwei leere GitHub-Repositorys bereit und gewähren über den Assistenten Zugriff. Die Plattform initialisiert einen für Aktions-Handler und den anderen für Widgets. |

Beide Optionen unterstützen eine spätere automatische Aktionserstellung. Für keine dieser Optionen müssen Aktionen manuell geschrieben werden.

## Verwenden der von Adobe verwalteten Infrastruktur {#adobe-managed-infrastructure}

Verwenden Sie diese Option, damit Adobe die Repositorys und Connected Services für Ihre App einrichten und verwalten kann. Sie müssen keine Repositorys erstellen oder [!DNL GitHub] Apps selbst installieren.

1. Wählen Sie **[!UICONTROL Übernehmen wir]** aus.
2. Wählen Sie **[!UICONTROL Nächster Schritt]** aus.
3. Vollständige [App-Details](#app-details).
4. Wählen Sie **[!UICONTROL App erstellen]** aus.

Auf der App-Seite wird **[!UICONTROL Einrichten der App]** angezeigt, während Adobe die Repositorys vorbereitet und die Widget-Site verbindet. Die Benutzeroberfläche zeigt an, dass die Einrichtung in der Regel 1-2 Minuten dauert. Aktionen, Tests und die Bereitstellung bleiben bis zur Einrichtung nicht verfügbar.

![App-Einrichtung - Adobe bereitet die App-Infrastruktur vor](/help/assets/guide-infrastructure/provisioning.png)

Wenn das Setup fehlschlägt, lesen Sie den Fehler und wählen Sie &quot;**[!UICONTROL &quot;]**. Wenn die Seite den Fortschritt nicht überprüfen kann, wird die Einrichtung im Hintergrund fortgesetzt. Wählen Sie **[!UICONTROL Erneut überprüfen]**, um die Verbindung wiederherzustellen.

Fahren Sie nach Abschluss des Setups mit [Aktionen erstellen](/help/guides/creating-actions.md) fort.

## Eigene Repositorys verbinden {#your-own-repositories}

Wählen Sie **[!UICONTROL Ich richte es selbst ein]** und dann **[!UICONTROL Nächster Schritt]**. Dieser Pfad weist vier Assistentenschritte auf:

**Erste Schritte → Einrichten der Aktionsinfrastruktur → Einrichten der Widget-Infrastruktur → App-Details.**

Sie benötigen Zugriff auf die ausgewählten Repositorys und die Berechtigung zum Installieren oder Konfigurieren der erforderlichen [!DNL GitHub] Apps. Bei Repositorys im Besitz eines Unternehmens muss ein Administrator möglicherweise den Zugriff genehmigen.

Erstellen Sie zwei leere Repositorys: eines für Aktions-Handler und eines für Widgets. Weder mit einer README, noch mit einer Lizenz oder `.gitignore` initialisieren. Die Plattform fügt die erforderliche Projektstruktur hinzu, wenn Sie jedes Repository im Assistenten überprüfen.

Wenn Sie bereits über kompatible Action- und EDS-Projekte verfügen, können Sie diese stattdessen verbinden, vorhandener Code ist jedoch nicht erforderlich.

>[!NOTE]
>
>Bei einem leeren Repository schreibt die Validierung die ersten Projektdateien. Wählen Sie die für diese Anwendung vorgesehenen Repositorys aus, bevor Sie **[!UICONTROL Repository validieren]** auswählen.

### Einrichten der Aktionsinfrastruktur

Das Aktions-Repository enthält die Server-seitigen Handler und Tests, die ausgeführt werden, wenn eine LLM-Plattform eine Aktion aufruft.

1. Wählen **[!UICONTROL unter „Aktionsquelle]** die Option **[!UICONTROL GitHub]**. Die anderen Code-Quellen sind derzeit nicht verfügbar.
2. Antwort **[!UICONTROL Haben Sie bereits ein GitHub-Repository erstellt?]**:
   - Wählen Sie **[!UICONTROL Ja, ich werde ein bereits erstelltes Repository verwenden]** um ein kompatibles vorhandenes Projekt oder ein leeres Repository zu verbinden.
   - Wählen Sie **[!UICONTROL Ich habe noch kein Repository]** und dann **[!UICONTROL Erstellen von Repositorys auf GitHub]** aus, um ein leeres Aktions-Repository zu erstellen. Kehren Sie anschließend zum Assistenten zurück.
3. Verbinden des Repositorys mit der Adobe LLM Apps [!DNL GitHub] App:
   - Wenn keine Organisation verbunden ist, wählen Sie **[!UICONTROL Auf GitHub installieren]** aus.
   - Wenn eine Organisation bereits verbunden ist, wählen Sie **[!UICONTROL Repository verbinden]** und fügen Sie das Repository zu dieser Installation hinzu.
4. Wählen Sie [!DNL GitHub] **[!UICONTROL Nur Repositorys auswählen]** und gewähren Sie nur Zugriff auf die Repositorys, die von dieser App verwendet werden.
5. Zurück zum Assistenten. Wählen **[!UICONTROL unter &quot;]** auswählen“ die Optionen **[!UICONTROL Organisation]** und **[!UICONTROL Repository]**.
6. Wählen Sie **[!UICONTROL Repository validieren]** aus. Bei der Validierung wird ein leeres Repository mit dem Adobe LLM Apps-Textbaustein initialisiert. Ein vorhandenes Repository muss ein kompatibles Aktionsprojekt enthalten.
7. Klicken Sie nach erfolgreicher Validierung auf **[!UICONTROL Nächster Schritt]**.

![Einrichtung der Aktionsinfrastruktur - Wählen Sie GitHub und verbinden Sie ein Aktions-Repository](/help/assets/guide-infrastructure/action-repository.png)

Wenn eine Organisation oder ein Repository fehlt, verwenden Sie **[!UICONTROL Andere Organisation verbinden]** oder **[!UICONTROL Anderes Repository verbinden]** in der Auswahl. Nachdem Sie den Zugriff auf [!DNL GitHub] geändert haben, wählen Sie **[!UICONTROL Organisationen und Repositorys aktualisieren]** aus.

### Einrichten der Widget-Infrastruktur

Widgets sind optionale visuelle Antworten, die an Aktionen angehängt werden, z. B. Produktkarten oder Buchungsformulare. Der Assistent verbindet ein [!DNL Edge Delivery Services]-Repository (EDS), um sie zu hosten.

1. Wählen **[!UICONTROL unter]** Widget-Quelle **[!UICONTROL die Option Widget-Repository manuell]**.
2. Wählen **[!UICONTROL unter &quot;]**&quot; **[!UICONTROL GitHub]** aus.
3. Wählen Sie aus, ob Sie bereits über ein Repository verfügen. Andernfalls wählen Sie **[!UICONTROL Erstellen von Repositorys auf GitHub]**, erstellen Sie ein leeres Widget-Repository und kehren Sie zum Assistenten zurück.
4. Installieren oder konfigurieren Sie die Adobe LLM Apps [!DNL GitHub] App für dieses Repository, wie Sie es auch für das Aktions-Repository getan haben.
5. Wählen Sie die **[!UICONTROL Organisation]** und **[!UICONTROL Repository]** aus. Wählen Sie nicht das Aktions-Repository.
6. Wählen Sie **[!UICONTROL Repository validieren]** aus. Bei der Validierung wird ein leeres Repository mit dem AEM-Textbaustein initialisiert. Ein vorhandenes Repository muss ein kompatibles EDS-Projekt sein.
7. Wählen Sie **[!UICONTROL AEM Code Sync installieren]** und gewähren Sie AEM Code Sync Zugriff auf das Widget-Repository.
8. Verwenden Sie auf der Seite zur Installationsbestätigung den Setup-Assistenten, um sich als Administrator der EDS-Site hinzuzufügen.
9. Kehren Sie zu [!DNL LLM Apps] zurück und wählen **[!UICONTROL Installation von AEM Code-Synchronisierung validieren]**.
10. Wenn **[!UICONTROL AEM Code Sync installed]** angezeigt wird, wählen Sie **[!UICONTROL Nächster Schritt]**.

>[!NOTE]
>
>Der Assistent markiert **[!UICONTROL Cloud Manager Edge Delivery Services verwenden]** als **[!UICONTROL In Kürze verfügbar]** und lässt keine Auswahl zu. Verwenden Sie die Option „Manual widget-repository“.

Wenn bei der Validierung keine AEM-Codesynchronisierung erkannt wird, stellen Sie sicher, dass AEM-Codesynchronisierung Zugriff auf das ausgewählte Widget-Repository hat und dass Sie sich selbst als Site-Admin hinzugefügt haben. Validieren Sie dann erneut.

## Mobile-App-Details eingeben {#app-details}

Beide Infrastrukturoptionen enden mit **[!UICONTROL App-Details]**.

![App-Details - Geben Sie einen App-Namen ein, wählen Sie ein AEM-Programm aus und wählen Sie die Region der Analytics-Daten aus](/help/assets/guide-infrastructure/app-details.png)

1. Geben Sie den **[!UICONTROL LLM-App-Namen]** ein. Verwenden Sie einen Namen, der die Marke oder das Erlebnis identifiziert, z. B *„Frescopa Coffee*.
2. Wählen Sie das **[!UICONTROL AEM]** Programm aus, zu dem die App gehört. Wenn Sie nur Zugriff auf ein Programm haben, wählt der Assistent dieses Programm automatisch aus.
3. Wählen **[!UICONTROL unter „Analytics]** Datenregion“ die Option **[!UICONTROL Vereinigte Staaten (USA)]** oder **[!UICONTROL Europa (EU)]**.
4. Wählen Sie **[!UICONTROL App erstellen]** aus.

>[!IMPORTANT]
>
>Das AEM-Programm und die Analytics-Datenregion können nach der Erstellung der App nicht mehr geändert werden. Die Analyseregion bestimmt, wo die Analysedaten der App gespeichert werden.

Wenn kein AEM-Programm verfügbar ist, bitten Sie Ihren Administrator um Zugriff. Wenn das Laden der Programme fehlschlägt, wählen Sie **[!UICONTROL Programme erneut laden]** aus.

Der Assistent öffnet die App-Seite nach der Erstellung. Warten Sie bei Adobe Managed Infrastructure, bis die Einrichtung der Infrastruktur abgeschlossen ist, bevor Sie fortfahren.

## Weiter: Erstellen der Aktionen

Ihre App verfügt jetzt über die Infrastruktur, es wurden jedoch noch keine Aktionen generiert. Fahren Sie mit [Aktionen erstellen](/help/guides/creating-actions.md) fort, um eine Website zu analysieren oder manuelle Aktionserstellung zu wählen.

Informationen zur Beziehung zwischen Aktionsmetadaten, Handlern, Widgets und dem bereitgestellten MCP-Server finden Sie [Wie eine App verkabelt wird](/help/guides/app-architecture.md).
