---
title: Übersicht über Adobe LLM-Apps
description: Erfahren Sie, was Adobe LLM-Apps sind, wie sie funktionieren und wie Sie mit der Erstellung Ihrer App beginnen.
source-git-commit: 14d94a68dabddd2f2ef2cef65c51d31645d4a958
workflow-type: tm+mt
source-wordcount: '606'
ht-degree: 1%
---

# Adobe LLM Apps - ein Überblick {#adobe-llm-apps-an-overview}

>[!IMPORTANT]
>
>[!DNL Adobe LLM Apps] befindet sich derzeit in Beta.
>
>Die hier gezeigten Funktionen, Workflows und Benutzeroberflächen stellen nicht unbedingt den endgültigen Status des Produkts dar. Um Beta beizutreten, senden Sie eine E-Mail an llm-apps-beta@adobe.com.

## Was ist [!DNL Adobe LLM Apps]?

[!DNL Adobe LLM Apps] ermöglicht es Ihrer Marke, innerhalb unterstützter KI-Assistenten wie [!DNL ChatGPT] und [!DNL Claude] nützliche Aktionen anzubieten, z. B. Produkterkennung, Verfügbarkeitsprüfungen oder Service-Buchungen.

[!DNL LLM Apps] ist verfügbar unter [experience.adobe.com/llm-apps](https://experience.adobe.com/llm-apps).

## Was man mit [!DNL LLM Apps] machen kann

- **Erstellen von markeneigenen LLM-Aktionen** - Definieren Sie die spezifischen Geschäftsabläufe, die Sie in KI-Assistenten aktivieren möchten (z. B. *Planen einer Testfahrt*, *Produkte vergleichen*, *Service buchen*).
- **Interaktive LLM-Widgets erstellen** - Erstellen Sie visuelle UI-Komponenten (Produktkarten, Buchungsformulare, Store-Locators), die als AEM-Komponenten in Ihrem [!DNL GitHub]-Repository verwaltet werden.
- **Zentrale Markenverwaltung** - Autoren und Entwickler behalten die volle Kontrolle über alle Inhalte, Kopien und Visualisierungen, die in der LLM-Plattform bereitgestellt werden. Genehmigungen werden über AEM verwaltet.
- **In Staging- und Produktionsumgebung bereitstellen** - Eine gesteuerte Bereitstellungs-Pipeline ermöglicht es Ihnen, das Erlebnis in einer Staging-Umgebung zu testen, bevor Sie zur Produktion weiterleiten.
- **Kontrollieren der Sichtbarkeit auf Aktionsebene** — Aktivieren oder deaktivieren Sie einzelne Aktionen und stellen Sie dann die App bereit, um die Änderung anzuwenden.
- **Messen, was Entscheidungen**: Integrierte Analyse (unterstützt von Adobe Customer Journey Analytics), Anzahl der Oberflächenaktionen, Erfolgsraten, Abbruchraten, Top-Benutzeraufforderungen und Sichtbarkeitsbewertung.

## Warum [!DNL LLM Apps] wichtig sind

LLM-Interaktionen unterscheiden sich grundlegend von der herkömmlichen Suche. Die durchschnittliche Dauer einer LLM-Sitzung ist viermal so lang wie bei einer herkömmlichen Suchsitzung. Mehr als 40 % der Verbraucher verlassen sich bei komplexen Kaufentscheidungen auf KI-Tools. Ohne [!DNL LLM Apps] könnten Sie die Erwähnung gewinnen, aber den Kunden verlieren. [!DNL LLM Apps] stellt sicher, dass Ihre Marke nicht nur sichtbar, sondern genau in dem Moment umsetzbar ist, in dem ein Benutzer bereit ist, eine Entscheidung zu treffen.

## Wichtige Konzepte {#key-concepts}

### LLM-App

Ihr gebrandeter Assistent, mit dem Benutzer innerhalb von [!DNL ChatGPT] oder anderen LLM-Plattformen interagieren. Es fasst alle Aktionen zusammen und wird als eine Einheit bereitgestellt.

### Aktion {#actions}

Eine Funktion, die Ihre App bietet, z *B. „Händler suchen* oder *Produkte durchsuchen*. Die LLM-Plattform ruft eine Aktion auf, wenn eine Anfrage mit ihrer Beschreibung übereinstimmt. [!DNL LLM Apps] verwaltet Aktionsmetadaten, und das [!DNL GitHub]-Repository speichert den Handler-Code.

### Aktions-Handler

Die Server-seitige Funktion, die ausgeführt wird, wenn die LLM-Plattform eine Aktion aufruft. Verwenden Sie den -Handler, um Eingaben zu validieren, Ihre APIs aufzurufen und Text sowie strukturierte Daten zurückzugeben.

### Widget {#widgets-eds}

Die visuelle Antwort, die mit der Antwort des LLM angezeigt wird, z. B. eine Karte, ein Karussell oder eine Tabelle. Generierte Widgets sind Blöcke in einem [!DNL Edge Delivery Services]-Repository (EDS). Mit der von Adobe verwalteten Infrastruktur bereitet Adobe das Repository für Sie vor und hostet es. Alternativ können Sie Ihr eigenes Repository verbinden.

### MCP-Server

Der Endpunkt, der nach der Bereitstellung verfügbar gemacht wird. Eine unterstützte LLM-Plattform stellt eine Verbindung zu diesem Endpunkt her, um Ihre Aktionen zu ermitteln und aufzurufen.

## Funktionsweise

Sie definieren die Funktionen Ihrer Marke in [!DNL LLM Apps]. Die Plattform stellt diese Funktionen als Aktionen zur Verfügung, und ein KI-Assistent verwendet die Aktionen, um Kundenanfragen im Gespräch zu beantworten.

```
┌────────────────────┐          ┌────────────────────┐          ┌────────────────────┐
│     Your brand     │          │      LLM Apps      │          │    AI assistant    │
│                    │          │                    │          │                    │
│   What you offer   │   ───▶   │  Turns that into   │   ───▶   │ Answers with your  │
│  and how you help  │          │    something AI    │          │    brand, live     │
│  customers today   │          │     can act on     │          │  inside the chat   │
└────────────────────┘          └────────────────────┘          └────────────────────┘
```

Technische Details dazu, was erstellt werden soll und wie die Komponenten zusammenarbeiten, finden Sie unter [Wie eine App verkabelt ist](/help/guides/app-architecture.md).

## Erste Schritte {#choose-your-journey}

Das Erstellen einer App und das Erstellen ihrer Aktionen sind separate Schritte:

1. [Richten Sie die Infrastruktur ](/help/guides/create-app.md). Verwenden Sie die von Adobe verwaltete Infrastruktur für eine schnelle Einrichtung oder verwenden Sie Ihre eigenen [!DNL GitHub]-Repositorys.
2. [Erstellen Sie Ihre Aktionen](/help/guides/creating-actions.md). Lassen Sie die Plattform Ihre Website analysieren und Aktionen generieren oder erstellen Sie sie selbst.
3. [Bereitstellen und Testen der App](/help/guides/deploy-your-app.md). Überprüfen Sie die generierte Arbeit, stellen Sie sie in einer unterstützten LLM-Plattform bereit und testen Sie sie.

Die automatische Generierung bietet einen Ausgangspunkt mit dem Beispielverhalten. Bevor Sie die App für Kunden verfügbar machen, [ Sie Ihre Handler mit Ihren Systemen ](/help/guides/customize-handler.md) und [ Sie die Widgets ](/help/guides/widgets.md).
