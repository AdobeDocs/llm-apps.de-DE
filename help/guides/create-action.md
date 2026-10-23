---
title: Erstellen einer neuen Aktion
description: Definieren Sie Aktionsmetadaten, implementieren Sie den Handler, verbinden Sie ein EDS-Widget, testen Sie es und stellen Sie es mit Adobe LLM-Apps bereit.
source-git-commit: 557173e8d95325b980efac4d72c06bb3d90dcd72
workflow-type: tm+mt
source-wordcount: '1476'
ht-degree: 0%
---

# Erstellen einer neuen Aktion {#create-action-from-scratch}

>[!IMPORTANT]
>
>[!DNL Adobe LLM Apps] befindet sich derzeit in Beta.
>
>Die hier gezeigten Funktionen, Workflows und Benutzeroberflächen stellen nicht unbedingt den endgültigen Status des Produkts dar. Um Beta beizutreten, senden Sie eine E-Mail an llm-apps-beta@adobe.com.

>[!NOTE]
>
>In diesem Handbuch wird von einer grundlegenden Vertrautheit mit Adobe Edge Delivery Services (EDS) ausgegangen. Wenn Sie neu bei EDS sind, lesen Sie zunächst das [EDS-Entwickler-Tutorial](https://www.aem.live/developer/tutorial) und [Erkunden von Blöcken](https://www.aem.live/docs/exploring-blocks) um die Grundlagen - Blöcke, die `decorate` und die EDS-Projektstruktur - zu lernen, bevor Sie ein Widget verbinden.

Fügen Sie eine Funktion hinzu, die von der Plattform nicht erstellt wurde. Definieren Sie die Aktion in [!DNL LLM Apps], schreiben Sie den Handler in das verknüpfte Repository und fügen Sie bei Bedarf ein Widget hinzu.

**In diesem Handbuch** Erfahren Sie, wie Sie eine Aktion definieren, ihren Handler implementieren und ein optionales Widget verbinden. Testen Sie die Aktion lokal, stellen Sie die App bereit und versuchen Sie die Aktion in einer unterstützten LLM-Plattform.

Beginnen Sie bei Ihrer ersten Mobile App mit [Einrichten der Infrastruktur](/help/guides/create-app.md) und [Erstellen von Aktionen](/help/guides/creating-actions.md). Im Handbuch Aktionen erstellen werden die automatische Erstellung auf einer Website und die manuelle Erstellung erläutert.

## Voraussetzungen

Sie benötigen:

- Eine vorhandene LLM-App.
- Ein verknüpftes Handler-Repository.
- Ein lokaler Klon des Repositorys mit installierten Abhängigkeiten.
- Ein EDS-Projekt, wenn die Aktion ein Widget anzeigt.
- Eine klare API oder Datenquelle für Produktionsergebnisse.

## Planen der Aktion

Entwerfen Sie jede Aktion für eine klare Benutzeraufgabe. Bevor Sie die Benutzeroberfläche öffnen, definieren Sie Folgendes:

- **Intent** - Was der Benutzer zu erreichen versucht.
- **Beschreibung** - wann die Aktion ausgewählt werden soll.
- **Eingaben** - die vom Benutzer mindestens benötigten Informationen.
- **Result** - der Text und die strukturierten Daten, die vom Handler zurückgegeben werden.
- **Verhalten** - ob die Aktion Daten liest, Daten ändert oder externe Systeme aufruft.
- **Widget** — ob das Ergebnis eine visuelle Schnittstelle benötigt.

Verwenden Sie diesen Plan beispielsweise für eine Aktion **Produkte suchen**:

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

Verwandte, aber unterschiedliche Aufgaben getrennt halten. Verwenden Sie separate Aktionen für die Produktsuche und den Produktkauf, da die Aufgaben unterschiedliche Eingaben, Risiken und Bestätigungsanforderungen aufweisen.

## Erstellen der Aktionsmetadaten

Öffnen Sie die App und wählen Sie **[!UICONTROL Aktionen]** und dann **[!UICONTROL Aktion erstellen]** aus.

Das Erstellungsdialogfeld enthält die Registerkarten **[!UICONTROL Aktion]** und **[!UICONTROL Widget-]**). Erweitern Sie jeden Abschnitt, um seine Felder zu konfigurieren.

