---
name: llm-apps-docs
description: Erstellen, überprüfen und validieren Sie die Dokumentation zu Adobe LLM Apps, wenden Sie die redaktionellen Standards an und erfassen Sie aktuelle Screenshots der Benutzeroberfläche durch die Browser-Automatisierung. Verwenden Sie für lm-apps.en-Artikel, Navigation, Screenshots und Offline-Review-Bundles.
source-git-commit: a48b4022979d9a13b4fc3aa870c4ee6375e8bc13
workflow-type: tm+mt
source-wordcount: '1346'
ht-degree: 0%
---

# Dokumentation zu LLM Apps

Erstellen Sie eine aufgabenorientierte, überprüfbare öffentliche Dokumentation für Adobe LLM-Apps.

Diese Repository-lokale Copilot-Fähigkeit lebt bei `.github/skills/llm-apps-docs/`.

## Source-of-Truth-Ordnung

Verwenden Sie die vom Benutzer angegebene aktive Benutzeroberfläche und Umgebung. Andernfalls lautet der Standardwert
Produktion bei `https://experience.adobe.com/llm-apps`.
Überprüfen Sie die Produktansprüche in dieser Reihenfolge für diese Umgebung:

1. Die aktuelle Benutzeroberfläche, die durch die Browser-Automatisierung beobachtet wird.
2. Die passende UI- und API-Implementierung im Arbeitsbereich.
3. Die aktuelle öffentliche SDK, die Laufzeitumgebung und das Textbausteinmodell.
4. Vorhandene öffentliche Dokumentation

Zeichnen Sie das Erfassungsdatum und die Quellumgebung im Screenshot-Manifest auf. Nicht
Zeigen Sie lokale oder Staging-Beobachtungen als verifiziertes Produktionsverhalten an. Wenn die Benutzeroberfläche
Die Implementierung und die veröffentlichten Leitlinien stimmen nicht überein, melden die Inkongruenz und beschreiben
Nur das Verhalten, das von den verfügbaren Beweisen unterstützt wird.

## Jede Aufgabe starten

1. `help/main-toc/TOC.md` lesen.
2. Lesen Sie den Target-Artikel und direkt damit zusammenhängende Artikel.
3. Klassifizieren Sie den Inhalt mit [content-model.md](content-model.md).
4. Identifizieren Sie Benutzeroberflächen-Kennzeichnungen, URLs, Befehle und Verträge, die überprüft werden müssen.
5. Beibehaltung der Konsistenz des End-to-End-Beispiels und der Terminologie auf allen Seiten.
6. Wenn für die Aufgabe eine UI-Überprüfung oder Screenshots erforderlich sind, überprüfen Sie die laufende Benutzeroberfläche und erfassen Sie die Bilder selbst. Bitten Sie den Benutzer, Ressourcenänderungen zu authentifizieren oder zu genehmigen, nicht ein Screenshot-Pack zusammenzustellen.

## Authoring-Regeln

- Führen Sie Erstbenutzende durch die Einrichtung **Infrastruktur** und dann **Aktionen erstellen**. Der Assistent zum Erstellen einer App richtet nur die Infrastruktur ein. **[!UICONTROL Aktionen automatisch erstellen]** und **[!UICONTROL Meine Aktionen erstellen]** Beginnen Sie anschließend auf der App-Seite mit der Erstellung einer Website-basierten Aktion. Verwenden Sie **Adobe Managed Infrastructure** für die Option mit der Bezeichnung **[!UICONTROL Let us handle]** in der Benutzeroberfläche.
- Organisieren Sie die Navigation um Journey und Ergebnisse der Benutzenden, nicht um Implementierungsthemen.
- Verwenden Sie am Anfang jedes Handbuchs **In diesem Handbuch:** gefolgt von einem oder zwei einfachen Sätzen, die die Aufgabe und den nächsten Schritt erläutern. „Learn how to…“ Über zukunftssichere Versprechen. Verwenden Sie keine **Journey:**-Beschriftungen oder durch Pfeile getrennte Schrittlisten.
- Verwenden Sie keine internen Codenamen (z. B. „Onboarding-Agent„) in kundenorientierten Dokumenten. Diese Funktion wird nie in der Produkt-Benutzeroberfläche verfügbar gemacht. Beschreiben Sie sie allgemein (z. B. „die Plattform„) und verwenden Sie die aktuelle Kopie der Benutzeroberfläche, z. B **[!UICONTROL „Meine Aktionen erstellen]** für Steuerelemente.
- [!DNL Adobe LLM Apps] ist plattformunabhängig - der MCP-Server funktioniert mit jeder unterstützten LLM-Plattform, nicht nur mit [!DNL ChatGPT]. Formulieren Sie keine allgemeinen oder illustrativen Behauptungen, als ob [!DNL ChatGPT] das einzige Ziel wären (ziehen Sie z. B. „eine unterstützte LLM-Plattform wie [!DNL ChatGPT]&quot; gegenüber „ChatGPT“ allein vor). Nur Name [!DNL ChatGPT] explizit in Inhalten, die wirklich und derzeit [!DNL ChatGPT] sind: der dedizierte [Test im ChatGPT-](/help/guides/test-in-chatgpt.md), seine direkten Querlinks/Verfahrensschritte und [!DNL ChatGPT] Referenz- oder Fehlerbehebungsinhalte.
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

