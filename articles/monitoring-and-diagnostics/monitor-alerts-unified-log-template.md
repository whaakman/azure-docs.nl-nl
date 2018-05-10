---
title: Maken van een waarschuwing in een logboek met Resource Manager-sjabloon | Microsoft Docs
description: Informatie over het logboek waarschuwing maken met behulp van een Azure Resource Manager-sjabloon en de API.
author: msvijayn
manager: kmadnani1
editor: ''
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: ''
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/01/2018
ms.author: vinagara
ms.openlocfilehash: 4148c6d3f9d4407681159870b858c629c0b5d878
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/08/2018
---
# <a name="create-a-log-alert-with-a-resource-manager-template"></a>Een waarschuwing in een logboek met Resource Manager-sjabloon maken
Dit artikel laat zien hoe u kunt beheren [waarschuwingen melden](monitor-alerts-unified-log.md) programmatisch op grote schaal, bij het gebruik van Azure [Azure Resource Manager-sjabloon](..//azure-resource-manager/resource-group-authoring-templates.md) via [Azure Powershell](../azure-resource-manager/resource-group-template-deploy.md) en [Azure CLI](../azure-resource-manager/resource-group-template-deploy-cli.md). Momenteel waarschuwingen van Azure, ondersteunt Meld u waarschuwingen op query's van [Azure Log Analytics](../log-analytics/log-analytics-tutorial-viewdata.md) en [Azure Application Insights](../application-insights/app-insights-analytics-tour.md).

## <a name="managing-log-alert-on-log-analytics"></a>Het beheren van het logboek een waarschuwing voor logboekanalyse
Waarschuwing voor logboek [Azure Log Analytics](../log-analytics/log-analytics-tutorial-viewdata.md) is geïntegreerd in de [nieuwe waarschuwingen van de Azure-ervaring](monitoring-overview-unified-alerts.md); terwijl er nog steeds Log Analytics-API's verdwijnt en compatibiliteit met eerdere gebruikt voor het beheren van schemablijft[waarschuwingen in de OMS-portal](..//log-analytics/log-analytics-alerts-creating.md).

> [!NOTE]
> Alle waarschuwingen in een werkruimte, dat is vanaf 14 mei 2018, wordt automatisch gestart om uit te breiden naar Azure. Een gebruiker kunt uitbreiden waarschuwingen naar Azure vrijwillig voordat 14 mei 2018 initiëren. Zie voor meer informatie [waarschuwingen uitbreiden naar Azure van OMS](monitoring-alerts-extend.md). 

### <a name="using-azure-resource-manager-template"></a>Met behulp van Azure Resource Manager-sjabloon
Logboek waarschuwingen voor logboekanalyse worden gemaakt door regels voor waarschuwingen die een opgeslagen zoekopdracht op een vast interval uitvoert. Als de resultaten van de query overeenkomen met criteria opgegeven, wordt een waarschuwing record gemaakt en een of meer acties worden uitgevoerd. 

Resource-sjabloon voor [Log analytics opgeslagen zoekopdracht](../log-analytics/log-analytics-log-searches.md) en [analytics waarschuwingen melden](../log-analytics/log-analytics-alerts.md) zijn beschikbaar in de sectie logboekanalyse van documentatie. Meer informatie over, [logboekanalyse toe te voegen opgeslagen zoekopdrachten en waarschuwingen](../operations-management-suite/operations-management-suite-solutions-resources-searches-alerts.md); waaronder aanvullende voorbeelden, evenals een schema-informatie.

### <a name="using-resource-template-via-apipowershell"></a>Met behulp van de sjabloon Resource via API/Powershell
De waarschuwing REST-API van Log Analytics RESTful is en toegankelijk zijn via de REST-API van Azure Resource Manager. De API dus toegankelijk zijn vanuit een PowerShell-opdrachtregel en wordt de uitvoer zoekresultaten aan u in de JSON-indeling, zodat u kunt de resultaten programmatisch op veel verschillende manieren gebruiken.

Meer informatie over [maken en beheren van waarschuwingsregels in logboekanalyse met REST-API](../log-analytics/log-analytics-api-alerts.md); inclusief voorbeelden van de toegang tot de API van Powershell.

## <a name="managing-log-alert-on-application-insights"></a>Het beheren van het logboek een waarschuwing voor Application Insights
Logboek waarschuwingen voor Azure Application Insights hebt geïntroduceerd als onderdeel van de nieuwe Azure waarschuwingen onder Azure bewaken. Daarom deze wordt uitgevoerd onder de API van Azure-Monitor als [queryregels gepland](https://docs.microsoft.com/en-us/rest/api/monitor/scheduledqueryrules/) groep voor REST-bewerking.

### <a name="using-azure-resource-manager-template"></a>Met behulp van Azure Resource Manager-sjabloon
Waarschuwing voor Application Insights-resources logboek heeft een type `Microsoft.Insights/scheduledQueryRules/`. Zie voor meer informatie over dit brontype [Azure Monitor - gepland Query regels API-referentiemateriaal](https://docs.microsoft.com/en-us/rest/api/monitor/scheduledqueryrules/).

Hieronder volgt de structuur voor [queryregels gepland maken](https://docs.microsoft.com/en-us/rest/api/monitor/scheduledqueryrules/createorupdate) op basis van de resource-sjabloon met voorbeeldgegevens als variabelen.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",    
    "variables": {
    "alertLocation": "southcentralus",
    "alertName": "samplelogalert",
    "alertTag": "hidden-link:/subscriptions/a123d7efg-123c-1234-5678-a12bc3defgh4/resourceGroups/myRG/providers/microsoft.insights/components/sampleAIapplication",
    "alertDesription": "Sample log search alert",
    "alertStatus": "true",
    "alertSource":{
       "Query":"requests",
       "SourceId": "/subscriptions/a123d7efg-123c-1234-5678-a12bc3defgh4/resourceGroups/myRG/providers/microsoft.insights/components/sampleAIapplication",
       "Type":"ResultCount"
        },
    "alertSchedule":{
        "Frequency": 15,
        "Time": 60
        },
    "alertActions":{
        "SeverityLevel": "4",
        },
     "alertTrigger":{
       "Operator":"GreaterThan",
       "Threshold":"1"
        },
      "actionGrp":{
       "ActionGroup": "/subscriptions/a123d7efg-123c-1234-5678-a12bc3defgh4/resourceGroups/myRG/providers/microsoft.insights/actiongroups/sampleAG",
      "Subject": "Customized Email Header",
      "Webhook": "{}"           
        }
  },
  "resources":[ {
     "name":"[variables('alertName')]",
     "type":"Microsoft.Insights/scheduledQueryRules",
     "apiVersion": "2018-04-16",
     "location": "[variables('alertLocation')]",
     "tags":{"hidden-link:[variables('alertTargetResource')]": "Resource"},
     "properties":{
        "description": "[variables('alertDescription')]",
        "enabled": "[variables('alertStatus')]",
        "source": {
            "query": "[variables('alertSource').Query]",
            "dataSourceId": "[variables('alertSource').SourceId]",
            "queryType":"[variables('alertSource').Type]"
        },
       "schedule":{
            "frequencyInMinutes": "[variables('alertSchedule').Frequency]",
            "timeWindowInMinutes": "[variables('alertSchedule').Time]"    
        },
       "action":{
            "severity":"[variables('alertActions').SeverityLevel]",
            "aznsAction":{
                "actionGroup":"[array(variables('actionGrp').ActionGroup)]",
                "emailSubject":"[variables('actionGrp').Subject]",
                "customWebhookPayload":"[variables('actionGrp').Webhook]"
            },
        "trigger":{
                "thresholdOperator":"[variables('alertTrigger').Operator]",
                "threshold":"[variables('alertTrigger').Threshold]"
            }
        }
      }
    }
  ]
}
```
> [!IMPORTANT]
> Tagveld met verborgen koppeling naar doelbron is verplicht in het gebruik van [queryregels gepland ](https://docs.microsoft.com/en-us/rest/api/monitor/scheduledqueryrules/) API-aanroep of resource-sjabloon. 

Het bovenstaande voorbeeld-json als (spreek) sampleScheduledQueryRule.json omwille van de in dit scenario kunnen worden opgeslagen en kan worden geïmplementeerd met [Azure Resource Manager in Azure-portal](../azure-resource-manager/resource-group-template-deploy-portal.md#deploy-resources-from-custom-template).

### <a name="using-resource-template-via-clipowershell"></a>Met behulp van de sjabloon Resource via CLI/Powershell
Controleprogramma voor Azure - API van de regels geplande Query is een REST-API en volledig compatibel is met de REST-API van Azure Resource Manager. Daarom kan worden gebruikt via Powershell met Resource Manager-cmdlet, evenals Azure CLI.

Hieronder gebruik via Azure Resource Manager PowerShell-cmdlet voor het voorbeeld Resource sjabloon eerder weergegeven (sampleScheduledQueryRule.json) weergegeven:
```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName "myRG" -TemplateFile "D:\Azure\Templates\sampleScheduledQueryRule.json"
```
Hieronder gebruik via Azure Resource Manager-opdracht in de Azure CLI voor Resource sjabloon eerder weergegeven (sampleScheduledQueryRule.json) weergegeven:

```azurecli
az group deployment create --resource-group myRG --template-file sampleScheduledQueryRule.json
```
Op de goede werking 201 wordt geretourneerd naar de status van het maken van nieuwe waarschuwingsregel of 200 wordt geretourneerd als een bestaande waarschuwingsregel is gewijzigd.


## <a name="next-steps"></a>Volgende stappen
* Begrijpen [webhookacties voor logboek-waarschuwingen](monitor-alerts-unified-log-webhook.md)
* Meer informatie over de nieuwe [Azure-waarschuwingen](monitoring-overview-unified-alerts.md)
* Meer informatie over [Application Insights](../application-insights/app-insights-analytics.md)
* Meer informatie over [logboekanalyse](../log-analytics/log-analytics-overview.md).   