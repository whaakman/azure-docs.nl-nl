---
title: Naslaginformatie over Python-ontwikkelaars voor Azure Functions
description: Meer informatie over het ontwikkelen van functies met Python
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
keywords: Azure functions, functies, gebeurtenisverwerking, dynamisch berekenen, architectuur zonder server, python
ms.service: azure-functions
ms.devlang: python
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 04/16/2018
ms.author: glenga
ms.openlocfilehash: 14594e95efe94fe38502dc6269627158c42a04be
ms.sourcegitcommit: dda9fc615db84e6849963b20e1dce74c9fe51821
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/08/2019
ms.locfileid: "67622360"
---
# <a name="azure-functions-python-developer-guide"></a>Handleiding voor ontwikkelaars van Azure Functions-Python

In dit artikel bevat een inleiding tot het ontwikkelen van Azure Functions met behulp van Python. De onderstaande inhoud wordt ervan uitgegaan dat u al hebt gelezen de [handleiding voor ontwikkelaars van Azure Functions voor](functions-reference.md).

[!INCLUDE [functions-python-preview-note](../../includes/functions-python-preview-note.md)]

## <a name="programming-model"></a>Programmeermodel

Een Azure-functie moet een stateless methode in uw Python-script waarmee invoer worden verwerkt en geeft een resultaat. Standaard de runtime wordt verwacht dat de methode om te worden geïmplementeerd als een algemene methode met de naam `main()` in de `__init__.py` bestand.

U kunt de standaard-configuratie wijzigen door op te geven de `scriptFile` en `entryPoint` eigenschappen in de *function.json* bestand. Bijvoorbeeld, de _function.json_ hieronder vertelt de runtime te gebruiken de `customentry()` methode in de _main.py_ bestand, als het toegangspunt voor uw Azure-functie.

```json
{
  "scriptFile": "main.py",
  "entryPoint": "customentry",
  ...
}
```

Gegevens van triggers en bindingen is gebonden aan de functie via methode kenmerken met behulp van de `name` -eigenschap worden gedefinieerd de *function.json* bestand. Bijvoorbeeld, de _function.json_ wordt hieronder beschreven een eenvoudige functie geactiveerd door een HTTP-aanvraag met de naam `req`:

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

(Optioneel) als u wilt gebruikmaken van de intellisense en automatisch aanvullen functies die worden geleverd door uw code-editor, kunt u ook de kenmerktypen zo declareren en retourtype in de functie met behulp van Python type aantekeningen. 

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
```

Er is een gedeelde [host.json](functions-host-json.md) -bestand dat kan worden gebruikt voor het configureren van de functie-app. Elke functie heeft een eigen codebestand en het configuratiebestand van de binding (function.json). 

Gedeelde code moet worden opgeslagen in een afzonderlijke map. Als u wilt verwijzen naar modules in de map SharedCode, kunt u de volgende syntaxis:

```
from __app__.SharedCode import myFirstHelperFunction
```

Bij het implementeren van een functieproject naar uw functie-app in Azure, de volledige inhoud van de *FunctionApp* map moet worden opgenomen in het pakket, maar niet de map zelf.

## <a name="triggers-and-inputs"></a>Triggers en -invoer

Invoer zijn onderverdeeld in twee categorieën in Azure Functions: invoer voor de werkstroomtrigger en aanvullende invoer. Hoewel ze verschillen in de `function.json` -bestand, gebruik is identiek in Python-code.  Tekenreeksen voor databaseverbindingen of geheimen voor trigger en invoer bronnen toewijzen aan de waarden in de `local.settings.json` bestand bij lokale uitvoering en de toepassingsinstellingen bij het uitvoeren in Azure. 

Bijvoorbeeld, toont de volgende code het verschil tussen de twee:

```json
// function.json
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

```json
// local.settings.json
{
  "IsEncrypted": false,
  "Values": {
    "FUNCTIONS_WORKER_RUNTIME": "python",
    "AzureWebJobsStorage": "<azure-storage-connection-string>"
  }
}
```

```python
# __init__.py
import azure.functions as func
import logging


def main(req: func.HttpRequest,
         obj: func.InputStream):

    logging.info(f'Python HTTP triggered function processed: {obj.read()}')
```

Wanneer de functie is aangeroepen, de HTTP-aanvraag wordt doorgegeven aan de functie als `req`. Een vermelding wordt opgehaald uit de Azure Blob-opslag op basis van de _ID_ in de URL van de route en beschikbaar gesteld als `obj` in de hoofdtekst van de functie.  Hier het opslagaccount dat is opgegeven wordt de verbindingsreeks gevonden `AzureWebJobsStorage` die hetzelfde opslagaccount die worden gebruikt door de functie-app is.


## <a name="outputs"></a>outputs

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

## <a name="async"></a>Async

Het is raadzaam dat u uw Azure-functie als het gebruik van een asynchrone coroutine schrijft de `async def` instructie.

```python
# Will be run with asyncio directly


async def main():
    await some_nonblocking_socket_io_op()
```

Als de functie main() synchroon is (geen `async` kwalificatie) we de functie voor het automatisch uitvoeren in een `asyncio` thread-pool.

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

## <a name="global-variables"></a>Globale variabelen

Is er geen garantie dat de status van uw app voor toekomstige uitvoeringen behouden blijft. De Azure Functions-runtime hergebruikt echter vaak dezelfde procedure voor meerdere uitvoeringen van dezelfde app. Als u wilt de resultaten van een dure berekening in cache opslaan, kunt u het declareren als een globale variabele. 

