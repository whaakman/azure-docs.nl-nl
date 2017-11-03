---
title: API Management bewaken met Azure Monitor | Microsoft Docs
description: Informatie over het bewaken van Azure API Management-service met behulp van Azure-Monitor.
services: api-management
documentationcenter: 
author: vladvino
manager: erikre
editor: 
ms.assetid: 2fa193cd-ea71-4b33-a5ca-1f55e5351e23
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: 717e033aa4bbd4dd8ebcc727c3f551aee81770dc
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="monitor-api-management-with-azure-monitor"></a>Monitor voor API Management met Azure-Monitor
Monitor voor Azure is een Azure-service met één bron voor de bewaking van alle Azure-resources. Met Azure-Monitor kunt u visualiseren, query, routeren, archiveren en acties uitvoeren op de metrische gegevens en de logboeken die afkomstig zijn van de Azure-bronnen zoals API Management. 

De volgende video toont het bewaken van API Management met behulp van Azure-Monitor. Zie voor meer informatie over Azure Monitor [aan de slag met Azure Monitor]. 


> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Monitor-API-Management-with-Azure-Monitor/player]
>
>
 
## <a name="metrics"></a>Metrische gegevens
API Management momenteel vijf metrische gegevens verzendt en meer in de toekomst toegevoegd. Deze metrische gegevens worden gegenereerd per minuut, zodat u bijna real-time inzicht in de status en gezondheid van uw API's. Hier volgt een samenvatting van de metrische gegevens:
* Totaal aantal verzoeken van de Gateway: het aantal API-aanvragen in de periode. 
* Geslaagde aanvragen voor de Gateway: het aantal API-aanvragen dat geslaagde HTTP-antwoordcodes waaronder 304, 307 en iets kleiner is dan 301 (bijvoorbeeld 200) ontvangen. 
* Mislukte aanvragen van de Gateway: het aantal API-aanvragen dat foutieve HTTP-antwoordcodes waaronder 400 en iets groter zijn dan 500 ontvangen.
* Niet-geautoriseerde Gateway verzoeken: het aantal API-aanvragen die HTTP-antwoordcodes waaronder 401, 403 en 429 ontvangen. 
* Andere Gateway verzoeken: het aantal API-aanvragen die HTTP-antwoordcodes die niet bij een van de voorgaande categorieën (bijvoorbeeld 418 horen) ontvangen.

U kunt toegang tot metrische gegevens in uw API Management-service of toegang tot metrische gegevens van alle Azure-resources in Azure-Monitor. Metrische gegevens weergeven in uw API Management-service:
1. Open de Azure-portal.
2. Ga naar uw API Management-service.
3. Klik op **metrische gegevens**.

![Blade metrische gegevens][metrics-blade]

Zie voor meer informatie over het gebruik van metrische gegevens [overzicht van metrische gegevens].

## <a name="activity-logs"></a>Activiteitenlogboeken
Activiteitenlogboeken bieden inzicht in de bewerkingen die zijn uitgevoerd op uw API Management-services. Het was voorheen bekend als 'controlelogboeken' of 'operationele logs'. Met activiteitenlogboeken, kunt u bepalen de ' wat, wie, en wanneer ' voor een (PUT, POST, verwijderen schrijfbewerkingen) die op uw API Management-services worden uitgevoerd. 

> [!NOTE]
> Activiteitenlogboeken bevatten geen leesbewerkingen (GET) of bewerkingen uitgevoerd in de klassieke Portal van de uitgever of met de oorspronkelijke Management-API's.

U kunt toegang krijgen tot activiteitenlogboeken in uw API Management-service of toegang tot de logboeken van alle Azure-resources in Azure-Monitor. Registreert activiteit weergeven in uw API Management-service:
1. Open de Azure-portal.
2. Ga naar uw API Management-service.
3. Klik op **activiteitenlogboek**.

![Activiteit logboeken blade][activity-logs-blade]

