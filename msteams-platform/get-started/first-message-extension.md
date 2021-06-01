---
title: Erste Schritte – Erstellen Sie Ihre erste Messaging-Erweiterung
author: adrianhall
description: Erstellen Sie mithilfe des Teams Toolkits eine Messaging-Erweiterung für Microsoft Teams.
ms.author: adhal
ms.date: 05/20/2021
ms.topic: quickstart
ms.openlocfilehash: ad341c386cc9e1bf03cf6e25c0d8be8add0880c6
ms.sourcegitcommit: 2c8b35899dd845acd66f1f927e40d99523c29a91
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2021
ms.locfileid: "52698113"
---
# <a name="build-and-run-your-first-messaging-extension-for-microsoft-teams"></a>Erstellen Sie Ihre erste Messaging-Erweiterung für Microsoft Teams, und führen Sie sie aus.

Es gibt zwei Arten von Teams-**Messaging-Erweiterungen**:

- Mit [Suchbefehlen](../messaging-extensions/how-to/search-commands/define-search-command.md) können Sie externe Systeme durchsuchen und die Ergebnisse dieser Suche in Form einer Karte in eine Nachricht einfügen.
- [Aktionsbefehle](../messaging-extensions/how-to/action-commands/define-action-command.md) ermöglichen Ihnen, Ihren Benutzern ein modales Popupfenster zum Sammeln oder Anzeigen von Informationen vorzuführen, ihre Interaktion zu verarbeiten und Informationen an Teams zurückzusenden.

In diesem Tutorial erstellen Sie einen *Suchbefehl*, um nach externen Daten zu suchen und die Ergebnisse in eine Nachricht einzufügen.  

## <a name="before-you-begin"></a>Bevor Sie beginnen

Stellen Sie sicher, dass Ihre Entwicklungsumgebung eingerichtet ist, indem Sie [Installieren erforderlicher Komponenten](prerequisites.md)

> [!div class="nextstepaction"]
> [Installieren erforderlicher Komponenten](prerequisites.md)

## <a name="create-your-project"></a>Erstellen Ihres Projekts

Verwenden Sie das Teams-Toolkit zum Erstellen Ihres ersten Projekts:

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/vscode)

1. Öffnen Sie Visual Studio Code.
1. Öffnen Sie das Teams-Toolkit, indem Sie das Teams-Symbol in der Randleiste auswählen:

    :::image type="content" source="../assets/images/teams-toolkit-v2/sidebar-icon.png" alt-text="Das Teams-Symbol in der Visual Studio Code-Randleiste.":::

1. Wählen Sie **Erstellen eines neuen Projekts** aus.

   :::image type="content" source="../assets/images/teams-toolkit-v2/create-project.png" alt-text="Standort des Links „Neues Projekt erstellen"::: in der Randleiste des Teams-Toolkits.

1. Wählen Sie **Eine neue Teams-App erstellen** aus.

   :::image type="content" source="../assets/images/teams-toolkit-v2/create-new-project-intro.png" alt-text="Assistenten für „Neues Projekt erstellen“ starten":::

1. Wählen Sie im Schritt **Funktionen auswählen** die Option **Nachrichtenerweiterung** aus und heben Sie die Auswahl **Registerkarte** auf. Drücken Sie **OK**.

   :::image type="content" source="../assets/images/teams-toolkit-v2/msgextn-create-project-capabilities.png" alt-text="Screenshot, der zeigt, wie Funktionen Ihrer neuen App hinzufügt werden können.":::

1. Wählen Sie im Schritt **Registrierung eines Bots** die Option **Eine neue Registrierung eines Bots erstellen**.

   :::image type="content" source="../assets/images/teams-toolkit-v2/create-bot-registration.png" alt-text="Wählen Sie „Eine neue Registrierung eines Bots erstellen“ aus":::

   > [!NOTE]
   > Messaging-Erweiterungen sind auf Bots angewiesen, um ein Dialogfeld zwischen Benutzer und Code zu gewährleisten.

1. Wählen Sie im Schritt **Programmiersprache** die Option **JavaScript** aus.

    :::image type="content" source="../assets/images/teams-toolkit-v2/create-project-programming-languages.png" alt-text="Screenshot, der zeigt, wie eine Programmiersprache ausgewählt werden kann.":::

