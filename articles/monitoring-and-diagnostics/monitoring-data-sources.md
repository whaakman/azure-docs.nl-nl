---
title: Bronnen van de bewaking van de gegevens in Azure
description: Informatie over alle bewaking gegevensbronnen beschikbaar zijn op Azure vandaag.
author: johnkemnetz
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 06/12/2018
ms.author: johnkem
ms.component: ''
ms.openlocfilehash: 78b3288cf4973efa2684252581000d0e4fc56cae
ms.sourcegitcommit: d1eefa436e434a541e02d938d9cb9fcef4e62604
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2018
ms.locfileid: "37082995"
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
| Activiteitenlogboek | Gebeurtenissen | Alle Azure-services | <ul><li>**REST-API:** [gebeurtenissen van Azure controleren API](https://docs.microsoft.com/en-us/rest/api/monitor/eventcategories)</li><li>**Opslag-blob of event hub:** [logboek profiel](monitoring-overview-activity-logs.md#export-the-activity-log-with-a-log-profile)</li></ul> |
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
| Meld u zoekopdracht waarschuwingen | Meldingen | Log Analytics | <ul><li>**Webhook:** [Webhook-actie voor waarschuwingsregels logboek](../monitoring-and-diagnostics/monitor-alerts-unified-log-webhook.md)</li></ul> |
| Application Insights metrische waarschuwingen | Meldingen | Application Insights | <ul><li>**Webhook:** [Application Insights-waarschuwingen](../application-insights/app-insights-alerts.md)</li></ul> |
| Application Insights-webtests | Meldingen | Application Insights | <ul><li>**Webhook:** [Application Insights-waarschuwingen](../application-insights/app-insights-alerts.md)</li></ul> |

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [Azure Monitor metrische gegevens](monitoring-overview-metrics.md)
- Meer informatie over [de Azure Activity Log](monitoring-overview-activity-logs.md)
- Meer informatie over [diagnostische logboeken van Azure](monitoring-overview-of-diagnostic-logs.md)
