---
title: Anpassen eines generierten EDS-Widgets
description: Machen Sie sich mit dem Edge Delivery Services-Widget vertraut, das vom Adobe LLM Apps Onboarding Agent erstellt wurde, und passen Sie es an.
source-git-commit: 4c259a4587c0a84bb634a9a56c043dfe1cfc31fb
workflow-type: tm+mt
source-wordcount: '650'
ht-degree: 0%

---


# Anpassen eines generierten Widgets {#customize-generated-widget}

>[!IMPORTANT]
>
>[!DNL Adobe LLM Apps] befindet sich derzeit in Beta.
>
>Die hier gezeigten Funktionen, Workflows und Benutzeroberflächen stellen nicht unbedingt den endgültigen Status des Produkts dar. Um Beta beizutreten, senden Sie eine E-Mail an llm-apps-beta@adobe.com.

>[!NOTE]
>
>In diesem Handbuch wird von einer grundlegenden Vertrautheit mit Adobe Edge Delivery Services (EDS) ausgegangen. Wenn Sie neu bei EDS sind, lesen Sie zunächst das [EDS-Entwickler-](https://www.aem.live/developer/tutorial) und [Erkunden von Blöcken](https://www.aem.live/docs/exploring-blocks) um die Grundlagen - Blöcke, die `decorate` und die EDS-Projektstruktur - zu lernen, bevor Sie ein Widget anpassen.

Der Onboarding-Agent erstellt für jede generierte Aktion ein EDS-Widget. Das Widget empfängt bereits das Aktionsergebnis, rendert Beispieldaten, wendet Host-Stile an und ist mit der Aktion in [!DNL LLM Apps] verknüpft.

Testen Sie zunächst das generierte Widget. Passen Sie dann den Datenvertrag, die Interaktion und das visuelle Design an.

**Journey:** Suchen Sie den generierten Block → richten Sie seinen Datenvertrag aus → passen Sie ihn sicher an → zeigen Sie eine lokale Vorschau → Bereitstellung und Tests an.

## Erstelltes Widget suchen

Öffnen Sie das beim Erstellen der App ausgewählte EDS-Repository. Jedes erzeugte Widget ist ein EDS-Block:

```text
blocks/
└── <action-name>/
    ├── <action-name>.js
    └── <action-name>.css
```

- Die JavaScript-Datei liest das Aktionsergebnis und erstellt die Schnittstelle.
- Die CSS-Datei steuert das Layout, das responsive Verhalten und das visuelle Design.
- Die generierte Pull-Anfrage zeigt die genauen Dateien an, die für die Aktion erstellt wurden.

Der Onboarding-Agent konfiguriert auch die Widget-URLs und unterstützende SDK-Dateien. Sie müssen kein zweites EDS-Projekt erstellen oder diese Werte erneut eingeben, um ein generiertes Widget anzupassen.

## Verbinden des LLM Apps SDK mit dem Widget

Das `@adobe/llmapps-sdk`-Paket verbindet das EDS-Widget mit dem LLM-Host. Das generierte EDS-Repository umfasst:

```text
scripts/
├── aem-embed.js
└── llmapps-sdk.js
```

`aem-embed.js` stellt die Host-Verbindung her, lädt die EDS-Seite und ruft Ihren -Block auf:

```javascript
export default async function decorate(block, bridge) {
  // Customize the widget here.
}
```

SDK wird nicht in den Baustein importiert. Die verbundene `bridge` wird automatisch bereitgestellt. Dadurch kann das Widget:

- Lesen Sie das Handler-Ergebnis mit `bridge.toolResult`.
- Anwenden von Host-Stilen mit `bridge.applyHostStyles()`.
- Setzen Sie das Gespräch mit `bridge.sendMessage()` fort.
- Rufen Sie eine weitere Aktion mit `bridge.callTool()` auf.
- Lassen Sie die Größe mit der `bridge.autoResize()` synchronisiert.

In diesem Handbuch werden die gängigen Bridge-Methoden behandelt. Die vollständige API finden Sie [&#128279;](https://www.npmjs.com/package/@adobe/llmapps-sdk) dem `@adobe/llmapps-sdk`-Paket .

## Grundlagen zum Datenvertrag

Der Aktions-Handler gibt `structuredContent` zurück, und der Block liest es aus `bridge.toolResult`.

```javascript
// Handler result
return {
  content: [{ type: 'text', text: `Found ${products.length} products.` }],
  structuredContent: { products, total: products.length }
};
```

```javascript
// EDS block
export default async function decorate(block, bridge) {
  const result = bridge ? await bridge.toolResult : null;
  const products = result?.structuredContent?.products ?? [];
  // Render products.
}
```

Wenn Sie `structuredContent` ändern, aktualisieren Sie den Handler und das Widget zusammen. Siehe [Anpassen eines generierten Handlers](/help/guides/customize-handler.md) für den vollständigen Rückgabevertrag.

## Sicheres Rendern externer Daten

Handler-Ausgabe als nicht vertrauenswürdige Daten behandeln. BEVORZUGEN Sie DOM-APIs wie `textContent`, anstatt Antwortwerte in `innerHTML` einzufügen.

```javascript
function createProductCard(product, bridge) {
  const card = document.createElement('article');
  card.className = 'product-card';

  const title = document.createElement('h3');
  title.textContent = String(product.name ?? 'Product');

  const button = document.createElement('button');
  button.type = 'button';
  button.textContent = 'Tell me more';
  button.addEventListener('click', () => {
    if (bridge && product.id) {
      bridge.sendMessage(`Show me details for product ${String(product.id)}`);
    }
  });

  card.append(title, button);
  return card;
}
```

Validieren Sie URLs, bevor Sie sie `href` oder `src` zuweisen, und lassen Sie nur die für das Erlebnis erforderlichen Protokolle zu.

## Verwenden der Host-Brücke

EDS übergibt eine verbundene Brücke an `decorate(block, bridge)`. Guard Bridge ruft auf, damit der Block auch während der direkten EDS-Vorschau gerendert wird.

### Anwenden von Host-Stilen

```javascript
if (bridge) {
  bridge.applyHostStyles();
}
```

Dies gilt für Host-Typografie und Design-Variablen. Ihr Widget-CSS sollte sowohl helle als auch dunkle Host-Designs unterstützen.

### Folgenachricht senden

```javascript
await bridge.sendMessage('Show me similar products.');
```

Verwenden Sie `sendMessage`, wenn eine Interaktion die Konversation fortsetzen soll.

### Andere Aktion aufrufen

```javascript
const result = await bridge.callTool('get-product-details', {
  id: product.id
});
```

Verwenden Sie `callTool` für eine explizite Interaktion, die ein anderes Aktionsergebnis erfordert. Übergeben Sie nur validierte Werte und behandeln Sie Fehler, ohne interne Details anzuzeigen.

### Widget-Größe synchronisieren

```javascript
if (bridge) {
  bridge.autoResize(block);
}
```

Rufen Sie `autoResize` nach dem ersten Rendern auf, damit der Host auf Inhaltsänderungen reagieren kann.

## Vorschau der Änderungen

Generierte Blöcke sollten Beispieldaten für die direkte Vorschau enthalten, wenn `bridge` nicht verfügbar ist.

So zeigen Sie eine lokale Vorschau des EDS-Projekts an:

```bash
npm install -g @adobe/aem-cli
aem up
```

Öffnen Sie die generierte Widget-Seite unter `http://localhost:3000`. Überprüfen Sie:

- Status „Leer“, „Laden“, „Erfolg“ und „Fehler“.
- Langer Text und fehlende optionale Felder.
- Tastaturnavigation und sichtbarer Fokus.
- Helle und dunkle Themen.
- Enge und breite Layouts.

Stellen Sie dann die App für das Staging bereit und testen Sie sie mit Live `structuredContent` in der LLM-Plattform.

## Veröffentlichen der Anpassung

1. Übergeben Sie die EDS-Änderungen und übertragen Sie sie.
2. Wenn Sie die Datenform geändert haben, übertragen Sie die entsprechenden Handler-Änderungen und übertragen Sie sie.
3. Stellen Sie die App für das Staging bereit.
4. Testen Sie die Aktion und das Widget in [!DNL ChatGPT].
5. Die verifizierte Version zur Produktion weiterleiten.

## Andere EDS-Setups

Wenn Sie den Onboarding-Agenten nicht verwendet haben oder eine vorhandene EDS-Site integrieren möchten, lesen Sie [Eigenes EDS-Projekt &#x200B;](/help/guides/bring-your-own-eds.md).
