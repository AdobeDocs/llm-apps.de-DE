---
source-git-commit: 0276ae4e4627e49157911ad579a42c0e77045eee
workflow-type: tm+mt
source-wordcount: '2314'
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
| `setup-choice.png` | Erste Schritte mit **Let us handle it** und **Use my Cloud Manager setup**, Callout auf der Option des eigenen Repositorys | Erstellen Ihrer LLM-App - Erste Schritte mit Lassen Sie uns die Aufgabe lösen und mein Cloud Manager-Setup verwenden |
| `action-repository.png` | Cloud Manager-Programm und -Repository ausgewählt, **Repository validiert** | Einrichtung der Aktionsinfrastruktur - Cloud Manager-Programm und -Repository ausgewählt |
| `widget-source.png` | Widget-Quelle mit **Widget-Repository manuell auswählen** (Cloud Manager EDS als „In Kürze verfügbar“ markiert) | Einrichtung der Widget-Infrastruktur - Widget-Quelle |
| `widget-repository.png` | Nicht-GitHub-Widget-Repository validiert, **Edge Delivery Services-Site** Hinweis | Einrichtung der Widget-Infrastruktur - Widget-Repository validiert und mit seiner verknüpften EDS-Site verknüpft |
| `app-details.png` | App-Name, aus dem Repository stammender AEM-Programmsatz, Datenregion „Application Insights“ | App-Details - Name, AEM-Programm und Datenregion „Application Insights“ |
| `settings-repositories.png` | Einstellungen > Repository mit sowohl Cloud Manager-Repository-Karten als auch der verknüpften Site | Anwendungseinstellungen - Abschnitt „Repository“ |
| `provisioning.png` | Verwaltetes Setup genehmigter LLM-Apps wird ausgeführt | App-Einrichtung - Adobe bereitet die Infrastruktur vor |

Letzte Erfassung (alle außer `provisioning.png` und `da-write-permissions.png`): 13.11.2026, Staging
(`experience-stage.adobe.com/llm-apps`), GitLab-Aktion und Widget-Repositorys mit Bring Your
Eigenes Git. Die Werte für Programm, Repository, Site und Herkunft werden maskiert. Rote Konturen sind Hinweisen.
Nicht erfasst: der GitHub-Widget-Pfad (AEM Code Sync installieren, AEM-Setup validieren). Noch nicht
in der Produktion überprüft.

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
| `generated-action-review.png` | Aktionsdetails, LLM-Widget-Details, Programm-Insights, ChatGPT-Erweiterungen, die **Verzweigung gepusht**-Benachrichtigung (eigene Repositorys) und Als geprüft markieren. Zuletzt erfasst: 2026-11-13, Staging, GitLab-App; App-Name maskiert | Erzeugte Aktionsüberprüfung - Zweig gepusht und als geprüft markieren |

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
| `action-basic-info.png` | Aktionsdetails: Suchen nach Produkten, ihrer Code-Kennung, Beschreibung und den `category`- und `query` | Neue Aktion erstellen - Aktionsdetails mit Aktionsinformationen und Eingabeparametern |
| `action-llm-settings.png` | LLM-Einstellungen erweitert: schreibgeschützt, idempotent, Open-World-Anmerkungen, Statustexte, Sichtbarkeit | Neue Aktion erstellen - LLM-Einstellungen |
| `action-analytics-user-intent.png` | Anwendungseinblicke: Erfassen der Benutzerabsicht und der Standardanweisungen | Neue Aktion erstellen - Anwendungseinblicke |
| `widget-metadata.png` | LLM-Widget-Details: Edge Delivery Services-Block, Beispiel-Widget-Domain, Widget-Beschreibung und Vorlagen-URLs | Neue Aktion erstellen - Details zum LLM-Widget |
| `widget-csp.png` | Vorlagen-URLs und die Liste der CSP-Konfigurations-Domain | Neue Aktion erstellen - CSP-Konfiguration |

Zuletzt erfasst: 13.11.2026, Staging, nach der Dialogumgestaltung (dieselben Registerkarten wie der Aktionseditor:
Aktionsdetails, LLM-Widget-Details, Programm-Insights, ChatGPT-Erweiterungen; Berechtigungen,
bevorzugt Rahmen, und der Abschnitt Dateiparameter wurde entfernt). Nur Beispielwerte; das Dialogfeld
wurde ohne Speichern abgebrochen. Rote Umrisse sind Hinweise.

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

