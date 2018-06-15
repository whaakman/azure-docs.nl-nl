---
title: Met Azure CLI Resource Manager-sjabloon exporteren | Microsoft Docs
description: Gebruik Azure Resource Manager en Azure CLI voor een sjabloon exporteren uit een resourcegroep.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: azurecli
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/23/2018
ms.author: tomfitz
ms.openlocfilehash: 1d73142931a5cfa84cb24df7a85c799a0f508385
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2018
ms.locfileid: "34358826"
---
# <a name="export-azure-resource-manager-templates-with-azure-cli"></a>Azure Resource Manager-sjablonen met Azure CLI exporteren

Met Resource Manager kunt u een Resource Manager-sjabloon exporteren uit bestaande resources in uw abonnement. Deze gegenereerde sjabloon kunt u vervolgens gebruiken om meer te weten te komen over de sjabloonsyntaxis of om desgewenst het hergebruik van uw oplossing te automatiseren.

Het is belangrijk te weten dat er zijn twee verschillende manieren om een sjabloon te exporteren:

* U kunt exporteren de **werkelijke sjabloon die wordt gebruikt voor een implementatie**. De geëxporteerde sjabloon bevat alle parameters en variabelen precies zoals ze worden weergegeven in de oorspronkelijke sjabloon. Deze methode is handig als u nodig hebt voor het ophalen van een sjabloon.
* U kunt een **gegenereerde sjabloon exporteren met de huidige status van de resourcegroep**. De geëxporteerde sjabloon is niet gebaseerd op een sjabloon die u voor de implementatie hebt gebruikt. In plaats daarvan wordt een sjabloon die een 'snapshot' of 'back-up' van de resourcegroep wordt gemaakt. De geëxporteerde sjabloon heeft veel vastgelegde waarden en waarschijnlijk niet zoveel parameters als u doorgaans zou definiëren. Gebruik deze optie om het implementeren van resources op de dezelfde resourcegroep. Als u wilt deze sjabloon voor een andere resourcegroep gebruikt, moet u wellicht aanzienlijk wijzigen.

Dit artikel ziet beide benaderingen.

## <a name="deploy-a-solution"></a>Een oplossing implementeren

Ter illustratie van beide benaderingen voor het exporteren van een sjabloon, laten we beginnen met het implementeren van een oplossing voor uw abonnement. Als u al een resourcegroep in uw abonnement die u wilt exporteren, kunt u beschikt niet over deze oplossing implementeren. Echter, de rest van dit artikel verwijst naar de sjabloon voor deze oplossing. Het voorbeeldscript implementeert een opslagaccount.

```azurecli
az group create --name ExampleGroup --location "Central US"
az group deployment create \
    --name NewStorage \
    --resource-group ExampleGroup \
    --template-uri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json" \
```  

## <a name="save-template-from-deployment-history"></a>Sjabloon opslaan in de implementatiegeschiedenis

U kunt een sjabloon ophalen uit de implementatiegeschiedenis van een met behulp van de [az implementatie exporteren](/cli/azure/group/deployment#az_group_deployment_export) opdracht. Het volgende voorbeeld wordt de sjabloon die u eerder implementeert:

```azurecli
az group deployment export --name NewStorage --resource-group ExampleGroup
```

Retourneert de sjabloon. De JSON kopiëren en opslaan als een bestand. U ziet dat deze de exacte sjabloon die u voor implementatie gebruikt. De parameters en variabelen overeen met de sjabloon vanuit GitHub. U kunt deze sjabloon opnieuw implementeren.


## <a name="export-resource-group-as-template"></a>Resourcegroep exporteren als sjabloon

In plaats van een sjabloon worden opgehaald uit de implementatiegeschiedenis van de, kunt u ophalen van een sjabloon die de huidige status van een resourcegroep met behulp van vertegenwoordigt de [exporteren az](/cli/azure/group#az_group_export) opdracht. U kunt deze opdracht gebruiken wanneer u veel wijzigingen hebt aangebracht in de resourcegroep en er geen bestaande sjabloon alle wijzigingen vertegenwoordigt. Het is bedoeld als een momentopname van de resourcegroep waarin u kunt implementeren naar dezelfde resourcegroep. Voor het gebruik van de geëxporteerde sjabloon voor andere oplossingen, moet u deze aanzienlijk wijzigen.

```azurecli
az group export --name ExampleGroup
```

Retourneert de sjabloon. De JSON kopiëren en opslaan als een bestand. U ziet dat deze anders dan de sjabloon in GitHub is. De sjabloon heeft verschillende parameters en geen variabelen. De SKU-opslag en de locatie zijn vastgelegde waarden. Het volgende voorbeeld ziet u de geëxporteerde sjabloon, maar uw sjabloon is een iets andere parameternaam:

```json
{
  "parameters": {
    "storageAccounts_mcyzaljiv7qncstandardsa_name": {
      "type": "String",
      "defaultValue": null
    }
  },
  "variables": {},
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "resources": [
    {
      "tags": {},
      "dependsOn": [],
      "apiVersion": "2016-01-01",
      "sku": {
        "name": "Standard_LRS",
        "tier": "Standard"
      },
      "kind": "Storage",
      "type": "Microsoft.Storage/storageAccounts",
      "location": "centralus",
      "name": "[parameters('storageAccounts_mcyzaljiv7qncstandardsa_name')]",
      "properties": {}
    }
  ],
  "contentVersion": "1.0.0.0"
}
```

U kunt deze sjabloon kunt implementeren, maar het vereist raden een unieke naam voor het opslagaccount. De naam van de parameter is enigszins anders.

```azurecli
az group deployment create --name NewStorage --resource-group ExampleGroup \
  --template-file examplegroup.json \
  --parameters "{\"storageAccounts_mcyzaljiv7qncstandardsa_name\":{\"value\":\"tfstore0501\"}}"
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