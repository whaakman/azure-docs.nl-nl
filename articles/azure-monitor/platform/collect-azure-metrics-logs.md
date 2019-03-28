---
title: Azure-service-logboeken en metrische gegevens verzamelen in Log Analytics-werkruimte | Microsoft Docs
description: Diagnostische gegevens configureren op Azure-resources om Logboeken en metrische gegevens schrijven naar Log Analytics-werkruimte in Azure Monitor.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: 84105740-3697-4109-bc59-2452c1131bfe
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 04/12/2017
ms.author: magoedte
ms.openlocfilehash: 5a619b768d61875a03e53a613dfb9a3fb01dd7aa
ms.sourcegitcommit: cf971fe82e9ee70db9209bb196ddf36614d39d10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/27/2019
ms.locfileid: "58540173"
---
# <a name="collect-azure-service-logs-and-metrics-into-log-analytics-workspace-in-azure-monitor"></a>Azure-service-logboeken en metrische gegevens verzamelen in Log Analytics-werkruimte in Azure Monitor

Er zijn vier verschillende manieren van het verzamelen van Logboeken en metrische gegevens voor Azure-services:

1. Azure diagnostics rechtstreeks naar Log Analytics-werkruimte in Azure Monitor (*Diagnostics* in de volgende tabel)
2. Azure diagnostics naar Azure-opslag naar Log Analytics-werkruimte in Azure Monitor (*opslag* in de volgende tabel)
3. Connectors voor Azure-services (*Connectors* in de volgende tabel)
4. Scripts voor het verzamelen en vervolgens boek gegevens in Log Analytics-werkruimte in Azure Monitor (lege cellen in de volgende tabel en voor services die niet zijn opgenomen)


