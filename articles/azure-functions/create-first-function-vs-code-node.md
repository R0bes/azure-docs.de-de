---
title: 'Erstellen einer JavaScript-Funktion mit Visual Studio Code: Azure Functions'
description: Erfahren Sie, wie Sie eine JavaScript-Funktion erstellen und dann das lokale Node.js-Projekt für serverloses Hosting in Azure Functions unter Verwendung der Azure Functions-Erweiterung in Visual Studio Code veröffentlichen.
ms.topic: quickstart
ms.date: 07/01/2021
adobe-target: true
adobe-target-activity: DocsExp–386541–A/B–Enhanced-Readability-Quickstarts–2.19.2021
adobe-target-experience: Experience B
adobe-target-content: ./create-first-function-vs-code-node_uiex
ms.openlocfilehash: 09c028d71d7cef4b83220a7c93a24c6bc3c256d4
ms.sourcegitcommit: ee8ce2c752d45968a822acc0866ff8111d0d4c7f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/14/2021
ms.locfileid: "113732575"
---
# <a name="quickstart-create-a-javascript-function-in-azure-using-visual-studio-code"></a>Schnellstart: Erstellen einer JavaScript-Funktion in Azure mit Visual Studio Code

[!INCLUDE [functions-language-selector-quickstart-vs-code](../../includes/functions-language-selector-quickstart-vs-code.md)]

Verwenden Sie Visual Studio Code zum Erstellen einer JavaScript-Funktion, die auf HTTP-Anforderungen reagiert. Testen Sie den Code lokal, und stellen Sie ihn anschließend in der serverlosen Umgebung von Azure Functions bereit.

Im Rahmen dieser Schnellstartanleitung fallen in Ihrem Azure-Konto ggf. geringfügige Kosten im Centbereich an.

Es gibt auch eine [CLI-basierte Version](create-first-function-cli-node.md) dieses Artikels.

## <a name="configure-your-environment"></a>Konfigurieren Ihrer Umgebung

Vergewissern Sie sich zunähst, dass Folgendes vorhanden ist:

+ Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).

