---
title: Een event hub maken met consumergroep - Azure Event Hubs | Microsoft Docs
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
ms.openlocfilehash: db5bb30c4049eca699f8adb45a923915033b4216
ms.sourcegitcommit: 78ec955e8cdbfa01b0fa9bdd99659b3f64932bba
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/10/2018
ms.locfileid: "53134451"
---
# <a name="quickstart-create-an-event-hub-using-azure-resource-manager-template"></a>Snelstartgids: Een event hub met behulp van Azure Resource Manager-sjabloon maken
Azure Event Hubs is een big data-platform voor het streamen van gegevens en een gebeurtenisopneemservice die miljoenen gebeurtenissen per seconde kan opnemen en verwerken. Event Hubs kan gebeurtenissen, gegevens of telemetrie die wordt geproduceerd door gedistribueerde software en apparaten verwerken en opslaan. Gegevens die naar een Event Hub worden verzonden, kunnen worden omgezet en opgeslagen via een provider voor realtime analytische gegevens of batchverwerking/opslagadapters. Zie [Overzicht van Event Hubs](event-hubs-about.md) en [Functies van Event Hubs](event-hubs-features.md) voor een gedetailleerd overzicht van Event Hubs.

In deze quickstart maakt u een event hub met behulp van een Azure Resource Manager-sjabloon. Een Azure Resource Manager-sjabloon maakt u een naamruimte van het type [Event Hubs](event-hubs-what-is-event-hubs.md), met één gebeurtenishub, en een consumentengroep beperkt. Het artikel wordt beschreven hoe om te definiëren welke resources worden geïmplementeerd en over het definiëren van parameters die zijn opgegeven wanneer de implementatie wordt uitgevoerd. U kunt deze sjabloon gebruiken voor uw eigen implementaties of de sjabloon aanpassen aan uw eisen. Zie voor meer informatie over het maken van sjablonen [Authoring Azure Resource Manager-sjablonen][Authoring Azure Resource Manager templates].


> [!NOTE]
> Zie voor de volledige sjabloon, de [Event hub- en klanttoepassingen groep sjabloon] [ Event Hub and consumer group template] op GitHub. Deze sjabloon gemaakt van een consumergroep naast een event hub-naamruimte en een event hub. Om te controleren op de meest recente sjablonen, gaat u naar de galerie [Azure-snelstartsjablonen][Azure Quickstart Templates] en zoekt u naar Event Hubs.

## <a name="prerequisites"></a>Vereisten
U hebt een Azure-abonnement nodig om deze snelstart te voltooien. Als u nog geen abonnement hebt, [maakt u een gratis account](https://azure.microsoft.com/free/) voordat u begint.

Als u wilt gebruiken **Azure PowerShell** om de Resource Manager-sjabloon te implementeren [Azure PowerShell installeren](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-5.7.0).

Als u wilt gebruiken **Azure CLI** om de Resource Manager-sjabloon te implementeren [Azure CLI installeren]( /cli/azure/install-azure-cli).

## <a name="create-the-resource-manager-template-json"></a>De JSON van de Resource Manager-sjabloon maken
Maak een JSON-bestand met de naam MyEventHub.json met de volgende inhoud en sla deze op een map (bijvoorbeeld: C:\EventHubsQuickstarts\ResourceManagerTemplate).

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "eventhub-namespace-name": {
            "type": "String"
        },
        "eventhub_name": {
            "type": "String"
        }
    },
    "resources": [
        {
            "type": "Microsoft.EventHub/namespaces",
            "sku": {
                "name": "Standard",
                "tier": "Standard",
                "capacity": 1
            },
            "name": "[parameters('eventhub-namespace-name')]",
            "apiVersion": "2017-04-01",
            "location": "East US",
            "tags": {},
            "scale": null,
            "properties": {
                "isAutoInflateEnabled": false,
                "maximumThroughputUnits": 0
            },
            "dependsOn": []
        },
        {
            "type": "Microsoft.EventHub/namespaces/eventhubs",
            "name": "[concat(parameters('eventhub-namespace-name'), '/', parameters('eventhub_name'))]",
            "apiVersion": "2017-04-01",
            "location": "East US",
            "scale": null,
            "properties": {
                "messageRetentionInDays": 7,
                "partitionCount": 1,
                "status": "Active"
            },
            "dependsOn": [
                "[resourceId('Microsoft.EventHub/namespaces', parameters('eventhub-namespace-name'))]"
            ]
        }
    ]
}
```

## <a name="create-the-parameters-json"></a>De parameters-JSON maken
Maak een JSON-bestand met de naam MyEventHub-Parameters.json met parameters voor de Azure Resource Manager-sjabloon. 

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
        "eventhub-namespace-name": {
            "value": "<specify a name for the event hub namespace>"
        },
        "eventhub_name": {
            "value": "<Specify a name for the event hub in the namespace>"
        }
  }
}
```



