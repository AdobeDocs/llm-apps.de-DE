---
title: Authentifizierungsreferenz
description: Felddefinitionen, Token-Anforderungen, Discovery-Endpunkte, Handler-API und das LLM-Plattformverhalten für die Endbenutzer-Authentifizierung in Adobe LLM-Apps.
source-git-commit: 0f9dd0a5b1008e183eb8ab5a4c2eae34a78923bb
workflow-type: tm+mt
source-wordcount: '1428'
ht-degree: 2%
---

# Authentifizierungsreferenz {#authentication-reference}

>[!IMPORTANT]
>
>[!DNL Adobe LLM Apps] befindet sich derzeit in Beta.
>
>Die hier gezeigten Funktionen, Workflows und Benutzeroberflächen stellen nicht unbedingt den endgültigen Status des Produkts dar. Um Beta beizutreten, senden Sie eine E-Mail an llm-apps-beta@adobe.com.

Auf dieser Seite können Sie Authentifizierungsfelder und Verträge nachschlagen. Anweisungen zum Einrichten finden Sie unter [Endbenutzer bei Ihrem eigenen Identitätsanbieter authentifizieren](/help/guides/authentication.md).

## Authentifizierungseinstellungen {#authentication-settings}

Öffnen Sie **[!UICONTROL Einstellungen]** > **[!UICONTROL Authentifizierung]**. Jede Umgebung speichert ihre eigenen Einstellungen. Wählen Sie mit der **[!UICONTROL Workspace]**-Auswahl die zu bearbeitende Umgebung aus. Das Speichern der Einstellungen wirkt sich nicht auf die andere Umgebung aus.

| Feld | Erforderlich | Beschreibung |
|-------|----------|-------------|
| **[!UICONTROL Arbeitsbereich]** | — | Umgebung für die Authentifizierungseinstellungen: **[!UICONTROL Staging]** oder **[!UICONTROL Produktion]** |
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

Nachfolgende Schrägstriche bei **[!UICONTROL Aussteller]** werden normalisiert. Der `iss` toleriert den Unterschied, sodass Token eines Anbieters, die immer einen Schrägstrich enthalten, weiterhin validiert werden.

## Authentifizierungsmodi {#auth-modes}

Festgelegt pro Aktion unter **[!UICONTROL Konfiguration pro Aktion]**.

| Modus | Token erforderlich | Handler erhält Identität | An die Plattform als |
|------|----------------|---------------------------|-------------------------------|
| **[!UICONTROL Ohne]** | Nein | Nur wenn der Aufrufer ein gültiges Token bereitstellt | `noauth` |
| **[!UICONTROL Erforderlich]** | Ja, mit jedem aufgelisteten Umfang | Immer | `oauth2` |
| **[!UICONTROL Optional]** | Nein | Wenn ein gültiges Token vorhanden ist | `noauth` und `oauth2` |

Der Handler einer **[!UICONTROL Erforderlich]**-Aktion wird nie ohne ein gültiges Token mit korrektem Bereich ausgeführt. Der Handler einer **[!UICONTROL Optional]**-Aktion wird immer ausgeführt und kann die Anmeldung selbst mit `extra.challengeAuth()` anfordern.

**[!UICONTROL Erforderlich]** ist daher der einzige Modus, der nicht authentifizierte Aufrufer zurückweist. Eine App lehnt alle anonymen Aufrufe nur ab, wenn jede Aktion **[!UICONTROL Erforderlich]** ist. Wenn eine Aktion **[!UICONTROL Keine]** oder **[!UICONTROL Optional]** ist, verwendet die App eine gemischte Authentifizierung, da mindestens eine Aktion anonyme Aufrufe akzeptiert.

Authentifizierungsmodi werden nur wirksam, wenn **[!UICONTROL Authentifizierung aktivieren]** aktiviert ist. Änderungen werden bei der nächsten Bereitstellung der App wirksam.

Durch das Ändern des Authentifizierungsschalters werden auch die Modi für die einzelnen Aktionen neu geschrieben:

| Switch change | Auswirkungen auf die Modi pro Aktion |
|---------------|----------------------------|
| Aus bis ein | Jede **[!UICONTROL Keine]**-Aktion wird **[!UICONTROL Erforderlich]**. Aktionen **[!UICONTROL Erforderlich]** oder **[!UICONTROL Optional]** behalten ihren Modus bei |
| An zu Aus | Der Modus und die Bereiche jeder Aktion werden für diese Umgebung gelöscht. Die Konfiguration wird nicht wiederhergestellt, wenn Sie den Schalter erneut einschalten |

Beim Wechsel zu **[!UICONTROL Workspace]** werden Modi nie neu geschrieben. Die Auswahl lädt die gespeicherte Konfiguration der anderen Umgebung, ohne sie zu ändern.

Wenn **[!UICONTROL Authentifizierung aktivieren]** und jede Aktion auf **[!UICONTROL Keine]** festgelegt ist, erfordert die App keine Authentifizierung, veröffentlicht aber dennoch ihren Autorisierungs-Server zur Erkennung. Um die App vollständig öffentlich zu machen, schalten Sie sie aus.