+ [Node.js 10.14.1+](https://nodejs.org/). Verwenden Sie den Befehl `node --version`, um Ihre Version zu überprüfen.  

+ [Visual Studio Code](https://code.visualstudio.com/) auf einer der [unterstützten Plattformen](https://code.visualstudio.com/docs/supporting/requirements#_platforms)

+ [Azure Functions-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions) für Visual Studio Code

## <a name="create-your-local-project"></a><a name="create-an-azure-functions-project"></a>Erstellen Ihres lokalen Projekts

In diesem Abschnitt wird mithilfe von Visual Studio Code ein lokales Azure Functions-Projekt in JavaScript erstellt. Weiter unten in diesem Artikel wird der Funktionscode in Azure veröffentlicht.

1. Wählen Sie auf der Aktivitätsleiste das Azure-Symbol und anschließend im Bereich **Azure: Funktionen** das Symbol **Neues Projekt erstellen** aus.

    ![Auswählen von „Neues Projekt erstellen“](./media/functions-create-first-function-vs-code/create-new-project.png)

1. Wählen Sie einen Verzeichnisspeicherort für Ihren Projektarbeitsbereich und anschließend **Auswählen** aus.

    > [!NOTE]
    > Diese Schritte sollten außerhalb eines Arbeitsbereichs ausgeführt werden. Wählen Sie in diesem Fall keinen Projektordner aus, der Teil eines Arbeitsbereichs ist.

1. Geben Sie nach entsprechender Aufforderung Folgendes ein:

    |Prompt|Auswahl|
    |--|--|
    |**Auswählen einer Sprache für Ihr Funktionsprojekt**|Klicken Sie auf die Option `JavaScript`.|
    |**Auswählen einer Vorlage für die erste Funktion Ihres Projekts**|Klicken Sie auf die Option `HTTP trigger`.|
    |**Angeben eines Funktionsnamens**|Geben Sie `HttpExample`ein.|
    |**Autorisierungsstufe**|Wählen Sie `Anonymous` aus, damit Ihr Funktionsendpunkt von jedem Benutzer aufgerufen werden kann. Weitere Informationen zur Autorisierungsstufe finden Sie unter [Autorisierungsschlüssel](functions-bindings-http-webhook-trigger.md#authorization-keys).|
    |**Auswählen, wie Sie Ihr Projekt öffnen möchten**|Klicken Sie auf die Option `Add to workspace`.|

    Auf der Grundlage dieser Informationen generiert Visual Studio Code ein Azure Functions-Projekt mit einem HTTP-Trigger. Die lokalen Projektdateien können im Explorer angezeigt werden. Weitere Informationen zu den erstellten Dateien finden Sie unter [Generierte Projektdateien](functions-develop-vs-code.md#generated-project-files). 

[!INCLUDE [functions-run-function-test-local-vs-code](../../includes/functions-run-function-test-local-vs-code.md)]

Nachdem Sie sich vergewissert haben, dass die Funktion auf Ihrem lokalen Computer korrekt ausgeführt wird, können Sie das Projekt mithilfe von Visual Studio Code direkt in Azure veröffentlichen.

[!INCLUDE [functions-sign-in-vs-code](../../includes/functions-sign-in-vs-code.md)]

<a name="Publish the project to Azure"></a>

## <a name="deploy-the-project-to-azure"></a>Bereitstellen des Projekts in Azure

In diesem Abschnitt erstellen Sie eine Funktions-App sowie zugehörige Ressourcen in Ihrem Azure-Abonnement und stellen anschließend Ihren Code bereit. 

> [!IMPORTANT]
> Bei der Bereitstellung in einer vorhandenen Funktions-App wird der Inhalt dieser App in Azure überschrieben. 


1. Wählen Sie auf der Aktivitätsleiste das Azure-Symbol und anschließend im Bereich **Azure: Funktionen** die Schaltfläche **Deploy to function app...** (In Funktions-App bereitstellen...) aus.

    ![Veröffentlichen Ihres Projekts in Azure](../../includes/media/functions-publish-project-vscode/function-app-publish-project.png)

1. Geben Sie nach entsprechender Aufforderung Folgendes ein:

    |Prompt| Auswahl|
    |--|--|
    |**Auswählen einer Funktions-App in Azure**|Klicken Sie auf die Option `+ Create new Function App`. (Wählen Sie nicht die Option `Advanced` aus, diese wird im vorliegenden Artikel nicht behandelt.)|
    |**Eingeben eines global eindeutigen Namens für die Funktions-App**|Geben Sie einen Namen ein, der in einem URL-Pfad gültig ist. Der eingegebene Name wird überprüft, um sicherzustellen, dass er in Azure Functions eindeutig ist.|
    |**Auswählen einer Runtime**|Wählen Sie die lokal ausgeführte Node.js-Version aus. Sie können den Befehl `node --version` ausführen, um Ihre Version zu überprüfen.|
    |**Auswählen eines Standorts für neue Ressourcen**|Wählen Sie eine [Region](https://azure.microsoft.com/regions/) in Ihrer Nähe aus, um eine bessere Leistung zu erzielen.|

    Die Erweiterung zeigt den Status einzelner Ressourcen an, während diese in Azure im Benachrichtigungsbereich erstellt werden.

    :::image type="content" source="../../includes/media/functions-publish-project-vscode/resource-notification.png" alt-text="Benachrichtigung über die Erstellung von Azure-Ressourcen":::

    Nach Abschluss des Vorgangs werden in Ihrem Abonnement die folgenden Azure-Ressourcen erstellt, deren Namen auf dem Namen Ihrer Funktions-App basieren:

    [!INCLUDE [functions-vs-code-created-resources](../../includes/functions-vs-code-created-resources.md)]

1. Nach der Erstellung der Funktions-App wird eine Benachrichtigung angezeigt, und das Bereitstellungspaket wird angewendet. 

    [!INCLUDE [functions-vs-code-create-tip](../../includes/functions-vs-code-create-tip.md)]

1. Wählen Sie in dieser Benachrichtigungen **Ausgabe anzeigen** aus, um die Erstellungs- und Bereitstellungsergebnisse (auch für die von Ihnen erstellten Azure-Ressourcen) anzuzeigen. Wenn Sie die Benachrichtigung übersehen haben, wählen Sie das Glockensymbol in der unteren rechten Ecke aus, um sie erneut anzuzeigen.

    ![Benachrichtigung nach Abschluss der Erstellung](./media/functions-create-first-function-vs-code/function-create-notifications.png)

[!INCLUDE [functions-vs-code-run-remote](../../includes/functions-vs-code-run-remote.md)]

## <a name="change-the-code-and-redeploy-to-azure"></a>Ändern des Codes und erneutes Bereitstellen in Azure

1. Wählen Sie in der Ansicht VSCode-Explorer die Datei `./HttpExample/index.js` aus. 
1. Ersetzen Sie die Datei durch den folgenden Code, um ein JSON-Objekt zu erstellen und zurückzugeben.

    ```javascript
    module.exports = async function (context, req) {
        
        try {
            context.log('JavaScript HTTP trigger function processed a request.');
    
            // Read incoming data
            const name = req.query.name;
            const sport = req.query.sport;
        
            // fail if incoming data is required
            if (!name || !sport) {
    
                context.res = {
                    status: 400
                };
                return;
            }
            
            // Add or change code here
            const message = `${name} likes ${sport}`;
        
            // Construct response
            const responseJSON = {
                "name": name,
                "sport": sport,
                "message": message,
                "success": true
            }
    
            context.res = {
                // status: 200, /* Defaults to 200 */
                body: responseJSON,
                contentType: 'application/json'
            };
        } catch(err) {
            context.res = {
                status: 500
            };
        }
    }
    ```
1. [Erneutes lokales Ausführen der Funktions-App](#run-the-function-locally)
1. Ändern Sie in der Eingabeaufforderung **Anforderungstext eingeben** den Text der Anforderungsnachricht in { „name“: „Tom“,„sport“:„basketball“ }. Drücken Sie die EINGABETASTE, um diese Anforderungsnachricht an Ihre Funktion zu senden.
1. Zeigen Sie die Antwort in der Benachrichtigung an:

    ```json
    {
      "name": "Tom",
      "sport": "basketball",
      "message": "Tom likes basketball",
      "success": true
    }
    ```

1. [Erneutes Bereitstellen der Funktion](#deploy-the-project-to-azure) in Azure.

## <a name="troubleshooting"></a>Problembehandlung

Nutzen Sie die folgende Tabelle, um die häufigsten Probleme zu beheben, die bei der Verwendung dieser Schnellstartanleitung auftreten.

|Problem|Lösung|
|--|--|
|Sie können kein lokales Funktionsprojekt erstellen?|Stellen Sie sicher, dass die [Azure Functions-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions) installiert ist.|
|Die Funktion kann nicht lokal ausgeführt werden?|Stellen Sie sicher, dass die [Azure Functions Core Tools](functions-run-local.md?tabs=windows%2Ccsharp%2Cbash) installiert sind. <br/>Sollten bei der Ausführung unter Windows Probleme auftreten, vergewissern Sie sich, dass die Standardshell des Terminals für Visual Studio Code nicht auf „WSL Bash“ festgelegt ist.|
|Können Sie die Funktion nicht in Azure bereitstellen?|Überprüfen Sie die Ausgabe auf Fehlerinformationen. Das Glockensymbol in der unteren rechten Ecke ist eine weitere Möglichkeit zum Anzeigen der Ausgabe. Haben Sie die Veröffentlichung in einer vorhandenen Funktions-App durchgeführt? Diese Aktion überschreibt den Inhalt dieser App in Azure.|
|Konnte die cloudbasierte Funktions-App nicht ausgeführt werden?|Denken Sie daran, die Abfragezeichenfolge für das Senden von Parametern zu verwenden.|

[!INCLUDE [functions-cleanup-resources-vs-code.md](../../includes/functions-cleanup-resources-vs-code-extension.md)]

## <a name="next-steps"></a>Nächste Schritte

Sie haben [Visual Studio Code](functions-develop-vs-code.md?tabs=javascript) genutzt, um eine Funktions-App mit einer einfachen Funktion zu erstellen, die über HTTP ausgelöst wird. Im nächsten Artikel erweitern Sie diese Funktion, indem Sie eine Verbindung mit Azure Cosmos DB oder Azure Storage herstellen. Weitere Informationen zum Herstellen einer Verbindung mit anderen Azure-Diensten finden Sie unter [Hinzufügen von Bindungen zu einer vorhandenen Funktion in Azure Functions](add-bindings-existing-function.md?tabs=javascript).  

> [!div class="nextstepaction"]
> [Herstellen einer Verbindung mit einer Datenbank](functions-add-output-binding-cosmos-db-vs-code.md?pivots=programming-language-javascript)
> [!div class="nextstepaction"]
> [Herstellen einer Verbindung mit einer Azure Storage-Warteschlange](functions-add-output-binding-storage-queue-vs-code.md?pivots=programming-language-javascript)
> [Sichern Ihrer Funktion](security-concepts.md)

[Azure Functions Core Tools]: functions-run-local.md
[Azure Functions extension for Visual Studio Code]: https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions
