---
title: Naslaginformatie over Python-ontwikkelaars voor Azure Functions
description: Meer informatie over het ontwikkelen van functies met Python
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
keywords: Azure functions, functies, gebeurtenisverwerking, dynamisch berekenen, architectuur zonder server, python
ms.service: functions
ms.devlang: python
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 04/16/2018
ms.author: glenga
ms.openlocfilehash: 8fa093d629eb7c655ea277b1d57f35193394f722
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/23/2019
ms.locfileid: "56729998"
---
# <a name="azure-functions-python-developer-guide"></a>Handleiding voor ontwikkelaars van Azure Functions-Python

In dit artikel bevat een inleiding tot het ontwikkelen van Azure Functions met behulp van Python. De onderstaande inhoud wordt ervan uitgegaan dat u al hebt gelezen de [handleiding voor ontwikkelaars van Azure Functions voor](functions-reference.md).

[!INCLUDE [functions-python-preview-note](../../includes/functions-python-preview-note.md)]

## <a name="programming-model"></a>Programmeermodel

Een Azure-functie moet een stateless methode in uw Python-script waarmee invoer worden verwerkt en geeft een resultaat. Standaard de runtime wordt verwacht dat deze optie om te worden geïmplementeerd als een algemene methode met de naam `main()` in de `__init__.py` bestand.

U kunt de standaard-configuratie wijzigen door op te geven de `scriptFile` en `entryPoint` eigenschappen in de `function.json` bestand. Bijvoorbeeld, de _function.json_ hieronder vertelt de runtime te gebruiken de _customentry()_ methode in de _main.py_ bestand, als het toegangspunt voor uw Azure-functie.

```json
{
  "scriptFile": "main.py",
  "entryPoint": "customentry",
  ...
}
```

Gegevens van triggers en bindingen is gebonden aan de functie via methode kenmerken met behulp van de `name` -eigenschap worden gedefinieerd de `function.json` configuratiebestand. Bijvoorbeeld, de _function.json_ wordt hieronder beschreven een eenvoudige functie geactiveerd door een HTTP-aanvraag met de naam `req`:

```json
{
  "bindings": [
    {
      "name": "req",
      "direction": "in",
      "type": "httpTrigger",
      "authLevel": "anonymous"
    },
    {
      "name": "$return",
      "direction": "out",
      "type": "http"
    }
  ]
}
```

De `__init__.py` -bestand bevat de volgende functiecode:

```python
def main(req):
    user = req.params.get('user')
    return f'Hello, {user}!'
```

U kunt eventueel ook de parametertypen declareren en type in de functie met behulp van Python type aantekeningen retourneren. Dezelfde functie kan bijvoorbeeld worden geschreven met behulp van aantekeningen, als volgt:

```python
import azure.functions

def main(req: azure.functions.HttpRequest) -> str:
    user = req.params.get('user')
    return f'Hello, {user}!'
```  

Gebruik de Python-aantekeningen opgenomen in de [azure.functions.*](/python/api/azure-functions/azure.functions?view=azure-python) pakket invoer en uitvoer binden aan uw methoden. 

## <a name="folder-structure"></a>mapstructuur

De mapstructuur voor een Python-Functions-project ziet er als volgt uit:

```
 FunctionApp
 | - MyFirstFunction
 | | - __init__.py
 | | - function.json
 | - MySecondFunction
 | | - __init__.py
 | | - function.json
 | - SharedCode
 | | - myFirstHelperFunction.py
 | | - mySecondHelperFunction.py
 | - host.json
 | - requirements.txt
 | - extensions.csproj
 | - bin
```

Er is een gedeelde [host.json](functions-host-json.md) -bestand dat kan worden gebruikt voor het configureren van de functie-app. Elke functie heeft een eigen codebestand en het configuratiebestand van de binding (function.json). 

Gedeelde code moet worden opgeslagen in een afzonderlijke map. Als u wilt verwijzen naar modules in de map SharedCode, kunt u de volgende syntaxis:

```
from ..SharedCode import myFirstHelperFunction
```

