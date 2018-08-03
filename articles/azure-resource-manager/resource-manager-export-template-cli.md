---
title: Exporteren van Resource Manager-sjabloon met Azure CLI | Microsoft Docs
description: Azure Resource Manager en Azure CLI gebruiken voor het exporteren van een sjabloon van een resourcegroep.
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
ms.openlocfilehash: d4a1a687700badc550d37bf74f6a7e1680388897
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39440312"
---
# <a name="export-azure-resource-manager-templates-with-azure-cli"></a>Exporteren van Azure Resource Manager-sjablonen met Azure CLI

Met Resource Manager kunt u een Resource Manager-sjabloon exporteren uit bestaande resources in uw abonnement. Deze gegenereerde sjabloon kunt u vervolgens gebruiken om meer te weten te komen over de sjabloonsyntaxis of om desgewenst het hergebruik van uw oplossing te automatiseren.

Het is belangrijk te weten dat er zijn twee verschillende manieren om een sjabloon te exporteren:

* U kunt exporteren de **daadwerkelijke sjabloon die wordt gebruikt voor een implementatie**. De geëxporteerde sjabloon bevat alle parameters en variabelen precies zoals ze worden weergegeven in de oorspronkelijke sjabloon. Deze methode is handig als u wilt ophalen van een sjabloon.
* U kunt een **gegenereerde sjabloon exporteren met de huidige status van de resourcegroep**. De geëxporteerde sjabloon is niet gebaseerd op een sjabloon die u voor de implementatie hebt gebruikt. In plaats daarvan wordt een sjabloon die een 'snapshot' of 'back-up van de resourcegroep is gemaakt. De geëxporteerde sjabloon heeft veel vastgelegde waarden en waarschijnlijk niet zoveel parameters als u doorgaans zou definiëren. Gebruik deze optie om resources aan dezelfde resourcegroep opnieuw te implementeren. Mogelijk moet u voor het gebruik van deze sjabloon voor een andere resourcegroep, aanzienlijk te wijzigen.

In dit artikel bevat beide methoden.

## <a name="deploy-a-solution"></a>Een oplossing implementeren

Ter illustratie van beide methoden voor het exporteren van een sjabloon, laten we beginnen met het implementeren van een oplossing aan uw abonnement. Als u al een resourcegroep in uw abonnement die u wilt exporteren hebt, hoeft u niet om deze oplossing te implementeren. Echter, de rest van dit artikel verwijst naar de sjabloon voor deze oplossing. Het voorbeeldscript implementeert u een opslagaccount.

```azurecli
az group create --name ExampleGroup --location "Central US"
az group deployment create \
    --name NewStorage \
    --resource-group ExampleGroup \
    --template-uri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json" \
```  

## <a name="save-template-from-deployment-history"></a>Sjabloon opslaan vanuit implementatiegeschiedenis

U kunt een sjabloon uit de implementatiegeschiedenis ophalen met behulp van de [az group deployment exporteren](/cli/azure/group/deployment#az-group-deployment-export) opdracht. Het volgende voorbeeld wordt de sjabloon die u eerder hebt geïmplementeerd:

```azurecli
az group deployment export --name NewStorage --resource-group ExampleGroup
```

Retourneert de sjabloon. De JSON kopiëren en opslaan als een bestand. Merk op dat deze de exacte sjabloon die u voor implementatie gebruikt. De parameters en variabelen overeenkomen met de sjabloon vanuit GitHub. U kunt deze sjabloon opnieuw implementeren.


## <a name="export-resource-group-as-template"></a>Resourcegroep exporteren als sjabloon

U kunt een sjabloon die staat voor de huidige status van een resourcegroep met behulp van ophalen in plaats van het ophalen van een sjabloon uit de implementatiegeschiedenis, de [az group exporteren](/cli/azure/group#az-group-export) opdracht. U kunt deze opdracht gebruiken wanneer u veel wijzigingen hebt aangebracht aan de resourcegroep en alle wijzigingen Hiermee geeft u geen bestaande sjabloon. Het is bedoeld als een momentopname van de resourcegroep die u gebruiken kunt om opnieuw aan dezelfde resourcegroep te implementeren. Als u wilt de geëxporteerde sjabloon gebruiken voor andere oplossingen, moet u deze aanzienlijk wijzigen.

```azurecli
az group export --name ExampleGroup
```

Retourneert de sjabloon. De JSON kopiëren en opslaan als een bestand. U ziet dat deze anders dan de sjabloon in GitHub is. De sjabloon bevat verschillende parameters en geen variabelen. De opslag-SKU en de locatie zijn vastgelegde waarden. Het volgende voorbeeld ziet u de geëxporteerde sjabloon, maar uw sjabloon is een enigszins parameternaam:

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

U kunt deze sjabloon opnieuw implementeren, maar hiervoor heeft een unieke naam voor het opslagaccount. De naam van de parameter is enigszins anders.

```azurecli
az group deployment create --name NewStorage --resource-group ExampleGroup \
  --template-file examplegroup.json \
  --parameters "{\"storageAccounts_mcyzaljiv7qncstandardsa_name\":{\"value\":\"tfstore0501\"}}"
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