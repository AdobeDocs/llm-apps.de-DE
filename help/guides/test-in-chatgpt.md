---
title: Testen der LLM-App als ChatGPT-Plug-in
description: Erstellen Sie ein ChatGPT-Plug-in aus Ihrer Adobe LLM Apps MCP-Server-URL und testen Sie es in einer Konversation.
source-git-commit: b7199fbb387d91a5c77deac47a2bc883381931c1
workflow-type: tm+mt
source-wordcount: '335'
ht-degree: 1%

---


# Testen der LLM-App als [!DNL ChatGPT] Plug-in {#test-in-chatgpt}

>[!IMPORTANT]
>
>[!DNL Adobe LLM Apps] befindet sich derzeit in Beta.
>
>Die hier gezeigten Funktionen, Workflows und Benutzeroberflächen stellen nicht unbedingt den endgültigen Status des Produkts dar. Um Beta beizutreten, senden Sie eine E-Mail an llm-apps-beta@adobe.com.

Nach der Bereitstellung stellt Ihre LLM-App eine MCP-Server-URL bereit. Fügen Sie diese URL zu [!DNL ChatGPT] als Plug-in hinzu und testen Sie dann die generierten Aktionen und Widgets.

Dies ist der letzte Überprüfungsschritt nach dem Erstellen, Anpassen oder Erweitern einer App.

## Plananforderungen

Der Entwicklermodus ist im Web für Pro-, Plus-, Business-, Enterprise- und Education-Konten verfügbar. Workspace-Administratoren können den Zugriff einschränken.

## Entwicklermodus aktivieren

In [!DNL ChatGPT]:

1. Öffnen Sie **[!UICONTROL Einstellungen] → [!UICONTROL Sicherheit und Anmeldung]**.
2. Aktivieren Sie **[!UICONTROL Entwicklermodus]**.

Die Plus-Schaltfläche auf der Plugins-Seite erstellt MCP-unterstützte Plugins erst, wenn der Developer Mode aktiviert ist. Siehe [ChatGPT-](https://developers.openai.com/api/docs/guides/developer-mode).

## MCP-Server-URL kopieren

In [!DNL LLM Apps]:

1. Öffnen Sie die App-Detailseite.
2. Suchen Sie **[!UICONTROL App testen]**.
3. Wählen **[!UICONTROL unter „Staging]** die Option **[!UICONTROL URL kopieren]** aus.

## Erstellen des Plug-ins

1. Öffnen Sie [chatgpt.com/plugins](https://chatgpt.com/plugins).
2. Wählen Sie auf **[!UICONTROL Registerkarte]** Plug-ins“ **+** neben dem Suchfeld aus.

   ![ChatGPT — Plugins-Seite](/help/assets/guide-onboarding-agent/chatgpt-plugins-page.png)

3. Geben **[!UICONTROL unter „Neues]**&quot; Folgendes ein:
   - **[!UICONTROL Name]** - der Plug-in-Name.
   - **[!UICONTROL Beschreibung]** — optional.
   - **[!UICONTROL Verbindung]** - Wählen Sie **[!UICONTROL Server-URL]** aus und fügen Sie die MCP-Server-URL ein.
   - **[!UICONTROL Authentifizierung]** — Wählen Sie **[!UICONTROL Keine Authentifizierung]** aus.
4. Wählen Sie **[!UICONTROL Ich verstehe und möchte fortfahren]**.
5. Wählen Sie **[!UICONTROL Erstellen]** aus.

   ![ChatGPT - Erstellen eines Plug-ins mit der MCP Server URL](/help/assets/guide-onboarding-agent/chatgpt-new-plugin.png)

6. Wählen Sie im Bestätigungsdialogfeld **[!UICONTROL Verbinden]** aus.

   ![ChatGPT — Verbinden Sie das neue Plug-in](/help/assets/guide-onboarding-agent/chatgpt-plugin-connect.png)

## Testen des Plug-ins

1. Neuen Chat starten.
2. Wählen Sie im Menü Plus die Option **[!UICONTROL Entwicklermodus]** und wählen Sie das Plug-in aus.
3. Stellen Sie eine Frage, die einer der generierten Aktionen entspricht. Beispiel: *Zeig mir Kaffee.*

![ChatGPT — generierte Antwort des LLM-App-Plug-ins](/help/assets/guide-onboarding-agent/chatgpt-generated-app.png)

Überprüfen Sie, ob:

- [!DNL ChatGPT] ruft die erwartete Aktion auf.
- Das Widget zeigt die erwarteten Beispieldaten an.
- Die Textantwort entspricht dem Widget.
- Widget-Steuerelemente funktionieren erwartungsgemäß.

## Wie geht es weiter

- [Anpassen der generierten Widgets](/help/guides/widgets.md).
- [Erstellen einer Aktion von Grund auf](/help/guides/create-action.md).
