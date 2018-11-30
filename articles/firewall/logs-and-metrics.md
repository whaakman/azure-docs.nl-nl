---
title: Overzicht van Azure-Firewall-logboeken
description: In dit artikel wordt een overzicht van de diagnostische logboeken van de Firewall van Azure.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 9/24/2018
ms.author: victorh
ms.openlocfilehash: 0698f1dbc491781089ef94eec32f2a427fd3cca4
ms.sourcegitcommit: c61c98a7a79d7bb9d301c654d0f01ac6f9bb9ce5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/27/2018
ms.locfileid: "52422385"
---
# <a name="azure-firewall-logs"></a>Azure Firewall-logboeken

U kunt Azure Firewall bewaken met behulp van firewall-logboeken. U kunt ook activiteitenlogboeken gebruiken om bewerkingen in Azure Firewall-resources te controleren.

Via de portal kunt u toegang verkrijgen tot sommige van deze logboeken. Logboeken kunnen worden verzonden naar [Log Analytics](../azure-monitor/insights/azure-networking-analytics.md), Storage en Event Hubs en kunnen worden geanalyseerd in Log Analytics of door verschillende hulpprogramma’s zoals Excel en Power BI.

## <a name="diagnostic-logs"></a>Diagnostische logboeken

 De volgende diagnostische logboeken zijn beschikbaar voor Azure Firewall:

* **Logboek voor toepassingsregels**

   Het logboek voor toepassingsregels wordt alleen in een opslagaccount opgeslagen, naar Event Hubs gestreamd en/of naar Log Analytics verzonden als u het voor elke Azure Firewall hebt ingeschakeld. Elke nieuwe verbinding die overeenkomt met een van uw geconfigureerde toepassingsregels, resulteert in een logboek voor de geaccepteerde/geweigerde verbinding. De gegevens worden geregistreerd in JSON-indeling, zoals weergegeven in het volgende voorbeeld:

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

   Het logboek voor netwerkregels wordt alleen in een opslagaccount opgeslagen, naar Event Hubs gestreamd en/of naar Log Analytics verzonden als u het voor elke Azure Firewall hebt ingeschakeld. Elke nieuwe verbinding die overeenkomt met een van uw geconfigureerde netwerkregels, resulteert in een logboek voor de geaccepteerde/geweigerde verbinding. De gegevens worden geregistreerd in JSON-indeling, zoals weergegeven in het volgende voorbeeld:

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

* **Opslagaccount**: opslagaccounts kunnen het best worden gebruikt wanneer logboeken voor een langere periode worden opgeslagen en moeten kunnen worden bekeken wanneer dat nodig is.
* **Event Hubs**: Event Hubs zijn een geweldige optie voor integratie met andere SIEM-hulpprogramma’s (Security Information and Event Management) om waarschuwingen over uw resources te krijgen.
* **Log Analytics**: Log Analytics kan het best worden gebruikt voor algemene realtime bewaking van uw toepassing of voor het bekijken van trends.

## <a name="activity-logs"></a>Activiteitenlogboeken

   Activiteitenlogboekitems worden standaard verzameld en kunnen in de Azure-portal worden bekeken.

   U kunt [Azure-activiteitenlogboeken](../azure-resource-manager/resource-group-audit.md) (voorheen bekend als operationele logboeken en auditlogboeken) gebruiken om alle bewerkingen te bekijken die naar uw Azure-abonnement worden verzonden.


## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het bewaken van Azure-Firewall-logboeken en metrische gegevens, [zelfstudie: logboeken van de Firewall van Azure Monitor](tutorial-diagnostics.md).