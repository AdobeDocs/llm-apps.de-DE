---
title: Anpassen eines generierten EDS-Widgets
description: Das automatisch von Adobe LLM-Apps erstellte Edge Delivery Services-Widget verstehen und anpassen.
source-git-commit: 0f9dd0a5b1008e183eb8ab5a4c2eae34a78923bb
workflow-type: tm+mt
source-wordcount: '1065'
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
>In diesem Handbuch wird von einer grundlegenden Vertrautheit mit Adobe Edge Delivery Services (EDS) ausgegangen. Wenn Sie mit EDS noch nicht vertraut sind, lesen Sie das [EDS-Entwickler-Tutorial](https://www.aem.live/developer/tutorial) und [Erkunden von Bausteinen](https://www.aem.live/docs/exploring-blocks) bevor Sie ein Widget anpassen. Diese Ressourcen decken Blöcke, die `decorate` und die EDS-Projektstruktur ab.

Nach erfolgreicher Generierung bietet die Plattform ein EDS-Widget, das mit der Aktion in [!DNL LLM Apps] verknüpft ist. Das Widget rendert repräsentative Daten für die Vorschau und liest das Handler-Ergebnis, wenn es mit einer unterstützten LLM-Plattform verbunden ist. Überprüfen Sie das generierte Design und Verhalten, bevor Sie das Widget anpassen.

Testen Sie zunächst das generierte Widget. Passen Sie dann den Datenvertrag, die Interaktion und das visuelle Design an.

**In diesem Handbuch:** Erfahren Sie, wie Sie das generierte Widget finden und an die Daten Ihrer Aktion und das Design Ihrer Marke anpassen. Zeigen Sie dann Ihre Änderungen lokal in der Vorschau an und testen Sie sie in der bereitgestellten App.

## Erstelltes Widget suchen

Öffnen Sie das beim Einrichten der [ angeschlossene EDS-Repository](/help/guides/create-app.md). Wenn Sie Adobe Managed Infrastructure verwenden, fordern Sie den Code-Zugriff an, bevor Sie fortfahren. Jedes erzeugte Widget ist ein EDS-Block:

```text
blocks/
└── <action-slug>/
    ├── <action-slug>.js
    └── <action-slug>.css
```

- Die JavaScript-Datei liest das Aktionsergebnis und erstellt die Schnittstelle.
- Die CSS-Datei steuert das Layout, das responsive Verhalten und das visuelle Design.
- Die generierte Pull-Anfrage zeigt die genauen Dateien an, die für die Aktion erstellt wurden.

Der Slug verwendet Bindestriche: eine Aktion mit Code-Kennung, `search_products` einen Block wie `blocks/search-products/` verwendet. Die generierte Widget-Seite ist in der Regel `/eds-widgets/search-products-demo`. Verwenden Sie die eigentliche **[!UICONTROL Widget-URL]** in den Aktionsmetadaten.

Auf der Plattform werden auch die Widget-URLs und unterstützende SDK-Dateien konfiguriert. Sie müssen kein zweites EDS-Projekt erstellen oder die Widget-URLs erneut eingeben, um ein generiertes Widget anzupassen.

## Verbinden des LLM Apps SDK mit dem Widget

Das `@adobe/llmapps-sdk`-Paket verbindet das EDS-Widget mit dem LLM-Host. Seine Einstiegspunkte im generierten EDS-Repository sind:

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

Die Einbettungskomponente ruft den `.plain.html` Inhalt der Widget-Seite ab und importiert ihre Blöcke. SDK wird nicht in den Baustein importiert. Die Einbettung stellt automatisch `bridge` bereit. Beim normalen EDS-Seitenladen wird dies nicht bereitgestellt. Die Brücke ermöglicht dem Widget:

- Lesen Sie das Handler-Ergebnis mit `bridge.toolResult`.
- Anwenden von Host-Stilen mit `bridge.applyHostStyles()`.
- Setzen Sie das Gespräch mit `bridge.sendMessage()` fort.
- Rufen Sie eine weitere Aktion mit `bridge.callTool()` auf.
- Lassen Sie die Größe mit der `bridge.autoResize()` synchronisiert.

In diesem Handbuch werden die gängigen Bridge-Methoden behandelt. Die vollständige API finden Sie ](https://www.npmjs.com/package/@adobe/llmapps-sdk) dem [`@adobe/llmapps-sdk`-Paket .

## Grundlagen zum Datenvertrag

Der Aktions-Handler gibt `structuredContent` zurück, und der Block liest es aus `bridge.toolResult`.

```javascript
// Handler result
return {
  content: [{ type: 'text', text: `Found ${records.length} products.` }],
  structuredContent: { arguments: { query: 'coffee' }, records }
};
```

```javascript
// EDS block
const PREVIEW_RESULT = {
  records: [{ id: 'P-100', name: 'Frescopa House Blend' }]
};

export default async function decorate(block, bridge) {
  const live = bridge?.isConnected && bridge.hostContext?.preview !== true;
  const result = live ? await bridge.toolResult : null;
  const data = live ? result?.structuredContent : PREVIEW_RESULT;
  const records = Array.isArray(data?.records) ? data.records : [];
  // Render records or an error state when result?.isError is true.
}
```

In diesem Beispiel ist `PREVIEW_RESULT` das lokale Beispieldatenobjekt des Blocks. Aktuell generierte Widgets verwenden `structuredContent.arguments` und `structuredContent.records`. Überprüfen Sie die generierten Dateien auf die exakte Form. Ersetzen Sie niemals ein leeres Live-Ergebnis durch Vorschaudatensätze.

`bridge.toolResult` wird nur mit dem ersten Ergebnis aufgelöst. Verwenden Sie `bridge.onToolResult(callback)`, wenn ein Widget eingebunden bleibt und spätere Ergebnisse verarbeiten muss.

Wenn Sie `structuredContent` ändern, aktualisieren Sie den Handler und das Widget zusammen. Gehen Sie nicht davon aus, dass strukturierte Daten im Modell ausgeblendet sind; dies ist Teil des Aktionsergebnisses des Hosts. Siehe [Anpassen eines generierten Handlers](/help/guides/customize-handler.md) für den vollständigen Rückgabevertrag.

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
  button.disabled = !bridge?.isConnected;
  const status = document.createElement('p');
  status.setAttribute('role', 'status');
  button.addEventListener('click', async () => {
    if (bridge?.isConnected && typeof product.id === 'string') {
      try {
        await bridge.sendMessage(`Show me details for product ${product.id}`);
      } catch {
        status.textContent = 'Unable to send the request. Try again.';
      }
    }
  });

  card.append(title, button, status);
  return card;
}
```

Validieren Sie URLs, bevor Sie sie `href` oder `src` zuweisen, und lassen Sie nur die für das Erlebnis erforderlichen Protokolle zu.

## Verwenden der Host-Brücke

Die Einbettung übergibt eine Brücke an `decorate(block, bridge)`. Schützen Sie interaktive Aufrufe mit `bridge?.isConnected`, damit der Block auch während der direkten EDS-Vorschau gerendert wird. Ein Bridge-Objekt kann im Standalone-Modus vorhanden sein, ohne verbunden zu werden. Das Warten auf `toolResult` im Standalone-Modus wird nie aufgelöst. Die LLM-App-Vorschau legt auch `bridge.hostContext.preview` fest, damit der Block Beispieldaten auswählen kann.

### Anwenden von Host-Stilen

```javascript
if (bridge?.isConnected) {
  bridge.applyHostStyles();
}
```

Die Methode wendet verfügbare Host-CSS-Variablen und Schriftstile an. Das Anwenden von Host-Stilen ist optional: generierte Widgets können stattdessen ihre eigenen Design-Token verwenden. Die -Einbettung behält die `.light`- und `.dark`-Klassen auf ihrem Schattenkörper bei. Verwenden Sie `bridge.hostContext.theme` und `bridge.onContextChange(callback)`, wenn Ihr Block auch auf Design-Änderungen reagieren muss.

### Folgenachricht senden

```javascript
if (bridge?.isConnected) {
  await bridge.sendMessage('Show me similar products.');
}
```

Verwenden Sie `sendMessage` , um die Konversation nach einer Interaktion fortzusetzen.

### Aktion direkt aufrufen

```javascript
if (bridge?.isConnected) {
  const result = await bridge.callTool('search_products', { query: 'coffee' });
  // Re-render this widget from result.structuredContent, or show result.isError.
}
```

Verwenden Sie `callTool` für eine explizite Interaktion, für die ein Aktionsergebnis erforderlich ist. Übergeben Sie die Code-Kennung der Zielaktion, nicht den Blockordner- oder Anzeigenamen. `search_products` Beispiel. Der Host muss Aktionsaufrufe unterstützen und die Zielaktion muss App-Sichtbarkeit ermöglichen. Das Aufrufen einer Aktion rendert das aktuelle Widget nicht automatisch neu. Übergeben Sie nur validierte Werte und verarbeiten Sie abgelehnte Aufrufe, ohne interne Details anzuzeigen.

Wenn sich die Sichtbarkeit der Zielaktion ändern muss, bearbeiten Sie nur diese Aktion unter **[!UICONTROL Aktionsdetails]** speichern Sie die Änderung und stellen Sie sie in der zu testenden Umgebung erneut bereit.

### Widget-Größe synchronisieren

```javascript
if (bridge?.isConnected) {
  bridge.autoResize(block);
}
```

Damit der Host auf Inhaltsänderungen reagieren kann, rufen Sie `autoResize` nach dem ersten Rendering auf. Die Methode gibt eine Bereinigungsfunktion zurück, die die Beobachtung beendet, wenn das Widget entfernt wird.

## Vorschau der Änderungen

Generierte Blöcke enthalten Beispieldaten für die direkte Vorschau, wenn die Brücke fehlt, getrennt ist oder als Vorschau markiert ist. Lassen Sie das Fallback der Beispieldaten von Live- und Fehlerstatus getrennt.

So zeigen Sie eine lokale Vorschau des EDS-Projekts an:

```bash
npm install -g @adobe/aem-cli
aem up
```

Öffnen Sie den Pfad über die Widget-URL der Aktion auf dem lokalen Server, z. B. `http://localhost:3000/eds-widgets/search-products-demo`, anstatt nur den Website-Stamm. Überprüfen Sie:

- Status „Leer“, „Laden“, „Erfolg“ und „Fehler“.
- Langer Text und fehlende optionale Felder.
- Tastaturnavigation und sichtbarer Fokus.
- Helle und dunkle Themen.
- Enge und breite Layouts.

Die direkte EDS-Vorschau testet nicht die Host-Handshake- oder Live-Aktionsdaten. Veröffentlichen Sie die EDS-Änderungen und testen Sie sie mit Live `structuredContent` in der unterstützten LLM-Plattform.

## Veröffentlichen der Anpassung

1. Übergeben Sie die EDS-Änderungen und übertragen Sie sie.
2. Überprüfen Sie, ob die Widget-Seite und die geänderten Skripte und Stile unter den konfigurierten EDS-URLs verfügbar sind. Veröffentlichen Sie Inhaltsänderungen über Ihren EDS-Veröffentlichungs-Workflow.
3. Wenn Sie die Handler- oder Aktionsmetadaten geändert haben, übertragen Sie diese Änderungen und stellen Sie die App im Staging bereit.
4. Testen Sie die Aktion und das Widget in einer unterstützten LLM-Plattform. Siehe [Test in ChatGPT](/help/guides/test-in-chatgpt.md) oder [Test in Claude](/help/guides/test-in-claude.md).
5. Stellen Sie nach erfolgreichem Test nach Bedarf Handler- und Metadatenänderungen für die Produktion bereit.

Die EDS-Veröffentlichung ist von der App-Bereitstellung getrennt. Wenn die Staging- und die Produktionsumgebung dieselben EDS-URLs verwenden, wirken sich Widget-Änderungen an diesen URLs auf beide Umgebungen aus. Bei der Bereitstellung im Staging wird keine isolierte Widget-Kopie erstellt.

## Andere EDS-Setups

Wenn Sie Aktionen selbst erstellt haben oder eine kompatible vorhandene EDS-Site integrieren möchten, lesen Sie [Eigenes EDS-Projekt ](/help/guides/bring-your-own-eds.md).
