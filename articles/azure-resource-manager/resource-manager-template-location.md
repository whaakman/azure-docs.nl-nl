---
title: Locatie van de Azure-resource in de sjabloon | Microsoft Docs
description: Laat zien hoe u een locatie voor een resource in een Azure Resource Manager-sjabloon is ingesteld
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: azure-resource-manager
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/03/2017
ms.author: tomfitz
ms.openlocfilehash: 73e50a593c41e841dcaf184abb895406ff5001e9
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="set-resource-location-in-azure-resource-manager-templates"></a>Locatie van de bron instellen in Azure Resource Manager-sjablonen
Bij het implementeren van een sjabloon, moet u een locatie voor elke resource opgeven. Dit onderwerp leest hoe u bepaalt de locaties die beschikbaar voor uw abonnement voor elk resourcetype zijn.

## <a name="determine-supported-locations"></a>Ondersteunde locaties bepalen

Zie voor een volledige lijst van ondersteunde locaties voor elk resourcetype [producten die beschikbaar zijn in elke regio](https://azure.microsoft.com/regions/services/). Echter, uw abonnement mogelijk geen toegang tot alle locaties in de lijst. Als een aangepaste lijst met locaties die beschikbaar voor uw abonnement zijn wilt weergeven, gebruikt u Azure PowerShell of Azure CLI. 

Het volgende voorbeeld maakt gebruik van PowerShell om op te halen van de locaties voor de `Microsoft.Web\sites` brontype:

```powershell
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).Locations
```

Het volgende voorbeeld maakt gebruik van Azure CLI 2.0 ophalen van de locaties voor de `Microsoft.Web\sites` brontype:

```azurecli
az provider show -n Microsoft.Web --query "resourceTypes[?resourceType=='sites'].locations"
```

## <a name="set-location-in-template"></a>Locatie instellen in sjabloon

Na het vaststellen van de ondersteunde locaties voor uw resources, moet u die locatie in uw sjabloon is ingesteld. De eenvoudigste manier om in te stellen van deze waarde is een resourcegroep maken op een locatie die ondersteuning biedt voor de volgende resourcetypen en elke locatie op instellen `[resourceGroup().location]`. U kunt de sjabloon aan resourcegroepen op verschillende locaties opnieuw implementeert en alle waarden in de sjabloon of parameters niet te wijzigen. 

Het volgende voorbeeld ziet u een opslagaccount dat is geïmplementeerd op dezelfde locatie als de resourcegroep:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "variables": {
      "storageName": "[concat('storage', uniqueString(resourceGroup().id))]"
    },
    "resources": [
    {
      "apiVersion": "2016-01-01",
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[variables('storageName')]",
      "location": "[resourceGroup().location]",
      "tags": {
        "Dept": "Finance",
        "Environment": "Production"
      },
      "sku": {
        "name": "Standard_LRS"
      },
      "kind": "Storage",
      "properties": { }
    }
    ]
}
```

Als u hardcode voor de locatie in de sjabloon wilt, geef de naam van een van de ondersteunde regio's. Het volgende voorbeeld ziet u een opslagaccount die altijd wordt geïmplementeerd op Noordelijk Centraal, VS:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
    {
      "apiVersion": "2016-01-01",
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[concat('storageloc', uniqueString(resourceGroup().id))]",
      "location": "North Central US",
      "tags": {
        "Dept": "Finance",
        "Environment": "Production"
      },
      "sku": {
        "name": "Standard_LRS"
      },
      "kind": "Storage",
      "properties": { }
    }
    ]
}
```

## <a name="next-steps"></a>Volgende stappen
* Zie voor aanbevelingen over het maken van sjablonen [aanbevolen procedures voor het maken van Azure Resource Manager-sjablonen](resource-manager-template-best-practices.md).