1. Wählen Sie einen Arbeitsbereichsordner (optional).  Innerhalb Ihres Arbeitsbereichsordners wird für das von Ihnen erstellte Projekt ein Ordner erstellt.

1. Geben Sie einen geeigneten Namen für Ihre App ein, wie z. B. `helloworld`.  Der Name der App darf nur aus alphanumerischen Zeichen bestehen.  Drücken Sie die **EINGABETASTE**, um fortzufahren.

Ihre Teams-App wird innerhalb weniger Sekunden erstellt.

# <a name="command-line"></a>[Befehlszeile](#tab/cli)

Verwenden Sie das `teamsfx` CLI zum Erstellen Ihres ersten Projekts.  Beginnen Sie in dem Ordner, in dem Sie den Projektordner erstellen möchten.

``` bash
teamsfx new
```

Das CLI geht einige Fragen zum Erstellen des Projekts durch.  Bei jeder Frage wird angegeben, wie Sie sie beantworten können (z. B. mithilfe von Pfeiltasten, um eine Option auszuwählen).  Wenn Sie eine Frage beantwortet haben, bestätigen Sie Ihre Auswahl, indem Sie die **EINGABETASTE** drücken.

1. Wählen Sie **Eine neue Teams-App erstellen** aus.
1. Wählen Sie die Funktion **Nachrichtenerweiterung** aus und heben Sie die Auswahl der Funktion **Registerkarte** auf.
1. Wählen Sie **Eine neue Registrierung eines Bots erstellen** aus.
1. Wählen Sie **JavaScript** aks Programmiersprache aus.
1. Drücken Sie die **EINGABETASTE**, um den Standardordner des Arbeitsbereichs auszuwählen.
1. Geben Sie einen geeigneten Namen für Ihre App ein, wie z. B. `helloworld`.  Der Name der App darf nur aus alphanumerischen Zeichen bestehen.

Sobald alle Fragen beantwortet sind, wird Ihr Projekt erstellt.

---

## <a name="take-a-tour-of-the-source-code"></a>Unternehmen einer Tour durch den Quellcode

