---
title: Eerste Azure Resource Manager-sjabloon maken | Microsoft Docs
description: Een stapsgewijze handleiding voor het maken van uw eerste Azure Resource Manager-sjabloon. U leert hoe u de sjabloonverwijzing voor een opslagaccount gebruikt om een sjabloon te maken.
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 10/02/2017
ms.topic: get-started-article
ms.author: tomfitz
ms.openlocfilehash: 7d20469aaf2dfdd7a5f3650983b59152de837837
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-and-deploy-your-first-azure-resource-manager-template"></a>Uw eerste Azure Resource Manager-sjabloon maken en implementeren
In dit onderwerp worden de stappen beschreven voor het maken van uw eerste Azure Resource Manager-sjabloon. Resource Manager-sjablonen zijn JSON-bestanden die de resources definiëren die u voor uw oplossing moet implementeren. Zie [Overzicht van Azure Resource Manager](resource-group-overview.md) voor inzicht in de concepten die gerelateerd zijn aan het implementeren en beheren van uw Azure-oplossingen. Zie [Een Azure Resource Manager-sjabloon uit bestaande resources exporteren](resource-manager-export-template.md) als u een sjabloon voor bestaande resources wilt maken.

U hebt een JSON-editor nodig om sjablonen te maken en reviseren. [Visual Studio Code](https://code.visualstudio.com/) is een lichte, open-source, platformoverschrijdende code-editor. U wordt sterk aangeraden om Visual Studio Code te gebruiken voor het maken van Resource Manager-sjablonen. In dit artikel wordt ervan uitgegaan dat u VS Code gebruikt. Als u een andere JSON-editor (zoals Visual Studio) hebt, kunt u die editor gebruiken.

## <a name="prerequisites"></a>Vereisten

* Visual Studio Code. U kunt VS Code, indien nodig, installeren via [https://code.visualstudio.com/](https://code.visualstudio.com/).
* Een Azure-abonnement. Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="create-template"></a>Sjabloon maken

Laten we beginnen met een eenvoudige sjabloon waarmee een opslagaccount in uw abonnement wordt geïmplementeerd.

1. Selecteer **Bestand** > **Nieuw bestand**. 

   ![Nieuw bestand](./media/resource-manager-create-first-template/new-file.png)

2. Kopieer en plak de volgende JSON-syntaxis in het bestand:

   ```json
   {
     "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
     "contentVersion": "1.0.0.0",
     "parameters": {
     },
     "variables": {
     },
     "resources": [
       {
         "name": "[concat('storage', uniqueString(resourceGroup().id))]",
         "type": "Microsoft.Storage/storageAccounts",
         "apiVersion": "2016-01-01",
         "sku": {
           "name": "Standard_LRS"
         },
         "kind": "Storage",
         "location": "South Central US",
         "tags": {},
         "properties": {}
       }
     ],
     "outputs": {  }
   }
   ```

   Er gelden enkele beperkingen voor opslagaccountnamen waardoor ze moeilijk in te stellen zijn. De naam moet tussen de 3 en 24 tekens lang zijn, alleen cijfers en kleine letters bevatten en uniek zijn. In de voorgaande sjabloon wordt de functie [uniqueString](resource-group-template-functions-string.md#uniquestring) gebruikt om een hashwaarde te genereren. Het voorvoegsel *opslag* wordt toegevoegd om deze hashwaarde meer betekenis te geven. 

3. Sla dit bestand als **azuredeploy.json** op in een lokale map.

   ![Sjabloon opslaan](./media/resource-manager-create-first-template/save-template.png)

## <a name="deploy-template"></a>Sjabloon implementeren

U kunt deze sjabloon nu implementeren. U kunt PowerShell of Azure CLI gebruiken om een resourcegroep te maken. Vervolgens implementeert u een opslagaccount in deze resourcegroep.

* Gebruik voor PowerShell de volgende opdrachten uit de map met de sjabloon:

   ```powershell
   Login-AzureRmAccount
   
   New-AzureRmResourceGroup -Name examplegroup -Location "South Central US"
   New-AzureRmResourceGroupDeployment -ResourceGroupName examplegroup -TemplateFile azuredeploy.json
   ```

* Gebruik voor een lokale installatie van Azure CLI de volgende opdrachten uit de map met de sjabloon:

   ```azurecli
   az login

   az group create --name examplegroup --location "South Central US"
   az group deployment create --resource-group examplegroup --template-file azuredeploy.json
   ```

Wanneer de implementatie is voltooid, bevindt het opslagaccount zich in de resourcegroep.

[!INCLUDE [resource-manager-cloud-shell-deploy.md](../../includes/resource-manager-cloud-shell-deploy.md)]

Gebruik de volgende opdrachten voor Azure CLI:

```azurecli-interactive
az group create --name examplegroup --location "South Central US"
az group deployment create --resource-group examplegroup --template-file clouddrive/templates/azuredeploy.json
```

PowerShell is momenteel als voorbeeld beschikbaar in Cloud Shell. Gebruik de volgende opdrachten voor PowerShell:

```powershell
New-AzureRmResourceGroup -Name examplegroup -Location "South Central US"
New-AzureRmResourceGroupDeployment -ResourceGroupName examplegroup -TemplateFile $home\CloudDrive\templates\azuredeploy.json
```

Wanneer de implementatie is voltooid, bevindt het opslagaccount zich in de resourcegroep.

## <a name="customize-the-template"></a>De sjabloon aanpassen

De sjabloon werkt goed, maar is niet flexibel. Deze implementeert altijd een lokaal redundante opslag in Zuid-centraal VS. De naam is altijd *Opslag* gevolgd door een hashwaarde. Voeg parameters toe aan de sjabloon om ervoor te zorgen dat u deze voor verschillende scenario's kunt gebruiken.

In het volgende voorbeeld wordt de sectie Parameters weergegeven met twee parameters. Met de eerste parameter `storageSKU` kunt u het type redundantie opgeven. Hiermee beperkt u de waarden die kunnen worden doorgegeven, tot waarden die geldig zijn voor een opslagaccount. Er wordt ook een standaardwaarde opgegeven. De tweede parameter `storageNamePrefix` is ingesteld om een maximum aantal van 11 tekens toe te staan. Hiermee wordt een standaardwaarde opgegeven.

```json
"parameters": {
  "storageSKU": {
    "type": "string",
    "allowedValues": [
      "Standard_LRS",
      "Standard_ZRS",
      "Standard_GRS",
      "Standard_RAGRS",
      "Premium_LRS"
    ],
    "defaultValue": "Standard_LRS",
    "metadata": {
      "description": "The type of replication to use for the storage account."
    }
  },
  "storageNamePrefix": {
    "type": "string",
    "maxLength": 11,
    "defaultValue": "storage",
    "metadata": {
      "description": "The value to use for starting the storage account name. Use only lowercase letters and numbers."
    }
  }
},
```

Voeg in de sectie Variabelen een variabele in met de naam `storageName`. Deze combineert de voorvoegselwaarde van de parameters met een hashwaarde van de functie [uniqueString](resource-group-template-functions-string.md#uniquestring). Deze maakt gebruik van de functie [toLower](resource-group-template-functions-string.md#tolower) om alle tekens te converteren naar kleine letters.

```json
"variables": {
  "storageName": "[concat(toLower(parameters('storageNamePrefix')), uniqueString(resourceGroup().id))]"
},
```

Als u deze nieuwe waarden wilt gebruiken voor uw opslagaccount, wijzigt u de resourcedefinitie:

```json
"resources": [
  {
    "name": "[variables('storageName')]",
    "type": "Microsoft.Storage/storageAccounts",
    "apiVersion": "2016-01-01",
    "sku": {
      "name": "[parameters('storageSKU')]"
    },
    "kind": "Storage",
    "location": "[resourceGroup().location]",
    "tags": {},
    "properties": {}
  }
],
```

U ziet nu dat de naam van het opslagaccount is ingesteld op de variabele die u hebt toegevoegd. De naam van de SKU is ingesteld op de waarde van de parameter. De locatie is ingesteld op dezelfde locatie als de resourcegroep.

Sla het bestand op. 

De sjabloon ziet er nu als volgt uit:

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageSKU": {
      "type": "string",
      "allowedValues": [
        "Standard_LRS",
        "Standard_ZRS",
        "Standard_GRS",
        "Standard_RAGRS",
        "Premium_LRS"
      ],
      "defaultValue": "Standard_LRS",
      "metadata": {
        "description": "The type of replication to use for the storage account."
      }
    },   
    "storageNamePrefix": {
      "type": "string",
      "maxLength": 11,
      "defaultValue": "storage",
      "metadata": {
        "description": "The value to use for starting the storage account name. Use only lowercase letters and numbers."
      }
    }
  },
  "variables": {
    "storageName": "[concat(toLower(parameters('storageNamePrefix')), uniqueString(resourceGroup().id))]"
  },
  "resources": [
    {
      "name": "[variables('storageName')]",
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2016-01-01",
      "sku": {
        "name": "[parameters('storageSKU')]"
      },
      "kind": "Storage",
      "location": "[resourceGroup().location]",
      "tags": {},
      "properties": {}
    }
  ],
  "outputs": {  }
}
```

## <a name="redeploy-template"></a>Sjabloon opnieuw implementeren

Implementeer de sjabloon opnieuw met verschillende waarden.

Gebruik voor PowerShell:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName examplegroup -TemplateFile azuredeploy.json -storageNamePrefix newstore -storageSKU Standard_RAGRS
```

Gebruik voor Azure CLI:

```azurecli
az group deployment create --resource-group examplegroup --template-file azuredeploy.json --parameters storageSKU=Standard_RAGRS storageNamePrefix=newstore
```

Upload voor Cloud Shell de gewijzigde sjabloon naar de bestandsshare. Overschrijf het bestaande bestand. Gebruik vervolgens deze opdracht:

```azurecli
az group deployment create --resource-group examplegroup --template-file clouddrive/templates/azuredeploy.json --parameters storageSKU=Standard_RAGRS storageNamePrefix=newstore
```

## <a name="use-autocomplete"></a>Automatisch aanvullen gebruiken

Uw werk op de sjabloon bestond tot nu toe alleen uit het kopiëren en plakken van JSON uit dit artikel. Bij het ontwikkelen van uw eigen sjablonen moet u echter eigenschappen en waarden zoeken en opgeven die beschikbaar zijn voor het resourcetype. VS Code leest het schema voor het resourcetype en stelt eigenschappen en waarden voor. Om te functie voor automatisch aanvullen te bekijken, gaat u naar het eigenschapselement van uw sjabloon en voegt u een nieuwe regel toe. Typ een aanhalingsteken. VS Code stelt onmiddellijk namen voor die beschikbaar zijn in het eigenschapselement.

![Beschikbare eigenschappen weergeven](./media/resource-manager-create-first-template/show-properties.png)

Selecteer **Versleuteling**. Typ een dubbele punt (:). VS Code stelt voor om een nieuw object toe te voegen.

![Object toevoegen](./media/resource-manager-create-first-template/add-object.png)

Druk op tab of enter als u het object wilt toevoegen.

Typ opnieuw een aanhalingsteken. VS Code stelt u eigenschappen voor die beschikbaar zijn voor versleuteling.

![Versleutelingseigenschappen weergeven](./media/resource-manager-create-first-template/show-encryption-properties.png)

Selecteer **services** en ga verder met het toevoegen van waarden op basis van VS Code-extensies totdat u het volgende hebt:

```json
"properties": {
    "encryption":{
        "services":{
            "blob":{
              "enabled":true
            }
        }
    }
}
```

U hebt blob-versleuteling voor het opslagaccount ingeschakeld. VS-Code heeft echter een probleem aangetroffen. U ziet dat versleuteling een waarschuwing heeft.

![Waarschuwing voor versleuteling](./media/resource-manager-create-first-template/encryption-warning.png)

Beweeg de muisaanwijzer over de groene lijn om de waarschuwing te bekijken.

![Ontbrekende eigenschap](./media/resource-manager-create-first-template/missing-property.png)

U ziet dat het versleutelingselement een keySource-eigenschap vereist. Voeg een komma toe na het serviceobject voeg de keySource-eigenschap toe. VS Code stelt **'Microsoft.Storage'** voor als een geldige waarde. Wanneer u klaar bent, is het eigenschapselement:

```json
"properties": {
    "encryption":{
        "services":{
            "blob":{
              "enabled":true
            }
        },
        "keySource":"Microsoft.Storage"
    }
}
```

De definitieve sjabloon is:

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageSKU": {
      "type": "string",
      "allowedValues": [
        "Standard_LRS",
        "Standard_ZRS",
        "Standard_GRS",
        "Standard_RAGRS",
        "Premium_LRS"
      ],
      "defaultValue": "Standard_LRS",
      "metadata": {
        "description": "The type of replication to use for the storage account."
      }
    },   
    "storageNamePrefix": {
      "type": "string",
      "maxLength": 11,
      "defaultValue": "storage",
      "metadata": {
        "description": "The value to use for starting the storage account name. Use only lowercase letters and numbers."
      }
    }
  },
  "variables": {
    "storageName": "[concat(toLower(parameters('storageNamePrefix')), uniqueString(resourceGroup().id))]"
  },
  "resources": [
    {
      "name": "[variables('storageName')]",
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2016-01-01",
      "sku": {
        "name": "[parameters('storageSKU')]"
      },
      "kind": "Storage",
      "location": "[resourceGroup().location]",
      "tags": {},
      "properties": {
        "encryption":{
          "services":{
            "blob":{
              "enabled":true
            }
          },
          "keySource":"Microsoft.Storage"
        }
      }
    }
  ],
  "outputs": {}
}
```

## <a name="deploy-encrypted-storage"></a>Versleutelde opslag implementeren

Implementeren de sjabloon opnieuw en geef de naam van een nieuw opslagaccount op.

Gebruik voor PowerShell:

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName examplegroup -TemplateFile azuredeploy.json -storageNamePrefix storesecure
```

