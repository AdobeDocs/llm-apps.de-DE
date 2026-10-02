---
title: Authentifizierungsreferenz
description: Felddefinitionen, Token-Anforderungen, Discovery-Endpunkte, Handler-API und das LLM-Plattformverhalten für die Endbenutzer-Authentifizierung in Adobe LLM-Apps.
source-git-commit: fd41dbcabc4db0cae766de19cb042d7c85d8b7aa
workflow-type: tm+mt
source-wordcount: '1260'
ht-degree: 3%
---

# Authentifizierungsreferenz {#authentication-reference}

>[!IMPORTANT]
>
>[!DNL Adobe LLM Apps] befindet sich derzeit in Beta.
>
>Die hier gezeigten Funktionen, Workflows und Benutzeroberflächen stellen nicht unbedingt den endgültigen Status des Produkts dar. Um Beta beizutreten, senden Sie eine E-Mail an llm-apps-beta@adobe.com.

Auf dieser Seite können Sie Authentifizierungsfelder und Verträge nachschlagen. Informationen zur Setup-Journey finden Sie unter [Endbenutzer bei Ihrem eigenen Identitätsanbieter authentifizieren](/help/guides/authentication.md).

## Authentifizierungseinstellungen {#authentication-settings}

Gefunden unter **[!UICONTROL Einstellungen]** > **[!UICONTROL Authentifizierung]**. Jedes Feld wird pro Umgebung gespeichert - die Auswahl **[!UICONTROL Workspace]** wählt aus, welche Sie bearbeiten, und das Speichern wirkt sich nie auf die andere Umgebung aus.

| Feld | Erforderlich | Beschreibung |
|-------|----------|-------------|
| **[!UICONTROL Arbeitsbereich]** | — | Für welche Umgebung diese Einstellungen gelten: **[!UICONTROL Staging]** oder **[!UICONTROL Produktion]** |
| **[!UICONTROL Authentifizierung aktivieren]** | — | Master-Schalter Wenn diese Option deaktiviert ist, ist jede Aktion unabhängig vom Authentifizierungsmodus öffentlich |
| **[!UICONTROL Aussteller]** | Ja | Die Aussteller-URL Ihres Identitätsanbieters und der erwartete `iss`. Muss HTTPS sein. Wird auch als Autorisierungs-Server dieser App veröffentlicht. Ein Identitätsanbieter pro App |
| **[!UICONTROL Unterstützte Bereiche]** | Nein | Die vollständigen Bereiche, die für die Aktionen dieser App erforderlich sein können. Auf LLM-Plattformen als unterstützte Bereiche der App veröffentlicht |
| **[!UICONTROL JWKS-URI]** | Nein | Erweitert. Die HTTPS-URL Ihres Signaturschlüsselsatzes. Wird nur benötigt, wenn er sich von dem unterscheidet, mit dem die Metadaten Ihres Autorisierungsservers werben |

### Validierungsregeln

| Regel | Ergebnis |
|------|--------|
| **[!UICONTROL Aussteller]** ist leer, während **[!UICONTROL Authentifizierung aktivieren]** aktiviert ist | Speichern ist blockiert |
| **[!UICONTROL Aussteller]** oder **[!UICONTROL JWKS-URI]** ist keine HTTPS-URL | Speichern ist blockiert |
| Eine Aktion erfordert einen Bereich, der in **[!UICONTROL Unterstützte Bereiche]** fehlt | Speichern wird blockiert, bis Sie den Bereich hinzufügen oder aus der Aktion entfernen |
| Ein Bereich wird aus entfernt **[!UICONTROL Bereiche werden unterstützt]** | Es wird sofort aus jeder Aktion entfernt, die es erforderlich machte, ohne auf eine Speicherung zu warten |
| **[!UICONTROL Unterstützte Bereiche]** ist leer | Es kann kein Bereich gewährt werden, sodass ein Bereich, der bereits für eine Aktion vorhanden ist, entfernt wird. In diesem Fall wird keine Warnung angezeigt |
| `offline_access` wird in **[!UICONTROL Unterstützte Bereiche]** oder in einer Aktion aufgeführt | Wird entfernt, wenn die App bereitgestellt wird, unabhängig von der Groß-/Kleinschreibung oder dem umgebenden Leerzeichen, sodass auf der Einstellungsseite ein Bereich angezeigt werden kann, den die bereitgestellte App nicht hat. `offline_access` fordert ein Aktualisierungs-Token von Ihrem Autorisierungs-Server an, anstatt Zugriff auf diese App zu gewähren, sodass es sich nicht um einen Bereich handelt, für den diese App wirbt. Sie müssen sie nicht auflisten - die LLM-Plattform fordert sie direkt von Ihrem Autorisierungs-Server an |

