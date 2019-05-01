---
title: Een Azure Storage-wachtrij-binding toevoegen aan uw Python-functie
description: Informatie over het toevoegen van een Azure Storage queue-Uitvoerbinding naar de Python-functie met behulp van de Azure CLI en Functions Core Tools.
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
ms.openlocfilehash: aaeee4238110faa7a842073af8431b30b885db3c
ms.sourcegitcommit: e7d4881105ef17e6f10e8e11043a31262cfcf3b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/29/2019
ms.locfileid: "64870038"
---
# <a name="add-an-azure-storage-queue-binding-to-your-function"></a>Een Azure Storage-wachtrij-binding toevoegen aan uw functie

Azure Functions kunt u Azure-services en andere resources verbinding maken met functions zonder te hoeven schrijven van uw eigen integratiecode. Deze *bindingen*, die staan voor zowel invoer en uitvoer, zijn gedefinieerd binnen de functiedefinitie. Gegevens van bindingen is opgegeven voor de functie als parameters. Een trigger is een speciaal type Invoerbinding. Als een functie slechts één trigger heeft, kan deze hebben verschillende invoer en uitvoerbindingen. Zie voor meer informatie, [Azure Functions-triggers en bindingen concepten](functions-triggers-bindings.md).

Dit artikel leest u over het integreren van de functie die u hebt gemaakt in de [vorige artikel](functions-create-first-function-python.md) met een Azure Storage-wachtrij. De Uitvoerbinding die u aan deze functie toevoegt schrijft gegevens uit de HTTP-aanvraag naar een bericht in de wachtrij. 

De meeste bindingen vereisen een opgeslagen verbindingsreeks die functies gebruikt voor toegang tot de afhankelijke service. Te vereenvoudigen, kunt u het opslagaccount dat u hebt gemaakt met de functie-app gebruiken. De verbinding met dit account is al opgeslagen in een app-instelling met de naam `AzureWebJobsStorage`.  

## <a name="prerequisites"></a>Vereisten

Voordat u dit artikel, voer de stappen in [deel 1 van de Python-snelstartgids](functions-create-first-function-python.md).

## <a name="download-the-function-app-settings"></a>De instellingen voor de functie-app downloaden

In de vorige artikel, moet u een functie-app in Azure, samen met een opslagaccount gemaakt. De verbindingsreeks voor dit account veilig opgeslagen in de instellingen van de app in Azure. In dit artikel, kunt u berichten schrijven naar een opslagwachtrij van hetzelfde account. Voor verbinding met uw Storage-account wanneer de functie lokaal wordt uitgevoerd, moet u app-instellingen downloaden naar het bestand local.settings.json. Voer de volgende opdracht uit de Azure Functions Core Tools-opdracht voor het downloaden van de instellingen te voegen aan local.settings.json, vervangen `<APP_NAME>` met de naam van uw functie-app uit de vorige artikel:

```bash
func azure functionapp fetch-app-settings <APP_NAME>
```

Mogelijk moet u zich aanmeldt bij uw Azure-account.

> [!IMPORTANT]  
> Omdat deze geheimen bevat, het bestand local.settings.json nooit wordt gepubliceerd en deze moet worden uitgesloten van broncodebeheer.

U moet de waarde `AzureWebJobsStorage`, dit is de verbindingsreeks van de Storage-account. U deze verbinding gebruiken om te controleren of de Uitvoerbinding werkt zoals verwacht.

## <a name="add-an-output-binding"></a>Een uitvoerbinding toevoegen

