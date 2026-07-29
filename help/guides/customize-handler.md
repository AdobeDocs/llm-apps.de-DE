---
title: Anpassen eines generierten Aktionshandlers
description: Machen Sie sich mit dem Adobe-LLM-Apps-Handler-Vertrag vertraut, ersetzen Sie generierte Beispieldaten und halten Sie die Handler-Ausgabe an sein Widget ausgerichtet.
source-git-commit: bb3d8a02f22a91ceeeba5999453aeb4221060f80
workflow-type: tm+mt
source-wordcount: '541'
ht-degree: 0%

---


# Anpassen eines generierten Handlers {#customize-generated-handler}

>[!IMPORTANT]
>
>[!DNL Adobe LLM Apps] befindet sich derzeit in Beta.
>
>Die hier gezeigten Funktionen, Workflows und Benutzeroberflächen stellen nicht unbedingt den endgültigen Status des Produkts dar. Um Beta beizutreten, senden Sie eine E-Mail an llm-apps-beta@adobe.com.

Die Plattform erstellt für jede generierte Aktion einen funktionierenden Handler. Der Handler gibt zunächst Beispieldaten zurück, damit Sie das gesamte Erlebnis testen können.

Verwenden Sie dieses Handbuch, um den Handler-Vertrag zu verstehen und die Beispieldaten durch Ihre APIs oder Datenquellen zu ersetzen.

**Journey:** Finden Sie den generierten Handler, → seine Eingaben und Ergebnisse zu verstehen → Ihr System zu verbinden → den Widget-Vertrag → testen und bereitzustellen ausgerichtet zu halten.

## Ermittelten Handler suchen

Öffnen Sie das beim Onboarding ausgewählte Handler-Repository:

```text
actions/
└── <action-name>/
    └── index.js
```

Die passenden Tests werden separat gespeichert:

```text
test/
└── actions/
    └── <action-name>.test.js
```

Bearbeiten Sie die generierte `index.js`. Ändern Sie keine Laufzeitdateien wie `entry.js`.

## Händlervertrag

Jeder Handler exportiert eine asynchrone Funktion:

```javascript
module.exports = async (args) => {
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

Die Funktion empfängt ein `args` Objekt und gibt ein Ergebnisobjekt zurück.

### Eingabe: `args`

`args` enthält die Parameter, die für die Aktion in [!DNL LLM Apps] definiert sind.

Für eine Aktion mit `category` und `query` Parametern:

```javascript
module.exports = async ({ category = '', query = '' } = {}) => {
  // Use the validated action arguments.
};
```

Die Laufzeit validiert das Eingabeschema, wenn die Aktionsmetadaten `inputSchema` enthalten, wie dies nach der Bereitstellung der Fall ist. Die lokale Discovery-Handler ohne `actions.json` wendet keine Schemavalidierung an. Der Handler sollte immer Geschäftsregeln durchsetzen, z. B. unterstützte Werte, maximale Längen und zulässige Kombinationen.

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

Halten Sie diese Antwort kurz. Geben Sie keine Anmeldeinformationen, internen Fehler oder Daten an, die der Benutzer nicht sehen darf.

### Ausgabe: `structuredContent`

Gibt `structuredContent` zurück, wenn die Aktion über ein Widget verfügt. Es muss ein einfaches Objekt sein, kein leeres Array.

```javascript
structuredContent: {
  products: [
    {
      id: 'P-100',
      name: 'Frescopa House Blend',
      price: '$14.99'
    }
  ],
  total: 1
}
```

`structuredContent` wird an das Widget gesendet, nicht an das LLM. Gibt nur die für die Schnittstelle erforderlichen Felder zurück.

Bei einer Aktion, die nur Text enthält, kann `structuredContent` weggelassen werden.

## Der Handler-Widget-Vertrag

Der Handler und das Widget haben einen gemeinsamen Vertrag: die Form der `structuredContent`.

```text
Action arguments
      ↓
Handler
      ├── content → LLM text response
      └── structuredContent → Widget
                                  ↓
                           bridge.toolResult
