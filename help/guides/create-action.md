---
title: Erstellen einer neuen Aktion
description: Definieren Sie Aktionsmetadaten, implementieren Sie den Handler, verbinden Sie ein EDS-Widget, testen Sie es und stellen Sie es mit Adobe LLM-Apps bereit.
source-git-commit: bb3d8a02f22a91ceeeba5999453aeb4221060f80
workflow-type: tm+mt
source-wordcount: '1137'
ht-degree: 1%

---


# Erstellen einer Aktion von Grund auf {#create-action-from-scratch}

>[!IMPORTANT]
>
>[!DNL Adobe LLM Apps] befindet sich derzeit in Beta.
>
>Die hier gezeigten Funktionen, Workflows und Benutzeroberflächen stellen nicht unbedingt den endgültigen Status des Produkts dar. Um Beta beizutreten, senden Sie eine E-Mail an llm-apps-beta@adobe.com.

>[!NOTE]
>
>In diesem Handbuch wird von einer grundlegenden Vertrautheit mit Adobe Edge Delivery Services (EDS) ausgegangen. Wenn Sie neu bei EDS sind, lesen Sie zunächst das [EDS-Entwickler-Tutorial](https://www.aem.live/developer/tutorial) und [Erkunden von Blöcken](https://www.aem.live/docs/exploring-blocks) um die Grundlagen - Blöcke, die `decorate` und die EDS-Projektstruktur - zu lernen, bevor Sie ein Widget verbinden.

Verwenden Sie dieses Handbuch, um eine Funktion hinzuzufügen, die von der Plattform nicht erstellt wurde. Sie definieren die Aktion in [!DNL LLM Apps], schreiben den Handler in das verknüpfte Repository, fügen bei Bedarf ein Widget hinzu, testen sie und stellen sie bereit.

**Journey:** Planen Sie die Aktion, → die Metadaten zu erstellen → den Handler zu schreiben → das Widget zu verbinden → lokal zu testen → das Plug-in bereitzustellen und zu testen.

Beginnen Sie Ihre erste App mit [Erste App automatisch erstellen](/help/guides/create-app.md).

## Bevor Sie beginnen

Sie benötigen:

- Eine vorhandene LLM-App.
- Ein verknüpftes Handler-Repository.
- Das Repository wird lokal geklont, und die Abhängigkeiten werden installiert.
- Ein EDS-Projekt, wenn die Aktion ein Widget anzeigt.
- Eine klare API oder Datenquelle für Produktionsergebnisse.

## Planen der Aktion

Eine Aktion sollte nur eine einzige klare Benutzeraufgabe ausführen. Bevor Sie die Benutzeroberfläche öffnen, definieren Sie Folgendes:

- **Intent** - Was der Benutzer zu erreichen versucht.
- **Beschreibung** - wann die LLM-Plattform diese Aktion auswählen soll.
- **Eingaben** - die vom Benutzer mindestens benötigten Informationen.
- **Result** - der Text und die strukturierten Daten, die vom Handler zurückgegeben werden.
- **Verhalten** - ob die Aktion Daten liest, Daten ändert oder externe Systeme aufruft.
- **Widget** — ob das Ergebnis eine visuelle Schnittstelle benötigt.

Eine Aktion **Produkte suchen** könnte beispielsweise Folgendes verwenden:

```text
Intent: Find products matching a category or search phrase
Inputs:
  category: optional string
  query: optional string
Result:
  content: text summary
  structuredContent: products and total count
Behavior: read-only, idempotent, open-world
Widget: product cards
```

Verwandte, aber unterschiedliche Aufgaben getrennt halten. Produktsuche und Produktkauf sollten nicht eine Aktion sein, da sie unterschiedliche Eingaben, Risiken und Bestätigungsanforderungen haben.

## Erstellen der Aktionsmetadaten

Öffnen Sie die App und wählen Sie **[!UICONTROL Aktionen]** und dann **[!UICONTROL Aktion erstellen]** aus.

Der Editor enthält die Registerkarten **[!UICONTROL Aktion]** und **[!UICONTROL Widget-]**).

### Einfache Informationen eingeben

![Aktion erstellen - Grundlegende Informationen](/help/assets/guide-create-action/action-basic-info.png)

Geben Sie Folgendes ein:

- **[!UICONTROL Aktionsname]** - Ein kurzer Aufgabenname, z. B *„Produkte*.
- **[!UICONTROL Beschreibung]** — Erklären Sie, wann die Aktion verwendet wird und was sie zurückgibt.

Eine nützliche Beschreibung ist spezifisch:

```text
Search the product catalog by category or keyword. Returns matching
products with their names, prices, categories, and image URLs.
```

Vermeiden Sie vage Beschreibungen wie *Abrufen von Produktinformationen*. Die LLM-Plattform verwendet die Beschreibung zur Auswahl zwischen Aktionen.

