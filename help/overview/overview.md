---
title: Übersicht über Adobe LLM-Apps
description: Erfahren Sie, was Adobe LLM-Apps sind, wie sie funktionieren und was Sie benötigen, um loszulegen.
source-git-commit: 344c5457eb79a19b1dae823732a1cd9866dcd9dc
workflow-type: tm+mt
source-wordcount: '831'
ht-degree: 2%

---


# Adobe LLM Apps - ein Überblick {#adobe-llm-apps-an-overview}

>[!IMPORTANT]
>
>[!DNL Adobe LLM Apps] befindet sich derzeit in Beta.
>
>Die hier gezeigten Funktionen, Workflows und Benutzeroberflächen stellen nicht unbedingt den endgültigen Status des Produkts dar. Um Beta beizutreten, senden Sie eine E-Mail an llm-apps-beta@adobe.com.

## Was ist [!DNL Adobe LLM Apps]?

[!DNL Adobe LLM Apps] ermöglicht es Ihrer Marke, wichtige Aktionen - wie Produkterkennung, Verfügbarkeitsprüfungen oder Service-Buchungen - direkt in KI-Assistenten wie [!DNL ChatGPT] oder Claude anzuzeigen. Statt in KI-generierten Antworten passiv erwähnt zu werden, kann Ihre Marke Kunden durch echte Geschäftsabläufe führen, ohne dass sie das Gespräch verlassen.

