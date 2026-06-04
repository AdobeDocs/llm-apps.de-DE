---
title: Fehlerbehebung
description: Lösungen für häufige Probleme beim Erstellen, Bereitstellen und Testen von Adobe LLM-Apps.
source-git-commit: c0f4affd586e77379f5c79731c7aed2c7a5d5d20
workflow-type: tm+mt
source-wordcount: '435'
ht-degree: 0%

---


# Fehlerbehebung

>[!IMPORTANT]
>
>**Haftungsausschluss:** Dies ist eine Beta-Version von [!DNL LLM Apps]. Die hier gezeigten Funktionen, Workflows und Benutzeroberflächen stellen nicht unbedingt den endgültigen Status der Anwendung oder des Produkts dar.

## Häufige Probleme

| Symptom | Mögliche Ursache | Was Sie versuchen sollten |
|---------|----------------|-------------|
| App wird nicht in der LLM-Plattform angezeigt | Ihr LLM-Plattformabonnement unterstützt keine benutzerdefinierten MCP-Apps oder der Entwicklermodus ist nicht aktiviert | Überprüfen Sie, ob Ihr Plan benutzerdefinierte MCP-Apps unterstützt. Aktivieren des Entwicklermodus in **Einstellungen → Apps → Erweiterte Einstellungen** |
| Fehler „Verbindung fehlgeschlagen“ in der LLM-Plattform | Die MCP-Server-URL ist falsch oder die Bereitstellung ist fehlgeschlagen | Überprüfen Sie die URL auf der Seite mit den App-Details. Überprüfen des Bereitstellungsverlaufs auf Fehler |
| Aktion wird nicht aufgerufen | Die LLM-Plattform konnte die Frage des Benutzers Ihrer Aktion nicht zuordnen | Verwenden Sie `@YourApp` , um es explizit aufzurufen. Verbessern Sie die Aktionsbeschreibung, damit das Modell den Intent abgleichen kann. |
| Widget wird nicht dargestellt | EDS-Widget-URLs oder CSP-Domains sind falsch konfiguriert | Überprüfen Sie die Skript-URL und die Widget-Einbettungs-URL im Dialogfeld Aktion erstellen . Überprüfen Sie, ob die CSP-Ressource und die Connect-Domains Ihren EDS-Ursprung enthalten. |
| Leere oder Fehlerantwort | Der Handler hat einen Fehler oder fehlt | Testen Sie zuerst lokal mit `npm start`. Siehe [Lokale Entwicklung](/help/reference/development.md#local-development) |
| Widget wird geladen, aber es werden keine Daten angezeigt | Die `structuredContent` Form entspricht nicht den Erwartungen des Blocks | Melden Sie `bridge.toolResult` in der `decorate` Ihres Blocks an und vergleichen Sie sie mit der Handler-Ausgabe |
| Bereitstellung schlägt bei „Klonen und erstellen“ fehl | `npm install`- oder Webpack-Build-Fehler in Ihrem Repository | `npm install && npm run build` lokal ausführen, um den Fehler zu reproduzieren |
| Bereitstellung schlägt bei „Sammeln von Anmeldeinformationen“ fehl | Repository nicht verknüpft oder Developer Console-Projekt falsch konfiguriert | Überprüfen, ob das Repository auf der Seite mit den App-Detaileinstellungen verknüpft ist |
| CORS-Fehler beim Laden des Widgets | EDS-Website fehlt `access-control-allow-origin` Kopfzeilen | Konfigurieren von CORS-Headern über `admin.hlx.page` |
| HTTP-Header-Editor gibt beim Speichern von CORS-Headern `404 Error updating config: config not found` zurück | In der Site-Konfiguration fehlt ein `headers` Abschnitt | Siehe [Initialisieren des Abschnitts „EDS-Site-Konfigurations-Header“](#initialize-the-eds-site-config-headers-section) unten |
| Widget wird in der Vorschau gerendert, jedoch nicht auf der LLM-Plattform | Der Block greift im Vorschaumodus auf Beispieldaten zurück, schlägt jedoch mit Live-Daten fehl | Testen Sie mit echten `structuredContent` mit dem MCP-Inspektor oder Curl |

## Initialisieren des Abschnitts „EDS-Site-Konfigurations-Header“

Wenn der HTTP-Header-Editor `404 Error updating config: config not found` zurückgibt, fehlt in der Site-Konfiguration ein `headers`. Beheben Sie das Problem manuell:

1. Navigieren Sie zu [tools.aem.live/tools/headers-edit/index.html](https://tools.aem.live/tools/headers-edit/index.html), geben Sie Ihre Organisation und Site ein und klicken Sie auf **[!UICONTROL Abrufen]**.
2. Öffnen Sie im Browser DevTools (Registerkarte „Netzwerk„) und kopieren Sie den Wert des `x-auth-token`-Headers aus der Abrufanfrage.
3. Abrufen der aktuellen Site-Konfiguration:

   ```bash
   curl -H "x-auth-token: $TOKEN" \
     https://admin.hlx.page/config/<your-github-org>/sites/<your-eds-repo>.json > config.json
   ```

4. Öffnen Sie `config.json` und fügen Sie dem JSON-Objekt `"headers": {}` hinzu.
5. POST die aktualisierte Konfiguration zurück:

   ```bash
   curl -X POST \
     -H "x-auth-token: $TOKEN" \
     -H "Content-Type: application/json" \
     -d @config.json \
     "https://admin.hlx.page/config/<your-github-org>/sites/<your-eds-repo>.json"
   ```

6. Laden Sie den Kopfzeilen-Editor neu und speichern Sie die `Access-Control-Allow-Origin` Kopfzeile wie gewohnt.