Bindinguitbreidingen die worden gebruikt door de Functions-runtime worden gedefinieerd de `extensions.csproj` bestand met de werkelijke dll-bestanden in de `bin` map. Als u lokaal ontwikkelt, moet u [bindinguitbreidingen registreren](./functions-bindings-register.md#local-development-azure-functions-core-tools) met behulp van Azure Functions Core Tools. 

Wanneer een Functions-project naar uw functie-app in Azure implementeert, moet de volledige inhoud van de map FunctionApp worden opgenomen in het pakket, maar niet de map zelf.

## <a name="inputs"></a>Invoer

Invoer zijn onderverdeeld in twee categorieën in Azure Functions: invoer voor de werkstroomtrigger en aanvullende invoer. Hoewel ze in verschillende zijn `function.json`, de syntaxis is vrijwel identiek in Python-code. We houden het volgende codefragment als voorbeeld:

```json
{
  "scriptFile": "__init__.py",
  "bindings": [
    {
      "name": "req",
      "direction": "in",
      "type": "httpTrigger",
      "authLevel": "anonymous",
      "route": "items/{id}"
    },
    {
      "name": "obj",
      "direction": "in",
      "type": "blob",
      "path": "samples/{id}",
      "connection": "AzureWebJobsStorage"
    }
  ]
}
```

```python
import azure.functions as func
import logging

def main(req: func.HttpRequest,
         obj: func.InputStream):

    logging.info(f'Python HTTP triggered function processed: {obj.read()}')
```

Wanneer de functie is aangeroepen, de HTTP-aanvraag wordt doorgegeven aan de functie als `req`. Een vermelding wordt opgehaald uit de Azure Blob-opslag op basis van de _id_ in de URL van de route en beschikbaar gesteld als `obj` in de hoofdtekst van de functie.

## <a name="outputs"></a>Uitvoer

Uitvoer kan worden uitgedrukt in geretourneerde waarde en de output-parameters. Als er slechts één uitvoer, wordt u aangeraden de geretourneerde waarde. Voor meerdere uitvoer hebt u output-parameters gebruiken.

De geretourneerde waarde van een functie gebruiken als de waarde van een Uitvoerbinding de `name` eigenschap van de binding moet worden ingesteld op `$return` in `function.json`.

Als u wilt meerdere uitvoer produceren, gebruiken de `set()` methode geleverd door de `azure.functions.Out` interface naar een waarde toewijzen aan de binding. De volgende functie kan bijvoorbeeld een bericht naar een wachtrij worden gepusht en ook een HTTP-antwoord retourneren.

```json
{
  "scriptFile": "__init__.py",
  "bindings": [
    {
      "name": "req",
      "direction": "in",
      "type": "httpTrigger",
      "authLevel": "anonymous"
    },
    {
      "name": "msg",
      "direction": "out",
      "type": "queue",
      "queueName": "outqueue",
      "connection": "AzureWebJobsStorage"
    },
    {
      "name": "$return",
      "direction": "out",
      "type": "http"
    }
  ]
}
```

```python
import azure.functions as func

def main(req: func.HttpRequest,
         msg: func.Out[func.QueueMessage]) -> str:

    message = req.params.get('body')
    msg.set(message)
    return message
```

## <a name="logging"></a>Logboekregistratie

Toegang tot de Azure Functions runtime logger is beschikbaar via een hoofdmap [ `logging` ](https://docs.python.org/3/library/logging.html#module-logging) -handler in uw functie-app. In dit logboek is gekoppeld aan Application Insights en kunt u met de vlag-waarschuwingen en fouten aangetroffen tijdens het uitvoeren van de functie.

Het volgende voorbeeld registreert een bericht met apparaatgegevens wanneer de functie is aangeroepen via een HTTP-trigger.

```python
import logging

def main(req):
    logging.info('Python HTTP trigger function processed a request.')
```

Er zijn aanvullende logboekregistratiemethoden beschikbaar waarmee u naar de console op verschillende traceringsniveaus schrijven:

| Methode                 | Description                                |
| ---------------------- | ------------------------------------------ |
| logboekregistratie. **kritieke (_bericht_)**   | Schrijft een bericht met niveau kritiek op het logboek van de hoofdmap.  |
| logboekregistratie. **fout (_bericht_)**   | Schrijft een bericht met de fout op niveau van het root-logboek.    |
| logboekregistratie. **waarschuwing (_bericht_)**    | Schrijft een bericht met het niveau waarschuwing op het logboek van de hoofdmap.  |
| logboekregistratie. **info (_bericht_)**    | Schrijft een bericht met niveau INFO op het logboek van de hoofdmap.  |
| logboekregistratie. **foutopsporing (_bericht_)** | Schrijft een bericht met niveau FOUTOPSPORING op de logger hoofdmap.  |

## <a name="importing-shared-code-into-a-function-module"></a>Gedeelde code importeren in een functiemodule

Python-modules die zijn gepubliceerd samen met de functie modules moeten worden geïmporteerd met behulp van de syntaxis van de relatieve importeren:

```python
from . import helpers  # Use more dots to navigate up the folder structure.
def main(req: func.HttpRequest):
    helpers.process_http_request(req)
```

U kunt ook gedeelde code in een zelfstandige pakket geplaatst, deze publiceren naar een openbare of een persoonlijke PyPI-exemplaar en dit opgeven als een reguliere afhankelijkheid.

## <a name="async"></a>Async

Aangezien een enkel Python proces per functie-app kan bestaan, wordt u aangeraden voor het implementeren van uw Azure-functie als een asynchrone coroutine met behulp van de `async def` instructie.

```python
# Will be run with asyncio directly
async def main():
    await some_nonblocking_socket_io_op()
```

Als de functie main() synchroon is (geen `async` kwalificatie) wordt automatisch uitgevoerd een `asyncio` thread-pool.

```python
# Would be run in an asyncio thread-pool
def main():
    some_blocking_socket_io()
```

## <a name="context"></a>Context

Als u de context van de aanroep van een functie tijdens de uitvoering, bevatten de `context` argument in de handtekening. 

Bijvoorbeeld:

```python
import azure.functions

def main(req: azure.functions.HttpRequest,
            context: azure.functions.Context) -> str:
    return f'{context.invocation_id}'
```

De **Context** klasse heeft de volgende methoden:

`function_directory`  
De map waarin de functie wordt uitgevoerd.

`function_name`  
Naam van de functie.

`invocation_id`  
ID van de huidige functie-aanroep.

## <a name="python-version-and-package-management"></a>Python-versie en package management

Azure Functions ondersteunt momenteel alleen Python 3.6.x (officiële CPython distributie).

Bij het ontwikkelen van lokaal via de Azure Functions Core Tools of Visual Studio Code, voeg de namen en versies van de vereiste pakketten naar de `requirements.txt` bestands- en deze installeren met behulp van `pip`.

Bijvoorbeeld, de volgende vereisten voor bestands- en pip opdracht kan worden gebruikt voor het installeren van de `requests` pakket van PyPI.

```bash
pip install requests
```

```txt
requests==2.19.1
```

```bash
pip install -r requirements.txt
```

Wanneer u klaar voor publicatie bent, zorg ervoor dat alle afhankelijkheden zijn vermeld de `requirements.txt` bestand, dat zich bevindt in de hoofdmap van de projectmap. Om uit te voeren is uw Azure-functies, moet het bestand vereisten minimaal de volgende pakketten bevatten:

```txt
azure-functions
azure-functions-worker
grpcio==1.14.1
grpcio-tools==1.14.1
protobuf==3.6.1
six==1.11.0
```

## <a name="publishing-to-azure"></a>Publiceren naar Azure

Als u een pakket dat een compiler vereist en biedt geen ondersteuning voor de installatie van manylinux-compatibele wheels van PyPI, mislukken publiceren naar Azure met de volgende fout: 

```
There was an error restoring dependencies.ERROR: cannot install <package name - version> dependency: binary dependencies without wheels are not supported.  
The terminal process terminated with exit code: 1
```

Automatisch installeren en configureren van de vereiste binaire bestanden, [Docker installeren](https://docs.docker.com/install/) op uw lokale computer en voer de volgende opdracht om te publiceren met behulp van de [Azure Functions Core Tools](functions-run-local.md#v2) (func). Vervang `<app name>` met de naam van uw functie-app in Azure. 

```bash
func azure functionapp publish <app name> --build-native-deps
```

Eronder, Core Tools docker wordt gebruikt om uit te voeren de [mcr.microsoft.com/azure-functions/python](https://hub.docker.com/r/microsoft/azure-functions/) afbeelding als een container op uw lokale computer. Met behulp van deze omgeving, deze vervolgens ontwikkelt en de vereiste modules te installeren vanuit de bron-distributie, voordat ze verpakking voor laatste implementatie naar Azure.

> [!NOTE]
> Het programma PyInstaller Core Tools (func) gebruikt voor het blokkeren van de gebruiker code en afhankelijkheden in een enkele zelfstandig uitvoerbaar bestand om uit te voeren in Azure. Deze functionaliteit is momenteel in preview en kan niet van toepassing op alle typen pakketten voor Python. Als u niet voor het importeren van modules, probeert u te publiceren opnieuw met behulp van de `--no-bundler` optie. 
> ```
> func azure functionapp publish <app_name> --build-native-deps --no-bundler
> ```
> Als u problemen zich blijft voordoen, laat het ons weten door [openen van een probleem](https://github.com/Azure/azure-functions-core-tools/issues/new) en een beschrijving van het probleem. 


Voor het bouwen van uw afhankelijkheden en publiceren met behulp van een continue integratie (CI) en continue levering (CD) systeem, kunt u een [Azure pijplijn](https://docs.microsoft.com/azure/devops/pipelines/get-started-yaml?view=vsts) of [Travis CI aangepast script](https://docs.travis-ci.com/user/deployment/script/). 

Hieronder volgt een voorbeeld `azure-pipelines.yml` script voor het bouwen en publiceren.
```yml
pool:
  vmImage: 'Ubuntu 16.04'

steps:
- task: NodeTool@0
  inputs:
    versionSpec: '8.x'

- script: |
    set -e
    echo "deb [arch=amd64] https://packages.microsoft.com/repos/azure-cli/ wheezy main" | sudo tee /etc/apt/sources.list.d/azure-cli.list
    curl -L https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
    sudo apt-get install -y apt-transport-https
    echo "install Azure CLI..."
    sudo apt-get update && sudo apt-get install -y azure-cli
    npm i -g azure-functions-core-tools --unsafe-perm true
    echo "installing dotnet core"
    curl -sSL https://dot.net/v1/dotnet-install.sh | bash /dev/stdin --channel 2.0
- script: |
    set -e
    az login --service-principal --username "$(APP_ID)" --password "$(PASSWORD)" --tenant "$(TENANT_ID)" 
    func settings add FUNCTIONS_WORKER_RUNTIME python
    func extensions install
    func azure functionapp publish $(APP_NAME) --build-native-deps
```

Hieronder volgt een voorbeeld `.travis.yaml` script voor het bouwen en publiceren.

```yml
sudo: required

language: node_js

node_js:
  - "8"

services:
  - docker

before_install:
  - echo "deb [arch=amd64] https://packages.microsoft.com/repos/azure-cli/ wheezy main" | sudo tee /etc/apt/sources.list.d/azure-cli.list
  - curl -L https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
  - sudo apt-get install -y apt-transport-https
  - sudo apt-get update && sudo apt-get install -y azure-cli
  - npm i -g azure-functions-core-tools --unsafe-perm true


script:
  - az login --service-principal --username "$APP_ID" --password "$PASSWORD" --tenant "$TENANT_ID"
  - az account get-access-token --query "accessToken" | func azure functionapp publish $APP_NAME --build-native-deps

```

## <a name="known-issues-and-faq"></a>Bekende problemen en veelgestelde vragen

Alle bekende problemen en functie-aanvragen zijn getraceerd door [GitHub issues](https://github.com/Azure/azure-functions-python-worker/issues) lijst. Als u een probleem opgetreden bij het probleem niet kunt vinden in GitHub, opent u een nieuw probleem en bevat een gedetailleerde beschrijving van het probleem.

## <a name="next-steps"></a>Volgende stappen

Zie de volgende bronnen voor meer informatie:

* [Aanbevolen procedures voor Azure Functions](functions-best-practices.md)
* [Azure Functions-triggers en bindingen](functions-triggers-bindings.md)
* [BLOB storage-bindingen](functions-bindings-storage-blob.md)
* [HTTP- en Webhook-bindingen](functions-bindings-http-webhook.md)
* [Queue storage-bindingen](functions-bindings-storage-queue.md)
* [Timertrigger](functions-bindings-timer.md)
