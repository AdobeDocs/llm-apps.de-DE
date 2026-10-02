---
title: Endbenutzer bei Ihrem eigenen Identitätsanbieter authentifizieren
description: Aktivieren Sie die Endbenutzerauthentifizierung für Ihre Adobe-LLM-App, damit eine unterstützte LLM-Plattform den Benutzer bei Ihrem Identitätsanbieter anmeldet, bevor geschützte Aktionen aufgerufen werden.
source-git-commit: fd41dbcabc4db0cae766de19cb042d7c85d8b7aa
workflow-type: tm+mt
source-wordcount: '2149'
ht-degree: 0%
---

# Endbenutzer bei Ihrem eigenen Identitätsanbieter authentifizieren {#authentication}

>[!IMPORTANT]
>
>[!DNL Adobe LLM Apps] befindet sich derzeit in Beta.
>
>Die hier gezeigten Funktionen, Workflows und Benutzeroberflächen stellen nicht unbedingt den endgültigen Status des Produkts dar. Um Beta beizutreten, senden Sie eine E-Mail an llm-apps-beta@adobe.com.

Standardmäßig ist jede Aktion in Ihrer App öffentlich: Jede LLM-Plattform mit Ihrer MCP-Server-URL kann sie aufrufen, und Ihr Handler kann den Endbenutzer nicht identifizieren.

Aktivieren Sie die Authentifizierung, wenn eine Aktion wissen muss, welcher Endbenutzer fragt - z. B. um seine Bestellungen, Berechtigungen oder Kontodetails zurückzugeben. Die LLM-Plattform meldet den Benutzer bei **Ihrem** Identitätsanbieter (IdP) an, sendet das daraus resultierende Zugriffstoken bei jedem Aufruf und Ihr Handler erhält die verifizierte Identität.

**Journey:** Kopieren Sie die Ressourcenkennung → konfigurieren Sie Ihren Identitätsanbieter → aktivieren Sie die Authentifizierung → legen Sie einen Authentifizierungsmodus für jede Aktion fest, → Sie die Identität in Ihrem Handler bereitstellen → lesen → die geschützte App testen.

Dies ist eine erweiterte Verzweigung, die nicht Teil der Erstausführung von Journey ist. Schließen Sie [Erste App automatisch erstellen](/help/guides/create-app.md) ab und [Bereitstellen der App](/help/guides/deploy-your-app.md).

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

Die Authentifizierung wird **pro Umgebung** konfiguriert. **[!UICONTROL Staging]** und **[!UICONTROL Produktion]** enthalten unabhängige Einstellungen, sodass Sie die Konfiguration mit einem Entwicklungs-IDp-Mandanten vergleichen können, bevor Sie sie auf **[!UICONTROL Produktion]** aktivieren.

## Voraussetzungen

