---
source-git-commit: a48b4022979d9a13b4fc3aa870c4ee6375e8bc13
workflow-type: tm+mt
source-wordcount: '701'
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

First-Run-Journey: `help/guides/create-app.md` (Infrastruktureinrichtung), dann `help/guides/creating-actions.md` (Aktionserstellung und -überprüfung).

### Konzept

Erläutert die Beziehung von Teilen, ohne dass sie zu einem Vorgangs- oder Feldkatalog werden.

- Konzentration auf mentale Modelle und Eigentumsgrenzen.
- Erläutern Sie, was die einzelnen Teile bewirken, wie sie den Benutzenden angezeigt werden und wo Änderungen vorgenommen werden können.
- Behalten Sie Build-Tools, Dateipfade, Protokollaufrufe und Code-Verträge in den verknüpften technischen Handbüchern oder Referenzen bei, nicht in der allgemeinen Übersicht.
- Verwenden Sie ein kleines Diagramm, wenn es das Verständnis verbessert.
- Beibehalten nützlicher vorhandener Diagramme beim Vereinfachen einer Seite; Vereinfachen der Beschriftungen, anstatt die visuelle Erläuterung zu entfernen.
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
- **Adobe Managed Infrastructure** - Testinfrastruktur, die von Adobe erstellt und gehostet wird. Wählen Sie **Let us handle** im Create-App-Assistenten aus.
- **Ich richte es selbst ein** - Assistentenoption zum Verbinden Ihrer eigenen Repositorys, keine Anforderung, Aktionen manuell zu schreiben. Der Standardpfad beginnt mit zwei leeren Repositorys, die von der Plattform initialisiert werden. Kompatible vorhandene Projekte sind optional.
- **Aktionen automatisch erstellen** - Empfohlene Auswahl zur Aktionserstellung auf der App-Seite nach der Einrichtung der Infrastruktur.
- **Build my actions** - Schaltfläche, die die Erstellung von Web-basierten Aktionen startet.
- **Action** - Funktion, die der LLM-Plattform bereitgestellt wird.
- **Aktionsmetadaten** - Name, Beschreibung, Schema, Anmerkungen, Sichtbarkeit und Widget-Konfiguration, die von LLM-Apps gespeichert werden.
- **Sichtbarkeit** - der Ein-/Ausschalter für die Aktion der App-Seite. Sie steuert, ob die nächste Bereitstellung die Aktion enthält. Der Aktionseditor hat keinen separaten aktivierten Schalter. Seine Modell- und Widget-Belichtungseinstellungen dienen einem anderen Zweck.
- **Action Handler** - Server-seitige Funktion im Handler-Repository.
- **Code-Kennung** - Unveränderliche Aktionskennung, die in MCP-Aufrufen verwendet wird und sich vom Anzeigenamen unterscheidet. Die generierten Handler- und Blockordnernamen verwenden ein Formular mit Bindestrichen. Die Laufzeit löst auch Unterstriche in der Kennung in Bindestriche in Handlerpfaden auf.
- **Handler-Repository** - Repository mit Handlern und Tests, die beim Einrichten der **-** beim Verbinden Ihrer eigenen Repositorys ausgewählt wurden.
- **EDS-Repository** - Repository, das Widget-Blöcke und Inhalte enthält.
- **Widget** — visuelle Antwort, die in der LLM-Plattform gerendert wird.
- **`structuredContent`** - Vom Widget verwendete Daten zu strukturierten Aktionsergebnissen. Beschreiben Sie sie nicht als privat, nicht modell-sichtbar oder ohne Token. Sie ist Teil des MCP-Ergebnisses, das an den Host übermittelt wird.
- **MCP Server URL** - Bereitgestellter Endpunkt, der bei einer LLM-Plattform registriert ist.
- **ChatGPT-Plug** - die ChatGPT-Integration, die aus einer MCP-Server-URL erstellt wurde.
- **Staging** und **Produktion** Bereitstellungsumgebungen.

Wechseln Sie nicht in einem Prosa mit Benutzerzugriff zwischen „tool“ und „action“, es sei denn, Sie erklären ein MCP-Protokolldetail.

Das Produkt ist plattformunabhängig: Der MCP-Server funktioniert mit jeder unterstützten LLM-Plattform, nicht nur mit ChatGPT. Verwenden Sie „eine unterstützte LLM-Plattform wie ChatGPT“ (oder Ähnliches) für allgemeine oder anschauliche Behauptungen. Benennen Sie ChatGPT nur in Inhalten, die heute wirklich ChatGPT-spezifisch sind - dem Test-in-ChatGPT-Handbuch, seinen direkten Querlinks und ChatGPT-spezifischen Referenz- oder Fehlerbehebungsinhalten.

## Empfohlene Lesersequenz

1. Überblick.
2. Erstellen Sie eine App mit der von Adobe verwalteten Infrastruktur oder Ihren eigenen Repositorys.
3. Starten Sie die Erstellung einer Website-basierten Aktion über die App-Seite und überprüfen Sie die generierten Aktionen.
4. Staging- und Testbereitstellung in einer unterstützten LLM-Plattform.
5. Generierte Handler und Widgets anpassen.
6. Stellen Sie die angepasste App für die Produktion bereit.

Das Erstellen einer neuen Aktion und das Einbringen eines EDS-Projekts sind erweiterte Verzweigungen und nicht die standardmäßige Erstausführungs-Journey.

Der BYO EDS Guide ist derzeit mit `hidefromtoc: true` im Inhaltsverzeichnis ausgeblendet.
Behalten Sie diesen Status bei, es sei denn, der Benutzer fragt nach der Wiederherstellung des Navigationseintrags.
