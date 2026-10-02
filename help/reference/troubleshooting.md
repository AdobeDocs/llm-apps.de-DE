---
title: Fehlerbehebung bei Adobe LLM-Apps
description: Beheben Sie allgemeine Probleme mit Repositorys, Onboarding, Handlern, Widgets, der Bereitstellung und dem ChatGPT-Plug-in.
source-git-commit: fd41dbcabc4db0cae766de19cb042d7c85d8b7aa
workflow-type: tm+mt
source-wordcount: '1144'
ht-degree: 0%
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

## Authentifizierung {#authentication}

Trifft zu, wenn **[!UICONTROL Authentifizierung aktivieren]** aktiviert ist. Siehe [Endbenutzer bei Ihrem eigenen Identitätsanbieter authentifizieren](/help/guides/authentication.md).

| Symptom | Was Sie versuchen sollten |
|---------|-------------|
| Aktionen sind nach dem Speichern der Einstellungen weiterhin öffentlich | Stellen Sie die App erneut in dieser Umgebung bereit. Authentifizierungsänderungen werden bei der nächsten Bereitstellung wirksam |
| Einstellungen sehen nach dem Wechsel der Umgebung falsch aus | Bestätigen Sie, dass die Auswahl **[!UICONTROL Workspace]** die gewünschte Umgebung anzeigt. **[!UICONTROL Staging]** und **[!UICONTROL Produktion]** werden unabhängig konfiguriert |
| Anmeldung wird nicht gestartet | Vergewissern Sie sich, dass die App bereitgestellt wurde, seit Sie die Authentifizierung aktiviert haben, und dass die Aktion, die Sie aufrufen, auf **[!UICONTROL Erforderlich“]**. Eine **[!UICONTROL optionale]** Aktion fordert nur dann zur Eingabe auf, wenn ihr Handler zur Anmeldung auffordert |
| Die Plattform sendet den Benutzer zur falschen Anmeldeseite | Überprüfen Sie **[!UICONTROL Aussteller]**, ob dies genau mit der Aussteller-URL Ihres Identitätsanbieters übereinstimmt und ob es über öffentliches HTTPS erreichbar ist |
| Die Anmeldung ist erfolgreich, aber jeder Aufruf wird weiterhin abgelehnt | Bestätigen Sie, dass Ihr Identitätsanbieter Token ausgibt, deren Zielgruppe die MCP-Server-URL der App für diese Umgebung ist und dass das Token ein JWT ist, das mit einem asymmetrischen Algorithmus signiert ist. Siehe [Token-Anforderungen](/help/reference/authentication-reference.md#token-requirements) |
| Der Identitätsanbieter erhält nie Traffic | Die Discovery-, Autorisierungs- und Token-Endpunkte Ihres Anbieters müssen über öffentliches HTTPS erreichbar sein. Suchen Sie nach einer Firewall, einem WAF oder einer IP-Zulassungsliste davor - Ihr Programm kann erreichbar sein, während Ihr Provider nicht erreichbar ist |
| Der Identitätsanbieter weigert sich, ein Token für die angeforderte Zielgruppe auszugeben | Einige Anbieter geben nur Token für eine Ressource aus, die bei ihnen registriert ist. Bestätigen Sie, dass die MCP-Server-URL der App als Ressourcenkennung bei Ihrem Identitätsanbieter registriert ist |
| Ein neu hinzugefügter Bereich wird bei der Anmeldung nicht angefordert | LLM-Plattformen speichern die veröffentlichten Metadaten der App für einige Minuten zwischen. Warten Sie und versuchen Sie dann erneut, sich anzumelden |
| Der Benutzer wird aufgefordert, sich für einen Bereich erneut anzumelden | Die Aktion erfordert einen Bereich, den das Token nicht enthält. Fügen Sie den Bereich zum Gewähren durch den Client in Ihrem Identitätsanbieter hinzu oder entfernen Sie ihn aus der Aktion |
| Der Benutzer wird gebeten, sich in einer Schleife immer wieder anzumelden | Eine Aktion ist bei jedem Aufruf eine Herausforderung. Ein Handler, der `extra.challengeAuth()` zurückgibt, ohne vorher zu überprüfen, ob der Aufrufer bereits authentifiziert ist, kann nie zufrieden gestellt werden, da die erneute Anmeldung dieselbe Herausforderung erzeugt. Herausforderung nur, wenn die Identität, die die Aktion benötigt, fehlt |
| Die Anmeldung wird abgelehnt, bevor der Benutzer Ihren Identitätsanbieter erreicht | Der von der LLM-Plattform gesendete Umleitungs-URI ist nicht bei Ihrem Autorisierungs-Server registriert. Einige Plattformen geben für jeden Connector ein anderes Problem aus. Registrieren Sie daher den Wert, der auf dem Bildschirm „Connector-Setup“ angezeigt wird |
| Speichern ist mit einer nicht unterstützten Bereichsmeldung blockiert | Fügen Sie den Bereich zu **[!UICONTROL Unterstützte Bereiche]** hinzu oder entfernen Sie ihn aus der Aktion, die ihn erfordert |
| Bei einer Aktion mit **[!UICONTROL Keine]** wird weiterhin zur Anmeldung aufgefordert | Wird für [!DNL Claude] erwartet, die sich pro Connector und nicht pro Aktion authentifiziert. |
| Die App fordert zur Anmeldung auf, obwohl jede Aktion &quot;**[!UICONTROL &quot;]** | Deaktivieren **[!UICONTROL Authentifizierung aktivieren]** und bereitstellen. Während sie aktiviert ist, gibt die App einen Autorisierungs-Server an, selbst wenn keine Aktion ausgelöst wird |

Fügen Sie keine Zugriffstoken, Anspruchssätze oder Client-Geheimnisse des Identitätsanbieters in eine Support-Anfrage ein.

## ChatGPT-Plug-ins

| Symptom | Was Sie versuchen sollten |
|---------|-------------|
| Plug-in wird nicht angezeigt | Aktivieren Sie den Entwicklermodus, öffnen Sie [chatgpt.com/plugins](https://chatgpt.com/plugins), überprüfen Sie, ob das Plug-in vorhanden ist, und wählen Sie **Verbinden** |
| Erstellung des Plug-ins schlägt fehl | Bestätigen Sie, dass der Entwicklermodus aktiviert ist, kopieren Sie die MCP-Server-URL erneut aus **Programm testen** und verwenden Sie **Server-URL** mit **Keine Auth** |
| Plug-in verbindet, kann aber keine Aktionen aufrufen | Bestätigen Sie, dass das Plug-in an den Chat angehängt ist, die Aktionen dem Modell bereitgestellt werden und die neueste Version bereitgestellt wird |
| Das Plug-in verwendet die falsche Umgebung | Bearbeiten Sie das Plug-in oder erstellen Sie es mit der vorgesehenen Staging- oder Produktions-MCP-Server-URL neu. |

Wenn das Problem weiterhin besteht, notieren Sie sich den App-Namen, die Umgebung, den fehlgeschlagenen Schritt, die Zeit und die sichtbare Fehlermeldung, bevor Sie sich an das Beta-Team wenden. Geheime Daten oder vertrauliche Kundendaten nicht einschließen.
