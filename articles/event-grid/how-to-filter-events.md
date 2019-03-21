---
title: Het filteren van gebeurtenissen voor Azure Event Grid
description: Laat zien hoe Azure Event Grid-abonnementen die gebeurtenissen filteren maken.
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: conceptual
ms.date: 01/07/2019
ms.author: spelluru
ms.openlocfilehash: 182a936e97cd6ed2527d618dfe777ae861c757e3
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58182248"
---
# <a name="filter-events-for-event-grid"></a>Gebeurtenissen filteren op Event Grid

In dit artikel laat zien hoe filteren van gebeurtenissen bij het maken van een Event Grid-abonnement. Zie voor meer informatie over de opties voor het filteren van gebeurtenissen, [begrijpen gebeurtenis voor Event Grid-abonnementen filteren](event-filtering.md).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="filter-by-event-type"></a>Filteren op gebeurtenistype

Bij het maken van een Event Grid-abonnement, kunt u aangeven welke [gebeurtenistypen](event-schema.md) om naar het eindpunt te verzenden. De voorbeelden in deze sectie gebeurtenisabonnementen voor een resourcegroep maken, maar beperken van de gebeurtenissen die worden verzonden naar `Microsoft.Resources.ResourceWriteFailure` en `Microsoft.Resources.ResourceWriteSuccess`. Als u meer flexibiliteit nodig hebt bij het filteren van gebeurtenissen op gebeurtenistypen, ziet u filteren op geavanceerde operators en velden.

Gebruik voor PowerShell de `-IncludedEventType` parameter bij het maken van het abonnement.

```powershell
$includedEventTypes = "Microsoft.Resources.ResourceWriteFailure", "Microsoft.Resources.ResourceWriteSuccess"

New-AzEventGridSubscription `
  -EventSubscriptionName demoSubToResourceGroup `
  -ResourceGroupName myResourceGroup `
  -Endpoint <endpoint-URL> `
  -IncludedEventType $includedEventTypes
```

Voor Azure CLI, gebruikt u de `--included-event-types` parameter. Het volgende voorbeeld maakt gebruik van Azure CLI in een Bash-shell:

```azurecli
includedEventTypes="Microsoft.Resources.ResourceWriteFailure Microsoft.Resources.ResourceWriteSuccess"

az eventgrid event-subscription create \
  --name demoSubToResourceGroup \
  --resource-group myResourceGroup \
  --endpoint <endpoint-URL> \
  --included-event-types $includedEventTypes
```

Voor een Resource Manager-sjabloon, gebruikt u de `includedEventTypes` eigenschap.

```json
"resources": [
  {
    "type": "Microsoft.EventGrid/eventSubscriptions",
    "name": "[parameters('eventSubName')]",
    "apiVersion": "2018-09-15-preview",
    "properties": {
      "destination": {
        "endpointType": "WebHook",
        "properties": {
          "endpointUrl": "[parameters('endpoint')]"
        }
      },
      "filter": {
        "subjectBeginsWith": "",
        "subjectEndsWith": "",
        "isSubjectCaseSensitive": false,
        "includedEventTypes": [
          "Microsoft.Resources.ResourceWriteFailure",
          "Microsoft.Resources.ResourceWriteSuccess"
        ]
      }
    }
  }
]
```

## <a name="filter-by-subject"></a>Filteren op onderwerp

U kunt gebeurtenissen filteren op het onderwerp in de gebeurtenisgegevens. U kunt een waarde voor het begin of einde van de certificaathouder opgeven. Als u meer flexibiliteit nodig hebt bij het filteren van gebeurtenissen op onderwerp, raadpleegt u filteren op geavanceerde operators en velden.

In de volgende PowerShell-voorbeeld maakt u een gebeurtenisabonnement waarmee gefilterd door het begin van het onderwerp. U gebruikt de `-SubjectBeginsWith` parameter gebeurtenissen voor een specifieke resource te beperken. U kunt doorgeven de resource-ID van een netwerkbeveiligingsgroep.

```powershell
$resourceId = (Get-AzResource -ResourceName demoSecurityGroup -ResourceGroupName myResourceGroup).ResourceId

New-AzEventGridSubscription `
  -Endpoint <endpoint-URL> `
  -EventSubscriptionName demoSubscriptionToResourceGroup `
  -ResourceGroupName myResourceGroup `
  -SubjectBeginsWith $resourceId
```

Het volgende PowerShell-voorbeeld maakt u een abonnement voor een blob-opslag. Hiermee beperkt u gebeurtenissen naar resources met een onderwerp dat eindigt op `.jpg`.

