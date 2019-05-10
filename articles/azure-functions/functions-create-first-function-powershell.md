---
title: Uw eerste PowerShell-functie maken met Azure Functions
description: Leer hoe u uw eerste PowerShell-functie maken in Azure met behulp van Visual Studio Code.
services: functions
keywords: ''
author: joeyaiello
manager: jeconnoc
ms.author: jaiello, glenga
ms.date: 04/25/2019
ms.topic: quickstart
ms.service: azure-functions
ms.devlang: powershell
ms.openlocfilehash: 24640c9013f7a5b81cd5b1b6f45de49c5baad0e6
ms.sourcegitcommit: 399db0671f58c879c1a729230254f12bc4ebff59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/09/2019
ms.locfileid: "65473272"
---
# <a name="create-your-first-powershell-function-in-azure-preview"></a>Uw eerste PowerShell-functie maken in Azure (preview)

[!INCLUDE [functions-powershell-preview-note](../../includes/functions-powershell-preview-note.md)]

In dit artikel begeleidt u bij het maken van uw eerste [serverloze](https://azure.com/serverless) PowerShell-functie met behulp van Visual Studio Code.

![Azure Functions-code in een project voor Visual Studio Code](./media/functions-create-first-function-powershell/powershell-project-first-function.png)

U gebruikt de [Azure Functions extension for Visual Studio Code] een PowerShell-functie lokaal maakt en deze vervolgens geïmplementeerd naar een nieuwe functie-app in Azure. De extensie is momenteel beschikbaar als preview-product. Zie de uitbreidingspagina [Azure Functions extension for Visual Studio Code] (Azure Functions-extensie voor Visual Studio Code) voor meer informatie.

> [!NOTE]  
> PowerShell-ondersteuning voor de [Azure Functions-extensie][azure functions extension for visual studio code] is momenteel standaard uitgeschakeld. Het inschakelen van PowerShell-ondersteuning is een van de stappen in dit artikel.

De volgende stappen uit worden op macOS, Windows en Linux-besturingssystemen ondersteund.

## <a name="prerequisites"></a>Vereisten

Dit zijn de vereisten voor het voltooien van deze snelstart:

* Installeer [PowerShell Core](/powershell/scripting/install/installing-powershell#powershell-core)

* Installeer [Visual Studio Code](https://code.visualstudio.com/) op een van de [ondersteunde platforms](https://code.visualstudio.com/docs/supporting/requirements#_platforms). 

* Installeer [PowerShell-extensie voor Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.PowerShell).

* Installeer [.NET Core SDK 2.2 +](https://www.microsoft.com/net/download) (wordt vereist door de Azure Functions Core Tools en beschikbaar zijn op alle ondersteunde platforms).

* Installeer versie 2.x van de [Azure Functions Core Tools](functions-run-local.md#v2).

* U moet ook een actief Azure-abonnement.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [functions-install-vs-code-extension](../../includes/functions-install-vs-code-extension.md)] 

## <a name="create-a-function-app-project"></a>Een functie-appproject maken

De Azure Functions-projectsjabloon in Visual Studio Code maakt een project dat kan worden gepubliceerd in een functie-app in Azure. Met een functie-app kunt u functies groeperen in een logische eenheid, zodat u resources kunt beheren, implementeren en delen. 

1. Selecteer het Azure-logo om weer te geven in Visual Studio Code, de **Azure: Functies** vlak- en selecteer vervolgens het pictogram Nieuw Project maken.

    ![Een functie-appproject maken](./media/functions-create-first-function-powershell/create-function-app-project.png)

1. Kies een locatie voor uw werkruimte Functions-project en kies **Selecteer**.

    > [!NOTE]
    > Dit artikel is bedoeld om buiten een werkruimte te worden voltooid. Selecteer in dit geval geen projectmap die deel uitmaakt van een werkruimte.

1. Selecteer de **Powershell (preview)** als de taal voor uw functie-app-project en vervolgens **Azure Functions v2**.

1. Kies **HTTP-Trigger** gebruiken als de sjabloon voor uw eerste functie `HTTPTrigger` als de functie een naam en kies een autorisatieniveau van **functie**.

    > [!NOTE]
    > De **functie** verificatieniveau is vereist een [functietoets](functions-bindings-http-webhook.md#authorization-keys) waarde bij het aanroepen van het eindpunt van de functie in Azure. Dit maakt het moeilijker voor anderen uw functie aanroept.

1. Kies **Add to workspace** (Aan werkruimte toevoegen) als daarom wordt gevraagd.

Visual Studio Code wordt de PowerShell-functie-app-project gemaakt in een nieuwe werkruimte. Dit project bevat de [host.json](functions-host-json.md) en [local.settings.json](functions-run-local.md#local-settings-file) configuratiebestanden, die voor alle functie in het project gelden. Dit [PowerShell project](functions-reference-powershell.md#folder-structure) is hetzelfde als een functie-app in Azure.

## <a name="run-the-function-locally"></a>De functie lokaal uitvoeren

Azure Functions Core Tools kan worden geïntegreerd met Visual Studio Code, zodat u kunt uitvoeren en fouten opsporen in een Azure Functions-project lokaal.  

1. Om op te sporen uw functie, Voeg een aanroep naar de [ `Wait-Debugger` ] cmdlet in de functiecode aan te geven voordat u het foutopsporingsprogramma koppelen druk op F5 om te starten van de functie-app-project en het foutopsporingsprogramma koppelen. De uitvoer van Core Tools wordt weergegeven in het deelvenster **Terminal**.

1. Kopieer het URL-eindpunt van de door HTTP getriggerde functie in het deelvenster **Terminal**.

    ![Lokale Azure-uitvoer](./media/functions-create-first-function-powershell/functions-vscode-f5.png)

1. Voeg de queryreeks `?name=<yourname>` toe aan deze URL en gebruik vervolgens `Invoke-RestMethod` voor het uitvoeren van de aanvraag, als volgt:

    ```powershell
    PS > Invoke-RestMethod -Method Get -Uri http://localhost:7071/api/HttpTrigger?name=PowerShell
    Hello PowerShell
    ```

    U kunt ook de GET-aanvraag uitvoeren vanuit een browser.

    Wanneer u het eindpunt HttpTrigger zonder doorgeven aanroepen een `name` parameter als een queryparameter of in de hoofdtekst van de functie geeft als resultaat een 500-fout. Wanneer u de code in run.ps1 bekijkt, ziet u dat deze fout treedt op standaard.

1. Als u wilt stoppen met fouten opsporen, drukt u op Shift + F5.

Nadat u hebt gecontroleerd of de functie correct wordt uitgevoerd op uw lokale computer, is het tijd om het project te publiceren in Azure.

> [!NOTE]
> Vergeet niet te verwijderen van de aanroepen voor `Wait-Debugger` voordat u uw functies naar Azure publiceert. 

> [!NOTE]
> Het maken van een functie-App in Azure vraagt alleen naar de naam van de functie-App. AzureFunctions.advancedCreation ingesteld op true voor alle andere waarden gevraagd.

[!INCLUDE [functions-publish-project-vscode](../../includes/functions-publish-project-vscode.md)]

## <a name="test"></a>De functie uitvoeren in Azure

Om te controleren dat de gepubliceerde functie wordt uitgevoerd in Azure, dan de volgende PowerShell-opdracht, vervangt de `Uri` parameter met de URL van de functie HTTPTrigger uit de vorige stap. Als voorheen, voeg de queryreeks `&name=<yourname>` naar de URL, zoals in het volgende voorbeeld:

```powershell
PS > Invoke-WebRequest -Method Get -Uri "https://glengatest-vscode-powershell.azurewebsites.net/api/HttpTrigger?code=nrY05eZutfPqLo0som...&name=PowerShell"

StatusCode        : 200
StatusDescription : OK
Content           : Hello PowerShell
RawContent        : HTTP/1.1 200 OK
                    Content-Length: 16
                    Content-Type: text/plain; charset=utf-8
                    Date: Thu, 25 Apr 2019 16:01:22 GMT

                    Hello PowerShell
Forms             : {}
Headers           : {[Content-Length, 16], [Content-Type, text/plain; charset=utf-8], [Date, Thu, 25 Apr 2019 16:01:22 GMT]}
Images            : {}
InputFields       : {}
Links             : {}
ParsedHtml        : mshtml.HTMLDocumentClass
RawContentLength  : 16
```

## <a name="next-steps"></a>Volgende stappen

Een PowerShell-functie-app maken met een eenvoudige HTTP-geactiveerde functie hebt u Visual Studio Code gebruikt. U kunt ook voor meer informatie over [opsporen van fouten in een PowerShell-functie lokaal](functions-debug-powershell-local.md) met behulp van Azure Functions Core Tools. Bekijk de [ontwikkelaarsgids van Azure Functions PowerShell](functions-reference-powershell.md).

> [!div class="nextstepaction"]
> [Application Insights-integratie inschakelen](functions-monitoring.md#manually-connect-an-app-insights-resource)

[Azure portal]: https://portal.azure.com
[Azure Functions Core Tools]: functions-run-local.md
[Azure Functions extension for Visual Studio Code]: https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions (Azure Functions-extensie voor Visual Studio Code)
[`Wait-Debugger`]: /powershell/module/microsoft.powershell.utility/wait-debugger?view=powershell-6
