---
title: Erstellen einer App
description: Erfahren Sie, wie Sie Ihre erste LLM-App erstellen und mit Ihrem GitHub-Repository verknüpfen.
source-git-commit: 344c5457eb79a19b1dae823732a1cd9866dcd9dc
workflow-type: tm+mt
source-wordcount: '720'
ht-degree: 1%

---


# Erstellen einer App

>[!IMPORTANT]
>
>[!DNL Adobe LLM Apps] befindet sich derzeit in Beta.
>
>Die hier gezeigten Funktionen, Workflows und Benutzeroberflächen stellen nicht unbedingt den endgültigen Status des Produkts dar. Um Beta beizutreten, senden Sie eine E-Mail an llm-apps-beta@adobe.com.

>[!NOTE]
>
>Bevor Sie beginnen, stellen Sie sicher, dass [Voraussetzungen](/help/overview/overview.md#prerequisites) erfüllt sind.

Dieses Handbuch führt Sie durch die Erstellung Ihrer ersten [!DNL Adobe LLM Apps] - vom leeren Status bis hin zu einem vollständig konfigurierten Projekt, das mit Ihrem [!DNL GitHub]-Repository verknüpft ist.

## Öffnen Sie [!DNL LLM Apps].

Navigieren Sie zu [experience.adobe.com/llm-apps](https://experience.adobe.com/llm-apps). Wenn noch keine App erstellt wurde, wird die Seite mit dem ersten Laden angezeigt, auf der Sie aufgefordert werden, Ihre erste App zu erstellen.

![Apps-Seite - noch keine Apps erstellt](/help/assets/guide-create-app/first-load.png)

In der linken Seitenleiste können Sie zwischen **[!UICONTROL Apps]** und **[!UICONTROL Actions]** navigieren. Klicken Sie **[!UICONTROL Create App]**, um zu beginnen.

## App-Details ausfüllen

Das Dialogfeld „App erstellen“ wird im Vollbildmodus geöffnet.

![Dialogfeld „App erstellen“](/help/assets/guide-create-app/app-details-1.png)

Geben Sie Folgendes ein:

- **[!UICONTROL LLM App Name]** (erforderlich) - der Anzeigename für Ihre App. Nur Buchstaben, Zahlen und Leerzeichen sind zulässig.
- **[!UICONTROL LLM-App-Beschreibung]** - eine kurze Beschreibung der Funktionen Ihrer App. Beispielsweise „Hilft *Benutzern, Produkte zu finden und Services über eine LLM-Plattform zu buchen*.
- **[!UICONTROL Ihre Website]** (erforderlich) - die URL Ihrer Markenwebsite. [!DNL LLM Apps] erstellt damit automatisch vorkonfigurierte Aktionen.

## Analytics-Datenregion auswählen

Wählen Sie die Region aus, in der Analytics-Daten für diese App gespeichert werden.

>[!IMPORTANT]
>
>Die Analytics-Datenregion kann nach der Erstellung der App nicht mehr geändert werden.

![Dropdown-Liste „Analytics-Datenregion“](/help/assets/guide-create-app/app-details-analytics-dropdown.png)

Die Dropdown **Liste „Analytics-Region** ist standardmäßig **Vereinigte Staaten (USA)**. Die verfügbaren Optionen sind **Vereinigte Staaten (USA)** und **Europa (EU)**. Wählen Sie die Region aus, die Ihren Datenresidenzanforderungen am besten entspricht, bevor Sie fortfahren.

## Verknüpfen eines [!DNL GitHub] Repositorys

Unter den App-Details können Sie ein [!DNL GitHub]-Repository verknüpfen. In diesem Repository befindet sich der Code Ihres Aktions-Handlers - JavaScript funktioniert unter einem `actions/` Ordner, der auf [!DNL Adobe I/O Runtime] ausgeführt wird, wenn die LLM-Plattform Ihre App aufruft.

Wenn Sie dies zum ersten Mal tun, werden keine Repositorys in der Liste angezeigt. Sie müssen die **[!DNL Adobe LLM Apps Link]** [!DNL GitHub] App in Ihrem Unternehmen installieren:

1. Klicken Sie **unteren Rand des Dialogfelds auf** Repos auf GitHub verwalten“.
2. Dadurch wird die Seite &quot;[!DNL Adobe LLM Apps Link] [!DNL GitHub] App“ in einer neuen Registerkarte geöffnet.

   ![Link zu Adobe LLM-Apps — GitHub-App-Installationsseite](/help/assets/guide-create-app/github-app-install.png)

3. Klicken Sie **[!UICONTROL Installieren]** und wählen Sie Ihre [!DNL GitHub] Organisation aus.
4. Wählen Sie **[!UICONTROL Repository-]**) die Option **Nur Repositorys auswählen** und wählen Sie das Repository aus, in dem der App-Code gehostet werden soll.

   ![Adobe LLM Apps Link — Repository-Zugriff](/help/assets/guide-create-app/github-repo-access.png)

5. Klicken Sie auf **[!UICONTROL Speichern]**. Kehren Sie zum Dialogfeld „App erstellen“ zurück - Ihr Repository wird jetzt in der Dropdown-Liste **Repository auswählen** angezeigt.
6. Wählen Sie das Repository aus, das Sie verwenden möchten.

![Dialogfeld „App erstellen“ - Repository-verknüpft](/help/assets/guide-create-app/app-details-repo-linked.png)

>[!NOTE]
>
>Sie können die Verknüpfung eines Repositorys während der App-Erstellung überspringen und sie später in den App-Einstellungen vornehmen. Sie können jedoch erst dann bereitstellen, wenn ein Repository verknüpft ist.

## Erstellen der App

Klicken Sie **[!UICONTROL Create App]**. Ein Ladebildschirm wird angezeigt, während das Projekt in Developer Console erstellt wird.

![App erstellen — Ladebildschirm](/help/assets/guide-create-app/app-loading.png)

Nach Abschluss des Vorgangs werden Sie zur Seite **App-Details** weitergeleitet.

## Die App-Detailseite

Die Seite mit den App-Details ist der zentrale Hub für die Verwaltung Ihrer App.

![App-Detailseite - obere Abschnitte](/help/assets/guide-create-app/app-detail-top.png)

### App-Banner

![App-Banner](/help/assets/guide-create-app/app-banner.png)

Das farbige Banner oben zeigt die aktuell ausgewählte App an - einschließlich App-Avatar, Name, Beschreibung und einem Dropdown-Menü zum Wechseln zwischen Apps. Das Banner bleibt beim Scrollen oben fixiert.

### Seitentitel und Aktionen

![App-Banner](/help/assets/guide-create-app/page-title.png)

Unter dem Banner wird der App-Name als Überschrift mit den folgenden Aktionsschaltflächen angezeigt:

- **…** (weitere Aktionen) — Neue App erstellen oder die aktuelle löschen.
- **[!UICONTROL Einstellungen]** - Konfigurieren des verknüpften Repositorys und anderer Optionen.
- **[!UICONTROL Bereitstellen]** - Bereitstellen der App für [!DNL Adobe I/O Runtime] (deaktiviert, bis ein Repository verknüpft ist).

### App-Informationskarte

![App-Informationskarte](/help/assets/guide-create-app/app-info-card.png)

Diese Karte fasst die wichtigsten Metadaten Ihrer App zusammen: Name, Beschreibung, Status-Badge (**Nicht bereitgestellt** oder **bereitgestellt**), App-ID und Erstellungsdatum. Außerdem werden die beiden verknüpften Repositorys angezeigt:

- **Handler-Repository** - Hier befindet sich der Aktionshandler-Code (JavaScript funktioniert auf [!DNL Adobe I/O Runtime]).
- **EDS repo** - Hier lebt die Widget-Benutzeroberfläche (von [!DNL Edge Delivery Services] bereitgestellte Blöcke und Stile).

### Aktionen, Testen der App und Bereitstellungsverlauf

![App-Detailseite - untere Abschnitte](/help/assets/guide-create-app/app-detail-bottom.png)

Unterhalb der Informationskarte befinden sich drei Bereiche:

- **[!UICONTROL Aktionen]** - Listet die für Ihre App definierten Aktions-Handler auf. Klicken Sie **Wechseln zu Aktionen**, um zur Seite „Aktionen“ zu navigieren.
- **[!UICONTROL Programm testen]** zeigt nach der Bereitstellung die MCP-Server-URLs für Staging- und Produktionsumgebungen an.
- **Bereitstellungsverlauf** - verfolgt jede Bereitstellung über Umgebungen hinweg mit Status und Datum.

## Nächste Schritte

- [Anleitung: Erstellen einer Aktion](/help/guides/create-action.md) - Definieren einer Aktion mit Metadaten- und Widget-Einstellungen.

