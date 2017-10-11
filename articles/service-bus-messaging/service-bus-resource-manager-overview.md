---
title: Azure Service Bus-resources met behulp van Azure Resource Manager-sjablonen maken | Microsoft Docs
description: Azure Resource Manager-sjablonen gebruiken om het maken van Service Bus-resources te automatiseren
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 24f6a207-0fa4-49cf-8a58-963f9e2fd655
ms.service: service-bus-messaging
ms.devlang: tbd
ms.topic: article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 08/07/2017
ms.author: sethm
ms.openlocfilehash: c8142d8edfd3a527b13d655bac21acf5332f2d14
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="create-service-bus-resources-using-azure-resource-manager-templates"></a>Service Bus-resources met behulp van Azure Resource Manager-sjablonen maken

In dit artikel wordt beschreven hoe maken en implementeren van Service Bus-resources met behulp van Azure Resource Manager-sjablonen, PowerShell en de Service Bus-resourceprovider.

Azure Resource Manager-sjablonen helpen u bij het definiëren van de resources te implementeren voor een oplossing en de parameters en variabelen die u voor het invoeren van waarden voor verschillende omgevingen opgeven. De sjabloon bestaat uit JSON en uitdrukkingen die u gebruiken kunt om waarden voor uw implementatie samen te stellen. Zie voor gedetailleerde informatie over het schrijven van Azure Resource Manager-sjablonen en een overzicht van de indeling template [structuur en de syntaxis van Azure Resource Manager-sjablonen](../azure-resource-manager/resource-group-authoring-templates.md).

> [!NOTE]
> De voorbeelden in dit artikel tonen het gebruik van Azure Resource Manager voor het maken van een Service Bus-naamruimte en Berichtentiteit (wachtrij). Andere voorbeelden sjabloon vindt u de [galerie van Azure-Snelstartsjablonen] [ Azure Quickstart Templates gallery] en zoek naar 'Servicebus'.
>
>

## <a name="service-bus-resource-manager-templates"></a>Service Bus-Resource Manager-sjablonen

Deze Service Bus Azure Resource Manager-sjablonen zijn beschikbaar voor download- en implementatie. Klik op de volgende koppelingen voor meer informatie over elk ervan, met koppelingen naar de sjablonen op GitHub:

* [Een Service Bus-naamruimte maken](service-bus-resource-manager-namespace.md)
* [Een Service Bus-naamruimte maken met de wachtrij](service-bus-resource-manager-namespace-queue.md)
* [Een Service Bus-naamruimte met onderwerp en een abonnement maken](service-bus-resource-manager-namespace-topic.md)
* [Een Service Bus-naamruimte met een wachtrij en autorisatie regel maken](service-bus-resource-manager-namespace-auth-rule.md)
* [Een Service Bus-naamruimte maken met onderwerp, abonnement en regel](service-bus-resource-manager-namespace-topic-with-rule.md)

## <a name="deploy-with-powershell"></a>Implementeren met PowerShell

