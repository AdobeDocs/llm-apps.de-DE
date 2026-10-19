---
title: Erstellen von Aktionen
description: Generieren von Adobe-LLM-App-Aktionen von einer öffentlichen Website, Verfolgen des Generierungsfortschritts, Überprüfen der generierten Arbeit oder Auswählen der manuellen Aktionserstellung.
source-git-commit: 0f9dd0a5b1008e183eb8ab5a4c2eae34a78923bb
workflow-type: tm+mt
source-wordcount: '938'
ht-degree: 0%
---

# Erstellen von Aktionen {#creating-actions}

>[!IMPORTANT]
>
>[!DNL Adobe LLM Apps] befindet sich derzeit in Beta.
>
>Die hier gezeigten Funktionen, Workflows und Benutzeroberflächen stellen nicht unbedingt den endgültigen Status des Produkts dar. Um Beta beizutreten, senden Sie eine E-Mail an llm-apps-beta@adobe.com.

Aktionen sind die Funktionen, die Ihre App innerhalb einer LLM-Konversation bietet, z. B. Produkte finden, die Verfügbarkeit überprüfen oder einen Service buchen. Der Handler jeder Aktion führt die Aufgabe aus, und ein optionales Widget stellt das Ergebnis dar.

Wählen [ nach &quot;](/help/guides/create-app.md)&quot; aus, wie Aktionen auf der App-Seite erstellt werden sollen. Die automatische Generierung ist sowohl mit der von Adobe verwalteten Infrastruktur als auch mit Ihren eigenen Repositorys verfügbar.

**In diesem Handbuch** Erfahren Sie, wie Sie Aktionen von Ihrer Website generieren oder selbst erstellen. Starten Sie für die automatische Generierung den -Prozess und überprüfen Sie die Ergebnisse, bevor Sie die App bereitstellen und testen.

## Auswählen, wie Aktionen erstellt werden

Öffnen Sie Ihre App. Wenn Adobe die Infrastruktur noch vorbereitet, warten Sie, bis das Setup abgeschlossen ist.

Im Abschnitt **[!UICONTROL Aktionen]** **[!UICONTROL Wie sollen Aktionen erstellt werden?]** Es gibt zwei Möglichkeiten:

| Auswahl | Verwenden Sie sie, wenn |
|---|---|
| **[!UICONTROL Aktionen automatisch erstellen]** | Sie möchten, dass die Plattform Ihre Website analysiert und vorgeschlagene Aktionen, Handler-Code, Tests und Widgets generiert. Dies ist der empfohlene Ausgangspunkt. |
| **[!UICONTROL Ich erstelle selbst Aktionen]** | Sie möchten jede Aktion definieren und ihre Implementierung selbst bereitstellen. |

Bei Auswahl der Infrastruktur im Assistenten zum Erstellen einer App wird keine dieser Optionen für Sie ausgewählt.

## Aktionen von einer Website generieren {#generate-from-website}

1. Wählen Sie **[!UICONTROL Aktionen automatisch erstellen]** aus.
2. Geben **[!UICONTROL in „Ihre Website]** eine öffentliche HTTPS-Website ein, die die Produkte, Services oder Kundenaufgaben Ihrer Marke darstellt. In diesem Beispiel wird `https://frescopa.coffee/` verwendet.
3. Wählen **[!UICONTROL Meine Aktionen erstellen]** aus.

![Aktionen - Automatische Generierung, die mit der vor dem Start eingegebenen öffentlichen Website ausgewählt wird](/help/assets/guide-creating-actions/build-choice.png)

Verwenden Sie eine Website, die Sie analysieren dürfen. Stellen Sie keine Website zur Verfügung, auf der vertrauliche oder zugriffskontrollierte Informationen verfügbar gemacht werden.

Wenn Sie **[!UICONTROL Meine Aktionen erstellen]** auswählen, wird die Generierung gestartet. Die Eingabe der URL allein funktioniert nicht. Sie müssen die App nicht neu erstellen oder den Infrastrukturassistenten erneut öffnen.

### Generierungsfortschritt verfolgen

Auf der App-Seite **[!UICONTROL Aktionen werden generiert]**. Wählen Sie **[!UICONTROL Gehe zu Aktionen]** aus oder öffnen Sie **[!UICONTROL Aktionen]** in der Navigation, um den Fortschritt zu verfolgen.

Die Benutzeroberfläche schätzt, dass bis zu 30 Minuten für die Generierung benötigt werden. Verlassen Sie die Seite und kehren Sie bei Bedarf später zurück.

![Aktionen - Die Generierung wird ausgeführt, mit einem Link, um den Fortschritt auf der Seite „Aktionen“ zu verfolgen](/help/assets/guide-creating-actions/generating.png)

Während der Generierung wird die Plattform:

1. Analysiert die Website und identifiziert Kundenaufgaben für die App.
2. Erstellt vorgeschlagene Aktionsmetadaten, einschließlich Beschreibungen und Eingabeparametern.
3. Erzeugt Server-seitige Handler und Tests im Aktions-Repository.
4. Erzeugt Widgets im EDS-Repository.
5. Bereitet die generierten Aktionen zur Überprüfung vor.

Während die Plattform die Website analysiert, zeigt die Seite Aktionen **[!UICONTROL Erkunden von Aktionen für Ihr Gesprächserlebnis]**. Es werden empfohlene Aktionen angezeigt, sobald die Plattform sie erkennt. Möglicherweise werden noch Code- und Widget-Vorschauen generiert. Warten Sie, bis die Generierung abgeschlossen ist, bevor Sie jede Aktion überprüfen.

