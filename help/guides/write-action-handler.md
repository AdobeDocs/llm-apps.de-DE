---
title: Schreiben des Aktions-Handlers
description: Erfahren Sie, wie Sie einen Aktionshandler für Ihre Adobe-LLM-App schreiben, einschließlich des Handlers Contract, StructuredContent und eines Arbeitsbeispiels.
source-git-commit: 1a99e2e80e50a3bcf9ce6fb910365202bf06e113
workflow-type: tm+mt
source-wordcount: '719'
ht-degree: 0%

---


# Schreiben des Aktions-Handlers

>[!IMPORTANT]
>
>[!DNL Adobe LLM Apps] befindet sich derzeit in Beta.
>
>Die hier gezeigten Funktionen, Workflows und Benutzeroberflächen stellen nicht unbedingt den endgültigen Status des Produkts dar. Um Beta beizutreten, senden Sie eine E-Mail an llm-apps-beta@adobe.com.

Nachdem Sie eine Aktion in der [!DNL Adobe LLM Apps]-Benutzeroberfläche erstellt haben, werden die Metadaten in der [!DNL LLM Apps]-API gespeichert, aber es gibt noch keinen Code dahinter. Dieses Handbuch führt Sie durch das Schreiben der Handler-Funktion, die ausgeführt wird, wenn eine LLM-Plattform (z. B. [!DNL ChatGPT] oder Claude) Ihre Aktion aufruft.

Details zum Projektlayout, zur lokalen Entwicklung und zu Tests finden Sie unter [Entwicklung](/help/reference/development.md).

## Entwicklervertrag

Sie schreiben nur Handler. Alle anderen Elemente - Aktionsname, Beschreibung, Eingabeschema, Anmerkungen, Widget-Sichtbarkeit, Berechtigungen, CSP - befinden sich in der [!DNL LLM Apps]-Benutzeroberfläche und werden zur Bereitstellungszeit automatisch an die Laufzeit übermittelt. Sie bearbeiten Metadaten nie manuell in Ihrem Repository und registrieren kein Tool im Code.

| Sorge | Wo sie lebt |
|---------|----------------|
| Metadaten (Name, Beschreibung, Schema, Widget-Einstellungen) | [!DNL LLM Apps] Benutzeroberfläche - in der API gespeichert |
| Handler-Code (die ausgeführte Funktion) | Ihr [!DNL GitHub] Repository - `actions/<name>/index.js` |
| `actions.json` (Metadaten-Snapshot) | Geschrieben von der Bereitstellungs-Pipeline; heruntergeladen von der Benutzeroberfläche für lokale Entwicklung |

## Erste Schritte

