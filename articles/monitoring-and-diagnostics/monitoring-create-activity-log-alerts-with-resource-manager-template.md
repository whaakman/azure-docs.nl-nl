---
title: Maken van een activiteit logboek waarschuwing met Resource Manager-sjabloon | Microsoft Docs
description: Op de hoogte worden gesteld wanneer uw Azure-resources zijn gemaakt.
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
ms.date: 07/06/2017
ms.author: ancav
ms.openlocfilehash: 92076c7fe1f867919b7e02abf79cf0fb74fb7eb4
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="create-an-activity-log-alert-with-a-resource-manager-template"></a>Een activiteit logboek waarschuwing met Resource Manager-sjabloon maken
Dit artikel laat zien hoe u een [Azure Resource Manager-sjabloon](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-authoring-templates) activiteit logboek waarschuwingen configureren. Met behulp van sjablonen, kunt u gemakkelijk veel waarschuwingen die worden geactiveerd op basis van specifieke activiteit logboek gebeurtenis voorwaarden als onderdeel van uw geautomatiseerde implementatie instellen.

De eenvoudige stappen zijn:

1. Een sjabloon maken als een JSON-bestand dat wordt beschreven hoe u de activiteit logboek-waarschuwing wilt maken.

2. De sjabloon implementeren met behulp van [eventuele implementatiemethode](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy).

## <a name="resource-manager-template-for-an-activity-log-alert"></a>Resource Manager-sjabloon voor een activiteit logboek-waarschuwing
Activiteit logboek waarschuwingen met een Resource Manager-sjabloon maken, die u maakt een resource van het type `microsoft.insights/activityLogAlerts`. Vervolgens vult u alle bijbehorende eigenschappen. Hier volgt een sjabloon die u een activiteit logboek-waarschuwing maakt.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "activityLogAlertName": {
      "type": "string",
      "metadata": {
        "description": "Unique name (within the Resource Group) for the Activity log alert."
      }
    },
    "activityLogAlertEnabled": {
      "type": "bool",
      "defaultValue": true,
      "metadata": {
        "description": "Indicates whether or not the alert is enabled."
      }
    },
    "actionGroupResourceId": {
      "type": "string",
      "metadata": {
        "description": "Resource Id for the Action group."
      }
    }
  },
  "resources": [   
    {
      "type": "Microsoft.Insights/activityLogAlerts",
      "apiVersion": "2017-04-01",
      "name": "[parameters('activityLogAlertName')]",      
      "location": "Global",
      "properties": {
        "enabled": "[parameters('activityLogAlertEnabled')]",
        "scopes": [
            "[subscription().id]"
        ],        
        "condition": {
          "allOf": [
            {
              "field": "category",
              "equals": "Administrative"
            },
            {
              "field": "operationName",
              "equals": "Microsoft.Resources/deployments/write"
            },
            {
              "field": "resourceType",
              "equals": "Microsoft.Resources/deployments"
            }
          ] 
        },
        "actions": {
          "actionGroups": 
          [
            {
              "actionGroupId": "[parameters('actionGroupResourceId')]"
            }
          ]
        }
      }
    }
  ]
}
```

Ga naar onze [Azure snelstartgalerie](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Insights) voor enkele voorbeelden van waarschuwing activiteitssjablonen logboek.

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [waarschuwingen](monitoring-overview-alerts.md).
- Meer informatie over het toevoegen van [actiegroepen met behulp van een Resource Manager-sjabloon](monitoring-create-action-group-with-resource-manager-template.md).
- Meer informatie over hoe [maken van een activiteit logboek-waarschuwing voor het bewaken van alle automatisch schalen engine bewerkingen voor uw abonnement](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-alert).
- Meer informatie over hoe [maken van een activiteit logboek-waarschuwing voor het bewaken van alle mislukte automatisch schalen scale-in/scale-out-bewerkingen op uw abonnement](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-failed-alert).
