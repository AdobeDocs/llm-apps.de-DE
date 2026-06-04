---
title: Testen in ChatGPT
description: Erfahren Sie, wie Sie Ihre bereitgestellte Adobe LLM-App zu ChatGPT hinzufügen und in einem echten Gespräch testen können.
source-git-commit: 483a71f5f1de5caf1bd89b26f4d67d2d5a0aa15a
workflow-type: tm+mt
source-wordcount: '798'
ht-degree: 2%

---


# Test in [!DNL ChatGPT]

>[!IMPORTANT]
>
>**Haftungsausschluss:** Dies ist eine Beta-Version von [!DNL LLM Apps]. Die hier gezeigten Funktionen, Workflows und Benutzeroberflächen stellen nicht unbedingt den endgültigen Status der Anwendung oder des Produkts dar.

>[!NOTE]
>
>In diesem Handbuch wird [!DNL ChatGPT] als Beispiel verwendet. Die allgemeinen Schritte - Registrieren einer MCP-Server-URL und Testen in einem Gespräch - gelten auch für andere LLM-Plattformen, obwohl der Einrichtungsablauf und die Benutzeroberfläche variieren.

Nach einer erfolgreichen Bereitstellung wird Ihre App auf [!DNL Adobe I/O Runtime] ausgeführt und gibt eine MCP-Server-URL an. In diesem Handbuch erfahren Sie, wie Sie sie zu [!DNL ChatGPT] hinzufügen und in einer echten Konversation testen können.

## Plananforderungen

Das Hinzufügen benutzerdefinierter Entwickler-Apps zu [!DNL ChatGPT] wird durch die Abonnementebenen von OpenAI gesteuert - dies ist keine [!DNL LLM Apps] Einschränkung, sondern beschreibt, wie OpenAI derzeit den Zugriff auf benutzerdefinierte MCP-Apps verwaltet.

| [!DNL ChatGPT] | Benutzerdefinierte MCP-Apps |
|--------------|-----------------|
| Kostenlos | Nicht verfügbar |
| Los | Nicht verfügbar |
| Plus | Nicht verfügbar |
| Pro | Verfügbar |
| Geschäft | Verfügbar |
| Unternehmen/EDU | Verfügbar |

>[!NOTE]
>
>Wenn Sie einen Free-, Go- oder Plus-Plan haben **können Sie Ihre bereitgestellte App nicht** [!DNL ChatGPT] hinzufügen. Führen Sie ein Upgrade auf **Pro** durch oder bitten Sie den Administrator Ihres Unternehmens, es in einem **Business** oder **Enterprise**-Arbeitsbereich zu aktivieren.

## Entwicklermodus aktivieren

Um eine benutzerdefinierte MCP-App hinzuzufügen, muss **Entwicklermodus** in Ihrem [!DNL ChatGPT]-Konto aktiviert sein. Folgen
Gehen Sie wie folgt vor, um sie zu überprüfen und zu aktivieren.

### Einstellungen öffnen

Klicken Sie unten links auf Ihren Profilavatar und dann auf **[!UICONTROL Einstellungen]**.

![ChatGPT — Menü Einstellungen](/help/assets/guide-test-chatgpt/chatgpt-settings-menu.png)

### Zu Apps navigieren

Wählen Sie im Dialogfeld „Einstellungen **[!UICONTROL in]** linken Seitenleiste die Option „Apps“ aus. Klicken Sie **[!UICONTROL unten]** „Erweiterte Einstellungen“.

![ChatGPT - Apps-Einstellungen](/help/assets/guide-test-chatgpt/chatgpt-apps-settings.png)

### Entwicklermodus aktivieren

Stellen Sie sicher **[!UICONTROL dass der Umschalter]** Entwicklermodus) aktiviert ist (blau). Auf diese Weise können Sie benutzerdefinierte, nicht verifizierte MCP-Server-URLs registrieren.

>[!NOTE]
>
>Der Entwicklermodus ist mit *Erhöhtes Risiko* gekennzeichnet, da er Apps zulässt, die nicht von OpenAI geprüft wurden. [!DNL ChatGPT] deaktiviert automatisch den Speicher für Unterhaltungen, die Entwicklermodus-Apps verwenden.

![ChatGPT — Entwicklermodus aktiviert](/help/assets/guide-test-chatgpt/chatgpt-developer-mode.png)

## Hinzufügen der App zu [!DNL ChatGPT]

### MCP-Server-URL kopieren

Navigieren Sie zur Seite **App-Details** in [!DNL LLM Apps] und suchen Sie den Abschnitt **[!UICONTROL App testen]**. Kopieren Sie entweder die **Staging**- oder **Produktions** URL - sie sieht wie folgt aus:

```
https://<namespace>.adobeioruntime.net/api/v1/web/llm-apps/mcp
```

### Öffnen der Seite „Apps“

Navigieren Sie [!DNL ChatGPT] zu **[!UICONTROL Einstellungen] → [!UICONTROL Apps]**.

