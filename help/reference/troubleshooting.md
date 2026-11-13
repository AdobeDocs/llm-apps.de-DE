---
title: Fehlerbehebung bei Adobe LLM-Apps
description: Beheben von Problemen mit der allgemeinen Infrastruktur, der Aktionserstellung, dem Handler, dem Widget, der Bereitstellung, der Authentifizierung und der LLM-Plattformverbindung.
source-git-commit: 9b2ec369baba5d533088d2dd4adefcb80f5c9727
workflow-type: tm+mt
source-wordcount: '1602'
ht-degree: 0%
---

# Fehlerbehebung {#troubleshooting}

>[!IMPORTANT]
>
>LLM-Apps stehen allen AEM Sites Cloud Service-Kunden zur Entwicklung und Verwaltung eigener Apps zur Verfügung. Sobald die Erfassung beginnt, wird jeder App-Aufruf für die AEM Sites-Inhaltsanfragen des Kunden angerechnet. Adobe gibt die Pipettierungsrate und das Startdatum vor dem Inkrafttreten der Pipettierung an.

Beginnen Sie mit dem beobachtbaren Symptom. Geben Sie während der Fehlerbehebung keine Anmeldeinformationen, Token, privaten MCP-URLs oder vertraulichen Handler-Ergebnisse frei.

## App-Erstellung und Aktionserstellung

