---
title: Lokale Handler-Entwicklung und -Tests
description: Handler-Projektstruktur, lokale Serverbefehle, MCP-Tests und Komponententests für Adobe LLM-Apps.
source-git-commit: 0f9dd0a5b1008e183eb8ab5a4c2eae34a78923bb
workflow-type: tm+mt
source-wordcount: '664'
ht-degree: 0%
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

Fordern Sie bei einer von Adobe verwalteten Infrastruktur den Repository-Zugriff an, bevor Sie dieser Referenz folgen. Für die lokale Handler-Ausführung sind keine Adobe-Anmeldeinformationen erforderlich, für geschützte Upstream-Services ist jedoch weiterhin ein eigener autorisierter Zugriff erforderlich.

## Projektstruktur

Der aktuelle Handler-Textbaustein folgt diesem Layout:

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

- **`entry.js`** ist der Einstiegspunkt für das Webpack. Zur Build-Zeit erkennt es jede `actions/*/index.js`-Datei und bündelt sie in einem einzigen `dist/index.js`. Ändern Sie den Einstiegspunkt nicht.
- **`actions.json`** wird ignoriert. Die Bereitstellungs-Pipeline schreibt sie automatisch aus den Aktionsmetadaten in [!DNL LLM Apps].
- **Tests** live unter `test/actions/`, **nicht** innerhalb `actions/`. Der Build erkennt Handler-Einstiegspunkte und bündelt deren Importe. Lassen Sie Tests außerhalb dieser Erkennungsstruktur.
- **Aktionspfade** Verwenden Sie die Code-Kennung oder die gekürzte Form. Beispielsweise löst `search_products` in `actions/search_products/index.js` auf oder für generierten Code `actions/search-products/index.js` auf. Der Anzeigename ist nicht der Lookup-Schlüssel.
- **Unterstützende Daten** z. B. eine generierte `data.json` werden gebündelt, wenn der Handler sie importiert.

## Lokale Entwicklung

Um Handler lokal ohne Adobe-Anmeldeinformationen zu entwickeln und zu testen, führen Sie Folgendes aus:

```bash
npm install
npm run dev:local
```

Der Entwicklungsbefehl erstellt das Projekt mit webpack und startet einen einfachen Node.js-HTTP-Server auf `http://localhost:9080`. Beenden Sie den `npm run dev:local` und führen Sie ihn erneut aus, nachdem der Handler oder die lokalen Metadaten geändert wurden. Der Befehl überwacht keine Dateien.

So verwenden Sie einen anderen Port:

```bash
PORT=9081 npm run dev:local
```

### Verhalten lokaler Metadaten

Die aktuelle Benutzeroberfläche bietet keinen `actions.json` Download, auch wenn die Textbausteindatei README und das Laufzeitstartbanner einen nennen. Der lokale Server startet auch ohne eine ausgefüllte Datei: Der Build erstellt `{"actions":[]}`, wenn die Datei fehlt, und die Laufzeit fällt auf die Erkennung des Handlers zurück.

Die Erkennung ist für die Auflistung von Handlern nützlich, ist jedoch kein vollständiger Argumenttestmodus. Ohne Eingabeschema ruft die MCP-SDK den Handler mit Laufzeitkontext als erstes Argument auf, anstatt die normale `(args, extra)`-Signatur zu verwenden. Die SDK leitet die bereitgestellten Aktionsargumente nicht weiter.

Erstellen Sie für MCP-Aufrufe mit Argumenten eine **nur lokal**-`actions.json` mit repräsentativen Metadaten, die mit dem Aktionseditor übereinstimmen. Für den Textbaustein-`echo`:

```json
{
  "actions": [
    {
      "name": "echo",
      "description": "Echo the supplied message.",
      "inputSchema": {
        "type": "object",
        "properties": {
          "message": { "type": "string" }
        },
        "required": ["message"]
      }
    }
  ]
}
```

Wenn das `actions`-Array nicht leer ist, registriert die Laufzeitumgebung nur die im Array genannten Aktionen. Verwenden Sie nach `name` die Code-Kennung jeder Aktion. Auch eine Aktion ohne Parameter benötigt ein Objektschema mit `"properties": {}`, um die normale Handler-Signatur beizubehalten.

Lokale Metadaten aktualisieren weder die Benutzeroberfläche noch die Bereitstellungskonfiguration. Die -Laufzeit validiert unterstützte Typen, erforderliche Felder, Aufzählungen und Array-Elemente, jedoch nicht alle JSON-Schema-Einschränkungen. Sie validiert Längen, Bereiche, verschachtelte Objekte und Geschäftsregeln im -Handler. Die Textbausteinintegrationstests verwenden `test/fixtures/actions.json` als repräsentative Metadaten.

### Lokale App-Variablen

Anwendungsvariablen erreichen den Handler über `extra.variables`, nicht über `process.env`. Lesen Sie sie mit `getVariable(extra, name)` aus `@adobe/llm-apps-runtime`. So simulieren Sie eine nicht empfindliche Einstellung nach dem Erstellen:

```bash
npm run build
node server/local.js \
  --param 'LLMA_VARIABLE_NAMES=["GREETING_PREFIX"]' \
  --param GREETING_PREFIX=Howdy
```

Geben Sie sowohl das JSON-Manifest als auch den benannten Wert an. Schließen Sie beim Testen seines normalen `(args, extra)`-Vertrags den Handler in lokale `actions.json` mit einem Eingabeschema ein. Die Befehlszeilenflags laden keine App-Einstellungen von der Plattform. Siehe [Konfigurieren von App-Variablen und ](/help/guides/app-variables.md)&quot; für umgebungsspezifische Bereitstellungseinstellungen.

### Testen mit cURL

Nachdem Sie die oben genannten lokalen `echo`-Metadaten hinzugefügt haben, erstellen und starten Sie den Server und führen Sie dann Folgendes aus:

```bash
# List all registered tools
curl -sX POST "http://localhost:9080" \
  -H 'content-type: application/json' \
  -H 'accept: application/json;q=1.0, text/event-stream;q=0.5' \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/list","params":{}}'

# Call the boilerplate echo action (returns "Echo: hello")
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

Handler-Komponententests sind live unter `test/actions/` und rufen die exportierte Funktion direkt ohne MCP-Schemavalidierung auf:

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
npm test                                 # configured Jest suite
npm test -- --runInBand test/actions/echo.test.js  # one boilerplate action
```

Einige generierte Tests verwenden den integrierten Test-Runner von Node anstelle von Jest. Überprüfen Sie die Testimporte und verwenden Sie den entsprechenden -Runner, z. B. `node --test test/actions/search-products.test.js` für eine Datei, die `node:test` importiert. Überprüfen Sie die generierten relativen Importe, nachdem Sie die Datei unter `test/actions/` platziert haben.

Modultests allein überprüfen keine Metadaten, MCP-Validierung, das Laden von Widgets oder das Verhalten des Hosts. Testen Sie den lokalen Endpunkt mit repräsentativen Metadaten und testen Sie dann die bereitgestellte App in einer unterstützten LLM-Plattform.

Nachdem die lokalen Tests erfolgreich waren, übertragen Sie die Änderungen und folgen Sie [Änderungen bereitstellen](/help/guides/deploy-your-app.md).