Ihr verknüpftes Repository benötigt die Projektstruktur, bevor Sie Handler schreiben können. Klonen Sie den **[Adobe LLM Apps-Textbaustein](https://github.com/Adobe-AIFoundations/llm-apps-boilerplate)** um mit einem leeren Ausgangspunkt zu beginnen.

Inhalte an das Repository senden, das Sie bei der Erstellung der App verknüpft haben (z. B. `your-org/your-repo`).

Sobald der Code eingerichtet ist, führen Sie Folgendes aus:

```bash
npm install
```

Dadurch werden alle Abhängigkeiten installiert, einschließlich [`@adobe/llm-apps-runtime`](https://www.npmjs.com/package/@adobe/llm-apps-runtime) - die Laufzeit, die die MCP-Protokollkommunikation, die Aktionssuche und das Anforderungsrouting verarbeitet. Sie interagieren nicht direkt mit der Laufzeit, sondern sie wird von `entry.js` zur Build-Zeit genutzt.

>[!TIP]
>
>Wenn Sie [Claude-Code](https://claude.ai/code) oder [Cursor](https://cursor.com) verwenden, enthält das Textbaustein `.claude/skills/llm-apps-action-author/` eine gebrauchsfertige Claude-Fähigkeit. Es kann neue Aktionen anlegen, Testdateien generieren, Handler-Shapes validieren und Sie durch den Handler-Vertrag führen - alles über Ihren Editor. Um sie zu verwenden, bitten Sie Claude, „eine Aktion namens search-products“ ** und sie folgt automatisch den richtigen Projektkonventionen.

## Händlervertrag

Ein Handler ist `actions/<name>/index.js` eine einzelne Datei, die eine asynchrone Funktion exportiert:

```javascript
module.exports = async (args) => {
  return {
    content: [{ type: 'text', text: 'response for the LLM' }],
    structuredContent: { /* data for the widget */ }
  }
}
```

Die Funktion empfängt die Eingabeargumente der Aktion als einfaches Objekt - dies sind die Parameter, die Sie im Dialogfeld Aktion erstellen definiert haben. Der Server validiert sie anhand des Eingabeschemas, bevor der Handler aufgerufen wird.

### `content` (erforderlich)

Ein Array von Inhaltskomponenten, die an den LLM gesendet werden, und Nur-Text-Hosts. So formuliert die LLM-Plattform ihre Antwort.

```javascript
content: [
  { type: 'text', text: 'Found 5 products matching category "bagged-coffee".' }
]
```

Geben Sie immer `content` zurück - dies ist der universelle Fallback für jeden Host.

### `structuredContent`

Ein einfaches JavaScript-Objekt, das an das Widget gesendet wird. Diese Daten haben **keine Token-Kosten** - sie werden vom EDS-Widget-Block verwendet, um eine Rich-UI wie ein Produktkarussell oder eine Zuordnung zu rendern.

```javascript
structuredContent: {
  products: [
    { name: 'Product A', category: 'bagged-coffee', imageUrl: '...' },
    { name: 'Product B', category: 'bagged-coffee', imageUrl: '...' }
  ],
  total: 2,
  category: 'bagged-coffee'
}
```

Die Struktur liegt bei Ihnen - sie muss mit dem übereinstimmen, was Ihr EDS-Widget-Block über `bridge.toolResult` erwartet.

>[!IMPORTANT]
>
>`structuredContent` muss ein einfaches Objekt sein, kein leeres Array.

### `_meta` (optional)

Zusätzliche Metadaten werden zusammen mit dem Ergebnis gesendet. Der `openai/widgetDescription` Schlüssel teilt der LLM-Plattform mit, wie das Widget präsentiert werden soll:

```javascript
_meta: {
  'openai/widgetDescription': 'The widget displays a scrollable product carousel. '
    + 'Do NOT repeat the product list. Instead, highlight one or two recommendations.'
}
```

## Beispiel: Produkt-Handler suchen

Im Folgenden finden Sie ein Beispiel für einen `search-products`. Er akzeptiert einen optionalen `category` und ein `query`, durchsucht einen Produktkatalog und gibt sowohl eine Textzusammenfassung für das LLM als auch strukturierte Daten für das Widget-Karussell zurück.

>[!NOTE]
>
>In diesem Beispiel wird aus Gründen der Einfachheit ein hartcodiertes Produkt-Array verwendet. In einer echten Anwendung rufen Sie normalerweise Ihre eigene Produkt-API oder Datenbank auf, um Ergebnisse dynamisch abzurufen.

```javascript
// actions/search-products/index.js

const PRODUCTS = [
  {
    name: 'Product A',
    description: 'A short description of Product A.',
    category: 'bagged-coffee',
    sub_category: 'dark-roast',
    image_url: 'https://www.example.com/products/product-a/hero.jpg',
    url: 'https://www.example.com/products/product-a',
    productId: 'PROD-001',
    rating: 4.7,
    reviewCount: 58
  },
  // ... more products
];

const WIDGET_DESCRIPTION = 'The widget displays a scrollable product carousel '
  + 'with images, star ratings, and review counts. Do NOT repeat the product list.';

module.exports = async ({ category = '', query = '' } = {}) => {
  let results = PRODUCTS;

  if (category) {
    const categoryLower = category.toLowerCase();
    results = results.filter((p) =>
      p.category.toLowerCase().includes(categoryLower)
      || p.sub_category.toLowerCase().includes(categoryLower)
    );
  }

  if (query) {
    const queryLower = query.toLowerCase();
    results = results.filter((p) =>
      p.name.toLowerCase().includes(queryLower)
      || p.description.toLowerCase().includes(queryLower)
    );
  }

  const products = results.map((p) => ({
    productId: p.productId,
    name: p.name,
    shortDescription: p.description,
    category: p.category,
    rating: p.rating,
    reviewCount: p.reviewCount,
    imageUrl: p.image_url,
    productUrl: p.url,
  }));

  if (products.length === 0) {
    return {
      content: [{ type: 'text', text: `No products found for "${category}".` }],
      structuredContent: { products: [], total: 0, category: null },
      _meta: { 'openai/widgetDescription': WIDGET_DESCRIPTION }
    };
  }

  return {
    content: [
      { type: 'text', text: `Found ${products.length} product(s) in "${category}".` }
    ],
    structuredContent: { products, total: products.length, category },
    _meta: { 'openai/widgetDescription': WIDGET_DESCRIPTION }
  };
};
```

**Was geschieht zur Laufzeit:**

1. Ein Benutzer fragt die LLM-Plattform *„Zeige mir deine Kaffeeprodukte“*
2. Die LLM-Plattform stimmt mit der Absicht *Produkte suchen* überein und extrahiert `category`.
3. Der MCP-Server ruft Ihren Handler mit `{ category: 'bagged-coffee' }` auf.
4. Ihr Handler filtert den Katalog und gibt `content` (Textzusammenfassung für das LLM) + `structuredContent` (Produkt-Array für das Widget) zurück.
5. Die LLM-Plattform zeigt die Textantwort an und übergibt die strukturierten Daten an das EDS-Widget, das ein Produktkarussell rendert.

## Was passiert, wenn der Handler fehlt?

Wenn Sie eine Aktion in der Benutzeroberfläche definiert haben, die Handler-Datei jedoch noch nicht erstellt haben, wird die Aktion zum Zeitpunkt der Bereitstellung weiterhin registriert. Für Aufrufe wird ein Standard-Stub-Handler verwendet, der leere Inhalte zurückgibt, bis Sie den echten Code hinzufügen. Dies bedeutet, dass Sie zuerst alle Ihre Aktionen in der Benutzeroberfläche definieren und inkrementell implementieren können.

