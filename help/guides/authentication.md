---
title: Endbenutzer bei Ihrem eigenen Identitätsanbieter authentifizieren
description: Aktivieren Sie die Endbenutzerauthentifizierung für Ihre Adobe-LLM-App, damit eine unterstützte LLM-Plattform den Benutzer bei Ihrem Identitätsanbieter anmeldet, bevor geschützte Aktionen aufgerufen werden.
source-git-commit: 0f9dd0a5b1008e183eb8ab5a4c2eae34a78923bb
workflow-type: tm+mt
source-wordcount: '2136'
ht-degree: 0%
---

# Endbenutzer bei Ihrem eigenen Identitätsanbieter authentifizieren {#authentication}

>[!IMPORTANT]
>
>[!DNL Adobe LLM Apps] befindet sich derzeit in Beta.
>
>Die hier gezeigten Funktionen, Workflows und Benutzeroberflächen stellen nicht unbedingt den endgültigen Status des Produkts dar. Um Beta beizutreten, senden Sie eine E-Mail an llm-apps-beta@adobe.com.

Standardmäßig ist jede Aktion in Ihrer App öffentlich. Jede LLM-Plattform mit Ihrer MCP-Server-URL kann diese Aktionen aufrufen, und Ihr Handler kann den Endbenutzer nicht identifizieren.

Aktivieren Sie die Authentifizierung, wenn eine Aktion wissen muss, welcher Endbenutzer fragt - z. B. um seine Bestellungen, Berechtigungen oder Kontodetails zurückzugeben. Die LLM-Plattform meldet den Benutzer bei **Ihrem** Identitätsanbieter (IdP) an, sendet das daraus resultierende Zugriffstoken bei jedem Aufruf und Ihr Handler erhält die verifizierte Identität.

**In diesem Handbuch:** Erfahren Sie, wie Sie Ihren Identitätsanbieter verbinden und auswählen, welche Aktionen eine Anmeldung erfordern. Verwenden Sie die verifizierte Identität in Ihren Handlern, stellen Sie die App bereit und testen Sie die Anmeldung und geschützte Aktionen.

Die Einrichtung der Authentifizierung ist eine erweiterte Aufgabe. Schließen Sie [Infrastruktur-Setup](/help/guides/create-app.md), [Aktionen erstellen](/help/guides/creating-actions.md) ab und [stellen Sie Ihr Programm bereit](/help/guides/deploy-your-app.md).

## Funktionsweise

Sie bringen Ihren eigenen Identitätsanbieter mit. Ihre bereitgestellte App ist nur ein OAuth 2.1 **Ressourcen-Server** - sie überprüft Token, die von Ihrem Autorisierungs-Server ausgestellt wurden. Es gibt nie Token aus und speichert [!DNL Adobe] nie Ihre Client-ID oder Ihr Client-Geheimnis.

```
┌── Your identity provider ───────────────────────────────────────────────┐
│  Authorization server — you own it                                      │
│  Issues access tokens, holds the user directory, defines the scopes     │
└─────────────────────────────────────────────────────────────────────────┘
        ▲  2  user signs in, platform gets an access token
        │                                    │
        │  1  platform discovers your        │  3  every tools/call carries
        │     authorization server from      │     Authorization: Bearer <token>
        │     your app's metadata            ▼
┌── LLM platform (ChatGPT, Claude, …) ────────────────────────────────────┐
└─────────────────────────────────────────────────────────────────────────┘
                                             │
                                             ▼
┌── Your LLM App on Adobe I/O Runtime ────────────────────────────────────┐
│  Resource server — verifies the token's signature, issuer, audience,    │
│  and expiry, then enforces the auth mode you set for each action        │
│                                                                         │
│  Your handler reads the verified identity from its second argument      │
└─────────────────────────────────────────────────────────────────────────┘
```

