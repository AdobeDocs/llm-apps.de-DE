---
source-git-commit: 03c918b1643d9c4e8ebee40fd67694acb6751a14
workflow-type: tm+mt
source-wordcount: '703'
ht-degree: 0%
---
# Screenshot-Verfahren für die Produktion

Verwenden Sie dieses Verfahren, um echte Screenshots der öffentlichen Dokumentation von folgenden Elementen zu erfassen:

`https://experience.adobe.com/#/@llmapps/llm-apps/`

Der bevorzugte Workflow ist die Erfassung durch Menschen, gefolgt von der agentengestützten Aufnahme. Der Anwender entscheidet, welche Produktionszustände relevant sind. Die Qualifikation organisiert, bereinigt und integriert sie in die Dokumentation.

## Posteingang erfassen

Platzieren Sie jeden Erfassungsdurchgang in:

```text
docs-captures/<YYYY-MM-DD>/
```

Das Verzeichnis wird von Git ignoriert. Rohe Screenshots müssen lokal bleiben und dürfen niemals übertragen werden.

Der Benutzer kann beliebige Dateinamen verwenden, aber geordnete Namen erleichtern die Überprüfung:

```text
01-create-app.png
02-connect-github.png
03-onboarding-enabled.png
04-generating-actions.png
05-review-actions.png
```

Eine optionale `capture-notes.md` kann fehlende Zustände, ungewöhnliches Verhalten oder die beabsichtigte Reihenfolge beschreiben.

## Sicherheitsgrenzen

- Der Benutzer gibt die Anmeldeinformationen für Adobe, GitHub und LLM-Plattform direkt im Browser ein.
- Stopp für MFA, Passkeys, Captchas, Organisationsauswahl und privilegiertes Einverständnis.
- Lesen, drucken, speichern oder übertragen Sie niemals Token, Cookies, Browser-Speicher oder Anmeldeinformationen.
- Verwenden Sie eine nicht vertrauliche öffentliche Website und neue reine Dokumentations-Repositorys.
- Gewähren Sie GitHub-Apps nur Zugriff auf die beiden Repositorys, die von der Vorrichtung verwendet werden.
- Vor dem Erstellen, Bereitstellen, Löschen, Archivieren oder Ändern des Repository-Zugriffs nachfragen.
- Erfassen Sie keine persönlichen Informationen, Organisations-IDs, Repository-Installations-IDs, Token oder vollständigen Laufzeit-URLs.

## Benennung der Vorrichtung

Namen verwenden, die die verfügbaren Dokumentationsressourcen eindeutig identifizieren:

```text
App: LLM Apps Docs <YYYY-MM-DD>
Handler repo: llm-apps-docs-<YYYYMMDD>
EDS repo: llm-apps-docs-<YYYYMMDD>-eds
```

Bevor Sie etwas erstellen, bestätigen Sie die Zielorganisation von Adobe, den GitHub-Eigentümer, die öffentliche Website und die Namen der Spiele mit dem Benutzer.

Erstellen Sie sowohl leere als auch private Repositorys. Initialisieren Sie sie nicht mit einer README, Lizenz oder `.gitignore`.

## Einstellungen erfassen

- Verwenden Sie einen Desktop-Viewport, der groß genug ist, um vollständige Dialogfelder ohne Browser-Chrome anzuzeigen.
- Zoom bei 100 % beibehalten.
- Verwenden Sie das Standard-Produktdesign, es sei denn, der Artikel lehrt Designs speziell.
- Erfassen Sie die kleinste abgeschlossene Region, die die Aufgabe und den erforderlichen Kontext enthält.
- Vermeiden Sie Cursor, öffnen Sie Menüs, die nichts mit dem Schritt zu tun haben, Popups aus vorherigen Aktionen und vorübergehende Spinner, es sei denn, das Spinner ist der dokumentierte Status.
- Verwenden Sie PNG.
- Dateinamen stabil halten; Bildinhalte ersetzen, anstatt Dateien während der Aktualisierung umzubenennen.

## Empfohlene Aufnahmesequenz

Der Benutzer sollte die relevanten Status aus dem Manifest erfassen, einschließlich:

1. App erstellen, bevor GitHub verbunden ist.
2. Repository-Zugriff für die GitHub-App.
3. **Meine App automatisch erstellen** aktiviert, wobei beide Repositorys ausgewählt sind.
4. App-Erstellung oder automatischer App-Build-Start.
5. Aktionen werden generiert.
6. Erzeugte Aktionen, die zur Überprüfung bereit sind.
7. Metadaten, Handler und Widget einer repräsentativen Aktion.
8. Überprüfung vor der Aktion und Status aller Aktionen überprüft.
9. Erfolgreiche Staging-Bereitstellung
10. App-Registrierung und ein repräsentatives Ergebnis in der LLM-Plattform.

Erfassen Sie zusätzliche Bildschirme, wenn sie eine echte Entscheidung, einen Fehler oder eine Voraussetzung erläutern. Nicht jeden Klick erfassen.

## Workflow zur Kenntnis-Aufnahme

Wenn der/die Benutzende gebeten wird, die Dokumentation aus einem Erfassungsordner zu aktualisieren:

1. Bestätigen Sie das genaue Erfassungsverzeichnis.
2. Inventarisieren Sie alle PNG-, JPEG- und WebP-Dateien und überprüfen Sie jedes Bild visuell.
3. Erstellen Sie eine Zuordnung von Quelldateien zu Einträgen in `screenshot-manifest.md`.
4. Vergleichen Sie sichtbare Benutzeroberflächen-Beschriftungen und -Sequenzen mit dem vorhandenen Tutorial.
5. Bericht:
   - erforderliche Zustände fehlen;
   - doppelte oder redundante Bilder;
   - mehrdeutige Reihenfolge;
   - Veraltete Screenshots;
   - vertrauliche Informationen;
   - Produktionsverhalten, das mit den Dokumenten kollidiert
6. Quellaufnahmen nicht bearbeiten.
7. Erstellen Sie für jedes akzeptierte Bild eine bereinigte Kopie mit dem stabilen Manifest-Dateinamen unter dem Ausgabeverzeichnis, das im Manifest-Abschnitt deklariert wird.
8. Nur zuschneiden, wenn die umgebende Benutzeroberfläche keinen nützlichen Kontext hinzufügt.
9. Maskieren sensibler Werte. Wenn eine sichere Maskierung nicht möglich ist, bitten Sie um eine erneute Aufnahme.
10. Aktualisieren Sie den Artikel und den Alternativtext, um ihn an den erfassten Workflow anzupassen.
11. Link- und Asset-Validierung ausführen.
12. Belassen Sie den Erfassungsordner an Ort und Stelle, bis der Benutzer ihn explizit entfernen möchte.

## Optionale agentengesteuerte Erfassung

Wenn der Benutzer den Agenten auffordert, den Browser zu steuern, verwenden Sie dasselbe Manifest und dieselben Sicherheitsgrenzen. Für Authentifizierung, berechtigtes Einverständnis, Repository-Änderungen, App-Erstellung, Bereitstellung und Bereinigung pausieren. Führen Sie diesen produktionsmutierenden Fluss niemals unbeaufsichtigt aus.

## Bildüberprüfung

Für jedes Bild:

- Mit einem Manifesteintrag abgleichen.
- Überprüfen Sie die Kopie der Benutzeroberfläche mit dem Artikel.
- Beschneiden Sie die Kontonavigation, wenn sie nicht benötigt wird.
- Maskieren Sie persönliche Namen, Avatare, Organisationskennungen, Repository-Installations-Kennungen, Laufzeitnamespaces und nicht verwandte Apps.
- Vergewissern Sie sich, dass kein automatisches Ausfüllen des Browsers, keine E-Mail-Adresse, kein Zugriffstoken oder keine Details zum privaten Repository sichtbar sind.
- Schreiben Sie Alt-Text, der sowohl den Bildschirm als auch den Status identifiziert.

## Anhalten

Stoppen und Melden eines Blockers, wenn:

- Die Produktion entspricht nicht dem dokumentierten Workflow.
- Der Überprüfungsfluss unterscheidet sich wesentlich von der veröffentlichten Dokumentation.
- Die Repository-Validierung lehnt den beabsichtigten Fluss des leeren Repositorys ab.
- Die Onboarding-Pipeline schlägt fehl.
- Für eine privilegierte Aktion ist ein Benutzer oder Administrator erforderlich.
- Ein Screenshot kann nicht sicher gemacht werden, ohne die für den Schritt wesentlichen Informationen zu verbergen.