```

Das Widget liest das Handler-Ergebnis aus der LLM Apps SDK Bridge:

```javascript
export default async function decorate(block, bridge) {
  const result = await bridge.toolResult;
  const products = result?.structuredContent?.products ?? [];

  // Render products.
}
```

Wenn der Handler zurückgibt:

```javascript
structuredContent: {
  products: [...],
  total: 3
}
```

Das Widget muss `structuredContent.products` und `structuredContent.total` lesen.

Das Ändern eines Feldnamens oder -typs kann das Widget beschädigen. Aktualisieren Sie Handler, Widget und Tests gemeinsam.

## Beispieldaten ersetzen

Generierte Handler enthalten normalerweise ein speicherinternes Beispielarray. Ersetzen Sie diese Datensuche durch einen Server-seitigen Aufruf an Ihr System.

```javascript
const API_ORIGIN = process.env.PRODUCT_API_ORIGIN;
const API_TOKEN = process.env.PRODUCT_API_TOKEN;

module.exports = async ({ query = '' } = {}) => {
  const normalizedQuery = String(query).trim();
  if (!normalizedQuery || normalizedQuery.length > 200) {
    return {
      content: [{ type: 'text', text: 'Enter a valid product search.' }],
      structuredContent: { products: [], total: 0 }
    };
  }

  if (!API_ORIGIN || !API_TOKEN) {
    throw new Error('Product API configuration is unavailable.');
  }

  const origin = new URL(API_ORIGIN);
  if (origin.protocol !== 'https:') {
    throw new Error('Product API configuration must use HTTPS.');
  }

  const url = new URL('/v1/products', origin);
  url.searchParams.set('query', normalizedQuery);

  const response = await fetch(url, {
    headers: { Authorization: `Bearer ${API_TOKEN}` },
    signal: AbortSignal.timeout(8000)
  });

  if (!response.ok) {
    throw new Error('Product service request failed.');
  }

  const payload = await response.json();
  if (!payload || !Array.isArray(payload.products)
      || !payload.products.every((product) =>
        product
        && typeof product.id === 'string'
        && typeof product.name === 'string'
        && typeof product.price === 'string')) {
    throw new Error('Product service returned an unexpected response.');
  }

  const products = payload.products.map(({ id, name, price }) => ({
    id,
    name,
    price
  }));

  return {
    content: [
      { type: 'text', text: `Found ${products.length} matching products.` }
    ],
    structuredContent: {
      products,
      total: products.length
    }
  };
};
```

Schützen Sie den Netzwerkzugriff im Handler. API-Anmeldeinformationen niemals in Widget-JavaScript oder in die Quell-Code-Verwaltung einfügen.

## Erwartete Status verarbeiten

Bewahren Sie für jedes Ergebnis eine vorhersehbare Ausgabe-Form auf.

### Ergebnisse gefunden

```javascript
{
  content: [{ type: 'text', text: 'Found 3 products.' }],
  structuredContent: { products: [...], total: 3 }
}
```

### Keine Ergebnisse

```javascript
{
  content: [{ type: 'text', text: 'No matching products were found.' }],
  structuredContent: { products: [], total: 0 }
}
```

Das Widget kann jetzt einen leeren Status rendern, ohne zu erraten, ob `products` vorhanden ist.

Bei Service-Fehlern wird ein sicherer Fehler zurückgegeben oder ausgelöst, ohne Stacktraces, Token, interne Hosts oder Upstream-Antwortkörper verfügbar zu machen.

## Vertrag testen

Aktualisieren Sie die generierten Tests, wenn sich der Handler ändert. Cover:

- Gültige und ungültige Argumente.
- Status „Ergebnisse“ und „Keine Ergebnisse“.
- API-Fehler und Zeitüberschreitungen.
- Fehlerhafte API-Antworten.
- `content` ist immer vorhanden.
- `structuredContent` ist ein einfaches Objekt.
- Die vom Widget erwartete Form.

Ausführen:

```bash
npm test
```

Informationen zu lokalen MCP-Tests finden Sie unter [Lokale Handler-Entwicklung und -Tests](/help/reference/development.md).

## Änderung bereitstellen

1. Übergeben Sie die Handler-Änderungen und übertragen Sie sie.
2. Wenn sich die Daten-Form geändert hat, aktualisieren und pushen Sie das Widget.
3. [Bereitstellen der App](/help/guides/deploy-your-app.md) für das Staging.
4. [Testen Sie das ChatGPT-Plug-in](/help/guides/test-in-chatgpt.md).
5. Nachdem die Staging-Umgebung erfolgreich ausgeführt wurde, stellen Sie sie in der Produktionsumgebung bereit.

Siehe als Nächstes [Anpassen eines generierten Widgets](/help/guides/widgets.md).
