---
title: Met Azure PowerShell Resource Manager-sjabloon exporteren | Microsoft Docs
description: Azure Resource Manager en Azure PowerShell gebruiken om te exporteren van een sjabloon van een resourcegroep.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/23/2018
ms.author: tomfitz
ms.openlocfilehash: 0313266c9e9bf7814d4581dc04d70cf80e1f8172
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55494711"
---
# <a name="export-azure-resource-manager-templates-with-powershell"></a>Exporteren van Azure Resource Manager-sjablonen met PowerShell

Met Resource Manager kunt u een Resource Manager-sjabloon exporteren uit bestaande resources in uw abonnement. Deze gegenereerde sjabloon kunt u vervolgens gebruiken om meer te weten te komen over de sjabloonsyntaxis of om desgewenst het hergebruik van uw oplossing te automatiseren.

Het is belangrijk te weten dat er zijn twee verschillende manieren om een sjabloon te exporteren:

* U kunt exporteren de **daadwerkelijke sjabloon die wordt gebruikt voor een implementatie**. De geëxporteerde sjabloon bevat alle parameters en variabelen precies zoals ze worden weergegeven in de oorspronkelijke sjabloon. Deze methode is handig als u wilt ophalen van een sjabloon.
* U kunt een **gegenereerde sjabloon exporteren met de huidige status van de resourcegroep**. De geëxporteerde sjabloon is niet gebaseerd op een sjabloon die u voor de implementatie hebt gebruikt. In plaats daarvan wordt een sjabloon die een 'snapshot' of 'back-up van de resourcegroep is gemaakt. De geëxporteerde sjabloon heeft veel vastgelegde waarden en waarschijnlijk niet zoveel parameters als u doorgaans zou definiëren. Gebruik deze optie om resources aan dezelfde resourcegroep opnieuw te implementeren. Mogelijk moet u voor het gebruik van deze sjabloon voor een andere resourcegroep, aanzienlijk te wijzigen.

In dit artikel bevat beide methoden.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="deploy-a-solution"></a>Een oplossing implementeren

Ter illustratie van beide methoden voor het exporteren van een sjabloon, laten we beginnen met het implementeren van een oplossing aan uw abonnement. Als u al een resourcegroep in uw abonnement die u wilt exporteren hebt, hebt u niet om deze oplossing te implementeren. Echter, de rest van dit artikel verwijst naar de sjabloon voor deze oplossing. Het voorbeeldscript implementeert u een opslagaccount.

```powershell
New-AzResourceGroup -Name ExampleGroup -Location "South Central US"
New-AzResourceGroupDeployment -ResourceGroupName ExampleGroup `
  -DeploymentName NewStorage
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json
```  

## <a name="save-template-from-deployment-history"></a>Sjabloon opslaan vanuit implementatiegeschiedenis

U kunt een sjabloon uit de implementatiegeschiedenis ophalen met behulp van de [opslaan AzureRmResourceGroupDeploymentTemplate](/powershell/module/az.resources/save-azresourcegroupdeploymenttemplate) opdracht. Het volgende voorbeeld wordt de sjabloon die u eerder hebt geïmplementeerd:

```powershell
Save-AzResourceGroupDeploymentTemplate -ResourceGroupName ExampleGroup -DeploymentName NewStorage
```

Retourneert de locatie van de sjabloon.

```powershell
Path
----
C:\Users\exampleuser\NewStorage.json
```

Open het bestand en u ziet dat het is de exacte sjabloon die u voor implementatie gebruikt. De parameters en variabelen overeenkomen met de sjabloon vanuit GitHub. U kunt deze sjabloon opnieuw implementeren.

## <a name="export-resource-group-as-template"></a>Resourcegroep exporteren als sjabloon

U kunt een sjabloon die staat voor de huidige status van een resourcegroep met behulp van ophalen in plaats van het ophalen van een sjabloon uit de implementatiegeschiedenis, de [Export-AzureRmResourceGroup](/powershell/module/az.resources/export-azresourcegroup) opdracht. U kunt deze opdracht gebruiken wanneer u veel wijzigingen hebt aangebracht aan de resourcegroep en alle wijzigingen Hiermee geeft u geen bestaande sjabloon. Het is bedoeld als een momentopname van de resourcegroep die u gebruiken kunt om opnieuw aan dezelfde resourcegroep te implementeren. Als u wilt de geëxporteerde sjabloon gebruiken voor andere oplossingen, moet u deze aanzienlijk wijzigen.

```powershell
Export-AzResourceGroup -ResourceGroupName ExampleGroup
```

Retourneert de locatie van de sjabloon.

```powershell
Path
----
C:\Users\exampleuser\ExampleGroup.json
```

Open het bestand en u ziet dat deze anders dan de sjabloon in GitHub is. Bevat verschillende parameters en geen variabelen. De opslag-SKU en de locatie zijn vastgelegde waarden. Het volgende voorbeeld ziet u de geëxporteerde sjabloon, maar uw sjabloon is een enigszins parameternaam:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageAccounts_nf3mvst4nqb36standardsa_name": {
      "defaultValue": null,
      "type": "String"
    }
  },
  "variables": {},
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "sku": {
        "name": "Standard_LRS",
        "tier": "Standard"
      },
      "kind": "Storage",
      "name": "[parameters('storageAccounts_nf3mvst4nqb36standardsa_name')]",
      "apiVersion": "2016-01-01",
      "location": "southcentralus",
      "tags": {},
      "properties": {},
      "dependsOn": []
    }
  ]
}
```

