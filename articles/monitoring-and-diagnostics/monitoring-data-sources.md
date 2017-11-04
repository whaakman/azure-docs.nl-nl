---
title: Bewakingsgegevens van Azure gebruiken | Microsoft Docs
description: Informatie over alle bewaking gegevensbronnen beschikbaar zijn op Azure vandaag.
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 3/27/2017
ms.author: johnkem
ms.openlocfilehash: c7075c2e1a2500eca1d0aa9b3a797e8a0e903ede
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="consume-monitoring-data-from-azure"></a>Bewakingsgegevens van Azure gebruiken

Via de Azure-platform meebrengen we samen bewakingsgegevens op één plaats met de Azure-Monitor met pipeline, maar vrijwel Bevestig dat vandaag nog niet alle bewakingsgegevens nog beschikbaar zijn in deze pijplijn is. In dit artikel wordt we een overzicht van de verschillende manieren waarop u programmatisch toegang tot bewakingsgegevens van Azure-services.

## <a name="options-for-data-consumption"></a>Opties voor het verbruik van gegevens

| Gegevenstype | Category | Ondersteunde Services | Toegangsmethoden |
| --- | --- | --- | --- |
| Azure-platform-niveau metrische gegevens van Monitor | Metrische gegevens | [Overzicht hier](monitoring-supported-metrics.md) | <ul><li>**REST-API:** [Azure Monitor metrische API](https://docs.microsoft.com/rest/api/monitor/metrics)</li><li>**Opslag-blob of event hub:** [diagnostische instellingen](monitoring-overview-of-diagnostic-logs.md#resource-diagnostic-settings)</li></ul> |
| COMPUTE Gast OS metrische gegevens (bv. -prestatiemeteritems) | Metrische gegevens | [Windows](../virtual-machines-dotnet-diagnostics.md) en virtuele Linux-Machines (v2) [Cloudservices](../cloud-services/cloud-services-dotnet-diagnostics-trace-flow.md), [Fabric-Service](../service-fabric/service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md) | <ul><li>**Table Storage of blob:** [Windows of Linux Azure diagnostics](../cloud-services/cloud-services-dotnet-diagnostics-storage.md)</li><li>**Event hub:** [Windows Azure diagnostics](../event-hubs/event-hubs-streaming-azure-diags-data.md)</li></ul> |
| Aangepaste of een toepassing metrische gegevens | Metrische gegevens | Alle toepassingen die zijn uitgerust met Application Insights | <ul><li>**REST-API:** [Application Insights REST-API](https://dev.applicationinsights.io/reference)</li></ul> |
| Metrische gegevens Storage | Metrische gegevens | Azure Storage | <ul><li>**Table Storage:** [Storage Analytics](https://docs.microsoft.com/rest/api/storageservices/storage-analytics)</li></ul> |
| Factureringsgegevens | Metrische gegevens | Alle Azure-services | <ul><li>**REST-API:** [Azure brongebruik en RateCard API's](../billing/billing-usage-rate-card-overview.md)</li></ul> |
| Activiteitenlogboek | Gebeurtenissen | Alle Azure-services | <ul><li>**REST-API:** [gebeurtenissen van Azure controleren API](https://docs.microsoft.com/rest/api/monitor/events)</li><li>**Opslag-blob of event hub:** [logboek profiel](monitoring-overview-activity-logs.md#export-the-activity-log-with-a-log-profile)</li></ul> |
| Diagnostische logboeken van Azure Monitor | Gebeurtenissen | [Overzicht hier](monitoring-diagnostic-logs-schema.md) | <ul><li>**Opslag-blob of event hub:** [diagnostische instellingen](monitoring-overview-of-diagnostic-logs.md#resource-diagnostic-settings)</li></ul> |
| COMPUTE Gast OS-Logboeken (bv. IIS, ETW, syslogs) | Gebeurtenissen | [Windows](../virtual-machines-dotnet-diagnostics.md) en virtuele Linux-Machines (v2) [Cloudservices](../cloud-services/cloud-services-dotnet-diagnostics-trace-flow.md), [Fabric-Service](../service-fabric/service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md) | <ul><li>**Table Storage of blob:** [Windows of Linux Azure diagnostics](../cloud-services/cloud-services-dotnet-diagnostics-storage.md)</li><li>**Event hub:** [Windows Azure diagnostics](../event-hubs/event-hubs-streaming-azure-diags-data.md)</li></ul> |
| App Service-Logboeken | Gebeurtenissen | App-services | <ul><li>**Bestand, tabel of blob-opslag:** [Web-app diagnose](../app-service/web-sites-enable-diagnostic-log.md)</li></ul> |
| Opslag Logboeken | Gebeurtenissen | Azure Storage | <ul><li>**Table Storage:** [Storage Analytics](https://docs.microsoft.com/rest/api/storageservices/storage-analytics)</li></ul> |
| Security Center-waarschuwingen | Gebeurtenissen | Azure Security Center | <ul><li>**REST-API:** [beveiligingswaarschuwingen](https://msdn.microsoft.com/library/mt704050.aspx)</li></ul> |
| Active Directory-rapportage | Gebeurtenissen | Azure Active Directory | <ul><li>**REST-API:** [Azure Active Directory graph API](../active-directory/active-directory-reporting-api-getting-started.md)</li></ul> |
| Security Center-resourcestatus | Status | [Alle ondersteunde bronnen](https://msdn.microsoft.com/library/mt704041.aspx#Anchor_1) | <ul><li>**REST-API:** [beveiliging statussen](https://msdn.microsoft.com/library/mt704041.aspx)</li></ul> |
| Status van resources | Status | Ondersteunde services | <ul><li>**REST-API:** [resourcestatus REST-API](https://azure.microsoft.com/blog/reduce-troubleshooting-time-with-azure-resource-health/)</li></ul> |
| Metrische waarschuwingen van Azure controleren | Meldingen | [Overzicht hier](monitoring-supported-metrics.md) | <ul><li>**Webhook:** [Azure metrische waarschuwingen](insights-webhooks-alerts.md)</li></ul> |
| Waarschuwingen van Azure activiteitenlogboek van Monitor | Meldingen | Alle Azure-services | <ul><li>**Webhook:** Azure Activity Log waarschuwingen</li></ul> |
| Meldingen over automatisch schalen | Meldingen | [Overzicht hier](monitoring-overview-autoscale.md#supported-services-for-autoscale) | <ul><li>**Webhook:** [schema voor automatisch schalen melding webhook nettolading](insights-autoscale-to-webhook-email.md#autoscale-notification-webhook-payload-schema)</li></ul> |
| Waarschuwingen van de zoekopdracht OMS-logboek | Meldingen | OMS Log Analytics | <ul><li>**Webhook:** [Log Analytics-waarschuwingen](../log-analytics/log-analytics-alerts-actions.md#webhook-actions)</li></ul> |
| Application Insights metrische waarschuwingen | Meldingen | Application Insights | <ul><li>**Webhook:** [Application Insights-waarschuwingen](../application-insights/app-insights-alerts.md)</li></ul> |
| Application Insights-webtests | Meldingen | Application Insights | <ul><li>**Webhook:** [Application Insights-waarschuwingen](../application-insights/app-insights-alerts.md)</li></ul> |

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [Azure Monitor metrische gegevens](monitoring-overview-metrics.md)
- Meer informatie over [de Azure Activity Log](monitoring-overview-activity-logs.md)
- Meer informatie over [diagnostische logboeken van Azure](monitoring-overview-of-diagnostic-logs.md)
