---
title: Testen der LLM-App als Claude-Connector
description: Erstellen Sie einen Claude-Connector aus Ihrer Adobe LLM Apps MCP-Server-URL und testen Sie ihn in einer Konversation.
source-git-commit: 0f9dd0a5b1008e183eb8ab5a4c2eae34a78923bb
workflow-type: tm+mt
source-wordcount: '676'
ht-degree: 0%
---

# Testen der LLM-App als [!DNL Claude] Connector {#test-in-claude}

>[!IMPORTANT]
>
>[!DNL Adobe LLM Apps] befindet sich derzeit in Beta.
>
>Die hier gezeigten Funktionen, Workflows und Benutzeroberflächen stellen nicht unbedingt den endgültigen Status des Produkts dar. Um Beta beizutreten, senden Sie eine E-Mail an llm-apps-beta@adobe.com.

Nach der Bereitstellung stellt Ihre LLM-App eine MCP-Server-URL bereit. Fügen Sie diese URL zu [!DNL Claude] als benutzerdefinierten Connector hinzu und testen Sie dann die generierten Aktionen und Widgets.

**In diesem Handbuch** Erfahren Sie, wie Sie Ihre bereitgestellte App als [!DNL Claude]-Connector hinzufügen, ihr Anmeldeverhalten auswählen und ihre Aktionen und Widgets in einer Konversation testen.

Bei einer App mit öffentlichen und geschützten Aktionen unterstützt [!DNL Claude] verzögerte Anmeldung, bis eine geschützte Aktion aufgerufen wird. Eine App, deren Aktionen alle Authentifizierung erfordern, muss vor der Verwendung angemeldet werden. Siehe [Endbenutzer bei Ihrem eigenen Identitätsanbieter authentifizieren](/help/guides/authentication.md).

## Plananforderungen

Benutzerdefinierte Connectoren mit Remote-MCP sind kostenlos für die Pläne Pro, Max, Team und Enterprise verfügbar. Freie Konten unterstützen einen benutzerdefinierten Connector. In Team und Enterprise fügt ein autorisierter Eigentümer den Connector für die Organisationsmitglieder hinzu, bevor diese eine Verbindung herstellen. Benutzerdefinierte Unternehmensrollen können diese Berechtigung ebenfalls erteilen.

