---
name: llm-apps-docs
description: Erstellen, aktualisieren, überprüfen und validieren Sie die öffentliche Dokumentation und Screenshots zu Adobe LLM Apps. Verwenden Sie beim Bearbeiten von lm-apps.en-Artikeln, des Experience League-Inhaltsverzeichnisses, der Anleitung für Onboarding-Agenten, der EDS-Widget-Dokumente, der Anleitung für die Produktionsbereitschaft oder von Dokumentations-Screenshots.
source-git-commit: ca0d8f49a295e6465f2e9b20809e69436bfa93d5
workflow-type: tm+mt
source-wordcount: '716'
ht-degree: 0%

---


# Dokumentation zu LLM Apps

Erstellen Sie eine aufgabenorientierte, überprüfbare öffentliche Dokumentation für Adobe LLM-Apps.

## Source-of-Truth-Ordnung

Überprüfen Sie die Produktansprüche in dieser Reihenfolge:

1. Aktuelle Produktions-Benutzeroberfläche unter `https://experience.adobe.com/#/@llmapps/llm-apps/`
2. Aktuelle im Arbeitsbereich verfügbare Benutzeroberflächen- und API-Implementierung
3. Aktuelles öffentliches SDK- und Textbausteinverhalten
4. Vorhandene öffentliche Dokumentation

Wenn die Produktion mit der Quelle oder den Plänen in Konflikt steht, dokumentieren Sie die Produktion und melden Sie die Diskrepanz. Keinen anstehenden Workflow veröffentlichen, da er derzeit verfügbar ist.

## Jede Aufgabe starten

1. `help/main-toc/TOC.md` lesen.
2. Lesen Sie den Target-Artikel und direkt damit zusammenhängende Artikel.
3. Klassifizieren Sie den Inhalt mit [content-model.md](content-model.md).
4. Identifizieren Sie Benutzeroberflächen-Kennzeichnungen, URLs, Befehle und Verträge, die überprüft werden müssen.
5. Beibehaltung der Konsistenz des End-to-End-Beispiels und der Terminologie auf allen Seiten.

## Authoring-Regeln

- Führen Sie Erstbenutzende durch den Onboarding-Agent.
- Organisieren Sie die Navigation um Journey und Ergebnisse der Benutzenden, nicht um Implementierungsthemen.
- Geben Sie die Journey-Sequenz am Anfang jedes Handbuchs an und stellen Sie den nächsten freigegebenen Schritt bereit.
- Verwenden Sie **Onboarding-Agent** für die Produktfunktion und eine exakte Kopie der Benutzeroberfläche wie **[!UICONTROL Programm automatisch erstellen]** für Steuerelemente.
- Erklären Sie ein technisches Konzept, wenn Sie dem Benutzer zum ersten Mal begegnen. Verweisen Sie auf ein tieferes Konzept oder Referenzmaterial.
- Halten Sie Tutorials linear, Anleitungen für aufgabenorientierte Aufgaben und Referenzseiten mit Fakten.
- Es sollten nur Informationen enthalten sein, die der Leser für die aktuelle Aufgabe benötigt; kurze, direkte Sätze sollten bevorzugt werden.
- Verwenden Sie eine repräsentative App auf der gesamten Journey.
- Unterscheiden Sie generierte Strukturvorlagen von produktionsfertigen Integrationen.
- Vermeiden Sie interne Worker-Namen, Datenbankfelder, Implementierungstickets und instabile Pipeline-Details.
- Feldtabellen nicht über Handbücher hinweg duplizieren; Link zu Referenz.
- Bewahren Sie die Schriftarten und Anweisungen von Experience League auf: `[!DNL]`, &grave;&grave;, `[!IMPORTANT]`, `[!NOTE]` und `[!TIP]`.
- Verwenden Sie stammbezogene interne Links: `/help/...`.
- Verwenden Sie das Satzbeispiel für Titel und Überschriften, sofern die Produktkennzeichnung nichts anderes erfordert.
- Verwenden Sie einen beschreibenden Bild-Alternativtext, der den Bildschirm und den Status erklärt.

## Geschützte PM-genehmigte Erzählung

In `help/overview/overview.md` sind diese Abschnitte PM-genehmigt:

- **Was Sie mit LLM Apps tun können**
- **Warum LLM-Apps wichtig sind**

Beibehaltung der Überschriften, Aufzählungspunkte, des Wortlauts, der Reihenfolge und der Behauptungen.
Während der allgemeinen Dokumentation dürfen Sie sie nicht kürzen, neu schreiben, neu organisieren oder entfernen
Aktualisierungen. Ändern Sie beide Abschnitte nur, wenn der Benutzer sie explizit anfordert, und
bestätigt, dass die neue Kopie PM-genehmigt ist.

