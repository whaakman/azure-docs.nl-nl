---
title: Met Azure PowerShell Resource Manager-sjabloon exporteren | Microsoft Docs
description: Azure Resource Manager en Azure PowerShell gebruiken voor het exporteren van een sjabloon uit een resourcegroep.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/01/2017
ms.author: tomfitz
ms.openlocfilehash: 7543811eb9448222b6e7c266756e68debc7d54be
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="export-azure-resource-manager-templates-with-powershell"></a>Exporteren van Azure Resource Manager-sjablonen met PowerShell

Met Resource Manager kunt u een Resource Manager-sjabloon exporteren uit bestaande resources in uw abonnement. Deze gegenereerde sjabloon kunt u vervolgens gebruiken om meer te weten te komen over de sjabloonsyntaxis of om desgewenst het hergebruik van uw oplossing te automatiseren.

Het is belangrijk te weten dat er twee verschillende manieren zijn om een sjabloon te exporteren:

* U kunt de daadwerkelijke sjabloon dat u voor een implementatie hebt gebruikt, exporteren. De geëxporteerde sjabloon bevat alle parameters en variabelen precies zoals ze worden weergegeven in de oorspronkelijke sjabloon. Deze methode is handig als u nodig hebt voor het ophalen van een sjabloon.
* U kunt een sjabloon exporteren met de huidige status van de resourcegroep. De geëxporteerde sjabloon is niet gebaseerd op een sjabloon die u voor de implementatie hebt gebruikt. In plaats daarvan wordt er een sjabloon gemaakt die een momentopname is van de resourcegroep. De geëxporteerde sjabloon heeft veel vastgelegde waarden en waarschijnlijk niet zoveel parameters als u doorgaans zou definiëren. Deze methode is handig als u de resourcegroep hebt gewijzigd. U moet nu de resourcegroep vastleggen als sjabloon.

Dit onderwerp bevat beide methoden.

## <a name="deploy-a-solution"></a>Een oplossing implementeren

Ter illustratie van beide benaderingen voor het exporteren van een sjabloon, laten we beginnen met het implementeren van een oplossing voor uw abonnement. Als u al een resourcegroep in uw abonnement die u wilt exporteren, kunt u beschikt niet over deze oplossing implementeren. Echter, de rest van dit artikel verwijst naar de sjabloon voor deze oplossing. Het voorbeeldscript implementeert een opslagaccount.

```powershell
New-AzureRmResourceGroup -Name ExampleGroup -Location "South Central US"
New-AzureRmResourceGroupDeployment -ResourceGroupName ExampleGroup `
  -DeploymentName NewStorage
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json
```  

## <a name="save-template-from-deployment-history"></a>Sjabloon opslaan in de implementatiegeschiedenis

U kunt een sjabloon ophalen uit de implementatiegeschiedenis van een met behulp van de [opslaan AzureRmResourceGroupDeploymentTemplate](/powershell/module/azurerm.resources/save-azurermresourcegroupdeploymenttemplate) opdracht. Het volgende voorbeeld wordt de sjabloon die u eerder implementeert:

```powershell
Save-AzureRmResourceGroupDeploymentTemplate -ResourceGroupName ExampleGroup -DeploymentName NewStorage
```

Wordt de locatie van de sjabloon.

```powershell
Path
----
C:\Users\exampleuser\NewStorage.json
```

Open het bestand en u ziet dat het is de exacte sjabloon die u voor implementatie gebruikt. De parameters en variabelen overeen met de sjabloon vanuit GitHub. U kunt deze sjabloon opnieuw implementeren.

## <a name="export-resource-group-as-template"></a>Resourcegroep exporteren als sjabloon

In plaats van een sjabloon worden opgehaald uit de implementatiegeschiedenis van de, kunt u ophalen van een sjabloon die de huidige status van een resourcegroep met behulp van vertegenwoordigt de [Export-AzureRmResourceGroup](/powershell/module/azurerm.resources/export-azurermresourcegroup) opdracht. U kunt deze opdracht gebruiken wanneer u veel wijzigingen hebt aangebracht in de resourcegroep en er geen bestaande sjabloon alle wijzigingen vertegenwoordigt.

```powershell
Export-AzureRmResourceGroup -ResourceGroupName ExampleGroup
```

Wordt de locatie van de sjabloon.

```powershell
Path
----
C:\Users\exampleuser\ExampleGroup.json
```

Open het bestand en u ziet dat deze anders dan de sjabloon in GitHub is. Bestaat uit verschillende parameters en geen variabelen. De SKU-opslag en de locatie zijn vastgelegde waarden. Het volgende voorbeeld ziet u de geëxporteerde sjabloon, maar uw sjabloon is een iets andere parameternaam:

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

U kunt deze sjabloon kunt implementeren, maar het vereist raden een unieke naam voor het opslagaccount. De naam van de parameter is enigszins anders.

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName ExampleGroup `
  -TemplateFile C:\Users\exampleuser\ExampleGroup.json `
  -storageAccounts_nf3mvst4nqb36standardsa_name tfnewstorage0501
```

## <a name="customize-exported-template"></a>Geëxporteerde sjabloon aanpassen

U kunt deze sjabloon om deze te gebruiken gemakkelijker en flexibeler wijzigen. Als u wilt toestaan voor meer locaties, de locatie-eigenschap voor het gebruik van dezelfde locatie als de resourcegroep te wijzigen:

```json
"location": "[resourceGroup().location]",
```

Om te voorkomen dat te raden een uniques naam op voor storage-account, verwijdert u de parameter voor de opslagaccountnaam. Een parameter voor een achtervoegsel voor opslag en een opslag SKU toevoegen:

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

Een variabele die wordt gemaakt van de naam van het opslagaccount met de functie uniqueString toevoegen:

```json
"variables": {
    "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'standardsa')]"
  },
```

De naam van het storage-account aan de variabele instellen:

```json
"name": "[variables('storageAccountName')]",
```

De SKU op de parameter ingesteld:

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
* Zie voor meer informatie over het exporteren van een sjabloon met de portal [een Azure Resource Manager-sjabloon uit bestaande resources exporteren](resource-manager-export-template.md).
* Om parameters te definiëren in de sjabloon, Zie [sjablonen](resource-group-authoring-templates.md#parameters).
* Zie voor tips over het oplossen van algemene implementatiefouten [oplossen van veelvoorkomende fouten voor Azure-implementatie met Azure Resource Manager](resource-manager-common-deployment-errors.md).