### Auswählen von Anmerkungen

Anmerkungen beschreiben das Verhalten der Aktion:

- **Destruktiver Hinweis** - Die Aktion kann Daten löschen oder dauerhaft ändern.
- **Idempotent (gleiche Argumente = kein zusätzlicher Effekt)** - Die Wiederholung derselben Anfrage hat die gleiche Wirkung.
- **Open World Hint** - Die Aktion kommuniziert mit externen Systemen.
- **Schreibgeschützter Hinweis** - Die Aktion ändert die Daten nicht.

Wählen Sie nur Anmerkungen aus, die wahr sind. Beispielsweise ist die Produktsuche normalerweise schreibgeschützt, idempotent und offen.

### OpenAI-Metadaten hinzufügen

Geben Sie kurze Nachrichten ein, die während und nach Abschluss der Aktion angezeigt werden:

```text
Invoking: Searching products...
Invoked: Products found
```

Für Aktionen mit Widgets fügen Sie **[!UICONTROL Widget-Beschreibung]** hinzu. Dies unterscheidet sich von der Aktionsbeschreibung:

- **Aktionsbeschreibung** hilft dem Modell bei der Entscheidung, wann die Aktion aufgerufen werden soll.
- **Widget-Beschreibung** wird `_meta["openai/widgetDescription"]` zugeordnet und fasst zusammen, was die gerenderte Komponente anzeigt, wodurch der wiederholte Narrativ reduziert wird.

[!DNL LLM Apps] gilt als Komponentenmetadaten. Geben Sie sie nicht vom Handler zurück.

### Sichtbarkeit konfigurieren

- **[!UICONTROL KI-Modell bereitstellen]** ermöglicht dem Modell die Auswahl der Aktion.
- **[!UICONTROL Als Widget in der Programmoberfläche anzeigen]** zeigt das konfigurierte Widget an.

Widget-Sichtbarkeit deaktivieren, wenn die Aktion nur Text zurückgibt.

### Eingabeparameter hinzufügen

Fügen Sie für jeden Wert, den der Handler akzeptiert, einen Parameter hinzu. Jeder Parameter benötigt:

- **Name** - der vom Handler empfangene Schlüssel.
- **type** - Zeichenfolge, Zahl, Ganzzahl oder Boolesch.
- **Beschreibung** - wie das Modell den Wert extrahieren soll.
- **Erforderlich** - ob die Aktion ohne sie ausgeführt werden kann.

Für **Produkte suchen**:

```text
category
  Type: String
  Required: No
  Description: Product category used to narrow the catalog.

query
  Type: String
  Required: No
  Description: Product name or search phrase.
```

Verwenden Sie stabile Parameternamen. Zum Ändern eines Namens müssen auch der Handler und seine Tests geändert werden.

### Analytics konfigurieren

Aktivieren Sie **[!UICONTROL Benutzerabsicht erfassen]** wenn Sie möchten, dass Analytics eine Zusammenfassung der Konversation enthält, die zu der Aktion geführt hat.

![Aktion erstellen - Analyse der Benutzerabsicht](/help/assets/guide-create-action/action-analytics-user-intent.png)

Vollständige Felddefinitionen finden Sie unter [Aktion und Widget-Felder](/help/reference/reference-docs.md).

## Konfigurieren des Widgets

Überspringen Sie diesen Abschnitt für eine Aktion, die nur Text enthält.

Öffnen Sie **[!UICONTROL Widget-Metadaten]**.

![Aktion erstellen — Widget-Metadaten](/help/assets/guide-create-action/widget-metadata.png)

Konfigurieren:

- **Typ** — EDS auswählen.
- **Widget-Domain** - der EDS-Ursprung, der das Widget hostet.
- **Bevorzugter Rahmen** - Fordert einen umrandeten Container im Host an.
- **Script URL** - der Einstiegspunkt des EDS-Widgets.
- **Widget URL** - die veröffentlichte EDS-Seite für diese Aktion.

Typische URLs sind:

```text
Script URL:
https://main--<repo>--<owner>.aem.live/scripts/aem-embed.js

Widget URL:
https://main--<repo>--<owner>.aem.live/<widget-page>
```

Gewähren Sie nur erforderliche Browser-Berechtigungen und CSP-Domains.

![Aktion erstellen - Berechtigungen und CSP](/help/assets/guide-create-action/widget-permissions-csp.png)

Wenn das EDS-Projekt oder die Widget-Seite noch nicht vorhanden ist, schließen Sie [Eigenes EDS-Projekt ](/help/guides/bring-your-own-eds.md)) ab und kehren Sie dann zur Aktion zurück.

## Speichern der Aktion

Wählen Sie **[!UICONTROL Neue Aktion erstellen]** aus. Die Aktion wird auf der Seite Aktionen mit dem Badge **Nicht bereitgestellt** angezeigt.

