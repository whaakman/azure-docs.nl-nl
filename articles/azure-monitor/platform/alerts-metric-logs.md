---
title: Metrische waarschuwingen maken voor logboeken in Azure Monitor
description: Zelfstudie over het bijna realtime metrische waarschuwingen maken op de populaire log analytics-gegevens.
author: msvijayn
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/17/2018
ms.author: vinagara
ms.subservice: alerts
ms.openlocfilehash: ac4391b91d818b21e392e134115294fb84473e69
ms.sourcegitcommit: 9b6492fdcac18aa872ed771192a420d1d9551a33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54449645"
---
# <a name="create-metric-alerts-for-logs-in-azure-monitor"></a>Metrische waarschuwingen maken voor logboeken in Azure Monitor

## <a name="overview"></a>Overzicht

Azure Monitor ondersteunt [metrische Waarschuwingstype](../../azure-monitor/platform/alerts-metric-near-real-time.md) welke voordelen heeft via de [klassieke waarschuwingen](../../azure-monitor/platform/alerts-classic-portal.md). Metrische gegevens zijn beschikbaar voor [lange lijst met Azure-services](../../azure-monitor/platform/metrics-supported.md). Dit artikel wordt uitgelegd voor het gebruik van een subset (dat wil zeggen) voor de resource - `Microsoft.OperationalInsights/workspaces`.

U kunt metrische waarschuwingen voor populaire Log Analytics-Logboeken als metrische gegevens als onderdeel van de metrische gegevens van Logboeken, met inbegrip van resources in Azure of on-premises hebt uitgepakt. Hieronder vindt u de ondersteunde Log Analytics-oplossingen:

- [Prestatiemeteritems](../../azure-monitor/platform/data-sources-performance-counters.md) voor Windows en Linux-machines
- [Heartbeat-records voor de status van Agent](../../azure-monitor/insights/solution-agenthealth.md)
- [Updatebeheer](../../automation/automation-update-management.md) records
- [Gebeurtenisgegevens](../../azure-monitor/platform/data-sources-windows-events.md) Logboeken

Er zijn veel voordelen voor het gebruik van **metrische waarschuwingen voor logboeken** op basis van de [Logboekwaarschuwingen](../../azure-monitor/platform/alerts-log.md) in Azure; sommige hiervan worden hieronder vermeld:

- Metrische waarschuwingen bieden in de buurt van real-time bewaking van functionaliteit en waarschuwingen voor metrische gegevens voor logboeken splitsingen gegevens van bron om ervoor te zorgen dat hetzelfde logboek.
- Metrische waarschuwingen zijn stateful - alleen melden eenmaal wanneer de waarschuwing wordt geactiveerd en eenmaal wanneer de waarschuwing is opgelost; in plaats van waarschuwingen, die zijn staatloos en houden geactiveerd op basis van elke interval als de waarschuwing voorwaarde wordt voldaan.
- Metrische waarschuwingen voor logboek bieden meerdere dimensies, zodat het filteren op specifieke waarden, zoals Computers, het Type besturingssysteem, enzovoort eenvoudiger; zonder de noodzaak voor query's in analytics penning.

> [!NOTE]
> Specifieke metrische gegevens en/of de dimensie wordt alleen weergegeven als de gegevens voor deze in de gekozen periode bestaat. Deze metrische gegevens zijn beschikbaar voor klanten met Azure Log Analytics-werkruimten.