De volgende procedure beschrijft hoe u PowerShell gebruikt voor het implementeren van een Azure Resource Manager-sjabloon die u maakt een **standaard** servicetier Service Bus-naamruimte en een wachtrij binnen deze naamruimte. In dit voorbeeld is gebaseerd op de [een Service Bus-naamruimte maken met de wachtrij](https://github.com/Azure/azure-quickstart-templates/tree/master/201-servicebus-create-queue) sjabloon. De geschatte werkstroom is als volgt:

1. Installeer PowerShell.
2. De sjabloon en (optioneel) een parameterbestand maken.
3. In PowerShell, moet u zich aanmelden bij uw Azure-account.
4. Een nieuwe resourcegroep maken als deze niet bestaat.
5. De implementatie testen.
6. Indien gewenst kunt u de implementatiemodus.
7. De sjabloon implementeert.

Zie voor meer informatie over het implementeren van Azure Resource Manager-sjablonen [implementeren van resources met Azure Resource Manager-sjablonen][Deploy resources with Azure Resource Manager templates].

### <a name="install-powershell"></a>PowerShell installeren

Azure PowerShell installeren door de instructies in [aan de slag met Azure PowerShell](/powershell/azure/get-started-azureps).

### <a name="create-a-template"></a>Een sjabloon maken

Kloon of kopieer de [201-servicebus-maken-queue](https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-queue/azuredeploy.json) sjabloon vanuit GitHub:

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "serviceBusNamespaceName": {
            "type": "string",
            "metadata": {
                "description": "Name of the Service Bus namespace"
            }
        },
        "serviceBusQueueName": {
            "type": "string",
            "metadata": {
                "description": "Name of the Queue"
            }
        },
        "serviceBusApiVersion": {
            "type": "string",
            "defaultValue": "2015-08-01",
            "metadata": {
                "description": "Service Bus ApiVersion used by the template"
            }
        }
    },
    "variables": {
        "location": "[resourceGroup().location]",
        "sbVersion": "[parameters('serviceBusApiVersion')]",
        "defaultSASKeyName": "RootManageSharedAccessKey",
        "authRuleResourceId": "[resourceId('Microsoft.ServiceBus/namespaces/authorizationRules', parameters('serviceBusNamespaceName'), variables('defaultSASKeyName'))]"
    },
    "resources": [{
        "apiVersion": "[variables('sbVersion')]",
        "name": "[parameters('serviceBusNamespaceName')]",
        "type": "Microsoft.ServiceBus/Namespaces",
        "location": "[variables('location')]",
        "kind": "Messaging",
        "sku": {
            "name": "StandardSku",
            "tier": "Standard"
        },
        "resources": [{
            "apiVersion": "[variables('sbVersion')]",
            "name": "[parameters('serviceBusQueueName')]",
            "type": "Queues",
            "dependsOn": [
                "[concat('Microsoft.ServiceBus/namespaces/', parameters('serviceBusNamespaceName'))]"
            ],
            "properties": {
                "path": "[parameters('serviceBusQueueName')]"
            }
        }]
    }],
    "outputs": {
        "NamespaceConnectionString": {
            "type": "string",
            "value": "[listkeys(variables('authRuleResourceId'), variables('sbVersion')).primaryConnectionString]"
        },
        "SharedAccessPolicyPrimaryKey": {
            "type": "string",
            "value": "[listkeys(variables('authRuleResourceId'), variables('sbVersion')).primaryKey]"
        }
    }
}
```

### <a name="create-a-parameters-file-optional"></a>Maak een parameterbestand (optioneel)

Een optionele parameters als bestand wilt gebruiken, kopieert u de [201-servicebus-maken-queue](https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-queue/azuredeploy.parameters.json) bestand. Vervang de waarde van `serviceBusNamespaceName` met de naam van de Service Bus-naamruimte die u wilt maken in deze implementatie en vervang de waarde van `serviceBusQueueName` met de naam van de wachtrij die u wilt maken.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "serviceBusNamespaceName": {
            "value": "<myNamespaceName>"
        },
        "serviceBusQueueName": {
            "value": "<myQueueName>"
        },
        "serviceBusApiVersion": {
            "value": "2015-08-01"
        }
    }
}
```