## ChatGPT-Erweiterung

Artikel: `/help/guides/chatgpt-extensions.md`

Ausgabeverzeichnis: `help/assets/guide-chatgpt-extensions/`

Screenshots, Anzeigeeinstellungen und Composer-Mention für globale und Thread-Einstiegspunkte
Konfigurations-Screenshots wurden am 10.11.2026 bereitgestellt und werden als
Aufgezeichnet unten. Andere Checkpoints bleiben geplant, keine vorhandenen Assets. Nicht
Fügen Sie Links für Artikelbilder hinzu, bis die tatsächlichen Aufnahmen bereinigt und visuell dargestellt wurden
Überprüft.

| Dateiname | Status und nützlicher Kontext | Vorgeschlagener Alternativtext |
|---|---|---|
| `extensions-tab.png` | Gespeicherte, Widget-unterstützte Aktion mit ausgewählten ChatGPT-Erweiterungen | ChatGPT-Erweiterungen - Einstiegspunkte, Anzeigemodus und Composer-Erwähnungen |
| `global-thread-entrypoints.png` | Globaler und Thread werden zusammen für das Startseiten-Widget ausgewählt, wobei die Option Datei deaktiviert ist. | ChatGPT-Erweiterungen - Global und Thread für die Startseiten-Aktion ausgewählt |
| `global-entrypoint-menu.png` | Einstiegspunkt für die Startseite im Navigationsmenü des Kunden | ChatGPT-Navigationsmenü mit dem globalen Startpunkt |
| `global-entrypoint-open.png` | Vollständiges Client-Fenster mit geöffnetem Startbildschirm und sichtbarer Navigationsleiste | Startseiten-Dashboard über den globalen Einstiegspunkt geöffnet |
| `thread-entrypoint-menu.png` | Konversationsmenü mit erweiterten Plug-ins | Konversationsmenü - Wählen Sie den Einstiegspunkt für den Home-Thread aus. |
| `thread-entrypoint-open.png` | Home-Dashboard neben einer Konversation | Startseiten-Dashboard neben einer ChatGPT-Konversation öffnen |
| `file-entrypoint.png` | Ungespeicherter Dateieinstiegspunkt mit .csv | Dateieinstiegspunkt - bietet einen benutzerdefinierten Viewer für CSV-Dateien |
| `entrypoint-icon.png` | Nicht gespeicherte optionale Symbol-URL, die von den ausgewählten Einstiegspunkten der Aktion freigegeben wird | Einstiegspunktsymbol - Festlegen eines optionalen Symbols für eine freigegebene Aktion |
| `display-settings.png` | Modellstart-Voreinstellung, unterstützte Ressourcenmodi und erste Anzeige bei Host-Standardwerten | Anzeigemodus - Voreinstellungen für Ressourcen und Modellstart bei Standardeinstellungen |
| `composer-mentions.png` | Die Aktion „Nur Text“ mit aktiviertem Umschalter für die Erwähnung wird in der Suche erwähnt | Erwähnungen suchen - Composer-Erwähnungen aktiviert |
| `composer-mentions-query.png` | Details zur Aktion bei der Suche mit einer optionalen Eingabe für eine Zeichenfolgenabfrage | Suchbegriffe - optionale Zeichenfolgeneingabe mit dem Namen „query“ |
| `composer-mentions-conflict.png` | Umschalter für Erwähnung und Warnung beim Benennen des vorhandenen Anbieters deaktiviert | Composer erwähnt nicht verfügbar, da Search Mentions bereits der Anbieter ist |

Verwenden Sie eine zugelassene, nicht empfindliche Katalogbefestigung. Formulare nicht automatisch speichern
Vor der Eingabe von Beispielwerten; nicht gespeicherte Änderungen später verwerfen. Nicht speichern,
Genehmigen Sie generierte Aktionen oder stellen Sie sie nur bereit, um diese Checkliste abzuschließen.
Drittanbieterergebnisse benötigen authentifizierten Zugriff auf einen kompatiblen Client. Datei
Einstiegspunkte erfordern einen Desktop. Der Arbeitsbrowser ist kein klassisches ChatGPT.

### Aktualisierung der Einstiegspunkt-Dokumentation, 2026-11-05

