---
title: Eerste Azure Resource Manager-sjabloon maken | Microsoft Docs
description: Een stapsgewijze handleiding voor het maken van uw eerste Azure Resource Manager-sjabloon. U leert hoe u de sjabloonverwijzing voor een opslagaccount gebruikt om een sjabloon te maken.
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 03/21/2017
ms.topic: get-started-article
ms.author: tomfitz
translationtype: Human Translation
ms.sourcegitcommit: 1429bf0d06843da4743bd299e65ed2e818be199d
ms.openlocfilehash: f861f2ba0376f00ff21e03cbb81a04a266f38630
ms.lasthandoff: 03/22/2017

---

# <a name="create-your-first-azure-resource-manager-template"></a>Uw eerste Azure Resource Manager-sjabloon maken
In dit onderwerp worden de stappen beschreven voor het maken van uw eerste Azure Resource Manager-sjabloon. Resource Manager-sjablonen zijn JSON-bestanden die de resources definiëren die u voor uw oplossing moet implementeren. Zie [Overzicht van Azure Resource Manager](resource-group-overview.md) voor inzicht in de concepten die gerelateerd zijn aan het implementeren en beheren van uw Azure-oplossingen. Zie [Een Azure Resource Manager-sjabloon uit bestaande resources exporteren](resource-manager-export-template.md) als u een sjabloon voor bestaande resources wilt maken.

