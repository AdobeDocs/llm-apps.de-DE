---
title: Aktions- und Widget-Felder
description: Felddefinitionen für Aktionsmetadaten, Parameter, Widgets, CSP und Berechtigungen in Adobe LLM-Apps.
source-git-commit: 9b2ec369baba5d533088d2dd4adefcb80f5c9727
workflow-type: tm+mt
source-wordcount: '1273'
ht-degree: 3%
---

# Aktions- und Widget-Felder {#action-widget-configuration}

>[!IMPORTANT]
>
>LLM-Apps stehen allen AEM Sites Cloud Service-Kunden zur Entwicklung und Verwaltung eigener Apps zur Verfügung. Sobald die Erfassung beginnt, wird jeder App-Aufruf für die AEM Sites-Inhaltsanfragen des Kunden angerechnet. Adobe gibt die Pipettierungsrate und das Startdatum vor dem Inkrafttreten der Pipettierung an.

Auf dieser Seite können Sie Felder im Aktionseditor nachschlagen. Die Erstellungsschritte finden Sie unter [Erstellen einer neuen Aktion](/help/guides/create-action.md).

## Suchen der Einstellungen

Das **[!UICONTROL Neue Aktion erstellen]** enthält die Registerkarten **[!UICONTROL Aktion]**, **[!UICONTROL Widget-]** und **[!UICONTROL ChatGPT-Erweiterungen]**.

Nach der Erstellung verwendet der Editor für gespeicherte Aktionen vier Registerkarten:

| Registerkarte | Einstellungen |
|---|---|
| **[!UICONTROL LLM-Widget-Details]** | Widget-Typ, Sandbox-Domain, Widget-Beschreibung, Vorlagen-URLs und CSP |
| **[!UICONTROL Aktionsdetails]** | Code-Kennung, Beschreibung, Eingabeparameter und **[!UICONTROL LLM-Einstellungen]** einschließlich Anmerkungen, Text zum Aufrufstatus und Sichtbarkeit |
| **[!UICONTROL Analytics]** | Benutzerabsichtserfassung und Anweisungen |
| **[!UICONTROL ChatGPT-Erweiterungen]** | Direkt öffnende Einstiegspunkte, Aktionssymbole, Anzeigevoreinstellungen und Erwähnungen von Composern |

Um Ihre Änderungen zu speichern, klicken Sie auf **[!UICONTROL Änderungen speichern]**. Erzeugte Aktionen, die auf eine Überprüfung warten, zeigen stattdessen **[!UICONTROL Als geprüft markieren]** an, wodurch auch die generierten Pull-Anforderungen oder Verzweigungen zusammengeführt werden.

## Aktionsparameter

Eingabeparameter sind die Werte, die die LLM-Plattform an Ihren Aktions-Handler sendet. Das Modell extrahiert die Werte aus der Nachricht des Benutzers und ordnet sie den konfigurierten Parametern zu.

| Eigenschaft | Beschreibung |
|----------|-------------|
| **Name** | Die Parameterkennung (z. B. `category`, `query`) |
| **Typ** | `String`, `Number`, `Integer`, `Boolean` oder `Object`; der gespeicherte Editor bietet auch `File` für Parameter hochgeladener Dateien |
| **Beschreibung** | Eine für Menschen lesbare Erklärung - Die LLM-Plattform verwendet die Erklärung, um den richtigen Wert zu extrahieren |
| **Erforderlich** | Falls aktiviert, muss das Modell diesen Parameter bereitstellen, bevor die Aktion aufgerufen wird |

### Dateiparameter

Dateiparameter sind im Aktionseditor konfigurierte Eingabefeldnamen. Wenn ein Benutzer eine Datei hochlädt, stellt der Host ein Dateiobjekt für diese Argumente bereit, normalerweise einschließlich `download_url` und `file_id`.