## Sicherheitsanforderungen

- Geben Sie niemals Anmeldeinformationen, Token, private URLs, persönliche Daten, interne Hostnamen oder Kundenkennungen an.
- Geheime Daten anzeigen, die aus der verwalteten Konfiguration geladen wurden, nie hartcodiert.
- HTTPS für externe Dienste erforderlich.
- Externe Eingabe und Upstream-Antworten validieren
- Rendern Sie externe Werte mit sicheren DOM-APIs. Es wird nicht empfohlen, sie in `innerHTML` zu interpolieren.
- Empfohlen werden die Berechtigungen der geringsten Berechtigung für GitHub-App, API, CSP, CORS und Browser.
- Verwenden Sie sichere Fehler für den Benutzer und vermeiden Sie die Protokollierung sensibler Daten.

## Screenshot-Workflow

Für neue oder aktualisierte Bilder folgen Sie &quot;[.md](screenshots.md) und [screenshot-manifest.md](screenshot-manifest.md).

Der Standard-Workflow verwendet ein vom Benutzer erstelltes Produktions-Capture-Pack:

1. Suchen Sie nach Screenshots unter `docs-captures/<run-id>/` oder verwenden Sie den vom Benutzer bereitgestellten Ordner.
2. Inventarisieren und untersuchen Sie alle PNG-, JPEG- und WebP-Dateien. Verlassen Sie sich nicht allein auf den Dateinamen.
3. Abgleichen von Screenshots mit Manifeststatus mithilfe sichtbarer Benutzeroberflächeninhalte.
4. Fehlende, doppelte, mehrdeutige, veraltete oder unsichere Aufzeichnungen melden, bevor die Dokumentation geändert wird.
5. Quellaufzeichnungen werden unverändert beibehalten.
6. Erstellen Sie bereinigte endgültige Kopien unter Verwendung der stabilen Manifest-Dateinamen unter `help/assets/`.
7. Aktualisieren Sie das Tutorial und die zugehörigen Handbücher, um sie an den tatsächlichen erfassten Produktions-Workflow anzupassen.
8. Fügen Sie genauen Alternativtext hinzu und führen Sie die Dokumentationsvalidierung durch.

Die agentengesteuerte Browser-Erfassung bleibt ein optionales Fallback. Speichern Sie weder den Browser-Status noch die Anmeldeinformationen und führen Sie keine Screenshot-Erfassung mit Produktionsmutation in CI aus.

Bei der Aufforderung, „Dokumente aus Screenshots zu aktualisieren“:

- Den neuesten explizit ausgewählten Erfassungsordner als Quelle behandeln.
- Fragen Sie nur, wenn der App-Fluss oder die Screenshot-Zuordnung wirklich mehrdeutig sind.
- Rohdaten-Erfassungsordner dürfen nicht übertragen werden.
- Nie Quellaufnahmen löschen oder ändern, ohne explizite Genehmigung.
- Wenn vertrauliche Informationen nicht entfernt werden können, ohne die Aufgabe zu verschleiern, fordern Sie eine sichere Rückerfassung an.

## Erstellen eines Überprüfungsarchivs

Erstellen einer freigebbaren Offline-HTML-Site und eines ZIP-Archivs:

```bash
node .cursor/skills/llm-apps-docs/scripts/build_review_bundle.mjs
```

Der Build wird neben dem Repository geschrieben, nicht in ihm. Es umfasst nur
Veröffentlichte Artikel und bereinigte Assets, wandelt Experience League-Anweisungen um
für die Offline-Überprüfung und stellt fest, dass die Formatierung nicht das endgültige Erlebnis ist
League-Rendering.

## Validieren

Ausführen:

```bash
python3 .cursor/skills/llm-apps-docs/scripts/validate_docs.py
```

Korrigieren Sie alle fehlenden internen Artikel, fehlenden Assets, ungültigen Stammpfad und fehlenden Frontend-Felder, bevor Sie sie weitergeben.

Überprüfen Sie auch:

- Benutzeroberflächen-Beschriftungen und Screenshots stimmen mit der Produktion überein.
- Skript- und Widget-URL-Beispiele stimmen in Handbüchern und Referenzen überein.
- Befehle entsprechen dem aktuellen Textbaustein.
- Neue Seiten werden mit dem Inhaltsverzeichnis verknüpft.
- Der Adobe-Workflow für die Artikelvalidierung verläuft erfolgreich, sofern verfügbar.

## Unterstützende Referenzen

- [Inhaltsmodell und Terminologie](content-model.md)
- [Screenshot-Verfahren für die Produktion](screenshots.md)
- [Screenshot-Manifest](screenshot-manifest.md)
