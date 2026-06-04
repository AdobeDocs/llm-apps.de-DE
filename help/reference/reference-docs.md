---
title: Verweis
description: Referenz auf Feldebene für die Aktionskonfiguration in der Adobe LLM Apps-Benutzeroberfläche.
source-git-commit: 483a71f5f1de5caf1bd89b26f4d67d2d5a0aa15a
workflow-type: tm+mt
source-wordcount: '489'
ht-degree: 6%

---


# Referenz

>[!IMPORTANT]
>
>**Haftungsausschluss:** Dies ist eine Beta-Version von [!DNL LLM Apps]. Die hier gezeigten Funktionen, Workflows und Benutzeroberflächen stellen nicht unbedingt den endgültigen Status der Anwendung oder des Produkts dar.

Dieser Abschnitt enthält Informationen auf Feldebene zur Konfiguration von Aktionen in der [!DNL LLM Apps]-Benutzeroberfläche.

## Aktionsparameter

Eingabeparameter sind die Werte, die die LLM-Plattform ([!DNL ChatGPT], Claude) an Ihren Aktions-Handler sendet. Das Modell extrahiert sie aus der Nachricht des Benutzers und ordnet sie automatisch diesen Feldern zu.

| Eigenschaft | Beschreibung |
|----------|-------------|
| **Name** | Die Parameterkennung (z. B. `category`, `query`) |
| **Typ** | `String`, `Number`, `Integer` oder `Boolean` |
| **Beschreibung** | Eine für Menschen lesbare Erklärung: Die LLM-Plattform verwendet diese, um den richtigen Wert zu extrahieren |
| **Erforderlich** | Falls aktiviert, muss das Modell diesen Parameter bereitstellen, bevor die Aktion aufgerufen wird |

### Dateiparameter

Dateiparameter enthalten Dateiobjekte mit `download_url`- und `file_id`. Definieren Sie Eingabefeldnamen, die Dateidaten erhalten sollen, wenn ein Benutzer eine Datei in die Konversation hochlädt.

## Metadatenfelder

### Grundlegende Informationen

| Feld | Erforderlich | Beschreibung |
|-------|----------|-------------|
| **Aktionsname** | Ja | Kennung für die Aktion (z. B *„Produkte suchen*) |
| **Beschreibung** | Ja | Erläuterung der Aktion - Die LLM-Plattform entscheidet hierüber, wann sie aufgerufen wird |

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

### Sichtbarkeit

| Umschalter | Beschreibung |
|--------|-------------|
| **KI-Modell bereitstellen** | Die Aktion kann vom KI-Modell während Konversationen aufgerufen werden |
| **Als Widget in Programmoberfläche anzeigen** | Die Aktion rendert ein visuelles Widget in der App |

### Widget-Informationen

| Feld | Beschreibung |
|-------|-------------|
| **Typ** | Widget-Technologie — derzeit **[!UICONTROL EDS]** |
| **Widget-Domain (Sandbox-Herkunft)** | Herkunft, in der das Widget gehostet wird; muss pro App eindeutig sein |
| **Bevorzugter Rahmen** | Wenn diese Option aktiviert ist, wird das Widget innerhalb einer eingefassten Karte in der LLM-Plattform gerendert |

### Vorlagen-URLs

| Feld | Beschreibung |
|-------|-------------|
| **[!UICONTROL Skript-URL]** | Einstiegspunktskript - `https://main--<repo>--<owner>.aem.live/scripts/aem-embed.js`. Shared across all actions |
| **Widget-Einbettungs-URL** | EDS-Seite für diese Aktion — `https://main--<repo>--<owner>.aem.live/eds-widgets/<action-name>`. Eindeutig pro Aktion |

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