![ChatGPT — Apps-Seite](/help/assets/guide-test-chatgpt/chatgpt-apps-page.png)

### Neue App erstellen

Klicken Sie **[!UICONTROL App erstellen]** in der Zeile „Erweiterte Einstellungen“.

![ChatGPT - Dialogfeld „App erstellen“](/help/assets/guide-test-chatgpt/chatgpt-create-app.png)

Füllen Sie Folgendes aus:

| Feld | Wert |
|-------|-------|
| **Symbol** | Optional — Hochladen einer 128 x 128 PNG (max. 10 KB) |
| **Name** | Ein Anzeigename für Ihre App (z. B. *Meine Brand App*) |
| **Beschreibung** | Eine kurze Beschreibung dessen, was die App tut |
| **MCP-Server-URL** | Einfügen der URL aus [!DNL LLM Apps] |
| **[!UICONTROL Authentifizierung]** | Wählen Sie *Keine Authentifizierung* |

Kontrollkästchen **Ich verstehe und möchte fortfahren** - Dadurch wird bestätigt, dass der MCP-Server
wurde nicht von OpenAI geprüft — und klicken Sie auf **Erstellen**.

### Überprüfen, ob die App aktiviert ist

Nach der Erstellung wird Ihre App unter **[!UICONTROL Aktivierte Apps]** mit einem **[!UICONTROL DEV]**-Badge angezeigt, das bestätigt, dass sie aktiv ist.

>[!NOTE]
>
>Ihre App wird auch unter **Entwürfe** angezeigt. Dabei handelt es sich um private Apps, die Sie im Entwicklermodus erstellt haben und die nur für Ihr Konto sichtbar sind.

Ihre App kann jetzt in [!DNL ChatGPT] Konversationen verwendet werden.

![ChatGPT — App aktiviert](/help/assets/guide-test-chatgpt/chatgpt-app-enabled.png)

## Test in einem Gespräch

Sobald die App aktiviert ist, beginnen Sie eine neue Konversation in [!DNL ChatGPT]. Bevor Sie eine Frage stellen, hängen Sie Ihre App mit einer von zwei Methoden an.

### Option 1 — Auswahl aus dem Menü

Klicken Sie in der Chat-Eingabe auf **+** und dann auf **Mehr**, um die vollständige Liste der verfügbaren Tools zu erweitern. Wählen Sie Ihre App aus der Liste aus, um sie an die aktuelle Unterhaltung anzuhängen.

![ChatGPT — App aus Menü auswählen](/help/assets/guide-test-chatgpt/chatgpt-select-app.png)

### Option 2 — @mention

Geben Sie **@** in die Chat-Eingabe ein und wählen Sie Ihre App aus dem Dropdown-Menü aus. Dadurch wird die App inline angehängt und Sie können Ihre Frage weiterhin in derselben Nachricht eingeben.

>[!NOTE]
>
>Bei der **von**@mention wird die Auswahl aufgehoben und die App aus der Konversation entfernt.

![ChatGPT - @mention die App](/help/assets/guide-test-chatgpt/chatgpt-mention-app.png)

Nach der Auswahl wird die App inline angehängt und Sie können Ihre Frage in derselben Nachricht eingeben:

![ChatGPT — App über @mention angehängt](/help/assets/guide-test-chatgpt/chatgpt-mention.png)

### Ergebnis anzeigen

Sobald die App angehängt ist, geben Sie eine Frage ein, die mit einer Ihrer konfigurierten Aktionen abgestimmt ist, z. B. *„Zeige mir deine Produkte“.* [!DNL ChatGPT] ordnet sie der entsprechenden Aktion zu, extrahiert die Eingabeparameter, ruft den Handler auf [!DNL Adobe I/O Runtime] auf und rendert das Ergebnis:

![ChatGPT — Aktionsergebnis](/help/assets/guide-test-chatgpt/chatgpt-response.png)

Die Antwort umfasst:

- **Das EDS-Widget** - eine umfangreiche UI-Komponente mit Bildern, Bewertungen und Aktionsschaltflächen.
- **Die Textantwort** - Unter dem Widget verwendet [!DNL ChatGPT] die von Ihrem Handler zurückgegebene `content`
um eine Zusammenfassung der Ergebnisse in natürlicher Sprache zu formulieren.
- **Statusanzeige** - der *aufgerufene Statustext* den Sie im Dialogfeld Aktion erstellen konfiguriert haben.

## Wie geht es weiter

- **Weitere Aktionen hinzufügen** - Definieren Sie zusätzliche Aktionen in der Benutzeroberfläche, schreiben Sie deren Handler und stellen Sie sie erneut bereit.
- **In Produktion bereitstellen** - Wenn Sie im Staging getestet haben, stellen Sie in der Produktion für das Live-Erlebnis bereit.
- **Für Ihr Team freigeben** - Verwenden Sie **URL kopieren** auf der Seite „Anwendungsdetails“, um die MCP-Server-URL für Teammitglieder freizugeben.

