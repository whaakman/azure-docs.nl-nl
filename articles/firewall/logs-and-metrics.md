---
title: Overzicht van Azure-Firewall-logboeken
description: In dit artikel wordt een overzicht van de diagnostische logboeken van de Firewall van Azure.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 9/24/2018
ms.author: victorh
ms.openlocfilehash: c129c394f3d694b832722287027c1f9e58028a33
ms.sourcegitcommit: fdd6a2927976f99137bb0fcd571975ff42b2cac0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/27/2019
ms.locfileid: "56957688"
---
# <a name="azure-firewall-logs"></a>Azure Firewall-logboeken

U kunt Azure Firewall bewaken met behulp van firewall-logboeken. U kunt ook activiteitenlogboeken gebruiken om bewerkingen in Azure Firewall-resources te controleren.

Via de portal kunt u toegang verkrijgen tot sommige van deze logboeken. Logboeken kunnen worden verzonden naar [logboeken van Azure Monitor](../azure-monitor/insights/azure-networking-analytics.md), opslag- en Event Hubs en geanalyseerd in Logboeken van Azure Monitor of door verschillende hulpprogramma's zoals Excel en Power BI.

## <a name="diagnostic-logs"></a>Diagnostische logboeken

 De volgende diagnostische logboeken zijn beschikbaar voor Azure Firewall:

* **Logboek voor toepassingsregels**

   Het toepassingslogboek van de regel wordt opgeslagen in een storage-account, worden gestreamd naar eventhubs en/of verzonden naar de logboeken van Azure Monitor alleen als u deze voor elke Azure-Firewall hebt ingeschakeld. Elke nieuwe verbinding die overeenkomt met een van uw geconfigureerde toepassingsregels, resulteert in een logboek voor de geaccepteerde/geweigerde verbinding. De gegevens worden geregistreerd in JSON-indeling, zoals weergegeven in het volgende voorbeeld:

   ```
   Category: application rule logs.
   Time: log timestamp.
   Properties: currently contains the full message. 
   note: this field will be parsed to specific fields in the future, while maintaining backward compatibility with the existing properties field.
   ```

   ```json
   {
    "category": "AzureFirewallApplicationRule",
    "time": "2018-04-16T23:45:04.8295030Z",
    "resourceId": "/SUBSCRIPTIONS/{subscriptionId}/RESOURCEGROUPS/{resourceGroupName}/PROVIDERS/MICROSOFT.NETWORK/AZUREFIREWALLS/{resourceName}",
    "operationName": "AzureFirewallApplicationRuleLog",
    "properties": {
        "msg": "HTTPS request from 10.1.0.5:55640 to mydestination.com:443. Action: Allow. Rule Collection: collection1000. Rule: rule1002"
    }
   }
   ```

* **Logboek voor netwerkregels**

   Het logboek van de regel netwerk wordt opgeslagen in een storage-account, worden gestreamd naar eventhubs en/of verzonden naar de logboeken van Azure Monitor alleen als u deze voor elke Azure-Firewall hebt ingeschakeld. Elke nieuwe verbinding die overeenkomt met een van uw geconfigureerde netwerkregels, resulteert in een logboek voor de geaccepteerde/geweigerde verbinding. De gegevens worden geregistreerd in JSON-indeling, zoals weergegeven in het volgende voorbeeld:

   ```
   Category: network rule logs.
   Time: log timestamp.
   Properties: currently contains the full message. 
   note: this field will be parsed to specific fields in the future, while maintaining backward compatibility with the existing properties field.
   ```

   ```json
  {
    "category": "AzureFirewallNetworkRule",
    "time": "2018-06-14T23:44:11.0590400Z",
    "resourceId": "/SUBSCRIPTIONS/{subscriptionId}/RESOURCEGROUPS/{resourceGroupName}/PROVIDERS/MICROSOFT.NETWORK/AZUREFIREWALLS/{resourceName}",
    "operationName": "AzureFirewallNetworkRuleLog",
    "properties": {
        "msg": "TCP request from 111.35.136.173:12518 to 13.78.143.217:2323. Action: Deny"
    }
   }

   ```

U hebt drie opties voor het opslaan van uw logboeken:

* **Storage-account**: Storage-accounts zijn beste worden gebruikt voor Logboeken als Logboeken worden opgeslagen voor een langere duur en gecontroleerd wanneer dit nodig is.
* **Eventhubs**: Eventhubs zijn een goede optie voor het integreren met andere security information en event management (SEIM) hulpprogramma's voor waarschuwingen over uw resources.
* **Logboeken in Azure Monitor**: Logboeken in Azure Monitor is best gebruikt voor algemene realtime-controle van uw toepassing of trends kijken.

## <a name="activity-logs"></a>Activiteitenlogboeken

   Activiteitenlogboekitems worden standaard verzameld en kunnen in de Azure-portal worden bekeken.

   U kunt [Azure-activiteitenlogboeken](../azure-resource-manager/resource-group-audit.md) (voorheen bekend als operationele logboeken en auditlogboeken) gebruiken om alle bewerkingen te bekijken die naar uw Azure-abonnement worden verzonden.


## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het bewaken van Azure-Firewall-logboeken en metrische gegevens, [zelfstudie: Azure-Firewall-logboeken bewaken](tutorial-diagnostics.md).