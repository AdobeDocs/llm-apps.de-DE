---
source-git-commit: bb3d8a02f22a91ceeeba5999453aeb4221060f80
workflow-type: tm+mt
source-wordcount: '436'
ht-degree: 0%

---
# Inhaltsmodell und Terminologie

## Inhaltstypen

### Tutorial

Bringt einem neuen Anwender eine komplette, erfolgreiche Journey bei.

- Geben Sie das Ergebnis und die Voraussetzungen an.
- Verwenden Sie eine Beispielanwendung und eine Sequenz.
- Erklären Sie nur die bei jedem Schritt erforderlichen Konzepte.
- Beenden Sie mit einem Arbeitsergebnis und löschen Sie die nächsten Schritte.

Primäres Tutorial: `help/guides/create-app.md`.

### Konzept

Erläutert die Beziehung von Teilen, ohne dass sie zu einem Vorgangs- oder Feldkatalog werden.

- Konzentration auf mentale Modelle und Eigentumsgrenzen.
- Verwenden Sie ein kleines Diagramm, wenn es das Verständnis verbessert.
- Link zu Tutorials, Anleitungen und Referenzen.

### Anleitung

Hilft einem informierten Benutzer, eine Aufgabe auszuführen.

- Beginnen Sie mit dem gewünschten Ergebnis.
- Nur für die Aufgabe spezifische Voraussetzungen einbeziehen.
- Einen empfohlenen Pfad bevorzugen.
- Link zur Referenz für vollständige Felder.

Beispiele: Erstellen einer neuen Aktion, Anpassen eines Widgets, Einbringen eines EDS-Projekts, Bereitstellen und Testen.

### Referenz

Stellt sachliche Informationen bereit, die Benutzende während der Arbeit konsultieren.

- Organisieren nach Produkt- oder Code-Oberfläche.
- Definieren Sie jedes Feld, jeden Vertrag, jeden Befehl, jedes Limit und jeden Status präzise.
- Vermeiden Sie Tutorial-Erzählungen und wiederholte Beispiele.

### Fehlerbehebung

Beginnt mit einem beobachtbaren Symptom.

- Beschreiben Sie wahrscheinliche Ursachen.
- Geben Sie sichere Diagnoseschritte.
- Vermeiden Sie es, Benutzer aufzufordern, Anmeldeinformationen oder vertrauliche Protokolle anzuzeigen.

## Kanonische Terminologie

- **Adobe LLM Apps** - Vollständiger Produktname bei der ersten Erwähnung.
- **LLM App** - eine vom Produkt verwaltete App.
- **Abschnitt „Meine App erstellen** - Benutzeroberfläche im Dialogfeld „App erstellen“.
- **Meine App automatisch erstellen** - Genaue Checkbox-Bezeichnung.
- **Action** - Funktion, die der LLM-Plattform bereitgestellt wird.
- **Aktionsmetadaten** - Name, Beschreibung, Schema, Anmerkungen, Sichtbarkeit und Widget-Konfiguration, die von LLM-Apps gespeichert werden.
- **Action Handler** - Server-seitige Funktion im Handler-Repository.
- **Handler-Repository** - Repository, das Handler und Tests enthält. Verwenden Sie die UI **Bezeichnung „Textbausteinrepository** nur bei der Beschreibung dieses Steuerelements.
- **EDS-Repository** - Repository, das Widget-Blöcke und Inhalte enthält.
- **Widget** — visuelle Antwort, die in der LLM-Plattform gerendert wird.
- **MCP Server URL** - Bereitgestellter Endpunkt, der bei einer LLM-Plattform registriert ist.
- **ChatGPT-Plug** - die ChatGPT-Integration, die aus einer MCP-Server-URL erstellt wurde.
- **Staging** und **Produktion** Bereitstellungsumgebungen.

Wechseln Sie nicht in einem Prosa mit Benutzerzugriff zwischen „tool“ und „action“, es sei denn, Sie erklären ein MCP-Protokolldetail.

Das Produkt ist plattformunabhängig: Der MCP-Server funktioniert mit jeder unterstützten LLM-Plattform, nicht nur mit ChatGPT. Verwenden Sie „eine unterstützte LLM-Plattform wie ChatGPT“ (oder Ähnliches) für allgemeine oder anschauliche Behauptungen. Benennen Sie ChatGPT nur in Inhalten, die heute wirklich ChatGPT-spezifisch sind - dem Test-in-ChatGPT-Handbuch, seinen direkten Querlinks und ChatGPT-spezifischen Referenz- oder Fehlerbehebungsinhalten.

## Empfohlene Reader-Journey

1. Übersicht und Voraussetzungen.
2. Automatisches Erstellen einer App.
3. Überprüfen Sie die generierten Aktionen.
4. Bereitstellen in der Staging-Umgebung und Testen des ChatGPT-Plug-ins.
5. Generierte Handler und Widgets anpassen.
6. Stellen Sie die angepasste App für die Produktion bereit.

Das Erstellen einer neuen Aktion und das Einbringen eines EDS-Projekts sind erweiterte Verzweigungen und nicht die standardmäßige Erstausführungs-Journey.
