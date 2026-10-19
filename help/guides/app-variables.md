---
title: Konfigurieren von App-Variablen und -Geheimnissen
description: Fügen Sie Ihrer Adobe-LLM-App umgebungsspezifische Variablen hinzu, lesen Sie sie in einem Aktions-Handler, stellen Sie sie bereit und beheben Sie gängige Probleme.
source-git-commit: 0f9dd0a5b1008e183eb8ab5a4c2eae34a78923bb
workflow-type: tm+mt
source-wordcount: '1170'
ht-degree: 1%
---

# Konfigurieren von App-Variablen und -Geheimnissen {#app-variables}

>[!IMPORTANT]
>
>[!DNL Adobe LLM Apps] befindet sich derzeit in Beta.
>
>Die hier gezeigten Funktionen, Workflows und Benutzeroberflächen stellen nicht unbedingt den endgültigen Status des Produkts dar. Um Beta beizutreten, senden Sie eine E-Mail an llm-apps-beta@adobe.com.

Verwenden Sie Variablen und Geheimnisse, um Ihre App ohne Hartkodierung in den Aktions-Handlern zu konfigurieren. Konfigurieren Sie beispielsweise die Produktkatalog-API-URL für die Verwendung eines Test-Services in der Staging-Umgebung und des Live-Services in der Produktion, ohne den Handler-Code zu ändern.

Variablen enthalten nicht vertrauliche Einstellungen. Geheime Daten sind für vertrauliche Werte wie API-Schlüssel und Zugriffstoken vorgesehen.

>[!IMPORTANT]
>
>Die Benutzeroberfläche unterstützt derzeit **nur Variablen**. Geheime Unterstützung ist für eine zukünftige Version geplant. Bis dahin **Kennwörter, API-Schlüssel** Zugriffstoken oder andere vertrauliche Informationen (**) NICHT gespeichert** Die Einstellungstabelle zeigt Variablenwerte an und ermöglicht es Benutzenden, diese zu kopieren.

**In diesem Handbuch:** Erfahren Sie, wie Sie eine App-Variable hinzufügen und in einem Aktions-Handler lesen. Stellen Sie die App bereit und überprüfen Sie, ob der Handler den erwarteten Wert verwendet.

## Voraussetzungen {#before-you-begin}

Sie benötigen:

- **Zugriff auf das Handler-Repository Ihrer App**, um Handler zu aktualisieren, die Variablen lesen müssen.
- **`@adobe/llm-apps-runtime`1.1.0 oder höher** in diesem Repository. Überprüfen Sie die Version, bevor Sie den unten stehenden Helper verwenden.

Um die Version zu überprüfen, führen Sie den folgenden Befehl in Ihrem Handler-Repository aus:

```bash
npm ls @adobe/llm-apps-runtime
```

Wenn die Version älter als 1.1.0 ist, aktualisieren Sie sie, übertragen und übertragen Sie `package.json` und `package-lock.json`:

```bash
npm install @adobe/llm-apps-runtime@latest
```

## Variablen verwalten {#manage-variables}

Jede Umgebung, **[!UICONTROL Staging]** oder **[!UICONTROL Produktion]**, verfügt über eigene Variablen. Überprüfen Sie **[!UICONTROL Workspace]**, bevor Sie eine Variable hinzufügen, aktualisieren oder löschen. Die Änderungen werden wirksam, wenn Sie die App das nächste Mal in dieser Umgebung bereitstellen.

### Variable hinzufügen {#add-variable-to-app}

In diesem Beispiel wird die Variable „non-sensitive“ `GREETING_PREFIX` mit dem Wert `Good day` verwendet, um die Standardbegrüßung des Handlers von `Hello` zu überschreiben. Durch das Hinzufügen einer Variablen **nicht** eine Aktion automatisch geändert werden: Der Handler **muss** die Variable lesen.

#### Schritt 1: Variable in der Benutzeroberfläche hinzufügen {#add-a-variable}

1. Öffnen Sie Ihre App und wählen **[!UICONTROL Einstellungen]** im linken Navigationsbereich aus.
2. Öffnen Sie die **[!UICONTROL „Variablen und]**&quot;.
3. Wählen Sie in **** die Option **[!UICONTROL Staging]** oder **[!UICONTROL Produktion]** aus.
4. Wählen Sie **[!UICONTROL Hinzufügen]** aus.

   ![Variablen und Geheimnisse - Leerer Staging-Arbeitsbereich mit der Schaltfläche „Hinzufügen“](/help/assets/guide-app-variables/variables-empty.png)

5. Geben Sie im Dialogfeld Folgendes ein:
   - **[!UICONTROL Name]**: `GREETING_PREFIX`.
   - **[!UICONTROL Typ]**: **[!UICONTROL Variable]** ausgewählt lassen.
   - **[!UICONTROL Wert]**: `Good day`.

   ![Variable oder Geheimnis hinzufügen - GRUSSWORT-PRÄFIX auf „Guten Tag“ gesetzt](/help/assets/guide-app-variables/add-variable-dialog.png)

