---
title: Einrichten des Widgets (EDS)
description: Erfahren Sie, wie Sie ein Edge Delivery Services-Widget-Projekt einrichten und den Blockvertrag für das Rendern visueller Antworten innerhalb von LLM-Plattformen implementieren.
source-git-commit: 483a71f5f1de5caf1bd89b26f4d67d2d5a0aa15a
workflow-type: tm+mt
source-wordcount: '1214'
ht-degree: 1%

---


# Einrichten des Widgets (EDS)

>[!IMPORTANT]
>
>[!DNL Adobe LLM Apps] befindet sich derzeit in Beta. Die hier gezeigten Funktionen, Workflows und Benutzeroberflächen stellen nicht unbedingt den endgültigen Status des Produkts dar.

In diesem Handbuch wird vollständig erklärt, wie Sie ein EDS-Widget erstellen: von der Konfiguration Ihrer Aktion in der [!DNL LLM Apps]-Benutzeroberfläche über die Einrichtung Ihres EDS-Projekts bis hin zum Schreiben des Block-Codes, der Ihre Daten in der LLM-Plattform rendert. Einen umfassenden Überblick finden Sie unter [Grundlegende Konzepte](/help/overview/overview.md#widgets-eds).

## Die [!DNL LLM Apps] SDK

Alles beginnt mit dem [`@adobe/llmapps-sdk`](https://www.npmjs.com/package/@adobe/llmapps-sdk) npm-Paket. SDK ist die JavaScript-Bibliothek, die den bidirektionalen Kommunikationskanal zwischen dem Widget und dem LLM-Host steuert.

Der SDK wird auch `aem-embed.js` ausgeliefert - der EDS-spezifische Einstiegspunkt, der den SDK an die standardmäßige EDS-Block-Pipeline anschließt. Beim `npm install @adobe/llmapps-sdk` kopiert ein Post-Install-Skript automatisch zwei Dateien in Ihr Projekt:

```
scripts/
└── llm-apps/
    ├── aem-embed.js     ← EDS widget entry point, ships with the SDK
    └── llmapps-sdk.js   ← core SDK, loaded internally by aem-embed.js
```

In EDS-Projekten **Sie SDK nie direkt in Ihrem Blockcode verwenden.** `aem-embed.js` erstellt und verwaltet die SDK-Verbindung und übergibt eine vollständig verbundene `LLMApp`-Instanz als `bridge` in `decorate(block, bridge)` an Ihren Block. Die vollständige SDK-API ist auf `bridge` verfügbar - kein Import erforderlich.

Wenn Sie ein Widget (**EDS) erstellen** ein Standard-Bundler- oder TypeScript-Projekt), können Sie die SDK direkt verwenden:

```javascript
import { LLMApp } from '@adobe/llmapps-sdk';

const app = new LLMApp({ appInfo: { name: 'MyWidget', version: '1.0.0' } });
await app.connect();

const { structuredContent } = await app.toolResult;
```

## Wie alles zusammenpasst

Wenn die KI Ihre Aktion aufruft und der Handler `structuredContent` zurückgibt, rendert die LLM-Plattform ein interaktives Widget im Gespräch. Drei Dinge sorgen dafür, dass dies zusammenfunktioniert:

**Die [!DNL LLM Apps]-Benutzeroberfläche** - Wenn Sie eine Aktion erstellen, geben Sie eine **[!UICONTROL Skript-]** und eine **[!UICONTROL Widget-URL]** in der Registerkarte Widget-Metadaten ein. Die Skript-URL verweist auf `aem-embed.js` - die Datei, die im Lieferumfang von SDK enthalten ist und sich in Ihrem EDS-Repository unter `scripts/llm-apps/aem-embed.js` befindet. Dadurch wird der LLM-Plattform mitgeteilt, welches Skript beim Aufrufen der Aktion geladen werden soll.

**`aem-embed.js`** - Die LLM-Plattform lädt dieses Skript in eine Sandbox-Widget-Oberfläche. `aem-embed.js` ist ein benutzerdefiniertes HTML-Element (`<aem-embed>`), das als EDS-orientierter Einstiegspunkt für Ihr Widget dient. Er führt den Handshake mit dem LLM-Host mithilfe der SDK durch, unterdrückt die normale EDS-Seiten-Pipeline (keine Kopf-/Fußzeile), ruft den EDS-Seiteninhalt von der Widget-URL ab, führt die EDS-Block-Pipeline aus und stellt der `decorate()` jedes Blocks ein Live `bridge`-Objekt bereit.

**Ihr Block-Code** - Sie schreiben einen standardmäßigen EDS-Block, der eine `decorate(block, bridge)` exportiert. Der `bridge` ist die verbundene SDK-Instanz. Sie erhalten das strukturierte Ergebnis der Aktion und können Nachrichten zurück an die Konversation senden.

## Hinzufügen zu einem vorhandenen EDS-Projekt

Wenn Sie bereits über ein EDS-Projekt verfügen, gibt es nur zwei Schritte, bevor Sie mit dem Schreiben von Blöcken beginnen können.

1. Installieren Sie `@adobe/llmapps-sdk`. Das Post-Install-Skript kopiert `aem-embed.js` und `llmapps-sdk.js` in `scripts/llm-apps/`:

   ```bash
   npm install @adobe/llmapps-sdk
   ```

2. Konfigurieren Sie CORS-Header, damit die LLM-Plattform Ihre Widget-Seiten und -Skripte ursprungsübergreifend laden kann - siehe [Konfigurieren von CORS-Headern](#configure-cors-headers) unten.

Erstellen Sie dann den Block entsprechend dem [`decorate(block, bridge)` Vertrag](#the-decorateblock-bridge-contract) erstellen Sie die Widget-Seite und geben Sie die URLs im Dialogfeld Aktion erstellen ein.

## Einrichten eines neuen EDS-Projekts

### Repository erstellen

1. Erstellen Sie ein neues [!DNL GitHub]-Repository basierend auf der Vorlage [AEM Boilerplate](https://github.com/adobe/aem-boilerplate).
2. Fügen Sie die [AEM Code Sync GitHub App](https://github.com/apps/aem-code-sync) zum Repository hinzu.
3. Installieren Sie die AEM-CLI für die lokale Entwicklung: `npm install -g @adobe/aem-cli`.
4. Installieren Sie `@adobe/llmapps-sdk`. Das Post-Install-Skript kopiert `aem-embed.js` und `llmapps-sdk.js` in `scripts/llm-apps/`:

   ```bash
   npm install @adobe/llmapps-sdk
   ```

Eine vollständige Anleitung zu EDS-Projekten finden Sie im [AEM-Entwickler-Tutorial](https://www.aem.live/developer/tutorial) und [Projektanatomie](https://www.aem.live/developer/anatomy-of-a-project).

Nach der Einrichtung ist Ihre EDS-Website verfügbar unter:

- **Vorschau:** `https://main--<repo>--<owner>.aem.page/`
- **Live:** `https://main--<repo>--<owner>.aem.live/`

### Repository-Struktur

```
my-brand-eds/
├── scripts/
│   ├── llm-apps/
│   │   ├── aem-embed.js           # Widget entry point — copied by post-install
│   │   └── llmapps-sdk.js         # Core SDK — copied by post-install
│   ├── aem.js                     # AEM core library
│   └── scripts.js                 # Site-level decoration and loading
├── blocks/
│   └── search-products/           # One folder per widget block
│       ├── search-products.js
│       └── search-products.css
├── styles/
│   └── styles.css
├── head.html
└── package.json
```

### CORS-Header konfigurieren

Ihre EDS-Widget-Seiten werden von der LLM-Plattform in eine Sandbox-Widget-Oberfläche geladen. Die EDS-Website muss korrekte `access-control-allow-origin`-Kopfzeilen zurückgeben, damit der Host Ihren Widget-Inhalt herkunftsübergreifend abrufen kann.

Kopfzeilen werden über das AEM-Admin-Bedienfeld unter `admin.hlx.page` mithilfe des [Konfigurations-Service](https://aem.live/docs/config-service-setup) konfiguriert. Fügen Sie benutzerdefinierte Antwort-Header für die Pfade hinzu, in denen Ihre Widget-Seiten und SDK-Skripte vorhanden sind:

```json
{
  "/<your-widget-pages-path>/**": [
    { "key": "access-control-allow-origin", "value": "*" }
  ],
  "/scripts/**": [
    { "key": "access-control-allow-origin", "value": "*" }
  ]
}
```

>[!NOTE]
>
>Die Verwendung von `*` als Ursprungswert ist für öffentliche Widget-Inhalte auf der `.aem.live` Domain akzeptabel. Wenn Ihre Site geschützte Inhalte enthält, beschränken Sie die Herkunft auf bestimmte Domains.

### Erstellen der Widget-Seite

Erstellen Sie eine Seite in Ihrem EDS-Authoring-Tool und fügen Sie Ihren -Block hinzu. Die Seiten-URL wird zur **[!UICONTROL Widget-URL]** die Sie in der Aktion konfigurieren - das ist die einzige Verbindung zwischen der Aktion und dem Block. Es gibt keine Namensanforderung zwischen dem Block und dem Aktionsnamen.

![EDS Authoring - Block zu Widget-Seite hinzugefügt](/help/assets/guide-widget/aem-author.png)

### Geben Sie die URLs im Dialogfeld Aktion erstellen ein

Navigieren Sie nach der Einrichtung des EDS-Repositorys zu **Widget-Metadaten → Vorlagen-URLs** wenn Sie Ihre Aktion erstellen:

**[!UICONTROL Skript-URL]** - verweist auf `aem-embed.js` in Ihrem EDS-Repository. Dies ist für jede Aktion im selben EDS-Projekt der gleiche Wert:

```
https://main--<repo>--<owner>.aem.live/scripts/llm-apps/aem-embed.js
```

**[!UICONTROL Widget URL]** - die URL der EDS-Seite, die Sie für dieses Widget erstellt haben. Eindeutig pro Aktion:

```
https://main--<repo>--<owner>.aem.live/<path-to-your-widget-page>
```

Die LLM-Plattform lädt `aem-embed.js` von der Skript-URL. `aem-embed.js` ruft dann die `.plain.html` aus der Widget-URL ab, um den Blockinhalt abzurufen.

## Datenfluss

Der vollständige Pfad von Ihrem Handler zu einem gerenderten Widget:

1. **Action Handler** gibt `structuredContent` zurück:

```javascript
// actions/search-products/index.js
return {
  structuredContent: {
    products: [
      { id: 'COF-001', name: 'Single Origin Ethiopian Coffee', price: '$18', rating: 4.7 },
      { id: 'COF-002', name: 'Colombia Huila Natural', price: '$22', rating: 4.5 },
    ],
    total: 2,
    category: 'coffee'
  }
};
```

1. **LLM-Plattform** Öffnet eine Widget-Oberfläche und lädt `aem-embed.js` von der Skript-URL.

1. **`aem-embed.js`** stellt eine Verbindung zum Host über die SDK her, ruft `.plain.html` von der Widget-URL ab, führt die EDS-Block-Pipeline aus und ruft `decorate(block, bridge)` auf Ihrem Block auf.

1. **Ihr Block** liest die Daten aus `bridge.toolResult` und rendert die Benutzeroberfläche.

1. **Benutzerinteraktion** Trigger `bridge.sendMessage(...)` oder `bridge.callTool(...)` und senden eine Folgenachricht an die Konversation.

## Der `decorate(block, bridge)`

Jeder EDS-Widget-Block sollte eine standardmäßige `decorate` exportieren. Dies ist die standardmäßige EDS-Blocksignatur, erweitert um ein zweites Argument - das verbundene `bridge`, bei dem es sich um eine [`LLMApp`](https://www.npmjs.com/package/@adobe/llmapps-sdk) SDK-Instanz mit der vollständigen API handelt:

```javascript
export default async function decorate(block, bridge) {
  // ...
}
```

`bridge` ist nur vorhanden, wenn es innerhalb der LLM-Plattform-Widget-Oberfläche ausgeführt wird. Schützen Sie Ihre Bridge-Aufrufe immer, damit Ihr Block auch gerendert wird, wenn Sie ihn direkt in einem Browser oder auf Ihrem lokalen Entwicklungs-Server in der Vorschau anzeigen.

### Rendern von Daten aus dem Aktionsergebnis

`bridge.toolResult` ist ein Promise, das mit dem vollständigen Ergebnis aufgelöst wird, das Ihr Handler zurückgegeben hat, einschließlich `structuredContent`.

```javascript
const SAMPLE_PRODUCTS = [
  { id: 'COF-001', name: 'Single Origin Ethiopian Coffee', price: '$18', rating: 4.7 },
];

export default async function decorate(block, bridge) {
  let products = SAMPLE_PRODUCTS;

  if (bridge) {
    const result = await bridge.toolResult;
    products = result?.structuredContent?.products ?? [];
  }

  block.innerHTML = products.map(p => `
    <div class="product-card">
      <h3>${p.name}</h3>
      <p class="price">${p.price}</p>
      <button data-id="${p.id}">Tell me more</button>
    </div>
  `).join('');
}
```

### Anwenden des Host-Designs

Rufen Sie `bridge.applyHostStyles()` früh in `decorate` auf, um die CSS-Variablen und -Schriftarten (helles/dunkles Design, Typografie) des Hosts in das Widget einzufügen. Dadurch bleibt Ihr Widget visuell konsistent mit der umgebenden LLM-Plattform-Benutzeroberfläche.

```javascript
export default async function decorate(block, bridge) {
  if (bridge) {
    bridge.applyHostStyles();
  }
  // ...
}
```

So reagieren Sie auf Designänderungen zur Laufzeit (z. B. wenn Benutzende zwischen dem hellen und dem dunklen Modus wechseln):

```javascript
if (bridge) {
  bridge.onContextChange(ctx => {
    block.dataset.theme = ctx.theme; // 'light' | 'dark'
  });
}
```

### Folgenachricht senden

`bridge.sendMessage(text)` fügt eine Benutzermeldung in die Konversation ein. Dies ist die primäre Möglichkeit, wie ein Widget die KI-Interaktion weiter antreibt - z. B. wenn ein Trigger auf eine Produktkarte klickt, um nach Details zu fragen.

```javascript
block.querySelectorAll('button[data-id]').forEach(btn => {
  btn.addEventListener('click', () => {
    bridge.sendMessage(`Show me details for product ${btn.dataset.id}`);
  });
});
```

### Direktes Aufrufen einer anderen Aktion

`bridge.callTool(name, args)` ruft eine weitere Aktion innerhalb des Widgets auf, ohne eine Benutzermeldung zu durchlaufen. Nützlich zum Laden verwandter Daten bei Bedarf.

```javascript
btn.addEventListener('click', async () => {
  const result = await bridge.callTool('get-product-details', { id: product.id });
  renderDetails(result.structuredContent);
});
```

### Widget-Größe automatisch ändern

Die LLM-Plattform skaliert das Widget basierend auf dem, was Sie melden. Verwenden Sie `bridge.autoResize(element)`, um die Widget-Höhe synchron zu halten, wenn sich Ihr Inhalt ändert - es verwendet intern eine `ResizeObserver`. Nach dem ersten Rendern aufrufen:

```javascript
export default async function decorate(block, bridge) {
  // ... render content ...

  if (bridge) {
    bridge.autoResize(block);
  }
}
```

Oder melden Sie eine feste Größe manuell:

```javascript
bridge.reportSize(block.offsetWidth, block.offsetHeight);
```

### Vorschaumodus und lokale Entwicklung

Bei der Vorschau einer EDS-Seite direkt im Browser oder auf dem lokalen Dev-Server wird `bridge` `undefined`. Verwenden Sie das oben dargestellte Fallback-Muster für Beispieldaten, damit Ihr Block sofort ohne einen Live-Handler gerendert wird.

So starten Sie einen lokalen Entwicklungsserver:

```bash
npm install -g @adobe/aem-cli
aem up
```

Dadurch wird `http://localhost:3000` geöffnet, wo Sie zu Ihren Widget-Seiten navigieren und Blöcke sehen können, die mit Beispieldaten gerendert werden. Änderungen an Block-JS und CSS werden sofort übernommen.

## Nächste Schritte

- [Anleitung: Schreiben des Aktions-Handlers](/help/guides/write-action-handler.md)

