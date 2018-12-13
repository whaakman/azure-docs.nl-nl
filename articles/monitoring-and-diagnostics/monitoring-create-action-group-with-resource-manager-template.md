---
title: Actiegroepen maken met Resource Manager-sjablonen
description: Leer hoe u een actiegroep maken met behulp van een Azure Resource Manager-sjabloon.
author: dkamstra
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 02/16/2018
ms.author: dukek
ms.component: alerts
ms.openlocfilehash: 0023b8aa5d4f79af397b937030c5308a970af991
ms.sourcegitcommit: 5b869779fb99d51c1c288bc7122429a3d22a0363
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/10/2018
ms.locfileid: "53187497"
---
# <a name="create-an-action-group-with-a-resource-manager-template"></a>Een actiegroep maken met een Resource Manager-sjabloon
In dit artikel leest u hoe u een [Azure Resource Manager-sjabloon](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authoring-templates) actiegroepen configureren. Met behulp van sjablonen, kunt u automatisch actiegroepen die opnieuw kunnen worden gebruikt in bepaalde typen waarschuwingen instellen. Deze actiegroepen ervoor te zorgen dat de juiste partijen worden gewaarschuwd wanneer een waarschuwing wordt geactiveerd.

De eenvoudige stappen zijn:

1. Een sjabloon maken als een JSON-bestand dat wordt beschreven hoe u de actiegroep maken.

2. De sjabloon implementeren met behulp van [eventuele implementatiemethode](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy).

Eerst wordt beschreven hoe u een Resource Manager-sjabloon voor een actiegroep waar de actie-definities vastgelegd in de sjabloon zijn. Ten tweede wordt beschreven hoe u een sjabloon maken die de configuratie-informatie van de webhook als invoerparameters moet worden uitgevoerd wanneer de sjabloon wordt geïmplementeerd.

## <a name="resource-manager-templates-for-an-action-group"></a>Resource Manager-sjablonen voor een actiegroep

Voor het maken van een actiegroep die u met behulp van Resource Manager-sjabloon maken van een resource van het type `Microsoft.Insights/actionGroups`. U Vul vervolgens alle verwante eigenschappen. Hier vindt u twee voorbeeldsjablonen die een actiegroep maken.

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
      "apiVersion": "2018-03-01",
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
      "apiVersion": "2018-03-01",
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
* Meer informatie over [actiegroepen](../azure-monitor/platform/action-groups.md).
* Meer informatie over [waarschuwingen](monitoring-overview-alerts.md).
* Informatie over het toevoegen [waarschuwingen met behulp van Resource Manager-sjabloon](../azure-monitor/platform/alerts-activity-log.md).
