---
title: Beta-Onboarding
description: Erste Schritte mit Adobe LLM Apps as a Beta Program Participant.
source-git-commit: f144ccfc0ede6c556ccf4d99173f91d372add6f7
workflow-type: tm+mt
source-wordcount: '1545'
ht-degree: 0%

---


>[!IMPORTANT]
>
>**Haftungsausschluss:** Dies ist eine Beta-Version von [!DNL LLM Apps]. Die hier gezeigten Funktionen, Workflows und Benutzeroberflächen stellen nicht unbedingt den endgültigen Status der Anwendung oder des Produkts dar.

>[!NOTE]
>
>Bevor Sie beginnen, stellen Sie sicher, dass [Voraussetzungen](/help/beta-onboarding/prerequisites.md) erfüllt sind.

Als Beta-Programmteilnehmer erhalten Sie eine E-Mail mit zwei ZIP-Archiven und einer App-Konfigurationsreferenz. Gehen Sie wie folgt vor, um Ihre App live zu schalten.

## Bevor Sie beginnen

Bevor Sie sich mit den Schritten vertraut machen, machen Sie sich mit den wichtigsten Konzepten vertraut, die in diesem Handbuch verwendet werden. Das spart Ihnen Zeit und hilft Ihnen, alles zu finden.

**LLM App** - Ihr gebrandeter Assistent, mit dem Benutzer in [!DNL ChatGPT] oder anderen LLM-Plattformen interagieren.

**Action** - eine Funktion, die Ihre App bietet. Zum Beispiel „Distributor suchen“ oder „Produkte durchsuchen“. Jede Aktion wird vom LLM aufgerufen, wenn der Benutzer eine relevante Frage stellt.

**Action Handler** - Der Code, der ausgeführt wird, wenn eine Aktion aufgerufen wird. Es kann Ihre APIs aufrufen, Live-Daten abrufen oder statische Daten zurückgeben. Die von Adobe bereitgestellten Beispiel-Handler geben hartcodierte Daten zurück, damit Sie die Einrichtung End-to-End überprüfen können, bevor Sie Ihr echtes Backend verbinden.

**Widget** - die visuelle Antwort, die dem Benutzer angezeigt wird - eine Karte, ein Karussell, eine Tabelle oder eine beliebige benutzerdefinierte Benutzeroberfläche, die zusammen mit der Textantwort des LLM gerendert wird.

**App-Konfigurationsreferenz** - Eine Datei, die von Adobe bereitgestellt wird und die Ihnen genau mitteilt, was Sie bei jeder Aktion beim Einrichten Ihrer App eingeben müssen.


## Schritt 1: Pushen der bereitgestellten Archive nach [!DNL GitHub]

Adobe bietet zwei ZIP-Archive per E-Mail:

- **Anwendungscode** (`<project-name>.zip`) - die Aktions-Handler, die auf [!DNL Adobe I/O Runtime] ausgeführt werden und die Logik Ihrer App unterstützen. Sie werden diese unverändert bereitstellen, damit die App durchgängig funktioniert, und sie dann später aktualisieren, um Ihr echtes Backend zu verbinden.
- **EDS-Projekt** (`<project-name>-eds.zip`) - der Frontend-Code für Ihre Widgets. Adobe hat diese für Sie vorkonfiguriert. Dies ist Ihre Codebasis, die Sie besitzen, anpassen und an Ihre Marke anpassen können.

Erstellen Sie **zwei neue leere Repositorys** auf [!DNL GitHub] (eines pro Archiv), entpacken Sie dann jedes Archiv und übertragen Sie es. Es wird empfohlen, jedes Repository nach der entsprechenden ZIP-Datei zu benennen - `<project-name>` für den Programm-Code und `<project-name>-eds` für das EDS-Projekt.

`<your-github-org>` bezieht sich entweder auf Ihren persönlichen [!DNL GitHub] Benutzernamen oder auf eine [!DNL GitHub] Organisation - je nachdem, welches Konto die Repositorys besitzen wird.