| Service                 | Resourcetype                           | Logboeken        | Metrische gegevens     | Oplossing |
| --- | --- | --- | --- | --- |
| Toepassingsgateways    | Microsoft.Network/applicationGateways   | Diagnostiek | Diagnostiek | [Azure Application Gateway Analytics](../../azure-monitor/insights/azure-networking-analytics.md#azure-application-gateway-analytics-solution-in-log-analytics) |
| Application insights    |                                         | Connector   | Connector   | [Application Insights-Connector](https://blogs.technet.microsoft.com/msoms/2016/09/26/application-insights-connector-in-oms/) (Preview) |
| Automation-accounts     | Microsoft.Automation/AutomationAccounts | Diagnostiek |             | [Meer informatie](../../automation/automation-manage-send-joblogs-log-analytics.md)|
| Batch-accounts          | Microsoft.Batch/batchAccounts           | Diagnostiek | Diagnostiek | |
| Klassieke cloudservices  |                                         | Storage     |             | [Meer informatie](azure-storage-iis-table.md) |
| Cognitieve services      | Microsoft.CognitiveServices/accounts    |             | Diagnostiek | |
| Data Lake analytics     | Microsoft.DataLakeAnalytics/accounts    | Diagnostiek |             | |
| Data Lake store         | Microsoft.DataLakeStore/accounts        | Diagnostiek |             | |
| Event hub-naamruimte     | Microsoft.EventHub/namespaces           | Diagnostiek | Diagnostiek | |
| IoT Hubs                | Microsoft.Devices/IotHubs               |             | Diagnostiek | |
| Key Vault               | Microsoft.KeyVault/vaults               | Diagnostiek |             | [KeyVault Analytics](../../azure-monitor/insights/azure-key-vault.md) |
| Load balancers          | Microsoft.Network/loadBalancers         | Diagnostiek |             |  |
| Logic Apps              | Microsoft.Logic/workflows <br> Microsoft.Logic/integrationAccounts | Diagnostiek | Diagnostiek | |
| Netwerkbeveiligingsgroepen | Microsoft.Network/networksecuritygroups | Diagnostiek |             | [Azure Network Security Group Analytics](../../azure-monitor/insights/azure-networking-analytics.md#azure-network-security-group-analytics-solution-in-log-analytics) |
| Recovery-kluizen         | Microsoft.RecoveryServices/vaults       |             |             | [Azure Recovery Services-analyse (Preview)](https://github.com/krnese/AzureDeploy/blob/master/OMS/MSOMS/Solutions/recoveryservices/)|
| Services zoeken         | Microsoft.Search/searchServices         | Diagnostiek | Diagnostiek | |
| Service Bus-naamruimte   | Microsoft.ServiceBus/namespaces         | Diagnostiek | Diagnostiek | [Service Bus-analyse (Preview)](https://github.com/Azure/azure-quickstart-templates/tree/master/oms-servicebus-solution)|
| Service Fabric          |                                         | Storage     |             | [Service Fabric-analyse (Preview)](../../service-fabric/service-fabric-diagnostics-oms-setup.md) |
| SQL (v12)               | Microsoft.Sql/servers/databases <br> Microsoft.Sql/servers/elasticPools |             | Diagnostiek | [Azure SQL Analytics (Preview)](../../azure-monitor/insights/azure-sql.md) |
| Storage                 |                                         |             | Script      | [Azure Storage Analytics (Preview)](https://github.com/Azure/azure-quickstart-templates/tree/master/oms-azure-storage-analytics-solution) |
| Virtuele machines        | Microsoft.Compute/virtualMachines       | Toestelnummer   | Toestelnummer <br> Diagnostiek  | |
| Schaalsets voor virtuele Machines | Microsoft.Compute/virtualMachines <br> Microsoft.Compute/virtualMachineScaleSets/virtualMachines |             | Diagnostiek | |
| Web Server-farms        | Microsoft.Web/serverfarms               |             | Diagnostiek | |
| Web Sites               | Microsoft.Web/sites <br> Microsoft.Web/sites/slots |             | Diagnostiek | [Azure Web Apps Analytics (Preview)](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-web-apps-analytics) |


> [!NOTE]
> Voor het bewaken van virtuele machines van Azure (Linux en Windows), het beste installeren de [Log Analytics VM-extensie](../../azure-monitor/learn/quick-collect-azurevm.md). De agent biedt u inzicht te krijgen die in uw virtuele machines zijn verzameld uit. U kunt ook de extensie voor virtuele-machineschaalsets.
>
>

## <a name="azure-diagnostics-direct-to-log-analytics"></a>Azure diagnostics rechtstreeks naar Log Analytics
Veel Azure-resources kunnen schrijven logboeken met diagnostische gegevens en metrische gegevens rechtstreeks naar een Log Analytics-werkruimte in Azure Monitor, en dit is de beste manier van het verzamelen van de gegevens voor analyse. Bij het gebruik van Azure diagnostics gegevens onmiddellijk worden geschreven naar de werkruimte en hoeft niet eerst de gegevens schrijven naar opslag.

Azure-resources die ondersteuning bieden voor [Azure monitor](../../azure-monitor/overview.md) hun logboeken en metrische gegevens rechtstreeks naar een Log Analytics-werkruimte kunt verzenden.

> [!NOTE]
> Multi-dimensionale metrische gegevens verzenden naar een Log Analytics-werkruimte hebt via diagnostische instellingen wordt momenteel niet ondersteund. Metrische gegevens met dimensies worden geëxporteerd als platte eendimensionale metrische gegevens, als totaal van alle dimensiewaarden.
>
> *Bijvoorbeeld*: De metriek 'Binnenkomende berichten' voor een Event Hub kan worden verkend en uitgezet op wachtrijniveau. Echter, als geëxporteerd via diagnostische instellingen voor die de metrische gegevens wordt weergegeven als alle binnenkomende berichten voor alle wachtrijen in de Event Hub.
>
>

* Raadpleeg voor de details van de beschikbare metrische gegevens, [ondersteunde metrische gegevens met Azure Monitor](../../azure-monitor/platform/metrics-supported.md).
* Raadpleeg voor de details van de beschikbare logboeken, [ondersteunde services en -schema voor diagnostische logboeken](../../azure-monitor/platform/diagnostic-logs-schema.md).

### <a name="enable-diagnostics-with-powershell"></a>Diagnosefunctie inschakelen met PowerShell

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

De volgende PowerShell-voorbeeld ziet u hoe u [Set AzDiagnosticSetting](/powershell/module/Az.Monitor/Set-AzDiagnosticSetting) om in te schakelen van diagnostische gegevens op een netwerkbeveiligingsgroep. Diezelfde aanpak werkt voor alle ondersteunde resources - instellen `$resourceId` naar de resource-id van de resource die u wilt inschakelen, diagnostische gegevens voor.

```powershell
$workspaceId = "/subscriptions/d2e37fee-1234-40b2-5678-0b2199de3b50/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/rollingbaskets"

$resourceId = "/SUBSCRIPTIONS/ec11ca60-1234-491e-5678-0ea07feae25c/RESOURCEGROUPS/DEMO/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/DEMO"

Set-AzDiagnosticSetting -ResourceId $ResourceId  -WorkspaceId $workspaceId -Enabled $true
```

### <a name="enable-diagnostics-with-resource-manager-templates"></a>Diagnosefunctie inschakelen met Resource Manager-sjablonen

Om in te schakelen van diagnostische gegevens op een resource als deze is gemaakt en de diagnostische gegevens naar u naar uw Log Analytics-werkruimte verzonden, kunnen een sjabloon die vergelijkbaar is met de onderstaande gebruiken. In dit voorbeeld is voor een Automation-account, maar werkt voor alle ondersteunde resourcetypen.

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

[!INCLUDE [log-analytics-troubleshoot-azure-diagnostics](../../../includes/log-analytics-troubleshoot-azure-diagnostics.md)]

## <a name="azure-diagnostics-to-storage-then-to-log-analytics"></a>Azure diagnostics naar opslag vervolgens naar Log Analytics

Voor het verzamelen van Logboeken van binnen enkele resources, is het mogelijk om de logboeken verzenden naar Azure storage en configureer vervolgens de Log Analytics-werkruimte de om Logboeken te lezen uit de opslag.

Azure Monitor kunt u deze aanpak gebruiken voor het verzamelen van diagnostische gegevens van Azure storage voor de volgende resources en de logboeken:

| Resource | Logboeken |
| --- | --- |
| Service Fabric |ETWEvent <br> Operationele gebeurtenissen <br> Reliable Actor-gebeurtenis <br> Betrouwbare Servicegebeurtenis |
| Virtuele machines |Linux Syslog <br> Windows-gebeurtenis <br> IIS-logboek <br> Windows ETWEvent |
| Webrollen <br> Werkrollen |Linux Syslog <br> Windows-gebeurtenis <br> IIS-logboek <br> Windows ETWEvent |

> [!NOTE]
> Normale Azure gegevenstarieven voor opslag en transacties in rekening gebracht wanneer u diagnostische gegevens verzenden naar een opslagaccount en voor wanneer de Log Analytics-werkruimte de gegevens van uw opslagaccount leest.
>
>

Zie [blob storage gebruiken voor IIS en table storage voor gebeurtenissen](azure-storage-iis-table.md) voor meer informatie over hoe Azure Monitor deze logboeken kunt verzamelen.

## <a name="connectors-for-azure-services"></a>Connectors voor Azure-services

Er is een connector voor Application Insights, waarmee de gegevens die zijn verzameld door Application Insights wordt verzonden naar Log Analytics-werkruimte.

Meer informatie over de [Application Insights-connector](https://blogs.technet.microsoft.com/msoms/2016/09/26/application-insights-connector-in-oms/).

## <a name="scripts-to-collect-and-post-data-to-log-analytics-workspace"></a>Scripts voor het verzamelen en boeken van gegevens naar Log Analytics-werkruimte

U kunt een Azure Automation-script gebruiken voor het verzamelen van Logboeken en metrische gegevens voor Azure-services die bieden een directe manier om te verzenden van Logboeken en metrische gegevens naar Log Analytics-werkruimte. Het script vervolgens de gegevens kunt verzenden naar de werkruimte met behulp van de [gegevensverzamelaar-API](../../azure-monitor/platform/data-collector-api.md)

De galerie met Azure-sjabloon is [voorbeelden van het gebruik van Azure Automation](https://azure.microsoft.com/resources/templates/?term=OMS) voor het verzamelen van gegevens van services en deze verzenden naar Azure Monitor.

## <a name="next-steps"></a>Volgende stappen

* [Blob storage gebruiken voor IIS en table storage voor gebeurtenissen](azure-storage-iis-table.md) lezen van de logboeken voor Azure-services die diagnosefuncties schrijven voor de table-opslag of IIS-logboeken geschreven naar de blob storage.
* [Oplossingen inschakelen](../../azure-monitor/insights/solutions.md) om inzicht in de gegevens te bieden.
* [Gebruik van zoekquery's](../../azure-monitor/log-query/log-query-overview.md) om de gegevens te analyseren.