### Einfache Informationen eingeben

![Aktion erstellen - Grundlegende Informationen](/help/assets/guide-create-action/action-basic-info.png)

Geben Sie Folgendes ein:

- **[!UICONTROL Aktionsname]** - Ein kurzer Aufgabenname, z. B *„Produkte*.
- **[!UICONTROL Beschreibung]** — Erklären Sie, wann die Aktion verwendet wird und was sie zurückgibt.

Das Dialogfeld leitet eine **[!UICONTROL Code-Kennung]** vom Namen ab, z. B. `search-products`. Die Kennung wird nach der Erstellung korrigiert und muss in der App eindeutig sein. Verwenden Sie die genaue Kennung für den Handler-Ordner.

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

Für Aktionen mit Widgets fügen Sie **[!UICONTROL Widget-Beschreibung]** hinzu. Die Widget-Beschreibung unterscheidet sich von der Aktionsbeschreibung:

- **Aktionsbeschreibung** hilft dem Modell bei der Entscheidung, wann die Aktion aufgerufen werden soll.
- **Widget-Beschreibung** wird `_meta["openai/widgetDescription"]` zugeordnet und fasst zusammen, was die gerenderte Komponente anzeigt, wodurch der wiederholte Narrativ reduziert wird.

[!DNL LLM Apps] wendet die Widget-Beschreibung als Komponentenmetadaten an. Geben Sie die Beschreibung nicht vom Handler zurück.

### Sichtbarkeit konfigurieren

- **[!UICONTROL KI-Modell bereitstellen]** ermöglicht dem Modell die Auswahl der Aktion.
- **[!UICONTROL Als Widget in der Programmoberfläche anzeigen]** zeigt das konfigurierte Widget an.

Wählen Sie für eine Aktion, die nur Text enthält, auch **[!UICONTROL Kein Widget]** unter **[!UICONTROL Widget-Metadaten]** > **[!UICONTROL Typ]**. Bei der Steuerung der Sichtbarkeit wird nicht der Widget-Typ der Aktion ausgewählt.

### Eingabeparameter hinzufügen

Fügen Sie für jeden Wert, den der Handler akzeptiert, einen Parameter hinzu. Jeder Parameter benötigt:

- **Name** - der Schlüssel, den der Handler erhält.
- **type** - Zeichenfolge, Zahl, Ganzzahl oder Boolesch.
- **Beschreibung** - wie der Wert extrahiert wird.
- **Erforderlich** - Angabe, ob für die Aktion der Wert erforderlich ist.

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

### Analytics konfigurieren {#configure-analytics}

Aktivieren Sie **[!UICONTROL Benutzerabsicht erfassen]** wenn Sie möchten, dass Analytics eine Zusammenfassung der Konversation enthält, die zu der Aktion geführt hat.

Wenn Sie Ihre eigene [!DNL Adobe Analytics] oder [!DNL Customer Journey Analytics] einrichten, wird auch die Benutzerabsicht an diese gesendet. Siehe [Einrichten von Adobe Analytics und Customer Journey Analytics](/help/guides/analytics-setup.md).

Die Benutzerabsichtserfassung ist standardmäßig aktiviert. Lesen Sie **[!UICONTROL Beschreibung der Benutzerabsicht]**, die die Anweisungen zum Anfordern der Zusammenfassung enthält. Beschränken Sie die Anweisungen auf einen nicht vertraulichen, anonymisierten Zweck und beschreiben Sie Ihre Datenerfassung in Ihrer Datenschutzerklärung. Die Beschreibung akzeptiert bis zu 2.000 Zeichen.

