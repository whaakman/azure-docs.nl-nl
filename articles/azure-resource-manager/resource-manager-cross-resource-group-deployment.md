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
ms.date: 12/18/2017
ms.author: tomfitz
ms.openlocfilehash: 48ba938db992ce192d8afb51365d87fba4422590
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2018
---
# <a name="deploy-azure-resources-to-more-than-one-subscription-or-resource-group"></a>Azure-resources implementeren op meer dan één abonnement of resourcegroep

Normaal gesproken het implementeren van alle resources in uw sjabloon met een afzonderlijke [resourcegroep](resource-group-overview.md). Er zijn echter scenario's waarin u wilt implementeren van een set resources samen, maar in verschillende resourcegroepen of abonnementen plaatsen. U wilt bijvoorbeeld de back-virtuele machine voor Azure Site Recovery implementeert naar een afzonderlijke resourcegroep en locatie. Resource Manager kunt u geneste sjablonen voor doel verschillende abonnementen en resourcegroepen dan het abonnement en resourcegroep gebruikt voor de bovenliggende sjabloon gebruiken.

## <a name="specify-a-subscription-and-resource-group"></a>Geef een abonnement en de resource

Als u wilt richten op een andere resource, een sjabloon geneste of gekoppelde te gebruiken. De `Microsoft.Resources/deployments` brontype biedt parameters voor `subscriptionId` en `resourceGroup`. Deze eigenschappen kunnen u Geef een ander abonnement en de resource voor de geneste implementatie. Alle brongroepen moeten bestaan voordat de implementatie wordt uitgevoerd. Als u de abonnement-ID of resourcegroep groep, het abonnement en de resourcegroep van de bovenliggende sjabloon niet opgeeft, wordt gebruikt.

Geef een andere resourcegroep en een abonnement door gebruiken:

```json
"resources": [
    {
        "apiVersion": "2017-05-10",
        "name": "nestedTemplate",
        "type": "Microsoft.Resources/deployments",
        "resourceGroup": "[parameters('secondResourceGroup')]",
        "subscriptionId": "[parameters('secondSubscriptionID')]",
        ...
    }
]
```

Als uw resourcegroepen zich in hetzelfde abonnement, kunt u de **subscriptionId** waarde.

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

Voor het implementeren van de voorbeeldsjabloon, het gebruik van Azure PowerShell 4.0.0 of later of Azure CLI 2.0.0 of hoger.

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

## <a name="example-templates"></a>Voorbeeld-sjablonen

De volgende sjablonen demonstreren meerdere resourcegroepimplementaties. Scripts voor het implementeren van de sjablonen worden weergegeven na de tabel.

|Template  |Beschrijving  |
|---------|---------|
|[Cross-abonnement sjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/crosssubscription.json) |Een opslagaccount aan een resourcegroep en één opslagaccount implementeert op een tweede resourcegroep. Een waarde voor de abonnements-ID bevatten wanneer de tweede resourcegroep bevindt zich in een ander abonnement. |
|[Cross-resource groep Eigenschappensjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/crossresourcegroupproperties.json) |Laat zien hoe de `resourceGroup()` werken wordt omgezet. Deze resources niet is geïmplementeerd. |

### <a name="powershell"></a>PowerShell

Voor PowerShell voor het implementeren van twee storage-accounts op twee resourcegroepen in de **hetzelfde abonnement**, gebruiken:

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

Voor PowerShell twee storage-accounts te implementeren **twee abonnementen**, gebruiken:

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

Voor PowerShell om te testen hoe de **resource group-object** wordt omgezet voor het gebruik van de bovenliggende sjabloon, de inline-sjabloon en de gekoppelde sjabloon:

```powershell
New-AzureRmResourceGroup -Name parentGroup -Location southcentralus
New-AzureRmResourceGroup -Name inlineGroup -Location southcentralus
New-AzureRmResourceGroup -Name linkedGroup -Location southcentralus

New-AzureRmResourceGroupDeployment `
  -ResourceGroupName parentGroup `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/crossresourcegroupproperties.json
```

### <a name="azure-cli"></a>Azure-CLI

Voor Azure CLI voor het implementeren van twee storage-accounts op twee resourcegroepen in de **hetzelfde abonnement**, gebruiken:

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

Voor Azure CLI voor het implementeren van twee storage-accounts naar **twee abonnementen**, gebruiken:

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

Voor Azure CLI om te testen hoe de **resource group-object** wordt omgezet voor het gebruik van de bovenliggende sjabloon, de inline-sjabloon en de gekoppelde sjabloon:

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
