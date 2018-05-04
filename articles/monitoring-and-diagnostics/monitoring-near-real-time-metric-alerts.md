---
title: Nieuwere metrische waarschuwingen in de Azure-Monitor ondersteund resources | Microsoft Docs
description: De verwijzing op ondersteuning metrische gegevens en logboeken voor nieuwere Azure bijna realtime metrische waarschuwingen.
author: snehithm
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
ms.date: 04/27/2018
ms.author: snmuvva, vinagara
ms.custom: ''
ms.openlocfilehash: 6d440a49cb30210d3c0eed7d24e4811cc56925b9
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="newer-metric-alerts-for-azure-services-in-the-azure-portal"></a>Nieuwere metrische waarschuwingen voor Azure-services in de Azure portal
Monitor voor Azure ondersteunt nu een nieuwe metrische Waarschuwingstype. De nieuwere waarschuwingen verschillen van [klassieke metrische waarschuwingen](insights-alerts-portal.md) in een aantal manieren:

- **Verbeterde latentie**: nieuwere metrische waarschuwingen zo vaak als elke één minuut kunnen uitvoeren. Oudere metrische waarschuwingen wordt altijd uitgevoerd met een frequentie van 5 minuten. Logboek waarschuwingen nog steeds een langer is dan 1 minuut vertraging vanwege de tijd vergt om op te nemen van de logboeken is. 
- **Ondersteuning voor multidimensionale metrische gegevens**: U kunt een waarschuwing op dimensionale metrische gegevens, zodat u voor het bewaken van een alleen een interessante segment van de metrische gegevens. 
- **Meer controle over metrische voorwaarden**: U kunt uitgebreidere waarschuwingsregels definiëren. De nieuwere waarschuwingen bewaken van de maximale, minimale gemiddelde en totale waarden van de metrische gegevens ondersteunen. 
- **Bewaking van meerdere metrische gegevens gecombineerd**: U kunt meerdere metrische gegevens (momenteel maximaal twee metrische gegevens) met een enkele regel bewaken. Er wordt een waarschuwing gegeven als beide metrische gegevens in strijd is met hun respectieve drempelwaarden voor de opgegeven periode. 
- **Voor een betere notification system**: alle nieuwere waarschuwingen gebruiken [actiegroepen](monitoring-action-groups.md), die groepen meldingen en acties die opnieuw kunnen worden gebruikt in meerdere waarschuwingen worden genoemd. Gebruik geen actiegroepen klassieke metrische waarschuwingen en oudere Log Analytics-waarschuwingen. 
- **Metrische gegevens uit logboeken** (beperkte openbare preview): logboek bij Log Analytics-gegevens nu kunnen worden uitgepakt en geconverteerd worden naar Azure Monitor metrische gegevens en vervolgens een melding op net als andere metrische gegevens. 

