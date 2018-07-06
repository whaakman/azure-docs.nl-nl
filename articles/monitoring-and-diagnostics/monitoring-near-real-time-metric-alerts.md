---
title: Ondersteunde bronnen voor nieuwere metrische waarschuwingen van Azure Monitor
description: Referentie voor de ondersteuning voor metrische gegevens en logboeken voor nieuwere Azure bijna realtime metrische waarschuwingen.
author: snehithm
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 04/27/2018
ms.author: snmuvva
ms.component: alerts
ms.openlocfilehash: 01c0b5897ab47a2a5091646aed1977779cf0234c
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/06/2018
ms.locfileid: "37868017"
---
# <a name="supported-metrics-and-creation-methods-for-new-metric-alerts"></a>Ondersteunde metrische gegevens en het maken van methoden voor het nieuwe metrische waarschuwingen
Azure Monitor nu ondersteunt een [nieuwe metrische Waarschuwingstype](monitoring-overview-unified-alerts.md) die heeft aanzienlijke voordelen boven de oudere [klassieke metrische waarschuwingen](insights-alerts-portal.md). De oudere waarschuwingen ondersteunen een [grote overzicht van metrische gegevens](monitoring-supported-metrics.md). De nieuwere waarschuwingen ondersteuning voor een (groeiende) subset van die lijst. In dit artikel geeft een lijst van deze subset. 

