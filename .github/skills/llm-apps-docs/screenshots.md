---
source-git-commit: a48b4022979d9a13b4fc3aa870c4ee6375e8bc13
workflow-type: tm+mt
source-wordcount: '1109'
ht-degree: 0%
---
# Agent-gesteuertes Screenshot-Verfahren

Erfassen Sie die laufende Benutzeroberfläche selbst mit der Browser-Automatisierung. Der Benutzer stellt
Authentifizierung und Validierung von Ressourcenänderungen; der Agent navigiert, erfasst,
Bereinigt und integriert die Bilder. Keine manuell zusammengesetzte
Screenshot Pack vor dem Start.

## Wählen der Umgebung

Verwenden Sie die vom Benutzer bereitgestellte URL. Verwenden Sie andernfalls Production:

`https://experience.adobe.com/llm-apps`

Erfassen Sie das Erfassungsdatum, die Umgebung und den beobachteten Status in
[screen-manifest.md](screenshot-manifest.md). Eine lokale oder Staging-Erfassung funktioniert nicht
Produktionsverhalten überprüfen. die Benutzeroberfläche mit der entsprechenden Implementierung vergleichen und
-Bericht steht in Konflikt, anstatt einen nicht verifizierten Fluss als veröffentlicht darzustellen.

## Browser öffnen und authentifizieren

1. Überprüfen Sie, ob die angeforderte Benutzeroberfläche ausgeführt wird. Starten Sie keinen doppelten Server.
2. Verwenden Sie die verfügbare Browser-Automatisierung. Browser-Sitzung nur wiederverwenden, wenn sie
Eigentümerschaft und Zugriffsbereich sind klar.
3. Wenn die Automatisierung nicht angehängt werden kann, öffnen Sie einen separaten Browser unter der Agentenkontrolle.
Verwenden Sie für Chrome ein dediziertes Sitzungsprofil außerhalb des Repositorys und binden Sie
Remote-Debugging zum `127.0.0.1` an einem nicht verwendeten lokalen Port. Nie kopieren des
Persönliches Browser-Profil anzeigen oder Debugging-Port für das Netzwerk freigeben.
4. Stellen Sie sicher, dass der Browser ausgeführt wird und sein Automatisierungsendpunkt reagiert.
5. Öffnen Sie die App und pausieren Sie, damit sich der Benutzer authentifizieren kann. Der Benutzer verarbeitet Adobe,
GitHub- und LLM-Plattform-Anmeldeinformationen, MFA, Passkeys, Captchas, Organisation
Auswahl und privilegierte Zustimmung.
6. Fortsetzen, nachdem der Benutzer die Authentifizierung bestätigt hat Mit Playwright verbinden
   `chromium.connect_over_cdp()` bei Verwendung einer kontrollierten Chrome-Debugsitzung.
7. Suchen Sie den App-Frame anhand seines Ursprungs oder sichtbaren Inhalts. Unified Shell kann Folgendes platzieren
Die App innerhalb eines iFrames; kein fester Frame-Index vorausgesetzt.

Anmeldeinformationen, Token, Cookies oder Browser niemals lesen, drucken, exportieren oder übertragen
Speicherung. Rufen Sie keine Authentifizierungskopfzeilen `storage_state()` oder überprüfen Sie diese. Lassen Sie
Browser verarbeitet die Anmeldung, ohne ihren Status zu extrahieren.

## Sicheres Überprüfen und Navigieren

