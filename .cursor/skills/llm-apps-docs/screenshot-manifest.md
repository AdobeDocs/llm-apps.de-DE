---
source-git-commit: 03c918b1643d9c4e8ebee40fd67694acb6751a14
workflow-type: tm+mt
source-wordcount: '1080'
ht-degree: 0%
---
# Screenshot-Manifest

Posteingang erfassen: `docs-captures/<YYYY-MM-DD>/`

Nur Checkpoints erfassen, die dem Benutzer wesentlich dabei helfen, eine Entscheidung zu treffen oder den Status zu überprüfen.

Die Dateinamen von Source müssen nicht mit den endgültigen Dateinamen übereinstimmen. Die Qualifikation ordnet Screenshots nach sichtbarem UI-Status zu, behält die Rohdateien bei und erstellt bereinigte Kopien mit den unten stehenden Namen.

Jedes nachstehende Handbuch deklariert ein eigenes Ausgabeverzeichnis. Verwenden Sie die Datei für den Abschnitt, zu dem die Aufnahme gehört.

# Onboarding-Handbuch

Ausgabeverzeichnis: `help/assets/guide-onboarding-agent/`

## Erforderliche Aufnahmen

### `app-details-onboarding.png`

- Status: App-Name, Analytics-Region und **Meine App automatisch erstellen** ausgewählt.
- Einschließen: App-Details, Analytics-Region und Beginn der Erstellung meiner App.
- Alt-Text: `Create LLM App — app details and Build My App enabled`

### `install-aem-code-sync.png`

- Status: Leeres EDS-Repository mit AEM-Textbaustein initialisiert; AEM-Codesynchronisierung erforderlich.
- Einschließen: EDS-Repository-Validierungsmeldung und Installationslink.
- Alt-Text: `Create LLM App — empty EDS repository initialized and AEM Code Sync required`

### `eds-admin-required.png`

- Status: AEM Code Sync installiert, der aktuelle Benutzer ist jedoch kein EDS Site-Administrator.
- Einschließen: die vollständige Validierungsmeldung und **AEM Live Admin öffnen**.
- Alt-Text: `Create LLM App — EDS administrator access required`

### `actions-generating.png`

- Status: Seite „Aktionen“, während das Onboarding aktiv ist.
- Einschließen: Schritte zum Versand der Fortschrittsmeldung und Generierung.
- Alt-Text: `Actions — generating recommendations`

### `actions-ready-for-review.png`

- Status: Generierte Aktionsliste nach Abschluss des Onboarding und vor der Genehmigung.
- Einschließen: Aktionsnamen, generierter/Überprüfungsstatus und Überprüfungssteuerung.
- Nur Spielinhalte verwenden.
- Alt-Text: `Actions — generated actions ready for review`

### `generated-action-review.png`

- Status: Eine repräsentative generierte Aktion.
- Einschließen: Navigation zu Aktions- und Widget-Metadaten, Generierungsergebnis des Handlers und **Als geprüft markieren**.
- Maske: Repository-Besitzer, falls erforderlich.
- Alt-Text: `Generated action — ready to mark as reviewed`

### `actions-reviewed.png`

- Status: Alle generierten Aktionen wurden überprüft.
- Einschließen: **Alle Aktionen werden geprüft**, Aktionsabzeichen und **Zur App-Seite wechseln**.
- Alt-Text: `Actions — all generated actions reviewed`

### `deploy-stage.png`

- Status: Das Dialogfeld „Bereitstellung“ vor dem Start.
- Einschließen: Staging-Zielumgebung und **Bereitstellen**.
- Alt-Text: `Deploy — select the Stage environment`

### `deploy-running.png`

- Status: Bereitstellungs-Pipeline wird ausgeführt.
- Einschließen: Schritte zum Vorbereiten, Starten, Erstellen und Veröffentlichen.
- Alt-Text: `Deploy — deployment pipeline running`

### `deploy-successful.png`

- Status: Erfolgreiche Staging-Bereitstellung.
- Einschließen: Umgebung und Erfolgsstatus.
- Maske: Laufzeitnamespace, vollständige MCP-URL, IDs, Zeitstempel bei der Identifizierung.
- Alt-Text: `Deploy — successful staging deployment`

### `app-mcp-url.png`

- Status: Testen Sie den App-Abschnitt nach der Bereitstellung.
- Dazu gehören: Staging-Umgebung **„URL kopieren** und erfolgreicher Bereitstellungsverlauf.
- Maske: Die MCP Server URL.
- Alt-Text: `App Detail — copy the staging MCP server URL`

### `chatgpt-plugins-page.png`

