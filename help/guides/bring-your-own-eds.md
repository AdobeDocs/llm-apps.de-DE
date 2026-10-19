---
title: Eigenes Edge Delivery Services-Projekt einbringen
description: Verbinden eines vorhandenen Adobe Edge Delivery Services-Projekts mit einer Adobe LLM Apps-Aktion.
hidefromtoc: true
source-git-commit: 0f9dd0a5b1008e183eb8ab5a4c2eae34a78923bb
workflow-type: tm+mt
source-wordcount: '866'
ht-degree: 1%
---

# Eigenes EDS-Projekt mitbringen {#bring-your-own-eds}

>[!IMPORTANT]
>
>[!DNL Adobe LLM Apps] befindet sich derzeit in Beta.
>
>Die hier gezeigten Funktionen, Workflows und Benutzeroberflächen stellen nicht unbedingt den endgültigen Status des Produkts dar. Um Beta beizutreten, senden Sie eine E-Mail an llm-apps-beta@adobe.com.

Verwenden Sie dieses Handbuch, um ein kompatibles vorhandenes Edge Delivery Services-Projekt (EDS) zu verbinden oder ein Widget zu einer selbst erstellten Aktion hinzuzufügen. Ein vorhandenes Projekt ist optional. Wenn Sie Ihre eigene Infrastruktur einrichten, beginnt der Standardpfad mit zwei leeren Repositorys, die von der Plattform beim Einrichten der [&#x200B; initialisiert &#x200B;](/help/guides/create-app.md).

Wenn die Plattform Ihr Widget automatisch erstellt hat, folgen Sie stattdessen [Generiertes Widget anpassen](/help/guides/widgets.md). Das generierte Projekt enthält bereits die hier beschriebenen SDK-Dateien, -Blöcke, -Inhalte und die Aktionskonfiguration.

**In diesem Handbuch:** Erfahren Sie, wie Sie Ihr EDS-Projekt vorbereiten, die SDK installieren und ein Widget erstellen und veröffentlichen. Verbinden Sie dann das Widget mit einer Aktion und testen Sie sie in der bereitgestellten App.

## Voraussetzungen

Sie benötigen:

- Ein mit AEM-Textbausteinen kompatibles EDS-Repository mit [installierter AEM](https://github.com/apps/aem-code-sync)Code-Synchronisierung und einer konfigurierten Inhaltsquelle.
- Berechtigung zum Hinzufügen von Abhängigkeiten und Erstellen von Blöcken in diesem Repository.
- Berechtigung zum Konfigurieren von Antwort-Headern für die EDS-Site.
- Eine Aktion in [!DNL LLM Apps] mit einem Handler, der `structuredContent` zurückgibt.

Wenn Sie ein vorhandenes Repository verknüpfen, sucht die Plattform nach `scripts/aem.js`, `blocks/`, `styles/styles.css` und `head.html`. Die -Einbettungskomponente importiert auch `scripts/scripts.js` und ruft die exportierte `decorateMain`-Funktion auf, um -Blöcke vorzubereiten. Den EDS-Blockladevertrag intakt halten. Die erforderlichen Dateien allein bestätigen nicht, dass ein stark angepasstes Projekt korrekt gerendert wird.

## Installieren des LLM Apps SDK

Aus dem EDS-Projektstamm:

```bash
npm install @adobe/llmapps-sdk
```

Der PostInstall-Hook des Pakets kopiert den Widget-Einstiegspunkt, die Bridge-Implementierung und unterstützende Adobe-Bibliotheken in das Projekt:

```text
scripts/
├── aem-embed.js
├── llmapps-sdk.js
├── alloy-base.min.js
└── alloy-standalone.min.js
```

Die Skript-URL der Aktion verweist auf `scripts/aem-embed.js`, nicht auf `scripts/llmapps-sdk.js`. Stellen Sie sicher, dass die Installation die SDK-Dateien kopiert, sie dann übergeben und mit dem Projekt übertragen hat. Der Hook überschreibt die kopierten Dateien, wenn Sie die SDK installieren oder aktualisieren. Passen Sie die kopierten SDK-Dateien nicht an.

## Erstellen des Widget-Blocks

Erstellen Sie einen Block für die Aktion:

```text
blocks/
└── search-products/
    ├── search-products.js
    └── search-products.css
```

Exportieren Sie die standardmäßige EDS-`decorate` mit der verbundenen Brücke als zweites Argument:

```javascript
const PREVIEW_RESULT = {
  records: [{ id: 'P-100', name: 'Frescopa House Blend' }]
};

export default async function decorate(block, bridge) {
  if (bridge?.isConnected) {
    bridge.applyHostStyles();
  }

  const live = bridge?.isConnected && bridge.hostContext?.preview !== true;
  const result = live ? await bridge.toolResult : null;
  const data = live ? result?.structuredContent : PREVIEW_RESULT;
  const records = Array.isArray(data?.records) ? data.records : [];

  if (result?.isError) {
    const message = document.createElement('p');
    message.textContent = 'Product search is unavailable. Try again later.';
    block.replaceChildren(message);
    bridge.autoResize(block);
    return;
  }

  const list = document.createElement('ul');
  records.forEach((product) => {
    const item = document.createElement('li');
    item.textContent = String(product?.name ?? 'Product');
    list.append(item);
  });

  if (records.length === 0) {
    const item = document.createElement('li');
    item.textContent = 'No matching products were found.';
    list.append(item);
  }
  block.replaceChildren(list);

  if (bridge?.isConnected) {
    bridge.autoResize(block);
  }
}
```

Verwenden Sie DOM-APIs, die Textwerte kodieren. Verketten Sie keine externen Daten in HTML.

Das Beispiel erwartet `structuredContent.records`. Passen Sie die Eigenschaftsnamen und Datensatzfelder an, die von Ihrem Handler zurückgegeben werden. Überprüfen Sie `bridge.isConnected`, nicht nur, ob eine Brücke vorhanden ist: `toolResult` wird im eigenständigen Modus nie aufgelöst. Verwenden Sie in der LLM-App-Vorschau die Beispieldaten des Blocks, wenn `bridge.hostContext.preview` den Vorschaumodus anzeigt.

## Verfassen und Veröffentlichen der Widget-Seite

Erstellen Sie eine EDS-Seite für das Widget und fügen Sie dieser Seite den Block hinzu. Veröffentlichen Sie die Seite.

Die Seiten-URL wird zur Widget-URL der Aktion. Beispielsweise verwendet die standardmäßig generierte Konfiguration den EDS-Vorschauursprung:

```text
https://main--<repo>--<owner>.aem.page/eds-widgets/search-products-demo
```

Ein EDS-Live-Ursprung wie `https://main--<repo>--<owner>.aem.live` funktioniert auch, wenn die Seite und die Assets dort veröffentlicht werden. Verwenden Sie den tatsächlichen Site-Ursprung. Konfigurierte EDS-Websites können Site- und Organisationsnamen enthalten, die sich vom GitHub-Repository und -Eigentümer unterscheiden.

Vergewissern Sie sich, dass beim Anhängen von `.plain.html` an die Widget-URL das Seitenfragment zurückgegeben wird und dass die Seite den `search-products` enthält. Der Seitenpfad muss nicht mit der Aktionskennung übereinstimmen, aber eine konsistente Konvention erleichtert die Pflege des Projekts.

## Konfigurieren von CORS

Das Widget lädt die EDS-Seite, Skripte, Stile, Blöcke und Medien über verschiedene Ursprünge hinweg. Konfigurieren Sie die Kopfzeile für die EDS-Site:

```json
{
  "/**": [
    {
      "key": "access-control-allow-origin",
      "value": "<allowed-host-origin>"
    }
  ]
}
```

Verwenden Sie den tatsächlichen Widget-Sandbox-Ursprung, der von der unterstützten LLM-Plattform benötigt wird, nicht nur den Ursprung der Chat-Website. Überprüfen Sie CORS für die `.plain.html` Antwort und für importierte JavaScript, Stile, Schriftarten und Medien.

Das automatische EDS-Setup der Plattform konfiguriert `access-control-allow-origin: *` für die Bereitstellung öffentlicher Widgets und kann einen engeren vorhandenen Wert ersetzen. Verwenden Sie diese Richtlinie nur für absichtlich öffentliche Ressourcen ohne Anmeldeinformationen für ursprungsübergreifende Anfragen und wenn Ihre Sicherheitsanforderungen dies zulassen. Wenn Sie eine eingeschränkte vorhandene Site verwalten, überprüfen Sie deren Richtlinie, bevor Sie sie verbinden. Legen Sie keinen Inhalt privater Site offen, damit ein Widget funktioniert.

Details zur EDS-Konfiguration finden Sie unter [Konfigurationsdienst](https://aem.live/docs/config-service-setup).

## Konfigurieren der Aktion

Öffnen Sie [!DNL LLM Apps] die Aktion und wählen Sie &quot;**[!UICONTROL Widget-Details“]**. Legen **[!UICONTROL unter „Widget-]**&quot; **[!UICONTROL Widget-Typ]** auf **[!UICONTROL EDS]** fest und erweitern Sie dann **[!UICONTROL Vorlagen-URLs]**.

Geben Sie Folgendes ein:

- **[!UICONTROL Skript-URL]**

  ```text
  https://main--<repo>--<owner>.aem.page/scripts/aem-embed.js
  ```

- **[!UICONTROL Widget-URL]**

  ```text
  https://main--<repo>--<owner>.aem.page/eds-widgets/search-products-demo
  ```

Ersetzen Sie die Beispiele durch dieselbe Ursprungs- und Ergebnisseite der Arbeitssite, die Sie oben überprüft haben. Konfigurieren Sie CSP-Domains und Browser-Berechtigungen mit den geringsten Berechtigungen. Der EDS-Ursprung muss für beide Verbindungen zulässig sein, z. B. für den Abruf von Seitenfragmenten, und für Ressourcen wie Blockskripte und -stile. Fügen Sie andere Ursprünge nur hinzu, wenn dies vom Widget benötigt wird.

Wählen **[!UICONTROL Änderungen speichern]** und dann für die Umgebung, die Sie testen, erneut bereitstellen. Die gespeicherte Widget-Konfiguration aktualisiert den laufenden MCP-Server erst bei der Bereitstellung.

Felddefinitionen finden Sie unter [Aktion und Widget-Felder](/help/reference/reference-docs.md).

## Testen der Integration

1. Zeigen Sie die EDS-Seite direkt in der Vorschau an und überprüfen Sie ihr Fallback für Beispieldaten.
2. Testen Sie den Handler lokal und vergleichen Sie seine `structuredContent` mit der vom Block erwarteten Form.
3. Stellen Sie den Handler und die Aktionsmetadaten für das Staging bereit. Die Veröffentlichung von EDS-Code und -Inhalten ist ein separater Schritt.
4. Rufen Sie die Aktion von einer unterstützten LLM-Plattform aus auf. Siehe [Test in ChatGPT](/help/guides/test-in-chatgpt.md) oder [Test in Claude](/help/guides/test-in-claude.md).
5. Überprüfen Sie den Status „Laden“, „Erfolg“, „leer“ und „Fehler“.

Wenn die Seite direkt, aber nicht in der LLM-Plattform funktioniert, überprüfen Sie CORS, CSP, HTTPS-URLs und die `structuredContent`. Siehe [Fehlerbehebung](/help/reference/troubleshooting.md).