Planungs[ und Organisationssteuerelemente finden Sie in ](https://claude.com/docs/connectors/custom/add-unlisted)Claude&#39;s aktuellen benutzerdefinierten Connector-Anweisungen . Der Remote-MCP-Endpunkt muss von der Claude-Infrastruktur aus erreichbar sein, nicht nur über Ihren lokalen Browser.

## MCP-Server-URL kopieren

In [!DNL LLM Apps]:

1. Öffnen Sie die App-Detailseite.
2. Suchen Sie **[!UICONTROL App testen]**.
3. Wählen **[!UICONTROL unter „Staging]** die Option **[!UICONTROL URL kopieren]**.

## Hinzufügen des benutzerdefinierten Connectors

1. Öffnen Sie [Anpassen > Connectoren](https://claude.ai/customize/connectors). Bei einem organisationsweiten Connector wird stattdessen ein autorisierter Eigentümer geöffnet [Organisationseinstellungen > Connectoren](https://claude.ai/admin-settings/connectors).
2. Wählen Sie **[!UICONTROL Benutzerdefinierten Connector hinzufügen]** oder **[!UICONTROL Hinzufügen]** > **[!UICONTROL Benutzerdefiniert]** > **[!UICONTROL Web]** für den Organisationsfluss aus.
3. Geben Sie den Connector-Namen ein, falls angefordert, und fügen Sie die vollständige HTTPS-MCP-Server-URL ein.
4. Wenn das Dialogfeld mehrere Schritte umfasst, wählen Sie **[!UICONTROL Fortfahren]** und überprüfen Sie die erkannten Authentifizierungseinstellungen:
   - **[!UICONTROL Keine Anmeldung]** für eine öffentliche App mit deaktivierter Authentifizierung.
   - **[!UICONTROL Bei Bedarf anmelden]** für eine App mit öffentlichen und geschützten Aktionen.
   - **[!UICONTROL Jetzt anmelden]** um die Anmeldung vor der Verwendung des Connectors anzufordern.
5. Wählen Sie für OAuth eine Client-Identitätsmethode aus, die von Ihrem Identitätsanbieter unterstützt wird. Wenn Sie einen vorregistrierten Client verwenden, geben Sie dessen Details nur in den OAuth-Einstellungen des Connectors ein, nicht in den URL- oder Aktionsmetadaten.
6. Wählen Sie **[!UICONTROL Hinzufügen]** aus. Für einen Organisations-Connector wählt dann jedes Mitglied **[!UICONTROL Verbinden]** aus seiner eigenen Connector-Liste aus.

Einige Konten zeigen ein Dialogfeld mit **[!UICONTROL erweiterten Einstellungen]** auf einem Bildschirm an, während andere einen mehrstufigen Fluss anzeigen. Verwenden Sie die [aktuellen Plattformanweisungen](https://claude.com/docs/connectors/custom/add-unlisted#choose-authentication-settings) wenn Ihr Layout unterschiedlich ist.

>[!NOTE]
>
>Verwenden Sie nur Connectoren von vertrauenswürdigen Entwicklern. Anthropic kontrolliert nicht, welche Tools Entwickler zur Verfügung stellen oder garantiert, dass diese Tools wie vorgesehen funktionieren oder unverändert bleiben.

## Die generierten Tools zulassen

[!DNL Claude] listet jede generierte Aktion unter **[!UICONTROL Tool-Berechtigungen]** auf der Connector-Seite auf. Für neue Tools ist standardmäßig **[!UICONTROL Genehmigung erforderlich]** festgelegt, bei der Sie aufgefordert werden, jeden Aufruf während des Tests zu genehmigen.

Lassen Sie Genehmigungsaufforderungen für Aktionen aktiviert, die Daten ändern oder vertrauliche Vorgänge ausführen. Verwenden Sie **[!UICONTROL Immer zulassen]** nur für bestimmte vertrauenswürdige Aktionen mit geringem Risiko, wenn dies für Ihren Test geeignet ist.

## Testen des Connectors

1. Neuen Chat starten.
2. Wählen Sie **+** im Meldungsfeld aus (oder geben Sie `/` ein), bewegen Sie den Mauszeiger **[!UICONTROL Connectoren]** und aktivieren Sie den Connector, den Sie für diese Konversation hinzugefügt haben.
3. Stellen Sie eine Frage, die einer der generierten Aktionen entspricht. Beispiel: *Zeig mir Kaffee.*

Überprüfen Sie, ob:

- [!DNL Claude] ruft die erwartete Aktion auf.
- Das Widget zeigt die erwarteten Beispieldaten an.
- Die Textantwort entspricht dem Widget.
- Widget-Steuerelemente funktionieren erwartungsgemäß.

Für gemischte Authentifizierung rufen Sie zunächst eine öffentliche Aktion auf, ohne sich anzumelden. Rufen Sie dann eine geschützte Aktion auf, schließen Sie die Anmeldeaufforderung ab und bestätigen Sie, dass dieselbe Anfrage anschließend erfolgreich ist. Testen Sie auch fehlende Berechtigungen und ungültige Eingaben.

Stellen Sie nach dem Ändern der App die betroffenen Tests erneut bereit und wiederholen Sie sie. Wenn Sie die gespeicherten Authentifizierungseinstellungen eines Connectors ändern müssen, müssen [!DNL Claude] möglicherweise den Connector entfernen und erneut hinzufügen.

## Wie geht es weiter

- [Anpassen der generierten Widgets](/help/guides/widgets.md).
- [Erstellen einer Aktion von Grund auf](/help/guides/create-action.md).
