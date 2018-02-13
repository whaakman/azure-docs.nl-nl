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
ms.date: 12/06/2017
ms.author: snmuvva
ms.custom: 
ms.openlocfilehash: 6370f4596e6b20962c6de0dbcbd5f86c3b7777cc
ms.sourcegitcommit: b32d6948033e7f85e3362e13347a664c0aaa04c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2018
---
# <a name="near-real-time-metric-alerts-preview"></a>Bijna realtime metrische waarschuwingen (preview)
Monitor voor Azure biedt ondersteuning voor een nieuwe Waarschuwingstype bijna realtime metrische waarschuwingen (preview) genoemd. Deze functie is momenteel in de openbare preview.

Bijna realtime metriek verschillen waarschuwingen van reguliere metrische waarschuwingen in een aantal manieren:

- **Verbeterde latentie**: bijna realtime metriek waarschuwingen wijzigingen in metrische waarden met een frequentie van 1 minuut zo klein kunnen bewaken.
- **Meer controle over metrische voorwaarden**: U kunt uitgebreidere waarschuwingsregels in bijna realtime metrische waarschuwingen definiëren. De waarschuwingen bewaken van de maximale, minimale gemiddelde en totale waarden van de metrische gegevens ondersteunen.
- **Bewaking van meerdere metrische gegevens gecombineerd**: bijna realtime metriek waarschuwingen meerdere metrische gegevens (momenteel maximaal twee metrische gegevens) met een enkele regel kunnen bewaken. Er wordt een waarschuwing gegeven als beide metrische gegevens in strijd is met hun respectieve drempelwaarden voor de opgegeven periode.
- **Modulaire waarschuwingssysteem**: bijna realtime metriek waarschuwingen gebruiken [actiegroepen](monitoring-action-groups.md). U kunt actiegroepen gebruiken om modulaire acties te maken. U kunt actiegroepen voor meerdere regels voor waarschuwingen hergebruiken.

> [!NOTE]
> De near realtime metrische waarschuwing functie is momenteel in de openbare preview. De functionaliteit en de gebruikerservaring kan worden gewijzigd.
>

## <a name="resources-you-can-use-with-near-real-time-metric-alerts"></a>U kunt gebruiken met bijna realtime metrische waarschuwingen
Hier volgt een volledige lijst met brontypen die worden ondersteund voor near realtime metrische waarschuwingen:

* Microsoft.ApiManagement/service
* Microsoft.Automation/automationAccounts
* Microsoft.Batch/batchAccounts
* Microsoft.Cache/Redis
* Microsoft.Compute/virtualMachines
* Microsoft.Compute/virtualMachineScaleSets
* Microsoft.DataFactory/factories
* Microsoft.DBforMySQL/servers
* Microsoft.DBforPostgreSQL/servers
* Microsoft.EventHub/namespaces
* Microsoft.Logic/workflows
* Microsoft.Network/applicationGateways
* Microsoft.Network/publicipaddresses
* Microsoft.Search/searchServices
* Microsoft.ServiceBus/namespaces
* Microsoft.Storage/storageAccounts
* Microsoft.Storage/storageAccounts/services
* Microsoft.StreamAnalytics/streamingjobs
* Microsoft.CognitiveServices/accounts

## <a name="near-real-time-metric-alerts-for-metrics-that-use-dimensions"></a>Bijna realtime metrische waarschuwingen voor de metrische gegevens die gebruikmaken van dimensies
Ondersteuning voor waarschuwingen voor de metrische gegevens die gebruikmaken van dimensies bijna realtime metrische waarschuwingen. U kunt dimensies gebruiken voor het filteren van uw metric naar het juiste niveau. Waarschuwingen voor de metrische gegevens die gebruikmaken van dimensies worden bijna realtime metrische gegevens voor de volgende resourcetypen ondersteund:

* Microsoft.ApiManagement/service
* Microsoft.Storage/storageAccounts (alleen ondersteund voor storage-accounts in regio's VS)
* Microsoft.Storage/storageAccounts/services (alleen ondersteund voor storage-accounts in regio's VS)

## <a name="create-a-near-real-time-metric-alert"></a>Een waarschuwing voor near realtime metrische maken
Op dit moment kunt u bijna realtime metrische waarschuwingen alleen in de Azure-portal. Ondersteuning voor het configureren van bijna realtime metrische waarschuwingen met behulp van PowerShell, de Azure-opdrachtregelinterface (Azure CLI) en de Monitor REST-API's van Azure is binnenkort beschikbaar.

De ervaring voor het maken van een near realtime metrische waarschuwing is verplaatst naar de nieuwe **waarschuwingen (Preview)** pagina. Zelfs als de huidige waarschuwingen pagina **waarschuwing toevoegen in de buurt van Real-Time metriek**, wordt u omgeleid naar de **waarschuwingen (Preview)** pagina.

Zie voor meer informatie over het maken van een waarschuwing voor near realtime metrische, [geen waarschuwingsregel maken in de Azure portal](monitor-alerts-unified-usage.md#create-an-alert-rule-with-the-azure-portal).

## <a name="manage-near-real-time-metric-alerts"></a>Bijna realtime metrische waarschuwingen beheren
Nadat u een near realtime metrische waarschuwing maakt, kunt u de waarschuwing beheren met behulp van de stappen in [uw waarschuwingen beheren in de Azure-portal](monitor-alerts-unified-usage.md#managing-your-alerts-in-azure-portal).

## <a name="payload-schema"></a>De nettolading van schema

De POST-bewerking bevat de volgende JSON-nettolading en het schema voor alle bijna realtime metrische waarschuwingen:

```json
{
    "WebhookName": "Alert1510875839452",
    "RequestBody": {
        "status": "Activated",
        "context": {
            "condition": {
                "metricName": "Percentage CPU",
                "metricUnit": "Percent",
                "metricValue": "17.7654545454545",
                "threshold": "1",
                "windowSize": "10",
                "timeAggregation": "Average",
                "operator": "GreaterThan"
            },
            "resourceName": "ContosoVM1",
            "resourceType": "microsoft.compute/virtualmachines",
            "resourceRegion": "westus",
            "portalLink": "https://portal.azure.com/#resource/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/automationtest/providers/Microsoft.Compute/virtualMachines/ContosoVM1",
            "timestamp": "2017-11-16T23:54:03.9517451Z",
            "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoVM/providers/microsoft.insights/alertrules/VMMetricAlert1",
            "name": "VMMetricAlert1",
            "description": "A metric alert for the VM Win2012R2",
            "conditionType": "Metric",
            "subscriptionId": "00000000-0000-0000-0000-000000000000",
            "resourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ContosoVM/providers/Microsoft.Compute/virtualMachines/ContosoVM1",
            "resourceGroupName": "ContosoVM"
        },
        "properties": {
                "key1": "value1",
                "key2": "value2"
        }
    },
    "RequestHeader": {
        "Connection": "Keep-Alive",
        "Host": "s1events.azure-automation.net",
        "User-Agent": "azure-insights/0.9",
        "x-ms-request-id": "00000000-0000-0000-0000-000000000000"
    }
}
```

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over de nieuwe [waarschuwingen (Preview) ervaring](monitoring-overview-unified-alerts.md).
* Meer informatie over [waarschuwingen melden in Azure waarschuwingen (Preview)](monitor-alerts-unified-log.md).
* Meer informatie over [waarschuwingen in Azure](monitoring-overview-alerts.md).