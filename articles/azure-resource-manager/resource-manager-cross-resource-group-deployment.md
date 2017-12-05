---
title: Azure-resources aan meerdere abonnement en de resource-groepen implementeren | Microsoft Docs
description: "Laat zien hoe u meer dan één Azure-abonnement en de resource groep als doel tijdens de implementatie."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: 
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/01/2017
ms.author: tomfitz
ms.openlocfilehash: 763f46b9b5be7edf06ee0604bfc51a2482405b60
ms.sourcegitcommit: 7136d06474dd20bb8ef6a821c8d7e31edf3a2820
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/05/2017
---
# <a name="deploy-azure-resources-to-more-than-one-subscription-or-resource-group"></a>Azure-resources implementeren op meer dan één abonnement of resourcegroep

Normaal gesproken implementeren u alle resources in de sjabloon één resourcegroep. Er zijn echter scenario's waarin u wilt implementeren van een set resources samen, maar in verschillende resourcegroepen of abonnementen plaatsen. U wilt bijvoorbeeld de back-virtuele machine voor Azure Site Recovery implementeert naar een afzonderlijke resourcegroep en locatie. Resource Manager kunt u geneste sjablonen voor doel verschillende abonnementen en resourcegroepen dan het abonnement en resourcegroep gebruikt voor de bovenliggende sjabloon gebruiken.

De resourcegroep is de container levenscyclus voor de toepassing en de verzameling van resources. U maakt de resourcegroep buiten de sjabloon en geef de resourcegroep toe te passen tijdens de implementatie. Zie voor een inleiding tot resourcegroepen, [overzicht van Azure Resource Manager](resource-group-overview.md).

## <a name="specify-a-subscription-and-resource-group"></a>Geef een abonnement en de resource

Als u wilt richten op een andere resource, moet u een sjabloon geneste of gekoppelde tijdens de implementatie. De `Microsoft.Resources/deployments` brontype biedt parameters voor `subscriptionId` en `resourceGroup`. Deze eigenschappen kunnen u Geef een ander abonnement en de resource voor de geneste implementatie. Alle brongroepen moeten bestaan voordat de implementatie wordt uitgevoerd. Als u de abonnement-ID of resourcegroep groep, het abonnement en de resourcegroep van de bovenliggende sjabloon niet opgeeft, wordt gebruikt.

Het volgende voorbeeld worden twee storage-accounts: in de resourcegroep die is opgegeven tijdens de implementatie, geïmplementeerd en één in een resourcegroep is opgegeven in de `secondResourceGroup` parameter:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "storagePrefix": {
            "type": "string",
            "maxLength": 11
        },
        "secondResourceGroup": {
            "type": "string"
        },
        "secondSubscriptionID": {
            "type": "string",
            "defaultValue": ""
        },
        "secondStorageLocation": {
            "type": "string",
            "defaultValue": "[resourceGroup().location]"
        }
    },
    "variables": {
        "firstStorageName": "[concat(parameters('storagePrefix'), uniqueString(resourceGroup().id))]",
        "secondStorageName": "[concat(parameters('storagePrefix'), uniqueString(parameters('secondSubscriptionID'), parameters('secondResourceGroup')))]"
    },
    "resources": [
        {
            "apiVersion": "2017-05-10",
            "name": "nestedTemplate",
            "type": "Microsoft.Resources/deployments",
            "resourceGroup": "[parameters('secondResourceGroup')]",
            "subscriptionId": "[parameters('secondSubscriptionID')]",
            "properties": {
                "mode": "Incremental",
                "template": {
                    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
                    "contentVersion": "1.0.0.0",
                    "parameters": {},
                    "variables": {},
                    "resources": [
                        {
                            "type": "Microsoft.Storage/storageAccounts",
                            "name": "[variables('secondStorageName')]",
                            "apiVersion": "2017-06-01",
                            "location": "[parameters('secondStorageLocation')]",
                            "sku":{
                                "name": "Standard_LRS"
                            },
                            "kind": "Storage",
                            "properties": {
                            }
                        }
                    ]
                },
                "parameters": {}
            }
        },
        {
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[variables('firstStorageName')]",
            "apiVersion": "2017-06-01",
            "location": "[resourceGroup().location]",
            "sku":{
                "name": "Standard_LRS"
            },
            "kind": "Storage",
            "properties": {
            }
        }
    ]
}
```

Als u instelt `resourceGroup` op de naam van een resourcegroep die niet bestaat, mislukt de implementatie.

## <a name="deploy-the-template"></a>De sjabloon implementeren

Gebruik voor het implementeren van de voorbeeldsjabloon, een versie van Azure PowerShell of Azure CLI van mei 2017 of hoger. Voor deze voorbeelden gebruiken de [cross-abonnement sjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/crosssubscription.json) in GitHub.

### <a name="two-resource-groups-in-the-same-subscription"></a>Twee brongroepen in hetzelfde abonnement

Voor PowerShell voor het implementeren van twee storage-accounts aan twee resourcegroepen in hetzelfde abonnement, gebruiken:

```powershell
$firstRG = "primarygroup"
$secondRG = "secondarygroup"

New-AzureRmResourceGroup -Name $firstRG -Location southcentralus
New-AzureRmResourceGroup -Name $secondRG -Location eastus

New-AzureRmResourceGroupDeployment `
  -ResourceGroupName $firstRG `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/crosssubscription.json `
  -storagePrefix storage `
  -secondResourceGroup $secondRG `
  -secondStorageLocation eastus
```