- Status: ChatGPT Plugins-Seite.
- Einschließen: Registerkarte „Plug-ins“, Schaltfläche „Suchen“ und „Erstellen“.
- Alt-Text: `ChatGPT — Plugins page`

### `chatgpt-new-plugin.png`

- Status: Dialogfeld Neues Plug-in.
- Einschließen: Name, Beschreibung, Server-URL, Authentifizierung, Bestätigung und Erstellen.
- Maske: Die MCP Server URL.
- Alt-Text: `ChatGPT — create a plugin with the MCP server URL`

### `chatgpt-plugin-connect.png`

- Status: Bestätigung nach der Erstellung des Plug-ins.
- Einschließen: **Hinzufügen <plugin> zu ChatGPT **und** Connect **.
- Maske: Browser-URL und Connector-IDs.
- Alt-Text: `ChatGPT — connect the new plugin`

### `chatgpt-generated-app.png`

- Status: In ChatGPT aufgerufenes Fixture-Plug-in.
- Einschließen: angehängte App, generiertes Widget und Textantwort.
- Ausschließen: Konversationsverlauf, Kontoname und nicht verwandte Apps.
- Alt-Text: `ChatGPT — generated LLM App plugin response`

## Optionale Aufnahmen

Fügen Sie nur dann eine Aufzeichnung hinzu, wenn die Entscheidung in der Prosa nicht klar erklärt werden kann:

- Repository-Zugriff für die GitHub-App.
- Status für fehlgeschlagenes Onboarding zur Fehlerbehebung.
- Plug-in-Symbol hochladen.

Fügen Sie keine Screenshots für statische Feldlisten hinzu, die in der Prosa bereits klar sind.

# Authentifizierungshandbuch

Ausgabeverzeichnis: `help/assets/guide-authentication/`

Referenziert von [authentication.md](../../../help/guides/authentication.md).

Der **[!UICONTROL „Ressourcenkennung kopieren]** verwendet die des Onboarding-Handbuchs
`app-mcp-url.png`. Nicht erneut erfassen.

Jede Aufzeichnung in diesem Abschnitt zeigt die Sicherheitskonfiguration. Vor dem Speichern maskieren:

- Die **[!UICONTROL Aussteller]**-URL und alle Hostnamen, die den Identitätsanbieter oder dessen Anbieter identifizieren.
- Die URL des MCP-Servers in voller Länge, wo auch immer sie angezeigt wird.
- Mandanten-, Client- und Organisationskennungen.
- Kontoname, Avatar und E-Mail.

Verwenden Sie neutrale Platzhalterwerte, wenn ein Feld lesbar bleiben muss, z. B. einen Herausgeber von
`https://auth.example.com`. Bereichsnamen sollten als allgemeine Beispiele wie `orders:read` gelesen werden.

## Erforderliche Aufnahmen

### `auth-core-settings.png`

- Status: **[!UICONTROL Einstellungen]** > **[!UICONTROL Authentifizierung]** mit **[!UICONTROL Authentifizierung aktivieren]** und **[!UICONTROL Core-Einstellungen]** ausgefüllt.
- Einschließen: Die **[!UICONTROL Workspace]**-Auswahl zeigt **[!UICONTROL Staging]**, **[!UICONTROL Authentifizierung aktivieren]** im eigenen Status, **[!UICONTROL Aussteller]** und **[!UICONTROL Unterstützte Bereiche]** mit mindestens zwei Bereichen.
- Schließen Sie das reduzierte Steuerelement **[!UICONTROL Erweiterte Einstellungen]** ein, damit der Leser sehen kann, dass **[!UICONTROL JWKS-URI]** optional ist und wo er sich befindet.
- Maske: Der Hostname des Ausstellers.
- Alt-Text: `Authentication — enable authentication and complete the core settings`

Aufgenommen am 25.08.2026. Beschnitten, um die leere Arbeitsfläche abzulegen. Keine Maskierung erforderlich, da
**[!UICONTROL Aussteller]** wurde vor dem auf im Produkt `https://auth.example.com` gesetzt
Erfassung. Ziehen Sie dies vor, um das Bild anschließend zu bearbeiten. **[!UICONTROL Unterstützte Bereiche]** gilt für
Ein Bereich (`read:all`); zwei würden das Feld besser veranschaulichen, aber dies ist kein Wert
Eigenständig neu erfassen.

### `auth-per-action.png`

