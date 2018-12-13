---
title: Resourcegroep maken in Azure Resource Manager-sjabloon | Microsoft Docs
description: Beschrijft hoe u een nieuwe resourcegroep maken in een Azure Resource Manager-sjabloon.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/11/2018
ms.author: tomfitz
ms.openlocfilehash: b92790161ae9ad9b56400429677ab0fdd6d552e9
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53316230"
---
# <a name="create-resource-groups-in-azure-resource-manager-templates"></a>Resourcegroepen in Azure Resource Manager-sjablonen maken

Voor het maken van een resourcegroep in een Azure Resource Manager-sjabloon, definieert een **Microsoft.Resources/resourceGroups** resource met een naam en locatie voor de resourcegroep. De sjabloon implementeren in uw Azure-abonnement. Zie voor meer informatie over niveau abonnementimplementaties [resources implementeren op een Azure-abonnement](deploy-to-subscription.md).

U kunt resources ook implementeren in deze resourcegroep in de dezelfde sjabloon.

Dit artikel wordt gebruikgemaakt van Azure CLI en PowerShell om de sjablonen te implementeren. U kunt de portal niet gebruiken voor de sjablonen niet implementeren omdat de interface van de portal worden geïmplementeerd op de resourcegroep, niet de Azure-abonnement.

## <a name="create-empty-resource-group"></a>Lege resourcegroep maken

Het volgende voorbeeld wordt een lege resourcegroep.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
    "contentVersion": "1.0.0.1",
    "parameters": {
        "rgName": {
            "type": "string"
        },
        "rgLocation": {
            "type": "string"
        }
    },
    "variables": {},
    "resources": [
        {
            "type": "Microsoft.Resources/resourceGroups",
            "apiVersion": "2018-05-01",
            "location": "[parameters('rgLocation')]",
            "name": "[parameters('rgName')]",
            "properties": {}
        }
    ],
    "outputs": {}
}
```

Voor het implementeren van deze sjabloon met Azure CLI gebruiken:

```azurecli-interactive
az deployment create \
  -n demoEmptyRG \
  -l southcentralus \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/emptyRG.json \
  --parameters rgName=demoRG rgLocation=northcentralus
```

Voor het implementeren van deze sjabloon met PowerShell, gebruikt u:

```azurepowershell-interactive
New-AzureRmDeployment `
  -Name demoEmptyRG `
  -Location southcentralus `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/emptyRG.json `
  -rgName demogroup `
  -rgLocation northcentralus
```

## <a name="create-several-resource-groups"></a>Maken van meerdere resourcegroepen

Gebruik de [kopie element](resource-group-create-multiple.md) met resourcegroepen om meer dan één resourcegroep te maken. 

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
    "contentVersion": "1.0.0.1",
    "parameters": {
        "rgNamePrefix": {
            "type": "string"
        },
        "rgLocation": {
            "type": "string"
        },
        "instanceCount": {
            "type": "int"
        }
    },
    "variables": {},
    "resources": [
        {
            "type": "Microsoft.Resources/resourceGroups",
            "apiVersion": "2018-05-01",
            "location": "[parameters('rgLocation')]",
            "name": "[concat(parameters('rgNamePrefix'), copyIndex())]",
            "copy": {
                "name": "rgCopy",
                "count": "[parameters('instanceCount')]"
            },
            "properties": {}
        }
    ],
    "outputs": {}
}
```

Het implementeren van deze sjabloon met Azure CLI en drie resourcegroepen gemaakt, gebruiken:

```azurecli-interactive
az deployment create \
  -n demoCopyRG \
  -l southcentralus \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/copyRG.json \
  --parameters rgNamePrefix=demoRG rgLocation=northcentralus instanceCount=3
```

Voor het implementeren van deze sjabloon met PowerShell, gebruikt u:

```azurepowershell-interactive
New-AzureRmDeployment `
  -Name demoCopyRG `
  -Location southcentralus `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/copyRG.json `
  -rgNamePrefix demogroup `
  -rgLocation northcentralus `
  -instanceCount 3
```

## <a name="create-resource-group-and-deploy-resource"></a>Resourcegroep maken en implementeren van resource

Voor het maken van de resourcegroep en resources te implementeren, moet u een geneste sjabloon gebruiken. De geneste sjabloon definieert de resources te implementeren in de resourcegroep. Stel de geneste sjabloon als afhankelijk van de resourcegroep om te controleren of dat de resourcegroep bestaat voor het implementeren van de resources.

Het volgende voorbeeld wordt een resourcegroep en implementeert een opslagaccount in de resourcegroep.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
    "contentVersion": "1.0.0.1",
    "parameters": {
        "rgName": {
            "type": "string"
        },
        "rgLocation": {
            "type": "string"
        },
        "storagePrefix": {
            "type": "string",
            "maxLength": 11
        }
    },
    "variables": {
        "storageName": "[concat(parameters('storagePrefix'), uniqueString(subscription().id, parameters('rgName')))]"
    },
    "resources": [
        {
            "type": "Microsoft.Resources/resourceGroups",
            "apiVersion": "2018-05-01",
            "location": "[parameters('rgLocation')]",
            "name": "[parameters('rgName')]",
            "properties": {}
        },
        {
            "type": "Microsoft.Resources/deployments",
            "apiVersion": "2018-05-01",
            "name": "storageDeployment",
            "resourceGroup": "[parameters('rgName')]",
            "dependsOn": [
                "[resourceId('Microsoft.Resources/resourceGroups/', parameters('rgName'))]"
            ],
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
                            "apiVersion": "2017-10-01",
                            "name": "[variables('storageName')]",
                            "location": "[parameters('rgLocation')]",
                            "kind": "StorageV2",
                            "sku": {
                                "name": "Standard_LRS"
                            }
                        }
                    ],
                    "outputs": {}
                }
            }
        }
    ],
    "outputs": {}
}
```

Voor het implementeren van deze sjabloon met Azure CLI gebruiken:

```azurecli-interactive
az deployment create \
  -n demoRGStorage \
  -l southcentralus \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/newRGWithStorage.json \
  --parameters rgName=rgStorage rgLocation=northcentralus storagePrefix=storage
```

Voor het implementeren van deze sjabloon met PowerShell, gebruikt u:

```azurepowershell-interactive
New-AzureRmDeployment `
  -Name demoRGStorage `
  -Location southcentralus `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/newRGWithStorage.json `
  -rgName rgStorage `
  -rgLocation northcentralus `
  -storagePrefix storage
```

## <a name="next-steps"></a>Volgende stappen
* Zie voor meer informatie over niveau abonnementimplementaties, [resources implementeren op een Azure-abonnement](deploy-to-subscription.md).
* Zie voor meer informatie over het oplossen van afhankelijkheden tijdens de implementatie, [veelvoorkomende problemen oplossen Azure-implementatie met Azure Resource Manager](resource-manager-common-deployment-errors.md).
* Zie voor meer informatie over het maken van Azure Resource Manager-sjablonen, [-sjablonen maken](resource-group-authoring-templates.md). 
* Zie voor een lijst van de beschikbare functies in een sjabloon, [sjabloonfuncties](resource-group-template-functions.md).

