---
title: Azure Video Indexer-concepten | Microsoft Docs
description: Dit onderwerp worden enkele concepten van de Video Indexer-service.
services: cognitive services
documentationcenter: ''
author: juliako
manager: erikre
ms.service: cognitive-services
ms.topic: article
ms.date: 07/31/2018
ms.author: juliako
ms.openlocfilehash: 224c8b05027f51fb99c8d58be34c3604032c0f77
ms.sourcegitcommit: d4c076beea3a8d9e09c9d2f4a63428dc72dd9806
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2018
ms.locfileid: "39399492"
---
# <a name="video-indexer-concepts"></a>Video Indexer-concepten
 
Dit onderwerp worden enkele concepten van de Video Indexer-service.
    
## <a name="summarized-insights"></a>Samengevatte insights

Samengevatte insights bevat een samengevoegde weergave van de gegevens: gezichten, trefwoorden, sentimenten. Bijvoorbeeld, in plaats van gaan over elk van de duizenden tijdsbereik en waarin gezichten worden weergegeven in het te controleren, de samengevatte inzichten bevat alle gezichten en voor elk adres, het tijdsbereik dat deze wordt weergegeven in en het percentage van de tijd die wordt weergegeven.

## <a name="topicskeywords"></a>Onderwerpen/trefwoorden

Er zijn onderwerpen/trefwoorden in de lijst met belangrijke zinnen die Video Indexer worden geëxtraheerd uit de tekst. Bijvoorbeeld, een video van Scott Guthrie de volgende onderwerpen/trefwoorden kan bevatten: beveiliging, Azure, Microsoft Cloud, omzet.

## <a name="sentiments"></a>sentimenten

Wanneer Video Indexer transcripties analyseert, wordt ook sentimenten gedetecteerd. Bijvoorbeeld, is 'Dit is een spannende gebeurtenis' een positieve stemming.

## <a name="time-range-vs-adjusted-time-range"></a>tijdsbereik versus aangepast tijdsbereik

TimeRange is het tijdsbereik in de oorspronkelijke video. AdjustedTimeRange is het tijdsbereik ten opzichte van de huidige afspeellijst. Omdat u een afspeellijst van verschillende regels van verschillende video's maken kunt, kunt u een video van 1 uur duren en slechts 1 regel van het, bijvoorbeeld, 10:00-10:15 gebruikt. In dat geval hebt u een afspeellijst met 1 regel, waar het tijdsbereik 10:00:15-10 is, maar de adjustedTimeRange is 00:00-00:15.
 
## <a name="blocks"></a>blokken

Blokken zijn bedoeld om gemakkelijker te doorlopen van de gegevens. Bijvoorbeeld, kan blok worden opgesplitst op basis van wanneer sprekers wijzigen of als er een pauze van lang.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over hoe u aan de slag [hoe u zich aanmelden en uw eerste video uploaden](video-indexer-get-started.md).

## <a name="see-also"></a>Zie ook

[Video Indexer-overzicht](video-indexer-overview.md)
