---
title: Fehlerbehebung bei Adobe LLM-Apps
description: Beheben Sie allgemeine Probleme mit Repositorys, Onboarding, Handlern, Widgets, der Bereitstellung und dem ChatGPT-Plug-in.
source-git-commit: eec74b87457bc852d7a8dd0e46c2a4385a93ae0a
workflow-type: tm+mt
source-wordcount: '632'
ht-degree: 1%

---


# Fehlerbehebung {#troubleshooting}

>[!IMPORTANT]
>
>[!DNL Adobe LLM Apps] befindet sich derzeit in Beta.
>
>Die hier gezeigten Funktionen, Workflows und Benutzeroberflächen stellen nicht unbedingt den endgültigen Status des Produkts dar. Um Beta beizutreten, senden Sie eine E-Mail an llm-apps-beta@adobe.com.

Beginnen Sie mit dem Symptom, das Sie sehen können. Geben Sie während der Fehlerbehebung keine Anmeldeinformationen, Token, privaten MCP-URLs oder vertraulichen Handler-Ergebnisse frei.

## App-Erstellung und Onboarding

| Symptom | Was Sie versuchen sollten |
|---------|-------------|
| Neue Repositorys werden nicht angezeigt | Wählen Sie **Repositorys auf GitHub verwalten**, gewähren Sie der Adobe LLM Apps GitHub App Zugriff auf beide Repositorys, kehren Sie zum Dialogfeld zurück und aktualisieren Sie die Listen |
| Das EDS-Repository erfordert die Synchronisierung von AEM-Code | Installieren Sie AEM Code Sync für das EDS-Repository und kehren Sie dann zum Dialogfeld „LLM-App erstellen“ zurück |
| Die EDS-Validierung besagt, dass Sie kein Administrator sind | Wählen Sie **AEM Live Admin öffnen**, fügen Sie sich selbst als Administrator für die EDS-Site hinzu und aktualisieren Sie dann das Repository |
| Onboarding wird noch erstellt | Die Wartezeit beträgt ca. 15 Minuten. Sie können die Seite verlassen und später zurückkehren |
| Onboarding-Berichte schlagen fehl | Vergewissern Sie sich, dass beide Repositorys verfügbar sind und dass die Website über HTTPS öffentlich ist, und wenden Sie sich mit der angezeigten Fehlermeldung an das Beta-Team |

## Aktionen und Handler

| Symptom | Was Sie versuchen sollten |
|---------|-------------|
| Aktion wird nicht aufgerufen | Fügen Sie das ChatGPT-Plug-in an, bestätigen Sie **dass das KI-Modell verfügbar**, verbessern Sie die Aktionsbeschreibung und stellen Sie Metadatenänderungen erneut bereit |
| Leere oder Fehlerantwort | Führen Sie `npm test` aus und rufen Sie dann den Handler mit MCP Inspector oder `curl` auf. Siehe [Lokale Handler-Entwicklung und -Tests](/help/reference/development.md) |
| Handler funktioniert lokal, aber nicht nach der Bereitstellung | Bestätigen Sie, dass der letzte Commit gesendet wurde, die Laufzeitkonfiguration vorhanden ist und die Aktionscode-Kennung mit `actions/<code-identifier>/index.js` übereinstimmt |
| Erzeugte Aktion kann nicht als geprüft markiert werden | Generierung von Handler- und Widget-Bestätigung erfolgreich. Überprüfen Sie die generierten Pull-Anfragen auf Zusammenführungskonflikte, laden Sie die Aktion neu und wählen Sie **Als geprüft markieren** erneut aus |

## Widgets

| Symptom | Was Sie versuchen sollten |
|---------|-------------|
| Widget wird nicht dargestellt | Überprüfen der Skript-URL, Widget-URL, HTTPS, EDS-Veröffentlichung, CSP-Domains und CORS-Header |
| Widget wird gerendert, zeigt aber keine Daten an | Rufen Sie den Handler mit MCP Inspector auf und vergleichen Sie seine `structuredContent` Form mit den aus `bridge.toolResult` gelesenen Feldern |
| Widget funktioniert in der direkten Vorschau, aber nicht in ChatGPT | Die direkte Vorschau kann Beispieldaten verwenden. Testen des bereitgestellten Handler-Ergebnisses und Überprüfen, ob der EDS-Ursprung durch CORS und CSP zulässig ist |
| Browser-Anfrage ist blockiert | Fügen Sie nur den erforderlichen Ursprung zum richtigen CSP-Feld hinzu und stellen Sie erneut bereit |
| HTTP-Kopfzeilen-Editor kann die Konfiguration nicht speichern | Verwenden Sie den [AEM-Konfigurations](https://aem.live/docs/config-service-setup)Service oder bitten Sie den EDS-Administrator, die Site-Header-Konfiguration zu initialisieren |

Protokollieren Sie keine vollständigen `bridge.toolResult`, wenn diese personenbezogene oder vertrauliche Daten enthalten können.

## Bereitstellung

| Symptom | Was Sie versuchen sollten |
|---------|-------------|
| Bereitstellung schlägt während **Vorbereitung** fehl | Überprüfen Sie, ob das Handler-Repository verknüpft ist und Ihr Adobe Developer Console-Zugriff weiterhin gültig ist. |
| Bereitstellung schlägt während **Build-App** fehl | Führen Sie `npm install`, `npm test` und `npm run build` lokal aus. Beheben von Abhängigkeits-, Syntax- oder Testfehlern und Übertragen der Änderungen |
| Bereitstellung erfolgreich, Änderungen fehlen jedoch | Bestätigen Sie, dass der erwartete Commit gepusht wurde, und stellen Sie ihn in derselben Umgebung erneut bereit |
| Aktion verbleibt **Nicht bereitgestellt** | Erneutes Bereitstellen nach Überprüfung der Aktion oder Ändern ihrer Metadaten |

## ChatGPT-Plug-ins

| Symptom | Was Sie versuchen sollten |
|---------|-------------|
| Plug-in wird nicht angezeigt | Aktivieren Sie den Entwicklermodus, öffnen Sie [chatgpt.com/plugins](https://chatgpt.com/plugins), überprüfen Sie, ob das Plug-in vorhanden ist, und wählen Sie **Verbinden** |
| Erstellung des Plug-ins schlägt fehl | Bestätigen Sie, dass der Entwicklermodus aktiviert ist, kopieren Sie die MCP-Server-URL erneut aus **Programm testen** und verwenden Sie **Server-URL** mit **Keine Auth** |
| Plug-in verbindet, kann aber keine Aktionen aufrufen | Bestätigen Sie, dass das Plug-in an den Chat angehängt ist, die Aktionen dem Modell bereitgestellt werden und die neueste Version bereitgestellt wird |
| Das Plug-in verwendet die falsche Umgebung | Bearbeiten Sie das Plug-in oder erstellen Sie es mit der vorgesehenen Staging- oder Produktions-MCP-Server-URL neu. |

Wenn das Problem weiterhin besteht, notieren Sie sich den App-Namen, die Umgebung, den fehlgeschlagenen Schritt, die Zeit und die sichtbare Fehlermeldung, bevor Sie sich an das Beta-Team wenden. Geheime Daten oder vertrauliche Kundendaten nicht einschließen.