## Redaktionelle Standards

- Wenden Sie sich direkt an den Leser und verwenden Sie die Anweisungen: „Klicken Sie auf Speichern“, nicht „Sie können auf Speichern klicken“.
- Stellen Sie das Ziel vor die Aktion, wenn der Zweck Erklärung benötigt: „Um einen Parameter hinzuzufügen, wählen Sie Hinzufügen aus“. Vermeiden Sie redundante Zweckbestimmungen.
- Aktive Stimme und Präsenz bevorzugen. Benennen Sie den Akteur, wenn es darauf ankommt: „Die Plattform generiert den Handler“.
- Ersetzen Sie mehrdeutige Verweise wie „this“, „these“ oder „it“ durch ein bestimmtes Substantiv, wenn der Verweis unklar ist.
- Behalte eine Grundidee pro Satz. Kürzen Sie lange Sätze und teilen Sie eingebettete Klauseln auf, wenn die zusätzlichen Details die Aufgabe verdecken. Beibehaltung der erforderlichen Bedingungen und Warnungen.
- Reduzieren Sie präpositionale Ketten und dichte Substantivsätze. „Konfigurieren von App-Variablen für das Staging“ wird gegenüber „Durchführen der Konfiguration der Variablen für die App im Staging“ bevorzugt.
- Verwenden Sie einfache Formulierungen anstelle von Idiomen, figurativer Sprache oder unnötigen Formulierungen. Erklären Sie unbekannte technische Begriffe, wenn der Leser sie zum ersten Mal trifft.
- Wortreihenfolge, Satzstruktur, fehlende oder doppelte Wörter, Satzzeichen und Satzanfang überprüfen. Listenelemente grammatisch parallel halten.
- Geben Sie offizielle Produktnamen und Großbuchstaben an: Adobe LLM Apps, Adobe I/O Runtime, Edge Delivery Services, GitHub, ChatGPT, Claude und Model Context Protocol (MCP). Bewahren Sie genaue UI-Bezeichnungen und Code-Schreibweise auf, auch wenn sie sich von den Konventionen für Prosa unterscheiden.
- Verwenden Sie eine inklusive, respektvolle Sprache. Vermeiden Sie Stereotypen, Geschlechterannahmen, stigmatisierende Metaphern und Formulierungen, die davon ausgehen, dass jeder Leser über die gleichen Fähigkeiten oder Erfahrungen verfügt. Verwenden Sie Begriffe wie &quot;Zulassungsliste&quot; und &quot;Blockierungsliste&quot;, sofern zutreffend. Benennen Sie keine literalen APIs, Verzweigungen, Befehle oder Code-Bezeichner für eine redaktionelle Voreinstellung um.
- Modale Verben beibehalten, wenn sie echte Optionen, Funktionen, Berechtigungen, Bedingungen, Unsicherheit oder Host-Variation ausdrücken. Wandeln Sie eine Schätzung nicht in eine Garantie oder eine Empfehlung in eine Anforderung um.
- Bewahren Sie während eines redaktionellen Versuchs exakte UI-Bezeichnungen, Befehle, Code-Beispiele, Kennungen, technische Verträge und PM-genehmigte Kopien auf.
- Verwenden Sie automatisierte Ergebnisse als Überprüfungssignale, nicht als obligatorische Ersetzungen. Suchen Sie den markierten Satz, beheben Sie das eigentliche Problem oder behalten Sie einen aussagekräftigen Wortlaut mit einem Grund bei. Eine generische Grammatikkategorie oder ein aggregierter Wert reicht nicht aus, um einen bestimmten Fehler zu identifizieren. Fordern Sie keine verbesserte Punktzahl an, ohne denselben Gutachter auszuführen.

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