```powershell
$storageId = (Get-AzStorageAccount -ResourceGroupName myResourceGroup -AccountName $storageName).Id

New-AzEventGridSubscription `
  -EventSubscriptionName demoSubToStorage `
  -Endpoint <endpoint-URL> `
  -ResourceId $storageId `
  -SubjectEndsWith ".jpg"
```

In de volgende Azure CLI-voorbeeld maakt u een gebeurtenisabonnement waarmee gefilterd door het begin van het onderwerp. U gebruikt de `--subject-begins-with` parameter gebeurtenissen voor een specifieke resource te beperken. U kunt doorgeven de resource-ID van een netwerkbeveiligingsgroep.

```azurecli
resourceId=$(az resource show --name demoSecurityGroup --resource-group myResourceGroup --resource-type Microsoft.Network/networkSecurityGroups --query id --output tsv)

az eventgrid event-subscription create \
  --name demoSubscriptionToResourceGroup \
  --resource-group myResourceGroup \
  --endpoint <endpoint-URL> \
  --subject-begins-with $resourceId
```

De volgende Azure CLI-voorbeeld maakt u een abonnement voor een blob-opslag. Hiermee beperkt u gebeurtenissen naar resources met een onderwerp dat eindigt op `.jpg`.

```azurecli
storageid=$(az storage account show --name $storageName --resource-group myResourceGroup --query id --output tsv)

az eventgrid event-subscription create \
  --resource-id $storageid \
  --name demoSubToStorage \
  --endpoint <endpoint-URL> \
  --subject-ends-with ".jpg"
```

In het volgende Sjabloonvoorbeeld Resource Manager maakt u een gebeurtenisabonnement waarmee gefilterd door het begin van het onderwerp. U gebruikt de `subjectBeginsWith` eigenschap gebeurtenissen voor een specifieke resource te beperken. U kunt doorgeven de resource-ID van een netwerkbeveiligingsgroep.

```json
"resources": [
  {
    "type": "Microsoft.EventGrid/eventSubscriptions",
    "name": "[parameters('eventSubName')]",
    "apiVersion": "2018-09-15-preview",
    "properties": {
      "destination": {
        "endpointType": "WebHook",
        "properties": {
          "endpointUrl": "[parameters('endpoint')]"
        }
      },
      "filter": {
        "subjectBeginsWith": "[resourceId('Microsoft.Network/networkSecurityGroups','demoSecurityGroup')]",
        "subjectEndsWith": "",
        "isSubjectCaseSensitive": false,
        "includedEventTypes": [ "All" ]
      }
    }
  }
]
```

Het volgende voorbeeld van de Resource Manager-sjabloon maakt u een abonnement voor een blob-opslag. Hiermee beperkt u gebeurtenissen naar resources met een onderwerp dat eindigt op `.jpg`.

```json
"resources": [
  {
    "type": "Microsoft.Storage/storageAccounts/providers/eventSubscriptions",
    "name": "[concat(parameters('storageName'), '/Microsoft.EventGrid/', parameters('eventSubName'))]",
    "apiVersion": "2018-09-15-preview",
    "properties": {
      "destination": {
        "endpointType": "WebHook",
        "properties": {
          "endpointUrl": "[parameters('endpoint')]"
        }
      },
      "filter": {
        "subjectEndsWith": ".jpg",
        "subjectBeginsWith": "",
        "isSubjectCaseSensitive": false,
        "includedEventTypes": [ "All" ]
      }
    }
  }
]
```

## <a name="filter-by-operators-and-data"></a>Operators en filteren

U kunt voor meer flexibiliteit bij het filteren van operators en eigenschappen van de gegevens te filteren van gebeurtenissen gebruiken.

[!INCLUDE [event-grid-preview-feature-note.md](../../includes/event-grid-preview-feature-note.md)]

### <a name="subscribe-with-advanced-filters"></a>Abonneren met geavanceerde filters

