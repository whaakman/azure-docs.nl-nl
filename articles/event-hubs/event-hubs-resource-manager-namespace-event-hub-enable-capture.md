---
title: Een Azure Event Hubs-naamruimte maken en Capture met behulp van een sjabloon inschakelen | Microsoft Docs
description: Een Azure Event Hubs-naamruimte met een gebeurtenishub maken en Capture inschakelen met behulp van een Azure Resource Manager-sjabloon
services: event-hubs
documentationcenter: .net
author: ShubhaVijayasarathy
manager: timlt
editor: 
ms.assetid: 8bdda6a2-5ff1-45e3-b696-c553768f1090
ms.service: event-hubs
ms.devlang: tbd
ms.topic: get-started-article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 06/28/2017
ms.author: shvija;sethm
ms.translationtype: Human Translation
ms.sourcegitcommit: 857267f46f6a2d545fc402ebf3a12f21c62ecd21
ms.openlocfilehash: f19a3d9b323d75ae23480d0699d55b79bb7d2e84
ms.contentlocale: nl-nl
ms.lasthandoff: 06/28/2017


---
# <a name="create-an-event-hubs-namespace-with-an-event-hub-and-enable-capture-using-an-azure-resource-manager-template"></a>Een Event Hubs-naamruimte met een gebeurtenishub maken en Capture inschakelen met behulp van een Azure Resource Manager-sjabloon
In dit artikel wordt beschreven u hoe u een Azure Resource Manager-sjabloon gebruikt om een Event Hubs-naamruimte te maken met één exemplaar van de gebeurtenishub en de functie Capture op de gebeurtenishub in te schakelen. In het artikel wordt beschreven hoe u kunt aangeven welke resources worden geïmplementeerd en hoe u de parameters definieert die bij de uitvoering van de implementatie zijn opgegeven. U kunt deze sjabloon gebruiken voor uw eigen implementaties of de sjabloon aanpassen aan uw eisen

Zie [Azure Resource Manager-sjablonen samenstellen][Authoring Azure Resource Manager templates] voor meer informatie over het maken van sjablonen.

Zie [Azure Resources Naming Conventions][Azure Resources Naming Conventions] voor meer informatie over de praktijken en patronen op Azure-Resources-naamconventies.

Zie voor de volledige sjabloon de [sjabloon Event hub and enable Capture][Event Hub and enable Capture template] op GitHub.

> [!NOTE]
> Om te controleren op de meest recente sjablonen, gaat u naar de galerie [Azure-snelstartsjablonen][Azure Quickstart Templates] en zoekt u naar Event Hubs.
> 
> 

## <a name="what-will-you-deploy"></a>Wat wilt u implementeren?
Met deze sjabloon implementeert u een Event Hubs-naamruimte met een gebeurtenishub en schakelt u tevens [Event Hubs Cinapture](event-hubs-capture-overview.md) in.

[Event Hubs](event-hubs-what-is-event-hubs.md) is een verwerkingsservice van gebeurtenissen, die wordt gebruikt om zeer grote hoeveelheden gebeurtenissen en telemetriegegevens verzamelt in Azure met een lage latentie en hoge betrouwbaarheid. Met Event Hubs Capture kunt u automatisch de gegevensstroom in uw gebeurtenishubs bezorgen in een Azure Blob-opslag van uw keuze binnen een opgegeven periode of grootte van uw keuze.

Klik op de volgende knop om de implementatie automatisch uit te voeren:

[![Implementeren in Azure](./media/event-hubs-resource-manager-namespace-event-hub/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-eventhubs-create-namespace-and-enable-capture%2Fazuredeploy.json)

## <a name="parameters"></a>Parameters
Met Azure Resource Manager kunt u parameters definiëren voor waarden die u wilt opgeven wanneer de sjabloon wordt geïmplementeerd. De sjabloon bevat een sectie met de naam `Parameters` die alle parameterwaarden bevat. U moet een parameter definiëren voor de waarden die variëren op basis van het project dat u wilt implementeren of op basis van de omgeving waarin u gaat implementeren. Definieer geen parameters voor waarden die altijd hetzelfde blijven. De waarde van elke parameter wordt gebruikt in de sjabloon voor het definiëren van de resources die worden geïmplementeerd.

De sjabloon definieert de volgende parameters.

### <a name="eventhubnamespacename"></a>eventHubNamespaceName
De naam van de Event Hubs-naamruimte die wordt gemaakt.

```json
"eventHubNamespaceName":{  
     "type":"string",
     "metadata":{  
         "description":"Name of the EventHub namespace"
      }
}
```

### <a name="eventhubname"></a>eventHubName
De naam van de gebeurtenishub die in de Event Hubs-naamruimte wordt gemaakt.

```json
"eventHubName":{  
    "type":"string",
    "metadata":{  
        "description":"Name of the event hub"
    }
}
```

### <a name="messageretentionindays"></a>messageRetentionInDays
Het aantal dagen dat u de berichten in de gebeurtenishub wilt bewaren. 

```json
"messageRetentionInDays":{
    "type":"int",
    "defaultValue": 1,
    "minValue":"1",
    "maxValue":"7",
    "metadata":{
       "description":"How long to retain the data in event hub"
     }
 }
```

### <a name="partitioncount"></a>partitionCount
Het aantal partities dat in de gebeurtenishub wordt gemaakt.

```json
"partitionCount":{
    "type":"int",
    "defaultValue":2,
    "minValue":2,
    "maxValue":32,
    "metadata":{
        "description":"Number of partitions chosen"
    }
 }
```

### <a name="captureenabled"></a>captureEnabled
Capture inschakelen op de gebeurtenishub.

```json
"captureEnabled":{
    "type":"string",
    "defaultValue":"true",
    "allowedValues": [
    "false",
    "true"],
    "metadata":{
        "description":"Enable or disable the Capture for your event hub"
    }
 }
```
### <a name="captureencodingformat"></a>captureEncodingFormat
De coderingsindeling die u opgeeft voor het serialiseren van de gebeurtenisgegevens.

```json
"captureEncodingFormat":{
    "type":"string",
    "defaultValue":"Avro",
    "allowedValues":[
    "Avro"],
    "metadata":{
        "description":"The encoding format in which Capture serializes the EventData"
    }
}
```

### <a name="capturetime"></a>captureTime
Het tijdsinterval waarin Event Hubs Capture begint met het vastleggen van de gegevens naar Azure Blob-opslag.

```json
"captureTime":{
    "type":"int",
    "defaultValue":300,
    "minValue":60,
    "maxValue":900,
    "metadata":{
         "description":"the time window in seconds for the capture"
    }
}
```

### <a name="capturesize"></a>captureSize
De grootte waarbij Event Hubs Capture begint met het vastleggen van de gegevens naar Azure Blob-opslag.

```json
"captureSize":{
    "type":"int",
    "defaultValue":314572800,
    "minValue":10485760,
    "maxValue":524288000,
    "metadata":{
        "description":"The size window in bytes for capture"
    }
}
```

### <a name="destinationstorageaccountresourceid"></a>destinationStorageAccountResourceId
Capture vereist een resource-id van het Azure Storage-account om te kunnen vastleggen naar uw gewenste opslagaccount.

```json
 "destinationStorageAccountResourceId":{
    "type":"string",
    "metadata":{
        "description":"Your existing Storage account resource id where you want the blobs be captured"
    }
 }
```

### <a name="blobcontainername"></a>blobContainerName
De blob-container waarin uw gebeurtenisgegevens worden vastgelegd.

```json
 "blobContainerName":{
    "type":"string",
    "metadata":{
        "description":"Your existing storage container in which you want the blobs captured"
    }
}
```


### <a name="apiversion"></a>apiVersion
De API-versie van de sjabloon.

```json
 "apiVersion":{  
    "type":"string",
    "defaultValue":"2015-08-01",
    "metadata":{  
        "description":"ApiVersion used by the template"
    }
 }
```

## <a name="resources-to-deploy"></a>Resources om te implementeren
Hiermee maakt u een naamruimte van het type **EventHubs** met een gebeurtenishub en wordt Capture ingeschakeld.

```json
"resources":[  
      {  
         "apiVersion":"[variables('ehVersion')]",
         "name":"[parameters('eventHubNamespaceName')]",
         "type":"Microsoft.EventHub/Namespaces",
         "location":"[variables('location')]",
         "sku":{  
            "name":"Standard",
            "tier":"Standard"
         },
         "resources":[  
            {  
               "apiVersion":"[variables('ehVersion')]",
               "name":"[parameters('eventHubName')]",
               "type":"EventHubs",
               "dependsOn":[  
                  "[concat('Microsoft.EventHub/namespaces/', parameters('eventHubNamespaceName'))]"
               ],
               "properties":{  
                  "path":"[parameters('eventHubName')]",
                  "MessageRetentionInDays":"[parameters('messageRetentionInDays')]",
                  "PartitionCount":"[parameters('partitionCount')]",
                  "CaptureDescription":{
                        "enabled":"[parameters('captureEnabled')]",
                        "encoding":"[parameters('captureEncodingFormat')]",
                        "intervalInSeconds":"[parameters('captureTime')]",
                        "sizeLimitInBytes":"[parameters('captureSize')]",
                        "destination":{
                            "name":"EventHubCapture.AzureBlockBlob",
                            "properties":{
                                "StorageAccountResourceId":"[parameters('destinationStorageAccountResourceId')]",
                                "BlobContainer":"[parameters('blobContainerName')]"
                            }
                        } 
                  }

               }

            }
         ]
      }
   ]
```

## <a name="commands-to-run-deployment"></a>Opdrachten om implementatie uit te voeren
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a>PowerShell
```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName \<resource-group-name\> -TemplateFile https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-eventhubs-create-namespace-and-enable-capture/azuredeploy.json
```

## <a name="azure-cli"></a>Azure CLI
```cli
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri [https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-eventhubs-create-namespace-and-enable-capture/azuredeploy.json][]
```
## <a name="next-steps"></a>Volgende stappen

U kunt Event Hubs Capture ook configureren via de [Azure-portal](https://portal.azure.com). Zie voor meer informatie [Event Hubs Capture inschakelen met behulp van de Azure-portal](event-hubs-capture-enable-through-portal.md).

U kunt meer informatie over Event Hubs vinden via de volgende koppelingen:

* [Event Hubs-overzicht](event-hubs-what-is-event-hubs.md)
* [Een Event Hub maken](event-hubs-create.md)
* [Veelgestelde vragen over Event Hubs](event-hubs-faq.md)

[Authoring Azure Resource Manager templates]: ../azure-resource-manager/resource-group-authoring-templates.md
[Azure Quickstart Templates]:  https://azure.microsoft.com/documentation/templates/?term=event+hubs
[Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
[Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md
[Azure Resources Naming Conventions]: https://azure.microsoft.com/documentation/articles/guidance-naming-conventions/
[Event hub and enable Capture template]: https://github.com/Azure/azure-quickstart-templates/tree/master/201-eventhubs-create-namespace-and-enable-capture

