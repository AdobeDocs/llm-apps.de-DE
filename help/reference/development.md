---
title: Lokale Handler-Entwicklung und -Tests
description: Handler-Projektstruktur, lokale Serverbefehle, MCP-Tests und Komponententests für Adobe LLM-Apps.
source-git-commit: eec74b87457bc852d7a8dd0e46c2a4385a93ae0a
workflow-type: tm+mt
source-wordcount: '280'
ht-degree: 2%

---


# Lokale Handler-Entwicklung und -Tests {#development}

>[!IMPORTANT]
>
>[!DNL Adobe LLM Apps] befindet sich derzeit in Beta.
>
>Die hier gezeigten Funktionen, Workflows und Benutzeroberflächen stellen nicht unbedingt den endgültigen Status des Produkts dar. Um Beta beizutreten, senden Sie eine E-Mail an llm-apps-beta@adobe.com.

Verwenden Sie diese Referenz, während Sie Handler lokal entwickeln. Den Handler-Ergebnisvertrag finden Sie unter [Anpassen eines generierten Handlers](/help/guides/customize-handler.md).

## Voraussetzungen

- Node.js 24 oder höher.
- npm.
- Ein lokaler Klon des verknüpften Handler-Repositorys

## Projektstruktur

Das verknüpfte Repository folgt diesem Layout:

```
your-llm-app/
├── entry.js                   # Webpack entry — do not modify
├── actions/                   # One folder per action
│   └── echo/
│       └── index.js           # Example handler
├── test/
│   ├── actions/
│   │   └── echo.test.js
│   ├── fixtures/
│   │   └── actions.json
│   ├── html-transform.js
│   ├── jest.setup.js
│   └── server.test.js
├── server/
│   └── local.js               # Local dev server (port 9080)
├── actions.json               # Gitignored — optional local metadata
├── app.config.yaml            # Adobe I/O Runtime config
├── webpack.config.js
└── package.json
```

Wichtigste Punkte:

- **`entry.js`** ist der Einstiegspunkt für das Webpack. Bei der Erstellung wird jede `actions/*/index.js`-Datei erkannt und zu einem einzigen `dist/index.js` gebündelt. Nicht ändern.
- **`actions.json`** wird ignoriert. Die Bereitstellungs-Pipeline schreibt sie automatisch aus den Aktionsmetadaten in [!DNL LLM Apps].
- **Tests** live unter `test/actions/`, **nicht** innerhalb `actions/`. Webpack bündelt alles unter `actions/` in dem bereitgestellten Artefakt - gemeinsame Ortungstests würden sie an [!DNL Adobe I/O Runtime] senden.

## Lokale Entwicklung

Sie können Handler lokal ohne Adobe-Anmeldeinformationen entwickeln und testen:

```bash
npm install
npm run dev:local
```

Dadurch wird das Projekt mit Webpack erstellt und ein einfacher Node.js-HTTP-Server auf `http://localhost:9080` gestartet. Der Server erkennt Ihre Handler-Dateien automatisch unter `actions/` und registriert sie als MCP-Tools.

### Verhalten lokaler Metadaten

Die aktuelle Benutzeroberfläche bietet keinen `actions.json` Download. Sie können den lokalen Server ohne diese Datei ausführen. Er erkennt Handler unter `actions/` und registriert sie mit minimalen Metadaten.

Ohne `actions.json` werden lokale Aktionsargumente nicht anhand des Eingabeschemas der Benutzeroberfläche validiert. Modultests und Integrationstests verwenden `test/fixtures/actions.json` für repräsentative Metadaten.

### Testen mit cURL

```bash
# List all registered tools
curl -sX POST "http://localhost:9080" \
  -H 'content-type: application/json' \
  -H 'accept: application/json;q=1.0, text/event-stream;q=0.5' \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/list","params":{}}'

# Call the boilerplate echo action
curl -sX POST "http://localhost:9080" \
  -H 'content-type: application/json' \
  -H 'accept: application/json;q=1.0, text/event-stream;q=0.5' \
  -d '{"jsonrpc":"2.0","id":2,"method":"tools/call","params":{"name":"echo","arguments":{"message":"hello"}}}'
```

### Testen mit MCP Inspector

```bash
npx @modelcontextprotocol/inspector
```

Legen Sie **Transport Type** auf `streamable-http` und **URL** auf `http://localhost:9080` fest.

## Testen

Handler-Komponententests werden live unter `test/actions/` durchgeführt und spiegeln das `actions/`-Layout:

```javascript
// test/actions/echo.test.js
const handler = require('../../actions/echo/index.js')

test('echoes the message', async () => {
  const result = await handler({ message: 'hello' })
  expect(result.content[0].text).toBe('Echo: hello')
})

test('always returns content parts', async () => {
  const result = await handler({})
  expect(Array.isArray(result.content)).toBe(true)
})
```

Ausführen von Tests mit:

```bash
npm test                                      # all tests
npx jest test/actions/echo                   # one action only
```

Nachdem die lokalen Tests erfolgreich waren, übertragen Sie die Änderungen und folgen Sie [Änderungen bereitstellen](/help/guides/deploy-your-app.md).

