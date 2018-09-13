---
title: Azure-resources implementeren op meerdere abonnement en de resourcegroep groepen | Microsoft Docs
description: Laat zien hoe u meer dan één Azure-abonnement en de resourcegroep groep als doel tijdens de implementatie.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: ''
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/02/2018
ms.author: tomfitz
ms.openlocfilehash: fec075a744b5f47a4be7f1b960cceedfea7b9a2c
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/12/2018
ms.locfileid: "35644096"
---
# <a name="deploy-azure-resources-to-more-than-one-subscription-or-resource-group"></a>Azure-resources implementeren op meer dan één abonnement of resourcegroep

Normaal gesproken implementeren u alle resources in uw sjabloon naar een enkele [resourcegroep](resource-group-overview.md). Er zijn echter scenario's waarin u wilt een set met resources samen te implementeren, maar plaats deze in verschillende resourcegroepen of abonnementen. U wilt bijvoorbeeld de back-virtuele machine voor Azure Site Recovery implementeren op een afzonderlijke resourcegroep en locatie. Resource Manager kunt u gebruikmaken van geneste sjablonen op doel verschillende abonnementen en resourcegroepen dan het abonnement en de resourcegroep voor de bovenliggende sjabloon gebruikt.

> [!NOTE]
> U kunt implementeren met slechts vijf resourcegroepen in een enkele implementatie. Normaal gesproken deze beperking betekent dat u kunt implementeren op een resourcegroep die is opgegeven voor de bovenliggende sjabloon en maximaal vier resourcegroepen in geneste of gekoppelde implementaties. Echter als uw bovenliggende sjabloon alleen geneste of gekoppelde sjablonen bevat en wordt niet zelf geen resources geïmplementeerd, kunt u maximaal vijf resourcegroepen in opnemen geneste of gekoppelde implementaties.

## <a name="specify-a-subscription-and-resource-group"></a>Geef een abonnement en de resourcegroep

Als u wilt een andere resource als doel, door een geneste of gekoppelde sjabloon te gebruiken. De `Microsoft.Resources/deployments` resourcetype biedt parameters voor `subscriptionId` en `resourceGroup`. Deze eigenschappen kunnen u een andere groep met abonnement en de resourcegroep voor de geneste implementatie opgeven. Alle resourcegroepen moeten bestaan voordat de implementatie wordt uitgevoerd. Als u de abonnement-ID of resource-groep, het abonnement en de resourcegroep van de bovenliggende sjabloon niet opgeeft, wordt gebruikt.

Het account dat u gebruikt om de sjabloon te implementeren moet gemachtigd zijn om te implementeren voor de opgegeven abonnements-ID. Als het opgegeven abonnement in een andere Azure Active Directory-tenant bestaat, moet u [gastgebruikers toevoegen van een andere directory](../active-directory/active-directory-b2b-what-is-azure-ad-b2b.md).

Als u een andere resourcegroep en een abonnement, gebruikt u:

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

Als uw resourcegroepen bevinden zich in hetzelfde abonnement, kunt u de **subscriptionId** waarde.

Het volgende voorbeeld worden twee opslagaccounts - in de resourcegroep die is opgegeven tijdens de implementatie, geïmplementeerd en in een resourcegroep is opgegeven in de `secondResourceGroup` parameter:

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

Als u `resourceGroup` op de naam van een resourcegroep die niet bestaat, mislukt de implementatie.

## <a name="use-the-resourcegroup-and-subscription-functions"></a>Gebruik de functies resourceGroup() en subscription()

