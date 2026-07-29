---
title: Aktions- und Widget-Felder
description: Felddefinitionen für Aktionsmetadaten, Parameter, Widgets, CSP und Berechtigungen in Adobe LLM-Apps.
source-git-commit: eec74b87457bc852d7a8dd0e46c2a4385a93ae0a
workflow-type: tm+mt
source-wordcount: '606'
ht-degree: 5%

---


# Aktionen- und Widget-Felder {#action-widget-configuration}

>[!IMPORTANT]
>
>[!DNL Adobe LLM Apps] befindet sich derzeit in Beta.
>
>Die hier gezeigten Funktionen, Workflows und Benutzeroberflächen stellen nicht unbedingt den endgültigen Status des Produkts dar. Um Beta beizutreten, senden Sie eine E-Mail an llm-apps-beta@adobe.com.

Auf dieser Seite können Sie Felder im Aktionseditor nachschlagen. Die vollständige Erstellungs-Journey finden Sie unter [Erstellen einer neuen Aktion](/help/guides/create-action.md).

## Aktionsparameter

Eingabeparameter sind die Werte, die die LLM-Plattform an Ihren Aktions-Handler sendet. Das Modell extrahiert sie aus der Nachricht des Benutzers und ordnet sie diesen Feldern zu.

| Eigenschaft | Beschreibung |
|----------|-------------|
| **Name** | Die Parameterkennung (z. B. `category`, `query`) |
| **Typ** | `String`, `Number`, `Integer` oder `Boolean` |
| **Beschreibung** | Eine für Menschen lesbare Erklärung: Die LLM-Plattform verwendet diese, um den richtigen Wert zu extrahieren |
| **Erforderlich** | Falls aktiviert, muss das Modell diesen Parameter bereitstellen, bevor die Aktion aufgerufen wird |

### Dateiparameter

Dateiparameter sind im Aktionseditor konfigurierte Eingabefeldnamen. Wenn ein Benutzer eine Datei hochlädt, stellt der Host ein Dateiobjekt für diese Argumente bereit, normalerweise einschließlich `download_url` und `file_id`.

## Metadatenfelder

### Grundlegende Informationen

| Feld | Erforderlich | Beschreibung |
|-------|----------|-------------|
| **Aktionsname** | Ja | Anzeigename für die Aktion (z. B. *Produkte suchen*) |
| **Beschreibung** | Ja | Erläuterung der Aktion - Die LLM-Plattform entscheidet hierüber, wann sie aufgerufen wird |

Nach der Erstellung zeigt der Editor auch eine unveränderliche **Code-Kennung** an. Dabei wird die Aktion dem `actions/<code-identifier>/index.js` im Handler-Repository zugeordnet.

### Anmerkungen

Optionale Hinweise, die das Verhalten der Aktion beschreiben:

| Anmerkung | Beschreibung |
|------------|-------------|
| **Destruktiver Hinweis** | Die Aktion ändert oder löscht Daten |
| **idempotent** | Ein mehrmaliges Aufrufen der Aktion mit denselben Argumenten hat dasselbe Ergebnis |
| **Open world hint** | Die Aktion interagiert mit externen Systemen |
| **Schreibgeschützter Hinweis** | Die Aktion liest nur Daten, schreibt nie |

### OpenAI-Metadaten

| Feld | Max. Länge | Beschreibung |
|-------|------------|-------------|
| **Aufrufen des Statustextes** | 64 Zeichen | Meldung, die auf der LLM-Plattform während der Ausführung der Aktion angezeigt wird (z. B. *Produkte werden geladen …* ) |
| **Aufgerufener Statustext** | 64 Zeichen | Meldung, die angezeigt wird, nachdem die Aktion abgeschlossen ist (z. B *„Produkte geladen …* ) |
| **Widget-Beschreibung** | 512 Zeichen | Ordnet `_meta["openai/widgetDescription"]` zu. Fasst die gerenderte Komponente für das Modell zusammen und reduziert wiederholte Aussagen. |

Die Aktionsbeschreibung steuert, wann das Modell die Aktion auswählt. In der Widget-Beschreibung wird erläutert, was die Komponente nach dem Rendern anzeigt.

### Sichtbarkeit

| Umschalter | Beschreibung |
|--------|-------------|
| **KI-Modell bereitstellen** | Die Aktion kann vom KI-Modell während Konversationen aufgerufen werden |
| **Als Widget in Programmoberfläche anzeigen** | Die Aktion rendert ein visuelles Widget in der App |

### Analytics

| Feld | Beschreibung |
|-------|-------------|
| **Benutzerabsicht erfassen** | Erfasst eine Zusammenfassung der Konversation, die zur Aktion für Analytics geführt hat |

## Widget-Felder

### Widget-Informationen

| Feld | Beschreibung |
|-------|-------------|
| **Typ** | Widget-Technologie — derzeit **[!UICONTROL EDS]** |
| **Widget-Domain (Sandbox-Herkunft)** | Herkunft, in der das Widget gehostet wird; muss pro App eindeutig sein |
| **Bevorzugter Rahmen** | Wenn diese Option aktiviert ist, wird das Widget innerhalb einer eingefassten Karte in der LLM-Plattform gerendert |

### Vorlagen-URLs

| Feld | Beschreibung |
|-------|-------------|
| **[!UICONTROL Skript-URL]** | HTTPS-URL für den EDS-`scripts/aem-embed.js`. Gemeinsam genutzt über Aktionen im selben EDS-Projekt |
| **Widget-URL** | HTTPS-URL für die EDS-Seite, die durch diese Aktion gerendert wird. Erzeugte Aktionen konfigurieren dies automatisch |

## CSP-Konfiguration

Die Content Security Policy steuert, an welche externen Domains der Widget-IFrame sich wenden darf. Jede externe Domain muss explizit angegeben werden.

| Anweisung | Beschreibung |
|-----------|-------------|
| **Ressourcendomänen** | Domains für statische Assets: Bilder, Schriftarten, Skripte, Stile |
| **Verbinden von Domains** | Domains, mit denen das Widget über `fetch`, `XHR` oder `WebSocket` Kontakt aufnehmen kann |
| **Frame-Domains** | Zulässige Ursprünge für verschachtelte iFrames; Trigger strengere App-Überprüfung |
| **Umleitungs-Domains** | Vertrauenswürdige Ziele für `openExternal` Umleitungs-Links ([!DNL ChatGPT]) |
| **Basis-URI-Domains** | Die `base-uri` CSP-Direktive (nur MCP Apps SDK, nicht [!DNL ChatGPT]) |

## Berechtigungen

Hardware- und Browser-APIs, auf die das Widget zugreifen darf. Diese werden der iframe-Berechtigungsrichtlinie zugeordnet.

| Berechtigung | Beschreibung |
|------------|-------------|
| **Kamera** | Zugriff auf die Gerätekamera |
| **Mikrofon** | Zugriff auf das Gerätemikrofon |
| **Geolocation** | Zugriff auf den Standort des Benutzers |
| **Zwischenablage** | Aus der Zwischenablage lesen oder in die Zwischenablage schreiben |