Nachfolgende Schrägstriche bei **[!UICONTROL Aussteller]** werden normalisiert, und der `iss` Vergleich toleriert den Unterschied - ein Anbieter, der immer einen nachfolgenden Schrägstrich ausgibt, validiert weiterhin.

## Authentifizierungsmodi {#auth-modes}

Festgelegt pro Aktion unter **[!UICONTROL Konfiguration pro Aktion]**.

| Modus | Token erforderlich | Handler erhält Identität | An die Plattform als |
|------|----------------|---------------------------|-------------------------------|
| **[!UICONTROL Ohne]** | Nein | Nur wenn der Aufrufer ein gültiges Token bereitstellt | `noauth` |
| **[!UICONTROL Erforderlich]** | Ja, mit jedem aufgelisteten Umfang | Immer | `oauth2` |
| **[!UICONTROL Optional]** | Nein | Wenn ein gültiges Token vorhanden ist | `noauth` und `oauth2` |

Der Handler einer **[!UICONTROL Erforderlich]**-Aktion wird nie ohne ein gültiges Token mit korrektem Bereich ausgeführt. Der Handler einer **[!UICONTROL Optional]**-Aktion wird immer ausgeführt und kann die Anmeldung selbst mit `extra.challengeAuth()` anfordern.

**[!UICONTROL Erforderlich]** ist daher der einzige Modus, der nicht authentifizierte Aufrufer ablehnt. Eine App wird nur dann vollständig erfasst, wenn jede ihrer Aktionen **[!UICONTROL Erforderlich]** ist. Eine einzelne **[!UICONTROL Keine]** oder **[!UICONTROL Optional]** Aktion sorgt für eine Mischung der App, da anonyme Aufrufe für mindestens eine Aktion immer noch erfolgreich sind.

Authentifizierungsmodi sind nur gültig, wenn **[!UICONTROL Authentifizierung aktivieren]** aktiviert ist. Änderungen werden bei der nächsten Bereitstellung der App wirksam.

Durch Umschalten dieses Schalters werden die Modi für die einzelnen Aktionen neu geschrieben:

| Switch change | Auswirkungen auf die Modi pro Aktion |
|---------------|----------------------------|
| Aus bis ein | Jede **[!UICONTROL Keine]**-Aktion wird **[!UICONTROL Erforderlich]**. Aktionen **[!UICONTROL Erforderlich]** oder **[!UICONTROL Optional]** behalten ihren Modus bei |
| An zu Aus | Der Modus und die Bereiche jeder Aktion werden für diese Umgebung gelöscht. Die Konfiguration wird nicht wiederhergestellt, wenn Sie den Schalter erneut einschalten |

Beim Wechsel zu **[!UICONTROL Workspace]** werden die Modi nie neu geschrieben. Die gespeicherte Konfiguration der anderen Umgebung wird unverändert geladen.

**[!UICONTROL Authentifizierung aktivieren]** bei jeder auf &quot;**[!UICONTROL &quot; gesetzten Aktion]** eine gültige, aber inaktive Kombination: Es wird nie ein Aufruf abgelehnt, und die App veröffentlicht dennoch ihren Autorisierungs-Server zur Erkennung. Schalten Sie den Schalter aus, um die App vollständig öffentlich zu machen.

