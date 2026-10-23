---
title: Vorhandene App aktualisieren
description: Aktualisieren Sie die Laufzeitumgebung und EDS-SDK einer älteren Adobe LLM-App, bevor Sie Adobe Analytics oder Customer Journey Analytics aktivieren.
source-git-commit: 557173e8d95325b980efac4d72c06bb3d90dcd72
workflow-type: tm+mt
source-wordcount: '793'
ht-degree: 0%
---

# Vorhandene App aktualisieren {#upgrade-existing-app}

>[!IMPORTANT]
>
>[!DNL Adobe LLM Apps] befindet sich derzeit in Beta.
>
>Die hier gezeigten Funktionen, Workflows und Benutzeroberflächen stellen nicht unbedingt den endgültigen Status des Produkts dar. Um Beta beizutreten, senden Sie eine E-Mail an llm-apps-beta@adobe.com.

Verwenden Sie dieses Handbuch nur, wenn Ihre App `@adobe/llm-apps-runtime` vor 1.2.0 verwendet oder ihre EDS-Widgets die LLM Apps SDK vor 0.1.13 verwenden. Apps, die beide Anforderungen bereits erfüllen, können direkt mit [Einrichten von Adobe Analytics und Customer Journey Analytics](/help/guides/analytics-setup.md) fortfahren.

**In diesem Handbuch:** Erfahren Sie, wie Sie die Laufzeit und EDS-SDK einer älteren App aktualisieren und überprüfen. Kehren Sie dann zum Analytics-Setup-Handbuch zurück, um Analytics zu aktivieren und die Sammlung zu überprüfen.

## Voraussetzungen {#before-you-begin}

