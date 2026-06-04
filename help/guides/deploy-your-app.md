---
title: Bereitstellen der App
description: Erfahren Sie, wie Sie Ihre Adobe-LLM-App über die Benutzeroberfläche für LLM-Apps für die Staging- und Produktionsumgebung bereitstellen.
source-git-commit: 483a71f5f1de5caf1bd89b26f4d67d2d5a0aa15a
workflow-type: tm+mt
source-wordcount: '354'
ht-degree: 0%

---


# Bereitstellen der App

>[!IMPORTANT]
>
>**Haftungsausschluss:** Dies ist eine Beta-Version von [!DNL LLM Apps]. Die hier gezeigten Funktionen, Workflows und Benutzeroberflächen stellen nicht unbedingt den endgültigen Status der Anwendung oder des Produkts dar.

Nachdem Sie Ihren Handler-Code geschrieben und an Ihr verknüpftes Repository gesendet haben, können Sie die App über die [!DNL LLM Apps]-Benutzeroberfläche bereitstellen.

## Starten der Bereitstellung

Navigieren Sie zur App-Detailseite. Klicken Sie auf **[!UICONTROL Bereitstellen]** in der oberen rechten Ecke:

![App-Details - bereit zur Bereitstellung](/help/assets/guide-deploy/app-detail-deploy-ready.png)

Dadurch wird das Bereitstellungsdialogfeld geöffnet. Wählen Sie die Zielumgebung aus dem Dropdown-Menü aus:

![Dialogfeld „Bereitstellen“ — Wählen Sie die Zielumgebung](/help/assets/guide-deploy/deploy-pipeline-dropdown.png)

Klicken Sie **[!UICONTROL Bereitstellen]**, um die Pipeline zu starten. Die vier Schritte sind:

1. **Anmeldeinformationen sammeln** - liest App-Metadaten, generiert ein [!DNL GitHub]-Token und ruft Laufzeitanmeldeinformationen von der Konsolen-API ab.
2. **Trigger-Build-Pipeline** - Sendet alle Parameter an die Build-Pipeline.
3. **Klonen und erstellen** - Die Pipeline klont Ihr Repository, generiert `actions.json` aus den Metadaten der Benutzeroberfläche, führt `npm install` und webpack aus, um `dist/index.js` zu generieren.
4. **Für die Laufzeit bereitstellen** - Stellt das Bundle im [!DNL Adobe I/O Runtime] Namespace Ihrer App bereit.

Nach dem Start wird die Pipeline automatisch ausgeführt und zeigt den Echtzeitfortschritt an:

![Pipeline-Ausführung bereitstellen](/help/assets/guide-deploy/deploy-pipeline-deploying.png)

>[!NOTE]
>
>Wenn eine Aktion Metadaten in der Benutzeroberfläche, aber keine übereinstimmende Handler-Datei im Repository hat, ist sie weiterhin registriert. Aufrufe verwenden einen Standard-Stub-Handler, bis Sie den echten Code hinzufügen.

## Nach erfolgreicher Bereitstellung

Wenn alle Schritte abgeschlossen sind, zeigt das Dialogfeld eine Bestätigung **Bereitstellung erfolgreich** mit den bereitgestellten URL- und Artefaktdetails an:

![Bereitstellung erfolgreich](/help/assets/guide-deploy/app-detail-deploy-finish.png)

Klicken Sie **Schließen**, um das Dialogfeld zu schließen. Scrollen Sie auf der Seite mit den App **[!UICONTROL Details zum Abschnitt]** Testen der App“:

![Testen der App - bereitgestellte URLs](/help/assets/guide-deploy/test-app-deployed.png)

Jede Umgebung (**Staging** und **Produktion** zeigt die MCP-Server-URL auf [!DNL Adobe I/O Runtime] an. Dies ist die URL, die Sie der LLM-Plattform bei der Registrierung Ihrer App bereitstellen. Klicken Sie **URL kopieren**, um sie in die Zwischenablage zu kopieren.

Der **Bereitstellungsverlauf** unten enthält ein vollständiges Protokoll jeder Bereitstellung in allen Umgebungen:

![Bereitstellungsverlauf](/help/assets/guide-deploy/deployment-history.png)

Jede Zeile zeigt die Zielumgebung **&#x200B;**&#x200B;(Staging- oder Produktionsumgebung), **Status** (Erfolg oder Fehlgeschlagen) und das Datum **bereitgestellt am** an. Sie können diese Tabelle verwenden, um zu verfolgen, wann Bereitstellungen stattgefunden haben, und sicherzustellen, dass die
Die letzte Bereitstellung war erfolgreich.

