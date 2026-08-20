---
title: Übersicht über Adobe LLM-Apps
description: Erfahren Sie, was Adobe LLM-Apps sind, wie sie funktionieren und was Sie benötigen, um loszulegen.
source-git-commit: 2f3480b3667a6ab7c4ed65b999eed4638c383edb
workflow-type: tm+mt
source-wordcount: '969'
ht-degree: 1%

---


# Adobe LLM Apps - ein Überblick {#adobe-llm-apps-an-overview}

>[!IMPORTANT]
>
>[!DNL Adobe LLM Apps] befindet sich derzeit in Beta.
>
>Die hier gezeigten Funktionen, Workflows und Benutzeroberflächen stellen nicht unbedingt den endgültigen Status des Produkts dar. Um Beta beizutreten, senden Sie eine E-Mail an llm-apps-beta@adobe.com.

## Was ist [!DNL Adobe LLM Apps]?

[!DNL Adobe LLM Apps] ermöglicht es Ihrer Marke, innerhalb von KI-Assistenten wie [!DNL ChatGPT] nützliche Aktionen anzubieten, z. B. Produkterkennung, Verfügbarkeitsprüfungen oder Service-Buchungen.

[!DNL LLM Apps] ist verfügbar unter [experience.adobe.com](https://experience.adobe.com/#/@llmapps/llm-apps/).

## Was man mit [!DNL LLM Apps] machen kann

- **Erstellen von markeneigenen LLM-Aktionen** - Definieren Sie die spezifischen Geschäftsabläufe, die Sie in KI-Assistenten aktivieren möchten (z. B. *Planen einer Testfahrt*, *Produkte vergleichen*, *Service buchen*).
- **Interaktive LLM-Widgets erstellen** - Erstellen Sie visuelle UI-Komponenten (Produktkarten, Buchungsformulare, Store-Locators), die als AEM-Komponenten in Ihrem [!DNL GitHub]-Repository verwaltet werden.
- **Zentrale Markenverwaltung** - Autoren und Entwickler behalten die volle Kontrolle über alle Inhalte, Kopien und Visualisierungen, die in der LLM-Plattform bereitgestellt werden. Genehmigungen werden über AEM verwaltet.
- **In Staging- und Produktionsumgebung bereitstellen** - Eine gesteuerte Bereitstellungs-Pipeline ermöglicht es Ihnen, das Erlebnis in einer Staging-Umgebung zu testen, bevor Sie zur Produktion weiterleiten.
- **Kontrollieren der Sichtbarkeit auf Aktionsebene** - Nach der Bereitstellung können einzelne Aktionen aktiviert oder deaktiviert werden, ohne dass die gesamte App neu bereitgestellt wird.
- **Messen, was Entscheidungen**: Integrierte Analyse (unterstützt von Adobe Customer Journey Analytics), Anzahl der Oberflächenaktionen, Erfolgsraten, Abbruchraten, Top-Benutzeraufforderungen und Sichtbarkeitsbewertung.

## Warum [!DNL LLM Apps] wichtig sind

LLM-Interaktionen unterscheiden sich grundlegend von der herkömmlichen Suche. Die durchschnittliche Dauer einer LLM-Sitzung ist viermal so lang wie bei einer herkömmlichen Suchsitzung. Mehr als 40 % der Verbraucher verlassen sich bei komplexen Kaufentscheidungen auf KI-Tools. Ohne [!DNL LLM Apps] könnten Sie die Erwähnung gewinnen, aber den Kunden verlieren. [!DNL LLM Apps] stellt sicher, dass Ihre Marke nicht nur sichtbar, sondern genau in dem Moment umsetzbar ist, in dem ein Benutzer bereit ist, eine Entscheidung zu treffen.

## Wichtige Konzepte {#key-concepts}

### LLM-App

Ihr gebrandeter Assistent, mit dem Benutzer innerhalb von [!DNL ChatGPT] oder anderen LLM-Plattformen interagieren. Es fasst alle Aktionen zusammen und wird als eine Einheit bereitgestellt.

### Aktion {#actions}

Eine Funktion, die Ihre App bietet, z *B. „Händler suchen* oder *Produkte durchsuchen*. Die LLM-Plattform ruft eine Aktion auf, wenn eine Anfrage mit ihrer Beschreibung übereinstimmt. Aktionsmetadaten werden in [!DNL LLM Apps] verwaltet, während ihr Handler Code in Ihrem [!DNL GitHub]-Repository ist.

### Aktions-Handler

Die Server-seitige Funktion, die ausgeführt wird, wenn eine Aktion aufgerufen wird. Er kann Eingaben validieren, APIs aufrufen und Text sowie strukturierte Daten zurückgeben.

### Widget {#widgets-eds}

Die visuelle Antwort, die mit der Antwort des LLM angezeigt wird, z. B. eine Karte, ein Karussell oder eine Tabelle. Generierte Widgets sind Blöcke in einem [!DNL Edge Delivery Services]-Repository (EDS) in Ihrem Besitz.

### MCP-Server

Der Endpunkt, der nach der Bereitstellung verfügbar gemacht wird. Eine unterstützte LLM-Plattform stellt eine Verbindung zu diesem Endpunkt her, um Ihre Aktionen zu ermitteln und aufzurufen.

## Funktionsweise

Drei Dinge passieren auf allgemeiner Ebene: Sie sagen [!DNL LLM Apps], was Ihre Marke anbietet, es verwandelt sich in etwas, auf das ein KI-Assistent reagieren kann, und Ihr Kunde erhält eine echte Antwort - direkt innerhalb des Chats.

```
┌────────────────────┐          ┌────────────────────┐          ┌────────────────────┐
│     Your brand     │          │      LLM Apps      │          │    AI assistant    │
│                    │          │                    │          │                    │
│   What you offer   │   ───▶   │  Turns that into   │   ───▶   │ Answers with your  │
│  and how you help  │          │    something AI    │          │    brand, live     │
│  customers today   │          │     can act on     │          │  inside the chat   │
└────────────────────┘          └────────────────────┘          └────────────────────┘
```

Wollten Sie die technischen Details — was Sie bauen, und wie die Einzelteile zusammenpassen? Siehe [Wie eine App verkabelt ist](/help/guides/app-architecture.md).

## Voraussetzungen {#requirements}

Führen Sie alle folgenden Anforderungen aus, bevor Sie eine App erstellen.

### Adobe-Entwicklerkonsole

Ihre Adobe IMS-Organisation muss Zugriff auf [[!DNL App Builder]](https://developer.adobe.com/app-builder/docs/intro_and_overview/) haben. Sie benötigen die Rolle **Entwickler** oder **Systemadministrator**.

Öffnen Sie [Adobe Developer Console, um Ihren Zugriff zu ](https://developer.adobe.com/console). Der Schnellstartbildschirm bestätigt, dass Sie über den erforderlichen Zugriff verfügen.

![Adobe Developer Console - Schnellstartbildschirm, der den Entwicklerzugriff bestätigt](/help/assets/overview/dev-console-access-granted.png)

Wenn Sie **Eingeschränkter Zugriff** sehen, wenden Sie sich an den Administrator Ihrer IMS-Organisation und fordern Sie die Rolle Entwickler an.

![Adobe Developer Console - Nachricht zu eingeschränktem Zugriff](/help/assets/overview/dev-console-access-denied.png)

### [!DNL GitHub]

Sie benötigen ein [!DNL GitHub] Konto, **Folgendes** kann. Dies ist eine Berechtigungsprüfung - installieren Sie noch nichts:

- Erstellen Sie zwei Repositorys in dem Konto oder der Organisation, dem bzw. der die App gehören wird.
- Installieren Sie [!DNL GitHub] Apps zu einem späteren Zeitpunkt im Einrichtungsprozess oder haben Sie einen Organisationsadministrator, der sie genehmigen kann.

Um den Zugriff auf die Repository-Erstellung zu überprüfen, öffnen Sie [github.com/new](https://github.com/new) und vergewissern Sie sich, dass das gewünschte Konto oder die gewünschte Organisation unter &quot;**&quot;**.

![GitHub - Repository-Besitzer auswählen](/help/assets/overview/github-repo-owner-dropdown.png)

Bei Repositorys im Besitz eines Unternehmens muss ein Organisationsadministrator möglicherweise die [!DNL GitHub] Apps genehmigen.

>[!NOTE]
>
>Dies ist eine Berechtigungsprüfung, kein Einrichtungsschritt. Installieren Sie noch keine [!DNL GitHub] Apps - [Erstellen Sie Ihre erste App automatisch](/help/guides/create-app.md) führt Sie durch die Installation der einzelnen Apps, die sich auf die exakten von Ihnen erstellten Repositorys beziehen, an der Stelle, an der sie benötigt werden.

### Website

Sie benötigen eine öffentliche HTTPS-Website, die die Produkte, Services oder Aufgaben darstellt, die die App unterstützen soll. Die Plattform analysiert diese Website, um Aktionen vorzuschlagen und repräsentative Beispieldaten zu erstellen.

Verwenden Sie keine Website, die vertrauliche oder zugriffskontrollierte Informationen zur Verfügung stellt.

### [!DNL ChatGPT] oder [!DNL Claude] zum Testen

Um das Tutorial Erste Schritte abzuschließen, verwenden Sie einen unterstützten [!DNL ChatGPT] mit aktiviertem Entwicklermodus oder einen unterstützten [!DNL Claude] mit aktivierten benutzerdefinierten Connectoren. Workspace- oder Organisationsadministratoren können den Zugriff einschränken. Siehe [Test in ChatGPT](/help/guides/test-in-chatgpt.md#plan-requirements) oder [Test in Claude](/help/guides/test-in-claude.md#plan-requirements).

## Journey auswählen {#choose-your-journey}

### &#x200B;1. Erstellen und Starten Ihrer ersten App

Beginnen Sie mit [Erstellen und starten Sie Ihre erste App](/help/guides/create-app.md). Diese Journey beginnt mit zwei leeren Repositorys und endet mit einer produktionsbereiten App, die als Plug-in in einer unterstützten LLM-Plattform wie [!DNL ChatGPT] getestet wird.

### &#x200B;2. Anpassen der generierten App

Wählen Sie diese Journey aus, wenn die Plattform die App automatisch erstellt hat und Sie das Beispielverhalten ersetzen möchten:

1. [Passen Sie die generierten Handler an](/help/guides/customize-handler.md) um Ihre APIs zu verbinden und die von den einzelnen Aktionen zurückgegebenen Daten zu definieren.
2. [Passen Sie die generierten Widgets an](/help/guides/widgets.md) um diese Daten zu verwenden und Ihre Interaktionen und Ihr Design anzuwenden.

### &#x200B;3. Neue Aktion von Grund auf hinzufügen

Wählen Sie [Neue Aktion von Grund auf hinzufügen](/help/guides/create-action.md), um neue Metadaten zu definieren, den Handler zu schreiben, ein Widget zu verbinden, zu testen und die Aktion bereitzustellen.

### &#x200B;4. Verbinden eines vorhandenen EDS-Projekts

Wählen Sie [Vorhandenes EDS-Projekt verbinden](/help/guides/bring-your-own-eds.md) aus, wenn Sie bereits eine EDS-Website haben oder die App nicht automatisch erstellt haben.

Jede Journey verwendet den freigegebenen [Bereitstellungs](/help/guides/deploy-your-app.md)-Schritt, dann [ChatGPT-Plug-in-](/help/guides/test-in-chatgpt.md) oder [Claude-Connector-](/help/guides/test-in-claude.md).

