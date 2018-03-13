---
title: Near-realtime metrische waarschuwingen in de Azure-Monitor | Microsoft Docs
description: Informatie over het gebruik van near realtime metrische waarschuwingen voor het bewaken van metrische gegevens voor Azure-resource met een frequentie van 1 minuut zo klein.
author: snehithm
manager: kmadnani1
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/26/2018
ms.author: snmuvva, vinagara
ms.custom: 
ms.openlocfilehash: 88995b1f3350fe485e28efccc93779ae0a42eb97
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/12/2018
---
# <a name="near-real-time-metric-alerts-preview"></a>Bijna realtime metrische waarschuwingen (preview)
Monitor voor Azure biedt ondersteuning voor een nieuwe Waarschuwingstype bijna realtime metrische waarschuwingen (preview) genoemd. Deze functie is momenteel in de openbare preview.

Bijna realtime metriek verschillen waarschuwingen van reguliere metrische waarschuwingen in een aantal manieren:

- **Verbeterde latentie**: bijna realtime metriek waarschuwingen wijzigingen in metrische waarden met een frequentie van één minuut zo klein kunnen bewaken.
- **Meer controle over metrische voorwaarden**: U kunt uitgebreidere waarschuwingsregels in bijna realtime metrische waarschuwingen definiëren. De waarschuwingen bewaken van de maximale, minimale gemiddelde en totale waarden van de metrische gegevens ondersteunen.
- **Metrische gegevens uit logboeken**: van populaire logboekgegevens binnenkort in [logboekanalyse](../log-analytics/log-analytics-overview.md), metrische gegevens in Azure-Monitor kan worden geëxtraheerd en worden gewaarschuwd bij bijna realtime
- **Bewaking van meerdere metrische gegevens gecombineerd**: bijna realtime metriek waarschuwingen meerdere metrische gegevens (momenteel maximaal twee metrische gegevens) met een enkele regel kunnen bewaken. Er wordt een waarschuwing gegeven als beide metrische gegevens in strijd is met hun respectieve drempelwaarden voor de opgegeven periode.
- **Modulaire waarschuwingssysteem**: bijna realtime metriek waarschuwingen gebruiken [actiegroepen](monitoring-action-groups.md). U kunt actiegroepen gebruiken om modulaire acties te maken. U kunt actiegroepen voor meerdere regels voor waarschuwingen hergebruiken.

> [!NOTE]
> De near realtime metrische waarschuwing is momenteel is openbare preview-versie. En metrische gegevens van onderdelen van Logboeken in *beperkt* openbare preview. De functionaliteit en de gebruikerservaring kan worden gewijzigd.
>

## <a name="metrics-and-dimensions-supported"></a>Metrische gegevens en dimensies die worden ondersteund
Ondersteuning voor waarschuwingen voor de metrische gegevens die gebruikmaken van dimensies bijna realtime metrische waarschuwingen. U kunt dimensies gebruiken voor het filteren van uw metric naar het juiste niveau. Alle ondersteunde metrische gegevens samen met de toepasselijke dimensies kunnen worden verkend en gevisualiseerd van [Azure Monitor - Metrics Explorer (Preview)](monitoring-metric-charts.md).

Hier volgt een volledige lijst met Azure monitor op basis van metrische bronnen die worden ondersteund voor near realtime metrische waarschuwingen:

|Metrische naam meer informatie  |Dimensies ondersteund  |
|---------|---------|
|Microsoft.ApiManagement/service     | Ja        |
|Microsoft.Automation/automationAccounts     |     N/A    |
|Microsoft.Automation/automationAccounts     |   N/A      |
|Microsoft.Cache/Redis     |    N/A     |
|Microsoft.Compute/virtualMachines     |    N/A     |
|Microsoft.Compute/virtualMachineScaleSets     |   N/A      |
|Microsoft.DataFactory/factories     |   N/A      |
|Microsoft.DBforMySQL/servers     |   N/A      |
|Microsoft.DBforPostgreSQL/servers     |    N/A     |
|Microsoft.EventHub/namespaces     |   N/A      |
|Microsoft.Logic/workflows     |     N/A    |
|Microsoft.Network/applicationGateways     |    N/A     |
|Microsoft.Network/publicipaddresses     |  N/A       |
|Microsoft.Search/searchServices     |   N/A      |
|Microsoft.ServiceBus/namespaces     |  N/A       |
|Microsoft.Storage/storageAccounts     |    Ja     |
|Microsoft.Storage/storageAccounts/services     |     Ja    |
|Microsoft.StreamAnalytics/streamingjobs     |  N/A       |
|Microsoft.CognitiveServices/accounts     |    N/A     |


Metrische gegevens uit logboeken, ondersteunt momenteel de volgende populaire OMS-Logboeken:
- [Prestatiemeteritems](../log-analytics/log-analytics-data-sources-performance-counters.md) voor Windows en Linux-machines
- Heartbeat-records voor machines
- [Updatebeheer](../operations-management-suite/oms-solution-update-management.md) records

