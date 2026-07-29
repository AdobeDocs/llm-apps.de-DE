---
title: Testen der LLM-App als Claude-Connector
description: Erstellen Sie einen Claude-Connector aus Ihrer Adobe LLM Apps MCP-Server-URL und testen Sie ihn in einer Konversation.
source-git-commit: bb3d8a02f22a91ceeeba5999453aeb4221060f80
workflow-type: tm+mt
source-wordcount: '399'
ht-degree: 1%

---


# Testen der LLM-App als [!DNL Claude] Connector {#test-in-claude}

>[!IMPORTANT]
>
>[!DNL Adobe LLM Apps] befindet sich derzeit in Beta.
>
>Die hier gezeigten Funktionen, Workflows und Benutzeroberflächen stellen nicht unbedingt den endgültigen Status des Produkts dar. Um Beta beizutreten, senden Sie eine E-Mail an llm-apps-beta@adobe.com.

Nach der Bereitstellung stellt Ihre LLM-App eine MCP-Server-URL bereit. Fügen Sie diese URL zu [!DNL Claude] als benutzerdefinierten Connector hinzu und testen Sie dann die generierten Aktionen und Widgets.

Dies ist der letzte Überprüfungsschritt nach dem Erstellen, Anpassen oder Erweitern einer App.

## Plananforderungen

Benutzerdefinierte Connectoren mit Remote-MCP sind auf [!DNL Claude]-, [!DNL Claude] Desktop- und Cowork-Plänen kostenlos, Pro-, Max-, Team- und Enterprise-Plänen verfügbar. Kostenlose Plankonten sind auf einen benutzerdefinierten Connector beschränkt. Für Team- und Enterprise-Organisationen muss ein Eigentümer oder ein Primärer Eigentümer Connectoren aktivieren, bevor andere Mitglieder sie verwenden können.

## MCP-Server-URL kopieren

In [!DNL LLM Apps]:

1. Öffnen Sie die App-Detailseite.
2. Suchen Sie **[!UICONTROL App testen]**.
3. Wählen **[!UICONTROL unter „Staging]** die Option **[!UICONTROL URL kopieren]** aus.

## Hinzufügen des benutzerdefinierten Connectors

1. Öffnen Sie [claude.ai/new?modal=add-custom-connector](https://claude.ai/new?modal=add-custom-connector#settings/customize-connectors). Dadurch wird das Dialogfeld **[!UICONTROL Benutzerdefinierten Connector hinzufügen]** direkt geöffnet.
2. Geben Sie Folgendes ein:
   - **[!UICONTROL Name]** - der Connector-Name.
   - **[!UICONTROL Remote-MCP-Server-URL]** - Die kopierte MCP-Server-URL.
3. Wählen Sie **[!UICONTROL Hinzufügen]** aus.

   ![Claude - Dialogfeld „Benutzerdefinierten Connector hinzufügen“](/help/assets/guide-test-claude/claude-add-custom-connector.png)

>[!NOTE]
>
>Verwenden Sie nur Connectoren von Entwicklern, denen Sie vertrauen. Anthropic kontrolliert nicht, welche Tools Entwickler zur Verfügung stellen und kann nicht überprüfen, ob sie wie beabsichtigt funktionieren oder ob sie sich nicht ändern.

## Die generierten Tools zulassen

Jede generierte Aktion wird unter **[!UICONTROL Tool-Berechtigungen]** auf der Connector-Seite aufgeführt. Standardmäßig sind neue Tools auf „Genehmigung erforderlich **[!UICONTROL festgelegt]** wodurch Sie während des Tests aufgefordert werden, jeden Aufruf zu genehmigen.

Stellen Sie jedes Tool — oder die gesamte **[!UICONTROL Interaktive Tools]**-Gruppe — auf **[!UICONTROL Immer zulassen]** ein, damit das Testen nicht durch Genehmigungsaufforderungen unterbrochen wird.

![Claude — Setzen Sie Tool-Berechtigungen auf Immer zulassen](/help/assets/guide-test-claude/claude-tool-permissions.png)

## Testen des Connectors

1. Neuen Chat starten.
2. Wählen Sie **+** im Meldungsfeld aus (oder geben Sie `/` ein), bewegen Sie den Mauszeiger **[!UICONTROL Connectoren]** und aktivieren Sie den Connector, den Sie für diese Konversation hinzugefügt haben.

   ![Claude - Aktivieren Sie den Connector für die Konversation](/help/assets/guide-test-claude/claude-enable-connector-chat.png)

3. Stellen Sie eine Frage, die einer der generierten Aktionen entspricht. Beispiel: *Zeig mir Kaffee.*

Überprüfen Sie, ob:

- [!DNL Claude] ruft die erwartete Aktion auf.
- Das Widget zeigt die erwarteten Beispieldaten an.
- Die Textantwort entspricht dem Widget.
- Widget-Steuerelemente funktionieren erwartungsgemäß.

## Wie geht es weiter

- [Anpassen der generierten Widgets](/help/guides/widgets.md).
- [Erstellen einer Aktion von Grund auf](/help/guides/create-action.md).