Sie benötigen Zugriff auf das Handler-Repository und - bei Apps mit EDS-Widgets - auf das mit Ihrer App verknüpfte EDS-Repository. Wenn Ihre App die von Adobe verwaltete Infrastruktur verwendet, fordern Sie Code-Zugriff an, bevor Sie Aktualisierungen vornehmen. Siehe [Verwenden von Adobe Managed Infrastructure](/help/guides/create-app.md#adobe-managed-infrastructure).

Das Erstellen einer anderen App ist nicht erforderlich. Aktualisieren Sie nur die Komponenten, die sich unter den unterstützten Versionen befinden. Sie können eine bestehende Adobe-Konfiguration wiederverwenden, die den [Analytics-Setup-Anforderungen“ &#x200B;](/help/guides/analytics-setup.md#setup-checklist).

Für Apps mit EDS-Widgets veröffentlichen und überprüfen Sie zunächst das SDK-Update, führen Sie dann das Laufzeitupdate zusammen und stellen Sie die App erneut bereit. Die beiden Repository-Aktualisierungen können unabhängig voneinander vorbereitet werden, aber beide müssen verfügbar sein, bevor Sie die Analytics-fähige Bereitstellung testen.

## Überprüfen und Aktualisieren des LLM Apps Action Repository {#upgrade-runtime}

1. Öffnen Sie das mit Ihrer App verknüpfte Handler-Repository und installieren Sie die Abhängigkeiten:

   ```bash
   npm install
   ```

1. Überprüfen Sie die installierte Laufzeit:

   ```bash
   npm ls @adobe/llm-apps-runtime
   ```

1. Wenn die Laufzeit vor 1.2.0 liegt, installieren Sie die aktuelle Version:

   ```bash
   npm install @adobe/llm-apps-runtime@latest
   npm ls @adobe/llm-apps-runtime
   ```

   Vergewissern Sie sich, dass die installierte Version 1.2.0 oder höher ist. Behalten Sie eine spätere unterstützte Version bei, wenn Ihre App bereits eine verwendet.

1. Vergewissern Sie sich, dass die Laufzeitabhängigkeit und die aufgelöste Version die Mindestanforderung erfüllen. Die eigene Version des Programms ist von der Laufzeitabhängigkeit getrennt.
1. Führen Sie die Prüfungen des Repositorys aus. Für das LLM Apps-Textbaustein:

   ```bash
   npm test -- --runInBand
   npm run build
   npm run lint
   ```

   Fehler vor der Bereitstellung untersuchen. Wenn eine Prüfung in der Originalversion bereits fehlschlägt, zeichnen Sie diese Baseline getrennt von allen Fehlern auf, die durch das Upgrade verursacht wurden. Bei einem erfolgreichen lokalen Build wird die Analytics-Sammlung nicht überprüft.

1. Bestätigen Sie die Abhängigkeitsaktualisierung und die Sperrdatei und übertragen Sie dann die Aktualisierung per Push und führen Sie sie in der Regel `main` in der Standardverzweigung des Handler-Repositorys zusammen. Bereitstellungs-Klone, die eine Verzweigung aufweisen Ein Update, das nur lokal oder in einer nicht zusammengeführten Funktionsverzweigung vorhanden ist, ist nicht in der Bereitstellung enthalten.

   Wenn eine Sperrdatei vorhanden ist, installiert die Bereitstellung Abhängigkeiten mit `npm ci`, das die aufgezeichneten Versionen verwendet. Beim erneuten Bereitstellen wird die Laufzeit nicht automatisch aktualisiert. Bestätigen Sie vor der erneuten Bereitstellung die upgegradete Abhängigkeit und aufgelöste Version auf der Standardverzweigung in GitHub.

## Überprüfen und Aktualisieren des LLM Apps Widget-Repositorys {#upgrade-sdk}

Bei Apps mit EDS-Widgets überprüfen Sie die SDK im EDS-Repository sowie die Server-Laufzeit. Beim Aktualisieren eines Repositorys wird das andere nicht aktualisiert.

Einige EDS-Repositorys enthalten kopierte SDK-Skripte ohne npm-Abhängigkeit. Da der Browser diese bereitgestellten Skripte direkt lädt, bedeutet das Fehlen eines Paketeintrags nicht, dass die SDK fehlt. Aktualisieren Sie die kopierte SDK-Verteilung mithilfe des SDK-Aktualisierungsprozesses Ihres Projekts. Das Hinzufügen einer persistenten npm-Abhängigkeit ist für ein Projekt, das die SDK als kopierte Skripte beibehält, nicht erforderlich.

Für Projekte, die den SDK über npm verwalten:

1. Installieren Sie im Stammverzeichnis des EDS-Repositorys die aktuelle SDK und bestätigen Sie deren Version:

   ```bash
   npm install --save-dev @adobe/llmapps-sdk@latest
   npm ls @adobe/llmapps-sdk
   ```

   Verwenden Sie 0.1.13 oder höher. Behalten Sie eine spätere unterstützte Version bei, wenn Ihr Projekt bereits eine verwendet.

1. Stellen Sie sicher, dass die Installation die bereitgestellte SDK-Distribution aktualisiert hat. Der Installations-Hook überschreibt die kopierten SDK-Skripte. Überprüfen Sie daher alle lokalen Anpassungen, bevor Sie einen Commit ausführen. Durch die Aktualisierung nur der Abhängigkeitsdeklaration werden die von EDS bereitgestellten Skripte nicht aktualisiert. Siehe [Installieren des LLM Apps SDK](/help/guides/bring-your-own-eds.md#install-the-llm-apps-sdk).
1. Führen Sie die Prüfungen des EDS-Repositorys aus, z. B. `npm run lint`, und überprüfen Sie, ob ein vorhandenes Widget weiterhin gerendert wird und auf Interaktionen reagiert.
1. Übertragen Sie die aktualisierte SDK-Distribution zusammen mit etwaigen Abhängigkeits- und Sperrdateiaktualisierungen. EDS stellt die kopierten Skripte bereit, nicht das installierte npm-Paket direkt.

Nach jeder Aktualisierungsmethode die Aktualisierung per Push übertragen und in die EDS-Verzweigung zusammenführen, die von der (Skript **[!UICONTROL URL) der Aktion verwendet]**. Vergewissern Sie sich, dass die URL für die aktualisierte SDK verwendet wird und dass die unterstützenden Bibliotheken erfolgreich geladen wurden. Wenn die URL Ihre EDS-Live-Site verwendet, führen Sie die Veröffentlichungsschritte für diese Site aus.

## Nächster Schritt {#next-step}

Kehren Sie zu [Einrichten von Adobe Analytics und Customer Journey Analytics](/help/guides/analytics-setup.md) zurück. Alle Apps müssen Analytics aktivieren und bereitstellen, um Ereignisse zu erfassen. Beim Upgrade von Runtime oder SDK allein wird die Erfassung nicht aktiviert.

Stellen Sie nach dem Speichern der Analytics-Einstellungen das Staging bereit und [&#x200B; Sie (neue Aktionsaufruf- und Widget-Interaktionsereignisse), &#x200B;](/help/guides/analytics-setup.md#verify) Sie es für die Produktion bereitstellen. Bestätigen Sie, dass die Aktion und das Widget weiterhin funktionieren und dass eine einzelne Link-Auswahl ein Widget-Interaction-Ereignis erzeugt.

Analytics beginnt mit der Erfassung neuer Interaktionen nach der Analytics-fähigen Bereitstellung. Beim Upgrade werden Interaktionen, die vor der Aktivierung der Sammlung aufgetreten sind, nicht rekonstruiert.
