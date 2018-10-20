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
ms.date: 10/16/2018
ms.author: shvija
ms.openlocfilehash: 48d41ef4df986f959dfabe04e07552e287fdfcd0
ms.sourcegitcommit: 668b486f3d07562b614de91451e50296be3c2e1f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/19/2018
ms.locfileid: "49457053"
---
# <a name="create-an-event-hubs-namespace-with-event-hub-and-consumer-group-using-an-azure-resource-manager-template"></a>Een Event Hubs-naamruimte maken met event hub en consumenten met een Azure Resource Manager-sjabloon
Azure Event Hubs is een big data-platform voor het streamen van gegevens en een gebeurtenisopneemservice die miljoenen gebeurtenissen per seconde kan opnemen en verwerken. Event Hubs kan gebeurtenissen, gegevens of telemetrie die wordt geproduceerd door gedistribueerde software en apparaten verwerken en opslaan. Gegevens die naar een Event Hub worden verzonden, kunnen worden omgezet en opgeslagen via een provider voor realtime analytische gegevens of batchverwerking/opslagadapters. Zie voor een gedetailleerd overzicht van Event Hubs [overzicht van Event Hubs](event-hubs-about.md) en [functies van Event Hubs](event-hubs-features.md).

In deze quickstart maakt u een event hub met behulp van een Azure Resource Manager-sjabloon. Een Azure Resource Manager-sjabloon maakt u een naamruimte van het type [Event Hubs](event-hubs-what-is-event-hubs.md), met één gebeurtenishub, en een consumentengroep beperkt. Het artikel wordt beschreven hoe om te definiëren welke resources worden geïmplementeerd en over het definiëren van parameters die zijn opgegeven wanneer de implementatie wordt uitgevoerd. U kunt deze sjabloon gebruiken voor uw eigen implementaties of de sjabloon aanpassen aan uw eisen. Zie voor meer informatie over het maken van sjablonen [Authoring Azure Resource Manager-sjablonen][Authoring Azure Resource Manager templates].

Zie voor de volledige sjabloon, de [Event hub- en klanttoepassingen groep sjabloon] [ Event Hub and consumer group template] op GitHub.

> [!NOTE]
> Om te controleren op de meest recente sjablonen, gaat u naar de galerie [Azure-snelstartsjablonen][Azure Quickstart Templates] en zoekt u naar Event Hubs.

## <a name="prerequisites"></a>Vereisten
U hebt een Azure-abonnement nodig om deze snelstart te voltooien. Als u nog geen hebt, [Maak een gratis account] [-] voordat u begint.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u **Azure PowerShell** om de Resource Manager-sjabloon te implementeren lokaal, moet u uitvoeren de nieuwste versie van PowerShell voor het voltooien van deze quickstart. Als u Azure PowerShell wilt installeren of upgraden, raadpleegt u [Azure PowerShell installeren en configureren](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-5.7.0).

Als u wilt installeren en gebruiken **Azure CLI** om de Resource Manager-sjabloon te implementeren lokaal, in deze zelfstudie moet u Azure CLI versie 2.0.4 of hoger. Voer `az --version` uit om uw versie te controleren. Als u uw CLI wilt installeren of upgraden, raadpleegt u [De Azure CLI installeren]( /cli/azure/install-azure-cli).

## <a name="what-will-you-deploy"></a>Wat wilt u implementeren?

Met deze sjabloon implementeert u een Event Hubs-naamruimte met een event hub en een consumergroep.

De implementatie automatisch wordt uitgevoerd, selecteert u de volgende knop:

[![Implementeren in Azure](./media/event-hubs-resource-manager-namespace-event-hub/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-event-hubs-create-event-hub-and-consumer-group%2Fazuredeploy.json)

## <a name="define-parameters"></a>De parameters definiëren
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

## <a name="define-resources-to-deploy"></a>Definieer resources om te implementeren

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

## <a name="azure-powershell"></a>Azure PowerShell
Voor het implementeren van de resources met behulp van Azure PowerShell, voer de volgende opdracht:

```azurepowershell
New-AzureRmResourceGroupDeployment -ResourceGroupName \<resource-group-name\> -TemplateFile https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-event-hubs-create-event-hub-and-consumer-group/azuredeploy.json
```

## <a name="azure-cli"></a>Azure-CLI
Voor het implementeren van de resources met behulp van Azure PowerShell, voer de volgende opdracht:

```azurecli
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri [https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-event-hubs-create-event-hub-and-consumer-group/azuredeploy.json][]
```

Gefeliciteerd! U hebt de Azure Resource Manager-sjabloon gebruikt om een Event Hubs-naamruimte en een event hub binnen deze naamruimte te maken.

## <a name="next-steps"></a>Volgende stappen

In dit artikel, kunt u de Event Hubs-naamruimte gemaakt en voorbeeldtoepassingen die wordt gebruikt voor het verzenden en ontvangen van gebeurtenissen van uw event hub. Zie de volgende zelfstudies voor stapsgewijze instructies voor het verzenden van gebeurtenissen naar (of) gebeurtenissen ontvangen van een event hub: 

- **Gebeurtenissen verzenden naar een event hub**: [.NET Standard](event-hubs-dotnet-standard-getstarted-send.md), [.NET Framework](event-hubs-dotnet-framework-getstarted-send.md), [Java](event-hubs-java-get-started-send.md), [Python](event-hubs-python-get-started-send.md), [Node.js ](event-hubs-node-get-started-send.md), [Gaat](event-hubs-go-get-started-send.md), [C](event-hubs-c-getstarted-send.md)
- **Gebeurtenissen ontvangen van een event hub**: [.NET Standard](event-hubs-dotnet-standard-getstarted-receive-eph.md), [.NET Framework](event-hubs-dotnet-framework-getstarted-receive-eph.md), [Java](event-hubs-java-get-started-receive-eph.md), [Python](event-hubs-python-get-started-receive.md), [ Node.js](event-hubs-node-get-started-receive.md), [gaat](event-hubs-go-get-started-receive-eph.md), [Apache Storm](event-hubs-storm-getstarted-receive.md)

[3]: ./media/event-hubs-quickstart-powershell/sender1.png
[4]: ./media/event-hubs-quickstart-powershell/receiver1.png
[5]: ./media/event-hubs-quickstart-powershell/metrics.png

[Authoring Azure Resource Manager templates]: ../azure-resource-manager/resource-group-authoring-templates.md
[Azure Quickstart Templates]:  https://azure.microsoft.com/documentation/templates/?term=event+hubs
[Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
[Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md
[Event hub and consumer group template]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-event-hubs-create-event-hub-and-consumer-group/
