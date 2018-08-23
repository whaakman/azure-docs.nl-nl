---
title: Maak een Azure Event Hubs-naamruimte en consumenten-groep met behulp van een sjabloon | Microsoft Docs
description: Een Event Hubs-naamruimte maken met een event hub en een consumentengroep met behulp van Azure Resource Manager-sjablonen
services: event-hubs
documentationcenter: .net
author: ShubhaVijayasarathy
manager: timlt
editor: ''
ms.assetid: 28bb4591-1fd7-444f-a327-4e67e8878798
ms.service: event-hubs
ms.devlang: tbd
ms.topic: article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 08/16/2018
ms.author: shvija
ms.openlocfilehash: 4feb75fd885efa86470f0e38e129eefe81c64271
ms.sourcegitcommit: f057c10ae4f26a768e97f2cb3f3faca9ed23ff1b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/17/2018
ms.locfileid: "42059441"
---
# <a name="create-an-event-hubs-namespace-with-event-hub-and-consumer-group-using-an-azure-resource-manager-template"></a>Een Event Hubs-naamruimte maken met event hub en consumenten met een Azure Resource Manager-sjabloon

Dit artikel wordt beschreven hoe u een Azure Resource Manager-sjabloon gebruiken om te maken van een naamruimte van het type [Event Hubs](event-hubs-what-is-event-hubs.md), met één gebeurtenishub, en een consumentengroep beperkt. Het artikel wordt beschreven hoe om te definiëren welke resources worden geïmplementeerd en over het definiëren van parameters die zijn opgegeven wanneer de implementatie wordt uitgevoerd. U kunt deze sjabloon gebruiken voor uw eigen implementaties of de sjabloon aanpassen aan uw eisen.

Zie voor meer informatie over het maken van sjablonen [Authoring Azure Resource Manager-sjablonen][Authoring Azure Resource Manager templates].

Zie voor de volledige sjabloon, de [Event hub- en klanttoepassingen groep sjabloon] [ Event Hub and consumer group template] op GitHub.

> [!NOTE]
> Om te controleren op de meest recente sjablonen, gaat u naar de galerie [Azure-snelstartsjablonen][Azure Quickstart Templates] en zoekt u naar Event Hubs.
> 
> 

## <a name="what-will-you-deploy"></a>Wat wilt u implementeren?

Met deze sjabloon implementeert u een Event Hubs-naamruimte met een event hub en een consumergroep.

Klik op de volgende knop om de implementatie automatisch uit te voeren:

[![Implementeren in Azure](./media/event-hubs-resource-manager-namespace-event-hub/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-event-hubs-create-event-hub-and-consumer-group%2Fazuredeploy.json)

## <a name="parameters"></a>Parameters

Met Azure Resource Manager kunt u parameters definiëren voor waarden die u wilt opgeven wanneer de sjabloon wordt geïmplementeerd. De sjabloon bevat een sectie met de naam `Parameters` die alle parameterwaarden bevat. U moet een parameter definiëren voor waarden die variëren, gebaseerd op het project dat u wilt implementeren of op basis van de omgeving waarnaar u implementeert. Definieer geen parameters voor waarden die altijd hetzelfde blijven. De waarde van elke parameter in de sjabloon definieert de resources die zijn geïmplementeerd.

De sjabloon definieert de volgende parameters:

### <a name="eventhubnamespacename"></a>eventHubNamespaceName

De naam van de Event Hubs-naamruimte die wordt gemaakt.

```json
"eventHubNamespaceName": {
"type": "string"
}
```

### <a name="eventhubname"></a>eventHubName

De naam van de gebeurtenishub die in de Event Hubs-naamruimte wordt gemaakt.

```json
"eventHubName": {
"type": "string"
}
```

### <a name="eventhubconsumergroupname"></a>eventHubConsumerGroupName

De naam van de consumergroep hebt gemaakt voor de event hub.

```json
"eventHubConsumerGroupName": {
"type": "string"
}
```

### <a name="apiversion"></a>apiVersion

De API-versie van de sjabloon.

```json
"apiVersion": {
"type": "string"
}
```

## <a name="resources-to-deploy"></a>Resources om te implementeren

Hiermee maakt u een naamruimte van het type **EventHubs**, met een event hub en een consumergroep:

```json
"resources":[  
      {  
         "apiVersion":"[variables('ehVersion')]",
         "name":"[parameters('namespaceName')]",
         "type":"Microsoft.EventHub/namespaces",
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
                  "[concat('Microsoft.EventHub/namespaces/', parameters('namespaceName'))]"
               ],
               "properties":{  
                  "path":"[parameters('eventHubName')]"
               },
               "resources":[  
                  {  
                     "apiVersion":"[variables('ehVersion')]",
                     "name":"[parameters('consumerGroupName')]",
                     "type":"ConsumerGroups",
                     "dependsOn":[  
                        "[parameters('eventHubName')]"
                     ],
                     "properties":{  

                     }
                  }
               ]
            }
         ]
      }
   ],
```

## <a name="commands-to-run-deployment"></a>Opdrachten om implementatie uit te voeren

[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a>PowerShell

```azurepowershell
New-AzureRmResourceGroupDeployment -ResourceGroupName \<resource-group-name\> -TemplateFile https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-event-hubs-create-event-hub-and-consumer-group/azuredeploy.json
```

## <a name="azure-cli"></a>Azure-CLI

```azurecli
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri [https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-event-hubs-create-event-hub-and-consumer-group/azuredeploy.json][]
```

## <a name="next-steps"></a>Volgende stappen

U kunt meer informatie over Event Hubs vinden via de volgende koppelingen:

* [Event Hubs-overzicht](event-hubs-what-is-event-hubs.md)
* [Een Event Hub maken](event-hubs-create.md)
* [Veelgestelde vragen over Event Hubs](event-hubs-faq.md)

[Authoring Azure Resource Manager templates]: ../azure-resource-manager/resource-group-authoring-templates.md
[Azure Quickstart Templates]:  https://azure.microsoft.com/documentation/templates/?term=event+hubs
[Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
[Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md
[Event hub and consumer group template]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-event-hubs-create-event-hub-and-consumer-group/