Wenn Sie diesen Abschnitt vorerst überspringen möchten, können Sie [Ihre App lokal ausführen](#run-your-app-locally).

Eine Messaging-Erweiterung verwendet das [Bot-Framework](https://docs.botframework.com), um dem Benutzer die Interaktion mit Ihrem Dienst über eine Unterhaltung zu ermöglichen.  Nach der Gerüsterstellung wird Ihr Projekt folgend aussehen:

:::image type="content" source="../assets/images/teams-toolkit-v2/msgextn-file-layout.png" alt-text="Dateilayout eines Bot-Projekts.":::

Der Bot-Code wird im `bot` Verzeichnis gespeichert.  Das `bots/messageExtensionBot.js` ist der Haupteinsteigerpunkt der Messaging-Erweiterung.

> [!Tip]
> Machen Sie sich mit Bots außerhalb von Teams vertraut, bevor Sie Ihren ersten Bot in Teams integrieren.  Sie können weitere Informationen zu Bots im Lernprogramme von [Azure Bot Service](/azure/bot-service/bot-builder-basics?view=azure-bot-service-4.0&preserve-view=true) finden.

## <a name="run-your-app-locally"></a>Ihre App lokal ausführen

Das Teams-Toolkit ermöglicht es Ihnen, Ihre App lokal zu hosten.  Gehen Sie hierfür folgendermaßen vor:

- Eine Azure Active Directory-Anwendung ist im M365-Mandanten registriert.
- Ein App-Manifest wurde dem Entwicklerportal für Teams übermittelt.
- Eine API wird mittels des Azure Functions Core Tools ausgeführt, um Ihre App zu unterstützen.
- [ngrok](https://ngrok.io) ist installiert und dazu verwendet, eine Tunnel zwischen Teams und Ihrer Messaging-Erweiterung bereitzustellen.

So erstellen Sie Ihre App und führen sie lokal aus:

1. Drücken Sie im Visual Studio Code **F5** um die Anwendung im Debugmodus ausführen.

   > Wenn Sie die App zum ersten Mal ausführen, werden alle Abhängigkeiten heruntergeladen und die App wird erstellt.  Wenn das Erstellen abgeschlossen ist, wird automatisch ein Browserfenster geöffnet.  Diese Änderung kann 3-5 Minuten dauern.

1. Teams wird in einem Webbrowser geladen, und Sie werden dazu aufgefordert, sich anzumelden. Wenn Sie zum Öffnen von Microsoft Teams aufgefordert werden, wählen Sie „Abbrechen“ aus, um im Browser zu verbleiben. Melden Sie sich mit Ihrem M365-Konto an.

1. Drücken Sie **Hinzufügen**, um die App zu Ihrem Konto hinzuzufügen.

Sobald die App geladen ist, wird Ihnen direkt ein Suchdialogfeld angezeigt:

:::image type="content" source="../assets/images/teams-toolkit-v2/msgextn-completed-app.png" alt-text="Suchbasierte Messaging-Erweiterung in Aktion":::

Geben Sie einen Text in das Suchfeld ein, und wählen Sie dann eine der Optionen aus.  Ihrem Eingabefeld wird eine adaptive Karte hinzugefügt.

<!-- markdownlint-disable MD033 -->
<details>
<summary>Erfahren Sie, was passiert, wenn Sie Ihre App lokal im Debugger ausführen.</summary>

Wenn Sie F5 gedrückt haben, hat das Teams Toolkit:

1. Ihre Anwendung bei Azure Active Directory registriert.
1. Ihre Anwendung bei Microsoft Teams für das „Querladen“ registriert.
1. Ihr Anwendungs-Back-End mithilfe von [Azure Function Core Tools](/azure/azure-functions/functions-run-local?#start) lokal gestartet.
1. Ein ngrok-Tunnel gestartet, sodass Teams mit Ihrer App kommunizieren kann.
1. Microsoft Teams mit einem Befehl gestartet, mit dem Teams angewiesen wird, die Anwendung querzuladen.

</details>

<!-- markdownlint-disable MD033 -->
<details>
<summary>In diesem Artikel erfahren Sie, wie Sie häufige Probleme bei dem lokalen Ausführen Ihrer App lösen können.</summary>

Um Ihre App in Teams erfolgreich auszuführen, müssen Sie ein Microsoft 365-Entwicklungskonto haben, das das Querladen von Apps ermöglicht. Weitere Informationen zum Öffnen von Apps finden Sie unter [Erforderliche Komponenten](prerequisites.md#enable-sideloading).

> [!TIP]
> Überprüfen Sie mithilfe des [Tools für die App-Überprüfung](https://dev.teams.microsoft.com/appvalidation.html), das im Toolkit enthalten ist, ob es Probleme gibt, bevor Sie ihre App querladen. Beheben Sie die Probleme, um die App erfolgreich querzuladen.
</details>

[!INCLUDE [Provision and Deploy your app on Azure](~/includes/get-started/azure-provisioning-instructions.md)]

<!-- markdownlint-disable MD033 -->

<details>
<summary>Erfahren Sie, was passiert, wenn Sie Ihre App Azure bereitstellen</summary>

Vor der Bereitstellung wurde die Anwendung lokal ausgeführt:

1. Das Back-End wird mithilfe von _Azure Functions Core Tools_ ausgeführt.
1. Der HTTP-Endpunkt der Anwendung, an dem Microsoft Teams die Anwendung ladet, wird lokal ausgeführt.

Die Bereitstellung umfasst die Bereitstellung von Ressourcen für ein aktives Azure-Abonnement und das Bereitstellen (Hochladen) des Back-Ends und des Frontend-Codes für die Anwendung in Azure. Das Back-End verwendet eine Vielzahl von Azure-Diensten, einschließlich Azure App Service und Azure Bot Service.

</details>

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu anderen Methoden zum Erstellen von Teams-Apps:

- [Erstellen einer Teams-App mit React](first-app-react.md)
- [Erstellen einer Teams-App mit Blazor](first-app-blazor.md)
- [Erstellen einer Teams-App als SharePoint-Webpart](first-app-spfx.md) (Azure nicht erforderlich)
- [Erstellen eines Unterhaltungs-Bots](first-app-bot.md)