In de functies, elk type binding vereist een `direction`, `type`, en een unieke `name` worden gedefinieerd in de function.json-bestand. Afhankelijk van het bindingstype mogelijk extra eigenschappen vereist. De [configuratie van de wachtrij uitvoer](functions-bindings-storage-queue.md#output---configuration) beschrijft de velden die nodig zijn voor een Azure Storage-wachtrij-binding.

Voor het maken van een binding die u toevoegt een configuratieobject voor de binding aan de `function.json` bestand. Bewerk het bestand function.json in uw map HttpTrigger om toe te voegen een object dat moet worden de `bindings` matrix met de volgende eigenschappen:

| Eigenschap | Waarde | Description |
| -------- | ----- | ----------- |
| **`name`** | `msg` | Naam ter identificatie van de bindingsparameter waarnaar wordt verwezen in uw code. |
| **`type`** | `queue` | De binding is een Azure Storage-wachtrij-binding. |
| **`direction`** | `out` | De binding is een Uitvoerbinding. |
| **`queueName`** | `outqueue` | De naam van de wachtrij die de binding naar schrijft. Wanneer de *queueName* niet bestaat, de binding wordt deze gemaakt bij het eerste gebruik. |
| **`connection`** | `AzureWebJobsStorage` | De naam van een app-instelling met de verbindingsreeks voor de Storage-account. De `AzureWebJobsStorage` instelling bevat de verbindingsreeks voor het Opslagaccount dat u hebt gemaakt met de functie-app. |

Het bestand function.json moet er nu uitzien als in het volgende voorbeeld:

```json
{
  "scriptFile": "__init__.py",
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
    },
  {
      "type": "queue",
      "direction": "out",
      "name": "msg",
      "queueName": "outqueue",
      "connection": "AzureWebJobsStorage"
    }
  ]
}
```

## <a name="add-code-that-uses-the-output-binding"></a>Code toevoegen die gebruikmaakt van de uitvoerbinding

Nadat deze geconfigureerd, kunt u beginnen met behulp van de `name` van de binding om deze te openen als een methode-kenmerk in de functiehandtekening. In het volgende voorbeeld `msg` is een exemplaar van de [ `azure.functions.InputStream class` ](/python/api/azure-functions/azure.functions.httprequest).

```python
import logging

import azure.functions as func


def main(req: func.HttpRequest, msg: func.Out[func.QueueMessage]) -> str:

    name = req.params.get('name')
    if not name:
        try:
            req_body = req.get_json()
        except ValueError:
            pass
        else:
            name = req_body.get('name')

    if name:
        msg.set(name)
        return func.HttpResponse(f"Hello {name}!")
    else:
        return func.HttpResponse(
             "Please pass a name on the query string or in the request body",
             status_code=400
        )
```

Met behulp van een Uitvoerbinding, kunt u geen gebruik van de Azure Storage-SDK-code voor verificatie ophalen van de verwijzing naar een wachtrij of schrijven van gegevens. De Azure Functions-runtime en queue-uitvoer binding die taken voor u doen.

## <a name="run-the-function-locally"></a>De functie lokaal uitvoeren

Gebruik de volgende opdracht als eerder de Functions-runtime lokaal starten:

```bash
func host start
```

> [!NOTE]  
> Omdat het vorige artikel had u bij het inschakelen van extensie-pakketten in de host.json de [opslag bindingsuitbreiding](functions-bindings-storage-blob.md#packages---functions-2x) is gedownload en geïnstalleerd voor u tijdens het opstarten.

Kopieer de URL van uw `HttpTrigger`-functie uit de uitvoer van de runtime en plak deze in de adresbalk van uw browser. Voeg de queryreeks `?name=<yourname>` toe aan de URL en voer de aanvraag uit. Als u in het vorige artikel hebt gedaan, moet u hetzelfde antwoord in de browser zien.

Deze keer de Uitvoerbinding maakt ook een wachtrij met de naam `outqueue` in uw Storage-account en wordt een bericht met deze dezelfde tekenreeks.

Vervolgens gebruikt u de Azure CLI voor het weergeven van de nieuwe wachtrij en controleren of een bericht is toegevoegd. U kunt ook bekijken uw wachtrij met de [Microsoft Azure Storage Explorer] [ Azure Storage Explorer] of in de [Azure-portal](https://portal.azure.com).

### <a name="set-the-storage-account-connection"></a>Stel de verbinding met Opslagaccount

Open het bestand local.settings.json en kopieer de waarde van `AzureWebJobsStorage`, dit is de verbindingsreeks van de Storage-account. Stel de `AZURE_STORAGE_CONNECTION_STRING` omgevingsvariabele op de verbindingstekenreeks die met de volgende Bash-opdracht:

```azurecli-interactive
export AZURE_STORAGE_CONNECTION_STRING=<STORAGE_CONNECTION_STRING>
```

Met de verbindingsreeks instellen in de `AZURE_STORAGE_CONNECTION_STRING` omgevingsvariabele, u kunt toegang krijgen tot uw Storage-account zonder om verificatie elke keer te bieden.

### <a name="query-the-storage-queue"></a>Query uitvoeren op de Storage-wachtrij

U kunt de [ `az storage queue list` ](/cli/azure/storage/queue#az-storage-queue-list) opdracht om de Storage-wachtrijen in uw account, zoals in het volgende voorbeeld weer te geven:

```azurecli-interactive
az storage queue list --output tsv
```

De uitvoer van deze opdracht bevat een wachtrij met de naam `outqueue`, dit is de wachtrij dat is gemaakt terwijl de functie werd uitgevoerd.

Gebruik vervolgens de [ `az storage message peek` ](/cli/azure/storage/message#az-storage-message-peek) opdracht om de berichten in deze wachtrij, zoals in het volgende voorbeeld weer te geven.

```azurecli-interactive
echo `echo $(az storage message peek --queue-name outqueue -o tsv --query '[].{Message:content}') | base64 --decode`
```

De geretourneerde tekenreeks moet gelijk zijn aan het bericht om de functie te testen.

> [!NOTE]  
> Het vorige voorbeeld decodeert de geretourneerde tekenreeks van base64. Dit komt doordat de Queue storage-bindingen van Azure Storage als lezen en naar schrijven [base64 tekenreeksen](functions-bindings-storage-queue.md#encoding).

Nu is het tijd om opnieuw te publiceren van de bijgewerkte functie-app naar Azure.

[!INCLUDE [functions-publish-project](../../includes/functions-publish-project.md)]

Nogmaals, kunt u cURL of een browser om de geïmplementeerde functie te testen. Net als voorheen Voeg de queryreeks `&name=<yourname>` naar de URL, zoals in het volgende voorbeeld:

```bash
curl https://myfunctionapp.azurewebsites.net/api/httptrigger?code=cCr8sAxfBiow548FBDLS1....&name=<yourname>
```

U kunt [onderzoekt de opslagwachtrijbericht](#query-the-storage-queue) om te controleren dat de Uitvoerbinding opnieuw worden gegenereerd voor een nieuw bericht in de wachtrij.

[!INCLUDE [functions-cleanup-resources](../../includes/functions-cleanup-resources.md)]

## <a name="next-steps"></a>Volgende stappen

U kunt uw HTTP-geactiveerde functie voor het schrijven van gegevens in een opslagwachtrij hebt bijgewerkt. Zie voor meer informatie over het ontwikkelen van Azure Functions met behulp van Python, de [ontwikkelaarsgids van Azure Functions Python](functions-reference-python.md) en [Azure Functions-triggers en bindingen](functions-triggers-bindings.md).

Vervolgens moet u de bewaking van Application Insights voor uw functie-app inschakelen:

> [!div class="nextstepaction"]
> [Application Insights-integratie inschakelen](functions-monitoring.md#manually-connect-an-app-insights-resource)

[Azure Storage Explorer]: https://storageexplorer.com/