Konfigurieren der **(pro Umgebung**. **[!UICONTROL Staging]** und **[!UICONTROL Produktion]** enthalten unabhängige Einstellungen. Überprüfen Sie die Konfiguration mit einem Entwicklungs-IDp-Mandanten, bevor Sie die Authentifizierung für **[!UICONTROL Produktion]** aktivieren.

## Voraussetzungen

- Ein OAuth 2.1- oder OpenID Connect-Identitätsanbieter, der **JWT**-Zugriffstoken ausgibt, die mit einem asymmetrischen Algorithmus signiert sind. Opake Token und HMAC-signierte Token werden nicht unterstützt. Siehe [Token-](/help/reference/authentication-reference.md#token-requirements).
- Administratorzugriff auf diesen Identitätsanbieter, um eine API und einen Client zu registrieren.
- Die Mobile App wird mindestens einmal in der Umgebung bereitgestellt, die Sie konfigurieren. Die bereitgestellte MCP-Server-URL ist der Wert, auf den Ihre Token angewendet werden müssen.

## Kopieren der Ressourcenkennung

Die „Ressourcenkennung **Ihrer App ist** MCP-Server-URL. Jedes Zugriffstoken, das Ihr Identitätsanbieter für die App ausgibt, muss diese URL als Zielgruppe benennen. Diese Zielgruppenbindung verhindert die Verwendung eines Tokens, das für einen anderen Service ausgestellt wurde.

1. Öffnen Sie die App-Detailseite.
2. Scrollen Sie zu **[!UICONTROL App testen]**.
3. Wählen Sie unter der Umgebung, die Sie konfigurieren, **[!UICONTROL URL kopieren]** aus.

![Anwendungsdetails - Kopieren Sie die Staging-MCP-Server-URL](/help/assets/guide-onboarding-agent/app-mcp-url.png)

Behalten Sie im nächsten Schritt die kopierte MCP-Server-URL für die Identity Provider-Konfiguration bei. Fügen Sie die URL ein, anstatt sie erneut einzugeben. Für die Zielgruppenprüfung ist eine exakte Zeichenfolgenübereinstimmung erforderlich, einschließlich des Pfads. Ein einzelner Zeichenunterschied führt dazu, dass die Validierung jedes Tokens fehlschlägt.

>[!NOTE]
>
>Jede Umgebung verfügt über eine eigene MCP-Server-URL und daher über eine eigene Zielgruppe. Konfigurieren Sie **[!UICONTROL Staging]** und **[!UICONTROL Produktion]** separat.

## Konfigurieren des Identitätsanbieters

Die Schritte variieren je nach Anbieter. Konfigurieren Sie die folgenden vier Elemente.

1. **Registrieren Sie Ihre App als API (Ressource).** Setzen Sie die Kennung - den Wert, den der Anbieter in den `aud` des Tokens eingibt - auf die kopierte MCP-Server-URL. Dieses Feld wird normalerweise von Anbietern *Kennung* oder *Zielgruppe* bezeichnet. Verwenden Sie einen eindeutigen Bezeichner für die App, keinen allgemeinen Wert wie `api`, um die Verwendung von Token zu verhindern, die für einen anderen Service ausgestellt wurden.
2. **Definieren Sie die Bereiche** die Ihre Aktionen schützen, z. B. `orders:read` oder `profile:read`. Verwenden Sie für jede aussagekräftige Berechtigung einen Bereich, damit jede Aktion nur den Zugriff anfordert, den sie benötigt.
3. **Unterstützung PKCE.** LLM-Plattformen senden bei jeder Autorisierungsanfrage eine `code_challenge` mit `code_challenge_method=S256`, sodass Ihr Autorisierungs-Server S256 PKCE unterstützen und `"code_challenge_methods_supported": ["S256"]` in seinen Metadaten anzeigen muss.
4. **Wählen Sie aus, wie die LLM-Plattform ihren OAuth-Client identifiziert.** Verwenden Sie Client-ID-Metadatendokumente (CIMD), wenn Ihr Anbieter die Anforderungen der Plattform, die dynamische Client-Registrierung (DCR) oder einen vorregistrierten OAuth-Client unterstützt. Geben Sie nur dann eine Client-ID und ein Client-Geheimnis an, wenn die von Ihnen gewählte Methode dies erfordert. Registrieren Sie den exakten Umleitungs-URI, der von der Plattform dokumentiert oder angezeigt wird, für die gehosteten Oberflächen von [!DNL Claude], der `https://claude.ai/api/mcp/auth_callback` ist. [!DNL ChatGPT] können je nach den angekündigten Funktionen des Anbieters einen stabilen oder Connector-spezifischen Callback verwenden. Kopieren Sie daher den angezeigten URI, anstatt ihn zu erraten. Siehe die aktuelle [ChatGPT-Authentifizierungsdokumentation](https://developers.openai.com/plugins/build/auth) und [Claude-Authentifizierungsdokumentation](https://claude.com/docs/connectors/building/authentication).

>[!IMPORTANT]
>
>Die Endpunkte Ihres Identitätsanbieters für Aussteller, JWKS, Autorisierung und Token müssen alle über öffentliches HTTPS erreichbar sein. Sowohl die LLM-Plattform als auch Ihre bereitgestellte App rufen Metadaten direkt von Ihrem Anbieter ab. Eine VPN-, IP-, Firewall- oder Web-Anwendungs-Firewall, die diese Anfragen blockiert, verhindert die Anmeldung, auch wenn die App selbst weiterhin erreichbar ist.

## Authentifizierung aktivieren

1. Wählen Sie in der linken Navigation **[!UICONTROL Einstellungen]** aus und öffnen Sie dann die Registerkarte **[!UICONTROL Authentifizierung]**.
2. Wählen Sie in **&#x200B;**&#x200B;die Option **[!UICONTROL Staging]** oder **[!UICONTROL Produktion]**.
3. Aktivieren Sie **[!UICONTROL Authentifizierung aktivieren]**.
4. Geben **[!UICONTROL unter &quot;]**&quot; Folgendes ein:
   - **[!UICONTROL Aussteller]** - Die HTTPS-Aussteller-URL Ihres Identitätsanbieters, die auch der Wert ist, den sie in den `iss` jedes Tokens eingibt. Die Plattform benötigt diese URL und veröffentlicht den Herausgeber als Autorisierungs-Server der App für die LLM-Client-Erkennung. Jede App unterstützt einen Identitätsanbieter.
   - **[!UICONTROL Unterstützte Bereiche]** - Jeder Bereich, den die Aktionen dieser App erfordern dürfen. Spiegeln Sie die Bereiche, die Sie in Ihrem Identitätsanbieter definiert haben.
5. **[!UICONTROL Erweiterte Einstellungen]** ist optional. Legen Sie **[!UICONTROL JWKS-URI]** nur fest, wenn Ihre Signierschlüssel nicht an der Stelle sind, an der die Metadaten Ihres Autorisierungsservers sie anzeigen. Andernfalls erkennt die App sie automatisch.
6. Klicken Sie auf **[!UICONTROL Speichern]**.

![Authentifizierung - Aktivierung der Authentifizierung und Vervollständigung der Kerneinstellungen](/help/assets/guide-authentication/auth-core-settings.png)

Informationen zu den Feldanforderungen finden Sie unter [Authentifizierungseinstellungen](/help/reference/authentication-reference.md#authentication-settings).

## Authentifizierungsmodus für jede Aktion auswählen

Wenn Sie die **[!UICONTROL Authentifizierung aktivieren]** ändert sich jede Aktion, die derzeit auf **[!UICONTROL Keine]** festgelegt ist, in **[!UICONTROL Erforderlich]**. Überprüfen **[!UICONTROL unter „Konfiguration pro Aktion]** diese Zuweisung und legen Sie den Modus fest, den jede Aktion benötigt:

| Modus | Verhalten |
|------|----------|
| **[!UICONTROL Ohne]** | Öffentlich Aufrufer benötigen kein Token. |
| **[!UICONTROL Erforderlich]** | Geschützt Anrufer benötigen ein gültiges Token, das alle aufgelisteten Bereiche enthält. Die Laufzeit fordert nicht authentifizierte Aufrufer zur Anmeldung auf. |
| **[!UICONTROL Optional]** | Anonym aufrufbar, aber die Aktion gibt auch an, dass sie die Anmeldung unterstützt. Ihr Handler entscheidet pro Aufruf, ob ein generisches Ergebnis bereitgestellt oder der Benutzer aufgefordert werden soll, sich für ein personalisiertes Ergebnis anzumelden. |

![Authentifizierung - Legt einen Authentifizierungsmodus und Bereiche für jede Aktion fest](/help/assets/guide-authentication/auth-per-action.png)

Aktionen, die bereits auf **[!UICONTROL Erforderlich]** oder **[!UICONTROL Optional]** festgelegt sind, behalten ihren vorhandenen Modus bei.

Fügen Sie für eine **[!UICONTROL Erforderlich]** oder **[!UICONTROL Optional]**-Aktion die **[!UICONTROL Bereiche]** erforderlichen hinzu. Jeder Bereich muss bereits in **[!UICONTROL Unterstützte Bereiche]** angezeigt werden, sodass die App jede erforderliche Berechtigung für LLM-Plattformen bekannt gibt. Die Plattform blockiert das Speichern, bis Sie eine nicht übereinstimmende Größe auflösen.

**[!UICONTROL Unterstützte Bereiche]** steuert die verfügbaren Bereiche. Wenn Sie einen Bereich aus dieser Liste entfernen, wird der Bereich sofort aus allen Aktionen entfernt, für die er erforderlich ist. Fügen Sie jeden Bereich der Liste hinzu, bevor Sie ihn einer Aktion zuweisen.

Wenn Sie **[!UICONTROL Authentifizierung für alle Aktionen verlangen]** werden **[!UICONTROL Keine]** Aktionen in **[!UICONTROL Erforderlich]** geändert, aber Aktionen, die bereits auf **[!UICONTROL Optional]** festgelegt sind, werden beibehalten. Um die Anmeldung für jede Aktion zu verlangen, ändern Sie alle verbleibenden **[!UICONTROL optional]** Zeilen einzeln in **[!UICONTROL Erforderlich]**. Wenn Sie das Kontrollkästchen deaktivieren, wird jede Aktion auf **[!UICONTROL Keine]** zurückgesetzt.

Wählen **[!UICONTROL abschließend]** Speichern“ aus. Änderungen an Authentifizierungsmodus und -umfang werden zusammen mit den Einstellungen auf Programmebene gespeichert.

>[!IMPORTANT]
>
>Wenn **[!UICONTROL Authentifizierung aktivieren]** deaktiviert wird, werden der Authentifizierungsmodus und die Bereiche jeder Aktion für die ausgewählte Umgebung gelöscht. Die Plattform behält diese Einstellungen nicht bei. Wenn Sie die Authentifizierung erneut aktivieren, werden alle Aktionen auf **[!UICONTROL Erforderlich]** gesetzt.

>[!NOTE]
>
>Wenn Sie jede Aktion auf **[!UICONTROL Keine]** setzen, wird die Authentifizierung nicht deaktiviert. In diesem Status ist keine Anmeldung erforderlich, aber die App sendet Ihren Autorisierungs-Server weiterhin an LLM-Plattformen. Ein Kunde kann eine Anmeldung anbieten, die keinen zusätzlichen Zugriff gewährt. Um die App vollständig öffentlich zu machen, deaktivieren **[!UICONTROL „Authentifizierung aktivieren]** und bereitstellen.

[!DNL ChatGPT] unterstützt gemischte Authentifizierungsmodi in einer App und wendet den Modus jeder Aktion einzeln an. Öffentliche Aktionen fordern nicht zur Anmeldung auf. Bei geschützten Aktionen wird nach Bedarf eine Eingabeaufforderung angezeigt.

[!DNL Claude] unterstützt auch gemischte Apps durch **Lazy Authentication**. Wenn **[!UICONTROL Bei Bedarf anmelden]** während der Connector-Einrichtung ausgewählt wurde, bleiben öffentliche Aktionen anonym nutzbar und die Plattform fordert zur Anmeldung auf, wenn eine Aktion **[!UICONTROL Erforderlich]** aufgerufen wird. Eine App, deren Aktionen alle **[!UICONTROL Erforderlich]** sind, muss vor der Verwendung angemeldet werden. Unter [LLM-Plattformverhalten](/help/reference/authentication-reference.md#platform-behavior) finden Sie den Unterschied zwischen einer erforderlichen Aktion und einem optionalen Handler, der die Anmeldung anfordert.

>[!NOTE]
>
>Die aktuelle -Laufzeit unterstützt dieses gemischte Authentifizierungsverhalten, das anhand von Version 1.1.5 überprüft wurde. Ältere bereitgestellte Anwendungen erfordern möglicherweise eine Aktualisierung und Neubereitstellung zur Laufzeit. Einige UI-Builds zeigen weiterhin eine alte Warnung an, dass Claude eine Authentifizierung für den gesamten Connector erfordert. Überprüfen Sie die bereitgestellte Laufzeit und verwenden Sie die aktuelle Option **[!UICONTROL Bei Bedarf anmelden]** von Claude, anstatt sich auf die Warnung zu verlassen.

## Änderung bereitstellen

Authentifizierungsänderungen werden bei der nächsten Bereitstellung dieser App wirksam. **Die App erneut bereitstellen** in der von Ihnen konfigurierten Umgebung. Siehe [Bereitstellen der App](/help/guides/deploy-your-app.md).

Ihre MCP Server URL ändert sich nicht. Aktualisieren Sie die Metadaten der Verbindung und stellen Sie sicher, dass ihre Authentifizierungseinstellungen mit der neuen Konfiguration der App übereinstimmen. Eine Plattform kann es erforderlich machen, dass Sie die Verbindung neu verbinden oder neu erstellen, wenn sich die Authentifizierungsmethode ändert.

## Lesen der Identität in Ihrem Handler

Eine verifizierte Identität erreicht Ihren Handler als zweites Argument, wenn der Aufrufer ein gültiges Token sendet, unabhängig vom Authentifizierungsmodus der Aktion. Eine **[!UICONTROL optionale]** Aktion kann diese Identität verwenden, um Ergebnisse zu personalisieren, sofern verfügbar, und andernfalls ein anonymes Ergebnis zurückzugeben.

Verwenden Sie `getAuthenticatedUser`, um den angemeldeten Benutzer zu lesen:

```javascript
const { getAuthenticatedUser } = require('@adobe/llm-apps-runtime');

module.exports = async ({ orderId }, extra) => {
  const userId = getAuthenticatedUser(extra);

  if (!userId) {
    return { content: [{ type: 'text', text: 'Sign in to see your orders.' }] };
  }

  const order = await fetchOrderForUser(userId, orderId);

  return {
    content: [{ type: 'text', text: `Order ${order.id} is ${order.status}.` }],
    structuredContent: order
  };
};
```

Die Laufzeit überprüft das Token für Sie. Für eine **[!UICONTROL Erforderlich]**-Aktion blockiert sie jeden Aufruf, dem ein gültiges Token mit den aufgelisteten Bereichen fehlt. Der Handler wird nur für einen autorisierten Aufrufer ausgeführt. Verwenden Sie `hasScope`, um das Handler-Verhalten basierend auf einer Berechtigung auszuwählen, z. B. in einer **[!UICONTROL optionalen]** Aktion.

Um die Anmeldung während eines Gesprächs anzufordern, geben Sie `extra.challengeAuth()` vom Handler zurück. Der Helper ist nur für (**[!UICONTROL )]** verfügbar:

```javascript
module.exports = async ({ signIn }, extra) => {
  if (signIn && !extra.authInfo) {
    return extra.challengeAuth({
      error: 'invalid_token',
      errorDescription: 'Sign in to see member pricing.'
    });
  }

  return {
    content: [{
      type: 'text',
      text: extra.authInfo ? await memberDeals() : await publicDeals()
    }]
  };
};
```

Verwenden Sie einen expliziten Eingabeparameter, z. B. `signIn` in diesem Beispiel, um zu entscheiden, wann eine Authentifizierung angefordert werden soll. Ziehen Sie diese Entscheidung nicht aus dem Wortlaut des Benutzers.

Stellen Sie `error` so ein, dass es der gemeldeten Bedingung entspricht. Verwenden Sie `invalid_token`, wenn der Aufrufer keine gültige Sitzung hat und sich anmelden muss, wie im Beispiel oben gezeigt. Verwenden Sie `insufficient_scope`, wenn der Aufrufer angemeldet ist, dem Token jedoch der erforderliche Bereich fehlt. Jede LLM-Plattform wählt den Prompt-Text und die Art und Weise, wie auf den Fehler-Code zu reagieren. Senden Sie den Code, der die Bedingung genau beschreibt.

Fordern Sie die Authentifizierung nur an, wenn die erforderliche Identität fehlt, wie in diesem Beispiel bei der `!extra.authInfo`. Die Anmeldung löst keine bedingungslose Herausforderung: Der Handler fordert die Authentifizierung bei jedem Aufruf erneut an.

>[!NOTE]
>
>`extra.challengeAuth()` gibt eine Tool-Ergebnisaufgabe für [!DNL ChatGPT] zurück. [!DNL Claude] benötigt stattdessen eine HTTP-Authentifizierungsproblematik, bevor der Handler ausgeführt wird. Verwenden Sie für einen Vorgang, für den die Anmeldung auf mehreren unterstützten Plattformen erforderlich ist, die Aktion **[!UICONTROL Erforderlich]**. Halten Sie eine separate **[!UICONTROL Keine]**-Aktion für das öffentliche Erlebnis bereit, falls erforderlich.

Bewahren Sie die Identität Server-seitig auf. Übergeben Sie nur die Daten, die das Widget `structuredContent` benötigt. Fügen Sie in `structuredContent` niemals ein Zugriffs-Token ein. Siehe [Anpassen eines generierten Handlers](/help/guides/customize-handler.md).

Den vollständigen Vertrag finden Sie unter [Handler-Auth-API](/help/reference/authentication-reference.md#handler-auth-api).

## Testen der geschützten App

Ihr vorhandenes Plug-in oder Connector verwendet die aktualisierte Konfiguration nach der Bereitstellung. Um eine neue Verbindung zu erstellen, folgen Sie den Anweisungen für Ihre Plattform:

### [!DNL ChatGPT]

Legen Sie im Dialogfeld **[!UICONTROL Neues Plug]** die Einstellung **[!UICONTROL Authentifizierung]** fest, um sie an die Konfiguration der App-Aktionen anzupassen:

| Aktionen Ihrer App | Auswahl |
|--------------------|--------|
| Alles auf **[!UICONTROL None]** eingestellt | **[!UICONTROL Keine Authentifizierung]** |
| Alle auf **[!UICONTROL Erforderlich]** eingestellt | **[!UICONTROL OAuth]** |
| Jede andere Kombination | **[!UICONTROL Gemischt]** |

Eine **[!UICONTROL Optional]**-Aktion akzeptiert immer anonyme Aufrufe, sodass eine App, die eine enthält, nie vollständig gated wird. Wählen Sie **[!UICONTROL Gemischt]**, auch wenn jede Aktion auf **[!UICONTROL Optional]** gesetzt ist. Nur **[!UICONTROL Erforderlich]** lehnt nicht authentifizierte Aufrufe ab.

Siehe [Testen des ChatGPT](/help/guides/test-in-chatgpt.md)Plug-ins für den Rest des Dialogfelds.

### [!DNL Claude]

Fügen Sie den benutzerdefinierten Connector hinzu und überprüfen Sie die Authentifizierungseinstellungen. Wählen Sie für eine gemischte öffentliche/geschützte App **[!UICONTROL Bei Bedarf anmelden]** wenn verfügbar. Um die Anmeldung vor der Verwendung des Connectors anzufordern, wählen Sie **[!UICONTROL Jetzt anmelden]** aus. Das Dialogfeld variiert je nach Konto oder Organisation. Siehe [Testen des Claude-Connectors](/help/guides/test-in-claude.md).

### Überprüfen

- Die Plattform leitet Sie zur Anmeldeseite Ihres eigenen Identitätsanbieters weiter.
- Eine geschützte Aktion gibt benutzerspezifische Daten nach der Anmeldung zurück.
- Eine geschützte Aktion fordert Sie zur Anmeldung auf, wenn Sie abgemeldet sind.
- In einer gemischten App reagiert eine Aktion mit dem Wert **[!UICONTROL Keine]** weiterhin ohne Anmeldung. Überprüfen Sie in [!DNL Claude] die öffentliche Aktion mit **[!UICONTROL Bei Bedarf anmelden]**.
- Wiederholen Sie nach der Anmeldung die geschützte Aktion und vergewissern Sie sich, dass die erwarteten Berechtigungen von der Laufzeit erzwungen werden.

Wenn die Anmeldung nicht gestartet oder ein Token abgelehnt wird, finden Sie weitere Informationen unter [Fehlerbehebung](/help/reference/troubleshooting.md#authentication).

## Sicherheitsleitfaden

- Gewähren Sie den engsten Umfang, den jede Aktion benötigt. Verwenden Sie nicht bei jeder Aktion einen breiten Bereich.
- Behalten Sie Ihr Client-Geheimnis bei Ihrem Identitätsanbieter und in der Connector-Konfiguration der LLM-Plattform bei. Nie in Aktionsmetadaten, Handler-Code, Widget-JavaScript oder Quellcodeverwaltung einfügen.
- Token-Ansprüche als Eingabe von einem externen System behandeln. Validieren Sie Werte aus `authInfo.extra`, bevor Sie diese Werte in einer Abfrage verwenden.
- Autorisieren und authentifizieren. Ein gültiges Token beweist die Identität des Benutzers, nicht die Berechtigung, einen bestimmten Datensatz zu lesen. Überprüfen Sie den Besitz in Ihrem Handler, bevor Sie Daten zurückgeben.
- Protokollieren Sie keine Token, vollständigen Anspruchssätze oder Benutzerkennung.
- Gibt sichere Fehler zurück. Dem Benutzer keine vorgeschalteten Identity Provider-Antworten oder Stack-Traces anzeigen.
- Richten Sie &quot;**[!UICONTROL &quot; ein und überprüfen Sie]** mit einem Nicht-Produktions-Identitätsanbieter-Mandanten, bevor Sie die Authentifizierung in &quot;**[!UICONTROL &quot;]**.

## Wie geht es weiter

- [Authentifizierungsreferenz](/help/reference/authentication-reference.md) - Felder, Token-Anforderungen und Plattformverhalten.
- [Generierten Handler anpassen](/help/guides/customize-handler.md) - Ruft eine geschützte Upstream-API aus einem Handler auf.
- [App bereitstellen](/help/guides/deploy-your-app.md).
