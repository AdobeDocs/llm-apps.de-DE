---
source-git-commit: a48b4022979d9a13b4fc3aa870c4ee6375e8bc13
workflow-type: tm+mt
source-wordcount: '1052'
ht-degree: 1%
---
# Screenshot-Manifest

Diese Status mit der Browser-Automatisierung erfassen mit
[screenshots.md](screenshots.md). Auswahl der für den Artikel relevanten Checkpoints;
Dieses Manifest autorisiert nicht die Erstellung, Generierung, Genehmigung oder
Bereitstellung.

Rohaufnahmen bleiben in `docs-captures/<run-id>/`. Nur visuell überprüft veröffentlichen,
bereinigte PNGs unter `help/assets/`. Die Dateinamen unten beibehalten, wenn
Bilder werden aktualisiert.

## Einrichten der Infrastruktur

Artikel: `/help/guides/create-app.md`

Ausgabeverzeichnis: `help/assets/guide-infrastructure/`

| Dateiname | Status und nützlicher Kontext | Vorgeschlagener Alternativtext |
|---|---|---|
| `setup-choice.png` | Erste Schritte mit beiden sichtbaren Infrastrukturoptionen | Erstellen Sie Ihre LLM-App - wählen Sie Adobe verwaltete Infrastruktur oder Ihre eigenen Repositorys |
| `app-details.png` | App-Name, AEM-Programm, Analytics-Region und Warnung bei unveränderlichen Einstellungen | App-Details : Geben Sie einen Namen ein, wählen Sie ein AEM-Programm aus und wählen Sie eine Analytics-Region aus |
| `action-repository.png` | Pfad zum eigenen Repository mit ausgewähltem GitHub und Anweisungen zur Repository-Verbindung | Einrichtung der Aktionsinfrastruktur - Verbindung zu einem leeren oder kompatiblen Aktions-Repository herstellen |
| `provisioning.png` | Verwaltetes Setup für genehmigte Adobe wird ausgeführt | App-Einrichtung - Adobe bereitet die Infrastruktur vor |

Kontodetails, Organisations-IDs, echte Programmnamen, Repository ausschließen
Kennungen und App-IDs. Leere Felder oder einen neutralen Namen für die Vorrichtung verwenden, wobei
Möglich. Repository-Validierung kann Code initialisieren, daher vor dem
Erfassen eines Validierungsergebnisses.

## Erstellen von Aktionen und Überprüfen der generierten Arbeit

Artikel: `/help/guides/creating-actions.md`

Ausgabeverzeichnis: `help/assets/guide-creating-actions/`

| Dateiname | Status und nützlicher Kontext | Vorgeschlagener Alternativtext |
|---|---|---|
| `build-choice.png` | Buildaktionen werden automatisch ausgewählt, eine öffentliche Website wird eingegeben und eine stabile Startregelung wird hinzugefügt, falls vorhanden | Aktionen : Wählen Sie die automatische Generierung aus und geben Sie eine öffentliche Website ein. |
| `generating.png` | Ausführung der Generierung, Zeitschätzung und Wechseln zu Aktionen | Aktionen - Generierung wird ausgeführt |
| `actions-ready-for-review.png` | Namen von Vorrichtungsaktionen, Prüfabzeichen und Prüfsteuerelemente | Aktionen - generierte Aktionen sind zur Überprüfung bereit und werden nicht bereitgestellt. |
| `generated-action-review.png` | Details zu LLM-Widgets, Aktionsdetails, Analytics, ein generierter Pull-Anfrage-Hinweis und „Als geprüft markieren“ | Erzeugte Aktion - Überprüfen des Widgets und des Handlers vor der Genehmigung |

Private Repository maskieren und Anfrage-Links abrufen. Zuschneiden generierter persönlicher oder
Kontaktdetails, wenn der Artikel nur den Prüfungsstatus benötigt. Nicht auswählen
**Als geprüft markieren** ohne Genehmigung.

den Status aller geprüften Aktionen nur erfassen, wenn die Genehmigung im Umfang ist, und
Staat existiert wirklich. Generierte Arbeit nicht genehmigen, um einen Screenshot zu erstellen
Checkliste.

## Erstellen einer neuen Aktion

Artikel: `/help/guides/create-action.md`

Ausgabeverzeichnis: `help/assets/guide-create-action/`

