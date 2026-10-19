---
title: Verkabeln einer App
description: Machen Sie sich mit den drei Teilen einer LLM-App vertraut, wie sie in einer Konversation funktionieren und wo Änderungen vorgenommen werden können.
source-git-commit: af83670aebde1e6f73585c89a891bc7f5e92fb3a
workflow-type: tm+mt
source-wordcount: '573'
ht-degree: 0%
---

# Verkabeln einer App {#app-architecture}

>[!IMPORTANT]
>
>[!DNL Adobe LLM Apps] befindet sich derzeit in Beta.
>
>Die hier gezeigten Funktionen, Workflows und Benutzeroberflächen stellen nicht unbedingt den endgültigen Status des Produkts dar. Um Beta beizutreten, senden Sie eine E-Mail an llm-apps-beta@adobe.com.

Eine **LLM** App bringt die Services Ihrer Marke in ein KI-Gespräch. Ein Kunde fragt beispielsweise nach Kaffeeempfehlungen, Ihre App findet passende Produkte und der Assistent zeigt die Ergebnisse an.

**In diesem Handbuch** Erfahren Sie, was die einzelnen Teile Ihrer App tun, wo Sie Änderungen vornehmen und wie diese Teile zusammenarbeiten, wenn jemand die App verwendet.

## Die drei Teile einer App

Eine App kann mehrere verwandte Aktionen gruppieren, z. B *„Produkte durchsuchen*, *Produkte vergleichen* und *Store suchen*. Jede Aktion verfügt über eigene Einstellungen, einen eigenen Handler und ein optionales Widget. Das Hinzufügen einer Aktion erfordert kein weiteres Paar von Repositorys.

Eine App mit interaktiven Widgets besteht aus drei Teilen: Aktionseinstellungen, Handler-Code und Widget-Code. Die Einstellungen werden in [!DNL LLM Apps] verwaltet. Der Handler- und Widget-Code ist in zwei separaten [!DNL GitHub]-Repositorys verfügbar.

```text
┌───────────────────────┐   ┌───────────────────────┐   ┌───────────────────────┐
│ 1. LLM Apps UI        │   │ 2. Handler repository │   │ 3. Widget repository  │
│                       │   │                       │   │                       │
│ Action descriptions   │   │ Connects to your APIs │   │ Widget layout         │
│ Required information  │   │ Runs the task         │   │ Branding and styles   │
│ When to use an action │   │ Returns results       │   │ Interactive controls  │
└───────────────────────┘   └───────────────────────┘   └───────────────────────┘
            │                           │                           │
            └───────────────────────────┼───────────────────────────┘
                                        ▼
                    ┌───────────────────────────────────────┐
                    │ Your app in an AI conversation        │
                    │ The action runs and the widget        │
                    │ shows the result.                     │
                    └───────────────────────────────────────┘
```

- **Aktionseinstellungen beschreiben die Aufgabe.** Sie teilen dem Assistenten mit, wann er eine Aktion verwenden soll und welche Informationen er benötigt. Bei der Produktsuche können diese Informationen eine Kategorie oder ein Suchbegriff sein.
- **Der Handler erledigt die Arbeit.** Es ruft Daten von Ihren Systemen ab oder führt eine Aufgabe aus. Ein Produktsuch-Handler ruft beispielsweise passende Produkte aus Ihrem Katalog ab.
- **Das Widget zeigt das Ergebnis an.** Es steuert, was der Kunde sieht und mit dem er interagiert, z. B. Produktkarten mit Namen, Preisen und Schaltflächen.

Es wird empfohlen, jeder Aktion ein Widget hinzuzufügen. Mit einem Widget können Sie steuern, wie Ergebnisse aussehen und wie Kunden mit ihnen interagieren, anstatt die Präsentation dem KI-Assistenten zu überlassen. Reine Textantworten werden ebenfalls unterstützt.

## Wo Änderungen vorgenommen werden können

| Was Sie ändern möchten | Wo bearbeitet werden soll |
|---|---|
| Wenn der Assistent eine Aktion verwendet oder nach welchen Informationen die Aktion fragt | [Aktionseinstellungen in LLM Apps](/help/reference/reference-docs.md) |
| Woher die Ergebnisse kommen oder was passiert, wenn eine Aktion ausgeführt wird | [Handler-Code im Handler-Repository](/help/guides/customize-handler.md) |
| Darstellung der Ergebnisse, einschließlich Layout, Branding und Interaktionen | [Widget-Code im Widget-Repository](/help/guides/widgets.md) |

Halten Sie die drei Teile gleich. Wenn Sie beispielsweise die von einem Handler zurückgegebenen Informationen ändern, aktualisieren Sie das Widget, um das neue Ergebnis anzuzeigen.

## Was passiert in einem Gespräch?

Nach der Bereitstellung verfügt Ihre App über eine Verbindungs-URL namens **MCP Server URL**. Fügen Sie diese URL zu einer unterstützten KI-Plattform hinzu, damit der Assistent die Aktionen Ihrer App ermitteln und verwenden kann.

Für eine App mit einer Aktion und einem Widget für die Produktsuche:

1. Ein Kunde fragt den Assistenten: *Show me Coffee Pods.*
2. Der Assistent verwendet die Beschreibung der Aktion, um die Produktsuchaktion auszuwählen.
3. Der Handler ruft entsprechende Produkte aus dem Katalog ab.
4. Das Widget zeigt die Produkte als Karten im Gespräch an.
5. Der Kunde untersucht die Ergebnisse oder stellt eine Folgefrage.

Der Kunde bleibt im Gespräch, während die App die Informationen oder den Service bereitstellt. Die Widget-Unterstützung hängt von der LLM-Plattform ab.

## Anwenden und Testen von Änderungen

Veröffentlichen Sie Widget-Änderungen und stellen Sie Aktionsänderungen bereit, bevor Sie das aktualisierte Erlebnis testen. Befolgen Sie [App bereitstellen](/help/guides/deploy-your-app.md), um die App in einer unterstützten LLM-Plattform für das Staging bereitzustellen und zu testen.

Automatisch generierte Aktionen beginnen mit Beispieldaten. Verbinden Sie die Handler mit Ihren echten Systemen und testen Sie die App, bevor Sie sie für Kunden verfügbar machen.