- Source: aktuelle lokale `llm-apps-ui` und Eingabevalidierung, keine
Anleitung zur Produktions-Benutzeroberfläche.
- Schnellzugriff-Steuerelemente wurden entfernt. Globale, Thread- und Dateiverwendung
Kontrollkästchen, die mehrere Auswahlen für ein Widget unterstützen. Symbol URL ist ein
Optionale Einstellung auf Aktionsebene, die von Einstiegspunkten freigegeben wird.
- Wenn Sie Datei auswählen, wird eine optionale Objekteingabe mit dem Namen `file` hinzugefügt, wenn sie nicht vorhanden ist.
Das Kombinieren von Datei mit „Global“ oder „Thread“ erfordert, dass diese Eingabe optional bleibt.
- Die Browser-Inspektion wurde blockiert, da beide Browser-Profile für die Automatisierung
Wird bereits verwendet. Es wurden keine neuen Screenshots aufgenommen.

### Text Herkunftsentwurf, 2026-11-03

- Source-Umgebung: lokale Implementierung von UI/API/Runtime/SDK, keine abgeschlossen
Anleitung für den Produktions-Aktionseditor.
- APIs für veröffentlichte, durch die Paketüberprüfung bestätigte Erweiterungen in Runtime 1.3.0 und
SDK 0.2.1. Dies sind die dokumentierten Baseline-Versionen, keine Behauptung, dass alle
Diese Funktion wurde in genau diesen Versionen eingeführt.
- Die öffentliche Erweiterungsspezifikation von OpenAI wurde auf Plattformbeschränkungen überprüft.
- Die Produktionsanmeldung war in einem separaten agentengesteuerten Browser erfolgreich. Keine App
Die Einstellungen wurden gespeichert, es wurde keine Vorrichtung ausgewählt und keine Bereitstellung gestartet.
- Der Benutzer hat Screenshots zurückgestellt. Es wurden keine neuen Bilder erfasst oder ersetzt.
Exakte Produktions-Control-Kennzeichnungen und das End-to-End-Host-Verhalten erfordern weiterhin eine
Live-exemplarische Vorgehensweise vor der Veröffentlichung.

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

## Analytics-Einrichtung

Artikel: `/help/guides/analytics-setup.md`

Ausgabeverzeichnis: `help/assets/guide-analytics/`

| Dateiname | Status und nützlicher Kontext | Vorgeschlagener Alternativtext |
|---|---|---|
| `analytics-settings.png` | Anwendungseinstellungen, Registerkarte Analytics, Analytics aktivieren, Leere Datenstrom-ID, Anforderungen erweitert, Änderungen speichern deaktiviert | Anwendungseinstellungen, Registerkarte „Analytics“ : Aktivieren der Analyse, ein leeres Feld für die Datenstrom-ID und die erweiterte Liste mit den Anforderungen |
| `datastream-analytics-service.png` | Datenerfassung, Service hinzufügen, Adobe Analytics aktiviert mit einer fiktiven Report Suite-ID | Service hinzufügen - Adobe Analytics mit einer Beispiel-Report Suite-ID aktiviert |
| `datastream-platform-service.png` | Datenerfassung, Adobe Experience Platform-Service mit ausgewähltem Ereignis-Datensatz | Adobe Experience Platform-Service - der ausgewählte Ereignis-Datensatz und die anderen Optionen gelöscht |
| `datastream-services.png` | Datenstromdetails mit beiden aktivierten Services, einer Platzhalter-Datenstrom-ID und dem Zuordnungsschema | Datenstromdetails - Beide Services sind aktiviert, mit der Beispiel-Datenstrom-ID und dem Zuordnungsschema |

Erfasst nie eine echte Datenstrom-ID. Wählen Sie nicht **Änderungen speichern**: nach einem Speichern,
Nur ein Administrator kann den Datenstrom ändern. Experience Platform, Adobe Analytics,
und Customer Journey Analytics-Bildschirme nur dort angezeigt werden, wo sie eine Einstellung speziell für
LLM Apps; Link zu Experience League für alles andere. Organisation ersetzen
Namen, Report Suite-IDs, Datenstrom-IDs, Datensatz-IDs, E-Mails und persönliche Namen
mit diesen Platzhalterwerten: `example-org`, `demo.<suffix>` Report Suites,
`00000000-0000-0000-0000-000000000000`, `0000…0001`, `user@example.com` und
`Example User`.

## Zur Übermittlung vorbereiten

Artikel: `/help/guides/prepare-submission.md`

Ausgabeverzeichnis: `help/assets/guide-submission/`