Zie voor meer informatie de [Parameters](../azure-resource-manager/resource-group-template-deploy.md#parameter-files) onderwerp.

### <a name="log-in-to-azure-and-set-the-azure-subscription"></a>Aanmelden bij Azure en het Azure-abonnement instellen

Voer de volgende opdracht vanaf een PowerShell-prompt:

```powershell
Login-AzureRmAccount
```

U wordt gevraagd om aan te melden bij uw Azure-account. Voer de volgende opdracht om de beschikbare abonnementen weer te geven na aanmelding.

```powershell
Get-AzureRMSubscription
```

Deze opdracht retourneert een lijst met beschikbare Azure-abonnementen. Kies een abonnement voor de huidige sessie met de volgende opdracht. Vervang `<YourSubscriptionId>` met de GUID voor de Azure-abonnement u wilt gebruiken.

```powershell
Set-AzureRmContext -SubscriptionID <YourSubscriptionId>
```

### <a name="set-the-resource-group"></a>Instellen van de resourcegroep

Als u nog geen een bestaande resource groep, maakt u een nieuwe resourcegroep met de ** New-AzureRmResourceGroup ** opdracht. Geef de naam van de resourcegroep en de locatie die u wilt gebruiken. Bijvoorbeeld:

```powershell
New-AzureRmResourceGroup -Name MyDemoRG -Location "West US"
```

Als dit lukt, wordt een samenvatting van de nieuwe resourcegroep wordt weergegeven.

```powershell
ResourceGroupName : MyDemoRG
Location          : westus
ProvisioningState : Succeeded
Tags              :
ResourceId        : /subscriptions/<GUID>/resourceGroups/MyDemoRG
```

### <a name="test-the-deployment"></a>De implementatie testen

Valideren van uw implementatie door de `Test-AzureRmResourceGroupDeployment` cmdlet. Bij het testen van de implementatie, Geef parameters op exact dezelfde manier als bij het uitvoeren van de implementatie.

```powershell
Test-AzureRmResourceGroupDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json
```

### <a name="create-the-deployment"></a>De implementatie maken

Als u wilt maken van nieuwe implementatie uitvoeren de `New-AzureRmResourceGroupDeployment` cmdlet, en geef de benodigde parameters op wanneer u wordt gevraagd. De parameters bevatten een naam voor uw implementatie, de naam van de resourcegroep en het pad of de URL naar het sjabloonbestand. Als de **modus** parameter niet is opgegeven, de standaardwaarde van **incrementele** wordt gebruikt. Zie voor meer informatie [incrementele en volledige implementaties](../azure-resource-manager/resource-group-template-deploy.md#incremental-and-complete-deployments).

De volgende opdracht wordt u gevraagd om de drie parameters in de PowerShell-venster:

```powershell
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json
```

Geef in plaats daarvan een parameterbestand door de volgende opdracht te gebruiken.

```powershell
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json -TemplateParameterFile <path to parameters file>\azuredeploy.parameters.json
```

U kunt ook parameters inline gebruiken wanneer u de deployment-cmdlet uitvoert. De opdracht is als volgt:

```powershell
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json -parameterName "parameterValue"
```

Om uit te voeren een [voltooid](../azure-resource-manager/resource-group-template-deploy.md#incremental-and-complete-deployments) implementatie, stel de **modus** -parameter voor **Complete**:

```powershell
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -Mode Complete -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json
```

### <a name="verify-the-deployment"></a>De implementatie controleren
Als de bronnen worden geïmplementeerd, wordt een samenvatting van de implementatie weergegeven in het venster PowerShell:

```powershell
DeploymentName    : MyDemoDeployment
ResourceGroupName : MyDemoRG
ProvisioningState : Succeeded
Timestamp         : 4/19/2016 10:38:30 PM
Mode              : Incremental
TemplateLink      :
Parameters        :
                    Name             Type                       Value
                    ===============  =========================  ==========
                    serviceBusNamespaceName  String             <namespaceName>
                    serviceBusQueueName  String                 <queueName>
                    serviceBusApiVersion  String                2015-08-01

```

## <a name="next-steps"></a>Volgende stappen
U hebt nu de basiswerkstroom en opdrachten voor het implementeren van een Azure Resource Manager-sjabloon gezien. Ga naar de volgende koppelingen voor meer gedetailleerde informatie:

* [Overzicht van Azure Resource Manager][Azure Resource Manager overview]
* [Resources met Resource Manager-sjablonen en Azure PowerShell implementeren][Deploy resources with Azure Resource Manager templates]
* [Azure Resource Manager-sjablonen maken](../azure-resource-manager/resource-group-authoring-templates.md)

[Azure Resource Manager overview]: ../azure-resource-manager/resource-group-overview.md
[Deploy resources with Azure Resource Manager templates]: ../azure-resource-manager/resource-group-template-deploy.md
[Azure Quickstart Templates gallery]: https://azure.microsoft.com/documentation/templates/?term=service+bus