| Dateiname | Status und nützlicher Kontext | Vorgeschlagener Alternativtext |
|---|---|---|
| `action-basic-info.png` | Suchen Sie nach Produkten, ihrer abgeleiteten Kennung, einer nützlichen Beschreibung und schreibgeschützten Anmerkungen | Neue Aktion erstellen - Grundlegende Informationen und Anmerkungen |
| `action-analytics-user-intent.png` | Erfassen der Benutzerabsicht und der Standardanweisungen für die anonymisierte Zusammenfassung | Neue Aktion erstellen - Benutzerabsichtssammlung überprüfen |
| `widget-metadata.png` | Edge Delivery Services-Blocktyp und sicherer Beispiel-Sandbox-Ursprung | Neue Aktion erstellen - Widget-Typ und Sandbox-Domain |
| `widget-template-urls.png` | Beispielskript und Widget-URLs | Neue Aktion erstellen - Skript- und Widget-URLs |
| `widget-permissions.png` | Optionale Browser-Berechtigungen, wobei nicht verwendete Berechtigungen deaktiviert sind | Neue Aktion erstellen - Browser-Berechtigungen |
| `widget-csp.png` | Domänensteuerelemente für Ressourcen, Verbindungen, Rahmen, Umleitung und Basis-URI | Neue Aktion erstellen - Domains mit Content Security Policy |

Das Erstellungsdialogfeld enthält die Registerkarten **Aktion** und **Widget-Metadaten**. Der gespeicherte Editor
verfügt über **Registerkarten** LLM-Widget-Details **,** Aktionsdetails **und Analytics**. Erfassen Sie die
Der Layout-Vorgang verwendet tatsächlich . Füllen Sie die Beispielfelder aus, ohne zu speichern, dann
Änderungen verwerfen.

## Authentifizierung

Artikel: `/help/guides/authentication.md`

Ausgabeverzeichnis: `help/assets/guide-authentication/`

| Dateiname | Status und nützlicher Kontext | Vorgeschlagener Alternativtext |
|---|---|---|
| `auth-core-settings.png` | Einstellungen für Staging, Authentifizierung aktiviert, `https://auth.example.com`, `catalog:read`, `profile:read` und Erweitert | Authentifizierung - Aktivierung der Authentifizierung und Vervollständigung der Kerneinstellungen |
| `auth-per-action.png` | Befestigungsaktionen, die „Keine“, „Erforderlich“, „Optional“ und einen zugewiesenen Bereich anzeigen | Authentifizierung - Wählen Sie für jede Aktion einen Authentifizierungsmodus und einen Gültigkeitsbereich aus. |

Nicht gespeicherte Einstellungen verwenden und anschließend verwerfen. Überprüfen, ob die Authentifizierung gespeichert wurde
bleibt unverändert. Der Schritt zur Ressourcenkennung verwendet
`help/assets/guide-onboarding-agent/app-mcp-url.png`.

Wenn in der Benutzeroberfläche eine Warnung angezeigt wird, die mit der bereitgestellten Laufzeit oder der aktuellen
Plattformverhalten, dokumentieren Sie die Diskrepanz. Es ist ein reiner Tabellenzuschnitt geeignet
Beim Unterrichten von Modi eine relevante Warnung jedoch nicht ausblenden, ohne sie zu erklären.

## Anwendungsvariablen

Artikel: `/help/guides/app-variables.md`

Ausgabeverzeichnis: `help/assets/guide-app-variables/`

Verwenden Sie `GREETING_PREFIX` mit `Good day` als Beispielwert und `Howdy` als
Ersatzwert. Erfasst nie einen echten Konfigurationswert.

| Dateiname | Status und nützlicher Kontext | Vorgeschlagener Alternativtext |
|---|---|---|
| `variables-empty.png` | Leerer Staging-Arbeitsbereich, Workspace-Auswahl und Hinzufügen | Variablen und Geheimnisse - leerer Staging-Arbeitsbereich |
| `add-variable-dialog.png` | Nicht gespeicherte Variable GRUSSBEGRÜSSUNGSPRÄFIX und aktueller geheimer Support-Hinweis | Variable oder Geheimnis hinzufügen - GRUSSWORT-PRÄFIX und Guten Tag eingeben |
| `variable-added.png` | Genehmigte Anlagenvariable mit Namen, Typ, Wert, Zeitstempel und Zeilenkontrollen | Variablen und Geheimnisse - GRUSSPRÄFIX im Staging-Arbeitsbereich |
| `update-variable-dialog.png` | Aktueller Wert Guter Tag und nicht gespeicherter neuer Wert Howdy | GRUSSWORT-PRÄFIX aktualisieren - Good day to Howdy ändern |
| `delete-variable-dialog.png` | Bestätigungsdialogfeld ohne Löschbestätigung | GREETING_PREFIX löschen - Überprüfen Sie die Löschbestätigung. |

