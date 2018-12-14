---
title: Ondersteunde resources voor metrische waarschuwingen in Azure Monitor
description: Referentie voor de ondersteuning voor metrische gegevens en logboeken voor metrische waarschuwingen in Azure Monitor
author: snehithm
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 06/29/2018
ms.author: snmuvva
ms.component: alerts
ms.openlocfilehash: 4a9f9c2592c7bf27e1caeb09dd492e4700768117
ms.sourcegitcommit: 85d94b423518ee7ec7f071f4f256f84c64039a9d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2018
ms.locfileid: "53383473"
---
# <a name="supported-resources-for-metric-alerts-in-azure-monitor"></a>Ondersteunde resources voor metrische waarschuwingen in Azure Monitor

Azure Monitor nu ondersteunt een [nieuwe metrische Waarschuwingstype](../azure-monitor/platform/alerts-overview.md) die heeft aanzienlijke voordelen boven de oudere [klassieke metrische waarschuwingen](../azure-monitor/platform/alerts-classic.overview.md). Metrische gegevens zijn beschikbaar voor [lange lijst met Azure-services](monitoring-supported-metrics.md). De nieuwere waarschuwingen ondersteunen slechts een subset (groeiende) van de resourcetypen. In dit artikel geeft een lijst van deze subset.


U kunt de nieuwere metrische waarschuwingen ook gebruiken voor populaire Log Analytics-Logboeken als metrische gegevens hebt uitgepakt. Raadpleeg voor meer informatie, [metrische waarschuwingen voor logboeken](../azure-monitor/platform/alerts-metric-logs.md).