![Aktion erstellen - Analyse der Benutzerabsicht](/help/assets/guide-create-action/action-analytics-user-intent.png)

Vollständige Felddefinitionen finden Sie unter [Aktion und Widget-Felder](/help/reference/reference-docs.md).

## Konfigurieren des Widgets

Für eine Aktion, die nur Text enthält, öffnen Sie **[!UICONTROL Widget-Metadaten]** und setzen Sie **[!UICONTROL Typ]** auf **[!UICONTROL Kein Widget]**. Überspringen Sie dann die verbleibenden Widget-Einstellungen.

Öffnen Sie **[!UICONTROL Widget-Metadaten]**.

![Aktion erstellen — Widget-Metadaten](/help/assets/guide-create-action/widget-metadata.png)

Konfigurieren:

- **[!UICONTROL Typ]** - Wählen Sie **[!UICONTROL Edge Delivery Services-Block]** für ein interaktives Widget aus.
- **[!UICONTROL Widget-Domain]** - der Sandbox-Ursprung des Widgets. Die Benutzeroberfläche identifiziert sie als erforderlich für die App-Übermittlung und eindeutig pro App. Sie ist nicht identisch mit der EDS-Seiten-URL.
- **Bevorzugter Rahmen** - Fordert einen umrandeten Container im Host an.

Erweitern Sie **[!UICONTROL Vorlagen-URLs]** und geben Sie Folgendes ein:

- **[!UICONTROL Script URL]** - der Einstiegspunkt des EDS-Widgets.
- **[!UICONTROL Widget URL]** - die veröffentlichte EDS-Seite für diese Aktion.

![Neue Aktion erstellen - Skript- und Widget-URLs für ein EDS-Widget](/help/assets/guide-create-action/widget-template-urls.png)

Typische URLs sind:

```text
Script URL:
https://main--<repo>--<owner>.aem.live/scripts/aem-embed.js

Widget URL:
https://main--<repo>--<owner>.aem.live/<widget-page>
```

Gewähren Sie nur erforderliche Browser-Berechtigungen und CSP-Domains.

Erweitern Sie **[!UICONTROL Berechtigungen (MCP-spezifisch)]** nur, wenn das Widget Browser-Funktionen wie Geolokalisierung benötigt. Lassen Sie nicht verwendete Berechtigungen deaktiviert.

![Neue Aktion erstellen — Optionale Browser-Berechtigungen](/help/assets/guide-create-action/widget-permissions.png)

Fügen **[!UICONTROL unter „CSP-]**&quot; die Ursprünge hinzu, die für Assets- und Netzwerkanfragen erforderlich sind. Ein Widget, das externe Ressourcen lädt, wird möglicherweise nicht gerendert, wenn sein CSP unvollständig ist.

![Neue Aktion erstellen - Domain-Listen der Content Security-Richtlinien](/help/assets/guide-create-action/widget-csp.png)