[!DNL LLM Apps] ist verfügbar unter [experience.adobe.com/llm-apps](https://experience.adobe.com/llm-apps).

## Was man mit [!DNL LLM Apps] machen kann

- **Erstellen von markeneigenen LLM-Aktionen** - Definieren Sie die spezifischen Geschäftsabläufe, die Sie in KI-Assistenten aktivieren möchten (z. B. *Planen einer Testfahrt*, *Produkte vergleichen*, *Service buchen*).
- **Interaktive LLM-Widgets erstellen** - Erstellen Sie visuelle UI-Komponenten (Produktkarten, Buchungsformulare, Store-Locators), die als AEM-Komponenten in Ihrem [!DNL GitHub]-Repository verwaltet werden.
- **Zentrale Markenverwaltung** - Autoren und Entwickler behalten die volle Kontrolle über alle Inhalte, Kopien und Visualisierungen, die in der LLM-Plattform bereitgestellt werden. Genehmigungen werden über AEM verwaltet.
- **In Staging- und Produktionsumgebung bereitstellen** - Eine gesteuerte Bereitstellungs-Pipeline ermöglicht es Ihnen, das Erlebnis in einer Staging-Umgebung zu testen, bevor Sie zur Produktion weiterleiten.
- **Kontrollieren der Sichtbarkeit auf Aktionsebene** - Nach der Bereitstellung können einzelne Aktionen aktiviert oder deaktiviert werden, ohne dass die gesamte App neu bereitgestellt wird.
- **Messen, was Entscheidungen**: Integrierte Analyse (unterstützt von Adobe Customer Journey Analytics), Anzahl der Oberflächenaktionen, Erfolgsraten, Abbruchraten, Top-Benutzeraufforderungen und Sichtbarkeitsbewertung.

## Warum [!DNL LLM Apps] wichtig sind

LLM-Interaktionen unterscheiden sich grundlegend von der herkömmlichen Suche. Die durchschnittliche [!DNL ChatGPT] dauert viermal länger als eine herkömmliche Suchsitzung. Mehr als 40 % der Verbraucher verlassen sich bei komplexen Kaufentscheidungen auf KI-Tools. Ohne [!DNL LLM Apps] könnten Sie die Erwähnung gewinnen, aber den Kunden verlieren. [!DNL LLM Apps] stellt sicher, dass Ihre Marke nicht nur sichtbar, sondern genau in dem Moment umsetzbar ist, in dem ein Benutzer bereit ist, eine Entscheidung zu treffen.

## Wichtige Konzepte

**LLM App** - Ihr gebrandeter Assistent, mit dem Benutzer in [!DNL ChatGPT] oder anderen LLM-Plattformen interagieren. Es fasst alle Aktionen zusammen und wird als eine Einheit bereitgestellt.

**Action** - eine Funktion, die Ihre App bietet. Zum Beispiel „Distributor suchen“ oder „Produkte durchsuchen“. Jede Aktion wird vom LLM aufgerufen, wenn der Benutzer eine relevante Frage stellt. Jede Aktion besteht aus zwei Teilen: Metadaten (Name, Beschreibung, Parameter), die in der [!DNL LLM Apps]-Benutzeroberfläche verwaltet werden, und einem Handler (Ihr Code) in [!DNL GitHub].

**Action Handler** - Der Code, der ausgeführt wird, wenn eine Aktion aufgerufen wird. Es kann Ihre APIs aufrufen, Live-Daten abrufen oder statische Daten zurückgeben. Handler sind in Ihrem [!DNL GitHub]-Repository unter `actions/<name>/index.js` verfügbar.

**Widget** - die visuelle Antwort, die dem Benutzer angezeigt wird - eine Karte, ein Karussell, eine Tabelle oder eine beliebige benutzerdefinierte Benutzeroberfläche, die zusammen mit der Textantwort des LLM gerendert wird. Widgets sind HTML-Seiten, die auf einer [!DNL Edge Delivery Services] (EDS)-Website gehostet werden.

## Funktionsweise

Das folgende Diagramm zeigt, wie die einzelnen Elemente zusammenpassen - von der Definition einer App in der Benutzeroberfläche bis zur Live-Anzeige von Ergebnissen in der LLM-Plattform.

```
┌─────────────────────────────────────────────────────────────┐
│                      LLM Apps UI                            │
│  ┌──────────┐   ┌──────────┐   ┌───────────────────────┐    │
│  │   App    │──▶│ Actions  │──▶│ Metadata + Widget cfg │    │
│  └──────────┘   └──────────┘   └───────────┬───────────┘    │
└─────────────────────────────────────────── │ ────────────-──┘
                                             │ deploy
                                             ▼
┌─────────────────────────────────────────────────────────────┐
│                  Adobe I/O Runtime                          │
│               MCP Server (auto-generated)                   │
│  ┌───────────────┐ ┌──────────────────┐ ┌───────────────┐   │
│  │ search-       │ │ get-product-     │ │ find-where-   │   │
│  │ products      │ │ details          │ │ to-buy        │   │
│  └───────────────┘ └──────────────────┘ └───────────────┘   │
└──────────────────────────────┬──────────────────────────────┘
                               │ MCP protocol
                               ▼
┌─────────────────────────────────────────────────────────────┐
│                        ChatGPT                              │
│  Conversation                                               │
│  ┌───────────────────────────────────────────────────────┐  │
│  │  EDS Widget                                           │  │
│  │  Product carousel, store locator, detail card ...     │  │
│  └───────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────┘
```

## Voraussetzungen

### Adobe-Entwicklerkonsole

Sie benötigen Zugriff auf [Adobe Developer Console](https://developer.adobe.com/console) mit der Rolle **Entwickler** (oder **Systemadministrator** in Ihrer Adobe IMS-Organisation. Stellen Sie sicher, dass Ihr Unternehmen Zugriff auf [[!DNL App Builder]](https://developer.adobe.com/app-builder/docs/intro_and_overview/) hat.

Zur Bestätigung navigieren Sie zu [developer.adobe.com/console](https://developer.adobe.com/console). Wenn der Schnellstartbildschirm angezeigt wird, sind Ihre Berechtigungen korrekt eingerichtet.

![Adobe Developer Console - Schnellstartbildschirm, der den Entwicklerzugriff bestätigt](/help/assets/overview/dev-console-access-granted.png)

Wenn stattdessen die Meldung **Eingeschränkter Zugriff** angezeigt wird, verfügen Sie nicht über die Rolle Entwickler . Wenden Sie sich an den Administrator Ihrer IMS-Organisation, um Zugriff zu erhalten.

![Adobe Developer Console - Nachricht zu eingeschränktem Zugriff](/help/assets/overview/dev-console-access-denied.png)

### [!DNL GitHub]

Sie benötigen in Ihrer Organisation ein [!DNL GitHub]-Konto mit den folgenden Berechtigungen:

- **Erstellen von Repositorys** - Sie müssen zwei Repositorys in Ihrer Organisation erstellen: eines für den Anwendungs-Code und eines für das EDS-Projekt. Zur Bestätigung navigieren Sie zu [github.com/new](https://github.com/new) - wenn Sie Ihre Organisation im Dropdown-Menü **Inhaber** auswählen können, verfügen Sie über die Berechtigung.

  ![Dropdown-Liste „Besitzer des neuen GitHub-Repositorys“ mit Auswahl der Organisation](/help/assets/overview/github-repo-owner-dropdown.png)

- **Installieren von [!DNL GitHub] Apps** - Sie benötigen die entsprechenden Berechtigungen, um eine [!DNL GitHub] App in Ihrem Unternehmen zu installieren. Siehe [Voraussetzungen für die Installation einer GitHub-App](https://docs.github.com/en/apps/using-github-apps/installing-a-github-app-from-a-third-party#requirements-to-install-a-github-app).

### AEM Sites mit [!DNL Edge Delivery Services]

Aktions-Widgets werden auf **Adobe Experience Manager [!DNL Edge Delivery Services] (EDS) gehostet**. Ihr Unternehmen benötigt eine AEM Sites-Lizenz mit [!DNL Edge Delivery Services]. Sie müssen in Ihrer EDS **Organisation über die** Admin“ verfügen.

Um dies zu überprüfen, gehen Sie zum [EDS User Admin Tool](https://tools.aem.live/tools/user-admin/index.html), geben Sie Ihren Organisationsnamen ein, lassen Sie **Site** leer und klicken Sie auf **Benutzer abrufen**. Suchen Sie Ihr Konto in der Liste und bestätigen Sie, dass es das **admin**-Badge aufweist.

![EDS-Benutzeradministrator-Tool, das einen Benutzer mit der Administratorrolle anzeigt](/help/assets/overview/eds-user-admin.png)

### LLM-Plattform (zum Testen)

Zum Testen der bereitgestellten App benötigen Sie eine unterstützte Abonnementebene, die benutzerdefinierte MCP-Apps und die Aktivierung **Entwicklermodus** ermöglicht. [!DNL ChatGPT] erfordert beispielsweise ein Abonnement **Pro**, **Business** oder **Enterprise/Edu**.

## Erste Schritte

Mit Blick auf einen Anwendungsfall sollten Sie [eine App erstellen](/help/guides/create-app.md) um mit der Erstellung und Bereitstellung Ihres [!DNL LLM Apps] Erlebnisses zu beginnen.

