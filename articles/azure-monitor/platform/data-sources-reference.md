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
ms.openlocfilehash: 62474aba9eb429755ff5e7c78725ee3ce2d90117
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2018
ms.locfileid: "53728207"
---
# <a name="consume-monitoring-data-from-azure"></a>Bewakingsgegevens van Azure gebruiken

In het Azure-platform, zijn we bijeenbrengt bewakingsgegevens op één plek met de Azure Monitor pipeline, maar nagenoeg erkent dat vandaag nog niet alle controlegegevens beschikbaar is in die pijplijn nog. In dit artikel wordt samengevat we de verschillende manieren waarop u kunt via een programma toegang tot bewakingsgegevens van Azure-services.

## <a name="options-for-data-consumption"></a>Opties voor het gegevensverbruik

| Gegevenstype | Categorie | Ondersteunde Services | Methoden voor toegang |
| --- | --- | --- | --- |
| Metrische gegevens van Azure Monitor-platform-niveau | Metrische gegevens | [Bekijk hier de lijst](metrics-supported.md) | <ul><li>**REST-API:** [Metrische API van Azure Monitor](https://docs.microsoft.com/rest/api/monitor/metrics)</li><li>**Storage blob of event hub:** [Diagnostische instellingen](diagnostic-logs-overview.md#diagnostic-settings)</li></ul> |
| COMPUTE Gast OS metrische gegevens (zoals) prestatiemeteritems) | Metrische gegevens | [Windows](/azure/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines) en Linux-Machines (v2), [Cloudservices](../../cloud-services/cloud-services-dotnet-diagnostics-trace-flow.md), [Service Fabric](../../service-fabric/service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md) | <ul><li>**- Of blob Storage-tabel:** [Windows- of Linux Azure diagnostics](diagnostics-extension-to-storage.md)</li><li>**Event hub:** [Windows Azure diagnostics](diagnostics-extension-stream-event-hubs.md)</li></ul> |
| Aangepaste of toepassing metrische gegevens | Metrische gegevens | Elke toepassing die is geïnstrumenteerd met Application Insights | <ul><li>**REST-API:** [Application Insights REST-API](https://dev.applicationinsights.io/reference)</li></ul> |
| Metrische opslaggegevens | Metrische gegevens | Azure Storage | <ul><li>**Storage-tabel:** [Storage Analytics](https://docs.microsoft.com/rest/api/storageservices/storage-analytics)</li></ul> |
| Factureringsgegevens | Metrische gegevens | Alle Azure-services | <ul><li>**REST-API:** [Azure-Resource gebruiks- en RateCard API 's](../../billing/billing-usage-rate-card-overview.md)</li></ul> |
| Activiteitenlogboek | Gebeurtenissen | Alle Azure-services | <ul><li>**REST-API:** [Azure Monitor gebeurtenissen API](https://docs.microsoft.com/rest/api/monitor/eventcategories)</li><li>**Storage blob of event hub:** [Logboekprofiel](activity-logs-overview.md#export-the-activity-log-with-a-log-profile)</li></ul> |
| Diagnostische logboeken van Azure Monitor | Gebeurtenissen | [Bekijk hier de lijst](tutorial-dashboards.md) | <ul><li>**Storage blob of event hub:** [Diagnostische instellingen](diagnostic-logs-overview.md#diagnostic-settings)</li></ul> |
| COMPUTE Gast OS-Logboeken (bv. IIS, ETW, syslogs) | Gebeurtenissen | [Windows](/azure/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines) en Linux-Machines (v2), [Cloudservices](../../cloud-services/cloud-services-dotnet-diagnostics-trace-flow.md), [Service Fabric](../../service-fabric/service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md) | <ul><li>**- Of blob Storage-tabel:** [Windows- of Linux Azure diagnostics](diagnostics-extension-to-storage.md)</li><li>**Event hub:** [Windows Azure diagnostics](diagnostics-extension-stream-event-hubs.md)</li></ul> |
| App Service-Logboeken | Gebeurtenissen | App-services | <ul><li>**Bestand, tabel of blob-opslag:** [Diagnostische gegevens van web-app](../../app-service/troubleshoot-diagnostic-logs.md)</li></ul> |
| Opslaglogboeken | Gebeurtenissen | Azure Storage | <ul><li>**Storage-tabel:** [Storage Analytics](https://docs.microsoft.com/rest/api/storageservices/storage-analytics)</li></ul> |
| Waarschuwingen van Beveiligingscentrum | Gebeurtenissen | Azure Security Center | <ul><li>**REST-API:** [Beveiligingswaarschuwingen](https://msdn.microsoft.com/library/mt704050.aspx)</li></ul> |
| Active Directory-rapportage | Gebeurtenissen | Azure Active Directory | <ul><li>**REST-API:** [Azure Active Directory graph API](../../active-directory/reports-monitoring/concept-reporting-api.md)</li></ul> |
| Security Center-resourcestatus | Status | [Alle ondersteunde resources](https://msdn.microsoft.com/library/mt704041.aspx#Anchor_1) | <ul><li>**REST-API:** [Beveiliging-statussen](https://msdn.microsoft.com/library/mt704041.aspx)</li></ul> |
| Status van resources | Status | Ondersteunde services | <ul><li>**REST-API:** [Resourcestatus REST-API](https://azure.microsoft.com/blog/reduce-troubleshooting-time-with-azure-resource-health/)</li></ul> |
| Metrische waarschuwingen van Azure Monitor | Meldingen | [Bekijk hier de lijst](metrics-supported.md) | <ul><li>**Webhook:** [Azure metrische waarschuwingen](alerts-webhooks.md)</li></ul> |
| Waarschuwingen van Azure Monitor Activity-Log | Meldingen | Alle Azure-services | <ul><li>**Webhook:** Waarschuwingen van Azure Activity Log</li></ul> |
| Meldingen over automatisch schalen | Meldingen | [Bekijk hier de lijst](autoscale-overview.md#supported-services-for-autoscale) | <ul><li>**Webhook:** [Automatisch schalen melding webhook-schema voor nettolading](autoscale-webhook-email.md#autoscale-notification-webhook-payload-schema)</li></ul> |
| Zoekquery waarschuwingen voor activiteitenlogboeken | Meldingen | Log Analytics | <ul><li>**Webhook:** [Webhookactie voor waarschuwingsregels](alerts-log-webhook.md)</li></ul> |
| Application Insights metrische waarschuwingen | Meldingen | Application Insights | <ul><li>**Webhook:** [Application Insights-waarschuwingen](../../azure-monitor/app/alerts.md)</li></ul> |
| Application Insights-webtests | Meldingen | Application Insights | <ul><li>**Webhook:** [Application Insights-waarschuwingen](../../azure-monitor/app/alerts.md)</li></ul> |

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [Azure Monitor metrics](data-collection.md)
- Meer informatie over [de Azure-activiteitenlogboek](activity-logs-overview.md)
- Meer informatie over [diagnostische logboeken van Azure](diagnostic-logs-overview.md)
