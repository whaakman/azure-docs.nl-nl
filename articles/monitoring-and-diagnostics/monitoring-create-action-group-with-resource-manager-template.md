---
title: Actiegroepen maken met Resource Manager-sjablonen | Microsoft Docs
description: Informatie over het maken van een actiegroep met een Azure Resource Manager-sjabloon.
author: anirudhcavale
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: ancav
ms.openlocfilehash: 5e715cad5cb28ad0c763ffb29c43e9ee98741699
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="create-an-action-group-with-a-resource-manager-template"></a>Een actiegroep met Resource Manager-sjabloon maken
Dit artikel laat zien hoe u een [Azure Resource Manager-sjabloon](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authoring-templates) actiegroepen configureren. Met behulp van sjablonen, kunt u automatisch actiegroepen die opnieuw kunnen worden gebruikt in bepaalde typen waarschuwingen instellen. Deze actiegroepen Zorg ervoor dat de juiste partijen worden gewaarschuwd wanneer een waarschuwing wordt geactiveerd.

De eenvoudige stappen zijn:

1. Een sjabloon maken als een JSON-bestand dat wordt beschreven hoe u de actiegroep te maken.

2. De sjabloon implementeren met behulp van [eventuele implementatiemethode](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy).

We beschrijven eerst het maken van een Resource Manager-sjabloon voor een actiegroep waarop de actie definities vastgelegd in de sjabloon zijn. Ten tweede beschreven hoe u een sjabloon maken die de configuratie-informatie van de webhook als invoerparameters wordt uitgevoerd wanneer de sjabloon wordt geïmplementeerd.

## <a name="resource-manager-templates-for-an-action-group"></a>Resource Manager-sjablonen voor een actiegroep

Een om actiegroep te maken met een Resource Manager-sjabloon, die u maakt een resource van het type `Microsoft.Insights/actionGroups`. Vervolgens vult u alle bijbehorende eigenschappen. Hier vindt u twee voorbeeldsjablonen die een actiegroep maken.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "actionGroupName": {
      "type": "string",
      "metadata": {
        "description": "Unique name (within the Resource Group) for the Action group."
      }
    },
    "actionGroupShortName": {
      "type": "string",
      "metadata": {
        "description": "Short name (maximum 12 characters) for the Action group."
      }
    }
  },
  "resources": [
    {
      "type": "Microsoft.Insights/actionGroups",
      "apiVersion": "2017-04-01",
      "name": "[parameters('actionGroupName')]",
      "location": "Global",
      "properties": {
        "groupShortName": "[parameters('actionGroupShortName')]",
        "enabled": true,
        "smsReceivers": [
          {
            "name": "contosoSMS",
            "countryCode": "1",
            "phoneNumber": "5555551212"
          },
          {
            "name": "contosoSMS2",
            "countryCode": "1",
            "phoneNumber": "5555552121"
          }
        ],
        "emailReceivers": [
          {
            "name": "contosoEmail",
            "emailAddress": "devops@contoso.com"
          },
          {
            "name": "contosoEmail2",
            "emailAddress": "devops2@contoso.com"
          }
        ],
        "webhookReceivers": [
          {
            "name": "contosoHook",
            "serviceUri": "http://requestb.in/1bq62iu1"
          },
          {
            "name": "contosoHook2",
            "serviceUri": "http://requestb.in/1bq62iu2"
          }
        ]
      }
    }
  ],
  "outputs":{
      "actionGroupId":{
          "type":"string",
          "value":"[resourceId('Microsoft.Insights/actionGroups',parameters('actionGroupName'))]"
      }
  }
}
```

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "actionGroupName": {
      "type": "string",
      "metadata": {
        "description": "Unique name (within the Resource Group) for the Action group."
      }
    },
    "actionGroupShortName": {
      "type": "string",
      "metadata": {
        "description": "Short name (maximum 12 characters) for the Action group."
      }
    },
    "webhookReceiverName": {
      "type": "string",
      "metadata": {
        "description": "Webhook receiver service Name."
      }
    },    
    "webhookServiceUri": {
      "type": "string",
      "metadata": {
        "description": "Webhook receiver service URI."
      }
    }    
  },
  "resources": [
    {
      "type": "Microsoft.Insights/actionGroups",
      "apiVersion": "2017-04-01",
      "name": "[parameters('actionGroupName')]",
      "location": "Global",
      "properties": {
        "groupShortName": "[parameters('actionGroupShortName')]",
        "enabled": true,
        "smsReceivers": [
        ],
        "emailReceivers": [
        ],
        "webhookReceivers": [
          {
            "name": "[parameters('webhookReceiverName')]",
            "serviceUri": "[parameters('webhookServiceUri')]"
          }
        ]
      }
    }
  ],
  "outputs":{
      "actionGroupResourceId":{
          "type":"string",
          "value":"[resourceId('Microsoft.Insights/actionGroups',parameters('actionGroupName'))]"
      }
  }
}
```


## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [actiegroepen](monitoring-action-groups.md).
* Meer informatie over [waarschuwingen](monitoring-overview-alerts.md).
* Meer informatie over het toevoegen van [waarschuwingen met een Resource Manager-sjabloon](monitoring-create-activity-log-alerts-with-resource-manager-template.md).