## <a name="portal-powershell-cli-rest-support"></a>Portal, PowerShell, CLI, REST-ondersteuning
U kunt op dit moment nieuwere metrische waarschuwingen maken alleen in de Azure-portal [REST-API](https://docs.microsoft.com/rest/api/monitor/metricalerts/), of [Resource Manager-sjablonen](../azure-monitor/platform/alerts-metric-create-templates.md). Ondersteuning voor het configureren van de nieuwere waarschuwingen met behulp van PowerShell en Azure CLI versie 2.0 en hoger is binnenkort beschikbaar.

## <a name="metrics-and-dimensions-supported"></a>Metrische gegevens en dimensies ondersteund
Nieuwere metrische waarschuwingen ondersteunt waarschuwingen voor metrische gegevens die gebruikmaken van dimensies. U kunt dimensies gebruiken voor het filteren van uw metrische gegevens naar het juiste niveau. Alle ondersteunde metrische gegevens, samen met de toepasselijke dimensies kunnen worden verkend en gevisualiseerd van [Azure Monitor - Metrics Explorer](../azure-monitor/platform/metrics-charts.md).

Dit is de volledige lijst met Azure monitor metrische bronnen die worden ondersteund door de nieuwere waarschuwingen:

|Resourcetype  |Dimensies ondersteund  | Metrische gegevens beschikbaar|
|---------|---------|----------------|
|Microsoft.ApiManagement/service     | Ja        | [API Management](monitoring-supported-metrics.md#microsoftapimanagementservice)|
|Microsoft.Automation/automationAccounts     |     Ja   | [Automation-Accounts](monitoring-supported-metrics.md#microsoftautomationautomationaccounts)|
|Microsoft.Batch/batchAccounts | N/A| [Batch-Accounts](monitoring-supported-metrics.md#microsoftbatchbatchaccounts)|
|Microsoft.Cache/Redis     |    N/A     |[Azure Cache voor Redis](monitoring-supported-metrics.md#microsoftcacheredis)|
|Microsoft.CognitiveServices/accounts     |    N/A     | [Cognitive Services](monitoring-supported-metrics.md#microsoftcognitiveservicesaccounts)|
|Microsoft.Compute/virtualMachines     |    N/A     | [Virtuele machines](monitoring-supported-metrics.md#microsoftcomputevirtualmachines)|
|Microsoft.Compute/virtualMachineScaleSets     |   N/A      |[Virtuele-machineschaalsets](monitoring-supported-metrics.md#microsoftcomputevirtualmachinescalesets)|
|Microsoft.ContainerInstance/containerGroups | Ja| [Containergroepen](monitoring-supported-metrics.md#microsoftcontainerinstancecontainergroups)|
|Microsoft.ContainerService/managedClusters | Ja | [Beheerde Clusters](monitoring-supported-metrics.md#microsoftcontainerservicemanagedclusters)|
|Microsoft.DataFactory/datafactories| Ja| [Data Factory V1](monitoring-supported-metrics.md#microsoftdatafactorydatafactories)|
|Microsoft.DataFactory/factories     |   Ja     |[Data Factory V2](monitoring-supported-metrics.md#microsoftdatafactoryfactories)|
|Microsoft.DBforMySQL/servers     |   N/A      |[DB voor MySQL](monitoring-supported-metrics.md#microsoftdbformysqlservers)|
|Microsoft.DBforPostgreSQL/servers     |    N/A     | [DB voor PostgreSQL](monitoring-supported-metrics.md#microsoftdbforpostgresqlservers)|
|Microsoft.EventHub/namespaces     |  Ja      |[Event Hubs](monitoring-supported-metrics.md#microsofteventhubnamespaces)|
|Microsoft.KeyVault/vaults| Nee | [Kluizen](monitoring-supported-metrics.md#microsoftkeyvaultvaults)|
|Microsoft.Logic/workflows     |     N/A    |[Logic Apps](monitoring-supported-metrics.md#microsoftlogicworkflows) |
|Microsoft.Network/applicationGateways     |    N/A     | [Toepassingsgateways](monitoring-supported-metrics.md#microsoftnetworkapplicationgateways) |
|Microsoft.Network/expressRouteCircuits | N/A |  [Express Route-Circuits](monitoring-supported-metrics.md#microsoftnetworkexpressroutecircuits) |
|Microsoft.Network/dnsZones | N/A| [DNS-Zones](monitoring-supported-metrics.md#microsoftnetworkdnszones) |
|Microsoft.Network/loadBalancers (alleen voor standaard-SKU's)| Ja| [Load Balancers](monitoring-supported-metrics.md#microsoftnetworkloadbalancers) |
|Microsoft.Network/publicipaddresses     |  N/A       |[Openbare IP-adressen](monitoring-supported-metrics.md#microsoftnetworkpublicipaddresses)|
|Microsoft.PowerBIDedicated/capacities | N/A | [Capaciteit](monitoring-supported-metrics.md#microsoftpowerbidedicatedcapacities)|
|Microsoft.Network/trafficManagerProfiles | Ja | [Traffic Manager-profielen](monitoring-supported-metrics.md#microsoftnetworktrafficmanagerprofiles) |
|Microsoft.Search/searchServices     |   N/A      |[Search-services](monitoring-supported-metrics.md#microsoftsearchsearchservices)|
|Microsoft.ServiceBus/namespaces     |  Ja       |[Service Bus](monitoring-supported-metrics.md#microsoftservicebusnamespaces)|
|Microsoft.Storage/storageAccounts     |    Ja     | [Opslagaccounts](monitoring-supported-metrics.md#microsoftstoragestorageaccounts)|
|Microsoft.Storage/storageAccounts/services     |     Ja    | [BLOB-Services](monitoring-supported-metrics.md#microsoftstoragestorageaccountsblobservices), [Bestandsservices](monitoring-supported-metrics.md#microsoftstoragestorageaccountsfileservices), [Services in de wachtrij](monitoring-supported-metrics.md#microsoftstoragestorageaccountsqueueservices) en [tabel Services](monitoring-supported-metrics.md#microsoftstoragestorageaccountstableservices)|
|Microsoft.StreamAnalytics/streamingjobs     |  N/A       | [Stream Analytics](monitoring-supported-metrics.md#microsoftstreamanalyticsstreamingjobs)|
| Microsoft.Web/serverfarms | Ja | [App Service-plannen](monitoring-supported-metrics.md#microsoftwebserverfarms)  |
| Microsoft.Web/sites | Ja | [App Services](monitoring-supported-metrics.md#microsoftwebsites-excluding-functions) en [functies](monitoring-supported-metrics.md#microsoftwebsites-functions)|
| Microsoft.Web/sites/slots | Ja | [App Service-sleuven](monitoring-supported-metrics.md#microsoftwebsitesslots)|
|Microsoft.OperationalInsights/workspaces| Ja|[Log Analytics-werkruimten](monitoring-supported-metrics.md#microsoftoperationalinsightsworkspaces)|



## <a name="payload-schema"></a>De nettolading van schema

De POST-bewerking bevat de volgende JSON-nettolading en het schema voor alle in de buurt van nieuwere metrische waarschuwingen wanneer een op de juiste wijze geconfigureerde [actiegroep](../azure-monitor/platform/action-groups.md) wordt gebruikt:

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

* Meer informatie over de nieuwe [ervaring waarschuwingen](../azure-monitor/platform/alerts-overview.md).
* Meer informatie over [waarschuwingen voor activiteitenlogboeken in Azure](../azure-monitor/platform/alerts-unified-log.md).
* Meer informatie over [waarschuwingen in Azure](../azure-monitor/platform/alerts-overview.md).
