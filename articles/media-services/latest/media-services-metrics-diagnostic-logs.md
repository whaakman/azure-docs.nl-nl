---
title: Bewaken van Media Services metrische gegevens en logboeken met diagnostische gegevens via Azure Monitor | Microsoft Docs
description: In dit artikel biedt een overzicht van het bewaken van Media Services metrische gegevens en logboeken met diagnostische gegevens via Azure Monitor.
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
ms.date: 03/05/2019
ms.author: juliako
ms.openlocfilehash: 4252b0f26ef3c02216a41dde5d2e1cb5ea0efd6a
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57554999"
---
# <a name="monitor-media-services-metrics-and-diagnostic-logs"></a>Diagnostische logboeken en metrische gegevens over Media Services controleren

[Azure Monitor](../../azure-monitor/overview.md) kunt u metrische gegevens controleren en tot diagnostische logboeken waarmee u inzicht in hoe uw toepassingen worden uitgevoerd. Alle gegevens die zijn verzameld door Azure Monitor in een van twee fundamentele typen, metrische gegevens en logboekbestanden past. U kunt diagnostische logboeken van Media Services controleren en maken van waarschuwingen en meldingen voor de verzamelde metrische gegevens en Logboeken. U kunt visualiseren en analyseren van de metrische gegevens via [Metrics explorer](../../azure-monitor/platform/metrics-getting-started.md). U kunt Logboeken verzenden [Azure Storage](https://azure.microsoft.com/services/storage/), ze te streamen [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/), en ze te exporteren [Log Analytics](https://azure.microsoft.com/services/log-analytics/), of 3rd party services gebruiken.

Zie voor een gedetailleerd overzicht [Azure Monitor Metrics](../../azure-monitor/platform/data-collection.md) en [Azure Monitor diagnostische logboeken](../../azure-monitor/platform/diagnostic-logs-overview.md).

In dit onderwerp worden momenteel beschikbaar [Media Services-metrische gegevens](#media-services-metrics) en [Media Services diagnostische logboeken](#media-services-diagnostic-logs).

## <a name="media-services-metrics"></a>Metrische gegevens over Media Services

Metrische gegevens worden verzameld met regelmatige tussenpozen al dan niet de waarde wordt gewijzigd. Ze zijn nuttig omdat ze vaak kunnen worden verzameld, en een waarschuwing kan snel worden geactiveerd met relatief eenvoudige logica op waarschuwingen.

Op dit moment in de volgende Media Services [Streaming-eindpunten](https://docs.microsoft.com/rest/api/media/streamingendpoints) metrische gegevens worden gegenereerd door Azure:

|Name|Description|
|---|---|
|Aanvragen|Deze code geeft informatie over het totale aantal aanvragen dat wordt verwerkt door de streaming-eindpunt.|
|Uitgaand verkeer|Totaal aantal uitgaande bytes. Bijvoorbeeld: bytes gestreamd door het streaming-eindpunt.|
|End-to-end latentie van geslaagde| Biedt informatie over end-to-end latentie van geslaagde aanvragen.|

Bijvoorbeeld, als u 'Uitgaande' metrische gegevens met CLI, Voer u de volgende `az monitor metrics` CLI-opdracht:

```cli
az monitor metrics list --resource \
   "/subscriptions/<subscription id>/resourcegroups/<resource group name>/providers/Microsoft.Media/mediaservices/<Media Services account name>/streamingendpoints/<streaming endpoint name>" \
   --metric "Egress"
```

Zie voor meer informatie over het maken van metrische waarschuwingen [maken, weergeven en beheren van metrische waarschuwingen met behulp van Azure Monitor](../../azure-monitor/platform/alerts-metric.md).

## <a name="media-services-diagnostic-logs"></a>Diagnostische logboeken van Media Services

Op dit moment kunt u de volgende diagnostische logboeken opvragen:

|Name|Description|
|---|---|
|Sleutellevering serviceaanvraag|De logboeken die de sleutel leveringsinformatie van de aanvraag weergegeven. Zie voor meer informatie, [schema's](media-services-diagnostic-logs-schema.md).|

Om in te schakelen opslag van logboeken met diagnostische gegevens in een Storage-Account, moet u het volgende uitvoeren `az monitor diagnostic-settings` CLI-opdracht: 

```cli
az monitor diagnostic-settings create --name <diagnostic name> \
    --storage-account <name or ID of storage account> \
    --resource <target resource object ID> \
    --resource-group <storage account resource group> \
    --logs '[
    {
        "category": <category name>,
        "enabled": true,
        "retentionPolicy": {
            "days": <# days to retain>,
            "enabled": true
        }
    }]'
```

Bijvoorbeeld:

```cli
az monitor diagnostic-settings create --name amsv3diagnostic \
    --storage-account storageaccountforamsv3  \
    --resource "/subscriptions/00000000-0000-0000-0000-0000000000/resourceGroups/amsv3ResourceGroup/providers/Microsoft.Media/mediaservices/amsv3account" \
    --resource-group "amsv3ResourceGroup" \
    --logs '[{"category": "KeyDeliveryRequests",  "enabled": true, "retentionPolicy": {"days": 3, "enabled": true }}]'
```

## <a name="next-steps"></a>Volgende stappen 

[Het verzamelen en gebruiken van logboekgegevens van uw Azure-resources](../../azure-monitor/platform/diagnostic-logs-overview.md).