Zie voor meer informatie over het gebruik van metrische gegevens [overzicht activiteitenlogboeken].

## <a name="alerts"></a>Waarschuwingen
U kunt configureren voor het ontvangen van meldingen op basis van de logboeken van metrische gegevens en de activiteit. Azure Monitor kunt u de volgende handelingen uit als er wordt een waarschuwing configureren:

* E-mailmelding verzenden
* een webhook aanroepen
* Een Azure Logic App aanroepen

U kunt regels voor waarschuwingen configureren in uw API Management-service of in de Azure-Monitor. Ze configureren in API Management: 
1. Open de Azure-portal.
2. Ga naar uw API Management-service.
3. Klik op **waarschuwing regels**.

![Regels voor waarschuwingen-blade][alert-rules-blade]

Zie voor meer informatie over het gebruik van waarschuwingen [overzicht van waarschuwingen].

## <a name="diagnostic-logs"></a>Diagnostische logboeken
Logboeken met diagnostische gegevens bevatten uitgebreide informatie over bewerkingen en fouten die belangrijk zijn voor controle, evenals het oplossen van problemen. Diagnostische logboeken verschillen van activiteitenlogboeken. Activiteitenlogboeken bieden inzicht in de bewerkingen die zijn uitgevoerd op uw Azure-resources. Diagnostische logboeken bieden inzicht in bewerkingen dat de bron zelf uitgevoerd.

API Management biedt momenteel diagnostische logboeken (batch verwerkt per uur) over de API van afzonderlijke aanvragen met elk item met de volgende structuur:

```
{
    "Tenant": "",
      "DeploymentName": "",
      "time": "",
      "resourceId": "",
      "category": "GatewayLogs",
      "operationName": "Microsoft.ApiManagement/GatewayLogs",
      "durationMs": ,
      "Level": ,
      "properties": "{
          "ApiId": "",
          "OperationId": "",
          "ProductId": "",
          "SubscriptionId": "",
          "Method": "",
          "Url": "",
          "RequestSize": ,
          "ServiceTime": "",
          "BackendMethod": "",
          "BackendUrl": "",
          "BackendResponseCode": ,
          "ResponseCode": ,
          "ResponseSize": ,
          "Cache": "",
          "UserId"
      }"
 }
```

U kunt toegang tot diagnoselogboeken in uw API Management-service of toegang tot de logboeken van alle Azure-resources in Azure-Monitor. Diagnostische logboeken weergeven in uw API Management-service:
1. Open de Azure-portal.
2. Ga naar uw API Management-service.
3. Klik op **diagnostische logboeken**.

![Blade met diagnostische logboeken][diagnostic-logs-blade]

Zie voor meer informatie over het gebruik van metrische gegevens [overzicht van diagnostische logboeken].

## <a name="next-step"></a>Volgende stap

* [aan de slag met Azure Monitor]
* [overzicht van metrische gegevens]
* [overzicht activiteitenlogboeken]
* [overzicht van diagnostische logboeken]
* [overzicht van waarschuwingen]

[aan de slag met Azure Monitor]: ../monitoring-and-diagnostics/monitoring-get-started.md
[overzicht van metrische gegevens]: ../monitoring-and-diagnostics/monitoring-overview-metrics.md
[overzicht activiteitenlogboeken]: ../monitoring-and-diagnostics/monitoring-overview-activity-logs.md
[overzicht van diagnostische logboeken]: ../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md
[overzicht van waarschuwingen]: ../monitoring-and-diagnostics/insights-alerts-portal.md



[metrics-blade]: ./media/api-management-azure-monitor/api-management-metrics-blade.png
[activity-logs-blade]: ./media/api-management-azure-monitor/api-management-activity-logs-blade.png
[alert-rules-blade]: ./media/api-management-azure-monitor/api-management-alert-rules-blade.png
[diagnostic-logs-blade]: ./media/api-management-azure-monitor/api-management-diagnostic-logs-blade.png