Gebruik voor Azure CLI:

```azurecli
az group deployment create --resource-group examplegroup --template-file azuredeploy.json --parameters storageNamePrefix=storesecure
```

Upload voor Cloud Shell de gewijzigde sjabloon naar de bestandsshare. Overschrijf het bestaande bestand. Gebruik vervolgens deze opdracht:

```azurecli
az group deployment create --resource-group examplegroup --template-file clouddrive/templates/azuredeploy.json --parameters storageNamePrefix=storesecure
```

## <a name="clean-up-resources"></a>Resources opschonen

Schoon de geïmplementeerd resources, wanneer u deze niet meer nodig hebt, op door de resourcegroep te verwijderen.

Gebruik voor PowerShell:

```powershell
Remove-AzureRmResourceGroup -Name examplegroup
```

Gebruik voor Azure CLI:

```azurecli
az group delete --name examplegroup
```

## <a name="next-steps"></a>Volgende stappen
* Als u meer hulp nodig hebt bij het ontwikkelen van sjablonen, kunt u een VS Code-extensie installeren. Zie voor meer informatie [Visual Studio Code-extensie gebruiken om Azure Resource Manager-sjablonen te maken](resource-manager-vscode-extension.md)
* Zie [Azure Resource Manager-sjablonen samenstellen](resource-group-authoring-templates.md) voor meer informatie over de structuur van een sjabloon.
* Zie [Storage accounts template reference](/azure/templates/microsoft.storage/storageaccounts) (Sjabloonverwijzing voor opslagaccounts) voor meer informatie over de eigenschappen van een opslagaccount.
* Zie de [Azure-snelstartsjablonen](https://azure.microsoft.com/documentation/templates/) voor volledige sjablonen voor verschillende soorten oplossingen.
