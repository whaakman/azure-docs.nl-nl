---
title: Inrichten van een Redis-Cache met Azure Resource Manager | Microsoft Docs
description: Gebruik Azure Resource Manager-sjabloon voor het implementeren van een Azure Redis-Cache.
services: app-service
documentationcenter: 
author: wesmc7777
manager: cfowler
editor: 
ms.assetid: ce6f5372-7038-4655-b1c5-108f7c148282
ms.service: cache
ms.workload: web
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: wesmc
ms.openlocfilehash: b26116b974abbfe410b0a6ebc0186d73f4eea1bf
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="create-a-redis-cache-using-a-template"></a>Een Redis Cache maken op basis van een sjabloon
In dit onderwerp leert u het maken van een Azure Resource Manager-sjabloon die u een Azure Redis-Cache implementeert. De cache kan worden gebruikt met een bestaand opslagaccount om diagnostische gegevens te behouden. U leert ook hoe om te definiëren welke bronnen worden geïmplementeerd en het definiëren van de parameters die zijn opgegeven wanneer de implementatie wordt uitgevoerd. U kunt deze sjabloon gebruiken voor uw eigen implementaties of de sjabloon aanpassen aan uw eisen.

Diagnostische instellingen worden op dit moment wordt gedeeld voor alle caches in dezelfde regio voor een abonnement. Bijwerken van een cache in het gebied van invloed op alle andere caches in de regio.

Zie voor meer informatie over het maken van sjablonen [Azure Resource Manager-sjablonen ontwerpen](../azure-resource-manager/resource-group-authoring-templates.md).

Zie voor de volledige sjabloon [Redis-Cache sjabloon](https://github.com/Azure/azure-quickstart-templates/blob/master/101-redis-cache/azuredeploy.json).

> [!NOTE]
> Resource Manager-sjablonen voor de nieuwe [Premium-laag](cache-premium-tier-intro.md) beschikbaar zijn. 
> 
> * [Maken van een Premium Redis-Cache met clustering](https://azure.microsoft.com/documentation/templates/201-redis-premium-cluster-diagnostics/)
> * [Premium Redis-Cache met gegevenspersistentie maken](https://azure.microsoft.com/documentation/templates/201-redis-premium-persistence/)
> * [Premium Redis-Cache maken met VNet en een optionele clustering](https://azure.microsoft.com/documentation/templates/201-redis-premium-vnet-cluster-diagnostics/)
> 
> Om te controleren of de meest recente sjablonen, Zie [Azure-Snelstartsjablonen](https://azure.microsoft.com/documentation/templates/) en zoek naar `Redis Cache`.
> 
> 

## <a name="what-you-will-deploy"></a>Wat u wilt implementeren
In deze sjabloon implementeert u een Azure Redis-Cache die gebruikmaakt van een bestaand opslagaccount voor diagnostische gegevens.

Klik op de volgende knop om de implementatie automatisch uit te voeren:

[![Implementeren in Azure](./media/cache-redis-cache-arm-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-redis-cache%2Fazuredeploy.json)

## <a name="parameters"></a>Parameters
Met Azure Resource Manager kunt u parameters definiëren voor waarden die u wilt opgeven wanneer de sjabloon wordt geïmplementeerd. De sjabloon bevat een sectie met de naam van de Parameters die alle van de parameterwaarden bevat.
U moet een parameter definiëren voor de waarden die variëren op basis van het project dat u wilt implementeren of op basis van de omgeving waarin u gaat implementeren. Definieer geen parameters voor waarden die altijd hetzelfde blijven. De waarde van elke parameter wordt gebruikt in de sjabloon voor het definiëren van de resources die worden geïmplementeerd. 

[!INCLUDE [app-service-web-deploy-redis-parameters](../../includes/cache-deploy-parameters.md)]

### <a name="rediscachelocation"></a>redisCacheLocation
De locatie van de Redis-Cache. Voor de beste prestaties gebruik van dezelfde locatie als de app moet worden gebruikt met de cache.

    "redisCacheLocation": {
      "type": "string"
    }

### <a name="existingdiagnosticsstorageaccountname"></a>existingDiagnosticsStorageAccountName
De naam van het bestaande opslagaccount voor diagnostische gegevens. 

    "existingDiagnosticsStorageAccountName": {
      "type": "string"
    }

### <a name="enablenonsslport"></a>enableNonSslPort
Een Boolean die aangeeft of toegang hebben via niet-SSL-poort.

    "enableNonSslPort": {
      "type": "bool"
    }

### <a name="diagnosticsstatus"></a>diagnosticsStatus
Een waarde die aangeeft of diagnostische gegevens is ingeschakeld. Gebruik ON of OFF.

    "diagnosticsStatus": {
      "type": "string",
      "defaultValue": "ON",
      "allowedValues": [
            "ON",
            "OFF"
        ]
    }

## <a name="resources-to-deploy"></a>Resources om te implementeren
### <a name="redis-cache"></a>Redis Cache
Hiermee maakt u de Azure Redis-Cache.

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
          "apiVersion": "2015-07-01",
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
    New-AzureRmResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-redis-cache/azuredeploy.json -ResourceGroupName ExampleDeployGroup -redisCacheName ExampleCache

### <a name="azure-cli"></a>Azure-CLI
    azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-redis-cache/azuredeploy.json -g ExampleDeployGroup