An dieser Stelle sind die Metadaten vorhanden, für die Aktion ist jedoch weiterhin ein Handler erforderlich.

## Implementieren des Handlers

Klonen Sie das verknüpfte Handler-Repository und installieren Sie dessen Abhängigkeiten:

```bash
npm install
```

Erstellen:

```text
actions/
└── search-products/
    └── index.js
```

Der Ordnername muss mit der Code-Kennung der Aktion übereinstimmen, die im Aktionseditor angezeigt wird.

Die vollständige Beziehung zwischen Ergebnisvertrag und Handler-Widget finden Sie unter [Anpassen eines generierten Handlers](/help/guides/customize-handler.md).

### Händlervertrag

Exportieren Sie eine asynchrone Funktion:

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

Der Handler empfängt die in der Benutzeroberfläche definierten Parameter.

### `content`

`content` ist der von der LLM-Plattform gelesene Fallback-Text:

```javascript
content: [
  { type: 'text', text: 'Found 3 matching products.' }
]
```

Geben Sie immer nützliche `content` zurück, auch wenn die Aktion über ein Widget verfügt.

### `structuredContent`

`structuredContent` ist ein einfaches Objekt, das vom Widget genutzt wird:

```javascript
structuredContent: {
  products: [
    { id: 'P-100', name: 'Product A', price: '$20' }
  ],
  total: 1
}
```

Die Form muss mit dem übereinstimmen, was der EDS-Block aus `bridge.toolResult` liest.

### Verbinden einer API

Schützen Sie den API-Zugriff im Server-seitigen Handler. Laden Sie die Konfiguration aus der Laufzeitumgebung und verwenden Sie eine feste HTTPS-Herkunft.

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

  const products = payload.products.map((product) => ({
    id: product.id,
    name: product.name,
    price: product.price
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

Geben Sie keine API-Anmeldeinformationen in Quell-Code, Aktionsmetadaten, Widget-JavaScript, Protokolle oder benutzerseitige Fehler ein.

Für Produktions-Code validieren Sie die vollständige Upstream-Antwort, bevor Sie genehmigte Felder `structuredContent` zuordnen.

## Hinzufügen von Handler-Tests

Erstellen Sie den passenden Test:

```text
test/
└── actions/
    └── search-products.test.js
```

Mindestens testen:

- Gültige Eingabe.
- Fehlende oder ungültige Eingabe.
- Leere Ergebnisse.
- API-Zeitüberschreitung oder -Fehler.
- Fehlerhafte API-Daten.
- Die vom Widget erwartete `structuredContent`.

Ausführen:

```bash
npm test
```

Informationen zum Projektlayout und zu lokalen MCP-Tests finden Sie unter [Lokale Handlerentwicklung und -tests](/help/reference/development.md).

## Lokales Testen der Aktion

Ausführen:

```bash
npm run dev:local
```

Ohne eine lokale `actions.json` erkennt der Server den Handler mit minimalen Metadaten und ohne Validierung des Eingabeschemas.

Verwenden Sie MCP Inspector oder `curl`, um:

1. Auflisten der registrierten Aktionen.
2. Rufen Sie die neue Aktion mit repräsentativen Argumenten auf.
3. Überprüfen Sie `content` und `structuredContent`.
4. Testen Sie ungültige und leere Anfragen.

## Verbinden und Testen des Widgets

Wenn die Aktion über ein Widget verfügt:

1. Veranlassen Sie das Widget, die `structuredContent` des Handlers zu lesen.
2. Rendern Sie externe Werte mit sicheren DOM-APIs wie `textContent`.
3. Fügen Sie den Status „Laden“, „Leer“ und „Fehler“ hinzu.
4. Zeigen Sie die EDS-Seite lokal in der Vorschau an.
5. Überprüfen Sie CSP-, CORS- und Widget-URLs.

Siehe [Eigenes EDS-Projekt ](/help/guides/bring-your-own-eds.md).

## Bereitstellen und Testen

1. Übertragen Sie die Änderungen an Handler und Widget und übertragen Sie sie.
2. [Bereitstellen der App](/help/guides/deploy-your-app.md) für das Staging.
3. [Testen Sie das ChatGPT-Plug-in](/help/guides/test-in-chatgpt.md).
4. Überprüfen Sie die Eingabeaufforderungen, die die Aktion aufrufen sollen und nicht sollten.
5. Nachdem die Staging-Umgebung erfolgreich ausgeführt wurde, stellen Sie sie in der Produktionsumgebung bereit.

Wenn Metadaten ohne übereinstimmenden Handler vorhanden sind, registriert die Bereitstellung die Aktion mit einem Standard-Stub. Fügen Sie den Handler hinzu, bevor Sie die Aktion den Benutzern zur Verfügung stellen.
- [Anleitung: Einrichten des Widgets (EDS)](/help/guides/widgets.md)
