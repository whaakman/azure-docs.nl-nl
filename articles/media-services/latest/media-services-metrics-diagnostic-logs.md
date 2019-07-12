---
title: Bewaken van Azure Media Services metrische gegevens en logboeken met diagnostische gegevens via Azure Monitor | Microsoft Docs
description: In dit artikel biedt een overzicht van hoe u Azure Media Services metrische gegevens en logboeken met diagnostische gegevens via Azure Monitor bewaakt.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2019
ms.author: juliako
ms.openlocfilehash: 6d26cd809d78bf05f66c9fa03be5063ca4d2d5e4
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67805997"
---
# <a name="monitor-media-services-metrics-and-diagnostic-logs"></a>Diagnostische logboeken en metrische gegevens over Media Services controleren

[Azure Monitor](../../azure-monitor/overview.md) kunt u metrische gegevens controleren en tot diagnostische logboeken waarmee u inzicht in hoe uw toepassingen worden uitgevoerd. Alle gegevens die zijn verzameld door Azure Monitor in een van twee fundamentele typen, logboeken en metrische gegevens past. U kunt diagnostische logboeken van Media Services controleren en maken van waarschuwingen en meldingen voor de verzamelde metrische gegevens en Logboeken. U kunt visualiseren en analyseren van de metrische gegevens via [Metrics explorer](../../azure-monitor/platform/metrics-getting-started.md). U kunt Logboeken verzenden [Azure Storage](https://azure.microsoft.com/services/storage/), ze te streamen [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/), en ze te exporteren [Log Analytics](https://azure.microsoft.com/services/log-analytics/), of 3rd party services gebruiken.

Zie voor een gedetailleerd overzicht [Azure Monitor Metrics](../../azure-monitor/platform/data-platform.md) en [Azure Monitor diagnostische logboeken](../../azure-monitor/platform/diagnostic-logs-overview.md).

In dit onderwerp worden ondersteunde [Media Services-metrische gegevens](#media-services-metrics) en [Media Services diagnostische logboeken](#media-services-diagnostic-logs).

## <a name="media-services-metrics"></a>Metrische gegevens over Media Services

Metrische gegevens worden verzameld met regelmatige tussenpozen al dan niet de waarde wordt gewijzigd. Ze zijn nuttig omdat ze vaak kunnen worden verzameld, en een waarschuwing kan snel worden geactiveerd met relatief eenvoudige logica op waarschuwingen. Zie voor meer informatie over het maken van metrische waarschuwingen [maken, weergeven en beheren van metrische waarschuwingen met behulp van Azure Monitor](../../azure-monitor/platform/alerts-metric.md).

Media Services ondersteunt de bewaking metrische gegevens voor de volgende resources:

* Account
* Streaming-eindpunt
 
### <a name="account"></a>Account

U kunt de volgende metrische gegevens controleren. 

|Naam van de meetwaarde|`Display name`|Description|
|---|---|---|
|AssetCount|Telling Assets|Assets in uw account.|
|AssetQuota|Asset-quotum|Asset-quota in uw account.|
|AssetQuotaUsedPercentage|Asset gebruikt quotum voor percentage|Het percentage van de quota voor Asset al gebruikt.|
|ContentKeyPolicyCount|Inhoud sleutel aantal|Beleid voor inhoud sleutels in uw account.|
|ContentKeyPolicyQuota|Inhoud sleutel beleid quotum|Inhoud quotum voor beleid voor sleutels in uw account.|
|ContentKeyPolicyQuotaUsedPercentage|Inhoud sleutel beleid quotum gebruikt percentage|Het percentage van de quota voor inhoud sleutel beleid al gebruikt.|
|StreamingPolicyCount|Beleid voor het aantal streaming|Streaming-beleid in uw account.|
|StreamingPolicyQuota|Streaming-beleid voor quotum|Streaming-beleidsregels voor quota in uw account.|
|StreamingPolicyQuotaUsedPercentage|Quota voor streaming beleid gebruikt percentage|Het percentage van de quota voor Streaming beleid al gebruikt.|
 
Lees ook [account quota en beperkingen](limits-quotas-constraints.md).

### <a name="streaming-endpoint"></a>Streaming-eindpunt

De volgende Media Services [Streaming-eindpunten](https://docs.microsoft.com/rest/api/media/streamingendpoints) metrische gegevens worden ondersteund:

|Naam van de meetwaarde|`Display name`|Description|
|---|---|---|
|Aanvragen|Aanvragen|Het totale aantal HTTP-aanvragen die zijn aangeleverd door de Streaming-eindpunt biedt.|
|Uitgaand verkeer|Uitgaand verkeer|Het totale aantal uitgaande bytes. Bijvoorbeeld: bytes gestreamd door het Streaming-eindpunt.|
|SuccessE2ELatency|End-to-end latentie van geslaagde|De tijdsduur wanneer het Streaming-eindpunt ontvangen voor de aanvraag voor wanneer de laatste byte van het antwoord is verzonden.|

### <a name="why-would-i-want-to-use-metrics"></a>Waarom zou ik willen metrische gegevens gebruiken? 

Hier volgen enkele voorbeelden van hoe Media Services metrische gegevens controleren krijgt u inzicht in hoe uw toepassingen worden uitgevoerd. Er zijn enkele vragen die kunnen worden aangepakt met metrische gegevens over Media Services:

* Hoe bewaak ik mijn Standard Streaming-eindpunt om te weten wanneer ik de overschreden?
* Hoe weet ik als ik heb voldoende schaaleenheden Premium Streaming-eindpunt? 
* Hoe kan ik een melding om te weten wanneer mijn Streaming-eindpunten kan worden uitgebreid instellen?
* Hoe stel ik een waarschuwing te weten wanneer de maximale uitgaande geconfigureerd op het account is bereikt?
* Hoe kan ik de uitsplitsing van mislukte aanvragen zien en wat de oorzaak is van de fout?
* Hoe kan ik zien hoeveel HLS of DASH-aanvragen worden wordt opgehaald uit de pakketbouwer?
* Hoe stel ik een waarschuwing te weten wanneer de drempelwaarde van het aantal mislukte aanvragen is bereikt? 

### <a name="example"></a>Voorbeeld

Zie [mediaservices metrische gegevens controleren](media-services-metrics-howto.md)

## <a name="media-services-diagnostic-logs"></a>Diagnostische logboeken van Media Services

Diagnoselogboeken bieden uitgebreide, regelmatig gegevens over de werking van een Azure-resource. Zie voor meer informatie, [over het verzamelen en gebruiken van logboekgegevens van uw Azure-resources](../../azure-monitor/platform/diagnostic-logs-overview.md).

Media Services ondersteunt de volgende logboeken met diagnostische gegevens:

* Sleutellevering

### <a name="key-delivery"></a>Sleutellevering

|Name|Description|
|---|---|
|Sleutellevering serviceaanvraag|De logboeken die de sleutel leveringsinformatie van de aanvraag weergegeven. Zie voor meer informatie, [schema's](media-services-diagnostic-logs-schema.md).|

### <a name="why-would-i-want-to-use-diagnostics-logs"></a>Waarom zou ik wil gebruiken, diagnostische logboeken? 

Er zijn enkele dingen die u met sleutel-levering logboeken met diagnostische gegevens controleren kunt:

* Zie het aantal licenties die worden geleverd door DRM-type
* Zie het aantal licenties die worden geleverd door het beleid 
* Er worden fouten weergegeven door DRM of het beleid
* Zie het aantal niet-geautoriseerde licentieaanvragen van clients

### <a name="example"></a>Voorbeeld

Zie [diagnostische logboeken van Media Service bewaken](media-services-diagnostic-logs-howto.md)

## <a name="next-steps"></a>Volgende stappen 

* [Het verzamelen en gebruiken van logboekgegevens van uw Azure-resources](../../azure-monitor/platform/diagnostic-logs-overview.md)
* [Maken, weergeven en beheren van metrische waarschuwingen met behulp van Azure Monitor](../../azure-monitor/platform/alerts-metric.md)
* [Media Services metrische gegevens controleren](media-services-metrics-howto.md)
* [Diagnostische logboeken van Media Service bewaken](media-services-diagnostic-logs-howto.md)