Wenn das EDS-Projekt oder die Widget-Seite noch nicht vorhanden ist, schließen Sie [Eigenes EDS-Projekt &#x200B;](/help/guides/bring-your-own-eds.md)) ab und kehren Sie dann zur Aktion zurück.

## Speichern der Aktion

Wählen Sie **[!UICONTROL Neue Aktion erstellen]** aus. Die Aktion wird auf der Seite Aktionen mit dem Badge **Nicht bereitgestellt** angezeigt.

Beim Speichern werden die Metadaten erstellt, für die Aktion ist jedoch ein Handler erforderlich.

Wenn Sie die gespeicherte Aktion erneut öffnen, verfügt der Editor über **[!UICONTROL LLM-Widget-]** Details **[!UICONTROL , Aktionsdetails]** und **[!UICONTROL Analytics]**. Um spätere Änderungen zu speichern, wählen Sie **[!UICONTROL Änderungen speichern]**. Das Erstellungsdialogfeld und der Editor für gespeicherte Aktionen haben unterschiedliche Layouts.

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

Die LLM-Plattform liest `content` als Textfallback:

```javascript
content: [
  { type: 'text', text: 'Found 3 matching products.' }
]
```

Geben Sie immer nützliche `content` zurück, auch wenn die Aktion über ein Widget verfügt.

### `structuredContent`

Das Widget nutzt `structuredContent` als einfaches Objekt:

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

API-Aufrufe im Server-seitigen Handler behalten. In diesem Beispiel wird eine öffentliche Produkt-API aufgerufen. Konfigurieren Sie den HTTPS-Ursprung der API in einer [App-Variablen](/help/guides/app-variables.md) namens `PRODUCT_API_ORIGIN`. Das Beispiel akzeptiert die oben definierten `query`- und `category`, erfordert mindestens einen Parameter und erfordert keine Anmeldeinformationen.

```javascript
const { getVariable } = require('@adobe/llm-apps-runtime');

module.exports = async ({ query = '', category = '' } = {}, extra) => {
  const normalizedQuery = typeof query === 'string' ? query.trim() : '';
  const normalizedCategory = typeof category === 'string' ? category.trim() : '';
  if (typeof query !== 'string' || typeof category !== 'string'
      || (!normalizedQuery && !normalizedCategory)
      || normalizedQuery.length > 200 || normalizedCategory.length > 100) {
    return {
      isError: true,
      content: [{ type: 'text', text: 'Enter a valid product search.' }],
      structuredContent: { products: [], total: 0 }
    };
  }

  const apiOrigin = getVariable(extra, 'PRODUCT_API_ORIGIN');
  if (!apiOrigin) {
    throw new Error('Product API configuration is unavailable.');
  }

  const origin = new URL(apiOrigin);
  if (origin.protocol !== 'https:') {
    throw new Error('Product API configuration must use HTTPS.');
  }

  const url = new URL('/v1/products', origin);
  if (normalizedQuery) {
    url.searchParams.set('query', normalizedQuery);
  }
  if (normalizedCategory) {
    url.searchParams.set('category', normalizedCategory);
  }

  const response = await fetch(url, {
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

Geben Sie keine API-Anmeldeinformationen in Quell-Code, Aktionsmetadaten, Widget-JavaScript, Protokolle oder benutzerseitige Fehler ein. Die Benutzeroberfläche **[!UICONTROL Variablen und Geheimnisse]** unterstützt derzeit nur nicht vertrauliche Variablen und keine geheimen Daten. Speichern Sie kein API-Token in einer Variablen. Wenn Ihre Upstream-API Anmeldeinformationen erfordert, vereinbaren Sie einen Mechanismus für genehmigte Server-seitige Anmeldeinformationen, bevor Sie sie verbinden.

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

Siehe [Eigenes EDS-Projekt &#x200B;](/help/guides/bring-your-own-eds.md).

## Bereitstellen und Testen

1. Übertragen Sie die Änderungen an Handler und Widget und übertragen Sie sie.
2. [Bereitstellen der App](/help/guides/deploy-your-app.md) für das Staging.
3. Testen Sie die App in [ChatGPT](/help/guides/test-in-chatgpt.md) oder [Claude](/help/guides/test-in-claude.md).
4. Überprüfen Sie die Aktionsauswahl mit Eingabeaufforderungen, die mit der Aktion übereinstimmen, und mit nicht verwandten Eingabeaufforderungen, die nicht dazu aufrufen.
5. Nachdem die Staging-Umgebung erfolgreich ausgeführt wurde, stellen Sie sie in der Produktionsumgebung bereit.

Wenn Metadaten ohne übereinstimmenden Handler vorhanden sind, registriert die Bereitstellung die Aktion mit einem Standard-Stub. Fügen Sie den Handler hinzu, bevor Sie die Aktion den Benutzern zur Verfügung stellen.
