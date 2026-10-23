---
title: Bereitstellen der App
description: Erfahren Sie, wie Sie Ihre Adobe-LLM-App über die Benutzeroberfläche für LLM-Apps für die Staging- und Produktionsumgebung bereitstellen.
source-git-commit: 557173e8d95325b980efac4d72c06bb3d90dcd72
workflow-type: tm+mt
source-wordcount: '444'
ht-degree: 0%
---

# Bereitstellen der App {#deploy-your-app}

>[!IMPORTANT]
>
>[!DNL Adobe LLM Apps] befindet sich derzeit in Beta.
>
>Die hier gezeigten Funktionen, Workflows und Benutzeroberflächen stellen nicht unbedingt den endgültigen Status des Produkts dar. Um Beta beizutreten, senden Sie eine E-Mail an llm-apps-beta@adobe.com.

Nachdem Sie generierte Aktionen überprüft oder Ihren eigenen Handler-Code an das verknüpfte Repository gepusht haben, stellen Sie die App über [!DNL LLM Apps] bereit. Überprüfen Sie für generierte Aktionen die generierte Arbeit und führen Sie sie vor der Bereitstellung zusammen.

**In diesem Handbuch:** Erfahren Sie, wie Sie Ihre App für die Staging-Umgebung bereitstellen, das Bereitstellungsergebnis überprüfen und die MCP-Server-URL zum Testen kopieren. Nach erfolgreichem Testen in der Produktion bereitstellen.

## Starten der Bereitstellung

Öffnen Sie die App-Detailseite und wählen Sie **[!UICONTROL Bereitstellen]** aus.

Wählen Sie die Zielumgebung und dann **[!UICONTROL Bereitstellen]** aus.

Wenn Ihre Handler [App-Variablen](/help/guides/app-variables.md) verwenden, konfigurieren Sie diese vor der Bereitstellung für die Zielumgebung. Hinzugefügte, aktualisierte oder gelöschte Variablen werden auf diese Bereitstellung wirksam. Staging- und Produktionsdaten haben unabhängige Werte.

![Bereitstellen - Wählen Sie die Zielumgebung aus](/help/assets/guide-onboarding-agent/deploy-stage.png)

Die Bereitstellung erfolgt in vier Schritten:

1. **Vorbereiten** - ruft die Konfiguration ab, die zur Bereitstellung der App erforderlich ist.
2. **Bereitstellung starten** - Startet den Bereitstellungsprozess im Hintergrund.
3. **Programm erstellen** - Installiert Abhängigkeiten und erstellt den neuesten Repository-Code.
4. **Veröffentlichen** - Veröffentlicht die App in [!DNL Adobe I/O Runtime].

>[!NOTE]
>
>Wenn eine Aktion Metadaten in der Benutzeroberfläche, aber keine übereinstimmende Handler-Datei im Repository hat, registriert die Bereitstellung die Aktion weiterhin. Aufrufe verwenden einen Standard-Stub-Handler, bis Sie den echten Code hinzufügen.

## Nach erfolgreicher Bereitstellung

Wenn alle Schritte abgeschlossen sind, wird im Dialogfeld **Bereitstellung erfolgreich** angezeigt.

Um das Dialogfeld zu schließen, wählen Sie **Schließen** aus. Scrollen Sie auf der Seite mit den App-Details zu **[!UICONTROL App testen]**:

![Anwendungsdetails - Kopieren Sie die MCP-Server-URL](/help/assets/guide-onboarding-agent/app-mcp-url.png)

Jede bereitgestellte Umgebung zeigt eine MCP Server URL an. Wählen Sie **[!UICONTROL URL kopieren]** und verwenden Sie diese, um ein Plug-in oder einen Connector in der Ziel-LLM-Plattform zu erstellen.

Im Abschnitt **Bereitstellungsverlauf** werden die letzten 10 Bereitstellungen angezeigt:

![Bereitstellungsverlauf](/help/assets/guide-deploy/deployment-history.png)

Jede Zeile zeigt die Zielumgebung **&#x200B;**&#x200B;(Staging- oder Produktionsumgebung), **Status** (Erfolg oder Fehlgeschlagen) und **bereitgestellt**. Verwenden Sie die Tabelle, um die Bereitstellungsdaten zu verfolgen und sicherzustellen, dass die neueste Bereitstellung erfolgreich war.

## Konfigurationsänderungen bereitstellen

Änderungen an Aktionsmetadaten, den „Sichtbarkeit“-**[!UICONTROL der App]** Seite, der Authentifizierung, den App-Variablen und den Analytics-Einstellungen werden bei der nächsten Bereitstellung in dieser Umgebung wirksam. Beim Speichern einer Einstellung wird der bereitgestellte MCP-Server nicht automatisch aktualisiert.

Überprüfen Sie die Zielumgebung jedes Mal. Bei der Bereitstellung im Staging werden nicht dieselben Änderungen in der Produktion bereitgestellt.

Wenn Ihre App nicht die Analytics-Laufzeit- oder SDK-Anforderungen erfüllt, befolgen Sie die Anweisungen unter [Vorhandene App aktualisieren](/help/guides/upgrade-existing-app.md) bevor Sie sie erneut bereitstellen.

## Nächster Schritt

- [Testen Sie die bereitgestellte App als ChatGPT-Plug-in](/help/guides/test-in-chatgpt.md).
- [Testen Sie die bereitgestellte App als einen Claude-Connector](/help/guides/test-in-claude.md).
- [Senden Sie Ereignisse Ihrer App an Adobe Analytics oder Customer Journey Analytics](/help/guides/analytics-setup.md).