- Status: **[!UICONTROL Konfiguration pro Aktion]** nach Aktivierung der Authentifizierung, wobei die Modi absichtlich gemischt werden.
- Einschließen: mindestens drei Aktionen, eine pro Modus - **[!UICONTROL Keine]**, **[!UICONTROL Erforderlich]** und **[!UICONTROL Optional]** - und die Spalte **[!UICONTROL Bereiche]**, die in den gated es ausgefüllt ist.
- Einschließen: **[!UICONTROL Authentifizierung für alle Aktionen erfordern]** idealerweise in seinem unbestimmten Status, der durch eine gemischte Konfiguration erzeugt wird.
- Verwenden Sie nur Namen für Spannvorrichtungen.
- Alt-Text: `Authentication — set an auth mode and scopes for each action`

Aufgenommen am 25.08.2026. Nur zugeschnitten, nichts zu maskieren. Zeigt alle drei Modi an, a ausgefüllt
**[!UICONTROL Bereiche]** Zelle und **[!UICONTROL Authentifizierung für alle Aktionen erforderlich]** in
Unbestimmter Zustand, mit `Test Action 1/2/3` als Vorrichtungsnamen.

Beschneiden **innen** den eigenen Container-Rahmen des Einstellungsbedienfelds - eine 1 px-Regel voller Höhe befindet sich an jedem Rand.
Die Seite der Aufnahme und das Belassen einer der beiden im Frame liest sich als eine verirrte Linie entlang der Kante des
Bild.

Die eigene Warnung des Produkts zur Anwendung [!DNL Claude] Authentifizierung pro Connector lautete
**Wird auf dieser Registerkarte in zwei Erfassungsrunden nicht beachtet** Daher ist er hier nicht erforderlich.  
Der Guide gibt an, dass stattdessen das Verhalten in Prosa ist. Wenn die Warnung in einem späteren Build vorhanden ist,
als `auth-claude-warning.png` erfassen und einen Eintrag hinzufügen.

### `chatgpt-authentication-mode.png`

- Status: Das Dialogfeld **[!UICONTROL Neues Plug]** mit dem **[!UICONTROL Authentifizierung]**-Dropdown wird geöffnet.
- Schließen Sie alle drei Werte - **[!UICONTROL Keine Auth]**, **[!UICONTROL Gemischt]** und **[!UICONTROL OAuth]** - ein, damit die Zuordnungstabelle im Handbuch mit der echten Steuerung verglichen werden kann.
- Maske: Die MCP-Server-URL und jede Connector-Kennung in der Browser-URL.
- Alt-Text: `ChatGPT — select the authentication mode for the plugin`

Führen Sie eine Rahmendarstellung auf die gleiche Weise durch wie die `chatgpt-new-plugin.png` des Onboarding-Handbuchs: die Dialogfeldkarte mit
Ein Rand der Seite noch um sie herum sichtbar, ungefähr 40px links und oben. Nicht auf Flush zuschneiden
Die Karte.

Aufgenommen am 25.08.2026, Lichtmodus, um jede andere Aufnahme in der Dokumentation zu berücksichtigen.  
das Feld **[!UICONTROL Server-URL]** verschließt, sodass die MCP-URL nicht lesbar ist — aber
Durch das durchscheinende Material kann neben dem Feld ein unscharfes Bild des Feldinhalts entweichen
Optionen. Die drei nicht hervorgehobenen Zeilen wurden mit dem Ausfüllen des Bedienfelds und ihren Beschriftungen neu aufgetragen
neu gerendert, wodurch es entfernt wird. Überprüfen Sie die Blutung durch eine Probenahme, nicht mit dem Auge: Die Blutung ist schwach genug, um
verpassen und es ist die MCP Server URL.

Beachten Sie, dass das Live-Steuerelement **vier** Werte bietet - **[!UICONTROL OAuth]**, **[!UICONTROL Access
Token/API-]**, **[!UICONTROL Keine]** und **[!UICONTROL Gemischt]**. Die Zuordnung des Handbuchs
Die -Tabelle behandelt nur die drei Authentifizierungsmodi einer App, die zugeordnet werden können, was richtig ist, aber nicht
Beschreiben Sie das Dropdown-Menü mit drei Optionen.

## Optionale Aufnahmen

Nur hinzufügen, wenn die Prosa nicht ausreicht:

- `auth-scope-blocked.png` - **[!UICONTROL Speichern]** blockiert, weil für eine Aktion ein Bereich fehlt (**[!UICONTROL Bereiche unterstützt]**. Nützlich für den Eintrag zur Fehlerbehebung.
- Bei der Eingabeaufforderung zur Anmeldung während der Konversation wird eine **[!UICONTROL optionale]** Aktion ausgelöst. Die plattformeigene Benutzeroberfläche, die sich häufig ändert, wird bereits prosa beschrieben.

Erfassen Sie nicht die eigene Anmeldeseite des Identitätsanbieters. Er identifiziert den Anbieter, den diese Dokumentation nicht benennt.
