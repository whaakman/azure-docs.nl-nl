---
title: Maken van een Service Bus-autorisatieregel met Azure Resource Manager-sjabloon | Microsoft Docs
description: Een Service Bus-autorisatieregel voor de naamruimte en de wachtrij met Azure Resource Manager-sjabloon maken
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 7f1443a0-5fa8-4d90-8637-1a977ef0b1f0
ms.service: service-bus-messaging
ms.devlang: tbd
ms.topic: article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 11/10/2017
ms.author: sethm;shvija
ms.openlocfilehash: 384a2fce4bf338ffc4ab6690980c12ad7ff34a6e
ms.sourcegitcommit: 6a22af82b88674cd029387f6cedf0fb9f8830afd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/11/2017
---
# <a name="create-a-service-bus-authorization-rule-for-namespace-and-queue-using-an-azure-resource-manager-template"></a>Een Service Bus-autorisatieregel voor de naamruimte en de wachtrij met een Azure Resource Manager-sjabloon maken

Dit artikel laat zien hoe u een Azure Resource Manager-sjabloon die u maakt een [autorisatieregel](service-bus-authentication-and-authorization.md#shared-access-signature-authentication) voor een Service Bus-naamruimte en de wachtrij. Het artikel wordt uitgelegd hoe om op te geven welke resources worden geïmplementeerd en het definiëren van de parameters die zijn opgegeven wanneer de implementatie wordt uitgevoerd. U kunt deze sjabloon gebruiken voor uw eigen implementaties of de sjabloon aanpassen aan uw eisen.

Zie voor meer informatie over het maken van sjablonen [Azure Resource Manager-sjablonen samenstellen][Authoring Azure Resource Manager templates].

Zie voor de volledige sjabloon, het [Service Bus autorisatie regelsjabloon] [ Service Bus auth rule template] op GitHub.

> [!NOTE]
> De volgende Azure Resource Manager-sjablonen zijn beschikbaar voor download- en implementatie.
> 
> * [Een Service Bus-naamruimte maken](service-bus-resource-manager-namespace.md)
> * [Een Service Bus-naamruimte maken met de wachtrij](service-bus-resource-manager-namespace-queue.md)
> * [Een Service Bus-naamruimte met onderwerp en een abonnement maken](service-bus-resource-manager-namespace-topic.md)
> * [Een Service Bus-naamruimte maken met onderwerp, abonnement en regel](service-bus-resource-manager-namespace-topic-with-rule.md)
> 
> Om te controleren of de meest recente sjablonen, gaat u naar de [Azure-Snelstartsjablonen] [ Azure Quickstart Templates] galerie en zoek naar **Service Bus**.
> 
> 

## <a name="what-will-you-deploy"></a>Wat wilt u implementeren?

Met deze sjabloon kunt u een Service Bus-autorisatieregel voor een naamruimte en Berichtentiteit (in dit geval een wachtrij) implementeren.

Maakt gebruik van deze sjabloon [Shared Access Signature (SAS)](service-bus-sas.md) voor verificatie. SAS kan toepassingen te verifiëren bij Service Bus met behulp van een toegangssleutel die is geconfigureerd op de naamruimte of op de Berichtentiteit (wachtrij of onderwerp) waaraan de specifieke rechten zijn gekoppeld. U kunt deze sleutel vervolgens gebruiken voor het genereren van een SAS-token die clients op hun beurt gebruiken kunnen om Service Bus te verifiëren.

Klik op de volgende knop om de implementatie automatisch uit te voeren:

[![Implementeren in Azure](./media/service-bus-resource-manager-namespace-auth-rule/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F301-servicebus-create-authrule-namespace-and-queue%2Fazuredeploy.json)

## <a name="parameters"></a>Parameters

Met Azure Resource Manager kunt u parameters definiëren voor waarden die u wilt opgeven wanneer de sjabloon wordt geïmplementeerd. De sjabloon bevat een sectie met de naam `Parameters` die de parameterwaarden bevat. U moet een parameter voor de waarden die variëren op basis van het project dat u wilt implementeren of op basis van de omgeving die u om te implementeert definiëren. Geen parameters op voor waarden die u altijd hetzelfde gedefinieerd. De waarde van elke parameter wordt gebruikt in de sjabloon voor het definiëren van de resources die worden geïmplementeerd.

De sjabloon definieert de volgende parameters.

### <a name="servicebusnamespacename"></a>serviceBusNamespaceName
De naam van de Service Bus-naamruimte maken.

```json
"serviceBusNamespaceName": {
"type": "string"
}
```

### <a name="namespaceauthorizationrulename"></a>namespaceAuthorizationRuleName
De naam van de autorisatieregel voor de naamruimte.

```json
"namespaceAuthorizationRuleName ": {
"type": "string"
}
```

### <a name="servicebusqueuename"></a>serviceBusQueueName
De naam van de wachtrij in de Service Bus-naamruimte.

```json
"serviceBusQueueName": {
"type": "string"
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
Maakt een standaard Service Bus-naamruimte van het type **Messaging**, en een Service Bus-autorisatieregel voor de naamruimte en de entiteit.

```json
"resources": [
        {
            "apiVersion": "[variables('sbVersion')]",
            "name": "[parameters('serviceBusNamespaceName')]",
            "type": "Microsoft.ServiceBus/namespaces",
            "location": "[variables('location')]",
            "kind": "Messaging",
            "sku": {
                "name": "StandardSku",
                "tier": "Standard"
            },
            "resources": [
                {
                    "apiVersion": "[variables('sbVersion')]",
                    "name": "[parameters('serviceBusQueueName')]",
                    "type": "Queues",
                    "dependsOn": [
                        "[concat('Microsoft.ServiceBus/namespaces/', parameters('serviceBusNamespaceName'))]"
                    ],
                    "properties": {
                        "path": "[parameters('serviceBusQueueName')]"
                    },
                    "resources": [
                        {
                            "apiVersion": "[variables('sbVersion')]",
                            "name": "[parameters('queueAuthorizationRuleName')]",
                            "type": "authorizationRules",
                            "dependsOn": [
                                "[parameters('serviceBusQueueName')]"
                            ],
                            "properties": {
                                "Rights": ["Listen"]
                            }
                        }
                    ]
                }
            ]
        }, {
            "apiVersion": "[variables('sbVersion')]",
            "name": "[variables('namespaceAuthRuleName')]",
            "type": "Microsoft.ServiceBus/namespaces/authorizationRules",
            "dependsOn": ["[concat('Microsoft.ServiceBus/namespaces/', parameters('serviceBusNamespaceName'))]"],
            "location": "[resourceGroup().location]",
            "properties": {
                "Rights": ["Send"]
            }
        }
    ]
```

## <a name="commands-to-run-deployment"></a>Opdrachten om implementatie uit te voeren
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a>PowerShell
```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName \<resource-group-name\> -TemplateFile <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/301-servicebus-create-authrule-namespace-and-queue/azuredeploy.json>
```

## <a name="azure-cli"></a>Azure CLI
```azurecli
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/301-servicebus-create-authrule-namespace-and-queue/azuredeploy.json>
```

## <a name="next-steps"></a>Volgende stappen
Nu dat u hebt gemaakt en geïmplementeerd met Azure Resource Manager bronnen, meer informatie over deze resources beheren door deze artikelen te bekijken:

* [Servicebus met PowerShell beheren](service-bus-powershell-how-to-provision.md)
* [Service Bus-resources beheren met de Service Bus-Explorer](https://github.com/paolosalvatori/ServiceBusExplorer/releases)
* [Service Bus-verificatie en autorisatie](service-bus-authentication-and-authorization.md)

[Authoring Azure Resource Manager templates]: ../azure-resource-manager/resource-group-authoring-templates.md
[Azure Quickstart Templates]: https://azure.microsoft.com/documentation/templates/?term=service+bus
[Using Azure PowerShell with Azure Resource Manager]: ../azure-resource-manager/powershell-azure-resource-manager.md
[Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../azure-resource-manager/xplat-cli-azure-resource-manager.md
[Service Bus auth rule template]: https://github.com/Azure/azure-quickstart-templates/blob/master/301-servicebus-create-authrule-namespace-and-queue/
