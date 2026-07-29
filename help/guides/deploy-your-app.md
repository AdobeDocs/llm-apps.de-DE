---
title: Bereitstellen der App
description: Erfahren Sie, wie Sie Ihre Adobe-LLM-App über die Benutzeroberfläche für LLM-Apps für die Staging- und Produktionsumgebung bereitstellen.
source-git-commit: eec74b87457bc852d7a8dd0e46c2a4385a93ae0a
workflow-type: tm+mt
source-wordcount: '309'
ht-degree: 0%

---


# Bereitstellen der App {#deploy-your-app}

>[!IMPORTANT]
>
>[!DNL Adobe LLM Apps] befindet sich derzeit in Beta.
>
>Die hier gezeigten Funktionen, Workflows und Benutzeroberflächen stellen nicht unbedingt den endgültigen Status des Produkts dar. Um Beta beizutreten, senden Sie eine E-Mail an llm-apps-beta@adobe.com.

Nachdem Sie Ihren Handler-Code geschrieben und an Ihr verknüpftes Repository gesendet haben, können Sie die App über die [!DNL LLM Apps]-Benutzeroberfläche bereitstellen.

Dies ist ein freigegebener Schritt für jede Journey. Fahren Sie nach der Bereitstellung mit [Testen des ChatGPT-Plug-ins](/help/guides/test-in-chatgpt.md) fort.

## Starten der Bereitstellung

Öffnen Sie die App-Detailseite und wählen Sie **[!UICONTROL Bereitstellen]** aus.

Wählen Sie die Zielumgebung und dann **[!UICONTROL Bereitstellen]** aus.

![Bereitstellen - Wählen Sie die Zielumgebung aus](/help/assets/guide-onboarding-agent/deploy-stage.png)

Die Bereitstellung erfolgt in vier Schritten:

1. **Vorbereiten** - ruft die Konfiguration ab, die zur Bereitstellung der App erforderlich ist.
2. **Bereitstellung starten** - Startet den Bereitstellungsprozess im Hintergrund.
3. **Programm erstellen** - Installiert Abhängigkeiten und erstellt den neuesten Repository-Code.
4. **Veröffentlichen** - Veröffentlicht die App in [!DNL Adobe I/O Runtime].

![Bereitstellen - Bereitstellungs-Pipeline wird ausgeführt](/help/assets/guide-onboarding-agent/deploy-running.png)

>[!NOTE]
>
>Wenn eine Aktion Metadaten in der Benutzeroberfläche, aber keine übereinstimmende Handler-Datei im Repository hat, ist sie weiterhin registriert. Aufrufe verwenden einen Standard-Stub-Handler, bis Sie den echten Code hinzufügen.

## Nach erfolgreicher Bereitstellung

Wenn alle Schritte abgeschlossen sind, wird im Dialogfeld **Bereitstellung erfolgreich** angezeigt.

![Bereitstellen - erfolgreiche Bereitstellung](/help/assets/guide-onboarding-agent/deploy-successful.png)

Klicken Sie **Schließen**, um das Dialogfeld zu schließen. Scrollen Sie auf der Seite mit den App **[!UICONTROL Details zum Abschnitt]** Testen der App“:

![Anwendungsdetails - Kopieren Sie die MCP-Server-URL](/help/assets/guide-onboarding-agent/app-mcp-url.png)

Jede bereitgestellte Umgebung zeigt eine MCP Server URL an. Wählen Sie **[!UICONTROL URL kopieren]** und verwenden Sie diese, um ein Plug-in in der Ziel-LLM-Plattform zu erstellen.

Im Abschnitt **Bereitstellungsverlauf** werden die letzten 10 Bereitstellungen angezeigt:

![Bereitstellungsverlauf](/help/assets/guide-deploy/deployment-history.png)

Jede Zeile zeigt die Zielumgebung **&#x200B;**&#x200B;(Staging- oder Produktionsumgebung), **Status** (Erfolg oder Fehlgeschlagen) und das Datum **bereitgestellt am** an. Sie können diese Tabelle verwenden, um zu verfolgen, wann Bereitstellungen stattgefunden haben, und sicherzustellen, dass die
Die letzte Bereitstellung war erfolgreich.

## Nächster Schritt

[Testen Sie die bereitgestellte App als ChatGPT-Plug-in](/help/guides/test-in-chatgpt.md).