U hebt een JSON-editor nodig om sjablonen te maken en reviseren. [Visual Studio Code](https://code.visualstudio.com/) is een lichte, open-source, platformoverschrijdende code-editor. Het programma ondersteunt het maken en bewerken van Resource Manager-sjablonen via een extensie. In dit onderwerp wordt ervan uitgegaan dat u VS Code gebruikt. Als u een andere JSON-editor (zoals Visual Studio) hebt, kunt u die editor gebruiken.

## <a name="get-vs-code-and-extension"></a>VS Code en extensie ophalen
1. U kunt VS Code installeren via [https://code.visualstudio.com/](https://code.visualstudio.com/).

2. Installeer de extensie voor [Azure Resource Manager-hulpprogramma's](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools) door Snel openen te starten (Ctrl + P) en het volgende uit te voeren: 

   ```
   ext install msazurermtools.azurerm-vscode-tools
   ```

3. Start VS Code opnieuw op wanneer u daarom wordt gevraagd om de extensie in te schakelen.

## <a name="create-blank-template"></a>Lege sjabloon maken

Laten we beginnen met een lege sjabloon die alleen de basisgedeelten van een sjabloon bevat.

1. Maak een bestand. 

2. Kopieer en plak de volgende JSON-syntaxis in het bestand:

   ```json
   {
     "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
     "contentVersion": "1.0.0.0",
     "parameters": {  },
     "variables": {  },
     "resources": [  ],
     "outputs": {  }
   }
   ```

3. Sla dit bestand op als **azuredeploy.json**. 

## <a name="add-storage-account"></a>Opslagaccount toevoegen
1. Voeg het opslagaccount toe in het gedeelte **Resources** van de sjabloon om het opslagaccount te definiëren voor implementatie. Kijk naar de [sjabloonverwijzing voor opslagaccounts](/azure/templates/microsoft.storage/storageaccounts) om de waarden te vinden die voor het opslagaccount beschikbaar zijn. Kopieer de JSON die wordt weergegeven voor het opslagaccount. 

3. Plak de JSON in het gedeelte **Resources** van uw sjabloon, zoals wordt weergegeven in het volgende voorbeeld: 

   ```json
   {
     "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
     "contentVersion": "1.0.0.0",
     "parameters": {  },
     "variables": {  },
     "resources": [
       {
         "name": "string",
         "type": "Microsoft.Storage/storageAccounts",
         "apiVersion": "2016-05-01",
         "sku": {
           "name": "string"
         },
         "kind": "string",
         "location": "string",
         "tags": {},
         "properties": {
           "customDomain": {
             "name": "string",
             "useSubDomain": boolean
           },
           "encryption": {
             "services": {
               "blob": {
                 "enabled": boolean
               }
             },
             "keySource": "Microsoft.Storage"
           },
           "accessTier": "string"
         }
       }
     ],
     "outputs": {  }
   }
   ```

  Het vorige voorbeeld bevat veel plaatsaanduidingswaarden en een aantal eigenschappen die u mogelijk niet nodig hebt in uw opslagaccount.

## <a name="set-values-for-storage-account"></a>Waarden voor opslagaccount instellen

U kunt nu waarden voor uw opslagaccount instellen. 

1. Bekijk nogmaals de [sjabloonverwijzing voor opslagaccounts](/azure/templates/microsoft.storage/storageaccounts) waarnaar u de JSON hebt gekopieerd. Er zijn meerdere tabellen die de eigenschappen beschrijven en beschikbare waarden bieden. 

2. Binnen het element **properties** worden **customDomain**, **encryption** en **accessTier** alle vermeld als niet vereist. Deze waarden kunnen belangrijk zijn voor uw scenario's, maar we verwijderen ze hier om dit voorbeeld eenvoudig te houden.

   ```json
   "resources": [
     {
       "name": "string",
       "type": "Microsoft.Storage/storageAccounts",
       "apiVersion": "2016-05-01",
       "sku": {
         "name": "string"
       },
       "kind": "string",
       "location": "string",
       "tags": {},
       "properties": {
       }
     }
   ],
   ```

3. Op dit moment is het element **kind** ingesteld op een plaatsaanduidingswaarde ("tekenreeks"). VS Code bevat veel functies die u inzicht geven in welke waarden u moet gebruiken in uw sjabloon. VS Code geeft aan dat deze waarde niet geldig is. Als u de muisaanwijzer op "tekenreeks" houdt, suggereert VS Code dat de geldige waarden voor **kind** `Storage` of `BlobStorage` zijn. 

   ![Gesuggereerde waarden van VS Code weergeven](./media/resource-manager-create-first-template/vs-code-show-values.png)

   Verwijder de tekens tussen de dubbele aanhalingstekens en selecteer **Ctrl + spatie** om de beschikbare waarden te zien. Selecteer **Opslag** uit de beschikbare opties.
  
   ![IntelliSense weergeven](./media/resource-manager-create-first-template/intellisense.png)

   Als u niet VS Code gebruikt, kijkt u op de pagina met sjabloonverwijzing voor opslagaccounts. In de beschrijving worden dezelfde geldige waarden weergegeven. Stel het element in op **Opslag**.

   ```json
   "kind": "Storage",
   ```

De sjabloon ziet er nu als volgt uit:

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {  },
  "variables": {  },
  "resources": [
    {
      "name": "string",
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2016-05-01",
      "sku": {
        "name": "string"
      },
      "kind": "Storage",
      "location": "string",
      "tags": {},
      "properties": {
      }
    }
  ],
  "outputs": {  }
}
```

## <a name="add-template-function"></a>Sjabloonfunctie toevoegen

U kunt functies gebruiken in uw sjabloon om de syntaxis van de sjabloon te vereenvoudigen en om waarden op te halen die alleen beschikbaar zijn wanneer de sjabloon wordt geïmplementeerd. Zie [Azure Resource Manager-sjabloonfuncties](resource-group-template-functions.md) voor een volledig overzicht van de sjabloonfuncties.

Als u wilt opgeven dat het opslagaccount naar dezelfde locatie als de resourcegroep wordt geïmplementeerd, stelt u de eigenschap **location** in op:

```json
"location": "[resourceGroup().location]",
```

VS Code helpt u opnieuw door beschikbare functies voor te stellen. 

![Functies weergeven](./media/resource-manager-create-first-template/show-functions.png)

De functie staat tussen vierkante haken. De functie [resourceGroup](resource-group-template-functions.md#resourcegroup) retourneert een object met een eigenschap genaamd `location`. De resourcegroep bevat alle verwante resources voor uw oplossing. U kunt de locatie-eigenschap hardcoden op een waarde zoals 'VS - midden', maar dan moet u handmatig de sjabloon wijzigen zodat deze opnieuw wordt geïmplementeerd naar een andere locatie. Met de functie `resourceGroup` kunt u de sjabloon gemakkelijk opnieuw implementeren naar een andere resourcegroep op een andere locatie.

De sjabloon ziet er nu als volgt uit:

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {  },
  "variables": {  },
  "resources": [
    {
      "name": "string",
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2016-05-01",
      "sku": {
        "name": "string"
      },
      "kind": "Storage",
      "location": "[resourceGroup().location]",
      "tags": {},
      "properties": {
      }
    }
  ],
  "outputs": {  }
}
```

## <a name="add-parameters-and-variables"></a>Parameters en variabelen toevoegen
U hoeft nog maar twee waarden in te stellen in uw sjabloon: **name** en **sku.name**. Voor deze eigenschappen voegt u parameters toe waarmee u deze waarden kunt aanpassen tijdens de implementatie. 

Er gelden enkele beperkingen voor opslagaccountnamen waardoor ze moeilijk in te stellen zijn. De naam moet tussen de 3 en 24 tekens lang zijn, alleen cijfers en kleine letters bevatten en uniek zijn. In plaats van te proberen een unieke waarde te vinden die aan de vereisten voldoet, kunt u de functie [uniqueString](resource-group-template-functions.md#uniquestring) gebruiken om een hash-waarde te genereren. Om deze hashwaarde betekenisvoller te maken, voegt u een voorvoegsel toe waarmee u het als een opslagaccount kunt identificeren na de implementatie. 

1. Ga naar het gedeelte **Parameters** van de sjabloon om een voorvoegsel door te geven voor de naam die overeenkomt met uw naamgevingsregels. Voeg een parameter toe aan de sjabloon die een voorvoegsel voor de opslagaccountnaam accepteert:

   ```json
   "parameters": {
     "storageNamePrefix": {
       "type": "string",
       "maxLength": 11,
       "defaultValue": "storage",
       "metadata": {
         "description": "The value to use for starting the storage account name."
       }
     }
   },
   ```

  Het voorvoegsel mag maximaal 11 tekens zijn omdat `uniqueString` 13 tekens retourneert. De naam mag niet langer dan 24 tekens zijn. Als u geen waarde doorvoert voor de parameter tijdens de implementatie, wordt de standaardwaarde gebruikt.

2. Ga naar het gedeelte **Variabelen** van de sjabloon. Voeg de volgende variabele toe om de naam uit het voorvoegsel en de unieke tekenreeks te maken:

   ```json
   "variables": {
     "storageName": "[concat(parameters('storageNamePrefix'), uniqueString(resourceGroup().id))]"
   },
   ```

3. Stel in het gedeelte **Resources** de opslagaccountnaam in op die variabele.

   ```json
   "name": "[variables('storageName')]",
   ```

3. Ga naar het gedeelte **Parameters** om doorvoer in verschillende SKU's voor het opslagaccount in te schakelen. Voeg na de parameter voor het voorvoegsel voor de opslagnaam een parameter toe waarmee de toegestane SKU-waarden en een standaardwaarde worden gespecificeerd. U vindt de toegestane waarden op de pagina voor de sjabloonverwijzing of in VS Code. In het volgende voorbeeld gebruikt u alle geldige waarden voor de SKU. U kunt de toegestane waarden echter beperken tot alleen de soorten SKU's die u wilt implementeren middels deze sjabloon.

   ```json
   "parameters": {
     "storageNamePrefix": {
       "type": "string",
       "maxLength": 11,
       "defaultValue": "storage",
       "metadata": {
         "description": "The value to use for starting the storage account name."
       }
     },
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
     }
   },
   ```

3. Wijzig de SKU-eigenschap zodat deze de waarde van de parameter gebruikt:

   ```json
   "sku": {
     "name": "[parameters('storageSKU')]"
   },
   ```    

4. Sla het bestand op.

De sjabloon ziet er nu als volgt uit:

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageNamePrefix": {
      "type": "string",
      "maxLength": 11,
      "defaultValue": "storage",
      "metadata": {
        "description": "The value to use for starting the storage account name."
      }
    },
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
    }
  },
  "variables": {
    "storageName": "[concat(parameters('storageNamePrefix'), uniqueString(resourceGroup().id))]"
  },
  "resources": [
    {
      "name": "[variables('storageName')]",
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2016-05-01",
      "sku": {
        "name": "[parameters('storageSKU')]"
      },
      "kind": "Storage",
      "location": "[resourceGroup().location]",
      "tags": {},
      "properties": {
      }
    }
  ],
  "outputs": {  }
}
```

## <a name="next-steps"></a>Volgende stappen
* De sjabloon is voltooid en u kunt deze nu implementeren op uw abonnement. Zie [Resources implementeren op Azure](resource-manager-quickstart-deploy.md) voor de implementatie.
* Zie [Azure Resource Manager-sjablonen samenstellen](resource-group-authoring-templates.md) voor meer informatie over de structuur van een sjabloon.
* Zie de [Azure-snelstartsjablonen](https://azure.microsoft.com/documentation/templates/) voor volledige sjablonen voor verschillende soorten oplossingen.