**Anwendungs-Code-Repository** - Entpacken Sie das Archiv, initialisieren Sie ein lokales Git-Repository und übertragen Sie es auf [!DNL GitHub]:

```bash
# Unzip and enter the folder
unzip <project-name>.zip
cd <project-name>

# Initialize and push
git init
git add .
git commit -m "first commit"
git branch -M main
git remote add origin git@github.com:<your-github-org>/<your-repo>.git
git push -u origin main
```

**EDS-Repository** - Wiederholen Sie dieselben Schritte für das EDS-Archiv, indem Sie auf das zweite Repository verweisen:

```bash
unzip <project-name>-eds.zip
cd <project-name>-eds

git init
git add .
git commit -m "first commit"
git branch -M main
git remote add origin git@github.com:<your-github-org>/<your-eds-repo>.git
git push -u origin main
```

## Schritt 2: Erstellen einer LLM-App

Navigieren Sie zu [experience.adobe.com/llm-apps/ &#x200B;](https://experience.adobe.com/llm-apps/) klicken Sie auf **[!UICONTROL LLM-App erstellen]**.

![Apps-Seite - noch keine Apps erstellt](/help/assets/guide-create-app/first-load.png)

Füllen Sie das Feld **[!UICONTROL App-Details]** mit den Werten aus dem Abschnitt **[!UICONTROL App-Details]** in Ihrer App-Konfigurationsreferenz aus:

- **[!UICONTROL LLM-App-Name]**
- **[!UICONTROL LLM-App-Beschreibung]**
- **[!UICONTROL Ihre Website]**

![Dialogfeld „App erstellen“](/help/assets/guide-create-app/app-details-1.png)

Wählen **[!UICONTROL unter „Analytics]** Datenregion“ die Region aus, in der Analytics-Daten gespeichert werden. Dies **kann nicht geändert werden** nachdem die App erstellt wurde.

>[!IMPORTANT]
>
>Die Analytics-Datenregion kann nach der Erstellung der App nicht mehr geändert werden.

![Dropdown-Liste „Analytics-Datenregion“](/help/assets/guide-create-app/app-details-analytics-dropdown.png)

Wählen **unter** Repository) die [!DNL GitHub] **Organisation und das zuvor** Anwendungs-Code-Repository) aus.

>[!NOTE]
>
>Wenn Sie zum ersten Mal eine App einrichten, wird Ihre [!DNL GitHub] Organisation noch nicht in der Liste angezeigt. Klicken Sie auf **[!UICONTROL Mit anderer GitHub-Organisation verbinden]**, um Ihre Organisation zu verknüpfen und Zugriff auf das Repository zu gewähren.

![Dialogfeld „App erstellen“ - Repository-verknüpft](/help/assets/guide-create-app/app-details-repo-linked.png)

Lassen Sie **[!UICONTROL Aktionen basierend auf Ihrer Website automatisch vorschlagen]** deaktiviert. Sie werden Aktionen manuell konfigurieren.

Akzeptieren Sie die **[!UICONTROL Adobe Developer-]** und klicken Sie dann auf **[!UICONTROL App erstellen]**.

![App erstellen — Ladebildschirm](/help/assets/guide-create-app/app-loading.png)

![App-Detailseite](/help/assets/guide-create-app/app-detail-top.png)


## Schritt 3: Widgets live schalten

In diesem Schritt richten Sie das bereitgestellte EDS-Projekt ein und veröffentlichen es über [!DNL DA.live] - die Authoring- und CDN-Ebene von Adobe. Jedes veröffentlichte Dokument wird zum Widget, das den Benutzenden angezeigt wird, wenn eine Aktion aufgerufen wird.

### Schritt 3.1: EDS-Repository mit [!DNL DA.live] verbinden

