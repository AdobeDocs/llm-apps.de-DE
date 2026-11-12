---
title: Konfigurieren von ChatGPT-Erweiterungen
description: Konfigurieren Sie reine Chat-GPT-Einstiegspunkte, Aktionssymbole, Anzeigevoreinstellungen und Composer-Erwähnungen mit Anforderungen und Fehlerbehebung.
source-git-commit: caf0a247e481e1b6e833ade63c5b243a5b7b22ec
workflow-type: tm+mt
source-wordcount: '1710'
ht-degree: 0%
---

# Konfigurieren von ChatGPT-Erweiterungen {#chatgpt-extensions}

>[!IMPORTANT]
>
>[!DNL Adobe LLM Apps] befindet sich derzeit in Beta.
>
>Die hier gezeigten Funktionen, Workflows und Benutzeroberflächen stellen nicht unbedingt den endgültigen Status des Produkts dar. Um Beta beizutreten, senden Sie eine E-Mail an llm-apps-beta@adobe.com.

**Hierbei handelt es sich um reine ChatGPT-Erweiterungen.** Sie konfigurieren keine Funktionen in [!DNL Claude] oder anderen LLM-Plattformen. Ihre App kann über ihre Standardaktionen und Widgets weiterhin auf diesen Plattformen funktionieren.

Verwenden Sie **[!UICONTROL ChatGPT-Erweiterungen]**, damit Benutzer Ihre App direkt öffnen, neben einer Konversation ablegen, unterstützte Dateien anzeigen oder beim Erstellen einer Nachricht Elemente suchen können.

**In diesem Handbuch** Erfahren Sie, wie Sie diese optionalen Funktionen konfigurieren und was Benutzer damit tun können. Ein Produktkatalog ist das Beispiel. Aktivieren Sie nur die Funktionen, die Ihre App benötigt.

Die roten Konturen in den Screenshots heben die in den Schritten verwendeten Steuerelemente hervor. Es handelt sich um Dokumentationsanmerkungen, nicht um Fehlerindikatoren der Benutzeroberfläche.

## Voraussetzungen {#before-you-begin}

Sie benötigen:

- eine vorhandene LLM-App, die Berechtigung zum Bearbeiten der Aktionen und eine Arbeitsaktion für die Aufgabe.
- Ein veröffentlichtes Widget zum direkten Öffnen oder Anzeigen von Dateien. Nur-Text-Aktionen können eine Erwähnung der Suche, aber keine Einstiegspunkte bereitstellen.
- `@adobe/llm-apps-runtime` 1.3.0 und für die Widget-Funktionen `@adobe/llmapps-sdk` 0.2.1 oder höher unterstützte Versionen. Bitten Sie Ihren App-Betreuer, beide Versionen zu bestätigen und die bereitgestellten Widget-Skripte bei Bedarf zu aktualisieren.
- Eine verbundene App und ein Client, der die ausgewählte Erweiterung unterstützt.

