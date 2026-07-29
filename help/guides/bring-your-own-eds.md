---
title: Eigenes Edge Delivery Services-Projekt einbringen
description: Verbinden eines vorhandenen Adobe Edge Delivery Services-Projekts mit einer Adobe LLM Apps-Aktion.
source-git-commit: eec74b87457bc852d7a8dd0e46c2a4385a93ae0a
workflow-type: tm+mt
source-wordcount: '472'
ht-degree: 3%

---


# Eigenes EDS-Projekt mitbringen {#bring-your-own-eds}

>[!IMPORTANT]
>
>[!DNL Adobe LLM Apps] befindet sich derzeit in Beta.
>
>Die hier gezeigten Funktionen, Workflows und Benutzeroberflächen stellen nicht unbedingt den endgültigen Status des Produkts dar. Um Beta beizutreten, senden Sie eine E-Mail an llm-apps-beta@adobe.com.

Verwenden Sie dieses Handbuch, wenn Sie bereits über ein Edge Delivery Services (EDS)-Projekt verfügen oder wenn Sie eine App ohne den Onboarding-Agenten erstellt haben.

Wenn der Onboarding-Agent Ihr Widget erstellt hat, folgen Sie stattdessen [Generiertes Widget anpassen](/help/guides/widgets.md). Das generierte Projekt enthält bereits die hier beschriebenen SDK-Dateien, -Blöcke, -Inhalte und die Aktionskonfiguration.

**Journey:** Bereiten Sie das EDS-Projekt vor → installieren Sie den SDK-→-Build und veröffentlichen Sie den -Block, → konfigurieren Sie die Aktion → Bereitstellen und Testen.

## Bevor Sie beginnen

Sie benötigen:

- Ein EDS-Repository mit [AEM Code Sync](https://github.com/apps/aem-code-sync) installiert.
- Berechtigung zum Hinzufügen von Abhängigkeiten und Erstellen von Blöcken in diesem Repository.
- Berechtigung zum Konfigurieren von Antwort-Headern für die EDS-Site.
- Eine Aktion in [!DNL LLM Apps] mit einem Handler, der `structuredContent` zurückgibt.

## Installieren des LLM Apps SDK

Aus dem EDS-Projektstamm:

```bash
npm install @adobe/llmapps-sdk
```

Das Paket kopiert den Widget-Einstiegspunkt und die Bridge-Implementierung in das Projekt:

```text
scripts/
├── aem-embed.js
└── llmapps-sdk.js
```

Die von der Aktion verwendete Skript-URL verweist auf `scripts/aem-embed.js`.

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
export default async function decorate(block, bridge) {
  if (bridge) {
    bridge.applyHostStyles();
  }

  const result = bridge ? await bridge.toolResult : null;
  const products = result?.structuredContent?.products ?? [];

  const list = document.createElement('ul');
  products.forEach((product) => {
    const item = document.createElement('li');
    item.textContent = String(product.name ?? 'Product');
    list.append(item);
  });

  block.replaceChildren(list);

  if (bridge) {
    bridge.autoResize(block);
  }
}
```

Verwenden Sie DOM-APIs, die Textwerte kodieren. Verketten Sie keine externen Daten in HTML.

## Verfassen und Veröffentlichen der Widget-Seite

Erstellen Sie eine EDS-Seite für das Widget und fügen Sie dieser Seite den Block hinzu. Veröffentlichen Sie die Seite.

Die Live-Seiten-URL wird zur Widget-URL der Aktion:

```text
https://main--<repo>--<owner>.aem.live/<widget-page>
```

Der Seitenpfad muss nicht mit dem Aktionsnamen übereinstimmen, aber eine konsistente Konvention erleichtert die Pflege des Projekts.

## Konfigurieren von CORS

Das Widget lädt die EDS-Seite sowie Skripte, Stile, Blöcke und Medien in allen Ursprüngen. Konfigurieren Sie die Kopfzeile für die EDS-Site:

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

Verwenden Sie den spezifischen Host-Ursprung, der für Ihre unterstützte LLM-Plattform erforderlich ist. Verwenden Sie `*` nur, wenn das Widget absichtlich öffentlich ist, keine Anmeldeinformationen für ursprungsübergreifende Anfragen verwendet und die Sicherheitsanforderungen dies zulassen.

Details zur EDS-Konfiguration finden Sie unter [Konfigurationsdienst](https://aem.live/docs/config-service-setup).

## Konfigurieren der Aktion

Öffnen Sie in [!DNL LLM Apps] die Aktion und wählen Sie **[!UICONTROL Widget-Metadaten]** aus.

Geben Sie Folgendes ein:

- **[!UICONTROL Skript-URL]**

  ```text
  https://main--<repo>--<owner>.aem.live/scripts/aem-embed.js
  ```

- **[!UICONTROL Widget-URL]**

  ```text
  https://main--<repo>--<owner>.aem.live/<widget-page>
  ```

Konfigurieren Sie CSP-Domains und Browser-Berechtigungen mit den geringsten Berechtigungen. Fügen Sie nur Ursprünge und Funktionen hinzu, die für das Widget erforderlich sind.

Felddefinitionen finden Sie unter [Aktion und Widget-Felder](/help/reference/reference-docs.md).

## Testen der Integration

1. Zeigen Sie die EDS-Seite direkt in der Vorschau an und überprüfen Sie ihr Fallback für Beispieldaten.
2. Testen Sie den Handler lokal und vergleichen Sie seine `structuredContent` mit der vom Block erwarteten Form.
3. Stellen Sie die App für das Staging bereit.
4. Rufen Sie die Aktion von [!DNL ChatGPT] aus auf.
5. Überprüfen Sie den Status „Laden“, „Erfolg“, „leer“ und „Fehler“.

Wenn die Seite direkt, aber nicht in der LLM-Plattform funktioniert, überprüfen Sie CORS, CSP, HTTPS-URLs und die `structuredContent`. Siehe [Fehlerbehebung](/help/reference/troubleshooting.md).
