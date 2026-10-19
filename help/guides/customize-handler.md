---
title: Anpassen eines generierten Aktionshandlers
description: Machen Sie sich mit dem Adobe-LLM-Apps-Handler-Vertrag vertraut, ersetzen Sie generierte Beispieldaten und halten Sie die Handler-Ausgabe an sein Widget ausgerichtet.
source-git-commit: 0f9dd0a5b1008e183eb8ab5a4c2eae34a78923bb
workflow-type: tm+mt
source-wordcount: '880'
ht-degree: 0%
---

# Anpassen eines generierten Handlers {#customize-generated-handler}

>[!IMPORTANT]
>
>[!DNL Adobe LLM Apps] befindet sich derzeit in Beta.
>
>Die hier gezeigten Funktionen, Workflows und Benutzeroberflächen stellen nicht unbedingt den endgültigen Status des Produkts dar. Um Beta beizutreten, senden Sie eine E-Mail an llm-apps-beta@adobe.com.

Die Plattform erstellt für jede generierte Aktion einen funktionierenden Handler. Der Handler gibt zunächst Beispieldaten zum Testen des gesamten Erlebnisses zurück.

**In diesem Handbuch** Erfahren Sie, wie Sie den generierten Handler finden, seine Beispieldaten durch Aufrufe an Ihre Systeme ersetzen und seine Antwort mit dem Widget kompatibel halten. Testen und stellen Sie dann Ihre Änderungen bereit.

## Ermittelten Handler suchen