## <a name="use-azure-powershell-to-deploy-the-template"></a>Azure PowerShell gebruiken om de sjabloon te implementeren

### <a name="sign-in-to-azure"></a>Aanmelden bij Azure
1. Start Azure PowerShell

2. Voer de volgende opdracht uit om u aan te melden bij Azure:

   ```azurepowershell
   Login-AzureRmAccount
   ```
3. Als u hebt voert u de volgende opdrachten om in te stellen de huidige context van het abonnement:

   ```azurepowershell
   Select-AzureRmSubscription -SubscriptionName "<YourSubscriptionName>" 
   ```

### <a name="provision-resources"></a>Resources inrichten
Als u de resources met behulp van Azure PowerShell implementeren/inrichten, wilt overschakelen naar de map C:\EventHubsQuickStart\ARM\, voer de volgende opdrachten uit:

> [!IMPORTANT]
> Geef een naam voor de Azure-resourcegroep gemaakt als een waarde voor $resourceGroupName voordat u de opdrachten uitvoert. 

```azurepowershell
$resourceGroupName = "<Specify a name for the Azure resource group>"

# Create an Azure resource group
New-AzureRmResourceGroup $resourceGroupName -location 'East US'

# Deploy the Resource Manager template. Specify the names of deployment itself, resource group, JSON file for the template, JSON file for parameters
New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName $resourceGroupName -TemplateFile MyEventHub.json -TemplateParameterFile MyEventHub-Parameters.json
```

## <a name="use-azure-cli-to-deploy-the-template"></a>Azure CLI gebruiken om de sjabloon te implementeren

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

De volgende stappen zijn niet vereist als u opdrachten in Cloud Shell uitvoert. Als u de CLI lokaal uitvoert, moet u de volgende stappen uitvoeren om u aan te melden bij Azure en uw huidige abonnement in te stellen:

Voer de volgende opdracht uit om u aan te melden bij Azure:

```azurecli
az login
```

Stel de context van het huidige abonnement in. Vervang `MyAzureSub` door de naam van het Azure-abonnement dat u wilt gebruiken:

```azurecli
az account set --subscription <Name of your Azure subscription>
``` 

### <a name="provision-resources"></a>Resources inrichten
Voor het implementeren van de resources met behulp van Azure CLI, Ga naar de map C:\EventHubsQuickStart\ARM\ en voer de volgende opdrachten uit:

> [!IMPORTANT]
> Geef een naam voor de Azure-resourcegroep in de groep az-opdracht maken. .

```azurecli
# Create an Azure resource group
az group create --name <YourResourceGroupName> --location eastus

# # Deploy the Resource Manager template. Specify the names of resource group, deployment, JSON file for the template, JSON file for parameters
az group deployment create --name <Specify a name for the deployment> --resource-group <YourResourceGroupName> --template-file MyEventHub.json --parameters @MyEventHub-Parameters.json
```

Gefeliciteerd! U hebt de Azure Resource Manager-sjabloon gebruikt om een Event Hubs-naamruimte en een event hub binnen deze naamruimte te maken.

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u de Event Hubs-naamruimte gemaakt en voorbeeldtoepassingen gebruikt om gebeurtenissen te verzenden en ontvangen vanuit uw Event Hub. Zie de volgende zelfstudies voor stapsgewijze instructies voor het verzenden van gebeurtenissen naar of ontvangen van gebeurtenissen vanuit een Event Hub: 

- **Gebeurtenissen verzenden naar een event hub**: [.NET Core](event-hubs-dotnet-standard-getstarted-send.md), [.NET Framework](event-hubs-dotnet-framework-getstarted-send.md), [Java](event-hubs-java-get-started-send.md), [Python](event-hubs-python-get-started-send.md), [Node.js](event-hubs-node-get-started-send.md), [Gaat](event-hubs-go-get-started-send.md), [C](event-hubs-c-getstarted-send.md)
- **Gebeurtenissen ontvangen van een event hub**: [.NET Core](event-hubs-dotnet-standard-getstarted-receive-eph.md), [.NET Framework](event-hubs-dotnet-framework-getstarted-receive-eph.md), [Java](event-hubs-java-get-started-receive-eph.md), [Python](event-hubs-python-get-started-receive.md), [Node.js ](event-hubs-node-get-started-receive.md), [Gaat](event-hubs-go-get-started-receive-eph.md), [Apache Storm](event-hubs-storm-getstarted-receive.md)

[3]: ./media/event-hubs-quickstart-powershell/sender1.png
[4]: ./media/event-hubs-quickstart-powershell/receiver1.png
[5]: ./media/event-hubs-quickstart-powershell/metrics.png

[Authoring Azure Resource Manager templates]: ../azure-resource-manager/resource-group-authoring-templates.md
[Azure Quickstart Templates]:  https://azure.microsoft.com/documentation/templates/?term=event+hubs
[Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
[Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md
[Event hub and consumer group template]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-event-hubs-create-event-hub-and-consumer-group/
