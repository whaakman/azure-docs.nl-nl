---
title: Bewaking van de gegevenslocaties in Azure Monitor | Microsoft Docs
description: Beschrijft de verschillende locaties waar door gegevens te controleren in Azure met inbegrip van het platform van Azure Monitor gegevens worden opgeslagen.
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.service: azure-monitor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/21/2019
ms.author: bwren
ms.openlocfilehash: 1d92973e32e9c694b1d0488753b9a701e7d71a5d
ms.sourcegitcommit: c05618a257787af6f9a2751c549c9a3634832c90
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/30/2019
ms.locfileid: "66416907"
---
# <a name="monitoring-data-locations-in-azure-monitor"></a>Bewaking van de gegevenslocaties in Azure Monitor

Azure Monitor is gebaseerd op een [gegevensplatform](data-platform.md) van [logboeken](data-platform-logs.md) en [metrische gegevens](data-platform-metrics.md) zoals beschreven in [gegevensplatform in de Azure Monitor](data-platform.md). Bewakingsgegevens uit Azure-resources kan worden geschreven naar andere locaties echter een voordat ze worden gekopieerd naar Azure Monitor of ter ondersteuning van aanvullende scenario's. 

## <a name="monitoring-data-locations"></a>Gegevenslocaties controleren

De volgende tabel bevat de verschillende locaties waar de bewakingsgegevens in Azure wordt verzonden en de verschillende methoden om er toegang toe hebben.

| Locatie | Description | Methoden voor toegang |
|:---|:---|:---|:--|
| Azure Monitor Metrics | Time series-database die is geoptimaliseerd voor het analyseren van gegevens met tijdstempel. | [Metrics Explorer](metrics-getting-started.md)<br>[Metrische API voor Azure Monitor](/rest/api/monitor/metrics) |
| Logboeken van Azure Monitor    | Log Analytics-werkruimte die gebaseerd op Azure Data Explorer biedt een krachtige analyse-engine en rijke querytaal. | [Log Analytics](../log-query/portals.md)<br>[Log Analytics-API](https://dev.loganalytics.io/)<br>[Application Insights-API](https://dev.applicationinsights.io/reference/get-query) |
| Activiteitenlogboek | Gegevens uit het activiteitenlogboek is vooral nuttig zijn wanneer verzonden naar Azure controleren logboeken analyseren met andere gegevens, maar worden ook op een eigen verzameld, zodat deze direct kan worden weergegeven in de Azure-portal. | [Azure Portal](activity-log-view.md#azure-portal)<br>[Azure Monitor gebeurtenissen API](/rest/api/monitor/eventcategories) |
| Azure Storage | Sommige gegevensbronnen wordt rechtstreeks naar Azure storage worden geschreven en configuratie vereist voor het verplaatsen van gegevens in Logboeken. U kunt ook gegevens verzenden naar Azure storage voor het archiveren en voor integratie met externe systemen.  | [Storage Analytics](/rest/api/storageservices/storage-analytics)<br>[Server Explorer](/visualstudio/azure/vs-azure-tools-storage-resources-server-explorer-browse-manage)<br>[Storage Explorer](/azure/vs-azure-tools-storage-manage-with-storage-explorer?tabs=windows) |
| Event Hubs | Gegevens verzenden naar Azure Event Hubs te streamen naar andere locaties. | [Vastleggen naar opslag](../../event-hubs/event-hubs-capture-overview.md)  |
| Azure Monitor voor virtuele machines | Azure Monitor voor virtuele machines worden opgeslagen statusgegevens van de werkbelasting in een aangepaste locatie die wordt gebruikt door de bewakingservaring in Azure portal. | [Azure Portal](../insights/vminsights-overview.md)<br>[Workload monitor REST-API](https://docs.microsoft.com/rest/api/monitor/microsoft.workloadmonitor/components)<br>[Azure Resource health REST-API](https://docs.microsoft.com/rest/api/resourcehealth/)  |
| Waarschuwingen | Waarschuwingen die zijn gemaakt door Azure Monitor. | [Azure Portal](alerts-managing-alert-instances.md)<br>[REST-API voor beheer van waarschuwingen](https://docs.microsoft.com/rest/api/monitor/alertsmanagement/alerts) |



## <a name="next-steps"></a>Volgende stappen

- Zie de verschillende bronnen van [door gegevens te controleren die worden verzameld door Azure Monitor](data-sources.md).
- Meer informatie over de [typen gegevens te controleren die worden verzameld door Azure Monitor](data-platform.md) en hoe u kunt weergeven en analyseren van deze gegevens.