Modi können in einer App frei gemischt werden. Siehe [LLM-Plattformverhalten](/help/reference/authentication-reference.md#platform-behavior), wie sie von den einzelnen Plattformen angewendet werden.

## Token-Anforderungen {#token-requirements}

Ihr Identitätsanbieter muss Zugriffstoken ausstellen, die alle folgenden Bedingungen erfüllen. Ein Token, bei dem eine Überprüfung fehlschlägt, wird als abwesend behandelt - der Aufrufer ist nicht authentifiziert, und eine **[!UICONTROL Erforderlich]**-Aktion fordert ihn zur Anmeldung auf.

| Anforderung | Detail |
|-------------|--------|
| Format | Signiertes JWT. Opake Token werden nicht unterstützt |
| Signaturalgorithmus | `RS256`, `RS384`, `RS512`, `ES256`, `ES384`, `ES512`, `PS256`, `PS384` oder `PS512`. HMAC-Algorithmen wie `HS256` werden abgelehnt |
| `iss` | Muss übereinstimmen **[!UICONTROL Aussteller]** |
| `aud` | Muss die Ressourcenkennung der App enthalten - die MCP-Server-URL für diese Umgebung |
| `exp` | Muss in der Zukunft sein |
| `scope` oder `scp` | Durch Leerzeichen getrennte Zeichenfolge oder ein Array von Zeichenfolgen. Liefert die Bereiche, die anhand der Anforderungen der einzelnen Aktionen überprüft wurden |
| `sub` | Die Benutzerkennung, die Ihr Handler liest, `getAuthenticatedUser` |
| Transport | `Authorization: Bearer <token>`-Header |

Alle zusätzlichen flachen Ansprüche, die Ihr Anbieter enthält - z. B. `tenant` oder `email` -, werden an Ihren Handler weitergeleitet. Verschachtelte Objekte werden entfernt, und lange Zeichenfolgenwerte werden abgeschnitten.

## Identitätsanbietererkennung {#discovery}

Ihre App veröffentlicht ihre eigenen [RFC 9728](https://datatracker.ietf.org/doc/html/rfc9728) geschützten Ressourcenmetadaten, damit LLM-Plattformen Ihren Autorisierungs-Server finden können. Sie erstellen, hosten oder konfigurieren keine hierfür erforderlichen Elemente.

Sie müssen Discovery auf Ihrer eigenen Seite bereitstellen:

| Anforderung | Detail |
|-------------|--------|
| Autorisierungs-Server-Metadaten | Ihr Herausgeber muss seine eigenen [RFC 8414](https://datatracker.ietf.org/doc/html/rfc8414)-Metadaten oder [!DNL OpenID Connect] Discovery unter seinem `/.well-known/` bereitstellen. Ihre App liest es, um Ihre Signierschlüssel zu finden |
| Ein Aussteller mit einem Pfad | Das bekannte Segment geht vor dem Pfad, nicht nach ihm. Ein Emittent unter `https://auth.example.com/oauth2/default` stellt seine Metadaten unter `https://auth.example.com/.well-known/oauth-authorization-server/oauth2/default` bereit |
| anderweitig gehostete Schlüssel | Legen Sie **[!UICONTROL JWKS-URI]** fest, wenn Ihre Signaturschlüssel nicht dort gespeichert sind, wo sie von den Metadaten angekündigt werden |

## Handler-Authentifizierungs-API {#handler-auth-api}

Aus `@adobe/llm-apps-runtime` exportiert. Jeder Helper benötigt `extra`, das zweite Argument, das Ihr Handler erhält.

| Helfer | Rückgabe |
|--------|---------|
| `getAuthenticatedUser(extra)` | Der `sub` Anspruch des angemeldeten Benutzers oder der `undefined`, wenn der Aufruf nicht authentifiziert wird |
| `hasScope(extra, scope)` | `true`, wenn das Token des Aufrufers `scope` enthält |

Die unbearbeiteten Informationen zum verifizierten Token befinden sich auf `extra.authInfo`, was für einen nicht authentifizierten Aufruf `undefined` wird.

| Eigenschaft | Beschreibung |
|----------|-------------|
| `authInfo.token` | Das rohe Bearer-Token. Nicht protokollieren oder an den Client zurückgeben |
| `authInfo.clientId` | die `client_id` oder `azp` Forderung oder `unknown` |
| `authInfo.scopes` | Array von zugewiesenen Bereichen |
| `authInfo.expiresAt` | Token-Ablauf, wie der `exp` behauptet |
| `authInfo.resource` | Die Ressourcenkennung der App, anhand der das Token validiert wurde |
| `authInfo.extra` | `sub` plus alle anderen Pauschalansprüche, die Ihr Identitätsanbieter beinhaltet |

`extra.challengeAuth(options)` ist nur für **[!UICONTROL optionale]** Aktionen verfügbar. Geben Sie das Ergebnis von Ihrem Handler zurück, um den Benutzer zu bitten, sich anzumelden, anstatt Inhalte zurückzugeben.

| Option | Beschreibung |
|--------|-------------|
| `error` | Ein [RFC 6750](https://datatracker.ietf.org/doc/html/rfc6750) Bearer-Fehler-Code: `invalid_token`, `insufficient_scope` oder `invalid_request`. Standardwert ist `insufficient_scope` |
| `errorDescription` | Die dem Benutzer angezeigte Nachricht. Standardmäßig wird eine allgemeine Anmeldeaufforderung angezeigt |
| `scope` | Durch Leerzeichen getrennte Bereiche für Anfragen. Lassen Sie sie weg, damit die Plattform auf die unterstützten Bereiche der App zurückgreifen kann |

>[!IMPORTANT]
>
>Immer explizit `error`. Verwenden Sie `invalid_token` für einen Aufrufer ohne gültige Sitzung und `insufficient_scope` nur für einen Aufrufer, dessen Token gültig ist, aber keinen erforderlichen Gültigkeitsbereich hat. Der Wert wird an die LLM-Plattform übergeben, die selbst entscheidet, wie die Eingabeaufforderung für den Benutzer formuliert wird. Senden Sie den Code, der die Bedingung genau beschreibt, und nicht den Code, dessen Eingabeaufforderung Sie bevorzugen.

## LLM-Plattformverhalten {#platform-behavior}

Die Unterstützung für die Authentifizierung einzelner Aktionen variiert je nach Plattform. Konfigurieren Sie für beide auf die gleiche Weise, denn der Unterschied besteht darin, was den Benutzenden angezeigt wird.

| Verhalten | [!DNL ChatGPT] | [!DNL Claude] |
|----------|----------------|---------------|
| Granularität | Pro Aktion | Pro Connector |
| Gemischte Authentifizierung, bei der die App nicht vollständig abgerufen wird | Unterstützt. Nur die **[!UICONTROL Erforderlich]**-Aktionen fordern zur Anmeldung auf | Nicht unterstützt. Der gesamte Connector fordert zur Anmeldung auf, einschließlich der nicht aktivierten Aktionen |
| Connector-Setup | Setzen Sie **[!UICONTROL Authentifizierung]** auf **[!UICONTROL Keine]**, wenn jede Aktion **[!UICONTROL Keine]**, **[!UICONTROL OAuth]**, wenn jede Aktion **[!UICONTROL Erforderlich]** ist, und **[!UICONTROL Gemischt]** andernfalls | Keine Authentifizierungsentscheidung zu treffen; Anmeldung beginnt bei **[!UICONTROL Verbinden]** |
| Erneute Authentifizierung | Im Gespräch aufgefordert, wenn eine gated-Aktion aufgerufen wird | Aufforderung für den Connector |


## Verwandt

- [Endbenutzer bei Ihrem eigenen Identitätsanbieter authentifizieren](/help/guides/authentication.md)
- [Aktions- und Widget-Felder](/help/reference/reference-docs.md)
- [Fehlerbehebung](/help/reference/troubleshooting.md#authentication)