1. Navigieren Sie zu [github.com/apps/aem-code-sync](https://github.com/apps/aem-code-sync). Wenn die App noch nicht installiert ist, klicken Sie auf **[!UICONTROL Installieren]**. Wenn es bereits installiert ist, klicken Sie auf **[!UICONTROL Konfigurieren]** und fügen Sie `<your-eds-repo>` zur Liste der Repositorys hinzu, auf die es zugreifen kann.
2. Nach der Installation landen Sie auf einer **[!DNL AEM Code Sync]registrierten** Bestätigungsseite. Klicken **unter „Was kommt als Nächstes → Inhalte erstellen** auf den Link [!DNL DA.live] .
3. Wählen Sie auf dem Bildschirm **Demoinhalt** die Option **Keine** und klicken Sie auf **Etwas Wunderbares erstellen**.
4. Sie gelangen zur [!DNL DA.live] Autorenansicht für Ihre Site.

### Schritt 3.2: Erstellen Sie für jede Aktion ein [!DNL DA.live]

In [!DNL DA.live] müssen Sie **ein Dokument pro Aktion** erstellen. Nach der Veröffentlichung wird jedes Dokument zum Widget, das dem Benutzer angezeigt wird, wenn diese Aktion aufgerufen wird.

Für jede Aktion:

1. Erstellen Sie in [!DNL DA.live] ein neues Dokument im Stammverzeichnis Ihrer Site und benennen Sie es wie in Ihrer App-Konfigurationsreferenz angegeben (siehe Abschnitt **[!DNL DA.live]-**).
2. Verwenden Sie im Dokument die linke Seitenleiste und klicken Sie auf **[!UICONTROL Block]**, um einen neuen Block einzufügen.
3. Setzen Sie die Blockkopfzeile auf den Blocknamen, der in Ihrer App-Konfigurationsreferenz angegeben ist (siehe Abschnitt **[!DNL DA.live]-**).
4. Veröffentlichen Sie das Dokument mithilfe der **[!UICONTROL Veröffentlichen]**-Schaltfläche (Papierebenensymbol in der oberen Symbolleiste).

Nach der Veröffentlichung ist jedes Dokument unter `https://main--<your-eds-repo>--<your-github-org>.aem.live/<document-name>` verfügbar. Diese URL geben Sie beim Konfigurieren jeder Aktion in Schritt 4 in das Feld **[!UICONTROL Widget]** ein.


### Schritt 3.3: Konfigurieren der CORS-Header für die EDS-Website

Damit LLM-Plattformen Ihre Widgets ursprungsübergreifend laden können, müssen Sie eine `Access-Control-Allow-Origin` Kopfzeile zu Ihrer EDS-Site hinzufügen.

Navigieren Sie zum **HTTP-Header-** unter [tools.aem.live/tools/headers-edit/index.html](https://tools.aem.live/tools/headers-edit/index.html).

1. Geben Sie Ihre **Organisation** (`<your-github-org>`) und **Site** (Ihren EDS-Repository-Namen) ein und klicken Sie auf **[!UICONTROL Abrufen]**. Sie werden aufgefordert, sich zu authentifizieren und den Zugriff auf Ihre Site zu autorisieren.
2. Klicken Sie unter Pfad `/**` auf **[!UICONTROL Kopfzeile hinzufügen]**.
3. Legen Sie den Header-Namen auf `Access-Control-Allow-Origin` und den Wert auf `*` fest.
4. Klicken Sie auf **[!UICONTROL Speichern]**.

Eine vollständige Dokumentation zu benutzerdefinierten HTTP-Kopfzeilen in [!DNL AEM Edge Delivery Services] finden Sie unter [aem.live/docs/custom-headers](https://www.aem.live/docs/custom-headers).

Geben Sie nach dem Speichern der Header einen Trigger zur Codesynchronisierung ein, um die Änderungen an alle Dateien weiterzugeben:

```bash
curl -X POST "https://admin.hlx.page/code/<your-github-org>/<your-eds-repo>/main/*"
```


## Schritt 4: Aktionen hinzufügen

Öffnen Sie in der [LLM Apps](https://experience.adobe.com/llm-apps/)Benutzeroberfläche Ihre App und navigieren Sie in der linken **zu** Aktionen. Klicken Sie auf **+**, um eine neue Aktion zu erstellen. Wiederholen Sie den Vorgang für jede Aktion, die in Ihrer App-Konfigurationsreferenz beschrieben wird (siehe **Aktion 1**, **Aktion**, **Aktion 3** Abschnitte).

![Seite „Aktionen“ - noch keine Aktionen](/help/assets/guide-create-action/actions-empty.png)

### Registerkarte „Aktion“

- **Aktionsname** und **Beschreibung** - wird von LLM-Plattformen verwendet, um zu entscheiden, wann die Aktion aufgerufen werden soll. Verwenden Sie die exakten Werte aus dem Abschnitt **Registerkarte**&quot; in Ihrer App-Konfigurationsreferenz.
- **Eingabeparameter** - Name, Typ und Beschreibung für jeden Parameter. Verwenden Sie die Werte aus dem Abschnitt **Registerkarte**&quot; in Ihrer App-Konfigurationsreferenz.

![Aktion erstellen - Grundlegende Informationen](/help/assets/guide-create-action/action-basic-info.png)

### Registerkarte Widget-Metadaten

- **Typ** — wählen Sie **[!UICONTROL EDS]**.

Erweitern Sie **[!UICONTROL CSP-Konfiguration]** und füllen Sie Folgendes aus:

- **[!UICONTROL CSP — Verbinden von Domains]** — Verwenden Sie die Werte aus dem Abschnitt **Registerkarte Widget-**&quot; in Ihrer App-Konfigurationsreferenz.
- **[!UICONTROL CSP — Ressourcen-Domains]** — Verwenden Sie die Werte aus dem Abschnitt **Widget-Metadaten** in Ihrer App-Konfigurationsreferenz.

![Aktion erstellen — Widget-Metadaten](/help/assets/guide-create-action/widget-metadata.png)

![Aktion erstellen - Berechtigungen und CSP](/help/assets/guide-create-action/widget-permissions-csp.png)

### Registerkarte Widget-Builder

Wählen **[!UICONTROL unter &quot;]**&quot; die Option **[!UICONTROL Vorhandenes Widget verwenden]** und füllen Sie dann Folgendes aus:

- **[!UICONTROL Skript-URL]** - Verwenden Sie den Wert aus dem Abschnitt **Registerkarte „Widget-**&quot; in Ihrer App-Konfigurationsreferenz.
- **[!UICONTROL Widget-]**: Verwenden Sie den Wert aus dem Abschnitt **Widget-Metadaten** in Ihrer App-Konfigurationsreferenz.

Klicken Sie **[!UICONTROL Aktion erstellen]**. Die Aktion wird als Karte auf der Seite „Aktionen“ mit einem **[!UICONTROL EDS]**-Badge und einer Parameteranzahl angezeigt.

![Seite „Aktionen“ - Aktion erstellt](/help/assets/guide-create-action/actions-with-action.png)


## Schritt 5: Bereitstellen

Sobald alle Aktionen konfiguriert sind, gehen Sie zur Seite mit den App-Details und klicken **[!UICONTROL oben rechts]** Bereitstellen“.

![App-Details - bereit zur Bereitstellung](/help/assets/guide-deploy/app-detail-deploy-ready.png)

Wählen Sie die Zielumgebung aus und klicken Sie auf **[!UICONTROL Bereitstellen]**. Die Pipeline durchläuft vier Schritte: Vorbereiten von Anmeldeinformationen, Starten der Bereitstellung, Erstellen der App aus dem Repository und Veröffentlichen in [!DNL Adobe I/O Runtime].

![Pipeline-Ausführung bereitstellen](/help/assets/guide-deploy/deploy-pipeline-deploying.png)

Scrollen Sie dann zum Abschnitt **[!UICONTROL Testen der App]** auf der Seite mit den App-Details und kopieren Sie die **[!UICONTROL MCP-Server-URL]**. Sie benötigen sie, um Ihre App in [!DNL ChatGPT] zu registrieren.

![Bereitstellung erfolgreich](/help/assets/guide-deploy/app-detail-deploy-finish.png)

![Testen der App - bereitgestellte URLs](/help/assets/guide-deploy/test-app-deployed.png)


## Schritt 6: Hinzufügen der App zu [!DNL ChatGPT]

Das Hinzufügen benutzerdefinierter Apps zu [!DNL ChatGPT] erfordert ein **Pro**-, **Business**- oder **Enterprise**-Abonnement. Kostenlose und Plus-Pläne unterstützen keine benutzerdefinierten MCP-Apps.

1. Klicken Sie in [!DNL ChatGPT] auf Ihren Profilavatar und gehen Sie zu **[!UICONTROL Einstellungen]**.

   ![ChatGPT — Menü Einstellungen](/help/assets/guide-test-chatgpt/chatgpt-settings-menu.png)

2. Wählen Sie **[!UICONTROL Apps]** in der Seitenleiste aus, klicken Sie auf **[!UICONTROL Erweiterte Einstellungen]** und aktivieren Sie **[!UICONTROL Entwicklermodus]**.

   ![ChatGPT — Entwicklermodus aktiviert](/help/assets/guide-test-chatgpt/chatgpt-developer-mode.png)

3. Gehen Sie zu **[!UICONTROL Einstellungen] → [!UICONTROL Apps]** und klicken Sie auf **[!UICONTROL App erstellen]**.

   ![ChatGPT - Dialogfeld „App erstellen“](/help/assets/guide-test-chatgpt/chatgpt-create-app.png)

4. Fügen Sie die **[!UICONTROL MCP-Server]** URL, die aus [!DNL LLM Apps] kopiert wurde, ein **[!UICONTROL Authentifizierung]**-Feld auf *Keine Auth* ein, aktivieren Sie das Bestätigungs-Kontrollkästchen, und klicken Sie auf **Erstellen**.

Ihre App wird unter **[!UICONTROL Aktivierte Apps]** mit einem **[!UICONTROL DEV]**-Badge angezeigt.

![ChatGPT — App aktiviert](/help/assets/guide-test-chatgpt/chatgpt-app-enabled.png)

Beginnen Sie eine neue Konversation, fügen Sie Ihre App über die Schaltfläche **+** an oder geben Sie **@** gefolgt von Ihrem App-Namen ein und stellen Sie eine Frage, die einer Ihrer konfigurierten Aktionen entspricht.

![ChatGPT — App aus Menü auswählen](/help/assets/guide-test-chatgpt/chatgpt-select-app.png)

![ChatGPT — Aktionsergebnis](/help/assets/guide-test-chatgpt/chatgpt-response.png)

## Wie geht es weiter

Die bereitgestellte Beispielanwendung verwendet hartcodierte Daten. So wandeln Sie sie in ein produktionsfähiges Erlebnis um:

- **Verbinden Sie Ihre APIs** - Aktualisieren Sie die Aktions-Handler in Ihrem Anwendungs-Code-Repository, um Ihre echten APIs, Datenbanken oder Services aufzurufen. Jeder Handler lebt in `actions/<action-name>/index.js`.
- **Überprüfen und verfeinern Sie Ihre Widgets** - Öffnen Sie Ihr EDS-Projekt, passen Sie die Blockstile und das Layout an Ihre Marke an und überprüfen Sie, ob das Widget mit Live-Daten korrekt dargestellt wird.
- **Neu bereitstellen** - Sobald Ihre Handler und Widgets aktualisiert wurden, übertragen Sie Ihre Änderungen auf [!DNL GitHub] und klicken Sie in der [!DNL LLM Apps]-Benutzeroberfläche **[!UICONTROL Bereitstellen]**, um die neue Version zu veröffentlichen.
- **Zur Veröffentlichung einreichen** - Wenn Sie mit dem Erlebnis zufrieden sind, senden Sie Ihre App zur Überprüfung über den Veröffentlichungsprozess des [!DNL ChatGPT]-Plug-ins oder -Connectors. Adobe hat keine Kontrolle über diesen Prozess. Die Anforderungen und Zeitpläne für die Übermittlung finden Sie in der Dokumentation der LLM-Plattform.
