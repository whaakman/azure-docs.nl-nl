---
title: Een Azure-Cache in te richten voor Redis met Azure Resource Manager | Microsoft Docs
description: Gebruik Azure Resource Manager-sjabloon voor het implementeren van een Azure-Cache voor Redis.
services: app-service
documentationcenter: ''
author: yegu-ms
manager: jhubbard
editor: ''
ms.assetid: ce6f5372-7038-4655-b1c5-108f7c148282
ms.service: cache
ms.workload: web
ms.tgt_pltfrm: cache
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: yegu
ms.openlocfilehash: e223cb060857d45d9f25e2ee1dfca7e159225d8b
ms.sourcegitcommit: de81b3fe220562a25c1aa74ff3aa9bdc214ddd65
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56237107"
---
# <a name="create-an-azure-cache-for-redis-using-a-template"></a>Een Azure-Cache voor Redis met behulp van een sjabloon maken

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

In dit onderwerp leert u over het maken van een Azure Resource Manager-sjabloon die een Azure-Cache voor Redis implementeert. De cache kan worden gebruikt met een bestaand opslagaccount voor diagnostische gegevens. U leert ook hoe om te definiëren welke resources worden geïmplementeerd en over het definiëren van parameters die zijn opgegeven wanneer de implementatie wordt uitgevoerd. U kunt deze sjabloon gebruiken voor uw eigen implementaties of de sjabloon aanpassen aan uw eisen.

Diagnostische instellingen worden op dit moment gedeeld voor alle caches in dezelfde regio voor een abonnement. Bijwerken van een cache in de regio is van invloed op alle caches in de regio.

Zie voor meer informatie over het maken van sjablonen, [Azure Resource Manager-sjablonen ontwerpen](../azure-resource-manager/resource-group-authoring-templates.md). Zie voor meer informatie over de JSON-syntaxis en de eigenschappen voor cache-resourcetypen, [Microsoft.Cache resourcetypen](/azure/templates/microsoft.cache/allversions).

Zie voor de volledige sjabloon, [Azure Cache voor Redis-sjabloon](https://github.com/Azure/azure-quickstart-templates/blob/master/101-redis-cache/azuredeploy.json).

> [!NOTE]
> Resource Manager-sjablonen voor de nieuwe [Premium-laag](cache-premium-tier-intro.md) beschikbaar zijn. 
> 
> * [Maken van een Premium Azure Cache voor Redis met clustering](https://azure.microsoft.com/resources/templates/201-redis-premium-cluster-diagnostics/)
> * [Premium Azure-Cache maken voor Redis met persistentie van gegevens](https://azure.microsoft.com/documentation/templates/201-redis-premium-persistence/)
> * [Premium Azure Cache voor Redis met VNet en optionele clusters maken](https://azure.microsoft.com/documentation/templates/201-redis-premium-vnet-cluster-diagnostics/)
> 
> Om te controleren of de meest recente sjablonen, Zie [Azure-Snelstartsjablonen](https://azure.microsoft.com/documentation/templates/) en zoek naar de `Azure Cache for Redis`.
> 
> 

## <a name="what-you-will-deploy"></a>Wat u wilt implementeren
In deze sjabloon implementeert u een Azure-Cache voor Redis die gebruikmaakt van een bestaand opslagaccount voor diagnostische gegevens.

Klik op de volgende knop om de implementatie automatisch uit te voeren:

[![Implementeren in Azure](./media/cache-redis-cache-arm-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-redis-cache%2Fazuredeploy.json)

## <a name="parameters"></a>Parameters
Met Azure Resource Manager kunt u parameters definiëren voor waarden die u wilt opgeven wanneer de sjabloon wordt geïmplementeerd. De sjabloon bevat een sectie Parameters die alle parameterwaarden bevat.
U moet een parameter definiëren voor de waarden die variëren op basis van het project dat u wilt implementeren of op basis van de omgeving waarin u gaat implementeren. Definieer geen parameters voor waarden die altijd hetzelfde blijven. De waarde van elke parameter wordt gebruikt in de sjabloon voor het definiëren van de resources die worden geïmplementeerd. 

[!INCLUDE [app-service-web-deploy-redis-parameters](../../includes/cache-deploy-parameters.md)]

### <a name="rediscachelocation"></a>redisCacheLocation
De locatie van de Azure-Cache voor Redis. Voor de beste prestaties gebruikt u dezelfde locatie als de app moet worden gebruikt met de cache.

    "redisCacheLocation": {
      "type": "string"
    }

### <a name="existingdiagnosticsstorageaccountname"></a>existingDiagnosticsStorageAccountName
De naam van het bestaande storage-account moet worden gebruikt voor diagnostische gegevens. 

    "existingDiagnosticsStorageAccountName": {
      "type": "string"
    }

### <a name="enablenonsslport"></a>enableNonSslPort
Een Booleaanse waarde die aangeeft of toegang via niet-SSL-poorten is toegestaan.

    "enableNonSslPort": {
      "type": "bool"
    }

### <a name="diagnosticsstatus"></a>diagnosticsStatus
Een waarde die aangeeft of de diagnostische gegevens is ingeschakeld. Gebruik aan of uit.

    "diagnosticsStatus": {
      "type": "string",
      "defaultValue": "ON",
      "allowedValues": [
            "ON",
            "OFF"
        ]
    }

## <a name="resources-to-deploy"></a>Resources om te implementeren
### <a name="azure-cache-for-redis"></a>Azure Cache voor Redis
Hiermee maakt u de Azure Cache voor Redis.

    {
      "apiVersion": "2015-08-01",
      "name": "[parameters('redisCacheName')]",
      "type": "Microsoft.Cache/Redis",
      "location": "[parameters('redisCacheLocation')]",
      "properties": {
        "enableNonSslPort": "[parameters('enableNonSslPort')]",
        "sku": {
          "capacity": "[parameters('redisCacheCapacity')]",
          "family": "[parameters('redisCacheFamily')]",
          "name": "[parameters('redisCacheSKU')]"
        }
      },
      "resources": [
        {
          "apiVersion": "2017-05-01-preview",
          "type": "Microsoft.Cache/redis/providers/diagnosticsettings",
          "name": "[concat(parameters('redisCacheName'), '/Microsoft.Insights/service')]",
          "location": "[parameters('redisCacheLocation')]",
          "dependsOn": [
            "[concat('Microsoft.Cache/Redis/', parameters('redisCacheName'))]"
          ],
          "properties": {
            "status": "[parameters('diagnosticsStatus')]",
            "storageAccountName": "[parameters('existingDiagnosticsStorageAccountName')]"
          }
        }
      ]
    }



## <a name="commands-to-run-deployment"></a>Opdrachten om implementatie uit te voeren
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a>PowerShell

    New-AzResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-redis-cache/azuredeploy.json -ResourceGroupName ExampleDeployGroup -redisCacheName ExampleCache

### <a name="azure-cli"></a>Azure-CLI
    azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-redis-cache/azuredeploy.json -g ExampleDeployGroup