Zie voor meer informatie over de operators en de sleutels die u voor Geavanceerd filteren gebruiken kunt, [Geavanceerd filteren](event-filtering.md#advanced-filtering).

Deze voorbeelden wordt een aangepast onderwerp maken. Ze op het aangepaste onderwerp te abonneren en filteren op een waarde in het gegevensobject. Gebeurtenissen die door de eigenschap kleur is ingesteld op blauw, rood of groen worden verzonden naar het abonnement.

Gebruik voor Azure CLI:

```azurecli-interactive
topicName=<your-topic-name>
endpointURL=<endpoint-URL>

az group create -n gridResourceGroup -l eastus2
az eventgrid topic create --name $topicName -l eastus2 -g gridResourceGroup

topicid=$(az eventgrid topic show --name $topicName -g gridResourceGroup --query id --output tsv)

az eventgrid event-subscription create \
  --source-resource-id $topicid \
  -n demoAdvancedSub \
  --advanced-filter data.color stringin blue red green \
  --endpoint $endpointURL \
  --expiration-date "<yyyy-mm-dd>"
```

U ziet dat er een [vervaldatum](concepts.md#event-subscription-expiration) is ingesteld voor het abonnement.

Gebruik voor PowerShell:

```azurepowershell-interactive
$topicName = <your-topic-name>
$endpointURL = <endpoint-URL>

New-AzResourceGroup -Name gridResourceGroup -Location eastus2
New-AzEventGridTopic -ResourceGroupName gridResourceGroup -Location eastus2 -Name $topicName

$topicid = (Get-AzEventGridTopic -ResourceGroupName gridResourceGroup -Name $topicName).Id

$expDate = '<mm/dd/yyyy hh:mm:ss>' | Get-Date
$AdvFilter1=@{operator="StringIn"; key="Data.color"; Values=@('blue', 'red', 'green')}

New-AzEventGridSubscription `
  -ResourceId $topicid `
  -EventSubscriptionName <event_subscription_name> `
  -Endpoint $endpointURL `
  -ExpirationDate $expDate `
  -AdvancedFilter @($AdvFilter1)
```

### <a name="test-filter"></a>Test-filter

Als u wilt testen of het filter, een gebeurtenis te verzenden met het kleurveld is ingesteld op groen. Omdat groen is een van de waarden in het filter, de gebeurtenis naar het eindpunt wordt geleverd.

Gebruik voor Azure CLI:

```azurecli-interactive
topicEndpoint=$(az eventgrid topic show --name $topicName -g gridResourceGroup --query "endpoint" --output tsv)
key=$(az eventgrid topic key list --name $topicName -g gridResourceGroup --query "key1" --output tsv)

event='[ {"id": "'"$RANDOM"'", "eventType": "recordInserted", "subject": "myapp/vehicles/cars", "eventTime": "'`date +%Y-%m-%dT%H:%M:%S%z`'", "data":{ "model": "SUV", "color": "green"},"dataVersion": "1.0"} ]'

curl -X POST -H "aeg-sas-key: $key" -d "$event" $topicEndpoint
```

Gebruik voor PowerShell:

```azurepowershell-interactive
$endpoint = (Get-AzEventGridTopic -ResourceGroupName gridResourceGroup -Name $topicName).Endpoint
$keys = Get-AzEventGridTopicKey -ResourceGroupName gridResourceGroup -Name $topicName

$eventID = Get-Random 99999
$eventDate = Get-Date -Format s

$htbody = @{
    id= $eventID
    eventType="recordInserted"
    subject="myapp/vehicles/cars"
    eventTime= $eventDate
    data= @{
        model="SUV"
        color="green"
    }
    dataVersion="1.0"
}

$body = "["+(ConvertTo-Json $htbody)+"]"

Invoke-WebRequest -Uri $endpoint -Method POST -Body $body -Headers @{"aeg-sas-key" = $keys.Key1}
```

Als u wilt testen in een scenario waarin de gebeurtenis niet verzonden, een gebeurtenis te verzenden met het kleurveld is ingesteld op geel. Gele is niet een van de waarden die zijn opgegeven in het abonnement, zodat de gebeurtenis wordt niet geleverd aan uw abonnement.

Gebruik voor Azure CLI:

```azurecli-interactive
event='[ {"id": "'"$RANDOM"'", "eventType": "recordInserted", "subject": "myapp/vehicles/cars", "eventTime": "'`date +%Y-%m-%dT%H:%M:%S%z`'", "data":{ "model": "SUV", "color": "yellow"},"dataVersion": "1.0"} ]'

curl -X POST -H "aeg-sas-key: $key" -d "$event" $topicEndpoint
```
Gebruik voor PowerShell:

```azurepowershell-interactive
$htbody = @{
    id= $eventID
    eventType="recordInserted"
    subject="myapp/vehicles/cars"
    eventTime= $eventDate
    data= @{
        model="SUV"
        color="yellow"
    }
    dataVersion="1.0"
}

$body = "["+(ConvertTo-Json $htbody)+"]"

Invoke-WebRequest -Uri $endpoint -Method POST -Body $body -Headers @{"aeg-sas-key" = $keys.Key1}
```

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over het controleren van de gebeurtenis leveringen [Monitor Event Grid berichtbezorging](monitor-event-delivery.md).
* Zie voor meer informatie over de verificatiesleutel [Event Grid-beveiliging en verificatie](security-authentication.md).
* Zie voor meer informatie over het maken van een Azure Event Grid-abonnement [Event Grid-abonnementsschema](subscription-creation-schema.md).