Eine App kann für einzelne Aktionen verschiedene Authentifizierungsmodi verwenden. Siehe [LLM-Plattformverhalten](/help/reference/authentication-reference.md#platform-behavior), wie sie von den einzelnen Plattformen angewendet werden.

## Token-Anforderungen {#token-requirements}

Ihr Identitätsanbieter muss Zugriffstoken ausgeben, die alle folgenden Anforderungen erfüllen. Die Laufzeit behandelt ein Token, das bei jeder Überprüfung als fehlend fehlschlägt: Der Aufrufer ist nicht authentifiziert, und eine Aktion **[!UICONTROL Erforderlich]** fordert den Aufrufer zur Anmeldung auf.

| Anforderung | Detail |
|-------------|--------|
| Format | Signiertes JWT. Opake Token werden nicht unterstützt |
| Signaturalgorithmus | `RS256`, `RS384`, `RS512`, `ES256`, `ES384`, `ES512`, `PS256`, `PS384` oder `PS512`. HMAC-Algorithmen wie `HS256` werden abgelehnt |
| `iss` | Muss übereinstimmen **[!UICONTROL Aussteller]** |
| `aud` | Muss die Ressourcenkennung der App enthalten - die MCP-Server-URL für diese Umgebung |
| `exp` | Muss in der Zukunft sein |
| `scope` oder `scp` | Die Laufzeit liest eine durch Leerzeichen getrennte `scope` Zeichenfolge oder greift auf ein Array von Zeichenfolgen in `scp` zurück. Verwenden Sie eine dieser Formen für Bereiche, die anhand der Anforderungen jeder Aktion überprüft werden |
| `sub` | Die Benutzerkennung, die Ihr Handler liest, `getAuthenticatedUser` |
| Transport | `Authorization: Bearer <token>`-Header |

Alle zusätzlichen flachen Ansprüche, die Ihr Anbieter enthält - z. B. `tenant` oder `email` -, werden an Ihren Handler weitergeleitet. Verschachtelte Objekte werden entfernt, und lange Zeichenfolgenwerte werden abgeschnitten.

## Identitätsanbietererkennung {#discovery}

Ihre App veröffentlicht ihre eigenen [RFC 9728](https://datatracker.ietf.org/doc/html/rfc9728) geschützten Ressourcenmetadaten, damit LLM-Plattformen Ihren Autorisierungs-Server finden können. Die geschützten Ressourcenmetadaten müssen nicht erstellt, gehostet oder konfiguriert werden.

Ihr Identitätsanbieter muss die folgenden Erkennungsinformationen bereitstellen:

| Anforderung | Detail |
|-------------|--------|
| Autorisierungs-Server-Metadaten | Ihr Autorisierungs-Server muss Discovery-Metadaten für LLM-Clients veröffentlichen. Sofern nicht **[!UICONTROL JWKS URI]** festgelegt ist, sucht die Laufzeit dieser App speziell nach [RFC 8414](https://datatracker.ietf.org/doc/html/rfc8414) OAuth-Autorisierungs-Server-Metadaten, um `jwks_uri` zu finden. Sie fällt nicht auf den OIDC-Erkennungspfad zurück |
| Ein Aussteller mit einem Pfad | Das bekannte Segment geht vor dem Pfad, nicht nach ihm. Ein Emittent unter `https://auth.example.com/oauth2/default` stellt seine Metadaten unter `https://auth.example.com/.well-known/oauth-authorization-server/oauth2/default` bereit |
| Direct Signing Key URL | Legen Sie **[!UICONTROL JWKS-URI]** fest, wenn die Schlüssel an einer anderen Stelle gehostet werden oder der Anbieter nur die OIDC-Erkennung bereitstellt. Dies umgeht die Laufzeiterkennung der Schlüssel, nicht die von LLM-Clients erforderliche Erkennung |

## Handler-Authentifizierungs-API {#handler-auth-api}

Das Paket `@adobe/llm-apps-runtime` exportiert die folgenden Helper. Jeder Helper akzeptiert `extra`, das zweite Argument, das Ihr Handler erhält.

| Helfer | Rückgabe |
|--------|---------|
| `getAuthenticatedUser(extra)` | Der `sub` Anspruch des angemeldeten Benutzers oder der `undefined`, wenn der Aufruf nicht authentifiziert wird |
| `hasScope(extra, scope)` | `true`, wenn das Token des Aufrufers `scope` enthält |

Die verifizierten Token-Informationen sind in `extra.authInfo` verfügbar, das für einen nicht authentifizierten Aufruf `undefined` wird.

| Eigenschaft | Beschreibung |
|----------|-------------|
| `authInfo.token` | Das rohe Bearer-Token. Nicht protokollieren oder an den Client zurückgeben |
| `authInfo.clientId` | die `client_id` oder `azp` Forderung oder `unknown` |
| `authInfo.scopes` | Array von zugewiesenen Bereichen |
| `authInfo.expiresAt` | Token-Ablauf, wie der `exp` behauptet |
| `authInfo.resource` | Die Ressourcenkennung der App, anhand der das Token validiert wurde |
| `authInfo.extra` | `sub` plus alle anderen Pauschalansprüche, die Ihr Identitätsanbieter beinhaltet |

`extra.challengeAuth(options)` ist nur für **[!UICONTROL optionale]** Aktionen verfügbar. Geben Sie das Ergebnis von Ihrem Handler zurück, um den Benutzer zu bitten, sich anzumelden, anstatt Inhalte zurückzugeben.

Der Helper gibt eine Tool-Ergebnisaufgabe für [!DNL ChatGPT] zurück. [!DNL Claude] erfordert eine HTTP-Herausforderung auf Transportebene. Die Laufzeit stellt diese Herausforderung für **[!UICONTROL erforderliche]** Aktionen bereit, jedoch nicht über den optionalen Handler-Helper.

| Option | Beschreibung |
|--------|-------------|
| `error` | Ein [RFC 6750](https://datatracker.ietf.org/doc/html/rfc6750) Bearer-Fehler-Code: `invalid_token`, `insufficient_scope` oder `invalid_request`. Standardwert ist `insufficient_scope` |
| `errorDescription` | Die dem Benutzer angezeigte Nachricht. Standardmäßig wird eine allgemeine Anmeldeaufforderung angezeigt |
| `scope` | Durch Leerzeichen getrennte Bereiche für Anfragen. Lassen Sie sie weg, damit die Plattform auf die unterstützten Bereiche der App zurückgreifen kann |

>[!IMPORTANT]
>
>Immer explizit `error`. Verwenden Sie `invalid_token` für einen Aufrufer ohne gültige Sitzung und `insufficient_scope` nur für einen Aufrufer, dessen Token gültig ist, aber keinen erforderlichen Gültigkeitsbereich hat. Die Laufzeit übergibt den Wert an die LLM-Plattform, die den Wortlaut der Anmeldeaufforderung auswählt. Senden Sie mit Ihrer bevorzugten Eingabeaufforderung den Code, der die Bedingung genau beschreibt, anstatt den Code.

## LLM-Plattformverhalten {#platform-behavior}

Beide Plattformen unterstützen öffentliche und geschützte Aktionen in derselben App, aber ihre Anmelde-Trigger unterscheiden sich.

| Verhalten | [!DNL ChatGPT] | [!DNL Claude] |
|----------|----------------|---------------|
| Gemischte Authentifizierung | Öffentliche Aktionen arbeiten anonym. **[!UICONTROL Erforderlich]** Aktionen fordern die Anmeldung an | Bei **[!UICONTROL Bei Bedarf anmelden]** funktionieren öffentliche Aktionen anonym und ein geschützter Aufruf beginnt mit der Anmeldung |
| App mit jeder Aktion **[!UICONTROL erforderlich]** | Konfigurieren der OAuth-Authentifizierung | Authentifizierung vor der Verwendung erforderlich |
| Connector-Setup | Verwenden Sie **[!UICONTROL Keine]**) für eine öffentliche App, **[!UICONTROL OAuth]** wenn jede Aktion **[!UICONTROL Erforderlich]** ist, und **[!UICONTROL Gemischt]** andernfalls, wenn diese Optionen angeboten werden | Überprüfen Sie die erkannten Einstellungen. Wählen Sie **[!UICONTROL Bei Bedarf anmelden]** für gemischte Apps oder **[!UICONTROL Jetzt anmelden]** für die Vorabanmeldung aus. |
| Fehlende Identität oder erforderlicher Umfang | Die Laufzeit gibt die Authentifizierungsprobleme der Plattform zurück | Die Laufzeit gibt HTTP 401 für fehlende/ungültige Identität oder HTTP 403 mit einer Herausforderung unzureichender Reichweite für ein gültiges Token ohne erforderlichen Bereich zurück |
| Optionale Handler-`extra.challengeAuth()` | Kann die erneute Authentifizierung starten | Eine erfolgreiche HTTP-Ergebnisantwort startet die Anmeldung nicht. Verwenden **[!UICONTROL Aktion „Erforderlich]** für ein mobiles Anmeldetor |

Siehe [Claude-Lazy](https://claude.com/docs/connectors/building/lazy-authentication)Authentifizierung und [ChatGPT-](https://developers.openai.com/plugins/build/auth) für aktuelles Client-Verhalten. Die Connector-Benutzeroberfläche und -Richtlinie können je nach Konto und Organisation variieren.


## Verwandt

- [Endbenutzer bei Ihrem eigenen Identitätsanbieter authentifizieren](/help/guides/authentication.md)
- [Aktions- und Widget-Felder](/help/reference/reference-docs.md)
- [Fehlerbehebung](/help/reference/troubleshooting.md#authentication)
