---
title: Service Bus Messaging-naamruimte maken met Azure Resource Manager-sjabloon | Microsoft Docs
description: Gebruik Azure Resource Manager-sjabloon voor het maken van een Service Bus Messaging-naamruimte
services: service-bus-messaging
documentationcenter: .net
author: spelluru
manager: timlt
editor: ''
ms.assetid: dc0d6482-6344-4cef-8644-d4573639f5e4
ms.service: service-bus-messaging
ms.devlang: tbd
ms.topic: article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 01/23/2019
ms.author: spelluru
ms.openlocfilehash: a79565661ae11e70364d64503b3b11bdeabeabdb
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57899754"
---
# <a name="create-a-service-bus-namespace-using-an-azure-resource-manager-template"></a>Een Service Bus-naamruimte met een Azure Resource Manager-sjabloon maken
In deze snelstartgids maakt u een Azure Resource Manager-sjabloon die wordt gemaakt van een Service Bus-naamruimte van het type **Messaging** met een **Standard** SKU. Het artikel definieert ook de parameters die zijn opgegeven voor het uitvoeren van de implementatie. U kunt deze sjabloon gebruiken voor uw eigen implementaties of de sjabloon aanpassen aan uw eisen. Zie [Azure Resource Manager-sjablonen samenstellen][Authoring Azure Resource Manager templates] voor meer informatie over het maken van sjablonen. Zie voor de volledige sjabloon, de [Service Bus-naamruimte sjabloon] [ Service Bus namespace template] op GitHub.

> [!NOTE]
> De volgende Azure Resource Manager-sjablonen zijn beschikbaar voor downloaden en implementeren. 
> 
> * [Een Service Bus-naamruimte met een wachtrij maken](service-bus-resource-manager-namespace-queue.md)
> * [Een Service Bus-naamruimte maken met een onderwerp en abonnement](service-bus-resource-manager-namespace-topic.md)
> * [Een Service Bus-naamruimte maken met de wachtrij en de autorisatie-regel](service-bus-resource-manager-namespace-auth-rule.md)
> * [Een Service Bus-naamruimte maken met een onderwerp, abonnement en regel](service-bus-resource-manager-namespace-topic-with-rule.md)
> 
> Om te controleren of de meest recente sjablonen, gaat u naar de [Azure-Snelstartsjablonen] [ Azure Quickstart Templates] galerie en zoek naar Service Bus.

## <a name="quick-deployment"></a>Snelle implementatie
De als voorbeeld wilt uitvoeren zonder dat elk JSON schrijven en uitvoeren van PowerShell/CLI-opdracht, selecteert u de volgende knop:

[![Implementeren in Azure](./media/service-bus-resource-manager-namespace/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-servicebus-create-namespace%2Fazuredeploy.json)

Voor het maken en de sjabloon handmatig implementeren, gaat u de volgende secties in dit artikel.

## <a name="prerequisites"></a>Vereisten
U hebt een Azure-abonnement nodig om deze snelstart te voltooien. Als u nog geen abonnement hebt, [maakt u een gratis account](https://azure.microsoft.com/free/) voordat u begint.

Als u wilt gebruiken **Azure PowerShell** om de Resource Manager-sjabloon te implementeren [Azure PowerShell installeren](https://docs.microsoft.com/powershell/azure/azurerm/install-azurerm-ps?view=azurermps-5.7.0).

Als u wilt gebruiken **Azure CLI** om de Resource Manager-sjabloon te implementeren [Azure CLI installeren]( /cli/azure/install-azure-cli).

## <a name="create-the-resource-manager-template-json"></a>De JSON van de Resource Manager-sjabloon maken 
Maak een JSON-bestand met de naam **MyServiceBusNamespace.json** met de volgende inhoud: 

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "serviceBusNamespaceName": {
            "type": "string",
            "metadata": {
                "description": "Name of the Service Bus namespace"
            }
        },
        "serviceBusSku": {
            "type": "string",
            "allowedValues": [
                "Basic",
                "Standard",
                "Premium"
            ],
            "defaultValue": "Standard",
            "metadata": {
                "description": "The messaging tier for service Bus namespace"
            }
        },
        "location": {
            "type": "string",
            "defaultValue": "[resourceGroup().location]",
            "metadata": {
                "description": "Location for all resources."
            }
        }
    },
    "resources": [
        {
            "apiVersion": "2017-04-01",
            "name": "[parameters('serviceBusNamespaceName')]",
            "type": "Microsoft.ServiceBus/namespaces",
            "location": "[parameters('location')]",
            "sku": {
                "name": "[parameters('serviceBusSku')]"
            }
        }
    ]
}
```

Deze sjabloon maakt u een standard Service Bus-naamruimte. Zie voor JSON-syntaxis en eigenschappen [naamruimten](/azure/templates/microsoft.servicebus/namespaces) verwijzing naar de sjabloon.

## <a name="create-the-parameters-json"></a>De parameters-JSON maken
De sjabloon die u in de vorige stap hebt gemaakt, heeft een sectie met de naam `Parameters`. U definiëren parameters voor de waarden die variëren op basis van het project dat u wilt implementeren of op basis van de doelomgeving. Deze sjabloon definieert de volgende parameters: **serviceBusNamespaceName**, **serviceBusSku**, en **locatie**. Zie voor meer informatie over SKU's van Service Bus, [Service Bus-SKU's](https://azure.microsoft.com/pricing/details/service-bus/) te maken.

Maak een JSON-bestand met de naam **MyServiceBusNamespace-Parameters.json** met de volgende inhoud: 

> [!NOTE] 
> Geef een naam voor uw Service Bus-naamruimte. 


```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "serviceBusNamespaceName": {
      "value": "<Specify a name for the Service Bus namespace>"
    },
    "serviceBusSku": {
      "value": "Standard"
    },
    "location": {
        "value": "East US"
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

### <a name="deploy-resources"></a>Resources implementeren
Voor het implementeren van de resources met behulp van Azure PowerShell, Ga naar de map waar u de JSON-bestanden hebt opgeslagen en voer de volgende opdrachten uit:

> [!IMPORTANT]
> Geef een naam voor de Azure-resourcegroep gemaakt als een waarde voor $resourceGroupName voordat u de opdrachten uitvoert. 

1. Declareer een variabele voor de naam van de resourcegroep en een waarde opgeven voor deze. 

    ```azurepowershell
    $resourceGroupName = "<Specify a name for the Azure resource group>"
    ```
2. Maak een Azure-resourcegroep.

    ```azurepowershell
    New-AzureRmResourceGroup $resourceGroupName -location 'East US'
    ```
3. De Resource Manager-sjabloon implementeren. Geef de namen van de implementatie zelf, de resourcegroep, JSON-bestand voor de sjabloon, JSON-bestand voor de parameters

    ```azurepowershell
    New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName $resourceGroupName -TemplateFile MyServiceBusNamespace.json -TemplateParameterFile MyServiceBusNamespace-Parameters.json
    ```

## <a name="use-azure-cli-to-deploy-the-template"></a>Azure CLI gebruiken om de sjabloon te implementeren

### <a name="sign-in-to-azure"></a>Aanmelden bij Azure

1. Voer de volgende opdracht uit om u aan te melden bij Azure:

    ```azurecli
    az login
    ```
2. Stel de context van het huidige abonnement in. Vervang `MyAzureSub` door de naam van het Azure-abonnement dat u wilt gebruiken:

    ```azurecli
    az account set --subscription <Name of your Azure subscription>
    ``` 

### <a name="deploy-resources"></a>Resources implementeren
Ga naar de map met JSON-bestanden voor het implementeren van de resources met behulp van Azure CLI, en voer de volgende opdrachten uit:

> [!IMPORTANT]
> Geef een naam voor de Azure-resourcegroep in de groep az-opdracht maken. .

1. Maak een Azure-resourcegroep. 
    ```azurecli
    az group create --name <YourResourceGroupName> --location eastus
    ```

2. De Resource Manager-sjabloon implementeren. Geef de namen van de resourcegroep, implementatie, JSON-bestand voor de sjabloon, JSON-bestand voor parameters.

    ```azurecli
    az group deployment create --name <Specify a name for the deployment> --resource-group <YourResourceGroupName> --template-file MyServiceBusNamespace.json --parameters @MyServiceBusNamespace-Parameters.json
    ```

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u een Service Bus-naamruimte gemaakt. Zie de andere Quick starts om informatie over het maken van wachtrijen, onderwerpen/abonnementen, en ze gebruiken: 

- [Aan de slag met Service Bus-wachtrijen](service-bus-dotnet-get-started-with-queues.md)
- [Aan de slag met Service Bus-onderwerpen](service-bus-dotnet-how-to-use-topics-subscriptions.md)

[Authoring Azure Resource Manager templates]: ../azure-resource-manager/resource-group-authoring-templates.md
[Service Bus namespace template]: https://github.com/Azure/azure-quickstart-templates/blob/master/101-servicebus-create-namespace/
[Azure Quickstart Templates]: https://azure.microsoft.com/documentation/templates/?term=service+bus
[Service Bus pricing and billing]: service-bus-pricing-billing.md
[Using Azure PowerShell with Azure Resource Manager]: ../azure-resource-manager/powershell-azure-resource-manager.md
[Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../azure-resource-manager/xplat-cli-azure-resource-manager.md
