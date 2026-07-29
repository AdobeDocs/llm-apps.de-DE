---
source-git-commit: eec74b87457bc852d7a8dd0e46c2a4385a93ae0a
workflow-type: tm+mt
source-wordcount: '398'
ht-degree: 0%

---
# Onboarding-Screenshot-Manifest

Posteingang erfassen: `docs-captures/<YYYY-MM-DD>/`

Ausgabeverzeichnis: `help/assets/guide-onboarding-agent/`

Nur Checkpoints erfassen, die dem Benutzer wesentlich dabei helfen, eine Entscheidung zu treffen oder den Status zu überprüfen.

Die Dateinamen von Source müssen nicht mit den endgültigen Dateinamen übereinstimmen. Die Qualifikation ordnet Screenshots nach sichtbarem UI-Status zu, behält die Rohdateien bei und erstellt bereinigte Kopien mit den unten stehenden Namen.

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
- Alt-Text: `Actions — Onboarding Agent generating recommendations`

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
- Einschließen: **Hinzufügen <plugin> zu ChatGPT &#x200B;** und **&#x200B; Connect &#x200B;**.
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
