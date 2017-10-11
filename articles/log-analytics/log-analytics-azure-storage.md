---
title: Azure service-logboeken en metrische gegevens verzamelen voor Log Analytics | Microsoft Docs
description: Diagnostische gegevens configureren op Azure-resources voor het schrijven van Logboeken en metrische gegevens met logboekanalyse.
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: 84105740-3697-4109-bc59-2452c1131bfe
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/12/2017
ms.author: magoedte
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 7a3785e39f0d1cf849dbbf0d83d89eaed58c5b0b
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="collect-azure-service-logs-and-metrics-for-use-in-log-analytics"></a>Verzamelen van Logboeken van de Azure-service en metrische gegevens voor gebruik in Log Analytics

Er zijn vier verschillende manieren van het verzamelen van Logboeken en metrische gegevens voor Azure-services:

1. Azure diagnostics direct met logboekanalyse (*Diagnostics* in de volgende tabel)
2. Azure diagnostics naar Azure-opslag met logboekanalyse (*opslag* in de volgende tabel)
3. Connectors voor Azure-services (*Connectors* in de volgende tabel)
4. Scripts voor het verzamelen en vervolgens postgegevens in logboekanalyse (lege cellen in de volgende tabel en voor services die niet worden weergegeven)


| Service                 | Resourcetype                           | Logboeken        | Metrische gegevens     | Oplossing |
| --- | --- | --- | --- | --- |
| Toepassingsgateways    | Microsoft.Network/applicationGateways   | Diagnostiek | Diagnostiek | [Azure Application Gateway Analytics](log-analytics-azure-networking-analytics.md#azure-application-gateway-analytics-solution-in-log-analytics) |
| Application insights    |                                         | Connector   | Connector   | [Application Insights-Connector](https://blogs.technet.microsoft.com/msoms/2016/09/26/application-insights-connector-in-oms/) (Preview) |
| Automation-accounts     | Microsoft.Automation/AutomationAccounts | Diagnostiek |             | [Meer informatie](../automation/automation-manage-send-joblogs-log-analytics.md)|
| Batch-accounts          | Microsoft.Batch/batchAccounts           | Diagnostiek | Diagnostiek | |
| Klassieke cloudservices  |                                         | Storage     |             | [Meer informatie](log-analytics-azure-storage-iis-table.md) |
| Cognitive Services      | Microsoft.CognitiveServices/accounts    |             | Diagnostiek | |
| Data Lake analytics     | Microsoft.DataLakeAnalytics/accounts    | Diagnostiek |             | |
| Data Lake store         | Microsoft.DataLakeStore/accounts        | Diagnostiek |             | |
| Event Hub-naamruimte     | Microsoft.EventHub/namespaces           | Diagnostiek | Diagnostiek | |
| IoT-Hubs                | Microsoft.Devices/IotHubs               |             | Diagnostiek | |
| Key Vault               | Microsoft.KeyVault/vaults               | Diagnostiek |             | [KeyVault Analytics](log-analytics-azure-key-vault.md) |
| Taakverdelers          | Microsoft.Network/loadBalancers         | Diagnostiek |             |  |
| Logic Apps              | Microsoft.Logic/workflows <br> Microsoft.Logic/integrationAccounts | Diagnostiek | Diagnostiek | |
| Netwerkbeveiligingsgroepen | Microsoft.Network/networksecuritygroups | Diagnostiek |             | [Netwerkbeveiligingsgroep Azure Analytics](log-analytics-azure-networking-analytics.md#azure-network-security-group-analytics-solution-in-log-analytics) |
| Recovery kluizen         | Microsoft.RecoveryServices/vaults       |             |             | [Azure Recovery Services-Analytics (Preview)](https://github.com/krnese/AzureDeploy/blob/master/OMS/MSOMS/Solutions/recoveryservices/)|
| Services zoeken         | Microsoft.Search/searchServices         | Diagnostiek | Diagnostiek | |
| Service Bus-naamruimte   | Microsoft.ServiceBus/namespaces         | Diagnostiek | Diagnostiek | [Service Bus Analytics (Preview)](https://github.com/Azure/azure-quickstart-templates/tree/master/oms-servicebus-solution)|
| Service Fabric          |                                         | Storage     |             | [Service Fabric Analytics (Preview)](log-analytics-service-fabric.md) |
| SQL (v12)               | Microsoft.Sql/servers/databases <br> Microsoft.Sql/servers/elasticPools |             | Diagnostiek | [Azure SQL Analytics (Preview)](log-analytics-azure-sql.md) |
| Storage                 |                                         |             | Script      | [Azure Storage Analytics (Preview)](https://github.com/Azure/azure-quickstart-templates/tree/master/oms-azure-storage-analytics-solution) |
| Virtuele machines        | Microsoft.Compute/virtualMachines       | Toestelnummer   | Toestelnummer <br> Diagnostiek  | |
| Virtuele Machines-schaalsets | Microsoft.Compute/virtualMachines <br> Microsoft.Compute/virtualMachineScaleSets/virtualMachines |             | Diagnostiek | |
| Webserver-farms        | Microsoft.Web/serverfarms               |             | Diagnostiek | |
| Web Sites               | Microsoft.Web/sites <br> Microsoft.Web/sites/slots |             | Diagnostiek | [Analytics met Azure-Web-Apps (Preview)](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.AzureWebAppsAnalyticsOMS?tab=Overview) |


> [!NOTE]
> Voor het bewaken van virtuele machines in Azure (Linux en Windows), wordt aangeraden installeren van de [Log Analytics VM-extensie](log-analytics-azure-vm-extension.md). De agent biedt u inzicht verzameld van binnen uw virtuele machines. U kunt ook de uitbreiding voor de virtuele-machineschaalsets gebruiken.
>
>

## <a name="azure-diagnostics-direct-to-log-analytics"></a>Azure diagnostics rechtstreeks naar Log Analytics
Veel Azure-resources kunnen schrijven logboeken met diagnostische gegevens en metrische gegevens rechtstreeks naar het Log Analytics en dit is de beste manier om de gegevens voor analyse te verzamelen. Bij gebruik van Azure diagnostics gegevens onmiddellijk worden geschreven met logboekanalyse en hoeft niet de gegevens eerst naar opslag schrijven.

Azure-resources die ondersteuning bieden voor [Azure monitor](../monitoring-and-diagnostics/monitoring-overview.md) hun logboeken en metrische gegevens rechtstreeks naar het Log Analytics kunt verzenden.

* Raadpleeg voor de details van de beschikbare metrische gegevens, [ondersteund met een Azure-Monitor](../monitoring-and-diagnostics/monitoring-supported-metrics.md).
* Raadpleeg voor de details van de beschikbare logboeken [ondersteund services en het schema voor diagnostische logboeken](../monitoring-and-diagnostics/monitoring-diagnostic-logs-schema.md).

### <a name="enable-diagnostics-with-powershell"></a>Diagnosefunctie inschakelen met PowerShell
U moet de November 2016 (v2.3.0) of later release van [Azure PowerShell](/powershell/azure/overview).

Het volgende PowerShell-voorbeeld ziet u hoe u [Set-AzureRmDiagnosticSetting](/powershell/module/azurerm.insights/set-azurermdiagnosticsetting) om in te schakelen van diagnostische gegevens op een netwerkbeveiligingsgroep. Dezelfde manier werkt voor alle ondersteunde resources - ingesteld `$resourceId` naar de resource-id van de resource die u wilt inschakelen van diagnostische gegevens voor.

```powershell
$workspaceId = "/subscriptions/d2e37fee-1234-40b2-5678-0b2199de3b50/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/rollingbaskets"

$resourceId = "/SUBSCRIPTIONS/ec11ca60-1234-491e-5678-0ea07feae25c/RESOURCEGROUPS/DEMO/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/DEMO"

Set-AzureRmDiagnosticSetting -ResourceId $ResourceId  -WorkspaceId $workspaceId -Enabled $true
```

### <a name="enable-diagnostics-with-resource-manager-templates"></a>Diagnostische gegevens met Resource Manager-sjablonen inschakelen

Voor het inschakelen van diagnostische gegevens van een resource als deze is gemaakt en de diagnostische gegevens naar de werkruimte voor logboekanalyse u verzonden hebben kunnen een sjabloon die lijkt op de onderstaande kunnen gebruiken. In dit voorbeeld is voor een Automation-account, maar werkt voor alle ondersteunde resourcetypen.

```json
        {
            "type": "Microsoft.Automation/automationAccounts/providers/diagnosticSettings",
            "name": "[concat(parameters('omsAutomationAccountName'), '/', 'Microsoft.Insights/service')]",
            "apiVersion": "2015-07-01",
            "dependsOn": [
                "[concat('Microsoft.Automation/automationAccounts/', parameters('omsAutomationAccountName'))]",
                "[concat('Microsoft.OperationalInsights/workspaces/', parameters('omsWorkspaceName'))]"
            ],
            "properties": {
                "workspaceId": "[resourceId('Microsoft.OperationalInsights/workspaces', parameters('omsWorkspaceName'))]",
                "logs": [
                    {
                        "category": "JobLogs",
                        "enabled": true
                    },
                    {
                        "category": "JobStreams",
                        "enabled": true
                    }
                ]
            }
        }
```

[!INCLUDE [log-analytics-troubleshoot-azure-diagnostics](../../includes/log-analytics-troubleshoot-azure-diagnostics.md)]

## <a name="azure-diagnostics-to-storage-then-to-log-analytics"></a>Azure diagnostics naar opslag vervolgens naar het Log Analytics

Voor het verzamelen van Logboeken van binnen enkele informatiebronnen die mogelijk is het mogelijk om de logboeken verzenden met Azure-opslag en configureer vervolgens Log Analytics om te lezen van de logboeken van de opslag.

Log Analytics kunt u deze benadering gebruiken voor het verzamelen van diagnostische gegevens naar Azure storage voor de volgende bronnen en de logboeken:

| Resource | Logboeken |
| --- | --- |
| Service Fabric |ETWEvent <br> Operationele gebeurtenissen <br> Betrouwbare Actor-gebeurtenis <br> Betrouwbare Service gebeurtenis |
| Virtuele machines |Linux Syslog <br> Windows-gebeurtenis <br> IIS-logboek <br> Windows ETWEvent |
| Web-rollen <br> Werkrollen |Linux Syslog <br> Windows-gebeurtenis <br> IIS-logboek <br> Windows ETWEvent |

> [!NOTE]
> U kunt de normale Azure gegevenskosten voor opslag en transacties wanneer u diagnostische gegevens naar een opslagaccount verzenden en voor wanneer logboekanalyse de gegevens van uw opslagaccount leest worden aangerekend.
>
>

Zie [gebruik blob storage voor IIS en tabel opslag voor gebeurtenissen](log-analytics-azure-storage-iis-table.md) voor meer informatie over hoe logboekanalyse deze logboeken kan verzamelen.

## <a name="connectors-for-azure-services"></a>Connectors voor Azure-services

Er is een connector voor Application Insights, waarmee gegevens verzameld door Application Insights wordt verzonden naar logboekanalyse.

Meer informatie over de [Application Insights-connector](https://blogs.technet.microsoft.com/msoms/2016/09/26/application-insights-connector-in-oms/).

## <a name="scripts-to-collect-and-post-data-to-log-analytics"></a>Scripts voor het verzamelen en postgegevens met Log Analytics

Voor Azure-services die geen bieden een directe manier logboeken en metrische gegevens verzenden naar Log Analytics kunt u een Azure Automation-script gebruiken voor het verzamelen van het logboek en metrische gegevens. Het script vervolgens de gegevens kunt verzenden naar het Log Analytics met behulp van de [gegevensverzamelaar API](log-analytics-data-collector-api.md)

De galerie van Azure-sjabloon is [voorbeelden van het gebruik van Azure Automation](https://azure.microsoft.com/en-us/resources/templates/?term=OMS) gegevens te verzamelen van services en te verzenden naar logboekanalyse.

## <a name="next-steps"></a>Volgende stappen

* [Blob storage gebruiken voor de opslag van IIS en de tabel voor gebeurtenissen](log-analytics-azure-storage-iis-table.md) lezen van de logboeken voor voor Azure-services die schrijven diagnostische gegevens naar table storage of de IIS-logboeken geschreven naar de blob storage.
* [Inschakelen van oplossingen](log-analytics-add-solutions.md) te bieden inzicht in de gegevens.
* [Gebruik zoekquery's](log-analytics-log-searches.md) om de gegevens te analyseren.
