---
title: Service Bus-berichtenservice naamruimte met behulp van Azure Resource Manager-sjabloon maken | Microsoft Docs
description: Gebruik Azure Resource Manager-sjabloon te maken van een Service Bus-berichtenservice-naamruimte
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: dc0d6482-6344-4cef-8644-d4573639f5e4
ms.service: service-bus-messaging
ms.devlang: tbd
ms.topic: article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 12/21/2017
ms.author: sethm;shvija
ms.openlocfilehash: f184cc9418e4af95423c0ede65bca312dfca7393
ms.sourcegitcommit: 6f33adc568931edf91bfa96abbccf3719aa32041
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/22/2017
---
# <a name="create-a-service-bus-namespace-using-an-azure-resource-manager-template"></a>Een Service Bus-naamruimte met een Azure Resource Manager-sjabloon maken

In dit artikel wordt beschreven hoe u een Azure Resource Manager-sjabloon die wordt gemaakt van een Service Bus-naamruimte van het type **Messaging** met een standaard SKU. Het artikel definieert ook de parameters die zijn opgegeven voor de uitvoering van de implementatie. U kunt deze sjabloon gebruiken voor uw eigen implementaties of de sjabloon aanpassen aan uw eisen.

Zie [Azure Resource Manager-sjablonen samenstellen][Authoring Azure Resource Manager templates] voor meer informatie over het maken van sjablonen.

Zie voor de volledige sjabloon, het [Service Bus-naamruimte sjabloon] [ Service Bus namespace template] op GitHub.

> [!NOTE]
> De volgende Azure Resource Manager-sjablonen zijn beschikbaar voor download- en implementatie. 
> 
> * [Een Service Bus-naamruimte maken met de wachtrij](service-bus-resource-manager-namespace-queue.md)
> * [Een Service Bus-naamruimte met onderwerp en een abonnement maken](service-bus-resource-manager-namespace-topic.md)
> * [Een Service Bus-naamruimte met een wachtrij en autorisatie regel maken](service-bus-resource-manager-namespace-auth-rule.md)
> * [Een Service Bus-naamruimte maken met onderwerp, abonnement en regel](service-bus-resource-manager-namespace-topic-with-rule.md)
> 
> Om te controleren of de meest recente sjablonen, gaat u naar de [Azure-Snelstartsjablonen] [ Azure Quickstart Templates] galerie en zoek naar Service Bus.
> 
> 

## <a name="what-will-you-deploy"></a>Wat wilt u implementeren?

Met deze sjabloon die u implementeert een Service Bus-naamruimte met een [Standard of Premium](https://azure.microsoft.com/pricing/details/service-bus/) SKU.

Klik op de volgende knop om de implementatie automatisch uit te voeren:

[![Implementeren in Azure](./media/service-bus-resource-manager-namespace/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-servicebus-create-namespace%2Fazuredeploy.json)

## <a name="parameters"></a>Parameters

Met Azure Resource Manager kunt u parameters definiëren voor waarden die u wilt opgeven wanneer de sjabloon wordt geïmplementeerd. De sjabloon bevat een sectie met de naam `Parameters` die de parameterwaarden bevat. U moet een parameter definiëren voor de waarden die variëren op basis van het project dat u wilt implementeren of op basis van de omgeving waarin u gaat implementeren. Definieer geen parameters voor waarden die altijd hetzelfde blijven. De waarde van elke parameter wordt gebruikt in de sjabloon voor het definiëren van de resources die worden geïmplementeerd.

Deze sjabloon definieert de volgende parameters:

### <a name="servicebusnamespacename"></a>serviceBusNamespaceName

De naam van de Service Bus-naamruimte maken.

```json
"serviceBusNamespaceName": {
"type": "string",
"metadata": { 
    "description": "Name of the Service Bus namespace" 
    }
}
```

### <a name="servicebussku"></a>serviceBusSKU

De naam van de Service Bus [SKU](https://azure.microsoft.com/pricing/details/service-bus/) maken.

```json
"serviceBusSku": { 
    "type": "string", 
    "allowedValues": [ 
        "Standard",
        "Premium" 
    ], 
    "defaultValue": "Standard", 
    "metadata": { 
        "description": "The messaging tier for service Bus namespace" 
    } 

```

De sjabloon definieert de waarden die zijn toegestaan voor deze parameter (Standard of Premium). Als geen waarde opgeeft, wijst de resourcemanager een standaardwaarde (standaard).

Zie voor meer informatie over prijzen voor Service Bus [Service Bus-prijzen en facturering][Service Bus pricing and billing].

### <a name="servicebusapiversion"></a>serviceBusApiVersion

De Service Bus-API-versie van de sjabloon.

```json
"serviceBusApiVersion": { 
       "type": "string", 
       "defaultValue": "2017-04-01", 
       "metadata": { 
           "description": "Service Bus ApiVersion used by the template" 
       } 
```

## <a name="resources-to-deploy"></a>Resources om te implementeren

### <a name="service-bus-namespace"></a>Service Bus-naamruimte

Maakt een standaard Service Bus-naamruimte van het type **Messaging**.

```json
"resources": [
    {
        "apiVersion": "[parameters('serviceBusApiVersion')]",
        "name": "[parameters('serviceBusNamespaceName')]",
        "type": "Microsoft.ServiceBus/Namespaces",
        "location": "[variables('location')]",
        "kind": "Messaging",
        "sku": {
            "name": "StandardSku",
            "tier": "Standard"
        },
        "properties": {
        }
    }
]
```

## <a name="commands-to-run-deployment"></a>Opdrachten om implementatie uit te voeren

[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a>PowerShell

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName <resource-group-name> -TemplateFile https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-servicebus-create-namespace/azuredeploy.json
```

### <a name="azure-cli"></a>Azure-CLI

```azurecli-interactive
azure config mode arm

azure group deployment create <my-resource-group> <my-deployment-name> --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-servicebus-create-namespace/azuredeploy.json
```

## <a name="next-steps"></a>Volgende stappen
Nu dat u hebt gemaakt en geïmplementeerd met Azure Resource Manager bronnen, meer informatie over deze resources beheren door te lezen van deze artikelen:

* [Servicebus met PowerShell beheren](service-bus-manage-with-ps.md)
* [Service Bus-resources beheren met de Service Bus-Explorer](https://github.com/paolosalvatori/ServiceBusExplorer/releases)

[Authoring Azure Resource Manager templates]: ../azure-resource-manager/resource-group-authoring-templates.md
[Service Bus namespace template]: https://github.com/Azure/azure-quickstart-templates/blob/master/101-servicebus-create-namespace/
[Azure Quickstart Templates]: https://azure.microsoft.com/documentation/templates/?term=service+bus
[Service Bus pricing and billing]: service-bus-pricing-billing.md
[Using Azure PowerShell with Azure Resource Manager]: ../azure-resource-manager/powershell-azure-resource-manager.md
[Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../azure-resource-manager/xplat-cli-azure-resource-manager.md
