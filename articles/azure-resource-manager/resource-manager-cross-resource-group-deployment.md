---
title: Azure-resources implementeren op meerdere resourcegroepen | Microsoft Docs
description: "Laat zien hoe u meer dan één Azure-resourcegroep als doel tijdens de implementatie."
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
ms.date: 06/15/2017
ms.author: tomfitz
ms.openlocfilehash: d8b041213b269775175a810e585103d3c538557f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-azure-resources-to-more-than-one-resource-group"></a>Azure-resources implementeren op meer dan één resourcegroep

Normaal gesproken implementeren u alle resources in de sjabloon één resourcegroep. Er zijn echter scenario's waarin u wilt implementeren van een set resources samen, maar in verschillende resourcegroepen plaatsen. U wilt bijvoorbeeld de back-virtuele machine voor Azure Site Recovery implementeert naar een afzonderlijke resourcegroep en locatie. Resource Manager kunt u geneste sjablonen met verschillende resourcegroepen bevinden dan de resourcegroep voor de bovenliggende sjabloon gebruikt als doel.

De resourcegroep is de container levenscyclus voor de toepassing en de verzameling van resources. U maakt de resourcegroep buiten de sjabloon en geef de resourcegroep toe te passen tijdens de implementatie. Zie voor een inleiding tot resourcegroepen, [overzicht van Azure Resource Manager](resource-group-overview.md).

## <a name="example-template"></a>Van de voorbeeldsjabloon

Als u wilt richten op een andere resource, moet u een sjabloon geneste of gekoppelde tijdens de implementatie. De `Microsoft.Resources/deployments` brontype beschikt over een `resourceGroup` parameter waarmee u een andere resourcegroep voor de geneste implementatie opgeven. Alle brongroepen moeten bestaan voordat de implementatie wordt uitgevoerd. Het volgende voorbeeld worden twee storage-accounts: in de resourcegroep die is opgegeven tijdens de implementatie, geïmplementeerd en één in een resourcegroep met de naam `crossResourceGroupDeployment`:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "StorageAccountName1": {
            "type": "string"
        },
        "StorageAccountName2": {
            "type": "string"
        }
    },
    "variables": {},
    "resources": [
        {
            "apiVersion": "2017-05-10",
            "name": "nestedTemplate",
            "type": "Microsoft.Resources/deployments",
            "resourceGroup": "crossResourceGroupDeployment",
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
                            "name": "[parameters('StorageAccountName2')]",
                            "apiVersion": "2015-06-15",
                            "location": "West US",
                            "properties": {
                                "accountType": "Standard_LRS"
                            }
                        }
                    ]
                },
                "parameters": {}
            }
        },
        {
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[parameters('StorageAccountName1')]",
            "apiVersion": "2015-06-15",
            "location": "West US",
            "properties": {
                "accountType": "Standard_LRS"
            }
        }
    ]
}
```

Als u instelt `resourceGroup` op de naam van een resourcegroep die niet bestaat, mislukt de implementatie. Als u een waarde op voor geen specifieke `resourceGroup`, Resource Manager maakt gebruik van de bovenliggende resource-groep.  

## <a name="deploy-the-template"></a>De sjabloon implementeren

U kunt de portal, Azure PowerShell of Azure CLI gebruiken voor het implementeren van de voorbeeldsjabloon. Voor Azure PowerShell of Azure CLI, moet u een release in mei 2017 of hoger. De voorbeelden wordt ervan uitgegaan dat u de sjabloon lokaal hebt opgeslagen als een bestand met de naam **crossrgdeployment.json**.

Voor PowerShell:

```powershell
Login-AzureRmAccount

New-AzureRmResourceGroup -Name mainResourceGroup -Location "South Central US"
New-AzureRmResourceGroup -Name crossResourceGroupDeployment -Location "Central US"
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName mainResourceGroup `
  -TemplateFile c:\MyTemplates\crossrgdeployment.json
```

Voor Azure CLI:

```azurecli
az login

az group create --name mainResourceGroup --location "South Central US"
az group create --name crossResourceGroupDeployment --location "Central US"
az group deployment create \
    --name ExampleDeployment \
    --resource-group mainResourceGroup \
    --template-file crossrgdeployment.json
```

Nadat de implementatie is voltooid, ziet u twee resourcegroepen. Elke resourcegroep bevat een opslagaccount.

## <a name="use-resourcegroup-function"></a>Gebruik de functie resourceGroup()

Voor cross-resourcegroepimplementaties, de [resouceGroup() functie](resource-group-template-functions-resource.md#resourcegroup) wordt omgezet is anders op basis van hoe u de geneste sjabloon opgeven. 

Als u een sjabloon in een andere sjabloon insluit, is resouceGroup() in de geneste sjabloon wordt omgezet naar de bovenliggende resource-groep. Een ingesloten sjabloon maakt gebruik van de volgende indeling:

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

Als u een koppeling naar een afzonderlijke sjabloon, wordt resouceGroup() in de gekoppelde sjabloon omgezet in de geneste resourcegroep. Een gekoppelde sjabloon maakt gebruik van de volgende indeling:

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

## <a name="next-steps"></a>Volgende stappen

* Om te begrijpen hoe parameters in de sjabloon definieert, Zie [inzicht in de structuur en de syntaxis van Azure Resource Manager-sjablonen](resource-group-authoring-templates.md).
* Zie voor tips over het oplossen van algemene implementatiefouten [oplossen van veelvoorkomende fouten voor Azure-implementatie met Azure Resource Manager](resource-manager-common-deployment-errors.md).
* Zie voor meer informatie over het implementeren van een sjabloon waarvoor een SAS-token [persoonlijke sjabloon implementeren met SAS-token](resource-manager-powershell-sas-token.md).
