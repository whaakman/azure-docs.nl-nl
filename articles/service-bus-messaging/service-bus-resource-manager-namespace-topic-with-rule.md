---
title: Maken van Azure Service Bus-onderwerp-abonnement en regel met behulp van Azure Resource Manager-sjabloon | Microsoft Docs
description: Een Service Bus-naamruimte maken met een onderwerp, abonnement en regel met behulp van Azure Resource Manager-sjabloon
services: service-bus-messaging
documentationcenter: .net
author: spelluru
manager: timlt
editor: ''
ms.assetid: 9e0aaf58-0214-4bca-bd00-d29c08f9b1bc
ms.service: service-bus-messaging
ms.devlang: tbd
ms.topic: article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 04/11/2018
ms.author: spelluru
ms.openlocfilehash: dd0ef94c7efb27641d5f0bf50d87bf852bcd1e9a
ms.sourcegitcommit: cb61439cf0ae2a3f4b07a98da4df258bfb479845
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/05/2018
ms.locfileid: "43696333"
---
# <a name="create-a-service-bus-namespace-with-topic-subscription-and-rule-using-an-azure-resource-manager-template"></a>Een Service Bus-naamruimte maken met een onderwerp, abonnement en regel met een Azure Resource Manager-sjabloon

In dit artikel laat zien hoe u een Azure Resource Manager-sjabloon die wordt gemaakt van een Service Bus-naamruimte met een onderwerp, abonnement en regel (filter). Het artikel wordt uitgelegd hoe om op te geven welke resources worden geïmplementeerd en over het definiëren van parameters die zijn opgegeven wanneer de implementatie wordt uitgevoerd. U kunt deze sjabloon gebruiken voor uw eigen implementaties of de sjabloon aanpassen aan uw eisen

Zie [Azure Resource Manager-sjablonen samenstellen][Authoring Azure Resource Manager templates] voor meer informatie over het maken van sjablonen.

Zie voor meer informatie over de praktijken en patronen op Azure-resources naamconventies [aanbevolen naamgevingsregels voor Azure-resources][Recommended naming conventions for Azure resources].

Zie voor de volledige sjabloon, de [Service Bus-naamruimte met een onderwerp, abonnement en regel] [ Service Bus namespace with topic, subscription, and rule] sjabloon.

> [!NOTE]
> De volgende Azure Resource Manager-sjablonen zijn beschikbaar voor downloaden en implementeren.
> 
> * [Een Service Bus-naamruimte maken met de wachtrij en de autorisatie-regel](service-bus-resource-manager-namespace-auth-rule.md)
> * [Een Service Bus-naamruimte met een wachtrij maken](service-bus-resource-manager-namespace-queue.md)
> * [Een Service Bus-naamruimte maken](service-bus-resource-manager-namespace.md)
> * [Een Service Bus-naamruimte maken met een onderwerp en abonnement](service-bus-resource-manager-namespace-topic.md)
> 
> Om te controleren of de meest recente sjablonen, gaat u naar de [Azure-Snelstartsjablonen] [ Azure Quickstart Templates] galerie en zoek naar Service Bus.
> 
> 

## <a name="what-will-you-deploy"></a>Wat wilt u implementeren?

Met deze sjabloon kunt implementeren u een Service Bus-naamruimte met een onderwerp, abonnement en regel (filter).