## <a name="metrics-and-dimensions-supported-for-logs"></a>Metrische gegevens en dimensies ondersteund voor logboeken

 metrische waarschuwingen ondersteunt waarschuwingen voor metrische gegevens die gebruikmaken van dimensies. U kunt dimensies gebruiken voor het filteren van uw metrische gegevens naar het juiste niveau. De volledige lijst met metrische gegevens die worden ondersteund voor logboeken van [Log Analytics-werkruimten](../../azure-monitor/platform/metrics-supported.md#microsoftoperationalinsightsworkspaces) wordt vermeld, op ondersteunde oplossingen.

> [!NOTE]
> Om weer te geven van ondersteunde metrische gegevens voor die worden geëxtraheerd uit Log Analytics-werkruimte via [Azure controleren - metrische gegevens](../../azure-monitor/platform/metrics-charts.md); een metrische waarschuwing voor het logboek moet worden gemaakt voor de genoemde metrische gegevens. De gekozen in metrische waarschuwingen voor logboeken - dimensies worden alleen weergegeven voor verkenning via Azure Monitor - metrische gegevens.

## <a name="creating-metric-alert-for-log-analytics"></a>Waarschuwing voor metrische gegevens maken voor Log Analytics

Metrische gegevens van populaire Logboeken is doorgesluisd voordat deze wordt verwerkt in Log Analytics, in Azure controleren - metrische gegevens. Hiermee kunnen gebruikers de mogelijkheden van de metrische platform, evenals de waarschuwing voor metrische gegevens - met inbegrip van die waarschuwingen met frequentie zo laag 1 minuut.
Hieronder ziet u de middelen voor het samenstellen van een waarschuwing voor metrische gegevens voor Logboeken.

## <a name="prerequisites-for-metric-alert-for-logs"></a>Vereisten voor de waarschuwing voor metrische gegevens voor logboeken

Voordat u metrische gegevens voor logboeken verzameld op Log Analytics data werkt, moet het volgende worden ingesteld en beschikbaar is:

1. **Active Log Analytics Workspace**: Een geldig en actief Log Analytics-werkruimte moet aanwezig zijn. Zie voor meer informatie, [een Log Analytics-werkruimte maken in Azure portal](../../azure-monitor/learn/quick-create-workspace.md).
2. **Agent is geconfigureerd voor Log Analytics-werkruimte**: Agent moet worden geconfigureerd voor virtuele Azure-machines (en/of) On-premises VM's om gegevens te verzenden naar de Log Analytics-werkruimte in de vorige stap gebruikt. Zie voor meer informatie, [Log Analytics - overzicht](../../azure-monitor/platform/agents-overview.md).
3. **Ondersteunde Log Analytics-oplossingen is geïnstalleerd**: Log Analytics-oplossing moet worden geconfigureerd en verzenden van gegevens in Log Analytics-werkruimte - ondersteunde oplossingen zijn [prestatiemeteritems voor Windows en Linux](../../azure-monitor/platform/data-sources-performance-counters.md), [Heartbeat-records voor de status van Agent](../../azure-monitor/insights/solution-agenthealth.md) , [Updatebeheer, en [gebeurtenisgegevens](../../azure-monitor/platform/data-sources-windows-events.md).
4. **Meld u Analytics-oplossingen die zijn geconfigureerd voor het verzenden van logboeken**: Log Analytics-oplossing moet de vereiste Logboeken/gegevens die overeenkomt met [metrische gegevens die worden ondersteund voor Log Analytics-werkruimten](../../azure-monitor/platform/metrics-supported.md#microsoftoperationalinsightsworkspaces) ingeschakeld. Bijvoorbeeld: voor *% beschikbaar geheugen* teller hiervan moet worden geconfigureerd in [prestatiemeteritems](../../azure-monitor/platform/data-sources-performance-counters.md) oplossing eerste.

## <a name="configuring-metric-alert-for-logs"></a>Waarschuwing voor metrische gegevens voor logboeken configureren

 metrische waarschuwingen kunnen worden gemaakt en beheerd via de Azure portal, Resource Manager-sjablonen, REST-API, PowerShell en Azure CLI. Waarschuwingen voor Logboeken, metrische gegevens is een variant van metrische waarschuwingen - wanneer u klaar bent met de vereisten, kan waarschuwing voor metrische gegevens voor logboeken worden gemaakt voor de opgegeven Log Analytics-werkruimte. Alle eigenschappen en functionaliteiten van [metrische waarschuwingen](../../azure-monitor/platform/alerts-metric-near-real-time.md) van toepassing is op metrische waarschuwingen voor Logboeken, evenals, inclusief nettolading schema, de quotalimieten voor van toepassing en worden gefactureerd prijs.

Zie voor stapsgewijze details en voorbeelden van - [maken en beheren van metrische waarschuwingen](https://aka.ms/createmetricalert). Met name voor metrische waarschuwingen voor logboeken - Volg de instructies voor het beheren van metrische waarschuwingen en controleer het volgende:

- Doel voor waarschuwing voor metrische gegevens een geldig is *Log Analytics-werkruimte*
- Signaal gekozen voor waarschuwing voor metrische gegevens voor geselecteerde *Log Analytics-werkruimte* is van het type **metrische gegevens**
- Filteren op specifieke voorwaarden of resource met behulp van dimensiefilters. metrische gegevens voor logboeken zijn multi-dimensionale
- Bij het configureren van *Signaallogica*, één waarschuwing kan worden gemaakt voor het bereik van meerdere waarden van de dimensie (bijvoorbeeld Computer)
- Als **niet** met behulp van Azure portal voor het maken van de waarschuwing voor metrische gegevens voor geselecteerde *Log Analytics-werkruimte*; vervolgens handmatig moet de gebruiker eerst maken een expliciete regel voor het converteren van logboekgegevens naar een metrische waarde met behulp van [Azure Monitor - geplande queryregels](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules).

> [!NOTE]
> Bij het maken van de waarschuwing voor metrische gegevens voor Log Analytics-werkruimte via Azure portal - regel voor het converteren van logboekgegevens naar metrische gegevens via overeenkomt [Azure Monitor - queryregels gepland](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) wordt automatisch gemaakt op de achtergrond,  *zonder de noodzaak van een tussenkomst van de gebruiker of een actie*. Zie voor waarschuwing voor metrische gegevens voor het maken van de logboeken met gebruikmaking van dan Azure portal, [Resource-sjabloon voor metrische waarschuwingen voor logboeken](#resource-template-for-metric-alerts-for-logs) sectie over de wijze van voorbeeld van het maken van een logboek ScheduledQueryRule op basis van metrische conversie-regel voor waarschuwing voor metrische gegevens maken - anders zal er geen gegevens voor de waarschuwing voor metrische gegevens op Logboeken die zijn gemaakt.

## <a name="resource-template-for-metric-alerts-for-logs"></a>Resource-sjabloon voor metrische waarschuwingen voor logboeken

Zoals eerder gezegd, is het proces voor het maken van metrische waarschuwingen van Logboeken twee dingen tegelijk:

1. Maak een regel voor het extraheren van metrische gegevens van ondersteunde logboeken scheduledQueryRule-API gebruiken
2. Maken van een metrische waarschuwing voor metrische gegevens die zijn geëxtraheerd uit logboekbestanden (in stap 1) en Log Analytics-werkruimte als een doelbron

### <a name="metric-alerts-for-logs-with-static-threshold"></a>Metrische waarschuwingen voor logboeken met statische drempelwaarde

Als u wilt bereiken hetzelfde, kunt gebruiken in het voorbeeld hieronder - Azure Resource Manager-sjabloon waarbij het maken van een waarschuwing voor metrische gegevens van statische drempelwaarde is afhankelijk van is gemaakt van de regel voor het extraheren van metrische gegevens uit logboeken via scheduledQueryRule.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "convertRuleName": {
            "type": "string",
            "minLength": 1,
            "metadata": {
                "description": "Name of the rule to convert log to metric"
            }
        },
        "convertRuleDescription": {
            "type": "string",
            "minLength": 1,
            "metadata": {
                "description": "Description for log converted to metric"
            }
        },
        "convertRuleRegion": {
            "type": "string",
            "minLength": 1,
            "metadata": {
                "description": "Name of the region used by workspace"
            }
        },
        "convertRuleStatus": {
            "type": "string",
            "defaultValue": "true",
            "metadata": {
                "description": "Specifies whether the log conversion rule is enabled"
            }
        },
        "convertRuleMetric": {
            "type": "string",
            "minLength": 1,
            "metadata": {
                "description": "Name of the metric once extraction done from logs."
            }
        },
        "alertName": {
            "type": "string",
            "minLength": 1,
            "metadata": {
                "description": "Name of the alert"
            }
        },
        "alertDescription": {
            "type": "string",
            "defaultValue": "This is a metric alert",
            "metadata": {
                "description": "Description of alert"
            }
        },
        "alertSeverity": {
            "type": "int",
            "defaultValue": 3,
            "allowedValues": [
                0,
                1,
                2,
                3,
                4
            ],
            "metadata": {
                "description": "Severity of alert {0,1,2,3,4}"
            }
        },
        "isEnabled": {
            "type": "bool",
            "defaultValue": true,
            "metadata": {
                "description": "Specifies whether the alert is enabled"
            }
        },
        "resourceId": {
            "type": "string",
            "minLength": 1,
            "metadata": {
                "description": "Full Resource ID of the resource emitting the metric that will be used for the comparison. For example /subscriptions/00000000-0000-0000-0000-0000-00000000/resourceGroups/ResourceGroupName/providers/Microsoft.compute/virtualMachines/VM_xyz"
            }
        },
        "metricName": {
            "type": "string",
            "minLength": 1,
            "metadata": {
                "description": "Name of the metric used in the comparison to activate the alert."
            }
        },
        "operator": {
            "type": "string",
            "defaultValue": "GreaterThan",
            "allowedValues": [
                "Equals",
                "NotEquals",
                "GreaterThan",
                "GreaterThanOrEqual",
                "LessThan",
                "LessThanOrEqual"
            ],
            "metadata": {
                "description": "Operator comparing the current value with the threshold value."
            }
        },
        "threshold": {
            "type": "string",
            "defaultValue": "0",
            "metadata": {
                "description": "The threshold value at which the alert is activated."
            }
        },
        "timeAggregation": {
            "type": "string",
            "defaultValue": "Average",
            "allowedValues": [
                "Average",
                "Minimum",
                "Maximum",
                "Total"
            ],
            "metadata": {
                "description": "How the data that is collected should be combined over time."
            }
        },
        "windowSize": {
            "type": "string",
            "defaultValue": "PT5M",
            "metadata": {
                "description": "Period of time used to monitor alert activity based on the threshold. Must be between five minutes and one day. ISO 8601 duration format."
            }
        },
        "evaluationFrequency": {
            "type": "string",
            "defaultValue": "PT1M",
            "metadata": {
                "description": "how often the metric alert is evaluated represented in ISO 8601 duration format"
            }
        },
        "actionGroupId": {
            "type": "string",
            "defaultValue": "",
            "metadata": {
                "description": "The ID of the action group that is triggered when the alert is activated or deactivated"
            }
        }
    },
    "variables": {
        "convertRuleTag": "hidden-link:/subscriptions/1234-56789-1234-567a/resourceGroups/resourceGroupName/providers/Microsoft.OperationalInsights/workspaces/workspaceName",
        "convertRuleSourceWorkspace": {
            "SourceId": "/subscriptions/1234-56789-1234-567a/resourceGroups/resourceGroupName/providers/Microsoft.OperationalInsights/workspaces/workspaceName"
        }
    },
    "resources": [
        {
            "name": "[parameters('convertRuleName')]",
            "type": "Microsoft.Insights/scheduledQueryRules",
            "apiVersion": "2018-04-16",
            "location": "[parameters('convertRuleRegion')]",
            "tags": {
                "[variables('convertRuleTag')]": "Resource"
            },
            "properties": {
                "description": "[parameters('convertRuleDescription')]",
                "enabled": "[parameters('convertRuleStatus')]",
                "source": {
                    "dataSourceId": "[variables('convertRuleSourceWorkspace').SourceId]"
                },
                "action": {
                    "odata.type": "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.Microsoft.AppInsights.Nexus.DataContracts.Resources.ScheduledQueryRules.LogToMetricAction",
                    "criteria": [{
                            "metricName": "[parameters('convertRuleMetric')]",
                            "dimensions": []
                        }
                    ]
                }
            }
        },
        {
            "name": "[parameters('alertName')]",
            "type": "Microsoft.Insights/metricAlerts",
            "location": "global",
            "apiVersion": "2018-03-01",
            "tags": {},
            "dependsOn":["[resourceId('Microsoft.Insights/scheduledQueryRules',parameters('convertRuleName'))]"],
            "properties": {
                "description": "[parameters('alertDescription')]",
                "severity": "[parameters('alertSeverity')]",
                "enabled": "[parameters('isEnabled')]",
                "scopes": ["[parameters('resourceId')]"],
                "evaluationFrequency":"[parameters('evaluationFrequency')]",
                "windowSize": "[parameters('windowSize')]",
                "criteria": {
                    "odata.type": "Microsoft.Azure.Monitor.SingleResourceMultipleMetricCriteria",
                    "allOf": [
                        {
                            "name" : "1st criterion",
                            "metricName": "[parameters('metricName')]",
                            "dimensions":[],
                            "operator": "[parameters('operator')]",
                            "threshold" : "[parameters('threshold')]",
                            "timeAggregation": "[parameters('timeAggregation')]"
                        }
                    ]
                },
                "actions": [
                    {
                        "actionGroupId": "[parameters('actionGroupId')]"
                    }
                ]
            }
        }
    ]
}
```

Stel dat u de bovenstaande JSON wordt opgeslagen als metricfromLogsAlertStatic.json - en deze kan worden gecombineerd met een JSON-bestand van de parameter voor het maken van Resource-sjabloon op basis van. Een voorbeeld van parameter JSON-bestand vindt u hieronder:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "convertRuleName": {
            "value": "TestLogtoMetricRule" 
        },
        "convertRuleDescription": {
            "value": "Test rule to extract metrics from logs via template"
        },
        "convertRuleRegion": {
            "value": "West Central US"
        },
        "convertRuleStatus": {
            "value": "true"
        },
        "convertRuleMetric": {
            "value": "Average_% Idle Time"
        },
        "alertName": {
            "value": "TestMetricAlertonLog"
        },
        "alertDescription": {
            "value": "New multi-dimensional metric alert created via template"
        },
        "alertSeverity": {
            "value":3
        },
        "isEnabled": {
            "value": true
        },
        "resourceId": {
            "value": "/subscriptions/1234-56789-1234-567a/resourceGroups/myRG/providers/Microsoft.OperationalInsights/workspaces/workspaceName"
        },
        "metricName":{
            "value": "Average_% Idle Time"
        },
        "operator": {
            "value": "GreaterThan"
        },
        "threshold":{
            "value": "1"
        },
        "timeAggregation":{
            "value": "Average"
        },
        "actionGroupId": {
            "value": "/subscriptions/1234-56789-1234-567a/resourceGroups/myRG/providers/microsoft.insights/actionGroups/actionGroupName"
        }
    }
}
```

Ervan uitgaande dat het bovenstaande parameterbestand wordt opgeslagen als metricfromLogsAlertStatic.parameters.json; een waarschuwing voor metrische gegevens voor logboeken met behulp van kunt maken [Resource-sjabloon voor het maken in Azure portal](../../azure-resource-manager/resource-group-template-deploy-portal.md).

U kunt ook kunt een ook de volgende Azure Powershell-opdracht gebruiken:

```PowerShell
New-AzureRmResourceGroupDeployment -ResourceGroupName "myRG" -TemplateFile metricfromLogsAlertStatic.json TemplateParameterFile metricfromLogsAlertStatic.parameters.json
```

Of gebruik implementeren met behulp van Azure CLI-Resourcesjabloon:

```CLI
az group deployment create --resource-group myRG --template-file metricfromLogsAlertStatic.json --parameters @metricfromLogsAlertStatic.parameters.json
```

### <a name="metric-alerts-for-logs-with-dynamic-thresholds"></a>Metrische waarschuwingen voor logboeken met dynamische drempelwaarden

Als u wilt bereiken hetzelfde zijn, kunt een het voorbeeld hieronder - Azure Resource Manager-sjabloon gebruiken waar het maken van een waarschuwing voor metrische gegevens van dynamische drempelwaarden is afhankelijk van is gemaakt van de regel voor het extraheren van metrische gegevens uit logboeken via scheduledQueryRule.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "convertRuleName": {
            "type": "string",
            "minLength": 1,
            "metadata": {
                "description": "Name of the rule to convert log to metric"
            }
        },
        "convertRuleDescription": {
            "type": "string",
            "minLength": 1,
            "metadata": {
                "description": "Description for log converted to metric"
            }
        },
        "convertRuleRegion": {
            "type": "string",
            "minLength": 1,
            "metadata": {
                "description": "Name of the region used by workspace"
            }
        },
        "convertRuleStatus": {
            "type": "string",
            "defaultValue": "true",
            "metadata": {
                "description": "Specifies whether the log conversion rule is enabled"
            }
        },
        "convertRuleMetric": {
            "type": "string",
            "minLength": 1,
            "metadata": {
                "description": "Name of the metric once extraction done from logs."
            }
        },
        "alertName": {
            "type": "string",
            "minLength": 1,
            "metadata": {
                "description": "Name of the alert"
            }
        },
        "alertDescription": {
            "type": "string",
            "defaultValue": "This is a metric alert",
            "metadata": {
                "description": "Description of alert"
            }
        },
        "alertSeverity": {
            "type": "int",
            "defaultValue": 3,
            "allowedValues": [
                0,
                1,
                2,
                3,
                4
            ],
            "metadata": {
                "description": "Severity of alert {0,1,2,3,4}"
            }
        },
        "isEnabled": {
            "type": "bool",
            "defaultValue": true,
            "metadata": {
                "description": "Specifies whether the alert is enabled"
            }
        },
        "resourceId": {
            "type": "string",
            "minLength": 1,
            "metadata": {
                "description": "Full Resource ID of the resource emitting the metric that will be used for the comparison. For example /subscriptions/00000000-0000-0000-0000-0000-00000000/resourceGroups/ResourceGroupName/providers/Microsoft.compute/virtualMachines/VM_xyz"
            }
        },
        "metricName": {
            "type": "string",
            "minLength": 1,
            "metadata": {
                "description": "Name of the metric used in the comparison to activate the alert."
            }
        },
        "operator": {
            "type": "string",
            "defaultValue": "GreaterOrLessThan",
            "allowedValues": [
                "GreaterThan",
                "LessThan",
                "GreaterOrLessThan"
            ],
            "metadata": {
                "description": "Operator comparing the current value with the threshold value."
            }
        },
        "alertSensitivity": {
            "type": "string",
            "defaultValue": "Medium",
            "allowedValues": [
                "High",
                "Medium",
                "Low"
            ],
            "metadata": {
                "description": "Tunes how 'noisy' the Dynamic Thresholds alerts will be: 'High' will result in more alerts while 'Low' will result in fewer alerts."
            }
        },
        "numberOfEvaluationPeriods": {
            "type": "string",
            "defaultValue": "4",
            "metadata": {
                "description": "The number of periods to check in the alert evaluation."
            }
        },
        "minFailingPeriodsToAlert": {
            "type": "string",
            "defaultValue": "3",
            "metadata": {
                "description": "The number of unhealthy periods to alert on (must be lower or equal to numberOfEvaluationPeriods)."
            }
        },
        "timeAggregation": {
            "type": "string",
            "defaultValue": "Average",
            "allowedValues": [
                "Average",
                "Minimum",
                "Maximum",
                "Total"
            ],
            "metadata": {
                "description": "How the data that is collected should be combined over time."
            }
        },
        "windowSize": {
            "type": "string",
            "defaultValue": "PT5M",
            "metadata": {
                "description": "Period of time used to monitor alert activity based on the threshold. Must be between five minutes and one day. ISO 8601 duration format."
            }
        },
        "evaluationFrequency": {
            "type": "string",
            "defaultValue": "PT1M",
            "metadata": {
                "description": "how often the metric alert is evaluated represented in ISO 8601 duration format"
            }
        },
        "actionGroupId": {
            "type": "string",
            "defaultValue": "",
            "metadata": {
                "description": "The ID of the action group that is triggered when the alert is activated or deactivated"
            }
        }
    },
    "variables": {
        "convertRuleTag": "hidden-link:/subscriptions/1234-56789-1234-567a/resourceGroups/resourceGroupName/providers/Microsoft.OperationalInsights/workspaces/workspaceName",
        "convertRuleSourceWorkspace": {
            "SourceId": "/subscriptions/1234-56789-1234-567a/resourceGroups/resourceGroupName/providers/Microsoft.OperationalInsights/workspaces/workspaceName"
        }
    },
    "resources": [
        {
            "name": "[parameters('convertRuleName')]",
            "type": "Microsoft.Insights/scheduledQueryRules",
            "apiVersion": "2018-04-16",
            "location": "[parameters('convertRuleRegion')]",
            "tags": {
                "[variables('convertRuleTag')]": "Resource"
            },
            "properties": {
                "description": "[parameters('convertRuleDescription')]",
                "enabled": "[parameters('convertRuleStatus')]",
                "source": {
                    "dataSourceId": "[variables('convertRuleSourceWorkspace').SourceId]"
                },
                "action": {
                    "odata.type": "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.Microsoft.AppInsights.Nexus.DataContracts.Resources.ScheduledQueryRules.LogToMetricAction",
                    "criteria": [{
                            "metricName": "[parameters('convertRuleMetric')]",
                            "dimensions": []
                        }
                    ]
                }
            }
        },
        {
            "name": "[parameters('alertName')]",
            "type": "Microsoft.Insights/metricAlerts",
            "location": "global",
            "apiVersion": "2018-03-01",
            "tags": {},
            "dependsOn":["[resourceId('Microsoft.Insights/scheduledQueryRules',parameters('convertRuleName'))]"],
            "properties": {
                "description": "[parameters('alertDescription')]",
                "severity": "[parameters('alertSeverity')]",
                "enabled": "[parameters('isEnabled')]",
                "scopes": ["[parameters('resourceId')]"],
                "evaluationFrequency":"[parameters('evaluationFrequency')]",
                "windowSize": "[parameters('windowSize')]",
                "criteria": {
                    "odata.type": "Microsoft.Azure.Monitor.MultipleResourceMultipleMetricCriteria",
                    "allOf": [
                        {
                            "criterionType": "DynamicThresholdCriterion",
                            "name" : "1st criterion",
                            "metricName": "[parameters('metricName')]",
                            "dimensions":[],
                            "operator": "[parameters('operator')]",
                            "alertSensitivity": "[parameters('alertSensitivity')]",
                            "failingPeriods": {
                                "numberOfEvaluationPeriods": "[parameters('numberOfEvaluationPeriods')]",
                                "minFailingPeriodsToAlert": "[parameters('minFailingPeriodsToAlert')]"
                            },
                            "timeAggregation": "[parameters('timeAggregation')]"
                        }
                    ]
                },
                "actions": [
                    {
                        "actionGroupId": "[parameters('actionGroupId')]"
                    }
                ]
            }
        }
    ]
}
```

Stel dat u de bovenstaande JSON wordt opgeslagen als metricfromLogsAlertDynamic.json - en deze kan worden gecombineerd met een JSON-bestand van de parameter voor het maken van Resource-sjabloon op basis van. Een voorbeeld van parameter JSON-bestand vindt u hieronder:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "convertRuleName": {
            "value": "TestLogtoMetricRule"
        },
        "convertRuleDescription": {
            "value": "Test rule to extract metrics from logs via template"
        },
        "convertRuleRegion": {
            "value": "West Central US"
        },
        "convertRuleStatus": {
            "value": "true"
        },
        "convertRuleMetric": {
            "value": "Average_% Idle Time"
        },
        "alertName": {
            "value": "TestMetricAlertonLog"
        },
        "alertDescription": {
            "value": "New multi-dimensional metric alert created via template"
        },
        "alertSeverity": {
            "value":3
        },
        "isEnabled": {
            "value": true
        },
        "resourceId": {
            "value": "/subscriptions/1234-56789-1234-567a/resourceGroups/myRG/providers/Microsoft.OperationalInsights/workspaces/workspaceName"
        },
        "metricName":{
            "value": "Average_% Idle Time"
        },
        "operator": {
            "value": "GreaterOrLessThan"
          },
          "alertSensitivity": {
              "value": "Medium"
          },
          "numberOfEvaluationPeriods": {
              "value": "4"
          },
          "minFailingPeriodsToAlert": {
              "value": "3"
          },
        "timeAggregation":{
            "value": "Average"
        },
        "actionGroupId": {
            "value": "/subscriptions/1234-56789-1234-567a/resourceGroups/myRG/providers/microsoft.insights/actionGroups/actionGroupName"
        }
    }
}
```

Ervan uitgaande dat het bovenstaande parameterbestand wordt opgeslagen als metricfromLogsAlertDynamic.parameters.json; een waarschuwing voor metrische gegevens voor logboeken met behulp van kunt maken [Resource-sjabloon voor het maken in Azure portal](../../azure-resource-manager/resource-group-template-deploy-portal.md).

U kunt ook kunt een ook de volgende Azure Powershell-opdracht gebruiken:

```PowerShell
New-AzureRmResourceGroupDeployment -ResourceGroupName "myRG" -TemplateFile metricfromLogsAlertDynamic.json TemplateParameterFile metricfromLogsAlertDynamic.parameters.json
```

Of gebruik implementeren met behulp van Azure CLI-Resourcesjabloon:

```CLI
az group deployment create --resource-group myRG --template-file metricfromLogsAlertDynamic.json --parameters @metricfromLogsAlertDynamic.parameters.json
```

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over de [metrische waarschuwingen](alerts-metric.md).
- Meer informatie over [waarschuwingen voor activiteitenlogboeken in Azure](../../azure-monitor/platform/alerts-unified-log.md).
- Meer informatie over [waarschuwingen in Azure](alerts-overview.md).