Das Öffnen eines Dialogfelds erlaubt kein Speichern oder Löschen. Wiederverwenden eines sicheren vorhandenen
Fixturvariable oder -frage vor der Erstellung einer Variablen. Älteres Bild nur beibehalten nach
Es wird überprüft, ob die Felder und Steuerelemente der Benutzeroberfläche weiterhin mit der aktuellen übereinstimmen.

## Bereitstellung und Tests

Artikel: `/help/guides/deploy-your-app.md`

| Ausgabeverzeichnis | Dateiname | Status und nützlicher Kontext |
|---|---|---|
| `help/assets/guide-onboarding-agent/` | `deploy-stage.png` | Staging-Umgebung der Zielgruppe und Steuerung „Bereitstellen“ vor dem Start |
| `help/assets/guide-onboarding-agent/` | `app-mcp-url.png` | Aktuelle Karte, Umgebungsbeschriftungen und URL kopieren testen; jede Laufzeit-URL maskieren |
| `help/assets/guide-deploy/` | `deployment-history.png` | Spalten mit Umgebungs-, Status- und Bereitstellungsdatum ohne App-Kennungen |

Verwenden Sie eine schreibgeschützte vorhandene Bereitstellung für die Testkarte und den Verlauf, wenn zulässig.
Starten Sie keine Bereitstellung, um diese Status abzurufen, es sei denn, der Benutzer genehmigt sie.
Nur bei einem erfolgreichen Bereitstellungsdialogfeld oder bei einem erfolgreichen Bereitstellungsstatus erfassen
relevant und zugelassen.

Zum `/help/guides/test-in-chatgpt.md` wird die beibehalten
`help/assets/guide-onboarding-agent/chatgpt-generated-app.png` stellt ein Produkt dar
Antwort. Die aktuelle Setup-Benutzeroberfläche wird nicht überprüft. Neues ChatGPT- oder Claude-Setup
Screenshots erfordern aktuellen authentifizierten Zugriff; andernfalls verwenden sie einen offiziellen Anbieter
Anweisungen und überspringen nicht verifizierte Bilder.

## Vorhandene Herkunft erfassen

Der Erfassungsdurchgang 2026.10.2019 verwendete die von Benutzenden angeforderte lokale Benutzeroberfläche mit entsprechender Benutzeroberfläche/API
Quell-Checks. Sie führt keine Produktionsparität ein.

- Raw erfasst: `docs-captures/2026-10-19/` und sein `full-review/` Unterverzeichnis.
- Frühere oder ersetzte öffentliche Bilder verbleiben in den lokalen `retired/`-Unterverzeichnissen.
- Das Dokumentationsspiel erreichte die Überprüfungsphase der generierten Aktion. Keine Aktion
Genehmigungen, Pull-Anfrage-Zusammenführungen, Bereitstellungen oder gespeicherte Authentifizierung/Variable
Änderungen waren Teil dieses Erfassungsdurchgangs.
- Variablenzeilen-, Aktualisierungs- und Löschbilder aus einer früheren Ausführung wurden beibehalten, nachdem
Überprüfung ihrer Kontrollen anhand der aktuellen Implementierung.
- Die Einrichtungsbildschirme von Drittanbietern wurden nicht erneut erfasst. Veraltete Setup-Screenshots waren
entfernt statt als aktuell angezeigt.

Aktualisieren Sie bei jeder neuen Aktualisierung das Datum, die Quellumgebung, den Status Geprüft ,
Bereinigung und etwaige verbleibende Überprüfungsbeschränkungen. Alte Provenienz nicht kopieren
Als Beweis für eine neue Erfassung vorlegen.

### Entfernen des Aktionseditor-Schalters, 19.10.2026

Aktualisierte `guide-creating-actions/generated-action-review.png` aus den laufenden
Lokale Benutzeroberfläche nach dem Entfernen des Umschalters „Aktiviert“ im Editor. Sichtbarkeit der App-Seite
Schalter bleibt die Aktion Ein/Aus-Steuerung; Modell- und Widget-Belichtungseinstellungen
Im Editor bleiben. Der Repository-Link wird maskiert und die Kontonavigation wird deaktiviert
Ausgeschlossen. Während dieser Aktualisierung wurde keine Aktion gespeichert, genehmigt, umgeschaltet oder bereitgestellt.
Rohaufnahme: `docs-captures/2026-10-19/remove-enabled/`.