| Dateiname | Status und nützlicher Kontext | Vorgeschlagener Alternativtext |
|---|---|---|
| `submission-card.png` | App-Seite Nach einer Produktionsbereitstellung, Kartenübermittlung vorbereiten mit Einrichten | Zur Übermittlung vorbereiten - Status und Einrichtung der Produktionsvorbereitung |
| `submission-setup.png` | Dialogfeld „Vorbereiten der Übermittlung“, „Einrichten und Vorbereiten“, „Beispiel-Publisher-Details“ und beide Symbole gespeichert | Einrichten und Vorbereiten - Herausgeber- und Rechtsdetails, genehmigte Symbole und Änderungen speichern |
| `submission-prepare.png` | Im selben Dialogfeld wird gescrollt, um das Paket vorzubereiten, Bereitschaft zeigt, Kontrollkästchen deaktiviert | Paket vorbereiten - Bereit zur Vorbereitung, Kontrollkästchen zur Bestätigung und Paket vorbereiten |
| `submission-progress.png` | Derselbe Dialog während eines Durchgangs | Vorbereitung läuft - Fortschrittsleiste und Startzeit |
| `submission-results.png` | Ergebnisse nach einem abgeschlossenen Durchlauf mit Platzhalter-Links: Vorbereitung muss beachtet werden, Paket herunterladen deaktiviert | Ergebnisse - Zusammenfassung der Vorbereitung, Bericht herunterladen, Paket herunterladen und Aufzeichnung herunterladen |
| `submission-test-cases.png` | Ergebnisse, Testfälle, erster Fall umrissen | Testfälle - ein Überprüfungsfall mit seiner Anfrage, dem erwarteten Verhalten und dem aufgezeichneten Status |
| `submission-before-submit.png` | Ergebnisse: Vor dem Senden der Liste und den Prüfungen „BESTANDEN“/„WARNUNG“/„FEHLGESCHLAGEN“; ID unterhalb des Zuschnitts ausführen | Vor dem Senden - verbleibende Details, Aktualisierungsdetails und Paketprüfungen |

Auf der Bühne erfasst, 2026-11-13, von einer in der Produktion bereitgestellten App. Nur verwenden
Beispiel für Publisher-Werte (`Frescopa Coffee (example)`, `https://www.example.com/...`)
und Platzhaltersymbole. Bei jeder Vorbereitungsausführung wird die bereitgestellte App aufgerufen und benötigt etwa
20 Minuten; nur starten, wenn die Ergebnisbildschirme aktualisiert werden müssen. Maskieren von MCP-URLs,
IDs und App-Namen in Ergebniserfassungen ausführen.

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

### Analytics-Einstellungen, 21.10.2026

Aktualisierte `guide-analytics/analytics-settings.png` von der vom Benutzer bereitgestellten
`Screenshot 2026-10-21 at 11.48.36.png`, kein agentengesteuerter Durchlauf. Quelle
Umgebung: nicht aufgezeichnet und nicht auf dem Bild sichtbar; Produktionsparität nicht
Verifiziert. Bestätigung der visuellen Überprüfung, dass „Analytics aktivieren“ aktiviert ist, im Feld Datenstrom-ID .
ist leer, Anforderungen werden erweitert und Änderungen speichern ist deaktiviert. Das Bild war
Ohne Änderungen kopiert; es sind keine Kennungen oder Kontodetails sichtbar.

### Datastream-Screenshots, 19.10.2026

`guide-analytics/datastream-analytics-service.png` hinzugefügt
`datastream-platform-service.png` und `datastream-services.png` von vorhandenen
Interne Datenerfassungen (29.07.2026 und 2026.10.19). Die Report Suite-ID
und die Datenstrom-ID wurden durch Platzhalter-Textüberlagerungen (`demo.aem.llm` und
`00000000-0000-0000-0000-000000000000`); die Überlagerungsschriftart ist San Francisco, also es
unterscheidet sich geringfügig von der Produktschriftart. Das Platform-Service-Image ist jetzt erforderlich
Änderungen. Das Bild des Analytics-Service zeigt weiterhin den Mauszeiger aus der
Ursprüngliche Erfassung. Bei einem OCR-Überlauf wurden die endgültigen Bilder ohne Originalkennungen gefunden.
Ersetzen Sie diese durch neue Erfassung, die Platzhalterwerte verwenden, die in die Benutzeroberfläche eingegeben wurden, wenn
Der Artikel wird als Nächstes aktualisiert.
