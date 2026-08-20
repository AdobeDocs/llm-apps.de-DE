---
title: Verkabeln einer App
description: Ein genauerer Blick darauf, wie die Teile, die Sie besitzen - Aktionsmetadaten, Handler-Code und Widgets - zur Build- und Laufzeit in einer laufenden LLM-App zusammengeführt werden.
source-git-commit: 2f3480b3667a6ab7c4ed65b999eed4638c383edb
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

Eine **LLM-** ist eine Reihe von **Aktionen** (jeweils ein Tool, das über das **Model Context Protocol** oder **MCP** verfügbar gemacht wird), die Sie an einem einzelnen Endpunkt veröffentlichen. Ein Chathost wie [!DNL ChatGPT] entdeckt diese Tools, nennt sie Mid-Conversation und rendert ein **interaktives Widget** mit dem Ergebnis - direkt im Chat.

## Die gesamte Verkabelung, Build → Run

**Diagramm 1 — Erstellungszeit.** Sie besitzen drei separate Oberflächen; die Plattform verbindet sie zu einer bereitstellbaren App.

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

- **LLM Apps UI** - Hier können Sie die Definition jeder Aktion erstellen, bearbeiten und verwalten: ihre **Code-Kennung** (ein fester Slug, den Sie hier einmal setzen, z. B. `my_action`, der dieselbe Aktion über die Benutzeroberfläche, den Handler und das Widget hinweg verbindet), Beschreibung, Eingabeschema, Widget-Auswahl und CSP-/Sichtbarkeitsflags. Kein Code.
- **Action Handler repo** - das Server-seitige Repository (auf der Grundlage unseres Textbausteins), in das Sie die Geschäftslogik schreiben. Jede Handler-Funktion gibt zwei Dinge zurück: `content` (Nur-Text, den *LLM* liest) und `structuredContent` (das Datenobjekt, das das *Widget* liest).
- **Widget repo** - das EDS-Repository, in dem jedes Widget als Block lebt und in einer öffentlichen `*.aem.page`-URL veröffentlicht wird. Jeder Baustein verwendet [`@adobe/llmapps-sdk`](https://www.npmjs.com/package/@adobe/llmapps-sdk), die Brücke zwischen dem Widget und dem Host/Server. Es implementiert die **MCP Apps Specification** - das zugrunde liegende Protokoll - hinter einer einfachen API und abstrahiert den LLM-Host selbst, sodass dasselbe Widget unverändert in [!DNL ChatGPT], [!DNL Claude], Gemini oder einem anderen MCP-Host funktioniert.

**Diagramm 2 — runtime.** Was passiert bei jeder Nachricht, die der Benutzer sendet, sobald dieser Server live ist? Wird mit [!DNL ChatGPT] als Beispiel-Host angezeigt. Für jeden MCP-Host, z. B. [!DNL Claude], gibt es dieselbe Sequenz.

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