[Service Bus-onderwerpen en abonnementen](service-bus-queues-topics-subscriptions.md#topics-and-subscriptions) voorzien in de vorm van een een-op-veel communicatie, in een *publiceren/abonneren* patroon. Wanneer u onderwerpen en abonnementen, communiceren onderdelen van een gedistribueerde toepassing niet rechtstreeks met elkaar worden verbonden, in plaats daarvan wisselen ze berichten uit via een onderwerp dat als een intermediair fungeert. Een abonnement op een onderwerp lijkt op een virtuele wachtrij die kopieën van berichten die zijn verzonden naar het onderwerp ontvangt. Een filter op abonnement kunt u opgeven welke berichten die worden verzonden naar een onderwerp moet worden weergegeven binnen een bepaald onderwerpabonnement.

## <a name="what-are-rules-filters"></a>Wat zijn regels (filters)?

In veel scenario's, moeten berichten met specifieke kenmerken op verschillende manieren worden verwerkt. U kunt abonnementen om te zoeken naar berichten die specifieke eigenschappen en voer de wijzigingen in deze eigenschappen configureren zodat deze aangepaste verwerking. Hoewel Service Bus-abonnementen alle berichten die naar het onderwerp weergeven, kunt u alleen een subset van deze berichten kopiëren naar de abonnementenwachtrij voor virtuele. Dit wordt bereikt met behulp van abonnementfilters. Zie voor meer informatie over regels (filters), [regels en acties](service-bus-queues-topics-subscriptions.md#rules-and-actions).

Klik op de volgende knop om de implementatie automatisch uit te voeren:

[![Implementeren in Azure](./media/service-bus-resource-manager-namespace-topic/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-servicebus-create-topic-subscription-rule%2Fazuredeploy.json)

## <a name="parameters"></a>Parameters

Met Azure Resource Manager, moet u parameters voor waarden die u opgeven wilt wanneer de sjabloon wordt geïmplementeerd definiëren. De sjabloon bevat een sectie met de naam `Parameters` die alle parameterwaarden bevat. U moet een parameter definiëren voor de waarden die variëren op basis van het project dat u wilt implementeren of op basis van de omgeving waarin u gaat implementeren. Definieer geen parameters voor waarden die altijd hetzelfde blijven. De waarde van elke parameter wordt gebruikt in de sjabloon voor het definiëren van de resources die worden geïmplementeerd.

De sjabloon definieert de volgende parameters:

### <a name="servicebusnamespacename"></a>serviceBusNamespaceName
De naam van de Service Bus-naamruimte te maken.

```json
"serviceBusNamespaceName": {
"type": "string"
}
```

### <a name="servicebustopicname"></a>serviceBusTopicName
De naam van het onderwerp in de Service Bus-naamruimte gemaakt.

```json
"serviceBusTopicName": {
"type": "string"
}
```

### <a name="servicebussubscriptionname"></a>serviceBusSubscriptionName
De naam van het abonnement in de Service Bus-naamruimte hebt gemaakt.

```json
"serviceBusSubscriptionName": {
"type": "string"
}
```
### <a name="servicebusrulename"></a>serviceBusRuleName
De naam van de rule(filter) in de Service Bus-naamruimte hebt gemaakt.

```json
   "serviceBusRuleName": {
   "type": "string",
  }
```
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
Hiermee maakt u een standard Service Bus-naamruimte van het type **Messaging**, met een onderwerp en abonnement en regels.

```json
 "resources": [{
        "apiVersion": "[variables('sbVersion')]",
        "name": "[parameters('serviceBusNamespaceName')]",
        "type": "Microsoft.ServiceBus/Namespaces",
        "location": "[variables('location')]",
        "sku": {
            "name": "Standard",
        },
        "resources": [{
            "apiVersion": "[variables('sbVersion')]",
            "name": "[parameters('serviceBusTopicName')]",
            "type": "Topics",
            "dependsOn": [
                "[concat('Microsoft.ServiceBus/namespaces/', parameters('serviceBusNamespaceName'))]"
            ],
            "properties": {
                "path": "[parameters('serviceBusTopicName')]"
            },
            "resources": [{
                "apiVersion": "[variables('sbVersion')]",
                "name": "[parameters('serviceBusSubscriptionName')]",
                "type": "Subscriptions",
                "dependsOn": [
                    "[parameters('serviceBusTopicName')]"
                ],
                "properties": {},
                "resources": [{
                    "apiVersion": "[variables('sbVersion')]",
                    "name": "[parameters('serviceBusRuleName')]",
                    "type": "Rules",
                    "dependsOn": [
                        "[parameters('serviceBusSubscriptionName')]"
                    ],
                    "properties": {
                        "filterType": "SqlFilter",
                        "sqlFilter": {
                            "sqlExpression": "StoreName = 'Store1'",
                            "requiresPreprocessing": "false"
                        },
                        "action": {
                            "sqlExpression": "set FilterTag = 'true'"
                        }
                    }
                }]
            }]
        }]
    }]
```

## <a name="commands-to-run-deployment"></a>Opdrachten om implementatie uit te voeren
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a>PowerShell
```powershell
New-AzureResourceGroupDeployment -Name \<deployment-name\> -ResourceGroupName \<resource-group-name\> -TemplateUri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-topic-subscription-rule/azuredeploy.json>
```

## <a name="azure-cli"></a>Azure-CLI
```azurecli
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-topic-subscription-rule/azuredeploy.json>
```

## <a name="next-steps"></a>Volgende stappen
Nu dat u hebt gemaakt en geïmplementeerd resources met behulp van Azure Resource Manager, meer informatie over het beheren van deze bronnen hiervoor deze artikelen:

* [Azure Servicebus beheren](service-bus-management-libraries.md)
* [Servicebus met PowerShell beheren](service-bus-manage-with-ps.md)
* [Service Bus-resources beheren met de Service Bus Explorer](https://github.com/paolosalvatori/ServiceBusExplorer/releases)

[Authoring Azure Resource Manager templates]: ../azure-resource-manager/resource-group-authoring-templates.md
[Azure Quickstart Templates]: https://azure.microsoft.com/documentation/templates/?term=service+bus
[Learn more about Service Bus topics and subscriptions]: service-bus-queues-topics-subscriptions.md
[Using Azure PowerShell with Azure Resource Manager]: ../azure-resource-manager/powershell-azure-resource-manager.md
[Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../azure-resource-manager/xplat-cli-azure-resource-manager.md
[Recommended naming conventions for Azure resources]: ../guidance/guidance-naming-conventions.md
[Service Bus namespace with topic, subscription, and rule]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-topic-subscription-rule/
[Service Bus queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md