Erfassen Sie Screenshots mit der verfügbaren Browser-Automatisierung selbst. Folgen
[screenshots.md](screenshots.md) für Browser-Einrichtung, Authentifizierung, sichere Navigation,
Stabilitätsprüfungen, Bereinigung und Bildüberprüfung. Verwenden Sie
[screen-manifest.md](screenshot-manifest.md) für aktuelle Checkpoints und Dateinamen.

1. Öffnen Sie die angeforderte aktive Benutzeroberfläche und halten Sie die Benutzerauthentifizierung an.
2. Verwenden Sie eine genehmigte Dokumentationsvorrichtung wieder und überprüfen Sie die erforderlichen Benutzeroberflächenstatus.
3. Erfassen Sie stabile Screenshots im ignorierten `docs-captures/<run-id>/`. Warten Sie nicht auf ein benutzererstelltes Capture Pack.
4. Beschneiden oder maskieren Sie sichere Kopien auf `help/assets/` und überprüfen Sie dann jedes endgültige Bild visuell.
5. Passen Sie den Artikel, die genauen Benutzeroberflächen-Bezeichnungen und den Alternativtext an den beobachteten Status an.
6. Validieren von Links und Assets und Überprüfen der gerenderten Überprüfungs-Site.

Fragen vor dem Erstellen von Apps oder Repositorys, Starten der Generierung, Validieren eines
Repository, das Textbausteindateien empfangen kann, Einstellungen speichern, generierte Zusammenführungen
Anfragen abrufen, bereitstellen, löschen oder Zugriff ändern. Beibehalten der Authentifizierung und
Privilegiertes Einverständnis mit dem Benutzer. Anmeldeinformationen, Token,
Cookies oder Browser-Speicher verwenden und niemals einen mutierenden Erfassungsfluss ohne Genehmigung ausführen.

Wenn keine Automatisierung, kein Zugriff oder ein erforderlicher Status verfügbar ist, melden Sie den jeweiligen Status
Blocker. Erfinden Sie keinen Screenshot, verwenden Sie kein veraltetes Bild im Hintergrund wieder oder präsentieren Sie es
Nur-Quelle-Verifizierung als neue UI-Erfassung. Vorhandene Quellaufnahmen beibehalten.

## Erstellen eines Überprüfungsarchivs

Erstellen einer freigebbaren Offline-HTML-Site und eines ZIP-Archivs:

```bash
node .github/skills/llm-apps-docs/scripts/build_review_bundle.mjs
```

Der Build wird neben dem Repository geschrieben, nicht in ihm. Es umfasst nur
Veröffentlichte Artikel und bereinigte Assets, wandelt Experience League-Anweisungen um
für die Offline-Überprüfung und stellt fest, dass die Formatierung nicht das endgültige Erlebnis ist
League-Rendering.

## Validieren

Ausführen:

```bash
python3 .github/skills/llm-apps-docs/scripts/validate_docs.py
```

Korrigieren Sie alle fehlenden internen Artikel, fehlenden Assets, ungültigen Stammpfad und fehlenden Frontend-Felder, bevor Sie sie weitergeben.

Überprüfen Sie auch:

- Benutzeroberflächen-Beschriftungen und Screenshots stimmen mit der aufgezeichneten Quellumgebung überein.
- Skript- und Widget-URL-Beispiele stimmen in Handbüchern und Referenzen überein.
- Befehle entsprechen dem aktuellen Textbaustein.
- Neue Seiten werden vom Inhaltsverzeichnis verlinkt, sofern nicht ausdrücklich `hidefromtoc: true` gekennzeichnet.
- Der Adobe-Workflow für die Artikelvalidierung verläuft erfolgreich, sofern verfügbar.

## Unterstützende Referenzen

- [Inhaltsmodell und Terminologie](content-model.md)
- [Agent-gesteuertes Screenshot-Verfahren](screenshots.md)
- [Screenshot-Manifest](screenshot-manifest.md)