U kunt deze sjabloon opnieuw implementeren, maar hiervoor heeft een unieke naam voor het opslagaccount. De naam van de parameter is enigszins anders.

```powershell
New-AzResourceGroupDeployment -ResourceGroupName ExampleGroup `
  -TemplateFile C:\Users\exampleuser\ExampleGroup.json `
  -storageAccounts_nf3mvst4nqb36standardsa_name tfnewstorage0501
```

## <a name="customize-exported-template"></a>Geëxporteerde sjabloon aanpassen

U kunt deze sjabloon kunt u eenvoudiger te gebruiken en flexibeler kunt wijzigen. Als u wilt toestaan voor meer locaties, de locatie-eigenschap voor het gebruik van dezelfde locatie als de resourcegroep te wijzigen:

```json
"location": "[resourceGroup().location]",
```

Om te voorkomen dat een uniques-naam voor het opslagaccount te bedenken, verwijdert u de parameter voor de naam van het opslagaccount. Voeg een parameter voor een achtervoegsel van de naam van opslag en een opslag-SKU toe:

```json
"parameters": {
    "storageSuffix": {
      "type": "string",
      "defaultValue": "standardsa"
    },
    "storageAccountType": {
      "defaultValue": "Standard_LRS",
      "allowedValues": [
        "Standard_LRS",
        "Standard_GRS",
        "Standard_ZRS"
      ],
      "type": "string",
      "metadata": {
        "description": "Storage Account type"
      }
    }
},
```

Een variabele die de naam van het opslagaccount met de functie uniqueString construeert toevoegen:

```json
"variables": {
    "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'standardsa')]"
  },
```

Stel de naam van het opslagaccount dat aan de variabele:

```json
"name": "[variables('storageAccountName')]",
```

De SKU ingesteld op de parameter:

```json
"sku": {
    "name": "[parameters('storageAccountType')]",
    "tier": "Standard"
},
```

De sjabloon ziet er nu als volgt uit:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageSuffix": {
      "type": "string",
      "defaultValue": "standardsa"
    },
    "storageAccountType": {
      "defaultValue": "Standard_LRS",
      "allowedValues": [
        "Standard_LRS",
        "Standard_GRS",
        "Standard_ZRS"
      ],
      "type": "string",
      "metadata": {
        "description": "Storage Account type"
      }
    }
  },
  "variables": {
    "storageAccountName": "[concat(uniquestring(resourceGroup().id), parameters('storageSuffix'))]"
  },
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "sku": {
        "name": "[parameters('storageAccountType')]",
        "tier": "Standard"
      },
      "kind": "Storage",
      "name": "[variables('storageAccountName')]",
      "apiVersion": "2016-01-01",
      "location": "[resourceGroup().location]",
      "tags": {},
      "properties": {},
      "dependsOn": []
    }
  ]
}
```

De gewijzigde sjabloon opnieuw implementeert.

## <a name="next-steps"></a>Volgende stappen
* Zie voor meer informatie over het gebruik van de portal om een sjabloon te exporteren, [een Azure Resource Manager-sjabloon exporteren uit bestaande resources](resource-manager-export-template.md).
* Zie voor het definiëren van parameters in sjabloon, [-sjablonen maken](resource-group-authoring-templates.md#parameters).
* Zie voor tips over het oplossen van veelvoorkomende implementatiefouten [veelvoorkomende problemen oplossen Azure-implementatie met Azure Resource Manager](resource-manager-common-deployment-errors.md).
