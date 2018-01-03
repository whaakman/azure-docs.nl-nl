---
title: Near-realtime metrische waarschuwingen in Azure Monitor | Microsoft Docs
description: Bijna realtime metriek kunnen waarschuwingen u Azure-resource metrische gegevens zo vaak als 1 min. controleren.
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
ms.openlocfilehash: cd1002929ad749ac1742e914a9f2411f09ec91d5
ms.sourcegitcommit: b7adce69c06b6e70493d13bc02bd31e06f291a91
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/19/2017
---
# <a name="near-real-time-metric-alerts-preview"></a>Bijna realtime metrische waarschuwingen (Preview)
Monitor voor Azure ondersteunt nu een nieuw type metrische waarschuwingen bijna Real-Time metriek waarschuwingen (Preview) genoemd. Deze functie is momenteel in de openbare preview.
Deze waarschuwingen verschillen van reguliere metrische waarschuwingen in enkele manieren

- **Verbeterde latentie** -bijna realtime metriek waarschuwingen kunnen bewaken wijzigingen in metrische waarden zo snel 1 min.
- **Meer controle over metrische voorwaarden** -bijna realtime metriek waarschuwingen toestaan dat gebruikers uitgebreidere waarschuwingsregels definiëren. De waarschuwingen bieden nu ondersteuning voor het bewaken van de maximale, minimale gemiddelde en totale waarden van de metrische gegevens.
- **Bewaking van meerdere metrische gegevens gecombineerd** -bijna realtime metriek waarschuwingen kunnen meerdere metrische gegevens controleren (momenteel twee) met een enkele regel. Waarschuwing wordt geactiveerd als zowel de metrische gegevens die in strijd is met hun respectieve drempelwaarden voor de opgegeven periode.
- **Modulaire waarschuwingssysteem** - bijna realtime metrische waarschuwingen gebruik [actiegroepen](monitoring-action-groups.md). Deze functionaliteit kan gebruikers acties in een modulaire manier maken en deze voor veel waarschuwingsregels opnieuw te gebruiken.

> [!NOTE]
> Bijna realtime metrische waarschuwingen is functie momenteel in de openbare preview. De functionaliteit en de gebruikerservaring kan worden gewijzigd.
>

## <a name="what-resources-can-i-create-near-real-time-metric-alerts-for"></a>Welke bronnen kan ik bijna realtime metrische waarschuwingen voor maken?
Volledige lijst met brontypen die worden ondersteund door bijna realtime metrische waarschuwingen:

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

## <a name="near-real-time-metric-alerts-on-metrics-with-dimensions"></a>Bijna Real-Time metriek waarschuwingen op metrische gegevens met dimensies
Bijna Real-Time metriek waarschuwingen ondersteunt waarschuwen voor metrische gegevens met dimensies. Dimensies zijn een manier voor het filteren van uw metric naar het juiste niveau. Bijna realtime metriek worden waarschuwingen op metrische gegevens met dimensies ondersteund voor de volgende resourcetypen

* Microsoft.ApiManagement/service
* Microsoft.Storage/storageAccounts (alleen ondersteund voor storage-accounts in regio's VS)
* Microsoft.Storage/storageAccounts/services (alleen ondersteund voor storage-accounts in regio's VS)


## <a name="create-a-near-real-time-metric-alert"></a>Een waarschuwing voor Near realtime metrische maken
Op dit moment is bijna realtime metriek kunnen waarschuwingen alleen worden gemaakt via de Azure portal. Ondersteuning voor het configureren van bijna realtime metrische waarschuwingen via PowerShell-opdrachtregelinterface (CLI) en Azure Monitor REST-API is binnenkort beschikbaar.

De waarschuwing ervaring maken voor bijna Real-Time metriek waarschuwing is verplaatst naar de nieuwe **Alerts(Preview)** optreden. Hoewel de huidige waarschuwingen pagina toont **toevoegen in de buurt van Real-Time metriek waarschuwing**, wordt u omgeleid naar de nieuwe ervaring.

U kunt een near realtime metrische waarschuwing met de stappen maken [hier](monitor-alerts-unified-usage.md#create-an-alert-rule-with-the-azure-portal).

## <a name="managing-near-real-time-metric-alerts"></a>Bijna realtime metrische waarschuwingen beheren
Nadat u hebt gemaakt een **bijna Real-Time metriek waarschuwing**, kunnen worden beheerd met de stappen [hier](monitor-alerts-unified-usage.md#managing-your-alerts-in-azure-portal).

## <a name="next-steps"></a>Volgende stappen

* [Meer informatie over de ervaring voor de nieuwe waarschuwingen (preview)](monitoring-overview-unified-alerts.md)
* [Meer informatie over waarschuwingen logboek in Azure waarschuwingen (preview)](monitor-alerts-unified-log.md)
* [Meer informatie over waarschuwingen in Azure](monitoring-overview-alerts.md)