- Bevorzugen Sie semantische Locators wie `get_by_role()` und `get_by_label()`. Inspizieren
Die Barrierefreiheitsstruktur und der sichtbare Text vor der Auswahl eines Steuerelements.
- Überprüfen Sie die genauen Benutzeroberflächen-Kennzeichnungen anhand des gerenderten Bildschirms. Dialogfeld „Erstellung“ und
Der Editor für gespeicherte Aktionen verfügt über verschiedene Registerkarten. Lehnen Sie nicht die eine vom anderen ab.
- Verwenden Sie nach Möglichkeit eine genehmigte Nur-Dokumentation-App erneut. Verwenden Sie einen öffentlichen
Unempfindliche Website und erkennbarer Spielinhalt.
- Füllen Sie nicht gespeicherte Formulare erst dann mit Beispielwerten aus, nachdem Sie bestätigt haben, dass das Formular
speichert nicht automatisch. Abbrechen oder Verwerfen dieser Änderungen nach der Erfassung und anschließende Überprüfung
dass die gespeicherten Einstellungen unverändert bleiben.
- Fragen Sie vor dem Erstellen von Apps oder Repositorys,
Validieren eines Repositorys, das Textbausteindateien empfangen kann, Speichern von Einstellungen,
Variablen hinzufügen oder löschen, Pull-Anforderungen zusammenführen, bereitstellen, löschen oder
Archivieren von Ressourcen oder Ändern des Repository-Zugriffs.
- &quot;**als geprüft**&quot; als Repository-Mutation behandeln, da es Pull-Zusammenführungen durchführen kann.
Anfragen. Das Öffnen des Überprüfungsbildschirms autorisiert keine Genehmigung.
- Halten Sie den Zugriff auf die GitHub-App auf die Fixture-Repositorys beschränkt. Anfordern
Administratorgenehmigung, falls erforderlich.
- Genehmigen Sie keine Aktionen und stellen Sie keine App bereit, nur um einen optionalen Screenshot zu erhalten.
Erfasst zugängliche Status und meldet den fehlenden Status.

Wenn neue Ressourcen benötigt werden, bestätigen Sie die Zielorganisation, die Website,
Infrastrukturoption, AEM-Programm und Befestigungsname vor der Erstellung. für
Adobe-verwaltete Infrastruktur, lassen Sie die Plattform die Repositorys erstellen. Für die
Pfad zum eigenen Repository, zwei leere Repositorys verwenden und Genehmigung einholen, bevor
Die Validierung initialisiert sie.

## Erfassen stabiler Benutzeroberflächenstatus

1. Wählen Sie die entsprechenden Checkpoints aus dem Manifest aus. Entscheidungen erfassen und
Nachprüfbare Ergebnisse, nicht bei jedem Klick.
2. Verwenden Sie einen Desktop-Viewport, der zum entsprechenden Bedienfeld passt, einen 100%igen Zoom und
Standardmäßiges Produktdesign.
3. Warten Sie, bis der erwartete Inhalt, die Schriftarten, die Menüs und das Dialogfeld-Layout festgelegt wurden.
Unified Shell ändert die Größe des IFrame um Dialogfelder herum, sodass nur eine Überschrift sichtbar ist
beweist nicht, dass die Erfassungsregion stabil ist.
4. Vergewissern Sie sich, dass der Zielbereich in derselben Position und Größe bleibt. Warten auf
Laden von Überlagerungen, Beenden von Animationen, Popups und von deaktivierten zu aktivierten Übergängen
zu beenden, es sei denn, einer dieser Zustände ist Gegenstand des Bildes.
5. Den Fokus von den Eingaben weg bewegen und Menüs schließen, die nichts damit zu tun haben, wenn dies nicht geschieht
Ändern des dokumentierten Status.
6. Erfassen Sie die kleinste abgeschlossene Region, die die Aufgabe und den nützlichen
Kontext. Bedienfeld- oder Dialog-Screenshot einem ganzen Desktop-Bild vorziehen.
7. Verwenden Sie mit Playwright Locator-Screenshots und -`animations='disabled'` plus
Erfassungszeitmasken für vertrauliche Werte. Überprüfen Sie das resultierende PNG.
Die erfolgreiche Screenshot-API beweist nicht, dass die Region korrekt ist.

Ändern Sie niemals das DOM, zeichnen Sie Kennzeichnungen neu oder ändern Sie nicht das Erscheinungsbild eines Steuerelements, um ein
Screenshot sieht bereit für Erfolg aus.

## Bilder speichern und bereinigen

Speichern Sie jede Ausführung im von Git ignorierten Verzeichnis:

```text
docs-captures/<run-id>/
```

