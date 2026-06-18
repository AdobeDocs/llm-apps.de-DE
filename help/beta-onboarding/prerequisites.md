---
title: Voraussetzungen für Adobe LLM-Apps
description: Was Sie vor der Onboarding-Sitzung für Adobe LLM Apps Beta einrichten müssen.
source-git-commit: 98d5590c927bf8ffad54061ee027664452c129c1
workflow-type: tm+mt
source-wordcount: '539'
ht-degree: 2%

---


# Voraussetzungen für Adobe LLM-Apps {#prerequisites-for-adobe-llm-apps}

Vergewissern Sie sich vor Ihrer Onboarding-Sitzung mit Adobe, dass Sie Folgendes eingerichtet haben. Führen Sie nach Möglichkeit die folgenden Verifizierungsschritte aus. Die Ergebnisse sagen Ihnen, wer im Raum sein muss, und nicht, ob Sie fortfahren können.

## Adobe-Entwicklerkonsole

Sie benötigen Zugriff auf [Adobe Developer Console](https://developer.adobe.com/console) mit der Rolle **Entwickler** (oder **Systemadministrator** in Ihrer Adobe IMS-Organisation. Stellen Sie sicher, dass Ihr Unternehmen Zugriff auf [[!DNL App Builder]](https://developer.adobe.com/app-builder/docs/intro_and_overview/) hat.

Zur Bestätigung navigieren Sie zu [developer.adobe.com/console](https://developer.adobe.com/console). Wenn der Schnellstartbildschirm angezeigt wird, sind Ihre Berechtigungen korrekt eingerichtet.

![Adobe Developer Console - Schnellstartbildschirm, der den Entwicklerzugriff bestätigt](/help/assets/overview/dev-console-access-granted.png)

Wenn stattdessen die Meldung **Eingeschränkter Zugriff** angezeigt wird, verfügen Sie nicht über die Rolle Entwickler . Laden Sie den Administrator Ihrer IMS-Organisation zur Onboarding-Sitzung ein.

![Adobe Developer Console - Nachricht zu eingeschränktem Zugriff](/help/assets/overview/dev-console-access-denied.png)

## [!DNL GitHub]

Sie benötigen in Ihrer Organisation ein [!DNL GitHub]-Konto mit den folgenden Berechtigungen:

- **Erstellen von Repositorys** - Sie müssen zwei Repositorys in Ihrer Organisation erstellen: eines für den Anwendungs-Code und eines für das EDS-Projekt. Zur Bestätigung navigieren Sie zu [github.com/new](https://github.com/new) - wenn Sie Ihre Organisation im Dropdown-Menü **Inhaber** auswählen können, verfügen Sie über die Berechtigung.

  ![Dropdown-Liste „Besitzer des neuen GitHub-Repositorys“ mit Auswahl der Organisation](/help/assets/overview/github-repo-owner-dropdown.png)

- **Installieren von [!DNL GitHub] Apps** - Sie benötigen die entsprechenden Berechtigungen, um eine [!DNL GitHub] App in Ihrem Unternehmen zu installieren. Siehe [Voraussetzungen für die Installation einer GitHub-App](https://docs.github.com/en/apps/using-github-apps/installing-a-github-app-from-a-third-party#requirements-to-install-a-github-app).

**Überprüfen Sie Ihre Berechtigungen vor Ihrer Onboarding-Sitzung**

Führen Sie diese Schnellprüfung durch, bevor Sie sich mit Adobe treffen. Das Ergebnis sagt einem, wer im Raum sein muss — nicht, ob man fortfahren kann.

1. Wechseln Sie zu [github.com/new](https://github.com/new), wählen Sie Ihre Organisation als Eigentümer aus und erstellen Sie ein Repository mit dem Namen `llm-apps-test`.
2. Wechseln Sie zur Installationsseite von [Adobe LLM Apps Permission Checker](https://github.com/apps/adobe-llm-apps-permission-checker/installations/new) und installieren Sie die Anwendung nur für das `llm-apps-test`-Repository.

| Ergebnis | Was es bedeutet | Aktion |
|---|---|---|
| Beide Schritte sind erfolgreich | Sie verfügen über die erforderlichen Berechtigungen | Sie sind bereit für die Onboarding-Sitzung |
| Schritt 2 zeigt **Anfrage** anstelle von **Installieren** | Keine Berechtigung zur Installation von [!DNL GitHub] Apps | Laden Sie den Administrator Ihrer [!DNL GitHub] Organisation zum Onboarding-Meeting ein. |

Löschen Sie anschließend das `llm-apps-test`-Repository und deinstallieren Sie die Berechtigungsprüfer-App in den Einstellungen Ihres Unternehmens.

## AEM Sites mit [!DNL Edge Delivery Services]

Aktions-Widgets werden auf **Adobe Experience Manager [!DNL Edge Delivery Services] (EDS) gehostet**. Ihr Unternehmen benötigt eine AEM Sites-Lizenz mit [!DNL Edge Delivery Services]. Sie müssen in Ihrer EDS **Organisation über die** Admin“ verfügen.

Um dies zu überprüfen, gehen Sie zum [EDS User Admin Tool](https://tools.aem.live/tools/user-admin/index.html), geben Sie Ihren Organisationsnamen ein, lassen Sie **Site** leer und klicken Sie auf **Benutzer abrufen**. Suchen Sie Ihr Konto in der Liste und bestätigen Sie, dass es das **admin**-Badge aufweist.

![EDS-Benutzeradministrator-Tool, das einen Benutzer mit der Administratorrolle anzeigt](/help/assets/overview/eds-user-admin.png)

Wenn Sie noch keine EDS-Organisation haben, ist keine Aktion erforderlich - eine wird während des Onboarding-Prozesses für Sie erstellt.

## LLM-Plattform (zum Testen)

Zum Testen der bereitgestellten App benötigen Sie eine unterstützte Abonnementebene, die benutzerdefinierte MCP-Apps und die Aktivierung **Entwicklermodus** ermöglicht. [!DNL ChatGPT] erfordert beispielsweise ein Abonnement **Pro**, **Business** oder **Enterprise/Edu**.
