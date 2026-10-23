---
title: Einrichten von Adobe Analytics und Customer Journey Analytics
description: Leiten Sie Adobe LLM Apps-Ereignisse über einen Adobe Experience Platform-Datenstrom an Ihre eigene Adobe Analytics-Report Suite und Customer Journey Analytics-Datenansicht weiter.
source-git-commit: 557173e8d95325b980efac4d72c06bb3d90dcd72
workflow-type: tm+mt
source-wordcount: '2962'
ht-degree: 3%
---

# Einrichten von Adobe Analytics und Customer Journey Analytics {#analytics-setup}

>[!IMPORTANT]
>
>[!DNL Adobe LLM Apps] befindet sich derzeit in Beta.
>
>Die hier gezeigten Funktionen, Workflows und Benutzeroberflächen stellen nicht unbedingt den endgültigen Status des Produkts dar. Um Beta beizutreten, senden Sie eine E-Mail an llm-apps-beta@adobe.com.

Senden Sie die Ereignisse Ihrer App an [!DNL Adobe Analytics], [!DNL Customer Journey Analytics] oder beide. [!DNL LLM Apps] leitet die Ereignisse über einen eigenen Datenstrom an die [!DNL Adobe Experience Platform] Edge Network weiter. Der Datenstrom leitet die Ereignisse an jeden Service weiter, den Sie ihm hinzufügen.

- **[!DNL Adobe Analytics]** gibt an, welche Aktionen die LLM-Plattform aufruft, welches Widget die ausgewählten Benutzer verknüpft und welche Absicht hinter jedem Aufruf steckt.
- **[!DNL Customer Journey Analytics]** analysiert dieselben Ereignisse in den Datenansichten, die Sie auf [!DNL Experience Platform] Daten aufbauen, nach Person.

Diese Einrichtung ist getrennt von der integrierten Analyse in [!DNL LLM Apps].

**In diesem Handbuch** Erfahren Sie, welche Einstellungen [!DNL LLM Apps] in [!DNL Experience Platform], [!DNL Adobe Analytics] und [!DNL Customer Journey Analytics] erfordert, mit Links zum Experience League-Verfahren für jeden Produktbildschirm. Aktivieren Sie dann Analytics in Ihrer App, stellen Sie es für die Staging-Umgebung bereit und überprüfen Sie die Daten, bevor Sie sie für die Produktion bereitstellen.

## Checkliste einrichten {#setup-checklist}

Führen Sie alle Schritte aus, **für die** Dienste als „Erforderlich“ gekennzeichnet sind. Ein übersprungener Schritt zeigt selten einen Fehler an. Stattdessen gelangen die Daten nicht.

| Schritt | [!DNL Adobe Analytics] | [!DNL Customer Journey Analytics] |
|---|---|---|
| [1. Erstellen Sie den Identity-Namespace](#identity-namespace) | Erforderlich | Erforderlich |
| [2. Erstellen Sie das Schema &#x200B;](#schema) | Erforderlich | Erforderlich |
| [3. Datensatz erstellen](#dataset) | Nicht erforderlich | Erforderlich |
| [4. Vorbereiten der Report Suite](#report-suite) | Erforderlich | Nicht erforderlich |
| [5. Erstellen Sie den Datenstrom](#datastream) | Erforderlich | Erforderlich |
| [6. Analytics aktivieren und für Staging bereitstellen](#enable-analytics) | Erforderlich | Erforderlich |
| [7. Erstellen Sie die Verbindung und die Datenansicht](#cja) | Nicht erforderlich | Erforderlich |
| [8. Überprüfen Sie die Daten und stellen Sie sie in der Produktionsumgebung bereit](#verify) | Erforderlich | Erforderlich |

Erstellen Sie den Namespace, das Schema, den Datensatz und den Datenstrom in derselben [!DNL Experience Platform] Sandbox. Verwenden Sie die Sandbox und den Datenstrom, aus denen Sie in der Produktion Berichte erstellen möchten. Nachdem Sie den Datenstrom in Ihrer App gespeichert haben, wenden Sie sich an das LLM Apps-Team, um ihn zu ändern.

## Voraussetzungen {#before-you-begin}

Sie benötigen:

- **Unterstützte Laufzeit und SDK.** Ihr Handler-Repository muss `@adobe/llm-apps-runtime` 1.2.0 oder höher verwenden. Verwenden Sie für EDS-Widgets die LLM-Apps SDK 0.1.13 oder höher. Um eine ältere App auf die neueste Laufzeit und SDK zu aktualisieren, bevor Sie Analytics aktivieren, folgen Sie [Aktualisieren einer bestehenden App](/help/guides/upgrade-existing-app.md). Apps, die diese Anforderungen erfüllen, müssen nicht aktualisiert werden.
- **Die agenten Feldergruppen.** Ihre Organisation muss für die Feldergruppen Agenteninformationen und Konversationsereignisse aktiviert sein. Wenn diese Feldergruppen beim Hinzufügen von Feldergruppen zu einem Schema nicht angezeigt werden, wenden Sie sich an Ihr Adobe-Accountteam. Siehe [Implementieren von Conversation Insights](https://experienceleague.adobe.com/en/docs/analytics-platform/using/conversation-insights/implement#schema-and-datasets). Aktionsaufrufe werden im `agenticExperience.tools` Bereich Agenteninformationen aufgezeichnet, der sich in Beta in [!DNL Experience Platform] befindet.
- **[!DNL Experience Platform]Berechtigungen** um Identity-Namespaces, Schemata, Datensätze und Datenströme in Ihrer Sandbox zu erstellen.
- **Für [!DNL Adobe Analytics]:** Berechtigung zum Verwalten von Report Suites und Verarbeitungsregeln. Produktadministratoren haben standardmäßig beide Möglichkeiten.
- **Für [!DNL Customer Journey Analytics]:** Produktadministratorzugriff zum Erstellen von Verbindungen und Datenansichten.

## Was Ihre App sendet {#what-your-app-sends}

Nachdem Sie Analytics aktiviert und bereitgestellt haben, sendet Ihre App zwei Arten von Ereignissen:

| Ereignis | `eventType` | Gesendet, wenn | Enthält |
|---|---|---|---|
| Aktionsaufruf | `mcp.tool_call` | Die LLM-Plattform ruft eine der Aktionen Ihrer App auf, einschließlich Aufrufen, die fehlschlagen oder eine Anmeldeaufforderung zurückgeben. | Die Code-Kennung der Aktion und ob der Aufruf fehlgeschlagen ist. Die Benutzerabsicht, wenn die Aktion sie erfasst und die Plattform eine bereitstellt. Wenn die Plattform sie bereitstellt: den Plattformnamen, die Konversations-ID, die Benutzer-ID, das Gebietsschema und die Organisation der Plattform und den ungefähren Standort des Benutzers. |
| Widget-Interaktion | `web.webinteraction.linkClicks` | Jemand wählt einen Link oder eine Schaltfläche in einem Edge Delivery Services-Widget aus. | Die Link- oder Schaltflächenbeschriftung mit bis zu 100 Zeichen. Wenn vom Aktionsaufruf angegeben, die Konversations-ID und die Turn-ID. Adobe Web SDK fügt auch die Widget-Frame-URL, die Bildschirm- und Browser-Fenstergröße, die lokale Zeit und Zeitzone sowie eine eigene Version hinzu. |

In [!DNL Adobe Analytics] zählt jeder Aktionsaufruf als Seitenansicht einer Seite mit dem Namen `mcp:<code identifier>`, und jede Widget-Interaktion zählt als Link-Klick. Siehe [Edge Network-Ereignistypen in Adobe Analytics](https://experienceleague.adobe.com/en/docs/analytics/implementation/aep-edge/hit-types).

Jeder Aktionsaufruf und jede Widget-Interaktion ist ein [!DNL Adobe Analytics] Treffer und ein Ereignis in Ihrem [!DNL Experience Platform] Datensatz. Vergewissern Sie sich bei einer Mobile App mit hohem Traffic, dass Ihr Vertrag dieses Volumen abdeckt, bevor Sie Analytics aktivieren.

Beachten Sie diese Details beim Lesen der Berichte:

- **Fehlgeschlagene Aufrufe enthalten Anmeldeaufforderungen.** Wenn für einige Aktionen eine Anmeldung erforderlich ist, ist eine höhere Fehlerrate zu erwarten.
- **Einige zurückgewiesene Aufrufe werden nicht aufgezeichnet.** Ein Aufruf wird abgelehnt, bevor die Aktion ausgeführt wird, und sendet kein Ereignis, wenn eine andere Plattform als [!DNL ChatGPT] eine Aktion aufruft, die eine Anmeldung ohne gültige Anmeldung oder den erforderlichen Umfang erfordert. In Apps, in denen für jede Aktion eine Anmeldung erforderlich ist, wird jeder Aufruf ohne Anmeldung auf dieselbe Weise abgelehnt.
- **Die Benutzerabsicht fehlt oft.** Die Plattform entscheidet, ob sie bereitgestellt wird, sodass viele Aufrufe keinen aufweisen und als „Unspecified **angezeigt**.
- **Plattformdetails variieren.** Derzeit stellt [!DNL ChatGPT] alle bereit. [!DNL Claude] wird nur der Plattformname aufgezeichnet.
- **Widget-Kennzeichnungen stammen von der Seite.** Um eine stabile Beschriftung festzulegen, fügen Sie dem Link oder der Schaltfläche ein `data-analytics` Attribut hinzu. Andernfalls wird sein sichtbarer Text verwendet.

Informationen zum Auswählen, welche Aktionen die Benutzerabsicht erfassen sollen, finden Sie unter [Analytics konfigurieren](/help/guides/create-action.md#configure-analytics).

>[!NOTE]
>
>Die Ereignisse können eine Platform-Benutzer-ID und den ungefähren Standort enthalten. Überprüfen Sie Ihre Einstellungen für Datenschutzrichtlinien und Data Governance, bevor Sie Analytics aktivieren.

### Identifizieren der Personen durch die Ereignisse {#identities}

| Ereignis | Primäre Identität | Namespace |
|---|---|---|
| Aktionsaufruf | Die Benutzer-ID aus der LLM-Plattform, wenn die Plattform eine bereitstellt | `MCPHOSTUSER` |
| Widget-Interaktion | Eine Experience Cloud ID (ECID), die von Edge Network während des Aktionsaufrufs ausgegeben wird. Die `MCPHOSTUSER`-ID wird, sofern verfügbar, als sekundäre Identität einbezogen. | `ECID` |

Einige Plattformen, z. B. [!DNL Claude], geben keine Benutzer-ID an. Aktionsaufrufe von diesen Plattformen haben keine Identität und werden daher von [!DNL Customer Journey Analytics] übersprungen.

>[!NOTE]
>
>Jeder Aktionsaufruf erhält eine neue ECID, und eine Widget-Interaktion verwendet die ECID des Aktionsaufrufs, der das Widget angezeigt hat. Das Ergebnis ist:
>
>- [!DNL Customer Journey Analytics] zählt die Aktionsaufrufe eines Benutzers als eine Person, aber zählt seine Widget-Interaktionen für jeden Aktionsaufruf als eine andere Person.
>- [!DNL Adobe Analytics] kann jeden Aktionsaufruf als neuen Besucher zählen.

## Schritt 1: Identity-Namespace erstellen {#identity-namespace}

**Erforderlich für:** [!DNL Adobe Analytics] und [!DNL Customer Journey Analytics].

Aktionsaufrufe identifizieren Personen im `MCPHOSTUSER`-Namespace, sodass der Namespace in Ihrer Sandbox vorhanden sein muss. Erstellen Sie einen benutzerdefinierten Namespace mit den folgenden Einstellungen:

| Einstellung | Wert |
|---|---|
| Anzeigename | `MCP host user` oder einen anderen von Ihnen ausgewählten Namen |
| Identitätssymbol | `MCPHOSTUSER`, genau wie abgebildet |
| Identitätstyp | **[!UICONTROL Individuelle geräteübergreifende ID]** |

Eine Anleitung finden Sie unter [Erstellen benutzerdefinierter Namespaces](https://experienceleague.adobe.com/en/docs/experience-platform/identity/features/namespaces#create-namespaces).

## Schritt 2: Schema erstellen {#schema}

**Erforderlich für:** [!DNL Adobe Analytics] und [!DNL Customer Journey Analytics].

Der Datenstrom und der Datensatz verwenden beide dieses Schema. Erstellen Sie ein Schema mit den folgenden Einstellungen:

| Einstellung | Wert |
|---|---|
| Schematyp | **[!UICONTROL Standard]**, erstellt mit der Option **[!UICONTROL Manuell]** |
| Basisklasse | **[!UICONTROL Erlebnisereignis]** |
| Feldergruppen | Agenteninformationen, Konversationsereignis, Web-Details, Umgebungsdetails und Implementierungsdetails |
| Anzeigename | Beliebiger Name, z. B. `AEM LLM Apps` |

>[!IMPORTANT]
>
>Alle fünf Feldergruppen hinzufügen. [!DNL LLM Apps] erfordert Agenteninformationen und Konversationsereignisse für Aktionsaufrufe. Die anderen drei enthalten die Felder Seite, Link, Gerät, Umgebung und Implementierung , die die Ereignisse ebenfalls enthalten. Wenn eine Feldergruppe fehlt, können [!DNL Experience Platform] diese Felder löschen oder das Ereignis ablehnen.

Die Adobe Analytics ExperienceEvent-Feldergruppe ist nicht erforderlich. Die Benutzerabsicht erreicht [!DNL Adobe Analytics] über eine Verarbeitungsregel. Siehe [Schritt 4](#report-suite).

Eine Anleitung finden Sie unter [Erstellen eines neuen Schemas](https://experienceleague.adobe.com/en/docs/experience-platform/xdm/ui/resources/schemas#create) und [Hinzufügen von Feldergruppen](https://experienceleague.adobe.com/en/docs/experience-platform/xdm/ui/resources/schemas#add-field-groups).

## Schritt 3: Datensatz erstellen {#dataset}

**Erforderlich für:** [!DNL Customer Journey Analytics].

Der Datensatz speichert die Ereignisse Ihrer App in [!DNL Experience Platform]. Erstellen Sie einen Datensatz mit den folgenden Einstellungen:

| Einstellung | Wert |
|---|---|
| Erstellungsmethode | **[!UICONTROL Erstellen eines Datensatzes aus einem Schema]** |
| Schema | Das Schema aus [Schritt 2](#schema) |
| Name | Beliebiger Name, z. B. `AEM LLM Apps` |

Eine Anleitung finden Sie unter [Erstellen eines Datensatzes](https://experienceleague.adobe.com/en/docs/experience-platform/catalog/datasets/user-guide#create).

## Schritt 4: Report Suite vorbereiten {#report-suite}

**Erforderlich für:** [!DNL Adobe Analytics].

[!DNL Adobe Analytics] ordnet die Felder Seitenname und Link automatisch zu, sodass die `mcp:<code identifier>` Seiten und die Widget-Link-Klicks nicht extra eingerichtet werden müssen. Für die Benutzerabsicht sind eine eVar und eine Verarbeitungsregel erforderlich.

### Report Suite erstellen oder auswählen {#create-report-suite}

Erstellen Sie eine Report Suite oder wählen Sie eine vorhandene aus. Beachten Sie die vollständige Report Suite-ID einschließlich des von [!DNL Adobe Analytics] hinzugefügten Präfixes. Sie benötigen die ID in [Schritt 5](#datastream).

Eine Anleitung finden Sie unter [Erstellen einer Report Suite](https://experienceleague.adobe.com/de/docs/analytics/admin/admin-tools/manage-report-suites/c-new-report-suite/t-create-a-report-suite).

### Aktivieren der User Intent-eVar {#user-intent-evar}

Aktivieren Sie in den Konversionsvariablen der Report Suite eine eVar mit den folgenden Einstellungen:

| Einstellung | Wert |
|---|---|
| eVar | eVar3 |
| Name | `User Intent` |
| Typ | **[!UICONTROL Textzeichenfolge]** |
| Zuteilung | **[!UICONTROL Zuletzt verwendet (Letzte)]** |
| Läuft ab nach | **[!UICONTROL Besuch]** |
| Status | **[!UICONTROL Aktiviert]** |

Wenn die Report Suite bereits eVar3 verwendet, wählen Sie eine andere nicht verwendete eVar aus und verwenden Sie diese eVar in der Verarbeitungsregel.

Eine Anleitung finden Sie unter [Hinzufügen oder Bearbeiten von Konversionsvariablen](https://experienceleague.adobe.com/en/docs/analytics/admin/admin-tools/manage-report-suites/edit-report-suite/conversion-variables/conversion-var-admin#add-or-edit-conversion-variables).

### Erstellen der Verarbeitungsregel {#processing-rule}

[!DNL Adobe Analytics] empfängt die Benutzerabsicht als Kontextdatenvariable, nicht als eVar. Weitere Informationen dazu finden Sie unter [Implizites Mapping](https://experienceleague.adobe.com/en/docs/analytics/implementation/aep-edge/xdm-var-mapping#implicit-mapping). Erstellen Sie eine Verarbeitungsregel, die die Benutzerabsicht in die eVar kopiert:

- **Bedingung:** die Kontextdatenvariable `a.x.conversation.prompt.raw.0.purpose` gleich `intents`.
- **Action:** Überschreiben Sie den Wert von **User Intent (eVar3)** mit der Kontextdatenvariablen `a.x.conversation.prompt.raw.0.text`.

Ohne diese Regel bleibt die Dimension **Benutzerabsicht** leer. Die Regel gilt nur für Daten, die nach dem Speichern erfasst wurden. Widget-Interaktionen haben keine Benutzerabsicht, sodass die Regel sie nicht ändert.

Andere Felder, z. B. ob der Aufruf fehlgeschlagen ist oder der Plattformname, werden ebenfalls als Kontextdatenvariablen angezeigt. Um in [!DNL Adobe Analytics] Berichte dazu zu erstellen, erstellen Sie auf die gleiche Weise weitere Verarbeitungsregeln.

Für die Prozedur siehe [Übersicht über Verarbeitungsregeln](https://experienceleague.adobe.com/en/docs/analytics/admin/admin-tools/manage-report-suites/edit-report-suite/report-suite-general/processing-rules/pr-overview) und [Anwendungsfälle für Verarbeitungsregeln](https://experienceleague.adobe.com/en/docs/analytics/admin/admin-tools/manage-report-suites/edit-report-suite/report-suite-general/processing-rules/pr-use-cases).

## Schritt 5: Erstellen des Datenstroms {#datastream}

**Erforderlich für:** [!DNL Adobe Analytics] und [!DNL Customer Journey Analytics].

Der Datenstrom empfängt die Ereignisse Ihrer App und leitet sie an jeden Service weiter, den Sie hinzufügen.

### Erstellen des Datenstroms {#create-datastream}

Erstellen Sie einen Datenstrom mit den folgenden Einstellungen:

| Einstellung | Wert |
|---|---|
| Name | Beliebiger Name, z. B. `AEM LLM Apps` |
| Zuordnungsschema | Das Schema aus [Schritt 2](#schema) |

Erstellen Sie den Datenstrom in derselben Sandbox wie das Schema. Sie können die Sandbox eines Datenstroms nach seiner Erstellung nicht ändern.

Eine Anleitung finden Sie unter [Erstellen eines Datenstroms](https://experienceleague.adobe.com/en/docs/experience-platform/datastreams/configure#create).

### Hinzufügen der Services {#add-services}

Fügen Sie für jedes Ziel einen Service hinzu:

| Dienst | Einstellungen | Erforderlich für |
|---|---|---|
| **[!UICONTROL Adobe Analytics]** | **[!UICONTROL Aktiviert]**. Geben **[!UICONTROL unter „Report Suite]** ID“ die ID aus [Schritt 4](#report-suite) ein. | [!DNL Adobe Analytics] |
| **[!UICONTROL Adobe Experience Platform]** | **[!UICONTROL Aktiviert]**. Wählen **[!UICONTROL unter]** den Datensatz aus [Schritt 3](#dataset). Lassen Sie die anderen Optionen deaktiviert. | [!DNL Customer Journey Analytics] |

![Service hinzufügen - Adobe Analytics mit einer Beispiel-Report Suite-ID aktiviert](/help/assets/guide-analytics/datastream-analytics-service.png)

![Adobe Experience Platform-Service - Der ausgewählte Ereignis-Datensatz und die anderen Optionen wurden gelöscht](/help/assets/guide-analytics/datastream-platform-service.png)

Eine Anleitung finden Sie unter [Hinzufügen von Services zu einem Datenstrom](https://experienceleague.adobe.com/en/docs/experience-platform/datastreams/configure#add-services), [Adobe Analytics-](https://experienceleague.adobe.com/en/docs/experience-platform/datastreams/configure#analytics) und [Adobe Experience Platform-Einstellungen](https://experienceleague.adobe.com/en/docs/experience-platform/datastreams/configure#aep).

Kopieren Sie dann **[!UICONTROL Datenstrom-ID]** aus den Datenstromdetails. Sie benötigen ihn in [Schritt 6](#enable-analytics).

![Datenstromdetails - Adobe Analytics- und Adobe Experience Platform-Services aktiviert, mit der Beispiel-Datenstrom-ID und dem Zuordnungsschema im Seitenbereich](/help/assets/guide-analytics/datastream-services.png)

## Schritt 6: Analytics aktivieren und für Staging bereitstellen {#enable-analytics}

**Erforderlich für:** [!DNL Adobe Analytics] und [!DNL Customer Journey Analytics].

>[!IMPORTANT]
>
>Nach dem Speichern ist der Datenstrom in den App-Einstellungen schreibgeschützt. Wenden Sie sich an das LLM Apps-Team, um dies zu ändern oder Analytics zu deaktivieren. Überprüfen Sie die Datenstrom-ID, bevor Sie speichern. Sie können weiterhin die Report Suite und den Datensatz im Datenstrom selbst ändern.

1. Öffnen Sie Ihre App und wählen **[!UICONTROL Einstellungen]** im linken Navigationsbereich aus.
2. Wählen Sie die Registerkarte **[!UICONTROL Analytics]** aus.
3. Aktivieren Sie **[!UICONTROL Analytics aktivieren]**.
4. Fügen **[!UICONTROL unter „Datenstrom-]**&quot; die ID aus ([&#x200B; 5) &#x200B;](#datastream).
5. Wählen Sie **[!UICONTROL Anforderungen]** die Option **[!UICONTROL Details anzeigen]** und bestätigen Sie, dass Ihre App die einzelnen Anforderungen erfüllt.

   ![App-Einstellungen, Registerkarte „Analytics“ - Analytics aktivieren, ein leeres Datenstrom-ID-Feld und die erweiterte Liste mit den Anforderungen](/help/assets/guide-analytics/analytics-settings.png)

6. Wählen **[!UICONTROL Änderungen speichern]**.
7. [Bereitstellen der App](/help/guides/deploy-your-app.md) für das Staging. Die Einstellung wird bei der nächsten Bereitstellung in jeder Umgebung wirksam.

>[!NOTE]
>
>Der Datenstrom gilt für die gesamte App. Staging- und Produktionsereignisse senden Ereignisse an denselben Datenstrom, und die Ereignisse zeichnen nicht auf, welche Umgebung sie gesendet hat. Ihre Testaufrufe im Staging werden in denselben Berichten wie Produktions-Traffic angezeigt.

## Schritt 7: Verbindung und Datenansicht erstellen {#cja}

**Erforderlich für:** [!DNL Customer Journey Analytics].

### Erstellen der Verbindung {#connection}

Erstellen Sie eine Verbindung und fügen Sie den Datensatz aus [Schritt 3](#dataset) mit den folgenden Einstellungen hinzu:

| Einstellung | Wert |
|---|---|
| Sandbox | Die Sandbox, die Ihren Datensatz enthält |
| Durchschnittliche Anzahl der täglichen Ereignisse | Der Bereich, der Ihrem erwarteten Volumen entspricht |
| Datensatz | Der Datensatz aus [Schritt 3](#dataset) |
| Personen-ID | **[!UICONTROL Identitätszuordnung]** |
| Verwenden eines primären Identity-Namespace | Ausgewählt |
| Zeitstempel | **[!UICONTROL Zeitstempel]** |
| Datenquellentyp | **[!UICONTROL Andere]** |
| Beschreibung der Datenquelle | Beliebige Beschreibung, z. B. `LLM Apps events` |
| Importieren neuer Daten | **[!UICONTROL Alle neuen Daten importieren]** eingeschaltet |
| Aufstockung des Datensatzes | **[!UICONTROL Alle vorhandenen Daten aufstocken]** aktiviert, wenn der Datensatz bereits Ereignisse enthält, die Sie analysieren möchten |

>[!IMPORTANT]
>
>Aktivieren Sie **[!UICONTROL Alle neuen Daten importieren]**. Durch diese Einstellung wird eine fortlaufende Verbindung hergestellt, sodass neue Ereignisse automatisch in Workspace verfügbar werden.

Mit dieser Personen-ID verwendet [!DNL Customer Journey Analytics] die primäre Identität jedes Ereignisses als Person und überspringt Ereignisse, die keine primäre Identität haben. Siehe [So identifizieren die Ereignisse Personen](#identities).

Eine Anleitung finden Sie unter [Verbindung erstellen](https://experienceleague.adobe.com/en/docs/analytics-platform/using/cja-connections/create-connection#create-connection), [Einstellungen für Ereignis-Datensätze](https://experienceleague.adobe.com/en/docs/analytics-platform/using/cja-connections/create-connection#event-dataset-settings) und [Identitätszuordnung als Personen-ID verwenden](https://experienceleague.adobe.com/en/docs/analytics-platform/using/cja-connections/create-connection#id-map).

### Datenansicht erstellen {#data-view}

Mit diesen Einstellungen eine Datenansicht für die Verbindung erstellen:

| Einstellung | Wert |
|---|---|
| Verbindung | Die soeben erstellte Verbindung |
| Name | Beliebiger Name, z. B. `LLM Apps` |
| Komponenten | Die Dimensionen und Metriken, zu denen Sie einen Bericht erstellen möchten. Siehe Tabelle unten. |

Fügen Sie diese Schemafelder als Komponenten hinzu:

| Bericht zu | Schemafeld | Hinzufügen als |
|---|---|---|
| Aktion | `agenticExperience.tools.name` | Dimension |
| Fehlgeschlagene Aktionsaufrufe | `agenticExperience.tools.failed` | Dimension |
| Grund des Fehlschlagens | `agenticExperience.tools.errorReason` | Dimension |
| Plattform | `agenticExperience.agents.name` | Dimension |
| Benutzerabsicht | `conversation.prompt.raw.text` | Dimension |
| Konversation | `conversation.conversationID` | Dimension |
| Widget-Bezeichnung | `web.webInteraction.name` | Dimension |

Fügen Sie Textfelder wie Benutzerabsicht als Dimensionen hinzu. Ein als Metrik hinzugefügtes Textfeld kann nur gezählt werden. Wenn es nach seinen Werten aufgeschlüsselt wird, wird **Ungültig** angezeigt.

Eine Anleitung finden Sie unter [Konfigurieren einer Datenansicht](https://experienceleague.adobe.com/en/docs/analytics-platform/using/cja-dataviews/create-dataview#configure) und [Komponenten](https://experienceleague.adobe.com/en/docs/analytics-platform/using/cja-dataviews/create-dataview#components).

## Schritt 8: Überprüfen der Daten und Bereitstellen für die Produktion {#verify}

**Erforderlich für:** [!DNL Adobe Analytics] und [!DNL Customer Journey Analytics].

Rufen Sie eine Aktion in Ihrer Staging-App von einer unterstützten LLM-Plattform auf und wählen Sie einen Link in ihrem Widget aus. Siehe [Test in ChatGPT](/help/guides/test-in-chatgpt.md) oder [Test in Claude](/help/guides/test-in-claude.md). Überprüfen Sie dann jeden Service, den Sie einrichten:

| Dienst | Wo geprüft werden muss | Was zu erwarten ist |
|---|---|---|
| [!DNL Experience Platform] | **[!UICONTROL Datensatzaktivität]** für den Datensatz aus [Schritt 3](#dataset) | Neue aufgenommene und keine fehlgeschlagenen Batches |
| [!DNL Adobe Analytics] | Ein Workspace-Projekt, das die Report Suite aus [Schritt 4](#report-suite) mit einer Freiformtabelle verwendet | **[!UICONTROL Seite]** Zeilen mit dem Namen `mcp:<code identifier>`, **[!UICONTROL Benutzerspezifischer Link]** Zeilen für Widget-Interaktionen und Intent-Text in der Dimension **User Intent** |
| [!DNL Customer Journey Analytics] | Ein Workspace-Projekt, das die Datenansicht aus ([&#x200B; 7) &#x200B;](#cja) | **[!UICONTROL Ereignisse]** für Ihre Aktionsaufrufe und Widget-Interaktionen |

Jeder Service verarbeitet Daten mit einer gewissen Verzögerung, sodass neue Ereignisse nicht sofort angezeigt werden.

Informationen zum Erstellen der Berichte finden Sie [Erstellen von Projekten in Adobe Analytics](https://experienceleague.adobe.com/en/docs/analytics/analyze/analysis-workspace/build-workspace-project/create-projects), [Freiformtabellen-Übersicht](https://experienceleague.adobe.com/en/docs/analytics/analyze/analysis-workspace/visualizations/freeform-table/freeform-table) und [Erstellen von Projekten in Customer Journey Analytics](https://experienceleague.adobe.com/en/docs/analytics-platform/using/cja-workspace/build-workspace-project/create-projects).

Wenn die Daten korrekt aussehen, [&#x200B; Sie Ihre App für &#x200B;](/help/guides/deploy-your-app.md) Produktion bereitstellen.

## Einschränkungen {#limitations}

- **Widget-Interaktionen sind in viele Personen unterteilt.** Jeder Aktionsaufruf erhält eine neue ECID, sodass [!DNL Customer Journey Analytics] Widget-Interaktionen aus verschiedenen Aktionsaufrufen als verschiedene Personen zählt. Siehe [So identifizieren die Ereignisse Personen](#identities).
- **Kein Link zu Ihrer Website.** Wenn jemand einem Widget-Link zu Ihrer Website folgt, ist der Website-Besuch nicht mit der Konversation verknüpft.
- **Staging- und Produktionsfreigabeberichte.** Beide Umgebungen senden an denselben Datenstrom, und die Ereignisse zeichnen nicht auf, welche Umgebung sie gesendet hat.
- **Einige Plattformen senden keine Identität.** Aktionsaufrufe von Plattformen, die keine Benutzer-ID angeben, wie z. B. [!DNL Claude], werden in [!DNL Customer Journey Analytics] übersprungen.
- **Ein Zug pro Aktionsaufruf.** Jeder Aktionsaufruf wird als eigene Konversation aufgezeichnet, sodass eine Eingabeaufforderung, die mehrere Aktionen Trigger, als mehrere Züge zählt.

## Fehlerbehebung {#troubleshooting}

| Was Sie sehen | Was zu überprüfen ist |
|---|---|
| Der Datensatz erhält keine Batches | Sie haben die App bereitgestellt, nachdem Sie die Analytics-Einstellungen in der Umgebung gespeichert haben, die Sie testen. Die Datenstrom-ID ist korrekt. Der Datenstrom verfügt über den **[!UICONTROL Adobe Experience Platform]**-Service für Ihren Datensatz. |
| Batches sind fehlgeschlagen | Das Schema umfasst alle fünf Feldergruppen aus [Schritt 2](#schema). |
| [!DNL Adobe Analytics] hat keine Daten | Für den Datenstrom ist der **[!UICONTROL Adobe Analytics]**-Service mit der richtigen Report Suite-ID aktiviert. |
| **Seite** zeigt `mcp:<code identifier>` an, **Benutzerabsicht** und die Aktionsfelder sind leer | Ihr Handler-Repository verwendet `@adobe/llm-apps-runtime` 1.2.0 oder höher und wird nach dem Upgrade bereitgestellt. Siehe [Bevor Sie beginnen](#before-you-begin). |
| **Benutzerabsicht** ist für einige Aktionen leer | Die Verarbeitungsregel aus [Schritt 4](#processing-rule) verwendet die Kontextdatenvariablen genau wie gezeigt in Kleinbuchstaben und die richtige eVar. Für die Aktion **[!UICONTROL Benutzerabsicht erfassen]** aktiviert. Die Regel gilt nur für Daten, die nach ihrer Speicherung erfasst wurden. |
| [!DNL Customer Journey Analytics] zeigt weniger Ereignisse als der Datensatz an | **[!UICONTROL Alle neuen Daten importieren]** ist aktiviert. Ereignisse ohne primäre Identität werden übersprungen, z. B. Aktionsaufrufe von Plattformen, die keine Benutzer-ID angeben. |
| Sie können die Datenstrom-ID nicht ändern oder Analytics deaktivieren | Diese Einstellungen sind nach der Aktivierung in den Mobile-App-Einstellungen schreibgeschützt. Wenden Sie sich an das LLM Apps-Team, um Hilfe zu erhalten. Um Daten an eine andere Stelle zu leiten, ändern Sie die Report Suite oder den Datensatz im Datenstrom selbst. |

## Wie geht es weiter {#whats-next}

- [Analytics für eine Aktion konfigurieren](/help/guides/create-action.md#configure-analytics)
- [Bereitstellen der App](/help/guides/deploy-your-app.md)
