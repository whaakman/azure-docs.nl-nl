---
title: Bronnen van bewakingsgegevens in Azure
description: Informatie over alle bewaking gegevensbronnen beschikbaar zijn op Azure vandaag nog.
author: johnkemnetz
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 06/12/2018
ms.author: johnkem
ms.component: ''
ms.openlocfilehash: 55dc43f238b487392be48047b3d608f84e1e3ee2
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/14/2018
ms.locfileid: "45579307"
---
# <a name="consume-monitoring-data-from-azure"></a>Bewakingsgegevens van Azure gebruiken

In het Azure-platform, zijn we bijeenbrengt bewakingsgegevens op één plek met de Azure Monitor pipeline, maar nagenoeg erkent dat vandaag nog niet alle controlegegevens beschikbaar is in die pijplijn nog. In dit artikel wordt samengevat we de verschillende manieren waarop u kunt via een programma toegang tot bewakingsgegevens van Azure-services.

## <a name="options-for-data-consumption"></a>Opties voor het gegevensverbruik

| Gegevenstype | Categorie | Ondersteunde Services | Methoden voor toegang |
| --- | --- | --- | --- |
| Metrische gegevens van Azure Monitor-platform-niveau | Metrische gegevens | [Bekijk hier de lijst](monitoring-supported-metrics.md) | <ul><li>**REST-API:** [metrische API van Azure Monitor](https://docs.microsoft.com/rest/api/monitor/metrics)</li><li>**Storage blob of event hub:** [diagnostische instellingen](monitoring-overview-of-diagnostic-logs.md#diagnostic-settings)</li></ul> |
| COMPUTE Gast OS metrische gegevens (zoals) prestatiemeteritems) | Metrische gegevens | [Windows](../virtual-machines-dotnet-diagnostics.md) en Linux-Machines (v2), [Cloudservices](../cloud-services/cloud-services-dotnet-diagnostics-trace-flow.md), [Service Fabric](../service-fabric/service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md) | <ul><li>**Storage-tabel of blob:** [Windows of Linux Azure diagnostics](../cloud-services/cloud-services-dotnet-diagnostics-storage.md)</li><li>**Event hub:** [Windows Azure diagnostics](../event-hubs/event-hubs-streaming-azure-diags-data.md)</li></ul> |
| Aangepaste of toepassing metrische gegevens | Metrische gegevens | Elke toepassing die is geïnstrumenteerd met Application Insights | <ul><li>**REST-API:** [Application Insights REST-API](https://dev.applicationinsights.io/reference)</li></ul> |
| Metrische opslaggegevens | Metrische gegevens | Azure Storage | <ul><li>**Storage-tabel:** [Storage Analytics](https://docs.microsoft.com/rest/api/storageservices/storage-analytics)</li></ul> |
| Factureringsgegevens | Metrische gegevens | Alle Azure-services | <ul><li>**REST-API:** [Azure-Resource gebruiks- en RateCard API's](../billing/billing-usage-rate-card-overview.md)</li></ul> |
| Activiteitenlogboek | Gebeurtenissen | Alle Azure-services | <ul><li>**REST-API:** [Azure Monitor gebeurtenissen API](https://docs.microsoft.com/rest/api/monitor/eventcategories)</li><li>**Storage blob of event hub:** [Logboekprofiel](monitoring-overview-activity-logs.md#export-the-activity-log-with-a-log-profile)</li></ul> |
| Diagnostische logboeken van Azure Monitor | Gebeurtenissen | [Bekijk hier de lijst](monitoring-diagnostic-logs-schema.md) | <ul><li>**Storage blob of event hub:** [diagnostische instellingen](monitoring-overview-of-diagnostic-logs.md#diagnostic-settings)</li></ul> |
| COMPUTE Gast OS-Logboeken (bv. IIS, ETW, syslogs) | Gebeurtenissen | [Windows](../virtual-machines-dotnet-diagnostics.md) en Linux-Machines (v2), [Cloudservices](../cloud-services/cloud-services-dotnet-diagnostics-trace-flow.md), [Service Fabric](../service-fabric/service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md) | <ul><li>**Storage-tabel of blob:** [Windows of Linux Azure diagnostics](../cloud-services/cloud-services-dotnet-diagnostics-storage.md)</li><li>**Event hub:** [Windows Azure diagnostics](../event-hubs/event-hubs-streaming-azure-diags-data.md)</li></ul> |
| App Service-Logboeken | Gebeurtenissen | App-services | <ul><li>**Bestand, tabel of blob-opslag:** [Web-app diagnostische gegevens](../app-service/web-sites-enable-diagnostic-log.md)</li></ul> |
| Opslaglogboeken | Gebeurtenissen | Azure Storage | <ul><li>**Storage-tabel:** [Storage Analytics](https://docs.microsoft.com/rest/api/storageservices/storage-analytics)</li></ul> |
| Waarschuwingen van Beveiligingscentrum | Gebeurtenissen | Azure Security Center | <ul><li>**REST-API:** [beveiligingswaarschuwingen](https://msdn.microsoft.com/library/mt704050.aspx)</li></ul> |
| Active Directory-rapportage | Gebeurtenissen | Azure Active Directory | <ul><li>**REST-API:** [Azure Active Directory graph API](../active-directory/active-directory-reporting-api-getting-started.md)</li></ul> |
| Security Center-resourcestatus | Status | [Alle ondersteunde resources](https://msdn.microsoft.com/library/mt704041.aspx#Anchor_1) | <ul><li>**REST-API:** [Security statussen](https://msdn.microsoft.com/library/mt704041.aspx)</li></ul> |
| Status van resources | Status | Ondersteunde services | <ul><li>**REST-API:** [resourcestatus REST-API](https://azure.microsoft.com/blog/reduce-troubleshooting-time-with-azure-resource-health/)</li></ul> |
| Metrische waarschuwingen van Azure Monitor | Meldingen | [Bekijk hier de lijst](monitoring-supported-metrics.md) | <ul><li>**Webhook:** [Azure metrische waarschuwingen](insights-webhooks-alerts.md)</li></ul> |
| Waarschuwingen van Azure Monitor Activity-Log | Meldingen | Alle Azure-services | <ul><li>**Webhook:** Azure Activity Log waarschuwingen</li></ul> |
| Meldingen over automatisch schalen | Meldingen | [Bekijk hier de lijst](monitoring-overview-autoscale.md#supported-services-for-autoscale) | <ul><li>**Webhook:** [voor automatisch schalen melding webhook-schema voor nettolading](insights-autoscale-to-webhook-email.md#autoscale-notification-webhook-payload-schema)</li></ul> |
| Zoekquery waarschuwingen voor activiteitenlogboeken | Meldingen | Log Analytics | <ul><li>**Webhook:** [webhookactie voor waarschuwingsregels](../monitoring-and-diagnostics/monitor-alerts-unified-log-webhook.md)</li></ul> |
| Application Insights metrische waarschuwingen | Meldingen | Application Insights | <ul><li>**Webhook:** [Application Insights-waarschuwingen](../application-insights/app-insights-alerts.md)</li></ul> |
| Application Insights-webtests | Meldingen | Application Insights | <ul><li>**Webhook:** [Application Insights-waarschuwingen](../application-insights/app-insights-alerts.md)</li></ul> |

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [Azure Monitor metrics](monitoring-overview-metrics.md)
- Meer informatie over [de Azure-activiteitenlogboek](monitoring-overview-activity-logs.md)
- Meer informatie over [diagnostische logboeken van Azure](monitoring-overview-of-diagnostic-logs.md)