- Ein OAuth 2.1- oder OpenID Connect-Identitätsanbieter, der **JWT**-Zugriffstoken ausgibt, die mit einem asymmetrischen Algorithmus signiert sind. Opake Token und HMAC-signierte Token werden nicht unterstützt. Siehe [Token-](/help/reference/authentication-reference.md#token-requirements).
- Administratorzugriff auf diesen Identitätsanbieter, damit Sie eine API und einen Client registrieren können.
- Die Mobile App wird mindestens einmal in der Umgebung bereitgestellt, die Sie konfigurieren. Die bereitgestellte MCP-Server-URL ist der Wert, auf den Ihre Token angewendet werden müssen.

## Kopieren der Ressourcenkennung

Die „Ressourcenkennung **Ihrer App ist** MCP-Server-URL. Jedes Zugriffstoken, das Ihr Identitätsanbieter für diese App ausgibt, muss diese exakte URL als Zielgruppe benennen - diese Bindung verhindert, dass ein Token, das für einen anderen Service geprägt wurde, erneut für Ihre App angezeigt wird.

1. Öffnen Sie die App-Detailseite.
2. Scrollen Sie zu **[!UICONTROL App testen]**.
3. Wählen Sie unter der Umgebung, die Sie konfigurieren, **[!UICONTROL URL kopieren]** aus.

![Anwendungsdetails - Kopieren Sie die Staging-MCP-Server-URL](/help/assets/guide-onboarding-agent/app-mcp-url.png)

Behalten Sie diesen Wert bei: Sie benötigen ihn im nächsten Schritt bei Ihrem Identitätsanbieter. Fügen Sie die kopierte URL ein, anstatt sie erneut einzugeben. Bei der Zielgruppenprüfung handelt es sich um eine exakte Zeichenfolgenübereinstimmung, einschließlich einer Pfadkomponente, sodass ein einzelner Zeichenunterschied dazu führt, dass die Validierung jedes Tokens fehlschlägt.

>[!NOTE]
>
>Jede Umgebung verfügt über eine eigene MCP-Server-URL und daher über eine eigene Zielgruppe. Konfigurieren Sie **[!UICONTROL Staging]** und **[!UICONTROL Produktion]** separat.

## Konfigurieren des Identitätsanbieters

Die genauen Schritte unterscheiden sich je nach Anbieter, aber jeder Anbieter benötigt die gleichen vier Dinge.

1. **Registrieren Sie Ihre App als API (Ressource).** Setzen Sie die Kennung - den Wert, den der Anbieter in den `aud` des Tokens eingibt - auf die kopierte MCP-Server-URL. Anbieter kennzeichnen dieses Feld unterschiedlich, häufig *Kennung* oder *Zielgruppe*. Verwenden Sie keinen generischen Wert wie `api`. Die Kennung muss für diese App eindeutig sein, sonst kann ein für einen anderen Service ausgestelltes Token für diese App wiederholt werden.
2. **Definieren Sie die Bereiche** mit denen Sie Aktionen steuern möchten, z. B. `orders:read` oder `profile:read`. Verwenden Sie für jede aussagekräftige Berechtigung einen Bereich, sodass eine Aktion nur das anfordert, was sie benötigt.
3. **Unterstützung PKCE.** LLM-Plattformen senden bei jeder Autorisierungsanfrage eine `code_challenge` mit `code_challenge_method=S256`, sodass Ihr Autorisierungs-Server S256 PKCE unterstützen und `"code_challenge_methods_supported": ["S256"]` in seinen Metadaten anzeigen muss.
4. **Zulassen, dass sich die LLM-Plattform als Client registriert.** Unterstützte LLM-Plattformen erstellen einen eigenen OAuth-Client für Ihren Autorisierungs-Server, aktivieren also die dynamische Client-Registrierung, wenn Ihr Anbieter sie anbietet. Erstellen Sie andernfalls manuell einen öffentlichen Client und geben Sie während der Connector-Einrichtung auf der Plattform dessen Client-ID - und das Geheimnis - nur an, wenn Ihr Anbieter eine Authentifizierung mit einem vertraulichen Client erfordert. Registrieren Sie den Umleitungs-URI in den Platform-Dokumenten für die gehosteten Oberflächen von [!DNL Claude], die `https://claude.ai/api/mcp/auth_callback` werden. Einige Plattformen geben für jeden Connector, den der Benutzer erstellt, einen eigenen Umleitungs-URI aus, [!DNL ChatGPT] unter ihnen. Lesen Sie daher den Wert aus dem Connector-Setup-Bildschirm und registrieren Sie ihn vor der ersten Anmeldung. Ein nicht registrierter Umleitungs-URI führt dazu, dass Ihr Autorisierungs-Server die Autorisierungsanfrage vollständig ablehnt.

>[!IMPORTANT]
>
>Die Endpunkte Ihres Identitätsanbieters für Aussteller, JWKS, Autorisierung und Token müssen alle über öffentliches HTTPS erreichbar sein. Sowohl die LLM-Plattform als auch die bereitgestellte App rufen Metadaten direkt von Ihrem Provider ab, sodass ein Identitätsanbieter, der sich hinter einem VPN oder einer IP-Zulassungsliste befindet, die Anmeldung nicht abschließen kann. Eine Firewall oder eine Firewall in der Web-Anwendung vor dem Provider ist eine häufige Ursache und kann den Fluss unterbrechen, auch wenn die App selbst erreichbar ist.

## Authentifizierung aktivieren

1. Wählen Sie in der linken Navigation **[!UICONTROL Einstellungen]** aus und öffnen Sie dann die Registerkarte **[!UICONTROL Authentifizierung]**.
2. Wählen Sie in **** die Option **[!UICONTROL Staging]** oder **[!UICONTROL Produktion]**.
3. Aktivieren Sie **[!UICONTROL Authentifizierung aktivieren]**.
4. Geben **[!UICONTROL unter &quot;]**&quot; Folgendes ein:
   - **[!UICONTROL Aussteller]** - Die Aussteller-URL Ihres Identitätsanbieters, die auch der Wert ist, den sie in den `iss` jedes Tokens eingibt. Dies ist erforderlich, muss HTTPS sein und wird auch als Autorisierungs-Server Ihrer App veröffentlicht, damit LLM-Plattformen ermitteln können, wohin Benutzer gesendet werden sollen. Pro App wird nur ein Identitätsanbieter unterstützt.
   - **[!UICONTROL Unterstützte Bereiche]** - Jeder Bereich, den die Aktionen dieser App erfordern dürfen. Spiegeln Sie die Bereiche, die Sie in Ihrem Identitätsanbieter definiert haben.
5. **[!UICONTROL Erweiterte Einstellungen]** ist optional. Legen Sie **[!UICONTROL JWKS-URI]** nur fest, wenn Ihre Signierschlüssel nicht an der Stelle sind, an der die Metadaten Ihres Autorisierungsservers sie anzeigen. Andernfalls erkennt die App sie automatisch.
6. Klicken Sie auf **[!UICONTROL Speichern]**.

![Authentifizierung - Aktivierung der Authentifizierung und Vervollständigung der Kerneinstellungen](/help/assets/guide-authentication/auth-core-settings.png)

Informationen dazu, was jedes Feld akzeptiert, finden Sie unter [Authentifizierungseinstellungen](/help/reference/authentication-reference.md#authentication-settings).

## Authentifizierungsmodus für jede Aktion auswählen

Wenn Sie die **[!UICONTROL Authentifizierung aktivieren]** ändert sich jede Aktion, die derzeit auf **[!UICONTROL Keine]** festgelegt ist, in **[!UICONTROL Erforderlich]**. Überprüfen **[!UICONTROL unter „Konfiguration pro Aktion]** diese Zuweisung und legen Sie den Modus fest, den jede Aktion benötigt:

| Modus | Verhalten |
|------|----------|
| **[!UICONTROL Ohne]** | Öffentlich Die Aktion kann ohne Token aufgerufen werden. |
| **[!UICONTROL Erforderlich]** | Geschlossen. Die Aktion kann nur mit einem gültigen Token aufgerufen werden, das jeden von Ihnen aufgelisteten Bereich enthält. Nicht authentifizierte Anrufer werden aufgefordert, sich anzumelden. |
| **[!UICONTROL Optional]** | Anonym aufrufbar, aber die Aktion gibt auch an, dass sie die Anmeldung unterstützt. Ihr Handler entscheidet pro Aufruf, ob ein generisches Ergebnis bereitgestellt oder der Benutzer aufgefordert werden soll, sich für ein personalisiertes Ergebnis anzumelden. |

![Authentifizierung - Legt einen Authentifizierungsmodus und Bereiche für jede Aktion fest](/help/assets/guide-authentication/auth-per-action.png)

Aktionen, die bereits auf **[!UICONTROL Erforderlich]** oder **[!UICONTROL Optional]** festgelegt sind, behalten ihren vorhandenen Modus bei.

Fügen Sie für eine **[!UICONTROL Erforderlich]** oder **[!UICONTROL Optional]**-Aktion die **[!UICONTROL Bereiche]** erforderlichen hinzu. Jeder Bereich muss bereits oben unter **[!UICONTROL Unterstützte Bereiche]** angezeigt werden. Andernfalls benötigt die App eine Berechtigung, die sie nicht an LLM-Plattformen weitergibt. Das Speichern ist blockiert, bis die Diskrepanz behoben ist.

**[!UICONTROL Unterstützte Bereiche]** ist die Autorität für diese Liste. Wenn Sie einen Bereich daraus entfernen, wird dieser Bereich aus jeder Aktion entfernt, für die er erforderlich ist, sobald Sie die Änderung vornehmen. Fügen Sie also zuerst einen Bereich hinzu und weisen Sie ihn dann einer Aktion zu.

**[!UICONTROL Authentifizierung für alle Aktionen verlangen]** setzt jede Aktion auf **[!UICONTROL Erforderlich]**. Wenn Sie sie löschen, wird jede Aktion auf **[!UICONTROL Keine]** zurückgesetzt.

Wählen **[!UICONTROL abschließend]** Speichern“ aus. Änderungen an Authentifizierungsmodus und -umfang werden zusammen mit den Einstellungen auf Programmebene gespeichert.

>[!IMPORTANT]
>
>Wenn Sie **[!UICONTROL Authentifizierung aktivieren]** deaktivieren, wird diese Konfiguration pro Aktion für die ausgewählte Umgebung verworfen - der Modus und die Bereiche jeder Aktion werden gelöscht und nicht gespeichert. Wenn Sie sie wieder einschalten, beginnt von „all-**[!UICONTROL &quot;]**.

>[!NOTE]
>
>Wenn Sie jede Aktion auf **[!UICONTROL Keine]** setzen, wird die Authentifizierung nicht deaktiviert. In diesem Zustand wird kein Aufruf verweigert, aber die App wirbt trotzdem mit Ihrem Autorisierungs-Server auf LLM-Plattformen, sodass ein Client dem Benutzer eine Anmeldung anbieten kann, die keinen zusätzlichen Zugriff gewährt. Um die App vollständig öffentlich zu machen, deaktivieren **[!UICONTROL „Authentifizierung aktivieren]** und bereitstellen.

Das Mischen von Modi in einer App - einige öffentliche, andere gated - wird unterstützt, und [!DNL ChatGPT] wendet den Modus jeder Aktion einzeln an: Nur die gated Aktionen fordern den Benutzer zur Anmeldung auf.

>[!IMPORTANT]
>
>[!DNL Claude] ist die Ausnahme. Sie wendet die Authentifizierung pro Connector und nicht pro Aktion an. Wenn also eine Aktion in der App auf **[!UICONTROL Erforderlich]** oder **[!UICONTROL Optional]** festgelegt ist, fordert [!DNL Claude] den Benutzer auf, sich anzumelden, bevor er den Connector überhaupt verwendet, einschließlich der Aktionen, die auf **[!UICONTROL Keine]** eingestellt sind. Um eine Aktion für [!DNL Claude] Benutzer öffentlich zu halten, hosten Sie sie in einer separaten App.

## Änderung bereitstellen

Authentifizierungsänderungen werden bei der nächsten Bereitstellung dieser App wirksam. **Die App erneut bereitstellen** in der von Ihnen konfigurierten Umgebung. Siehe [Bereitstellen der App](/help/guides/deploy-your-app.md).

Ihre MCP-Server-URL ändert sich nicht, sodass alle bereits erstellten Plug-ins oder Connectoren weiterhin funktionieren. Es ist jetzt beschriftet, sodass seine Benutzer aufgefordert werden, sich bei der nächsten Verwendung anzumelden.

## Lesen der Identität in Ihrem Handler

Eine verifizierte Identität erreicht Ihren Handler als zweites Argument. Sie ist immer vorhanden, wenn der Aufrufer ein gültiges Token gesendet hat, unabhängig vom Authentifizierungsmodus der Aktion. Daher kann eine **[!UICONTROL Optional]**-Aktion ihr Ergebnis personalisieren, wenn ein Token vorhanden ist, und dennoch ein Ergebnis zurückgeben, wenn dies nicht der Fall ist.

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

Sie müssen das Token nicht selbst überprüfen. Bei einer **[!UICONTROL Erforderlich]**-Aktion blockiert die Laufzeit jeden Aufruf, dem ein gültiges Token fehlt, das die aufgeführten Bereiche enthält, sodass der Handler nur für autorisierte Aufrufer ausgeführt wird. Verwenden Sie `hasScope`, wenn Sie eine Verzweigung für eine Berechtigung erstellen möchten, anstatt sich auf das Gate zu verlassen, z. B. in einer **[!UICONTROL optionalen]** Aktion.

Eine **[!UICONTROL optionale]** Aktion kann den Benutzer bitten, sich während der Konversation anzumelden, indem `extra.challengeAuth()` zurückgegeben wird. Dies ist nur für **[!UICONTROL optionale]** Aktionen verfügbar:

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

Entscheiden Sie, ob eine Eskalation von einem expliziten Eingabeparameter aus erfolgen soll, wie `signIn` dies hier tut, anstatt den Text des Benutzers zu überprüfen.

Stellen Sie `error` so ein, dass es der von Ihnen gemeldeten Bedingung entspricht. Verwenden Sie `invalid_token`, wenn der Aufrufer keine gültige Sitzung hat und sich anmelden muss, wie im Beispiel oben, und `insufficient_scope`, wenn der Aufrufer bereits angemeldet ist, aber das Token keinen Umfang hat, den die Aktion benötigt. Die LLM-Plattform wählt den Wortlaut der Eingabeaufforderung aus, die der Benutzer sieht, und wie weit dieser Wortlaut mit diesem Wert variiert, hängt von der Plattform ab. Senden Sie daher den Code, der die Bedingung genau beschreibt.

Herausforderung nur dann, wenn die benötigte Identität wirklich fehlt, wie hier bei der `!extra.authInfo`. Ein Handler, der Herausforderungen ohne Bedingungen stellt, kann nicht durch die Anmeldung erfüllt werden, sodass der Benutzer bei jedem Aufruf aufgefordert wird, sich erneut zu authentifizieren.

>[!NOTE]
>
>Beim [!DNL ChatGPT] wird der Benutzer bei einem auf diese Weise ausgelösten Anmelden aufgefordert, den Connector erneut zu verbinden, anstatt eine zusätzliche Berechtigung zu erteilen. Beim [!DNL Claude] meldet sich der Benutzer an, bevor eine Aktion ausgeführt wird, sodass eine Aktion nie eine auslösen muss.

Bewahren Sie die Identität Server-seitig auf. Übergeben Sie nur das, was das Widget benötigt, in `structuredContent` und fügen Sie das Zugriffstoken nie dort ein - siehe [Anpassen eines generierten Handlers](/help/guides/customize-handler.md).

Den vollständigen Vertrag finden Sie unter [Handler-Auth-API](/help/reference/authentication-reference.md#handler-auth-api).

## Testen der geschützten App

Das vorhandene Plug-in oder der vorhandene Connector übernimmt die Änderung nach der Bereitstellung. So richten Sie eine neue ein:

### [!DNL ChatGPT]

Legen Sie im Dialogfeld **[!UICONTROL Neues Plug]** die Einstellung **[!UICONTROL Authentifizierung]** fest, um sie an die Konfiguration der App-Aktionen anzupassen:

| Aktionen Ihrer App | Auswahl |
|--------------------|--------|
| Alles auf **[!UICONTROL None]** eingestellt | **[!UICONTROL Keine Authentifizierung]** |
| Alle auf **[!UICONTROL Erforderlich]** eingestellt | **[!UICONTROL OAuth]** |
| Jede andere Kombination | **[!UICONTROL Gemischt]** |

![ChatGPT - Wählen Sie den Authentifizierungsmodus für das Plug-in aus](/help/assets/guide-authentication/chatgpt-authentication-mode.png)

Eine **[!UICONTROL Optional]**-Aktion akzeptiert immer anonyme Aufrufe, sodass eine App, die eine enthält, nie vollständig gated wird. Wählen Sie **[!UICONTROL Gemischt]**, auch wenn jede Aktion auf **[!UICONTROL Optional]** gesetzt ist. Nur **[!UICONTROL Erforderlich]** lehnt nicht authentifizierte Aufrufe ab.

Siehe [Testen des ChatGPT](/help/guides/test-in-chatgpt.md)Plug-ins für den Rest des Dialogfelds.

### [!DNL Claude]

Fügen Sie den benutzerdefinierten Connector hinzu, wählen Sie **[!UICONTROL Verbinden]** und schließen Sie die Anmeldung ab, die Ihr Identitätsanbieter vorstellt. Es gibt keine Authentifizierungsauswahl - [!DNL Claude] tort den gesamten Connector, wenn eine Aktion tortiert wird. Siehe [Testen des Claude-Connectors](/help/guides/test-in-claude.md).

### Überprüfen

- Die Plattform leitet Sie zur Anmeldeseite Ihres eigenen Identitätsanbieters weiter.
- Eine geschützte Aktion gibt benutzerspezifische Daten nach der Anmeldung zurück.
- Eine geschützte Aktion fordert Sie zur Anmeldung auf, wenn Sie abgemeldet sind.
- Auf [!DNL ChatGPT] reagiert eine Aktion mit **[!UICONTROL Keine]** weiterhin ohne Anmeldung. Bei [!DNL Claude] wird der gesamte Connector getast.

Wenn die Anmeldung nicht gestartet oder ein Token abgelehnt wird, finden Sie weitere Informationen unter [Fehlerbehebung](/help/reference/troubleshooting.md#authentication).

## Sicherheitsleitfaden

- Gewähren Sie den engsten Umfang, den jede Aktion benötigt. Verwenden Sie nicht bei jeder Aktion einen breiten Bereich.
- Behalten Sie Ihr Client-Geheimnis bei Ihrem Identitätsanbieter und in der Connector-Konfiguration der LLM-Plattform bei. Nie in Aktionsmetadaten, Handler-Code, Widget-JavaScript oder Quellcodeverwaltung einfügen.
- Token-Ansprüche als Eingabe von einem externen System behandeln. Validieren Sie alles, was Sie aus `authInfo.extra` lesen, bevor Sie es in einer Abfrage verwenden.
- Autorisieren und authentifizieren. Ein gültiges Token beweist, wer der Benutzer ist, nicht, dass er einen bestimmten Datensatz sehen kann - überprüfen Sie die Eigentümerschaft in Ihrem Handler, bevor Sie Daten zurückgeben.
- Protokollieren Sie keine Token, vollständigen Anspruchssätze oder Benutzerkennung.
- Gibt sichere Fehler zurück. Dem Benutzer keine vorgeschalteten Identity Provider-Antworten oder Stack-Traces anzeigen.
- Richten Sie &quot;**[!UICONTROL &quot; ein und überprüfen Sie]** mit einem Nicht-Produktions-Identitätsanbieter-Mandanten, bevor Sie die Authentifizierung in &quot;**[!UICONTROL &quot;]**.

## Wie geht es weiter

- [Authentifizierungsreferenz](/help/reference/authentication-reference.md) - Felder, Token-Anforderungen und Plattformverhalten.
- [Generierten Handler anpassen](/help/guides/customize-handler.md) - Ruft eine geschützte Upstream-API aus einem Handler auf.
- [App bereitstellen](/help/guides/deploy-your-app.md).