Öffnen Sie das Handler-Repository, das während der [Infrastruktureinrichtung“ verbunden &#x200B;](/help/guides/create-app.md). Wenn Sie Adobe Managed Infrastructure verwenden, fordern Sie den Code-Zugriff an, bevor Sie fortfahren.

```text
actions/
└── <action-slug>/
    ├── index.js
    └── data.json
```

Übereinstimmende Tests werden in einem separaten Verzeichnis live geschaltet:

```text
test/
└── actions/
    └── <action-slug>.test.js
```

Verwenden Sie die generierte Pull-Anfrage , um die genauen Dateien zu finden. Der Ordner-Slug verwendet Bindestriche: Die Plattform generiert eine Aktion mit der Code-Kennung `search_products` unter `actions/search-products/`. Die Laufzeit erkennt das Formular mit Bindestrich sowie einen Ordner, der genau mit der Kennung übereinstimmt.

Bearbeiten Sie die generierte `index.js` und alle zugehörigen Beispieldaten. Ändern Sie keine Laufzeitdateien wie `entry.js`.

## Händlervertrag

Jeder Handler exportiert eine asynchrone Funktion:

```javascript
module.exports = async (args, extra) => {
  return {
    content: [
      { type: 'text', text: 'Response for the LLM platform.' }
    ],
    structuredContent: {
      // Data for the widget.
    }
  };
};
```

Für eine Aktion mit einem Eingabeschema empfängt die Funktion ein `args` und einen optionalen Laufzeitkontext, `extra` und gibt ein Ergebnisobjekt zurück. Verwenden Sie `extra` für Runtime-Helper wie `getVariable(extra, 'PRODUCT_API_ORIGIN')`; App-Variablen werden nicht automatisch über `process.env` verfügbar gemacht. Siehe [Konfigurieren von App-Variablen und Geheimnissen](/help/guides/app-variables.md).

### Eingabe: `args`

`args` enthält die Parameter, die für die Aktion in [!DNL LLM Apps] definiert sind.

Für eine Aktion mit `category` und `query` Parametern:

```javascript
module.exports = async ({ category = '', query = '' } = {}, extra) => {
  // Validate business rules before using the arguments.
};
```

Mit Aktionsmetadaten konvertiert die Laufzeit das deklarierte Eingabeschema in die Validierung für unterstützte Regeln, einschließlich erforderlicher Felder, primitiver Typen, Aufzählungen und Array-Elemente. Die Laufzeit führt keine vollständige Validierung des JSON-Schemas durch. Überprüfen Sie Einschränkungen wie Zeichenfolgenlängen, numerische Bereiche und verschachtelte Objekteigenschaften im -Handler.

Die lokale Erkennung ohne Schema-Metadaten entspricht nicht der Bereitstellung: Die -Laufzeitumgebung übergibt keine Aktionsargumente im normalen `(args, extra)`. Verwenden Sie direkte Handler-Tests oder repräsentative lokale `actions.json`-Metadaten, um Argumente zu testen. Siehe [Lokale Handlerentwicklung und -tests](/help/reference/development.md).

### Ausgabe: `content`

Kehren Sie immer `content` zurück. Es handelt sich um ein Array von Inhaltskomponenten, die von der LLM-Plattform und von Hosts gelesen werden, die keine Widgets anzeigen.

```javascript
content: [
  {
    type: 'text',
    text: 'Found 3 products matching your search.'
  }
]
```

Halten Sie die Textantwort kurz. Geben Sie keine Anmeldeinformationen, internen Fehler oder Daten an, die der Benutzer nicht sehen darf.

### Ausgabe: `structuredContent`

Gibt `structuredContent` zurück, wenn die Aktion über ein Widget verfügt. Es muss ein einfaches Objekt sein, kein leeres Array.

```javascript
structuredContent: {
  arguments: { query: 'coffee' },
  records: [
    {
      id: 'P-100',
      name: 'Frescopa House Blend',
      price: '$14.99'
    }
  ]
}
```

`structuredContent` ist Teil des MCP-Aktionsergebnisses, das an den Host übermittelt wird. Gehen Sie nicht davon aus, dass dies für das Modell oder den Benutzer ausgeblendet ist. Nur autorisierte Daten zurückgeben, die für das Erlebnis erforderlich sind, nie Anmeldeinformationen oder Konfiguration des privaten Servers.

Bei einer Aktion, die nur Text enthält, ist die Rückgabe von `structuredContent` optional.

## Der Handler-Widget-Vertrag

Der Handler und das Widget haben einen gemeinsamen Vertrag: die Form der `structuredContent`.

| Handler-Ausgabe | Zweck |
|---|---|
| `content` | Modelllesbare Antwort und Fallback nur Text |
| `structuredContent` | Daten, die an den Host übermittelt und vom Widget über `bridge.toolResult` gelesen werden |

Das Widget liest das Handler-Ergebnis aus der LLM Apps SDK Bridge:

```javascript
export default async function decorate(block, bridge) {
  if (!bridge?.isConnected || bridge.hostContext?.preview === true) {
    // Render the block's local preview data instead.
    return;
  }
  const result = await bridge.toolResult;
  const records = result?.structuredContent?.records ?? [];

  // Render records.
}
```

Aktuell generierte Handler verwenden ein -Objekt, das `arguments` und `records` enthält. Bestehende Apps können eine andere Form verwenden. Überprüfen Sie daher Ihre generierten Dateien, bevor Sie sie bearbeiten. Das Ändern eines Feldnamens oder -typs kann das Widget beschädigen. Aktualisieren Sie Handler, Widget und Tests gemeinsam.

## Beispieldaten ersetzen

Generierte Handler laden repräsentative Datensätze aus `data.json`. Ersetzen Sie die Beispieldatensuche durch einen Server-seitigen Aufruf an Ihr System. Im folgenden Beispiel wird eine öffentliche HTTPS-Katalog-API verwendet und ihr nicht sensibler Ursprung aus einer App-Variablen gelesen. Passen Sie die Validierung der Antwort an Ihre API an.

```javascript
const { getVariable } = require('@adobe/llm-apps-runtime');

module.exports = async ({ query = '' } = {}, extra) => {
  const normalizedQuery = typeof query === 'string' ? query.trim() : '';
  if (!normalizedQuery || normalizedQuery.length > 200) {
    return {
      content: [{ type: 'text', text: 'Enter a valid product search.' }],
      structuredContent: { arguments: {}, records: [] },
      isError: true
    };
  }

  const knownArguments = { query };
  try {
    const apiOrigin = getVariable(extra, 'PRODUCT_API_ORIGIN');
    if (!apiOrigin) {
      throw new Error('Missing configuration');
    }
    const origin = new URL(apiOrigin);
    if (origin.protocol !== 'https:' || origin.username || origin.password) {
      throw new Error('Invalid API origin');
    }
    const url = new URL('/v1/products', origin);
    url.searchParams.set('query', normalizedQuery);

    const response = await fetch(url, {
      signal: AbortSignal.timeout(8000)
    });
    if (!response.ok) {
      throw new Error('Request failed');
    }
    const payload = await response.json();
    if (!payload || !Array.isArray(payload.products)
        || !payload.products.every((product) =>
          product
          && typeof product.id === 'string'
          && typeof product.name === 'string'
          && typeof product.price === 'string')) {
      throw new Error('Unexpected response');
    }

    const records = payload.products.map(({ id, name, price }) => ({
      id,
      name,
      price
    }));

    return {
      content: [
        { type: 'text', text: `Found ${records.length} matching products.` }
      ],
      structuredContent: { arguments: knownArguments, records }
    };
  } catch {
    return {
      content: [{ type: 'text', text: 'Product search is unavailable. Try again later.' }],
      structuredContent: { arguments: knownArguments, records: [] },
      isError: true
    };
  }
};
```

Schützen Sie den Netzwerkzugriff im Handler. API-Anmeldeinformationen niemals in Widget-JavaScript oder in die Quell-Code-Verwaltung einfügen. Informationen zu Anfragen, die im Namen eines angemeldeten Benutzers gestellt werden, finden Sie unter [Endbenutzer authentifizieren](/help/guides/authentication.md).

Informationen zu nicht sensiblen Einstellungen, die Ihr Handler benötigt, z. B. eine Service-URL, finden Sie unter [Konfigurieren von App-Variablen und -Geheimnissen](/help/guides/app-variables.md). Konfigurieren Sie Variablen für jede Umgebung separat. Variablenänderungen werden bei der nächsten Bereitstellung wirksam. Die aktuelle Benutzeroberfläche unterstützt nur das Hinzufügen von Variablen. Verwenden Sie keine Variablen zum Speichern von API-Anmeldeinformationen.

## Erwartete Status verarbeiten

Bewahren Sie für jedes Ergebnis eine vorhersehbare Ausgabe-Form auf.

### Ergebnisse gefunden

```javascript
{
  content: [{ type: 'text', text: 'Found 3 products.' }],
  structuredContent: { arguments: { query: 'coffee' }, records: [...] }
}
```

### Keine Ergebnisse

```javascript
{
  content: [{ type: 'text', text: 'No matching products were found.' }],
  structuredContent: { arguments: { query: 'coffee' }, records: [] }
}
```

Durch eine vorhersehbare Ergebnisform kann das Widget einen leeren Status rendern, ohne zu erraten, ob `records` vorhanden ist.

Bei Service-Fehlern geben Sie mit `isError: true` ein sicheres Fehlerergebnis zurück, wie im Beispiel gezeigt. Aktivieren Sie `result.isError` im -Widget, damit eine fehlgeschlagene Anfrage nicht als erfolgreiche leere Suche angezeigt wird. Stellen Sie keine Stacktraces, Anmeldeinformationen, internen Hosts oder Upstream-Antworttexte bereit.

## Vertrag testen

Aktualisieren Sie die generierten Tests, wenn sich der Handler ändert. Cover:

- Gültige und ungültige Argumente.
- Status „Ergebnisse“ und „Keine Ergebnisse“.
- API-Fehler und Zeitüberschreitungen.
- Fehlerhafte API-Antworten.
- Vorhandensein von `content` in jedem Ergebnis.
- Ein einfaches Objekt für `structuredContent`.
- Die vom Widget erwartete Form.

Führen Sie die konfigurierte Test-Suite des Repositorys aus:

```bash
npm test
```

Wenn ein generierter Test `node:test` anstatt Jest importiert, führen Sie ihn mit `node --test test/actions/<action-slug>.test.js` aus und überprüfen Sie, ob der Importpfad von `test/actions/` aufgelöst wird. Verwenden Sie den der Datei entsprechenden Test-Runner. Der `npm test`-Befehl des Textbausteins führt Jest aus.

Informationen zu lokalen MCP-Tests finden Sie unter [Lokale Handler-Entwicklung und -Tests](/help/reference/development.md).

## Änderung bereitstellen

1. Übergeben Sie die Handler-Änderungen und übertragen Sie sie.
2. Wenn Sie die Datenform ändern, aktualisieren und pushen Sie das Widget.
3. [Bereitstellen der App](/help/guides/deploy-your-app.md) für das Staging.
4. Testen Sie in einer unterstützten LLM-Plattform. Siehe [Test in ChatGPT](/help/guides/test-in-chatgpt.md) oder [Test in Claude](/help/guides/test-in-claude.md).
5. Nachdem die Tests im Staging bestanden wurden, stellen Sie sie in der Produktion bereit.

Siehe als Nächstes [Anpassen eines generierten Widgets](/help/guides/widgets.md).
