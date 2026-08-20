---
title: Verkabeln einer App
description: Ein genauerer Blick darauf, wie die Teile, die Sie besitzen - Aktionsmetadaten, Handler-Code und Widgets - zur Build- und Laufzeit in einer laufenden LLM-App zusammengeführt werden.
source-git-commit: e066f66b37914e2f747176e865e26dcc074bff20
workflow-type: tm+mt
source-wordcount: '335'
ht-degree: 0%

---


# Verkabeln einer App {#app-architecture}

>[!IMPORTANT]
>
>[!DNL Adobe LLM Apps] befindet sich derzeit in Beta.
>
>Die hier gezeigten Funktionen, Workflows und Benutzeroberflächen stellen nicht unbedingt den endgültigen Status des Produkts dar. Um Beta beizutreten, senden Sie eine E-Mail an llm-apps-beta@adobe.com.

## In einem Satz

Eine **LLM-** ist eine Reihe von **Aktionen** (jeweils ein Tool, das über dem **verfügbar gemacht wird)
Kontextprotokoll **&#x200B; oder &#x200B;** MCP**), das Sie an einem einzelnen Endpunkt veröffentlichen. Ein Chat-Host
Wie [!DNL ChatGPT] diese Tools entdeckt, sie während der Unterhaltung aufruft und rendert
Ein **interaktives** mit dem Ergebnis - direkt im Chat.

## Die gesamte Verkabelung, Build → Run

**Diagramm 1 — Erstellungszeit.** Sie besitzen drei separate Oberflächen; die Plattform sichert
sie in einer bereitstellbaren App zusammenfassen.

```
┌─────────────────────┐   ┌─────────────────────┐   ┌─────────────────────┐
│ 1  LLM Apps UI      │   │ 2  Action Handler   │   │ 3  Widget repo      │
│                     │   │    repo             │   │                     │
│ Create, edit, and   │   │                     │   │ Each widget is an   │
│ manage your action  │   │ Business logic —    │   │ EDS block,          │
│ definitions here    │   │ built from our      │   │ published to a      │
│ (metadata)          │   │ boilerplate         │   │ public URL on       │
│                     │   │                     │   │ *.aem.page          │
│                     │   │ Returns content     │   │                     │
│                     │   │ (for the LLM) +     │   │                     │
│                     │   │ structuredContent   │   │                     │
│                     │   │ (for the widget)    │   │                     │
└─────────────────────┘   └─────────────────────┘   └─────────────────────┘
           │                         │                         │
           └─────────────────────────┼─────────────────────────┘
                                     ▼
                       ┌─────────────────────────────┐
                       │ LLM Apps deploy pipeline    │
                       │ Combines the 3 surfaces     │
                       │ into one running app        │
                       └─────────────────────────────┘
                                     │
                                     ▼
                 ┌─────────────────────────────────────────┐
                 │ ONE MCP server on Adobe I/O Runtime     │
                 │ https://<ns>.adobeioruntime.net/.../mcp │
                 └─────────────────────────────────────────┘
```

- **LLM Apps UI** - Hier können Sie die Definition jeder Aktion erstellen, bearbeiten und verwalten:
Sein **Code-Bezeichner** (ein fester Slug, den Sie hier einmal gesetzt haben, z.B. `my_action`, dass
verknüpft dieselbe Aktion über die Benutzeroberfläche, den Handler und das Widget hinweg),
Beschreibung, Eingabeschema, Widget-Auswahl und CSP-/Sichtbarkeitsflags. Kein Code.
- **Action Handler repo** - das Server-seitige Repository (basierend auf unserem Textbaustein)
wo Sie die Geschäftslogik schreiben. Jede Handler-Funktion gibt zwei Dinge zurück:
  `content` (Nur Text, den *LLM* liest) und `structuredContent` (das Datenobjekt
  Das *Widget* liest).
- **Widget repo** — das EDS-Repository, in dem jedes Widget als Block lebt und
in einer öffentlichen `*.aem.page`-URL veröffentlicht. Jeder Baustein verwendet
  [`@adobe/llmapps-sdk`](https://www.npmjs.com/package/@adobe/llmapps-sdk), der
Brücke zwischen dem Widget und dem Host/Server. Es implementiert die **MCP-Apps
Spezifikation** — das zugrunde liegende Protokoll — hinter einer einfachen API, und zwar
abstrahiert den LLM-Host selbst, sodass dasselbe Widget unverändert in funktioniert
  [!DNL ChatGPT], [!DNL Claude], Gemini oder ein anderer MCP-Host.

**Diagramm 2 — runtime.** Was passiert bei jeder Nachricht, die der Benutzer einmal sendet?
Dieser eine Server ist live. Wird mit [!DNL ChatGPT] als Beispiel-Host angezeigt — dem
Die gleiche Sequenz wird für jeden MCP-Host ausgegeben, z. B. [!DNL Claude].

```
┌── ChatGPT  (the MCP host) ──────────────────────────────────────────────┐
│  1  tools/list  >  sees `my_action` + its description + input schema    │
│  2  user asks   >  "I need help with …"                                 │
│  3  model picks >  the description matches -> calls this tool           │
│  4  tools/call  >  { name: "my_action", arguments: {situation, ...} }   │
└─────────────────────────────────────────────────────────────────────────┘
                                     │
                 routes by CODE IDENTIFIER  ->  my_action
                                     ▼
┌── Adobe I/O Runtime ────────────────────────────────────────────────────┐
│  actions/my_action/index.js  --  your handler runs                      │
│  returns  { content -> text for the model , structuredContent -> data } │
└─────────────────────────────────────────────────────────────────────────┘
                                     │
                                     ▼
┌── Rendered inside the conversation ─────────────────────────────────────┐
│  5  render      >  ChatGPT renders the Widget repo's EDS block          │
│  The EDS block reads the structuredContent the Action Handler repo      │
│  returned, and draws the interactive card — live, inside the chat.      │
└─────────────────────────────────────────────────────────────────────────┘
```
