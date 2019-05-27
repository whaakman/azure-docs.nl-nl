---
title: Een door HTTP geactiveerde functie maken in Azure
description: Informatie over hoe u met de Azure Functions Core Tools en de Azure CLI uw eerste Python-functie maakt in Azure.
services: functions
keywords: ''
author: ggailey777
ms.author: glenga
ms.date: 04/24/2019
ms.topic: quickstart
ms.service: azure-functions
ms.custom: mvc
ms.devlang: python
manager: jeconnoc
ms.openlocfilehash: 88e8d543e8bc3c3ae07133ec333b33c4486c2684
ms.sourcegitcommit: 4c2b9bc9cc704652cc77f33a870c4ec2d0579451
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/17/2019
ms.locfileid: "65864516"
---
# <a name="create-an-http-triggered-function-in-azure"></a>Een door HTTP geactiveerde functie maken in Azure

[!INCLUDE [functions-python-preview-note](../../includes/functions-python-preview-note.md)]

In dit artikel wordt beschreven hoe u opdrachtregelprogramma's gebruiken om te maken van een Python-project dat wordt uitgevoerd in Azure Functions. De functie die u maakt, wordt geactiveerd door HTTP-aanvragen. Ten slotte het publiceren van uw project uit te voeren als een [serverloze functie](functions-scale.md#consumption-plan) in Azure.

In dit artikel is de eerste twee snelstartgidsen voor Azure Functions. Na het voltooien van dit artikel hebt u [toevoegen van een Azure Storage queue Storage-Uitvoerbinding](functions-add-output-binding-storage-queue-python.md) naar uw functie.

## <a name="prerequisites"></a>Vereisten

Voordat u begint, moet u het volgende hebt:

+ Installeer [Python 3.6](https://www.python.org/downloads/).

+ Installeer [Azure Functions Core Tools](./functions-run-local.md#v2) versie 2.6.666 of hoger.

+ Dat u de [Azure CLI](/cli/azure/install-azure-cli) versie 2 of hoger installeert.

+ Een actief Azure-abonnement.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-and-activate-a-virtual-environment"></a>Een virtuele omgeving maken en activeren

Lokaal ontwikkelen en testen van Python-functies, moet u in een omgeving met Python 3.6 werken. Voer de volgende opdrachten uit om een virtuele omgeving met de naam `.env` te maken.

### <a name="bash-or-a-terminal-window"></a>Bash- of een terminalvenster:

```bash
python3.6 -m venv .env
source .env/bin/activate
```

### <a name="powershell-or-a-windows-command-prompt"></a>PowerShell of een Windows-opdrachtprompt:

```powershell
py -3.6 -m venv .env
.env\scripts\activate
```

De resterende opdrachten worden uitgevoerd binnen de virtuele omgeving.

## <a name="create-a-local-functions-project"></a>Een lokaal Functions-project maken

Een Functions-project is het equivalent van een functie-app in Azure. Er kunnen meerdere functies dat alle de dezelfde lokale en hosting-configuraties delen.

In de virtuele omgeving, voert u de volgende opdracht, kiezen **python** als de worker-runtime.

```command
func init MyFunctionProj
```

Een map met de naam _MyFunctionProj_ is gemaakt, die de volgende drie bestanden bevat:

* `local.settings.json` wordt gebruikt voor het opslaan van app-instellingen en verbindingsreeksen bij lokale uitvoering. Dit bestand niet publiceren naar Azure.
* `requirements.txt` bevat de lijst met pakketten die moeten worden geïnstalleerd op de publicatie naar Azure.
* `host.json` met de globale configuratie-opties die invloed hebben op alle functies in een functie-app. Dit bestand publiceren naar Azure.

Navigeer naar de nieuwe MyFunctionProj-map:

```command
cd MyFunctionProj
```

Vervolgens kunt u het bestand host.json om in te schakelen van extensie-bundels bijwerken.  

## <a name="reference-bindings"></a>Referentie-bindingen

Extensie-bundels gemakkelijker om toe te voegen bindinguitbreidingen omlaag onderweg. U verwijdert ook de vereiste van het installeren van de .NET Core SDK 2.x. Extensie-bundels versie 2.6.1071 van de essentiële hulpprogramma of een latere versie vereist. 

[!INCLUDE [functions-extension-bundles](../../includes/functions-extension-bundles.md)]

U kunt nu een functie toevoegen aan uw project.

## <a name="create-a-function"></a>Een functie maken

Een functie toevoegen aan uw project, moet u de volgende opdracht uitvoeren:

```command
func new
```

Kies de **HTTP-trigger** sjabloon, type `HttpTrigger` als de naam voor de functie, druk op Enter.

Een submap met de naam _HttpTrigger_ is gemaakt, waarin de volgende bestanden:

* **Function.JSON**: configuratiebestand dat de functie, trigger en andere bindingen bepaalt. Dit bestand controleren en zien dat de waarde voor `scriptFile` verwijst naar het bestand met de functie, terwijl de aanroep-trigger en bindingen zijn gedefinieerd in de `bindings` matrix.

  Elke binding is vereist voor een richting, type en een unieke naam. De HTTP-trigger heeft een Invoerbinding van het type [ `httpTrigger` ](functions-bindings-http-webhook.md#trigger) en de uitvoer van de binding van het type [ `http` ](functions-bindings-http-webhook.md#output).

* **\_\_Init\_\_.py**: scriptbestand dat uw met HTTP geactiveerde functie. Bekijk dit script en dat deze een standaard bevat `main()`. HTTP-gegevens van de trigger wordt doorgegeven aan deze functie gebruikt de `req` bindende parameter met de naam. Gedefinieerd in de function.json, `req` is een exemplaar van de [azure.functions.HttpRequest klasse](/python/api/azure-functions/azure.functions.httprequest). 

    Het geretourneerde object, dat is gedefinieerd als `$return` in function.json, is een exemplaar van [azure.functions.HttpResponse klasse](/python/api/azure-functions/azure.functions.httpresponse). Zie voor meer informatie, [Azure Functions-HTTP-triggers en bindingen](functions-bindings-http-webhook.md).

## <a name="run-the-function-locally"></a>De functie lokaal uitvoeren

De volgende opdracht start de functie-app, die wordt uitgevoerd lokaal via de Azure Functions-runtime die deel uitmaakt van Azure.

```bash
func host start
```

Wanneer de Functions-host wordt gestart, schrijft ongeveer de volgende uitvoer die voor de leesbaarheid zijn afgekapt:

```output

                  %%%%%%
                 %%%%%%
            @   %%%%%%    @
          @@   %%%%%%      @@
       @@@    %%%%%%%%%%%    @@@
     @@      %%%%%%%%%%        @@
       @@         %%%%       @@
         @@      %%%       @@
           @@    %%      @@
                %%
                %

...

Content root path: C:\functions\MyFunctionProj
Now listening on: http://0.0.0.0:7071
Application started. Press Ctrl+C to shut down.

...

Http Functions:

        HttpTrigger: http://localhost:7071/api/MyHttpTrigger

[8/27/2018 10:38:27 PM] Host started (29486ms)
[8/27/2018 10:38:27 PM] Job host started
```

Kopieer de URL van uw `HttpTrigger`-functie uit de uitvoer van de runtime en plak deze in de adresbalk van uw browser. Voeg de queryreeks `?name=<yourname>` toe aan de URL en voer de aanvraag uit. Hieronder ziet u de reactie op de GET-aanvraag die door de lokale functie wordt geretourneerd, weergegeven in de browser:

![Lokaal testen in de browser](./media/functions-create-first-function-python/function-test-local-browser.png)

Nu u de functie lokaal hebt uitgevoerd, kunt u de functie-app en andere vereiste resources maken in Azure.

[!INCLUDE [functions-create-resource-group](../../includes/functions-create-resource-group.md)]

[!INCLUDE [functions-create-storage-account](../../includes/functions-create-storage-account.md)]

## <a name="create-a-function-app-in-azure"></a>Een functie-app maken in Azure

Een functie-app biedt een omgeving voor het uitvoeren van uw functiecode aan te geven. U kunt er functies mee groeperen in een logische eenheid, zodat u resources eenvoudiger kunt beheren, implementeren en delen.

Voer in de volgende opdracht uit met daarin een unieke functie-appnaam in plaats van de tijdelijke plaatsaanduiding `<APP_NAME>` en gebruik de naam van het opslagaccount in plaats van `<STORAGE_NAME>`. De `<APP_NAME>` is ook het standaard DNS-domein voor de functie-app. Deze naam moet uniek zijn in alle apps in Azure.

```azurecli-interactive
az functionapp create --resource-group myResourceGroup --os-type Linux \
--consumption-plan-location westeurope  --runtime python \
--name <APP_NAME> --storage-account  <STORAGE_NAME>
```

> [!NOTE]
> Linux- en Windows-apps kunnen niet worden gehost in dezelfde resourcegroep bevinden. Hebt u een bestaande resourcegroep met de naam `myResourceGroup` met een Windows-functie-app of web-app, moet u een andere resourcegroep.

U kunt nu uw lokale functions-project publiceren naar de functie-app in Azure.

[!INCLUDE [functions-publish-project](../../includes/functions-publish-project.md)]

[!INCLUDE [functions-test-function-code](../../includes/functions-test-function-code.md)]

## <a name="next-steps"></a>Volgende stappen

U hebt een Python-functions-project gemaakt met een HTTP-geactiveerde functie uitvoeren op uw lokale computer en deze geïmplementeerd naar Azure. Nu uw functie door verlengen...

> [!div class="nextstepaction"]
> [Toevoegen van een Azure Storage Storage queue-Uitvoerbinding](functions-add-output-binding-storage-queue-python.md)