>[!IMPORTANT]
>
>Die Verfügbarkeit variiert je nach Client. Dateieinstiegspunkte sind **nur für Desktops** Composer-Erwähnungen erfordern **Desktop oder den Arbeitsbrowser**. Die Spalte Web-Support von OpenAI schließt klassische [!DNL ChatGPT] aus. Überprüfen Sie [Plattformunterstützung](https://github.com/openai/mcp-extensions/blob/main/docs/spec.md#platform-support) vor dem Testen.

>[!IMPORTANT]
>
>Mit den Einstellungen **weder** Datei-Viewer erstellt noch die Suche implementiert. Lassen Sie Ihren Betreuer zuerst die erforderlichen [Aktion](/help/guides/customize-handler.md) und [Widget](/help/guides/widgets.md) vorbereiten.

## ChatGPT-Erweiterungen öffnen {#open-extensions}

1. Öffnen Sie Ihre App und wählen Sie **[!UICONTROL Aktionen]** aus.
1. Öffnen Sie die Aktion, die Sie konfigurieren möchten.
1. Wählen Sie die **[!UICONTROL ChatGPT-Erweiterungen]** aus.

Die Registerkarte ist auch während der Erstellung einer Aktion verfügbar. Dieses Handbuch verwendet eine vorhandene, überprüfte Aktion.

Wählen Sie **[!UICONTROL Bearbeitung]**&#x200B;Änderungen speichern“ aus. Um die Konfiguration zu veröffentlichen, folgen Sie [App bereitstellen](/help/guides/deploy-your-app.md) und [aktualisieren Sie die ChatGPT-Verbindung](/help/guides/test-in-chatgpt.md#refresh-after-app-changes).

## Auswählen, wo Benutzer Ihre App öffnen {#configure-entrypoints}

Ein **Einstiegspunkt** ist ein Ort, an dem Benutzer Ihre App öffnen, ohne das Modell zu bitten, sie aufzurufen.

>[!NOTE]
>
>Sie können mehrere Einstiegspunkte für dasselbe Widget auswählen. Einstiegspunkte bieten zusätzliche Möglichkeiten zum Öffnen; sie ersetzen nicht den normalen Chat-Aufruf. Wenn die Aktion für das Modell verfügbar ist, können Personen weiterhin im Chat danach fragen und sein Widget erhalten.

1. Geben **[!UICONTROL unter &quot;]**&quot; optional eine öffentliche HTTPS-Bild-URL in **[!UICONTROL Symbol-URL]** ein. Das Symbol wird von den Einstiegspunkten dieser Aktion gemeinsam genutzt. Verwenden Sie ein Bild, das Sie veröffentlichen dürfen.
1. Wählen **[!UICONTROL unter „Wo Benutzer diese App öffnen können]** eine beliebige Kombination der folgenden Kontrollkästchen für dasselbe Widget aus. Lassen Sie die Option Alle deaktiviert, wenn die Aktion nicht direkt geöffnet werden muss.

   | Typ | Ergebnis |
   |---|---|
   | **[!UICONTROL Global]** | Katalog im Vollbildmodus von der Navigation aus öffnen |
   | **[!UICONTROL Thread]** | Katalog als Registerkarte neben einer Konversation öffnen |
   | **[!UICONTROL Datei]** | Öffnen Sie übereinstimmende Dateien in Ihrem benutzerdefinierten Viewer |

1. Bestätigen Sie für „Global“ oder „Thread“, dass die Aktion ohne erforderliche Eingaben funktioniert.
1. Führen Sie für die Datei die Einrichtung unten aus.

Wählen Sie beispielsweise sowohl global als auch Thread aus, um dasselbe Katalog-Widget in der Navigation oder neben einer Konversation zu öffnen. Sie können auch Datei auswählen, wenn das Widget die Dateianzeige unterstützt. Wenn Datei mit „Global“ oder „Thread“ kombiniert wird, lassen Sie **[!UICONTROL Erforderlich]** für die `file` Eingabe deaktiviert: Dateistarts liefern weiterhin das Dateiobjekt, aber „Global“ und „Thread-Launch“ ohne Eingaben. Handler und Widget müssen beide Fälle unterstützen. Für eine Aktion „Nur Datei“ kann `file` erforderlich sein.

![ChatGPT-Erweiterungen - Global und Thread für die Startseiten-Aktion ausgewählt, mit nicht aktivierter Datei und einer freigegebenen optionalen Symbol-URL](/help/assets/guide-chatgpt-extensions/global-thread-entrypoints.png)

**Erwartetes Ergebnis:** Öffnen Sie den Katalog in der Navigation für „global“ oder als unterhaltungsseitige Registerkarte für „Thread“.

### Aus der Navigation oder einer Konversation öffnen {#test-global-thread}

Die folgenden vom Benutzer bereitgestellten Screenshots veranschaulichen das Dashboard **Startseite** anstelle des Katalogbeispiels. Navigation und Menüplatzierung können je nach Client variieren.

Für **Global** öffnen Sie das Navigationsmenü und wählen Sie den Einstiegspunkt Ihrer App aus. In diesem Beispiel lautet der Einstiegspunkt **Home**.

![Vollständiges ChatGPT-Fenster mit geöffnetem Navigationsmenü und hervorgehobener Startseite als globaler Einstiegspunkt](/help/assets/guide-chatgpt-extensions/global-entrypoint-menu.png)

Vergewissern Sie sich, dass das Dashboard im Vollbildmodus geöffnet wird und die Navigationsleiste weiterhin verfügbar ist.

![Vollständiges ChatGPT-Fenster mit dem vom globalen Einstiegspunkt geöffneten Startseiten-Dashboard](/help/assets/guide-chatgpt-extensions/global-entrypoint-open.png)

Öffnen **für** Thread“ das Menü Konversation, wählen Sie **[!UICONTROL Plug-ins]** aus und wählen Sie den Einstiegspunkt aus. Bestätigen Sie, dass das Widget neben der Konversation geöffnet wird, ohne dass eine neue Eingabeaufforderung erforderlich ist.

![Vollständiges ChatGPT-Fenster mit Seitenleiste, verschwommener Konversation und Home-Dashboard daneben geöffnet](/help/assets/guide-chatgpt-extensions/thread-entrypoint-open.png)

### Öffnen Sie eine unterstützte Datei in Ihrer App {#file-entrypoint}

Für einen vorbereiteten CSV-Katalog-Viewer:

1. Aktivieren Sie das Kontrollkästchen **[!UICONTROL Datei]**, wählen Sie **[!UICONTROL Erweiterung hinzufügen]** aus und geben Sie `.csv` ein. Fügen Sie jede unterstützte Erweiterung in einem eigenen Feld hinzu.
1. Bestätigen **[!UICONTROL unter]** > **[!UICONTROL Eingabeparameter]** einen **Objekt** Parameter namens `file`, der `name` und `resourceUri` akzeptiert. Wenn Sie Datei auswählen, wird eine optionale Objekteingabe hinzugefügt, wenn kein `file` vorhanden ist. Überprüfen Sie alle vorhandenen Definitionen. Es können keine weiteren Eingaben erforderlich sein. `file` optional beibehalten, wenn auch „Global“ oder „Thread“ ausgewählt ist.
1. Öffnen Sie in einem unterstützten Desktop-Client eine nicht vertrauliche `.csv`-Datei mit Ihrer App.

**Erwartetes Ergebnis:** Ihr Widget zeigt die Datei an. Das Speichern von Bearbeitungen erfordert zusätzlich eine implementierte Bearbeitungsunterstützung und eine beschreibbare Datei.

>[!IMPORTANT]
>
>Dateieinstiegspunkte unterscheiden sich von hochgeladenen Dateiparametern. Sie erhalten eine Ressourcenreferenz für geöffnete Dateien, keine Download-URL oder Datei-ID.

## Auswählen, wie die App angezeigt wird {#display-settings}

Unter **[!UICONTROL Anzeigemodus]**:

1. Legen Sie **[!UICONTROL Anzeige beim Start durch das Modell]** auf `inline`, `fullscreen` oder **[!UICONTROL Keine Voreinstellung]** fest.
1. Wählen Sie die **[!UICONTROL Unterstützte Ressourcenanzeigemodi]** des Widgets aus: Inline, Vollbild oder beides. Lassen Sie beide deaktiviert, um Host-Standardwerte zu verwenden.
1. Wählen Sie **[!UICONTROL Erste Ressourcenanzeige]**. Wenn Sie unterstützte Modi ausgewählt haben, muss der ursprüngliche Modus eingeschlossen werden.

Dies sind Voreinstellungen; [!DNL ChatGPT] wählt die endgültige Präsentation aus. Im Vollbildmodus wird keine Registerkarte für das seitliche Bedienfeld erstellt: Verwenden Sie dafür einen Thread-Einstiegspunkt.

![Anzeigemodus - Inline und Vollbild deaktiviert, Anzeige der ursprünglichen Ressource auf Host-Standard und Modellstart auf Keine Voreinstellung eingestellt](/help/assets/guide-chatgpt-extensions/display-settings.png)

Vergewissern Sie sich, dass das Widget in jedem Werbemodus weiterhin verwendet werden kann.

## Lassen Sie Menschen Ressourcen im Komponisten finden {#composer-mentions}

Bei der Erwähnung von Composer können Benutzende beim Schreiben einer Nachricht Katalogelemente suchen und auswählen. Verwenden Sie eine Aktion, die bereits auswählbare Suchergebnisse zurückgibt.

1. Öffnen Sie die Suchaktion und wählen Sie **[!UICONTROL Aktionsdetails]** aus.
1. Bestätigen Sie eine **Zeichenfolge**-Eingabe mit dem Namen `query`, die leeren Text akzeptiert, ohne andere erforderliche Eingaben.
1. Aktivieren Sie **[!UICONTROL LLM]** Einstellungen“ **[!UICONTROL Schreibgeschützter Hinweis]** und **[!UICONTROL Als Widget anzeigen in der Programmoberfläche]**. Die Suchaktion muss eigentlich schreibgeschützt sein.
1. Aktivieren Sie **[!UICONTROL ChatGPT]** Erweiterungen“ **[!UICONTROL Composer-Erwähnungen aktivieren]**.
1. Pro App ist nur ein Anbieter zulässig. Wenn Sie einen Anbieter ersetzen, deaktivieren Sie Erwähnungen auf dem alten Anbieter und speichern Sie ihn, bevor Sie den neuen Anbieter aktivieren.

**Erwartetes Ergebnis:** Suchen Sie nach einem bekannten Produkt im Composer der App, erwähnen Sie die Suche, wählen Sie sie aus und bestätigen Sie, dass in der Nachricht auf die gewünschte Ressource verwiesen wird.

Mit der Einstellung „Sichtbarkeit“ wird die Suchaktion für die App verfügbar. Der Anbieter benötigt kein eigenes visuelles Widget.

Im folgenden Beispiel wird **Search Mentions** verwendet, eine reine Textaktion, die nach synthetischen Dokumenten und nicht nach dem Katalog sucht. Die `query` Eingabe ist eine **Zeichenfolge** mit **Erforderlich** deaktiviert.

![Aktionsdetails für Suchbeschriftungen - Optionale Zeichenfolgeneingabe mit dem Namen „Abfrage“](/help/assets/guide-chatgpt-extensions/composer-mentions-query.png)

Auf der Registerkarte **[!UICONTROL ChatGPT-Erweiterungen]** der gleichen Aktion **[!UICONTROL Composer-Erwähnungen aktivieren]** ist aktiviert.

![Suchbegriffe - Composer-Erwähnungen sind für eine Aktion nur Text aktiviert](/help/assets/guide-chatgpt-extensions/composer-mentions.png)

Wenn eine andere Aktion bereits eine Erwähnungssuche bereitstellt, ist der Umschalter nicht verfügbar und eine Warnung identifiziert diese Aktion. Erwähnungen im vorhandenen Provider deaktivieren und vor der Aktivierung eines Ersatzes speichern.

![Composer erwähnt nicht verfügbar, da Search Mentions bereits eine Erwähnungssuche für die App bietet](/help/assets/guide-chatgpt-extensions/composer-mentions-conflict.png)

## Zusätzliche ChatGPT-Widget-Funktionen {#additional-widget-features}

SDK unterstützt auch ChatGPT-spezifische Deep-Links, erweiterten Auswahlkontext, bearbeitbare Entwürfe und Konversations-Targeting. Diese benötigen eine Widget-Implementierung, keinen weiteren Schalter auf dieser Registerkarte.

| Wenn Ihr Widget Folgendes bietet | Ausprobieren |
|---|---|
| Ein Link zu einem Element | Öffnen Sie den Link und bestätigen Sie, dass das beabsichtigte Produkt angezeigt wird |
| Auswahlkontext | Wählen Sie ein nicht empfindliches Produkt aus und stellen Sie eine Frage dazu |
| Ein Entwurf oder eine neue Unterhaltungssteuerung | Bestätigen Sie, dass die Nachricht an der Stelle geöffnet wird, an der sie erwartet wurde, und ein Entwurf nicht automatisch gesendet wird |

Grundlegende Widget-Nachrichten und andere standardmäßige SDK-APIs sind nicht nur ChatGPT. Fragen Sie Ihren Betreuer, welche Erweiterungen Ihr Widget implementiert. Weitere Informationen finden Sie in der [SDK-Dokumentation](https://www.npmjs.com/package/@adobe/llmapps-sdk).

## Ändern oder Entfernen einer Erweiterung {#change-or-remove}

Deaktivieren Sie auf derselben Registerkarte das Kontrollkästchen des Einstiegspunkts oder deaktivieren Sie die Option **[!UICONTROL Composer-Erwähnungen aktivieren]**. Datei löschen, wenn keine Erweiterungen mehr vorhanden sind. Um das Symbol Freigegeben zu entfernen, klicken Sie auf **[!UICONTROL Symbol Löschen]**. Um die Standardeinstellungen wiederherzustellen, wählen Sie **[!UICONTROL Keine Voreinstellung]**, löschen Sie die unterstützten Modi und wählen Sie **[!UICONTROL Host-Standard]** aus.

Beim Entfernen eines Einstiegspunkts werden weder die Aktion noch die Benutzerdateien gelöscht. Siehe [Felddefinitionen und -regeln](/help/reference/reference-docs.md#chatgpt-extension-fields).

## Fehlerbehebung {#troubleshooting}

| Was Sie sehen | Vorgehensweise |
|---|---|
| Funktion oder Steuerelemente fehlen | Überprüfen Sie [Client-Support](#before-you-begin). Bestätigen Sie bei Einstiegspunkten, dass ein Widget konfiguriert ist. Diese Erweiterungen gelten nicht für Claude oder andere Hosts |
| Berichte als inkompatible erforderliche Eingabe speichern | Globale/Thread-Bereitstellung - keine Eingaben, `file` Dateibereitstellung und Erwähnung von `query`. Bitten Sie den Betreuer, die Nichtübereinstimmung zu beheben. Entfernen Sie keine Eingabe, die die Aktion benötigt |
| Dateieinrichtung oder -anzeige schlägt fehl | Fügen Sie eine unterstützte Erweiterung hinzu, bestätigen Sie die Eingabe des `file` und verwenden Sie einen kompatiblen Desktop-Client. Das Widget muss dieses Dateiformat implementieren |
| Speichern der Datei schlägt fehl | Bestätigen der Bearbeitungsunterstützung und der Schreibberechtigung. Konflikte ausgleichen, anstatt neuere Inhalte zu überschreiben; übergroße Inhalte reduzieren |
| Symbol-URL kann nicht gespeichert werden | Geben Sie eine gültige Bild-URL oder Daten-URI ein oder löschen Sie das optionale Symbol. Öffentliche HTTPS-Bild-URL bevorzugen |
| Anzeigeeinstellung wird abgelehnt oder ignoriert | Schließen Sie den Anfangsmodus unter den unterstützten Modi ein. ChatGPT steuert die endgültige Präsentation |
| Erwähnungen können nicht aktiviert werden | Überprüfen Sie das schreibgeschützte Verhalten, die Sichtbarkeit der App und die `query`. Entfernen Sie den alten Provider, bevor Sie dessen Ersatz aktivieren. |
| Die Suche gibt keine Elemente oder einen Fehler zurück | Ein bekanntes Element ausprobieren. Leere Ergebnisse können gültig sein. Melden Sie fehlende bekannte Elemente oder Fehler dem Suchbetreuer |
| Aktionen können nicht geladen werden | Verwenden Sie **[!UICONTROL Laden von App-Aktionen wiederholen]** oder **[!UICONTROL App-Aktionen aktualisieren]** wenn angeboten |
| Erstellung des Entwurfs schlägt fehl | Überprüfen Sie den Client-Support. Senden Sie nicht automatisch eine Nachricht, die als Entwurf gedacht war |
| Gespeicherte Änderungen fehlen | In der getesteten Umgebung bereitstellen, die Verbindung aktualisieren und bestätigen, dass Widget-Änderungen veröffentlicht werden |
| Nicht unterstützte Einstellungen Einstiegspunkt-Blöcke speichern | Aktivieren Sie die unterstützten Kontrollkästchen Global, Thread oder Datei oder lassen Sie alle Kontrollkästchen deaktiviert |

Notieren Sie zur Unterstützung das Programm, die Umgebung, den Client, den fehlgeschlagenen Schritt und den sichtbaren Fehler. Schließen Sie keine Token, privaten URLs oder vertraulichen Daten ein.

## Wie geht es weiter {#whats-next}

- [Bereitstellen der App](/help/guides/deploy-your-app.md)
- [Testen der LLM-App als ChatGPT-Plug-in](/help/guides/test-in-chatgpt.md)
- [Anpassen eines generierten Widgets](/help/guides/widgets.md)