Voor cross-brongroepimplementaties, de [resourceGroup()](resource-group-template-functions-resource.md#resourcegroup) en [subscription()](resource-group-template-functions-resource.md#subscription) functies los anders op basis van hoe u de geneste sjabloon opgeven. 

Als u een sjabloon in een andere sjabloon insluit, worden de functies in de geneste sjabloon omgezet naar de bovenliggende resource group- en -abonnement. Een ingesloten sjabloon maakt gebruik van de volgende indeling:

```json
"apiVersion": "2017-05-10",
"name": "embeddedTemplate",
"type": "Microsoft.Resources/deployments",
"resourceGroup": "crossResourceGroupDeployment",
"properties": {
    "mode": "Incremental",
    "template": {
        ...
        resourceGroup() and subscription() refer to parent resource group/subscription
    }
}
```

Als u een koppeling naar een afzonderlijke sjabloon, worden de functies in de gekoppelde sjabloon omgezet naar het geneste resourcegroep en het abonnement. Een gekoppelde sjabloon maakt gebruik van de volgende indeling:

```json
"apiVersion": "2017-05-10",
"name": "linkedTemplate",
"type": "Microsoft.Resources/deployments",
"resourceGroup": "crossResourceGroupDeployment",
"properties": {
    "mode": "Incremental",
    "templateLink": {
        ...
        resourceGroup() and subscription() in linked template refer to linked resource group/subscription
    }
}
```

## <a name="example-templates"></a>Voorbeeldsjablonen

De volgende sjablonen laten zien meerdere brongroepimplementaties. Scripts voor het implementeren van de sjablonen worden weergegeven na de tabel.

|Template  |Beschrijving  |
|---------|---------|
|[Cross-sjabloon voor abonnement](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/crosssubscription.json) |Implementeert een storage-account aan een resourcegroep en één opslagaccount naar een tweede resourcegroep. Een waarde bevatten voor de abonnements-ID als de tweede resourcegroep bevindt zich op een ander abonnement. |
|[Cross-eigenschappen resourcegroepsjabloon](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/crossresourcegroupproperties.json) |Laat zien hoe de `resourceGroup()` wordt omgezet functie. Het biedt niet alle resources te implementeren. |

### <a name="powershell"></a>PowerShell

Voor PowerShell, het implementeren van twee opslagaccounts op twee resourcegroepen in de **hetzelfde abonnement**, gebruiken:

```azurepowershell-interactive
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

Voor PowerShell, om te implementeren twee opslagaccounts op **twee abonnementen**, gebruiken:

```azurepowershell-interactive
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

Voor PowerShell, om te testen hoe de **resource group-object** oplossing voor het gebruik van bovenliggende sjabloon, inline-sjabloon en gekoppelde sjabloon:

```azurepowershell-interactive
New-AzureRmResourceGroup -Name parentGroup -Location southcentralus
New-AzureRmResourceGroup -Name inlineGroup -Location southcentralus
New-AzureRmResourceGroup -Name linkedGroup -Location southcentralus

New-AzureRmResourceGroupDeployment `
  -ResourceGroupName parentGroup `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/crossresourcegroupproperties.json
```

In het voorgaande voorbeeld, beide **parentRG** en **inlineRG** worden omgezet naar **parentGroup**. **linkedRG** wordt omgezet naar **linkedGroup**. De uitvoer uit het vorige voorbeeld is:

```powershell
 Name             Type                       Value
 ===============  =========================  ==========
 parentRG         Object                     {
                                               "id": "/subscriptions/<subscription-id>/resourceGroups/parentGroup",
                                               "name": "parentGroup",
                                               "location": "southcentralus",
                                               "properties": {
                                                 "provisioningState": "Succeeded"
                                               }
                                             }
 inlineRG         Object                     {
                                               "id": "/subscriptions/<subscription-id>/resourceGroups/parentGroup",
                                               "name": "parentGroup",
                                               "location": "southcentralus",
                                               "properties": {
                                                 "provisioningState": "Succeeded"
                                               }
                                             }
 linkedRG         Object                     {
                                               "id": "/subscriptions/<subscription-id>/resourceGroups/linkedGroup",
                                               "name": "linkedGroup",
                                               "location": "southcentralus",
                                               "properties": {
                                                 "provisioningState": "Succeeded"
                                               }
                                             }
```

### <a name="azure-cli"></a>Azure-CLI

Voor Azure CLI, voor het implementeren van twee opslagaccounts aan twee resourcegroepen in de **hetzelfde abonnement**, gebruiken:

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

Voor Azure CLI, voor het implementeren van twee opslagaccounts op **twee abonnementen**, gebruiken:

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

Voor Azure CLI, om te testen hoe de **resource group-object** oplossing voor het gebruik van bovenliggende sjabloon, inline-sjabloon en gekoppelde sjabloon:

```azurecli-interactive
az group create --name parentGroup --location southcentralus
az group create --name inlineGroup --location southcentralus
az group create --name linkedGroup --location southcentralus

az group deployment create \
  --name ExampleDeployment \
  --resource-group parentGroup \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/crossresourcegroupproperties.json 
```

In het voorgaande voorbeeld, beide **parentRG** en **inlineRG** worden omgezet naar **parentGroup**. **linkedRG** wordt omgezet naar **linkedGroup**. De uitvoer uit het vorige voorbeeld is:

```azurecli
...
"outputs": {
  "inlineRG": {
    "type": "Object",
    "value": {
      "id": "/subscriptions/<subscription-id>/resourceGroups/parentGroup",
      "location": "southcentralus",
      "name": "parentGroup",
      "properties": {
        "provisioningState": "Succeeded"
      }
    }
  },
  "linkedRG": {
    "type": "Object",
    "value": {
      "id": "/subscriptions/<subscription-id>/resourceGroups/linkedGroup",
      "location": "southcentralus",
      "name": "linkedGroup",
      "properties": {
        "provisioningState": "Succeeded"
      }
    }
  },
  "parentRG": {
    "type": "Object",
    "value": {
      "id": "/subscriptions/<subscription-id>/resourceGroups/parentGroup",
      "location": "southcentralus",
      "name": "parentGroup",
      "properties": {
        "provisioningState": "Succeeded"
      }
    }
  }
},
...
```

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over het definiëren van parameters in uw sjabloon, [inzicht in de structuur en de syntaxis van Azure Resource Manager-sjablonen](resource-group-authoring-templates.md).
* Zie voor tips over het oplossen van veelvoorkomende implementatiefouten [veelvoorkomende problemen oplossen Azure-implementatie met Azure Resource Manager](resource-manager-common-deployment-errors.md).
* Zie voor meer informatie over het implementeren van een sjabloon waarvoor een SAS-token [persoonlijke sjablonen implementeren met SAS-token](resource-manager-powershell-sas-token.md).