6. Um die Variable zu speichern, klicken Sie auf **[!UICONTROL Hinzufügen]**.

Die Variable wird in der Tabelle mit dem **[!UICONTROL Name]**, **[!UICONTROL Type]**, **[!UICONTROL Value]** und **[!UICONTROL Last updated]** angezeigt. Um den Wert zu kopieren, verwenden Sie das angrenzende Kopiersteuerelement.

![Variablen und Geheimnisse - GRUSSWORT-PRÄFIX im Staging-Arbeitsbereich gespeichert](/help/assets/guide-app-variables/variable-added.png)

>[!IMPORTANT]
>
>Durch Speichern wird die Variable zur Konfiguration der ausgewählten Umgebung hinzugefügt. Die bereitgestellte App wird **aktualisiert** bis Sie sie erneut bereitstellen.

#### Schritt 2: Variable in Handler lesen {#use-a-variable}

Öffnen Sie `actions/<action-name>/index.js` in Ihrem Handler-Repository. Stellen Sie sicher, dass der Handler ein zweites Argument akzeptiert, `extra` und lesen Sie die Variable mit `getVariable`:

```javascript
const { getVariable } = require('@adobe/llm-apps-runtime');

module.exports = async ({ name = 'there' } = {}, extra) => {
  const prefix = getVariable(extra, 'GREETING_PREFIX') || 'Hello';

  return {
    content: [{ type: 'text', text: `${prefix}, ${name}!` }]
  };
};
```

Bei dem Wert `Good day` gibt ein Aufruf mit `name` auf `Ada` den Wert `Good day, Ada!` anstelle des `Hello, Ada!` zurück. Wenn Sie den Wert später in `Howdy` ändern, ändert sich die Grußformel nach der erneuten Bereitstellung ohne weitere Handler-Bearbeitung.

Der Name im Code **muss** mit dem Namen in der Benutzeroberfläche genau übereinstimmen. Wenn die Variable nicht festgelegt ist, gibt `getVariable` `undefined` zurück. Wählen Sie einen geeigneten Standardwert aus, wie im Beispiel, oder geben Sie einen eindeutigen Fehler zurück, wenn die Aktion die Einstellung erfordert.

>[!NOTE]
>
>Variablen sind nur in den Aktions-Handlern Ihrer App verfügbar; sie sind **nicht** automatisch für Widgets verfügbar.

Wenn Ihre Handler-Änderungen bereit sind, übertragen Sie sie in das Handler-Repository Ihrer App. Bei der nächsten Bereitstellung wird der neueste Push-Code verwendet. Weitere Informationen zum Bearbeiten von Handlern finden Sie unter [Anpassen eines generierten Handlers](/help/guides/customize-handler.md).

#### Schritt 3: Bereitstellen und Testen {#deploy-and-verify}

1. [Stellen Sie Ihre App ](/help/guides/deploy-your-app.md) derselben Umgebung bereit, die Sie in **[!UICONTROL Workspace ausgewählt haben]**.
2. Rufen Sie die Aktion von einer unterstützten LLM-Plattform aus auf, wobei `name` auf `Ada` gesetzt ist. Siehe [Testen des ChatGPT-Plug](/help/guides/test-in-chatgpt.md) oder [Testen des Claude-Connectors](/help/guides/test-in-claude.md).
3. Bestätigen Sie, dass die Antwort `Good day, Ada!` ist. Die Antwort überprüft, ob der Handler die konfigurierte Variable liest, und überschreibt die Standardbegrüßung.

Um die andere Umgebung zu konfigurieren, wählen Sie diese Umgebung in **[!UICONTROL Workspace]** aus, wiederholen Sie die Einrichtung mit dem entsprechenden Wert und stellen Sie dann die App dort bereit und testen Sie sie.

>[!NOTE]
>
>Staging- und Produktionsumgebungen haben **unabhängige Konfigurationen**. Änderungen in einer Umgebung wirken sich **auf** andere Umgebung aus. Verwenden Sie denselben Namen in beiden Umgebungen, falls erforderlich, und wählen Sie für jede Umgebung den entsprechenden Wert aus.

>[!TIP]
>
>Informationen zum lokalen Testen vor der Bereitstellung finden Sie unter [Lokale Handlerentwicklung und ](/help/reference/development.md)) und übergeben Sie die Variablen an den lokalen Server:
>
>`node server/local.js --param 'LLMA_VARIABLE_NAMES=["GREETING_PREFIX"]' --param GREETING_PREFIX=Howdy`

### Aktualisieren einer Variablen {#update-or-delete}

1. Wählen Sie das Eingabefeld in der Zeile der Variablen aus.
2. Überprüfen Sie den (**[!UICONTROL Wert) der Variablen]** geben Sie einen **[!UICONTROL Neuer Wert]** ein.

   ![GREETING_PREFIX aktualisieren — Ändern Sie den Wert von Good Day zu Howdy](/help/assets/guide-app-variables/update-variable-dialog.png)