Zie voor informatie over het maken van een nieuwere metrische waarschuwing in Azure portal, [geen waarschuwingsregel maken in de Azure portal](monitor-alerts-unified-usage.md#create-an-alert-rule-with-the-azure-portal). Na het maken, kunt u de waarschuwing beheren met behulp van de stappen in [uw waarschuwingen beheren in de Azure-portal](monitor-alerts-unified-usage.md#managing-your-alerts-in-azure-portal).


## <a name="portal-powershell-cli-rest-support"></a>Portal, PowerShell, CLI, REST-ondersteuning
Op dit moment kunt u waarschuwingen voor nieuwere metrische alleen in de Azure-portal [REST-API](https://docs.microsoft.com/en-us/azure/monitoring-and-diagnostics/monitoring-create-action-group-with-resource-manager-template) of [Resource Manager-sjablonen](monitoring-create-metric-alerts-with-templates.md). Ondersteuning voor het configureren van de nieuwere waarschuwingen met behulp van PowerShell en de Azure-opdrachtregelinterface (Azure CLI 2.0) is binnenkort beschikbaar.

## <a name="metrics-and-dimensions-supported"></a>Metrische gegevens en dimensies die worden ondersteund
Ondersteuning voor nieuwere metrische waarschuwingen waarschuwingen voor de metrische gegevens die gebruikmaken van dimensies. U kunt dimensies gebruiken voor het filteren van uw metric naar het juiste niveau. Alle ondersteunde metrische gegevens samen met de toepasselijke dimensies kunnen worden verkend en gevisualiseerd van [Azure Monitor - Metrics Explorer (Preview)](monitoring-metric-charts.md).

Hier volgt een volledige lijst met Azure monitor metrische bronnen die worden ondersteund door de nieuwere waarschuwingen:

|Resourcetype  |Dimensies ondersteund  | Metrische gegevens beschikbaar|
|---------|---------|----------------|
|Microsoft.ApiManagement/service     | Ja        | [API Management](monitoring-supported-metrics.md#microsoftapimanagementservice)|
|Microsoft.Automation/automationAccounts     |     Ja   | [Automation-Accounts](monitoring-supported-metrics.md#microsoftautomationautomationaccounts)|
|Microsoft.Batch/batchAccounts | N/A| [Batch-Accounts](monitoring-supported-metrics.md#microsoftbatchbatchaccounts)|
|Microsoft.Cache/Redis     |    N/A     |[Redis Cache](monitoring-supported-metrics.md#microsoftcacheredis)|
|Microsoft.Compute/virtualMachines     |    N/A     | [Virtuele machines](monitoring-supported-metrics.md#microsoftcomputevirtualmachines)|
|Microsoft.Compute/virtualMachineScaleSets     |   N/A      |[Virtuele-machineschaalsets](monitoring-supported-metrics.md#microsoftcomputevirtualmachinescalesets)|
|Microsoft.ContainerInstance/containerGroups | Ja| [Containergroepen](monitoring-supported-metrics.md#microsoftcontainerinstancecontainergroups)|
|Microsoft.DataFactory/datafactories| Ja| [Data Factory's V1](monitoring-supported-metrics.md#microsoftdatafactorydatafactories)|
|Microsoft.DataFactory/factories     |   Ja     |[Data Factory's V2](monitoring-supported-metrics.md#microsoftdatafactoryfactories)|
|Microsoft.DBforMySQL/servers     |   N/A      |[DB voor MySQL](monitoring-supported-metrics.md#microsoftdbformysqlservers)|
|Microsoft.DBforPostgreSQL/servers     |    N/A     | [DB voor PostgreSQL](monitoring-supported-metrics.md#microsoftdbforpostgresqlservers)|
|Microsoft.EventHub/namespaces     |  Ja      |[Event Hubs](monitoring-supported-metrics.md#microsofteventhubnamespaces)|
|Microsoft.KeyVault/vaults| Nee | [Kluizen](monitoring-supported-metrics.md#microsoftkeyvaultvaults)|
|Microsoft.Logic/workflows     |     N/A    |[Logic Apps](monitoring-supported-metrics.md#microsoftlogicworkflows) |
|Microsoft.Network/applicationGateways     |    N/A     | [Toepassingsgateways](monitoring-supported-metrics.md#microsoftnetworkapplicationgateways) |
|Microsoft.Network/dnsZones | N/A| [DNS-Zones](monitoring-supported-metrics.md#microsoftnetworkdnszones) |
|Microsoft.Network/loadBalancers (alleen voor standaard SKU's)| Ja| [Taakverdelers](monitoring-supported-metrics.md#microsoftnetworkloadbalancers) |
|Microsoft.Network/publicipaddresses     |  N/A       |[Openbare IP-adressen](monitoring-supported-metrics.md#microsoftnetworkpublicipaddresses)|
|Microsoft.PowerBIDedicated/capacities | N/A | [Capaciteit](monitoring-supported-metrics.md#microsoftpowerbidedicatedcapacities)|
|Microsoft.Search/searchServices     |   N/A      |[Search-services](monitoring-supported-metrics.md#microsoftsearchsearchservices)|
|Microsoft.ServiceBus/namespaces     |  Ja       |[Service Bus](monitoring-supported-metrics.md#microsoftservicebusnamespaces)|
|Microsoft.Storage/storageAccounts     |    Ja     | [Opslagaccounts](monitoring-supported-metrics.md#microsoftstoragestorageaccounts)|
|Microsoft.Storage/storageAccounts/services     |     Ja    | [BLOB-Services](monitoring-supported-metrics.md#microsoftstoragestorageaccountsblobservices), [Bestandsservices](monitoring-supported-metrics.md#microsoftstoragestorageaccountsfileservices), [Services in de wachtrij](monitoring-supported-metrics.md#microsoftstoragestorageaccountsqueueservices) en [tabel Services](monitoring-supported-metrics.md#microsoftstoragestorageaccountstableservices)|
|Microsoft.StreamAnalytics/streamingjobs     |  N/A       | [Stream Analytics](monitoring-supported-metrics.md#microsoftstreamanalyticsstreamingjobs)|
|Microsoft.CognitiveServices/accounts     |    N/A     | [Cognitive Services](monitoring-supported-metrics.md#microsoftcognitiveservicesaccounts)|
|Microsoft.OperationalInsights/workspaces (Preview) | Ja|[Log Analytics-werkruimten](#log-analytics-logs-as-metrics-for-alerting)|


## <a name="log-analytics-logs-as-metrics-for-alerting"></a>Log Analytics Logboeken als metrische gegevens voor waarschuwingen 

U kunt ook nieuwere metrische waarschuwingen van populaire logboekanalyse Logboeken als metrische gegevens als onderdeel van de metrische gegevens van Logboeken Preview hebt uitgepakt.  
- [Prestatiemeteritems](../log-analytics/log-analytics-data-sources-performance-counters.md) voor Windows en Linux-machines
- [Heartbeat-records voor de status van Agent](../operations-management-suite/oms-solution-agenthealth.md)
- [Updatebeheer](../operations-management-suite/oms-solution-update-management.md) records
 
> [!NOTE]
> Specifieke metrische gegevens en/of de dimensie wordt alleen weergegeven als de gegevens voor deze in gekozen periode bestaat. Deze metrische gegevens zijn beschikbaar voor klanten met werkruimten in VS-Oost, West-Centraal VS en West-Europa die zich hebben aangemeld voor de preview. Als u worden van een deel van deze preview wilt, zich aanmelden met behulp van [de enquête](https://aka.ms/MetricLogPreview).

De volgende lijst met logboekanalyse op basis van het logboek metrische bronnen wordt ondersteund:

Metrische naam meer informatie  |Dimensies ondersteund  | Type logboek  |
|---------|---------|---------|
|Average_Avg. Leestijd schijf     |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Windows-prestatiemeteritem      |
| Average_Avg. Schrijftijd schijf     |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Windows-prestatiemeteritem      |
| Wachtrijlengte voor schijf Average_Current   |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Windows-prestatiemeteritem      |
| Average_Disk Reads/sec    |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Windows-prestatiemeteritem      |
| Average_Disk Schijfoverdrachten per seconde    |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Windows-prestatiemeteritem      |
|   Average_ % vrije ruimte    |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Windows-prestatiemeteritem      |
| Average_Available megabytes (MB)     |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Windows-prestatiemeteritem      |
| Average_ percentage toegewezen Bytes In gebruik    |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Windows-prestatiemeteritem      |
| Average_Bytes ontvangen per seconde    |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Windows-prestatiemeteritem      |
|  Average_Bytes verzonden per seconde    |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Windows-prestatiemeteritem      |
|  Totaal aantal Average_Bytes per seconde    |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Windows-prestatiemeteritem      |
|  Average_ percentage processortijd    |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Windows-prestatiemeteritem      |
|   Wachtrijlengte van Average_Processor    |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Windows-prestatiemeteritem      |
|   Average_ percentage vrije Inodes   |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritem      |
|    Average_ % vrije ruimte   |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritem      |
|    Average_ percentage gebruikte Inodes  |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritem      |
|    Average_ percentage gebruikte ruimte   |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritem      |
|    Average_Disk gelezen Bytes per seconde    |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritem      |
|    Average_Disk Reads/sec |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritem      |
|    Average_Disk Schijfoverdrachten per seconde |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritem      |
|    Average_Disk geschreven Bytes per seconde   |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritem      |
|    Average_Disk schrijfbewerkingen per seconde    |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritem      |
|    Average_Free Megabytes |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritem      |
|    Average_Logical gelezen Bytes per seconde |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritem      |
|    Average_ percentage beschikbaar geheugen |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritem      |
|    Percentage beschikbare wisselruimte Average_ |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritem      |
|    Average_ percentage gebruikt geheugen  |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritem      |
|    Average_ percentage gebruikte wisselruimte  |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritem      |
|    Geheugen in megabytes Average_Available    |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritem      |
|    Average_Available megabytes voor wisselen  |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritem      |
|    Average_Page leesbewerkingen per seconde |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritem      |
|    Average_Page schrijfbewerkingen per seconde    |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritem      |
|    Average_Pages per seconde  |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritem      |
|    Average_Used megabytes voor wisselen ruimte |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritem      |
|    Average_Used geheugen in megabytes |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritem      |
|    Average_Total Bytes verzonden    |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritem      |
|    Average_Total Bytes ontvangen   |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritem      |
|    Average_Total Bytes    |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritem      |
|    Verzonden Average_Total pakketten  |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritem      |
|    Average_Total pakketten ontvangen |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritem      |
|    Average_Total Rx-fouten    |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritem      |
|    Average_Total Tx-fouten    |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritem      |
|    Average_Total conflicten   |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritem      |
|    Average_Avg. Leestijd schijf |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritem      |
|    Average_Avg. Schijfoverdrachten per seconde |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritem      |
|    Average_Avg. Schrijftijd schijf    |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritem      |
|    Average_Physical gelezen Bytes per seconde    |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritem      |
|    Tijd in beschermde modus Average_Pct    |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritem      |
|    Tijd in gebruikersmodus van Average_Pct  |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritem      |
|    Average_Used geheugen kB |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritem      |
|    Average_Virtual gedeeld geheugen  |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritem      |
|    Average_ percentage DPC-tijd |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritem      |
|    Average_ percentage niet-actieve tijd    |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritem      |
|    Average_ percentage Interrupt-tijd   |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritem      |
|    Average_ % i/o-wachttijd |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritem      |
|    Average_ percentage tijd in Nice    |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritem      |
|    Tijd in beschermde modus Average_ %  |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritem      |
|    Average_ percentage processortijd   |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritem      |
|    Average_ percentage tijd in gebruikersmodus    |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritem      |
|    Average_Free fysiek geheugen   |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritem      |
|    Average_Free ruimte in Wisselgeheugenbestanden |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritem      |
|    Average_Free virtueel geheugen    |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritem      |
|    Average_Processes  |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritem      |
|    Average_Size opgeslagen In Pagineringbestanden    |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritem      |
|    Average_Uptime |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritem      |
|    Average_Users  |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritem      |
|    Heartbeat  |     Ja - Computer, besturingssysteemtype, versie en SourceComputerId    |   Heartbeat-Records |
|    Update |     Ja - Computer, Product, classificatie, UpdateState, optionele & goedgekeurde    |   Updatebeheer |



## <a name="payload-schema"></a>De nettolading van schema

De POST-bewerking bevat de volgende JSON-nettolading en het schema voor alle bijna nieuwere metrische waarschuwingen wanneer een geconfigureerde [actiegroep](monitoring-action-groups.md) wordt gebruikt:

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
* Meer informatie over [waarschuwingen aanmelden voor Azure](monitor-alerts-unified-log.md).
* Meer informatie over [waarschuwingen in Azure](monitoring-overview-alerts.md).
