---
title: Erstellen einer Aktion
description: Erfahren Sie, wie Sie eine Aktion in der Benutzeroberfläche von LLM Apps definieren, einschließlich Metadaten, Eingabeparametern und Widget-Konfiguration.
source-git-commit: ae2748319b5401555c3a616971f5697c17e74ac3
workflow-type: tm+mt
source-wordcount: '900'
ht-degree: 1%

---


# Erstellen einer Aktion

>[!IMPORTANT]
>
>[!DNL Adobe LLM Apps] befindet sich derzeit in Beta.
>
>Die hier gezeigten Funktionen, Workflows und Benutzeroberflächen stellen nicht unbedingt den endgültigen Status des Produkts dar. Um Beta beizutreten, senden Sie eine E-Mail an llm-apps-beta@adobe.com.

Dieses Handbuch führt Sie durch die Definition einer Aktion in der [!DNL LLM Apps]-Benutzeroberfläche. Hintergrundinformationen dazu, was Aktionen sind und wie sie funktionieren, finden Sie unter [Grundlegende Konzepte](/help/overview/overview.md#actions).

## Öffnen der Seite Aktionen

Navigieren Sie **[!UICONTROL linken Seitenleiste zu]** Aktionen“ oder klicken Sie auf **Seite mit den App** Details auf Zu Aktionen wechseln . Wenn noch keine Aktionen vorhanden sind, zeigt die Seite einen leeren Status an.

![Seite „Aktionen“ - noch keine Aktionen](/help/assets/guide-create-action/actions-empty.png)

Klicken Sie auf **+ Aktion erstellen**, um das Vollbilddialogfeld zu öffnen.

## Aktionskarten

Jede Aktion wird als Karte angezeigt, die Folgendes enthält:

- Die Aktion **name** und **description**
- Ein **Widget-Vorschaubild**, das automatisch aus dem Widget generiert wird und zeigt, wie die Aktionsausgabe in der LLM-Plattform aussieht
- **Badges**: Widget-Typ (**[!UICONTROL EDS]**), Bereitstellungsstatus (**Nicht bereitgestellt**, **Im Staging bereitgestellt**, **In Produktion bereitgestellt**) **Änderungen nicht bereitgestellt** wenn die Aktion seit der letzten Bereitstellung geändert wurde, und Parameteranzahl
- Ein **Sichtbarkeit**-Umschalter - Aktiviert oder deaktiviert die Aktion am Live-Endpunkt ohne erneute Bereitstellung
- Ein **Überprüfen**-Link oben rechts, um den Aktionseditor zu öffnen

![Seite „Aktionen“ - Aktionskarten](/help/assets/guide-create-action/action-card.png)

Wenn eine oder mehrere Aktionen seit der letzten Bereitstellung geändert wurden, wird **Banner „Bereitstellung erforderlich** oben auf der Seite Aktionen angezeigt. Stellen Sie die App erneut bereit, um die Änderungen anzuwenden.

## Registerkarte „Aktion“

Das Dialogfeld enthält zwei Registerkarten: **Aktion** und **[!UICONTROL Widget-Metadaten]**.

### Grundlegende Informationen

![Aktion erstellen - Grundlegende Informationen](/help/assets/guide-create-action/action-basic-info.png)

- **Aktionsname** (erforderlich) - Die Kennung für Ihre Aktion (z. B *„Produkte*).
- **Beschreibung** (erforderlich) - eine klare Erläuterung der Aktion. Die LLM-Plattform verwendet dies, um zu entscheiden, wann Ihre Aktion aufgerufen werden soll. Beispiel: *Durchsuchen Sie den Produktkatalog nach Schlüsselwort. Gibt übereinstimmende Produkte mit Namen, Kategorie, Bild und Preis zurück.*
- **Anmerkungen** - optionale Hinweise, die das Verhalten der Aktion beschreiben:

  | Anmerkung | Beschreibung |
  |-----------|-------------|
  | **Destruktiver Hinweis** | Die Aktion ändert oder löscht Daten |
  | **idempotent** | Ein mehrmaliges Aufrufen der Aktion mit denselben Argumenten führt zum gleichen Ergebnis |
  | **Open world hint** | Die Aktion interagiert mit externen Systemen |
  | **Schreibgeschützter Hinweis** | Die Aktion liest nur Daten, schreibt nie |

  Weitere [ finden Sie unter „Referenz](/help/reference/reference-docs.md) Metadatenfelder“.

### OpenAI-Metadaten

- **Aufrufen des Statustextes** - Die Meldung, die auf der LLM-Plattform angezeigt wird, während die Aktion ausgeführt wird (maximal 64 Zeichen). Beispiel: *Produkte werden geladen …*
- **Aufgerufener Statustext** - die Meldung, die nach Abschluss der Aktion angezeigt wird (maximal 64 Zeichen). Beispiel: *products loaded.*

### Sichtbarkeits- und Eingabeparameter

**Sichtbarkeit** steuert, wo die Aktion verfügbar ist:

- **Für KI-Modell verfügbar machen** - die Aktion kann vom KI-Modell aufgerufen werden.
- **Als Widget in Programmoberfläche anzeigen** - Die Aktion rendert ein visuelles Widget.

**Eingabeparameter** sind die Werte, die die LLM-Plattform an Ihren Handler sendet. Das Modell extrahiert sie automatisch aus der Nachricht des Benutzers. Für *Produkte suchen* definieren wir:

- **category** (Zeichenfolge, optional) - Kategoriefilter zur Eingrenzung der Ergebnisse (z. B. ein Produkttyp oder eine Abteilung).
- **query** (String, optional) — Freitext-Suchbegriff.

Jeder Parameter verfügt über **Name**, **Type** (String, Number, Integer, Boolean), **Description** und ein **Required**-Kontrollkästchen. Klicken Sie auf **+**, um weitere Parameter hinzuzufügen.

Weitere Informationen finden Sie unter [Referenz: Aktionsparameter](/help/reference/reference-docs.md).

### Analytics

![Aktion erstellen - Analytics-Benutzerabsicht](/help/assets/guide-create-action/action-analytics-user-intent.png)

- **Benutzerabsicht** - Nach der Aktivierung wird [!DNL ChatGPT] aufgefordert, die Konversation zusammenzufassen, die zum Aufrufen dieser Aktion geführt hat. Diese Zusammenfassung wird erfasst und in Analytics angezeigt, sodass Sie in insight erfahren, was Benutzer beim Auslösen der Aktion versucht haben.

## Registerkarte Widget-Metadaten

Auf dieser Registerkarte wird konfiguriert, wie die visuelle Antwort der Aktion in der LLM-Plattform gerendert wird. Eine vollständige Erläuterung der Funktionsweise von Widgets finden Sie unter [Handbuch: Einrichten des Widgets (EDS)](/help/guides/widgets.md).

![Aktion erstellen — Widget-Metadaten](/help/assets/guide-create-action/widget-metadata.png)

### Widget-Informationen

- **Type** - die Widget-Technologie (derzeit **[!UICONTROL EDS]**).
- **Widget-Domain (Sandbox-Herkunft)** - Die Herkunft, in der Ihr Widget gehostet wird. Erforderlich für die Übermittlung der App an OpenAI; muss pro App eindeutig sein.
- **Bevorzugter Rahmen** - rendert das Widget innerhalb einer eingefassten Karte.

### Vorlagen-URLs

- **[!UICONTROL Skript-URL]** - der Einstiegspunkt für das Bootstrapping des Widgets, der für alle Aktionen freigegeben ist:
  `https://main--<repo>--<owner>.aem.live/scripts/aem-embed.js`
- **Widget-Einbettungs**-URL - Die EDS-Seite für diese spezifische Aktion:
  `https://main--<repo>--<owner>.aem.live/eds-widgets/<action-name>`

### Berechtigungen

Hardware- und Browser-APIs, auf die das Widget zugreifen kann:

| Berechtigung | Beschreibung |
|-----------|-------------|
| **Kamera** | Zugriff auf die Gerätekamera |
| **Mikrofon** | Zugriff auf das Gerätemikrofon |
| **Geolocation** | Zugriff auf den Standort des Benutzers |
| **Zwischenablage** | Aus der Zwischenablage lesen oder in die Zwischenablage schreiben |

### CSP-Konfiguration

![Aktion erstellen - Berechtigungen und CSP](/help/assets/guide-create-action/widget-permissions-csp.png)

Steuert, welche externen Domains der Widget-IFrame kontaktieren darf. Jede externe Domain muss explizit angegeben werden.

| Anweisung | Beschreibung |
|-----------|-------------|
| **Ressourcendomänen** | Domains für statische Assets: Bilder, Schriftarten, Skripte, Stile |
| **Verbinden von Domains** | Domains, mit denen das Widget über `fetch`, `XHR` oder `WebSocket` Kontakt aufnehmen kann |
| **Frame-Domains** | Zulässige Ursprünge für verschachtelte iFrames; Hinzufügen von Einträgen Trigger strengere App-Überprüfung von OpenAI |
| **Umleitungs-Domains** | Vertrauenswürdige Ziele für `openExternal` Umleitungs-Links ([!DNL ChatGPT]) |
| **Basis-URI-Domains** | Die `base-uri` CSP-Direktive (nur MCP Apps SDK, nicht unterstützt von [!DNL ChatGPT]) |

Klicken Sie **Neue Aktion erstellen**, um zu speichern.

## Nach dem Erstellen einer Aktion

Ihre Aktion wird als Karte auf der Seite Aktionen angezeigt:

![Seite „Aktionen“ - Aktion erstellt](/help/assets/guide-create-action/actions-with-action.png)

Jede Karte zeigt den Aktionsnamen, die Beschreibung, den Abzeichentyp (**[!UICONTROL EDS]**), den Bereitstellungsstatus (**Nicht bereitgestellt**) und die Anzahl der Parameter an. Sie können auf **…** klicken, um die Konfiguration zu bearbeiten oder zu löschen, oder auf **Überprüfen** klicken, um sie zu überprüfen.

![App-Details - nicht bereitgestellt](/help/assets/guide-create-action/app-detail-not-deployed.png)

Die Aktionsmetadaten werden gespeichert, es wurde jedoch noch kein Code bereitgestellt. Damit die Aktion funktioniert, ist Folgendes erforderlich:

1. **Einrichten des EDS-Widgets** - siehe [Anleitung: Einrichten des Widgets (EDS)](/help/guides/widgets.md).
2. **Den Handler schreiben** - siehe [Handbuch: Den Aktionshandler schreiben](/help/guides/write-action-handler.md).
3. **[!UICONTROL Bereitstellen]** - siehe [Anleitung: Bereitstellen Ihrer App](/help/guides/deploy-your-app.md).

## Nächste Schritte

- [Anleitung: Einrichten des Widgets (EDS)](/help/guides/widgets.md)