Beschreibende Dateinamen wie `setup-choice.png`, `action-basic-info.png`,
und `generated-action-review.png`. Quellaufzeichnungen werden unverändert beibehalten. Rohdaten beibehalten
Erfasst und aktualisiert Browser-Profile aus Commits und überprüft Archive.

Erstellen Sie endgültige Kopien unter den Ausgabeverzeichnissen im Manifest:

- Beschneiden Sie Browser-Chrome, die Kontonavigation, nicht verwandte Apps und die leere Arbeitsfläche.
- Geheime Daten vollständig aus der Erfassung ausschließen, einschließlich Rohdaten.
- Maskieren von Personennamen, Avataren, Organisations- und Installationskennungen, privat
Repository-Details, Runtime-Namespaces und vollständige MCP-Server-URLs. Solid bevorzugen
Neutrale Masken verschwimmen, wodurch Werte wiederhergestellt werden können.
- Verwenden Sie sichere Werte wie `https://auth.example.com`, `catalog:read`,
  `GREETING_PREFIX` und `Good day` in nicht gespeicherten Beispielformularen.
- Ausschließen generierter Kontaktdaten und anderer persönlich aussehender Beispieldaten, wenn
Dieser Inhalt ist für den Schritt nicht erforderlich.
- Beibehalten des tatsächlichen Benutzeroberflächenstatus. Zuschneiden oder Redigieren sensibler Inhalte, nicht Fehler,
Warnungen oder fehlende Funktionen, die der Artikel erläutern muss.
- Wiederverwenden stabiler öffentlicher Dateinamen beim Aktualisieren eines vorhandenen Checkpoints.

Quellaufnahmen, Browser-Profile oder Fixture-Ressourcen nicht ohne löschen
Genehmigung. Entfernen Sie kein aktives Browser-Profil, während der Browser ausgeführt wird.

## Überprüfen und integrieren

Für jedes endgültige Bild:

1. Öffnen Sie das Bild und überprüfen Sie es visuell.
2. Überprüfen Sie, ob das beabsichtigte Bedienfeld, die Beschriftungen, der Status und die relevanten Steuerelemente sichtbar sind.
3. Prüfen auf beschnittenen Text, leere oder falsche Bereiche, Laden von Überlagerungen, Übergang
Menüs und Kontodetails.
4. Bestätigen Sie, dass der Artikel den beobachteten Workflow beschreibt und dass der Alternativtext
Identifiziert den Bildschirm und den Status.
5. Aktualisieren des Manifests mit der Erfassungsumgebung, dem Datum und etwaigen Beschränkungen für
Überprüfung.
6. Führen Sie den Dokumentationsprüfer aus und erstellen Sie die Überprüfungs-Site mit den folgenden Befehlen
in `SKILL.md`.
7. Überprüfen Sie den gerenderten Artikel und stellen Sie sicher, dass jedes Bild an einem lesbaren Punkt geladen wird
Größe.

Bei Plattformen von Drittanbietern verwenden Sie einen authentifizierten Browser nur innerhalb der
Genehmigter Umfang. Wenn der Zugriff nicht verfügbar ist, prüfen Sie die Anweisungen anhand der aktuellen
Offizielle Anbieterdokumentation, nicht geprüfte Setup-Screenshots und Status
dass diese Bildschirme nicht erfasst wurden. Keine alte Drittanbietereinrichtung vorlegen
Bild als aktuell.

Wenn der Benutzer Bilder bereitstellt, überprüfen und verifizieren Sie diese Bilder vor der Wiederverwendung. Sie
sind zusätzliches Quellmaterial und keine Voraussetzung für den Erfassungs-Workflow.

## Report Blocker

Bei Authentifizierung, Einverständnis oder einer nicht genehmigten Ressourcenänderung pausieren. Bericht
Automatisierungsfehler, nicht verfügbare Status, unsichere Aufzeichnungen, Repository-Validierung
Fehler, Generierungsfehler und explizite Diskrepanzen zwischen Benutzeroberfläche und Quelle. Weiter mit
Verifizierte, nicht-mutierende Arbeit, sofern möglich; erfinden Sie nie einen erfolgreichen Status oder
Ersetzen Sie im Hintergrund fehlende Beweise durch ein veraltetes Bild.