Voor Azure CLI voor het implementeren van twee storage-accounts aan twee resourcegroepen in hetzelfde abonnement, gebruiken:

```azurecli-interactive
firstRG="primarygroup"
secondRG="secondarygroup"

az group create --name $firstRG --location southcentralus
az group create --name $secondRG --location eastus
az group deployment create \
  --name ExampleDeployment \
  --resource-group $firstRG \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/crosssubscription.json \
  --parameters storagePrefix=tfstorage secondResourceGroup=$secondRG secondStorageLocation=eastus
```

Nadat de implementatie is voltooid, ziet u twee resourcegroepen. Elke resourcegroep bevat een opslagaccount.

### <a name="two-resource-groups-in-different-subscriptions"></a>Twee brongroepen tot verschillende abonnementen behoren

Voor PowerShell voor het implementeren van twee storage-accounts aan abonnementen, twee, gebruiken:

```powershell
$firstRG = "primarygroup"
$secondRG = "secondarygroup"

$firstSub = "<first-subscription-id>"
$secondSub = "<second-subscription-id>"

Select-AzureRmSubscription -Subscription $secondSub
New-AzureRmResourceGroup -Name $secondRG -Location eastus

Select-AzureRmSubscription -Subscription $firstSub
New-AzureRmResourceGroup -Name $firstRG -Location southcentralus

New-AzureRmResourceGroupDeployment `
  -ResourceGroupName $firstRG `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/crosssubscription.json `
  -storagePrefix storage `
  -secondResourceGroup $secondRG `
  -secondStorageLocation eastus `
  -secondSubscriptionID $secondSub
```

Voor Azure CLI voor het implementeren van twee storage-accounts aan abonnementen, twee, gebruiken:

```azurecli-interactive
firstRG="primarygroup"
secondRG="secondarygroup"

firstSub="<first-subscription-id>"
secondSub="<second-subscription-id>"

az account set --subscription $secondSub
az group create --name $secondRG --location eastus

az account set --subscription $firstSub
az group create --name $firstRG --location southcentralus

az group deployment create \
  --name ExampleDeployment \
  --resource-group $firstRG \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/crosssubscription.json \
  --parameters storagePrefix=storage secondResourceGroup=$secondRG secondStorageLocation=eastus secondSubscriptionID=$secondSub
```

## <a name="use-the-resourcegroup-function"></a>Gebruik de functie resourceGroup()

Voor cross-resourcegroepimplementaties, de [resourceGroup() functie](resource-group-template-functions-resource.md#resourcegroup) wordt omgezet is anders op basis van hoe u de geneste sjabloon opgeven. 

Als u een sjabloon in een andere sjabloon insluit, is resourceGroup() in de geneste sjabloon wordt omgezet naar de bovenliggende resource-groep. Een ingesloten sjabloon maakt gebruik van de volgende indeling:

```json
"apiVersion": "2017-05-10",
"name": "embeddedTemplate",
"type": "Microsoft.Resources/deployments",
"resourceGroup": "crossResourceGroupDeployment",
"properties": {
    "mode": "Incremental",
    "template": {
        ...
        resourceGroup() refers to parent resource group
    }
}
```

Als u een koppeling naar een afzonderlijke sjabloon, wordt resourceGroup() in de gekoppelde sjabloon omgezet in de geneste resourcegroep. Een gekoppelde sjabloon maakt gebruik van de volgende indeling:

```json
"apiVersion": "2017-05-10",
"name": "linkedTemplate",
"type": "Microsoft.Resources/deployments",
"resourceGroup": "crossResourceGroupDeployment",
"properties": {
    "mode": "Incremental",
    "templateLink": {
        ...
        resourceGroup() in linked template refers to linked resource group
    }
}
```

Voor het testen van de verschillende manieren `resourceGroup()` wordt omgezet, implementeert een [voorbeeldsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/crossresourcegroupproperties.json) die de resource-group-object geretourneerd voor de bovenliggende sjabloon, inline-sjabloon en de gekoppelde sjabloon. De sjabloon van het bovenliggende en inline die beide in dezelfde resourcegroep omzetten. De gekoppelde sjabloon wordt omgezet naar de gekoppelde resourcegroep.

Gebruik voor PowerShell:

```powershell
New-AzureRmResourceGroup -Name parentGroup -Location southcentralus
New-AzureRmResourceGroup -Name inlineGroup -Location southcentralus
New-AzureRmResourceGroup -Name linkedGroup -Location southcentralus

New-AzureRmResourceGroupDeployment `
  -ResourceGroupName parentGroup `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/crossresourcegroupproperties.json
```

Gebruik voor Azure CLI:

```azurecli-interactive
az group create --name parentGroup --location southcentralus
az group create --name inlineGroup --location southcentralus
az group create --name linkedGroup --location southcentralus

az group deployment create \
  --name ExampleDeployment \
  --resource-group parentGroup \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/crossresourcegroupproperties.json 
```

## <a name="next-steps"></a>Volgende stappen

* Om te begrijpen hoe parameters in de sjabloon definieert, Zie [inzicht in de structuur en de syntaxis van Azure Resource Manager-sjablonen](resource-group-authoring-templates.md).
* Zie voor tips over het oplossen van algemene implementatiefouten [oplossen van veelvoorkomende fouten voor Azure-implementatie met Azure Resource Manager](resource-manager-common-deployment-errors.md).
* Zie voor meer informatie over het implementeren van een sjabloon waarvoor een SAS-token [persoonlijke sjabloon implementeren met SAS-token](resource-manager-powershell-sas-token.md).