Hier volgt een volledige lijst met OMS logboek gebaseerd metrische bronnen die worden ondersteund voor near realtime metrische waarschuwingen:

Metrische naam meer informatie  |Dimensies ondersteund  | Type logboek  |
|---------|---------|---------|
|Average_Avg. Leestijd schijf     |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Windows-prestatiemeteritem      |
| Average_Avg. Schrijftijd schijf     |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Windows-prestatiemeteritem      |
| Wachtrijlengte voor schijf Average_Current   |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Windows-prestatiemeteritem      |
| Average_Disk Reads/sec    |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Windows-prestatiemeteritem      |
| Average_Disk Schijfoverdrachten per seconde    |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Windows-prestatiemeteritem      |
|   Average_ % vrije ruimte    |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Windows-prestatiemeteritem      |
| Average_Available MBytes     |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Windows-prestatiemeteritem      |
| Average_ percentage toegewezen Bytes In gebruik    |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Windows-prestatiemeteritem      |
| Average_Bytes ontvangen per seconde    |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Windows-prestatiemeteritem      |
|  Average_Bytes Sent/sec    |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Windows-prestatiemeteritem      |
|  Totaal aantal Average_Bytes per seconde    |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Windows-prestatiemeteritem      |
|  Average_ percentage processortijd    |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Windows-prestatiemeteritem      |
|   Wachtrijlengte van Average_Processor    |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Windows-prestatiemeteritem      |
|   Average_ percentage vrije Inodes   |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritem      |
|    Average_ % vrije ruimte   |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritem      |
|    Average_ percentage gebruikte Inodes  |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritem      |
|    Average_ percentage gebruikte ruimte   |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritem      |
|    Average_Disk Read Bytes/sec    |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritem      |
|    Average_Disk Reads/sec |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritem      |
|    Average_Disk Schijfoverdrachten per seconde |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritem      |
|    Average_Disk geschreven Bytes per seconde   |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritem      |
|    Average_Disk Writes/sec    |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritem      |
|    Average_Free Megabytes |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritem      |
|    Average_Logical Disk Bytes/sec |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritem      |
|    Average_ percentage beschikbaar geheugen |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritem      |
|    Percentage beschikbare wisselruimte Average_ |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritem      |
|    Average_ percentage gebruikt geheugen  |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritem      |
|    Average_ percentage gebruikte wisselruimte  |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritem      |
|    Geheugen in megabytes Average_Available    |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritem      |
|    Average_Available megabytes voor wisselen  |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritem      |
|    Average_Page leesbewerkingen per seconde |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritem      |
|    Average_Page schrijfbewerkingen per seconde    |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritem      |
|    Average_Pages/sec  |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritem      |
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
|    Average_% IO Wait Time |     Ja - Computer, ObjectName, InstanceName, itempad & SourceSystem    |   Linux-prestatiemeteritem      |
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

> [!NOTE]
> Specifieke metrische gegevens en/of de dimensie wordt alleen weergegeven als de gegevens voor deze in gekozen periode bestaat

## <a name="create-a-near-real-time-metric-alert"></a>Een waarschuwing voor near realtime metrische maken
Op dit moment kunt u bijna realtime metrische waarschuwingen alleen in de Azure-portal. Ondersteuning voor het configureren van bijna realtime metrische waarschuwingen met behulp van PowerShell, de Azure-opdrachtregelinterface (Azure CLI) en de Monitor REST-API's van Azure is binnenkort beschikbaar.

De ervaring voor het maken van een near realtime metrische waarschuwing is verplaatst naar de nieuwe **waarschuwingen (Preview)** pagina. Zelfs als de huidige waarschuwingen pagina **waarschuwing toevoegen in de buurt van Real-Time metriek**, wordt u omgeleid naar de **waarschuwingen (Preview)** pagina.

Zie voor meer informatie over het maken van een waarschuwing voor near realtime metrische, [geen waarschuwingsregel maken in de Azure portal](monitor-alerts-unified-usage.md#create-an-alert-rule-with-the-azure-portal).

## <a name="manage-near-real-time-metric-alerts"></a>Bijna realtime metrische waarschuwingen beheren
Nadat u een near realtime metrische waarschuwing maakt, kunt u de waarschuwing beheren met behulp van de stappen in [uw waarschuwingen beheren in de Azure-portal](monitor-alerts-unified-usage.md#managing-your-alerts-in-azure-portal).

## <a name="payload-schema"></a>De nettolading van schema

De POST-bewerking bevat de volgende JSON-nettolading en het schema voor alle bijna realtime metrische waarschuwingen wanneer op de juiste wijze geconfigureerd [actiegroep](monitoring-action-groups.md) wordt gebruikt:

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

* Meer informatie over de nieuwe [waarschuwingen (Preview) ervaring](monitoring-overview-unified-alerts.md).
* Meer informatie over [waarschuwingen melden in Azure waarschuwingen (Preview)](monitor-alerts-unified-log.md).
* Meer informatie over [waarschuwingen in Azure](monitoring-overview-alerts.md).
