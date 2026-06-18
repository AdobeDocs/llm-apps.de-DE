---
title: Entwicklung für Adobe LLM-Apps
description: Projektstruktur, lokaler Entwicklungs-Workflow und Testeinrichtung für den Adobe LLM Apps Handler-Code.
source-git-commit: 51ffb31eec82f9639bd7ade9052d61028c262d0e
workflow-type: tm+mt
source-wordcount: '318'
ht-degree: 4%

---


# Entwicklung {#development}

>[!IMPORTANT]
>
>**Haftungsausschluss:** Dies ist eine Beta-Version von [!DNL LLM Apps]. Die hier gezeigten Funktionen, Workflows und Benutzeroberflächen stellen nicht unbedingt den endgültigen Status der Anwendung oder des Produkts dar.

In diesem Abschnitt werden die Handler-Projektstruktur, der lokale Entwicklungs-Workflow und das Testsetup behandelt. Den Handler-Vertrag und den Beispiel-Code finden Sie unter [Action Handler schreiben](/help/guides/write-action-handler.md).

## Projektstruktur

Das verknüpfte Repository folgt diesem Layout:

```
your-llm-app/
├── entry.js                   # Webpack entry — do not modify
├── actions/                   # One folder per action
│   ├── search-products/
│   │   └── index.js           # Handler (async function)
│   ├── get-product-details/
│   │   └── index.js
│   └── echo/
│       └── index.js
├── test/
│   ├── actions/
│   │   └── search-products.test.js
│   ├── fixtures/
│   │   └── actions.json
│   ├── html-transform.js
│   ├── jest.setup.js
│   └── server.test.js
├── server/
│   └── local.js               # Local dev server (port 9080)
├── actions.json               # Gitignored — local copy of UI metadata
├── app.config.yaml            # Adobe I/O Runtime config
├── webpack.config.js
└── package.json
```

Wichtigste Punkte:

- **`entry.js`** ist der Einstiegspunkt für das Webpack. Bei der Erstellung wird jede `actions/*/index.js`-Datei erkannt und zu einem einzigen `dist/index.js` gebündelt. Nicht ändern.
- **`actions.json`** wird ignoriert. Herunterladen von der Seite Aktionen in der Benutzeroberfläche für die lokale Entwicklung. Bei Bereitstellungen schreibt die Pipeline sie automatisch aus der API.
- **Tests** live unter `test/actions/`, **nicht** innerhalb `actions/`. Webpack bündelt alles unter `actions/` in dem bereitgestellten Artefakt - gemeinsame Ortungstests würden sie an [!DNL Adobe I/O Runtime] senden.

## Lokale Entwicklung

Sie können Handler lokal ohne Adobe-Anmeldeinformationen entwickeln und testen:

```bash
npm install
npm run dev:local
```

Dadurch wird das Projekt mit Webpack erstellt und ein einfacher Node.js-HTTP-Server auf `http://localhost:9080` gestartet. Der Server erkennt Ihre Handler-Dateien automatisch unter `actions/` und registriert sie als MCP-Tools.

### `actions.json` herunterladen

Damit der lokale Server von Ihren Aktionsmetadaten (Name, Beschreibung, Eingabeschema) weiß, laden Sie `actions.json` von der Seite Aktionen in der [!DNL LLM Apps]-Benutzeroberfläche herunter und platzieren Sie es im Repository-Stamm. Ohne sie erkennt der Server Ihre Handler, registriert sie jedoch mit minimalen Metadaten.

Sie können `actions.example.json` auch als Ausgangspunkt nach `actions.json` kopieren.

### Testen mit cURL

```bash
# List all registered tools
curl -sX POST "http://localhost:9080" \
  -H 'content-type: application/json' \
  -H 'accept: application/json;q=1.0, text/event-stream;q=0.5' \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/list","params":{}}'

# Call the search-products action
curl -sX POST "http://localhost:9080" \
  -H 'content-type: application/json' \
  -H 'accept: application/json;q=1.0, text/event-stream;q=0.5' \
  -d '{"jsonrpc":"2.0","id":2,"method":"tools/call","params":{"name":"search-products","arguments":{"category":"bagged-coffee"}}}'
```

### Testen mit MCP Inspector

```bash
npx @modelcontextprotocol/inspector
```

Legen Sie **Transport Type** auf `streamable-http` und **URL** auf `http://localhost:9080` fest.

## Testen

Handler-Komponententests werden live unter `test/actions/` durchgeführt und spiegeln das `actions/`-Layout:

```javascript
// test/actions/search-products.test.js
const handler = require('../../actions/search-products/index.js')

test('returns all products when no filter is given', async () => {
  const result = await handler({})
  expect(result.content[0].text).toContain('product')
  expect(result.structuredContent.products.length).toBeGreaterThan(0)
})

test('filters by category', async () => {
  const result = await handler({ category: 'bagged-coffee' })
  expect(result.structuredContent.products.every(
    (p) => p.category === 'bagged-coffee'
  )).toBe(true)
})

test('filters by query', async () => {
  const result = await handler({ query: 'dark-roast' })
  expect(result.structuredContent.products.length).toBeGreaterThan(0)
})

test('returns empty result for unknown category', async () => {
  const result = await handler({ category: 'nonexistent' })
  expect(result.structuredContent.products).toHaveLength(0)
})
```

Ausführen von Tests mit:

```bash
npm test                                      # all tests
npx jest test/actions/search-products        # one action only
```

## Bereitstellung

Sie erstellen oder implementieren sie nicht manuell. Eine vollständige Anleitung zur Bereitstellungs-Pipeline finden Sie unter [Bereitstellen Ihrer App](/help/guides/deploy-your-app.md).

Ihr täglicher Arbeitsablauf ist:

| Schritt | Aktion |
|------|--------|
| &#x200B;1. Schreib- oder Bearbeitungshandler | `actions/<name>/index.js` |
| &#x200B;2. Metadaten herunterladen | Seite „Aktionen→ **Aktionen.json herunterladen** |
| &#x200B;3. Lokaler Test | `npm run dev:local` |
| &#x200B;4. Push-Code | `git push` |
| &#x200B;5. Bereitstellen | App-Detailseite → **[!UICONTROL Bereitstellen]** |