## <a name="portal-powershell-cli-rest-support"></a>Portal, PowerShell, CLI, REST-ondersteuning
U kunt op dit moment nieuwere metrische waarschuwingen maken alleen in de Azure-portal [REST-API](https://docs.microsoft.com/en-us/rest/api/monitor/metricalerts/createorupdate) of [Resource Manager-sjablonen](monitoring-create-metric-alerts-with-templates.md). Ondersteuning voor het configureren van de nieuwere waarschuwingen met behulp van PowerShell en de Azure-opdrachtregelinterface (Azure CLI 2.0) is binnenkort beschikbaar.

## <a name="metrics-and-dimensions-supported"></a>Metrische gegevens en dimensies ondersteund
Nieuwere metrische waarschuwingen ondersteunt waarschuwingen voor metrische gegevens die gebruikmaken van dimensies. U kunt dimensies gebruiken voor het filteren van uw metrische gegevens naar het juiste niveau. Alle ondersteunde metrische gegevens, samen met de toepasselijke dimensies kunnen worden verkend en gevisualiseerd van [Azure Monitor - Metrics Explorer (Preview)](monitoring-metric-charts.md).

Dit is de volledige lijst met Azure monitor metrische bronnen die worden ondersteund door de nieuwere waarschuwingen:

|Resourcetype  |Dimensies ondersteund  | Metrische gegevens beschikbaar|
|---------|---------|----------------|
|Microsoft.ApiManagement/service     | Ja        | [API Management](monitoring-supported-metrics.md#microsoftapimanagementservice)|
|Microsoft.Automation/automationAccounts     |     Ja   | [Automation-Accounts](monitoring-supported-metrics.md#microsoftautomationautomationaccounts)|
|Microsoft.Batch/batchAccounts | N/A| [Batch-Accounts](monitoring-supported-metrics.md#microsoftbatchbatchaccounts)|
|Microsoft.Cache/Redis     |    N/A     |[Redis Cache](monitoring-supported-metrics.md#microsoftcacheredis)|
|Microsoft.Compute/virtualMachines     |    N/A     | [Virtuele machines](monitoring-supported-metrics.md#microsoftcomputevirtualmachines)|
|Microsoft.Compute/virtualMachineScaleSets     |   N/A      |[Virtuele-machineschaalsets](monitoring-supported-metrics.md#microsoftcomputevirtualmachinescalesets)|
|Microsoft.ContainerInstance/containerGroups | Ja| [Containergroepen](monitoring-supported-metrics.md#microsoftcontainerinstancecontainergroups)|
|Microsoft.DataFactory/datafactories| Ja| [Data Factory V1](monitoring-supported-metrics.md#microsoftdatafactorydatafactories)|
|Microsoft.DataFactory/factories     |   Ja     |[Data Factory V2](monitoring-supported-metrics.md#microsoftdatafactoryfactories)|
|Microsoft.DBforMySQL/servers     |   N/A      |[DB voor MySQL](monitoring-supported-metrics.md#microsoftdbformysqlservers)|
|Microsoft.DBforPostgreSQL/servers     |    N/A     | [DB voor PostgreSQL](monitoring-supported-metrics.md#microsoftdbforpostgresqlservers)|
|Microsoft.EventHub/namespaces     |  Ja      |[Event Hubs](monitoring-supported-metrics.md#microsofteventhubnamespaces)|
|Microsoft.KeyVault/vaults| Nee | [Kluizen](monitoring-supported-metrics.md#microsoftkeyvaultvaults)|
|Microsoft.Logic/workflows     |     N/A    |[Logic Apps](monitoring-supported-metrics.md#microsoftlogicworkflows) |
|Microsoft.Network/applicationGateways     |    N/A     | [Toepassingsgateways](monitoring-supported-metrics.md#microsoftnetworkapplicationgateways) |
|Microsoft.Network/dnsZones | N/A| [DNS-Zones](monitoring-supported-metrics.md#microsoftnetworkdnszones) |
|Microsoft.Network/loadBalancers (alleen voor standaard-SKU's)| Ja| [Load Balancers](monitoring-supported-metrics.md#microsoftnetworkloadbalancers) |
|Microsoft.Network/publicipaddresses     |  N/A       |[Openbare IP-adressen](monitoring-supported-metrics.md#microsoftnetworkpublicipaddresses)|
|Microsoft.PowerBIDedicated/capacities | N/A | [Capaciteit](monitoring-supported-metrics.md#microsoftpowerbidedicatedcapacities)|
|Microsoft.Search/searchServices     |   N/A      |[Search-services](monitoring-supported-metrics.md#microsoftsearchsearchservices)|
|Microsoft.ServiceBus/namespaces     |  Ja       |[Service Bus](monitoring-supported-metrics.md#microsoftservicebusnamespaces)|
|Microsoft.Storage/storageAccounts     |    Ja     | [Opslagaccounts](monitoring-supported-metrics.md#microsoftstoragestorageaccounts)|
|Microsoft.Storage/storageAccounts/services     |     Ja    | [BLOB-Services](monitoring-supported-metrics.md#microsoftstoragestorageaccountsblobservices), [Bestandsservices](monitoring-supported-metrics.md#microsoftstoragestorageaccountsfileservices), [Services in de wachtrij](monitoring-supported-metrics.md#microsoftstoragestorageaccountsqueueservices) en [tabel Services](monitoring-supported-metrics.md#microsoftstoragestorageaccountstableservices)|
|Microsoft.StreamAnalytics/streamingjobs     |  N/A       | [Stream Analytics](monitoring-supported-metrics.md#microsoftstreamanalyticsstreamingjobs)|
|Microsoft.CognitiveServices/accounts     |    N/A     | [Cognitive Services](monitoring-supported-metrics.md#microsoftcognitiveservicesaccounts)|
|Microsoft.OperationalInsights/workspaces (Preview) | Ja|[Log Analytics-werkruimten](#log-analytics-logs-as-metrics-for-alerting)|


## <a name="log-analytics-logs-as-metrics-for-alerting"></a>Log Analytics-Logboeken als metrische gegevens voor waarschuwingen 

U kunt de nieuwere metrische waarschuwingen ook gebruiken voor populaire Log Analytics-Logboeken als metrische gegevens als onderdeel van de metrische gegevens van de Preview-versie hebt uitgepakt.  
- [Prestatiemeteritems](../log-analytics/log-analytics-data-sources-performance-counters.md) voor Windows en Linux-machines
- [Heartbeat-records voor de status van Agent](../operations-management-suite/oms-solution-agenthealth.md)
- [Updatebeheer](../operations-management-suite/oms-solution-update-management.md) records
 
> [!NOTE]
> Specifieke metrische gegevens en/of de dimensie wordt alleen weergegeven als de gegevens voor deze in de gekozen periode bestaat. Deze metrische gegevens zijn beschikbaar voor klanten met toegang tot werkruimten in VS-Oost, West-Centraal VS en West-Europa die zijn aangemeld bij de Preview-versie. Als u wilt worden van een deel van deze Preview-versie, zich aanmelden met behulp van [de enquête](https://aka.ms/MetricLogPreview).

De volgende lijst met Log Analytics log gebaseerde metrische bronnen wordt ondersteund:

Metrische naam/gegevens  |Dimensies ondersteund  | Type logboek  |
|---------|---------|---------|
|Average_Avg. Schijf sec/lezen     |     Ja, Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Windows-prestatiemeteritem      |
| Average_Avg. Schijf sec/schrijven     |     Ja, Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Windows-prestatiemeteritem      |
| Average_Current wachtrijlengte voor schijf   |     Ja, Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Windows-prestatiemeteritem      |
| Average_Disk Reads/sec    |     Ja, Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Windows-prestatiemeteritem      |
| Average_Disk Schijfoverdrachten per seconde    |     Ja, Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Windows-prestatiemeteritem      |
|   Average_ % vrije ruimte    |     Ja, Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Windows-prestatiemeteritem      |
| Average_Available megabytes (MB)     |     Ja, Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Windows-prestatiemeteritem      |
| Average_ percentage toegewezen Bytes In gebruik    |     Ja, Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Windows-prestatiemeteritem      |
| Average_Bytes ontvangen per seconde    |     Ja, Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Windows-prestatiemeteritem      |
|  Average_Bytes verzonden per seconde    |     Ja, Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Windows-prestatiemeteritem      |
|  Average_Bytes in totaal/sec    |     Ja, Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Windows-prestatiemeteritem      |
|  Average_ % processortijd    |     Ja, Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Windows-prestatiemeteritem      |
|   Average_Processor-wachtrijlengte    |     Ja, Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Windows-prestatiemeteritem      |
|   Average_ percentage beschikbare Inodes   |     Ja, Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritems      |
|    Average_ % vrije ruimte   |     Ja, Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritems      |
|    Average_ percentage gebruikte Inodes  |     Ja, Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritems      |
|    Average_ percentage gebruikte ruimte   |     Ja, Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritems      |
|    Average_Disk gelezen Bytes per seconde    |     Ja, Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritems      |
|    Average_Disk Reads/sec |     Ja, Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritems      |
|    Average_Disk Schijfoverdrachten per seconde |     Ja, Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritems      |
|    Average_Disk geschreven Bytes per seconde   |     Ja, Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritems      |
|    Average_Disk schrijfbewerkingen per seconde    |     Ja, Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritems      |
|    Average_Free Megabytes |     Ja, Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritems      |
|    Average_Logical schijf Bytes per seconde |     Ja, Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritems      |
|    Average_ percentage beschikbaar geheugen |     Ja, Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritems      |
|    Percentage beschikbare wisselruimte Average_ |     Ja, Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritems      |
|    Average_ percentage gebruikt geheugen  |     Ja, Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritems      |
|    Average_ percentage gebruikte wisselruimte  |     Ja, Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritems      |
|    Average_Available megabytes (MB) geheugen    |     Ja, Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritems      |
|    Average_Available megabytes voor wisselen  |     Ja, Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritems      |
|    Average_Page leesbewerkingen per seconde |     Ja, Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritems      |
|    Average_Page schrijfbewerkingen per seconde    |     Ja, Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritems      |
|    Average_Pages/sec  |     Ja, Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritems      |
|    Average_Used megabytes voor wisselen ruimte |     Ja, Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritems      |
|    Average_Used geheugen in megabytes |     Ja, Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritems      |
|    Average_Total Bytes verzonden    |     Ja, Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritems      |
|    Average_Total Bytes ontvangen   |     Ja, Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritems      |
|    Average_Total Bytes    |     Ja, Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritems      |
|    Average_Total pakketten verzonden  |     Ja, Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritems      |
|    Average_Total pakketten ontvangen |     Ja, Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritems      |
|    Average_Total Rx-fouten    |     Ja, Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritems      |
|    Average_Total Tx-fouten    |     Ja, Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritems      |
|    Average_Total conflicten   |     Ja, Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritems      |
|    Average_Avg. Schijf sec/lezen |     Ja, Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritems      |
|    Average_Avg. Schijfoverdrachten per seconde |     Ja, Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritems      |
|    Average_Avg. Schijf sec/schrijven    |     Ja, Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritems      |
|    Average_Physical schijf Bytes per seconde    |     Ja, Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritems      |
|    Tijd in beschermde modus Average_Pct    |     Ja, Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritems      |
|    Tijd in gebruikersmodus van Average_Pct  |     Ja, Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritems      |
|    Average_Used geheugen kB |     Ja, Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritems      |
|    Average_Virtual gedeeld geheugen  |     Ja, Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritems      |
|    Average_ percentage DPC-tijd |     Ja, Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritems      |
|    Average_ % niet-actieve tijd    |     Ja, Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritems      |
|    Average_ percentage Interrupt-tijd   |     Ja, Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritems      |
|    Average_ % i/o-wachttijd |     Ja, Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritems      |
|    Tijd in Nice Average_    |     Ja, Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritems      |
|    Average_ % gemachtigde tijd  |     Ja, Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritems      |
|    Average_ % processortijd   |     Ja, Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritems      |
|    Average_ percentage tijd in gebruikersmodus    |     Ja, Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritems      |
|    Average_Free fysiek geheugen   |     Ja, Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritems      |
|    Average_Free ruimte in Wisselgeheugenbestanden |     Ja, Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritems      |
|    Average_Free virtueel geheugen    |     Ja, Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritems      |
|    Average_Processes  |     Ja, Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritems      |
|    Average_Size opgeslagen In Pagineringbestanden    |     Ja, Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritems      |
|    Average_Uptime |     Ja, Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritems      |
|    Average_Users  |     Ja, Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritems      |
|    Heartbeat  |     Ja, Computer, OSType, versie en SourceComputerId    |   Heartbeat-Records |
|    Update |     Ja, Computer, Product, classificatie, UpdateState, optionele & goedgekeurde    |   Updatebeheer |



## <a name="payload-schema"></a>De nettolading van schema

De POST-bewerking bevat de volgende JSON-nettolading en het schema voor alle in de buurt van nieuwere metrische waarschuwingen wanneer een op de juiste wijze geconfigureerde [actiegroep](monitoring-action-groups.md) wordt gebruikt:

```json
{"schemaId":"AzureMonitorMetricAlert","data":
    {
    "version": "2.0",
    "status": "Activated",
    "context": {
    "timestamp": "2018-02-28T10:44:10.1714014Z",
    "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/Contoso/providers/microsoft.insights/metricAlerts/StorageCheck",
    "name": "StorageCheck",
    "description": "",
    "conditionType": "SingleResourceMultipleMetricCriteria",
    "condition": {
      "windowSize": "PT5M",
      "allOf": [
        {
          "metricName": "Transactions",
          "dimensions": [
            {
              "name": "AccountResourceId",
              "value": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/Contoso/providers/Microsoft.Storage/storageAccounts/diag500"
            },
            {
              "name": "GeoType",
              "value": "Primary"
            }
          ],
          "operator": "GreaterThan",
          "threshold": "0",
          "timeAggregation": "PT5M",
          "metricValue": 1.0
        },
      ]
    },
    "subscriptionId": "00000000-0000-0000-0000-000000000000",
    "resourceGroupName": "Contoso",
    "resourceName": "diag500",
    "resourceType": "Microsoft.Storage/storageAccounts",
    "resourceId": "/subscriptions/1e3ff1c0-771a-4119-a03b-be82a51e232d/resourceGroups/Contoso/providers/Microsoft.Storage/storageAccounts/diag500",
    "portalLink": "https://portal.azure.com/#resource//subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/Contoso/providers/Microsoft.Storage/storageAccounts/diag500"
  },
        "properties": {
                "key1": "value1",
                "key2": "value2"
        }
    }
}
```

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over de nieuwe [ervaring waarschuwingen](monitoring-overview-unified-alerts.md).
* Meer informatie over [waarschuwingen voor activiteitenlogboeken in Azure](monitor-alerts-unified-log.md).
* Meer informatie over [waarschuwingen in Azure](monitoring-overview-alerts.md).