```python
CACHED_DATA = None


def main(req):
    global CACHED_DATA
    if CACHED_DATA is None:
        CACHED_DATA = load_json()

    # ... use CACHED_DATA in code
```

## <a name="python-version-and-package-management"></a>Python-versie en package management

Azure Functions ondersteunt momenteel alleen Python 3.6.x (officiële CPython distributie).

Bij het ontwikkelen van lokaal via de Azure Functions Core Tools of Visual Studio Code, voeg de namen en versies van de vereiste pakketten naar de `requirements.txt` bestands- en deze installeren met behulp van `pip`.

Bijvoorbeeld, de volgende vereisten voor bestands- en pip opdracht kan worden gebruikt voor het installeren van de `requests` pakket van PyPI.

```txt
requests==2.19.1
```

```bash
pip install -r requirements.txt
```

## <a name="publishing-to-azure"></a>Publiceren naar Azure

Wanneer u klaar om te publiceren bent, zorg ervoor dat alle afhankelijkheden zijn vermeld de *requirements.txt* -bestand, dat zich bevindt in de hoofdmap van de projectmap. Als u een pakket dat een compiler vereist en biedt geen ondersteuning voor de installatie van manylinux-compatibele wheels van PyPI, mislukken publiceren naar Azure met de volgende fout: 

```
There was an error restoring dependencies.ERROR: cannot install <package name - version> dependency: binary dependencies without wheels are not supported.  
The terminal process terminated with exit code: 1
```

Automatisch installeren en configureren van de vereiste binaire bestanden, [Docker installeren](https://docs.docker.com/install/) op uw lokale computer en voer de volgende opdracht om te publiceren met behulp van de [Azure Functions Core Tools](functions-run-local.md#v2) (func). Vervang `<app name>` met de naam van uw functie-app in Azure. 

```bash
func azure functionapp publish <app name> --build-native-deps
```

Eronder, Core Tools docker wordt gebruikt om uit te voeren de [mcr.microsoft.com/azure-functions/python](https://hub.docker.com/r/microsoft/azure-functions/) afbeelding als een container op uw lokale computer. Met behulp van deze omgeving, deze vervolgens ontwikkelt en de vereiste modules te installeren vanuit de bron-distributie, voordat ze verpakking voor laatste implementatie naar Azure.

De afhankelijkheden maken en publiceren met behulp van een systeem voor continue levering (CD), [gebruiken Azure DevOps pijplijnen](https://docs.microsoft.com/azure/azure-functions/functions-how-to-azure-devops). 

## <a name="unit-testing"></a>Moduletests uitvoeren

Functies die zijn geschreven in Python kunnen worden getest als de andere Python-code met behulp van standaard testen frameworks. Voor de meeste bindingen, is het mogelijk te maken van een mock-invoerobject met het maken van een exemplaar van een juiste klasse van de `azure.functions` pakket. Omdat de [ `azure.functions` ](https://pypi.org/project/azure-functions/) pakket niet onmiddellijk beschikbaar is, zorg ervoor dat u installeert via uw `requirements.txt` zoals beschreven in het bestand [Python-versie en package management](#python-version-and-package-management) hierboven.

Volgende is bijvoorbeeld een mock een door HTTP geactiveerde functie te testen:

```json
{
  "scriptFile": "httpfunc.py",
  "entryPoint": "my_function",
  "bindings": [
    {
      "authLevel": "function",
      "type": "httpTrigger",
      "direction": "in",
      "name": "req",
      "methods": [
        "get",
        "post"
      ]
    },
    {
      "type": "http",
      "direction": "out",
      "name": "$return"
    }
  ]
}
```

```python
# myapp/httpfunc.py
import azure.functions as func
import logging

def my_function(req: func.HttpRequest) -> func.HttpResponse:
    logging.info('Python HTTP trigger function processed a request.')

    name = req.params.get('name')
    if not name:
        try:
            req_body = req.get_json()
        except ValueError:
            pass
        else:
            name = req_body.get('name')

    if name:
        return func.HttpResponse(f"Hello {name}")
    else:
        return func.HttpResponse(
             "Please pass a name on the query string or in the request body",
             status_code=400
        )
```

```python
# myapp/test_httpfunc.py
import unittest

import azure.functions as func
from httpfunc import my_function


class TestFunction(unittest.TestCase):
    def test_my_function(self):
        # Construct a mock HTTP request.
        req = func.HttpRequest(
            method='GET',
            body=None,
            url='/api/HttpTrigger',
            params={'name': 'Test'})

        # Call the function.
        resp = my_function(req)

        # Check the output.
        self.assertEqual(
            resp.get_body(),
            b'Hello Test',
        )
```

Hier volgt een voorbeeld, met een wachtrij geactiveerde functie:

```python
# myapp/__init__.py
import azure.functions as func


def my_function(msg: func.QueueMessage) -> str:
    return f'msg body: {msg.get_body().decode()}'
```

```python
# myapp/test_func.py
import unittest

import azure.functions as func
from . import my_function


class TestFunction(unittest.TestCase):
    def test_my_function(self):
        # Construct a mock Queue message.
        req = func.QueueMessage(
            body=b'test')

        # Call the function.
        resp = my_function(req)

        # Check the output.
        self.assertEqual(
            resp,
            'msg body: test',
        )
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