3. Wählen Sie **[!UICONTROL Aktualisieren]** aus.
4. Stellen Sie die App erneut in derselben Umgebung bereit und überprüfen Sie das geänderte Verhalten.

Beim Aktualisieren einer Variablen wird nur ihr Wert geändert. Variablen **nicht** umbenannt werden. Um einen anderen Namen zu verwenden, löschen Sie die vorhandene Variable, fügen Sie eine neue Variable hinzu und aktualisieren Sie Ihren Handler, um den neuen Namen zu lesen.

### Löschen einer Variablen {#delete-a-variable}

1. Überprüfen, ob für eine Aktion weiterhin die Variable erforderlich ist. Aktualisieren und übertragen Sie bei Bedarf den Handler **zuerst**.
2. Wählen Sie das Steuerelement zum Löschen in der Zeile der Variablen aus. Um mehrere Einträge zu löschen, aktivieren Sie die entsprechenden Kontrollkästchen und klicken Sie auf **[!UICONTROL Löschen]**.
3. Überprüfen Sie die Namen im Bestätigungsdialogfeld und klicken Sie dann auf **[!UICONTROL Löschen]**.

   ![GREETING_PREFIX löschen — Bestätigen Sie den endgültigen Löschvorgang](/help/assets/guide-app-variables/delete-variable-dialog.png)

4. Stellen Sie die App erneut in derselben Umgebung bereit.

>[!IMPORTANT]
>
>Sie **das** nicht rückgängig machen. Vergewissern Sie sich, dass Sie die richtige Variable ausgewählt haben. Die bereitgestellte App behält ihre vorhandene Konfiguration bis zur nächsten Bereitstellung bei. Nach dieser Bereitstellung **Handler die** Variable nicht mehr erhalten, sodass eine Aktion, für die die Variable erforderlich ist, fehlschlagen kann.

## Regeln und Beschränkungen {#good-to-know}

| Element | Regel |
|------|------|
| Name | Bis zu 64 Zeichen: Großbuchstaben, Ziffern und Unterstriche, nicht beginnend mit einer Ziffer. **Muss** pro App und Umgebung eindeutig sein und mit dem -Handler übereinstimmen. |
| Reservierte Namen | Namen, die mit `LLMA_` und `MCP_SERVER_URL` beginnen. |
| Wert | **Erforderlich**, bis zu 500 Zeichen. Die Plattform entfernt führende und nachgestellte Leerzeichen. |
| Beschränkung | 50 Variablen pro App und Umgebung. |
| Sichtbarkeit | Variablenwerte sind sichtbar und können kopiert werden. Die Unterstützung geplanter geheimer Daten hält gespeicherte Werte ausgeblendet. |
| Änderungen | Wird bei der nächsten Bereitstellung in der ausgewählten Umgebung wirksam. |

## Fehlerbehebung {#verify-configuration}

| Was Sie sehen | Vorgehensweise |
|--------------|------------|
| **[!UICONTROL Hinzufügen]** ist deaktiviert und auf der Seite wird angezeigt, dass das **[!UICONTROL Workspace-Limit erreicht wurde]** | Löschen Sie nicht mehr benötigte Variablen. |
| *Verwenden Sie nur Großbuchstaben, Ziffern und Unterstriche* | Benennen Sie um, z. B. `API_BASE_URL`. |
| *Dieser Name ist von der Plattform reserviert* | Wählen Sie einen Namen, der nicht mit `LLMA_` beginnt und nicht `MCP_SERVER_URL` ist. |
| *Eine Variable mit diesem Namen ist bereits vorhanden* | Aktualisieren Sie stattdessen die vorhandene Variable . |
| *Diese Variable wurde nur an anderer Stelle geändert* | Aktualisieren Sie die Seite und versuchen Sie es erneut. |
| *Variablen konnten nicht geladen werden* | Laden Sie die Seite neu. Wenn der Fehler weiterhin auftritt, überprüfen Sie, ob Sie Zugriff auf die App haben. |
| Die Aktion verwendet nicht den neuen Wert | Vergewissern Sie sich, dass Sie **nach** Speichern der Variablen bereitgestellt und dieselbe Umgebung getestet haben. Bestätigen Sie, dass Sie die Handler-Änderung gepusht haben und dass der Variablenname mit **exakt** übereinstimmt. **[!UICONTROL Zuletzt aktualisiert]** zeigt an, wann Sie den Wert gespeichert haben, nicht bei der Bereitstellung der App. |
| `getVariable is not a function` | Ihre App verwendet eine Laufzeit, die älter als 1.1.0 ist. Aktualisieren Sie sie wie unter [Bevor Sie beginnen](#before-you-begin) und dann bereitstellen beschrieben. |
| Die Aktion schlägt nach dem Löschen einer Variablen fehl | Fügen Sie die Variable zurück oder aktualisieren Sie den Handler so, dass sie nicht mehr benötigt wird, und stellen Sie dann bereit. |

## Wie geht es weiter {#whats-next}

- [Anpassen eines generierten Handlers](/help/guides/customize-handler.md)
- [Bereitstellen der App](/help/guides/deploy-your-app.md)
