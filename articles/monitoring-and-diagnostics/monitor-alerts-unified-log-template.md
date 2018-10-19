---
title: Een waarschuwing voor een logboek maken met een Resource Manager-sjabloon
description: Informatie over het maken van een waarschuwing met behulp van een Azure Resource Manager-sjabloon en de API.
author: msvijayn
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: vinagara
ms.component: alerts
ms.openlocfilehash: 6ec07d02e61d50aa588d75ba7337eb9237e11207
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/18/2018
ms.locfileid: "49402894"
---
# <a name="create-a-log-alert-with-a-resource-manager-template"></a>Een waarschuwing voor een logboek maken met een Resource Manager-sjabloon
Dit artikel wordt beschreven hoe u kunt beheren [waarschuwingen voor activiteitenlogboeken](monitor-alerts-unified-log.md) via een programma op schaal in Azure met [Azure Resource Manager-sjabloon](..//azure-resource-manager/resource-group-authoring-templates.md) via [Azure Powershell](../azure-resource-manager/resource-group-template-deploy.md) en [Azure CLI](../azure-resource-manager/resource-group-template-deploy-cli.md). Momenteel Azure-waarschuwingen ondersteunt waarschuwingen voor activiteitenlogboeken op query's uit de [Azure Log Analytics](../log-analytics/log-analytics-tutorial-viewdata.md) en [Azure Application Insights](../application-insights/app-insights-analytics-tour.md).

## <a name="managing-log-alert-on-log-analytics"></a>Waarschuwing in Log Analytics beheren
Waarschuwing voor [Azure Log Analytics](../log-analytics/log-analytics-tutorial-viewdata.md) is geïntegreerd in de [waarschuwingen voor nieuwe Azure-ervaring](monitoring-overview-unified-alerts.md); terwijl er nog steeds wordt uitgevoerd uit Log Analytics-API's en compatibile met het schema dat u eerder hebt gebruikt blijft.

> [!NOTE]
> Vanaf 14 mei 2018 worden is alle waarschuwingen in een werkruimte automatisch uitgebreid naar Azure. Zie voor meer informatie, [waarschuwingen uitbreiden naar Azure](monitoring-alerts-extend.md). 

### <a name="using-azure-resource-manager-template"></a>Met behulp van Azure Resource Manager-sjabloon
Waarschuwingen voor logboeken voor Log Analytics worden gemaakt door waarschuwingsregels die een opgeslagen zoekopdracht op een vast interval uitvoert. Als de resultaten van de query overeenkomen met criteria opgegeven, wordt een waarschuwingsrecord gemaakt en een of meer acties worden uitgevoerd. 

Resource-sjabloon voor Log analytics opgeslagen zoekopdracht en beschikbare Log analytics alertsare in Log Analytics-sectie van de documentatie bij. Voor meer informatie over meer Zie [toe te voegen met Log Analytics opgeslagen zoekopdrachten en waarschuwingen](../operations-management-suite/operations-management-suite-solutions-resources-searches-alerts.md); die bevat illustratieve voorbeelden, evenals de schemadetails van het.

### <a name="using-resource-template-via-apipowershell"></a>Met behulp van Resourcesjabloon via API/Powershell
De Log Analytics Alert REST-API is RESTful en zijn toegankelijk via de Azure Resource Manager REST API. De API kan dus worden benaderd vanaf een PowerShell-opdrachtregel en wordt de uitvoer zoekresultaten aan u in JSON-indeling, zodat u kunt de resultaten een programmatische manier op veel verschillende manieren gebruiken.

Meer informatie over [maken en beheren van regels voor waarschuwingen in Log Analytics met REST-API](../log-analytics/log-analytics-api-alerts.md), inclusief voorbeelden van toegang tot de API vanuit Powershell.

## <a name="managing-log-alert-on-application-insights"></a>Waarschuwing voor Application Insights beheren
Waarschuwingen voor Azure Application Insights zijn geïntroduceerd als onderdeel van de nieuwe Azure-waarschuwingen onder Azure Monitor. Daarom deze wordt uitgevoerd onder de API van Azure Monitor als [queryregels gepland](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules/) groep voor REST-bewerking.

### <a name="using-azure-resource-manager-template"></a>Met behulp van Azure Resource Manager-sjabloon
Waarschuwing voor Application Insights-resources heeft een type `Microsoft.Insights/scheduledQueryRules/`. Zie voor meer informatie over dit brontype [Azure Monitor - gepland Query regels API-verwijzing](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules/).

Hieronder volgt de structuur voor [queryregels gepland maken](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules/createorupdate) op basis van resource-sjabloon met de voorbeeld-gegevensset als variabelen.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0", 
    "parameters": {      
    },   
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
         "SeverityLevel": "4"
         },
      "alertTrigger":{
        "Operator":"GreaterThan",
        "Threshold":"1"
         },
       "actionGrp":{
        "ActionGroup": "/subscriptions/a123d7efg-123c-1234-5678-a12bc3defgh4/resourceGroups/myRG/providers/microsoft.insights/actiongroups/sampleAG",
        "Subject": "Customized Email Header",
        "Webhook": "{ \"alertname\":\"#alertrulename\", \"IncludeSearchResults\":true }"           
         }
  },
  "resources":[ {
    "name":"[variables('alertName')]",
    "type":"Microsoft.Insights/scheduledQueryRules",
    "apiVersion": "2018-04-16",
    "location": "[variables('alertLocation')]",
    "tags":{"[variables('alertTag')]": "Resource"},
    "properties":{
       "description": "[variables('alertDesription')]",
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
           "odata.type": "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.Microsoft.AppInsights.Nexus.DataContracts.Resources.ScheduledQueryRules.AlertingAction",
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
> Label-veld met verborgen-koppeling naar de doelresource is verplicht in het gebruik van [queryregels gepland ](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules/) sjabloon voor API-oproep of een resource. 

Het bovenstaande voorbeeld-json als (bijvoorbeeld) sampleScheduledQueryRule.json ten behoeve van dit scenario kunnen worden opgeslagen en kunnen worden geïmplementeerd met [Azure Resource Manager in Azure portal](../azure-resource-manager/resource-group-template-deploy-portal.md#deploy-resources-from-custom-template).

### <a name="using-resource-template-via-clipowershell"></a>Met behulp van Resourcesjabloon via CLI/Powershell
Azure Monitor - geplande regels API-Query is een REST-API en volledig compatibel met Azure Resource Manager REST API. Daarom kan deze worden gebruikt via Powershell met Resource Manager-cmdlet, evenals Azure CLI.

Hieronder gebruik via Azure Resource Manager PowerShell-cmdlet voor het voorbeeld hierboven Resourcesjabloon (sampleScheduledQueryRule.json) weergegeven:
```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName "myRG" -TemplateFile "D:\Azure\Templates\sampleScheduledQueryRule.json"
```
Hieronder gebruik via Azure Resource Manager-opdracht in de Azure CLI voor het voorbeeld hierboven Resourcesjabloon (sampleScheduledQueryRule.json) weergegeven:

```azurecli
az group deployment create --resource-group myRG --template-file sampleScheduledQueryRule.json
```
Op de goede werking 201 te maken van waarschuwingsregel nieuwe status wordt geretourneerd of 200 wordt geretourneerd als een bestaande waarschuwingsregel is gewijzigd.


## <a name="next-steps"></a>Volgende stappen
* Inzicht in [webhookacties voor logboekwaarschuwingen](monitor-alerts-unified-log-webhook.md)
* Meer informatie over de nieuwe [Azure-waarschuwingen](monitoring-overview-unified-alerts.md)
* Meer informatie over [Application Insights](../application-insights/app-insights-analytics.md)
* Meer informatie over [Log Analytics](../log-analytics/log-analytics-overview.md).   
