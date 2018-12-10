---
title: Uw eerste Python-functie maken in Azure
description: Informatie over hoe u met de Azure Functions Core Tools en de Azure CLI uw eerste Python-functie maakt in Azure.
services: functions
keywords: ''
author: ggailey777
ms.author: glenga
ms.date: 08/29/2018
ms.topic: quickstart
ms.service: functions
ms.custom: mvc
ms.devlang: python
manager: jeconnoc
ms.openlocfilehash: 1ab6bb341c0b21db1e50f7dcada5b0a6b4267a49
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52853363"
---
# <a name="create-your-first-python-function-in-azure-preview"></a>Uw eerste Python-functie maken in Azure (preview)

[!INCLUDE [functions-python-preview-note](../../includes/functions-python-preview-note.md)]

In dit quickstart-artikel leert u hoe u de Azure CLI gebruikt om uw eerste [serverloze](https://azure.microsoft.com/overview/serverless-computing/) Python-functie-app te maken die in Linux wordt uitgevoerd. De functiecode wordt lokaal gemaakt en vervolgens naar Azure geïmplementeerd met behulp van de [Azure Functions Core Tools](functions-run-local.md). Zie [dit Werkt op Linux-artikel](https://aka.ms/funclinux) voor meer informatie over preview-overwegingen voor het uitvoeren van uw functie-apps op Linux.

De volgende stappen worden ondersteund op een Mac-, Windows- of Linux-computer.

## <a name="prerequisites"></a>Vereisten

Als u lokaal wilt ontwikkelen en testen, gaat u als volgt te werk:

+ Installeer [Python 3.6](https://www.python.org/downloads/).

+ Installeer [Azure Functions Core Tools](functions-run-local.md#v2) (versie 2.2.70 of hoger).

Voor het publiceren en uitvoeren in Azure is vereist:

+ Dat u de [Azure CLI]( /cli/azure/install-azure-cli) versie 2 of hoger installeert.

+ Een Azure-abonnement.
  [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-and-activate-a-virtual-environment"></a>Een virtuele omgeving maken en activeren

Voor het maken van een Functions-project is het vereist dat u een virtuele Python 3.6-omgeving werkt. Voer de volgende opdrachten uit om een virtuele omgeving met de naam `.env` te maken.

```bash
# In Bash
python3.6 -m venv .env
source .env/bin/activate

# In PowerShell
py -3.6 -m venv .env
.env\scripts\activate
```

## <a name="create-a-local-functions-project"></a>Een lokaal Functions-project maken

U kunt nu een lokaal Functions-project maken. Deze map is het equivalent van een functie-app in Azure. Deze kan meerdere functies bevatten met dezelfde lokale configuratie en hostingconfiguratie.

Voer vanuit een terminalvenster of vanaf een opdrachtprompt de volgende opdracht uit:

```bash
func init MyFunctionProj
```

Kies **Python** als de gewenste runtime.

```output
Select a worker runtime:
1. dotnet
2. node
3. python
```

Er verschijnt uitvoer die er ongeveer als volgt uitziet.

```output
Installing wheel package
Installing azure-functions package
Installing azure-functions-worker package
Running pip freeze
Writing .gitignore
Writing host.json
Writing local.settings.json
Writing /MyFunctionProj/.vscode/extensions.json
```

Een nieuwe map met de naam _MyFunctionProj_ is gemaakt. Wijzig van map en ga naar de volgende map om door te gaan.

```bash
cd MyFunctionProj
```

## <a name="create-a-function"></a>Een functie maken

Voer de volgende opdracht uit om een functie te maken:

```bash
func new
```

Kies `HTTP Trigger` als sjabloon en geef een **functienaam** van `HttpTrigger` op.

```output
Select a template:
1. Blob trigger
2. Cosmos DB trigger
3. Event Grid trigger
4. Event Hub trigger
5. HTTP trigger
6. Queue trigger
7. Service Bus Queue trigger
8. Service Bus Topic trigger
9. Timer trigger

Choose option: 5
Function name: HttpTrigger
```

Er verschijnt uitvoer die er ongeveer als volgt uitziet.

```output
Writing /MyFunctionProj/HttpTrigger/sample.dat
Writing /MyFunctionProj/HttpTrigger/__init__.py
Writing /MyFunctionProj/HttpTrigger/function.json
The function "HttpTrigger" was created successfully from the "HTTP trigger" template.
```

Er is een submap met de naam _HttpTrigger_ gemaakt. Deze bevat `__init__.py`, wat het primaire scriptbestand is en het bestand `function.json` waarin de trigger en bindingen worden beschreven die worden gebruikt door de functie. Meer informatie over het programmeermodel vindt u in de [ontwikkelaarsgids van Azure Functions Python](functions-reference-python.md).

## <a name="run-the-function-locally"></a>De functie lokaal uitvoeren

Gebruik de volgende opdracht om de host functies lokaal uit te voeren.

```bash
func host start
```

Wanneer de Functions-host wordt gestart, bestaat de uitvoer uit de URL van de functie die door HTTP is geactiveerd. (Houd er rekening mee dat de volledige uitvoer is afgekapt voor een betere leesbaarheid.)

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
Now listening on: http://0.0.0.0:7071
Application started. Press Ctrl+C to shut down.
...

Http Functions:

        HttpTrigger: http://localhost:7071/api/HttpTrigger
```

Kopieer de URL van uw functie uit de uitvoer en plak deze in de adresbalk van uw browser. Voeg de queryreeks `?name=<yourname>` toe aan de URL en voer de aanvraag uit.

    http://localhost:7071/api/HttpTrigger?name=<yourname>

In de volgende schermafbeelding ziet u het antwoord van de functie, wanneer deze wordt geactiveerd vanuit de browser:

![test](./media/functions-create-first-function-python/function-test-local-browser.png)

U bent nu klaar om een functie-app en andere vereiste resources te maken om naar Azure te publiceren.

[!INCLUDE [functions-create-resource-group](../../includes/functions-create-resource-group.md)]

[!INCLUDE [functions-create-storage-account](../../includes/functions-create-storage-account.md)]

## <a name="create-a-linux-function-app-in-azure"></a>Een Linux-functie-app maken in Azure

De functie-app biedt een omgeving waarin uw functiecode kan worden uitgevoerd. U kunt er functies mee groeperen in een logische eenheid, zodat u resources eenvoudiger kunt beheren, implementeren en delen. U maakt een **Python-functie-app die in Linux wordt uitgevoerd** met behulp van de opdracht [az functionapp create](/cli/azure/functionapp#az_functionapp_create).

Voer in de volgende opdracht uit met daarin een unieke functie-appnaam in plaats van de tijdelijke plaatsaanduiding `<app_name>` en gebruik de naam van het opslagaccount in plaats van `<storage_name>`. De `<app_name>` is ook het standaard DNS-domein voor de functie-app. Deze naam moet uniek zijn in alle apps in Azure.

```azurecli-interactive
az functionapp create --resource-group myResourceGroup --os-type Linux \
--consumption-plan-location westus  --runtime python \
--name <app_name> --storage-account  <storage_name>
```

> [!NOTE]
> Als u al een resourcegroep met de naam `myResourceGroup` hebt die niet-Linux App Service-apps bevat, moet u een andere resourcegroep gebruiken. Het is niet mogelijk om zowel Windows- als Linux-apps in dezelfde resourcegroep uit te voeren.  

Nadat de functie-app is gemaakt, ziet u het volgende bericht:

```output
Your serverless Linux function app 'myfunctionapp' has been successfully created.
To active this function app, publish your app content using Azure Functions Core Tools or the Azure portal.
```

U kunt nu uw lokale functieproject publiceren naar de functie-app in Azure.

## <a name="deploy-the-function-app-project-to-azure"></a>Het functie-appproject implementeren in Azure

Voer de volgende opdracht uit om de Azure Functions Core Tools te kunnen gebruiken. Vervang `<app_name>` door de naam van uw app uit de vorige stap.

```bash
func azure functionapp publish <app_name>
```

De uitvoer die wordt weergegeven, ziet er ongeveer als volgt uit, en is afgekapt voor een beter leesbaarheid.

```output
Getting site publishing info...

...

Preparing archive...
Uploading content...
Upload completed successfully.
Deployment completed successfully.
Syncing triggers...
```

[!INCLUDE [functions-test-function-code](../../includes/functions-test-function-code.md)]

[!INCLUDE [functions-cleanup-resources](../../includes/functions-cleanup-resources.md)]

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het ontwikkelen van Azure-functies met behulp van Python.

> [!div class="nextstepaction"]
> [De Azure Functions Python-ontwikkelaarsgids](functions-reference-python.md)
> [Azure Functions-triggers en -bindingen](functions-triggers-bindings.md)
