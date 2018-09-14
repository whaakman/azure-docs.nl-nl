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
ms.openlocfilehash: 67bbb7046d71b904dfdcaad0e44024e8f30ac288
ms.sourcegitcommit: f983187566d165bc8540fdec5650edcc51a6350a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/13/2018
ms.locfileid: "45543696"
---
# <a name="video-indexer-concepts"></a>Video Indexer-concepten
 
Dit artikel worden enkele concepten van de Video Indexer-service.
    
## <a name="summarized-insights"></a>Samengevatte insights

Samengevatte insights bevat een samengevoegde weergave van de gegevens: gezichten, onderwerpen en emoties. Bijvoorbeeld, in plaats van gaan over elk van de duizenden tijdsbereik en waarin gezichten worden weergegeven in het te controleren, de samengevatte inzichten bevat alle gezichten en voor elk adres, het tijdsbereik dat deze wordt weergegeven in en het percentage van de tijd die wordt weergegeven.

## <a name="time-range-vs-adjusted-time-range"></a>tijdsbereik versus aangepast tijdsbereik

TimeRange is het tijdsbereik in de oorspronkelijke video. AdjustedTimeRange is het tijdsbereik ten opzichte van de huidige afspeellijst. Omdat u een afspeellijst van verschillende regels van verschillende video's maken kunt, kunt u een video van 1 uur duren en slechts 1 regel van het, bijvoorbeeld, 10:00-10:15 gebruikt. In dat geval hebt u een afspeellijst met 1 regel, waar het tijdsbereik 10:00:15-10 is, maar de adjustedTimeRange is 00:00-00:15.
 
## <a name="blocks"></a>blokken

Blokken zijn bedoeld om gemakkelijker te doorlopen van de gegevens. Bijvoorbeeld, kan blok worden opgesplitst op basis van wanneer sprekers wijzigen of als er een pauze van lang.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over hoe u aan de slag [hoe u zich aanmelden en uw eerste video uploaden](video-indexer-get-started.md).

## <a name="see-also"></a>Zie ook

[Video Indexer-overzicht](video-indexer-overview.md)