| Symptom | Was Sie versuchen sollten |
|---------|-------------|
| [!DNL LLM Apps] Einrichtung der verwalteten Infrastruktur schlägt fehl | Lesen Sie den Fehler unter **Setup fehlgeschlagen** und wählen Sie dann **Wiederholen** aus. Wenn die Seite den Fortschritt nicht überprüfen kann, wählen Sie **Erneut überprüfen**; die Einrichtung wird im Hintergrund fortgesetzt |
| Kein AEM-Programm verfügbar | Bitten Sie Ihren Administrator um Programmzugriff. Wenn das Laden fehlgeschlagen ist, wählen Sie **Programme erneut laden** |
| Neue Repositorys werden nicht angezeigt | Fügen Sie das Repository dem Cloud Manager-Programm als privates Repository hinzu, warten Sie, bis der Status **Bereit** ist, und öffnen Sie dann die Repository-Liste erneut. Von Adobe in Cloud Manager gehostete Repositorys werden nicht unterstützt |
| Keine Edge Delivery Services-Site verknüpft | Ein Widget-Repository, das sich nicht auf GitHub befindet, muss mit einer EDS-Site in Cloud Manager mit Bring Your Own Git verknüpft sein. Verknüpfen Sie es und wählen Sie dann erneut **Repository**. Siehe [Eigenes Git mitbringen](https://www.aem.live/developer/byo-git) |
| Die Repository-Validierung der Aktion oder des Widgets schlägt fehl | Verwenden Sie ein kompatibles vorhandenes Projekt oder ein leeres Repository. Separate Repositorys für Aktionen und Widgets verwenden und dann erneut **Repository überprüfen** auswählen |
| AEM-Site-Setup oder Zugriffsvalidierung schlägt fehl (GitHub-Widget-Repository) | Folgen Sie [Einrichten des Site-Zugriffs für AEM](/help/guides/create-app.md#aem-site-access). Für eine neue Code-Sync-Installation schließen Sie den Bestätigungsseiten-Assistenten ab. Behalten Sie für eine bestehende Site die Installation bei und bitten Sie einen Site-Administrator, Ihrem Adobe-Konto über den AEM-Benutzeradministrator Zugriff `admin` gewähren. Vergewissern Sie sich, dass die Code-Synchronisierung das Widget-Repository abdeckt, und wählen Sie dann **AEM-Setup validieren** |
| DAC-Schreibzugriff ist erforderlich | Bitten Sie einen DAM-Organisationsadministrator, dem in LLM-Apps verwendeten Adobe-Konto Schreibzugriff auf alle Inhalte der Site zu gewähren. Befolgen Sie [Schreibzugriff für DAM gewähren](/help/guides/create-app.md#da-write-access), melden Sie sich mit diesem Konto an und wählen Sie dann **AEM-Setup validieren** aus. AEM-Site-Rollen gewähren keine DAM-Berechtigungen |
| Zugriff auf AEM oder DAM konnte nicht verifiziert werden | Versuchen Sie **AEM-Setup validieren** erneut, um einen temporären Überprüfungsfehler zu ermitteln. Wenn die Nachricht stattdessen eine Zugriffsverweigerung meldet, korrigieren Sie die AEM- oder DAM-Berechtigungen anhand der obigen Anweisungen, bevor Sie es erneut versuchen |
| Beim Erstellen der App wurden keine Aktionen generiert | Bei der App-Erstellung wird nur die Infrastruktur eingerichtet. Wählen Sie auf der App-Seite **Aktionen automatisch erstellen**, geben Sie **Ihre Website** ein und wählen Sie **Meine Aktionen erstellen**. Siehe [Erstellen von Aktionen](/help/guides/creating-actions.md) |
| Aktionen werden noch generiert | Die Benutzeroberfläche schätzt, dass bis zu 30 Minuten für die Generierung benötigt werden. Den Fortschritt auf der Seite Aktionen verfolgen oder die Seite verlassen und später zurückkehren |
| Fehlgeschlagene Erzeugung von Aktionsberichten | Vergewissern Sie sich, dass die Website über HTTPS öffentlich ist und dass der Repository-Zugriff für Ihre eigenen Repositorys weiterhin gültig ist. Wenden Sie sich mit der sichtbaren Fehlermeldung an das LLM Apps-Team, wenn das Problem weiterhin besteht |

## Aktionen und Handler

| Symptom | Was Sie versuchen sollten |
|---------|-------------|
| Aktion wird nicht aufgerufen | Aktivieren Sie das Plug-in oder den Connector der App in der Konversation. Bestätigen Sie **dass das KI** Modell verfügbar gemacht werden soll, wenn die Aktionseinstellungen und der **Sichtbarkeitsschalter** der Aktion auf der App-Seite aktiviert sind. Verbessern der Beschreibung und erneutes Bereitstellen von Metadatenänderungen |
| Leere oder Fehlerantwort | Führen Sie `npm test` aus und rufen Sie dann den Handler mit MCP Inspector oder `curl` auf. Siehe [Lokale Handler-Entwicklung und -Tests](/help/reference/development.md) |
| Handler funktioniert lokal, aber nicht nach der Bereitstellung | Überprüfen Sie den neuesten Push-Commit, bestätigen Sie die Laufzeitkonfiguration und überprüfen Sie, ob die Aktionscode-Kennung mit `actions/<code-identifier>/index.js` übereinstimmt |
| Erzeugte Aktion kann nicht als geprüft markiert werden | Generierung von Handler- und Widget-Bestätigung erfolgreich. Überprüfen Sie die generierten Pull-Anforderungen oder Verzweigungen auf Zusammenführungskonflikte, laden Sie die Aktion neu und wählen Sie **Als überprüft**. Wenn Sie bei der Überprüfung aufgefordert werden, eine Verzweigung selbst zusammenzuführen, führen Sie sie zuerst mit Ihrem üblichen Prozess zusammen |

## Widgets

| Symptom | Was Sie versuchen sollten |
|---------|-------------|
| Widget wird nicht dargestellt | Überprüfen der Skript-URL, Widget-URL, HTTPS, EDS-Veröffentlichung, CSP-Domains und CORS-Header |
| Widget wird gerendert, zeigt aber keine Daten an | Rufen Sie den Handler mit MCP Inspector auf und vergleichen Sie seine `structuredContent` Form mit den aus `bridge.toolResult` gelesenen Feldern |
| Widget funktioniert in der direkten Vorschau, aber nicht in ChatGPT | Die direkte Vorschau kann Beispieldaten verwenden. Testen des bereitgestellten Handler-Ergebnisses und Überprüfen, ob der EDS-Ursprung durch CORS und CSP zulässig ist |
| Browser-Anfrage ist blockiert | Fügen Sie nur den erforderlichen Ursprung zum richtigen CSP-Feld hinzu und stellen Sie erneut bereit |
| HTTP-Kopfzeilen-Editor kann die Konfiguration nicht speichern | Verwenden Sie den [AEM-Konfigurations](https://aem.live/docs/config-service-setup)Service oder bitten Sie den EDS-Administrator, die Site-Header-Konfiguration zu initialisieren |

Protokollieren Sie keine vollständigen `bridge.toolResult`, wenn diese personenbezogene oder vertrauliche Daten enthalten können.

## ChatGPT-Erweiterung

Informationen zu fehlenden Einstiegspunkten, Problemen mit dem Datei-Viewer, ungültigen Symbol-URLs, Anzeigeeinstellungen oder Composer-Erwähnungsfehlern finden Sie unter [Fehlerbehebung bei ChatGPT-Erweiterungen](/help/guides/chatgpt-extensions.md#troubleshooting). Überprüfen Sie den Client-Support, bevor Sie die App ändern: Eine funktionierende Verbindung bedeutet nicht, dass jede Erweiterung in diesem Client unterstützt wird.

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
| Anmeldung wird nicht gestartet | Vergewissern Sie sich, dass Sie die App bereitgestellt haben, nachdem Sie die Authentifizierung aktiviert haben, und dass für die Aktion **[!UICONTROL Erforderlich“]**. Eine **[!UICONTROL optionale]** Aktion fordert nur dann zur Eingabe auf, wenn ihr Handler zur Anmeldung auffordert |
| Die Plattform sendet den Benutzer zur falschen Anmeldeseite | Überprüfen Sie **[!UICONTROL Aussteller]**, ob dies genau mit der Aussteller-URL Ihres Identitätsanbieters übereinstimmt und ob es über öffentliches HTTPS erreichbar ist |
| Die Anmeldung ist erfolgreich, aber jeder Aufruf wird weiterhin abgelehnt | Bestätigen Sie, dass Ihr Identitätsanbieter Token ausgibt, deren Zielgruppe die MCP-Server-URL der App für diese Umgebung ist und dass das Token ein JWT ist, das mit einem asymmetrischen Algorithmus signiert ist. Siehe [Token-Anforderungen](/help/reference/authentication-reference.md#token-requirements) |
| Der Identitätsanbieter erhält nie Traffic | Die Discovery-, Autorisierungs- und Token-Endpunkte Ihres Anbieters müssen über öffentliches HTTPS erreichbar sein. Suchen Sie nach einer Firewall, einem WAF oder einer IP-Zulassungsliste davor - Ihr Programm kann erreichbar sein, während Ihr Provider nicht erreichbar ist |
| Der Identitätsanbieter weigert sich, ein Token für die angeforderte Zielgruppe auszugeben | Einige Anbieter geben nur Token für eine Ressource aus, die bei ihnen registriert ist. Bestätigen Sie, dass die MCP-Server-URL der App als Ressourcenkennung bei Ihrem Identitätsanbieter registriert ist |
| Ein neu hinzugefügter Bereich wird bei der Anmeldung nicht angefordert | LLM-Plattformen speichern die veröffentlichten Metadaten der App für einige Minuten zwischen. Warten Sie und versuchen Sie dann erneut, sich anzumelden |
| Der Benutzer wird aufgefordert, sich für einen Bereich erneut anzumelden | Die Aktion erfordert einen Bereich, den das Token nicht enthält. Fügen Sie den Bereich zum Gewähren durch den Client in Ihrem Identitätsanbieter hinzu oder entfernen Sie ihn aus der Aktion |
| Der Benutzer erhält wiederholte Anmeldungsaufforderungen | Ein Handler, der bei jedem Aufruf `extra.challengeAuth()` zurückgibt, ohne die Identitätsanfragen des Aufrufers zu überprüfen, meldet sich nach der Authentifizierung erneut an. Authentifizierung nur anfordern, wenn die für die Aktion benötigte Identität fehlt |
| Die Anmeldung wird abgelehnt, bevor der Benutzer Ihren Identitätsanbieter erreicht | Der von der LLM-Plattform gesendete Umleitungs-URI ist nicht bei Ihrem Autorisierungs-Server registriert. Einige Plattformen geben für jeden Connector ein anderes Problem aus. Registrieren Sie daher den Wert, der auf dem Bildschirm „Connector-Setup“ angezeigt wird |
| Speichern ist mit einer nicht unterstützten Bereichsmeldung blockiert | Fügen Sie den Bereich zu **[!UICONTROL Unterstützte Bereiche]** hinzu oder entfernen Sie ihn aus der Aktion, die ihn erfordert |
| Eine öffentliche Aktion fordert weiterhin zur Anmeldung bei [!DNL Claude] auf | Vergewissern Sie sich, dass der Connector **[!UICONTROL Bei Bedarf anmelden]** und nicht **[!UICONTROL Jetzt anmelden]** verwendet. Aktualisieren Sie die Laufzeit für eine ältere bereitgestellte App und stellen Sie sie erneut bereit. Bei einer gemischten App zur aktuellen Laufzeit bleiben öffentliche Aktionen anonym aufrufbar |
| Eine **[!UICONTROL Optional]** Aktion gibt Anmeldetext, aber keine Claude-Anmelde-Eingabeaufforderung zurück | `extra.challengeAuth()` ist eine ChatGPT Tool-Result Challenge. Verwenden Sie für ein mobiles Anmelde-Gate eine **[!UICONTROL Erforderlich]** Aktion, damit die Laufzeit die von Claude erwartete HTTP-Herausforderung zurückgibt |
| Die App fordert zur Anmeldung auf, obwohl jede Aktion &quot;**[!UICONTROL &quot;]** | Deaktivieren **[!UICONTROL Authentifizierung aktivieren]** und bereitstellen. Während sie aktiviert ist, gibt die App einen Autorisierungs-Server an, selbst wenn keine Aktion ausgelöst wird |

Fügen Sie keine Zugriffstoken, Anspruchssätze oder Client-Geheimnisse des Identitätsanbieters in eine Support-Anfrage ein.

## ChatGPT-Plug-ins

| Symptom | Was Sie versuchen sollten |
|---------|-------------|
| Plug-in wird nicht angezeigt | Öffnen Sie [chatgpt.com/plugins](https://chatgpt.com/plugins), bestätigen Sie, dass Ihr Konto oder Arbeitsbereich benutzerdefinierte MCP-Server zulässt, und installieren oder verbinden Sie das resultierende Plug-in |
| Erstellung des Plug-ins schlägt fehl | Kopieren Sie die vollständige HTTPS-MCP-Server-URL erneut aus **Testen Sie die App** und wählen Sie die Authentifizierungsmethode aus, die der App entspricht. Verwenden Sie **Keine Auth** nur für eine öffentliche App, nicht als Problemumgehung für fehlgeschlagene OAuth |
| Plug-in verbindet, kann aber keine Aktionen aufrufen | Bestätigen Sie, dass das Plug-in an den Chat angehängt ist, die Aktionen dem Modell bereitgestellt werden und die neueste Version bereitgestellt wird |
| Das Plug-in verwendet die falsche Umgebung | Bearbeiten Sie das Plug-in oder erstellen Sie es mit der vorgesehenen Staging- oder Produktions-MCP-Server-URL neu. |
| Plug-in zeigt weiterhin alte Aktionsmetadaten an | Stellen Sie die Änderungen bereit, wählen **Aktualisieren** für die ChatGPT-Verbindung aus und beginnen Sie dann eine neue Konversation |

Wenn das Problem weiterhin besteht, notieren Sie sich den App-Namen, die Umgebung, den fehlgeschlagenen Schritt, die Zeit und die sichtbare Fehlermeldung, bevor Sie sich an das LLM Apps-Team wenden. Geheime Daten oder vertrauliche Kundendaten nicht einschließen.