Die Parameter für hochgeladene Dateien unterscheiden sich von [Datei-Einstiegspunkten](/help/guides/chatgpt-extensions.md#file-entrypoint). Ein Dateieinstiegspunkt verwendet einen Objektparameter mit dem Namen `file`, der die `name` und `resourceUri` der geöffneten Datei enthält.

## Metadatenfelder

### Grundlegende Informationen

| Feld | Erforderlich | Beschreibung |
|-------|----------|-------------|
| **Aktionsname** | Ja | Anzeigename für die Aktion (z. B. *Produkte suchen*) |
| **Beschreibung** | Ja | Erläuterung der Funktionsweise der Aktion - Die LLM-Plattform entscheidet anhand der Erklärung, wann die Aktion aufgerufen wird |

Im Erstellungsdialogfeld wird eine unveränderliche **Code-Kennung**, die vom Aktionsnamen abgeleitet ist, in der Vorschau angezeigt. Sie ist innerhalb der App eindeutig und enthält bis zu 63 Zeichen, beginnend mit einem Kleinbuchstaben und unter Verwendung von Kleinbuchstaben, Ziffern, Bindestrichen oder Unterstrichen. Dabei wird die Aktion dem `actions/<code-identifier>/index.js` im Handler-Repository zugeordnet.

### Anmerkungen

Optionale Hinweise, die das Verhalten der Aktion beschreiben:

| Anmerkung | Beschreibung |
|------------|-------------|
| **Destruktiver Hinweis** | Die Aktion kann Daten löschen oder überschreiben |
| **Idempotent (gleiche Argumente = kein zusätzlicher Effekt)** | Eine Wiederholung derselben Anfrage hat keine zusätzlichen Auswirkungen auf die Umgebung. Wiederholte Anfragen geben nicht unbedingt identische Daten zurück |
| **Open world hint** | Die Aktion interagiert mit externen Systemen |
| **Schreibgeschützter Hinweis** | Die Aktion liest nur Daten, schreibt nie |

### OpenAI-Metadaten

| Feld | Max. Länge | Beschreibung |
|-------|------------|-------------|
| **Aufrufen des Statustextes** | 64 Zeichen | Meldung, die auf der LLM-Plattform während der Ausführung der Aktion angezeigt wird (z. B *„Produkte werden geladen …*) |
| **Aufgerufener Statustext** | 64 Zeichen | Meldung, die nach Abschluss der Aktion angezeigt wird (z. B *„Produkte geladen …*) |
| **Widget-Beschreibung** | 512 Zeichen | Ordnet `_meta["openai/widgetDescription"]` zu. Fasst die gerenderte Komponente für das Modell zusammen und reduziert wiederholte Aussagen. |

Die Aktionsbeschreibung steuert, wann das Modell die Aktion auswählt. In der Widget-Beschreibung wird erläutert, was die Komponente nach dem Rendern anzeigt.

### Sichtbarkeit

Um eine Aktion zu aktivieren oder zu deaktivieren, verwenden Sie ihren **[!UICONTROL Sichtbarkeit]** Schalter auf der App-Seite. Der Schalter steuert, ob die nächste Bereitstellung die Aktion enthält. Der Aktionseditor verfügt über keinen separaten Ein-/Ausschalter.

Die folgenden Einstellungen des Aktionseditors steuern die Modell- und Widget-Belichtung, nicht den Ein-/Aus-Status auf Programmebene:

| Umschalter | Beschreibung |
|--------|-------------|
| **KI-Modell bereitstellen** | Die Aktion kann vom KI-Modell während Konversationen aufgerufen werden |
| **Als Widget in Programmoberfläche anzeigen** | Die Aktion rendert ein visuelles Widget in der App |

Die Einstellungen für die Modell- und Widget-Belichtung ersetzen nicht die Auswahl eines Widget-Typs.

### Analytics

| Feld | Beschreibung |
|-------|-------------|
| **Benutzerabsicht erfassen** | Standardmäßig aktiviert. Fordert eine anonymisierte Zusammenfassung des Ziels an, das zu der Aktion für Analytics geführt hat |
| **Beschreibung der Benutzerabsicht** | Anweisungen zur Anforderung der Zusammenfassung, bis zu 2.000 Zeichen. Die Standardeinstellung schließt personenbezogene und vertrauliche Informationen aus. Überprüfen Sie alle Anpassungen anhand Ihrer Datenschutzrichtlinien |

## Widget-Felder

### Widget-Informationen

| Feld | Beschreibung |
|-------|-------------|
| **Typ/Widget-Typ** | Wählen Sie im Erstellungsdialogfeld **[!UICONTROL Edge Delivery Services-Block]** oder **[!UICONTROL Kein Widget]** aus. Der gespeicherte Editor kennzeichnet die Auswahl „Nur Text“ **[!UICONTROL keine interaktive Benutzeroberfläche]** |
| **Widget-Domain** | Vom Host verwendeter Sandbox-Ursprung. Die Benutzeroberfläche identifiziert sie als erforderlich für die Übermittlung der App und eindeutig pro App. Dies ist von der Widget-URL der EDS **Seite getrennt** |
| **Bevorzugter Rahmen** | Fordert einen umrandeten Container an; die Darstellung hängt vom Host ab. Verfügbar im Erstellungsdialogfeld |

### Vorlagen-URLs

| Feld | Beschreibung |
|-------|-------------|
| **[!UICONTROL Skript-URL]** | HTTPS-URL für den EDS-`scripts/aem-embed.js`. Gemeinsam genutzt über Aktionen im selben EDS-Projekt |
| **Widget-URL** | HTTPS-URL für die von der Aktion gerenderte EDS-Seite. Erzeugte Aktionen konfigurieren die URL automatisch |

## CSP-Konfiguration

Die Content Security Policy steuert, an welche externen Domains der Widget-IFrame sich wenden darf. Jede externe Domain muss explizit angegeben werden.

| Anweisung | Beschreibung |
|-----------|-------------|
| **Ressourcendomänen** | Domains für statische Assets: Bilder, Schriftarten, Skripte, Stile |
| **Verbinden von Domains** | Domains, mit denen das Widget über `fetch`, `XHR` oder `WebSocket` Kontakt aufnehmen kann |
| **Frame-Domains** | Zulässige Ursprünge für verschachtelte iFrames; Trigger strengere App-Überprüfung |
| **Umleitungs-Domains** | Vertrauenswürdige Ziele für `openExternal` Umleitungs-Links ([!DNL ChatGPT]) |
| **Basis-URI-Domains** | Die `base-uri` CSP-Direktive (nur MCP Apps SDK, nicht [!DNL ChatGPT]) |

## ChatGPT-Erweiterungsfelder {#chatgpt-extension-fields}

Diese optionalen Einstellungen sind nur für ChatGPT; sie konfigurieren weder Claude noch andere LLM-Plattformen. Die Verfügbarkeit variiert je nach ChatGPT-Client. Anforderungen und Schritte finden Sie unter [Konfigurieren von ChatGPT-Erweiterungen](/help/guides/chatgpt-extensions.md).

### Einstiegspunkte und Aktionssymbole

Einstiegspunkte erfordern ein Widget. Wählen Sie eine beliebige Kombination aus Global, Thread und Datei für dasselbe Widget aus oder lassen Sie „Alle“ deaktiviert. Global und Thread rufen die Aktion ohne Eingaben auf. Die Datei stellt nur das `file` Objekt bereit. Andere erforderliche Eingaben verhindern, dass der entsprechende Einstiegspunkt funktioniert. Wenn die Datei mit „Global“ oder „Thread“ kombiniert ist, muss `file` optional sein.

| Feld | Beschreibung |
|---|---|
| **Wo Benutzer diese App öffnen können** | Kontrollkästchen für Global (Vollbild aus der Navigation), Thread (eine konversationsseitige Inhaltsregisterkarte) und Datei (ein benutzerdefinierter Viewer für übereinstimmende Dateierweiterungen). Es können mehrere Typen ausgewählt werden |
| **Symbol URL** | Optionales Aktionssymbol, das von den jeweiligen Einstiegspunkten gemeinsam genutzt wird. Akzeptiert eine HTTP(S)-URL oder einen Daten-URI; bevorzugen Sie eine öffentliche HTTPS-Bild-URL. Das Ändern oder Löschen der URL ersetzt alle gespeicherten Symbolvarianten |
| **Dateierweiterungen** | Mindestens eine Erweiterung, z. B. `.csv`, für einen Dateieinstiegspunkt. Der Editor fügt einen führenden Punkt hinzu, wenn er ausgelassen wird |

Wenn Sie Datei auswählen, wird eine optionale Objekteingabe mit dem Namen `file` hinzugefügt, falls sie fehlt. Vorhandene Definitionen bleiben erhalten und müssen die `name` und `resourceUri` der geöffneten Datei akzeptieren.

### Voreinstellungen anzeigen

Diese Widget-Einstellungen sind von Einstiegspunkten getrennt. Der Gastgeber wählt die endgültige Präsentation aus.

| Feld | Auswahlmöglichkeiten und Standardverhalten |
|---|---|
| **Beim Start durch das Modell anzeigen** | `inline`, `fullscreen` oder **Keine Präferenz** |
| **Unterstützte Ressourcenanzeigemodi** | **Inline**, **Vollbild** oder beides. Keine der beiden Optionen lässt die Host-Standardeinstellungen in Kraft |
| **Erste Ressourcenanzeige** | **Host default**, **inline** oder **fullscreen**. Wenn unterstützte Modi ausgewählt sind, muss der anfängliche Modus einbezogen werden |

### Composer erwähnt

**Composer-Erwähnungen aktivieren** legt die Aktion zur Ressourcensuche der App fest. Nur eine Aktion pro App kann die Erwähnung der Suche ermöglichen. Die Aktion muss schreibgeschützt und in der App sichtbar sein, mit einem Zeichenfolgenparameter namens `query`, der leeren Text akzeptiert und keine anderen erforderlichen Eingaben. Ein Anbieter kann nur Text enthalten. Um die App-Sichtbarkeit zu aktivieren, muss kein Widget für den Suchanbieter konfiguriert werden.

## Berechtigungen

Widget-Berechtigungen steuern den Zugriff auf Hardware- und Browser-APIs. Die Berechtigungen sind der iframe-Berechtigungsrichtlinie zugeordnet.

Berechtigungssteuerelemente werden unter **[!UICONTROL Berechtigungen (MCP-spezifisch)]** im Erstellungsdialogfeld angezeigt. Gewähren Sie nur Funktionen, die das Widget erfordert. Es gelten auch Hostunterstützung und Benutzerzustimmung.

| Berechtigung | Beschreibung |
|------------|-------------|
| **Kamera** | Zugriff auf die Gerätekamera |
| **Mikrofon** | Zugriff auf das Gerätemikrofon |
| **Geolocation** | Zugriff auf den Standort des Benutzers |
| **Zwischenablage** | Aus der Zwischenablage lesen oder in die Zwischenablage schreiben |