>[!IMPORTANT]
>
>Generierte Handler verwenden zunächst repräsentative Beispieldaten. Die Website-Analyse verbindet Ihre App nicht automatisch mit Produktions-APIs, Live-Inventar, Zahlungs-Services oder Buchungs-Systemen.

Wenn die Generierung einen Fehler meldet, lesen Sie den sichtbaren Fehler durch und lesen Sie die [Anleitung zur Fehlerbehebung](/help/reference/troubleshooting.md). Eine Aktion nicht genehmigen, nur um einen Generierungsfehler zu umgehen.

## Überprüfen der generierten Aktionen {#review-generated-actions}

Überprüfen Sie jede vorgeschlagene Aktion vor der Bereitstellung. Vergewissern Sie sich, dass es sich um eine nützliche Aufgabe handelt und dass das generierte Verhalten mit dem übereinstimmt, was die App tun soll.

![Aktionen - generierte Frescopa-Aktionen sind zur Überprüfung bereit und wurden nicht bereitgestellt](/help/assets/guide-creating-actions/actions-ready-for-review.png)

1. Wählen Sie auf der **[!UICONTROL Aktionen]** die Option **[!UICONTROL Überprüfen]** für eine Aktion aus, die bereit ist.
2. Überprüfen Sie die Beschreibung, die Eingabeparameter, die Anmerkungen, die Sichtbarkeit und den generierten Handler der Aktion.
3. Überprüfen Sie die Widget-Konfiguration und die Vorschau, wenn die Aktion über ein Widget verfügt.
4. Überprüfen Sie den generierten Code und die Tests, einschließlich aller verknüpften Pull-Anforderungen. Überprüfen Sie, was Beispielverhalten ist und was durch echte Integrationen ersetzt werden muss.
5. Wenn die Aktion fertig ist, wählen Sie **[!UICONTROL Als geprüft markieren]** aus.
6. Kehren Sie zur Aktionsliste zurück und wiederholen Sie den Vorgang für die verbleibenden Aktionen.

Der Editor für gespeicherte Aktionen trennt **[!UICONTROL LLM-Widget-Details]**, **[!UICONTROL Aktionsdetails]** und **[!UICONTROL Analytics]**. Überprüfen Sie die Widget- und Handler-Pull-Anforderungen auf den entsprechenden Registerkarten.

![Erzeugte Aktion - aktuelle Editor-Registerkarten und als geprüft markieren, wobei der Repository-Link ausgeblendet ist](/help/assets/guide-creating-actions/generated-action-review.png)

Wenn Sie **[!UICONTROL Als geprüft markieren]** wählen, werden die generierten Pull-Anfragen der Aktion zusammengeführt, wenn keine Merge-Blocker mehr übrig sind. Wenn eine offene Pull-Anfrage die Überprüfung blockiert, lösen Sie die Blockierung und versuchen Sie es erneut. Keine Implementierung mit nicht behobenen Code-Generierungs- oder Zusammenführungsfehlern genehmigen

Nachdem Sie alle generierten Aktionen überprüft haben, wird auf der Seite Aktionen **[!UICONTROL Alle Aktionen werden überprüft!]** angezeigt. Um zur App zurückzukehren, wählen Sie **[!UICONTROL Zur App-Seite gehen]**.

Überprüfung ist keine Bereitstellung. Die Aktionen sind erst dann über den MCP-Server Ihrer App verfügbar, wenn Sie die App bereitstellen.

## Aktionen selbst erstellen {#manual-actions}

Wenn Sie keine Aktionen von einer Website generieren möchten:

1. Wählen Sie in der Auswahl zur Aktionserstellung der App die Option **[!UICONTROL Ich erstelle selbst Aktionen]** aus.
2. Wählen Sie **[!UICONTROL Ich erstelle meine Aktionen]** aus.
3. Wählen Sie auf der Seite Aktionen die Option **[!UICONTROL Aktion erstellen]** aus.
4. Folgen Sie [Erstellen einer Aktion von Grund auf](/help/guides/create-action.md) um die Metadaten zu definieren, einen Handler zu implementieren und bei Bedarf ein Widget zu verbinden.

Die Infrastruktur bleibt gleich. Wenn Sie die manuelle Erstellung wählen, wird kein Handler-Code erstellt oder Ihre Geschäftssysteme werden für Sie verbunden.

Fügen Sie manuelle Aktionen hinzu, nachdem Sie automatisch generierte Aktionen nach Bedarf überprüft haben.

## Bereitstellen und Testen

Nach Überprüfung der erstellten Arbeit:

1. [Bereitstellen der App](/help/guides/deploy-your-app.md) für **[!UICONTROL Staging]**.
2. Testen Sie sie in [ChatGPT](/help/guides/test-in-chatgpt.md) oder [Claude](/help/guides/test-in-claude.md) mithilfe der Staging-MCP-Server-URL.
3. Testaufforderungen, die jeder Aktion entsprechen, und nicht verwandte Aufforderungen, die diese Aktion nicht aufrufen. Überprüfen Sie sowohl die Textantwort als auch die Widget-Interaktionen.

Bevor Sie die App für Kundinnen und Kunden verfügbar machen, [ Sie „die Handler anpassen](/help/guides/customize-handler.md), um Ihre APIs zu verbinden und [die Widgets anzupassen](/help/guides/widgets.md). Testen Sie die App in „Staging“, um zu bestätigen, dass sie erwartungsgemäß funktioniert. Bereitstellung in der Produktion erst nach erfolgreichem Test.
