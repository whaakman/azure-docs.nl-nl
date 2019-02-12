---
title: Video Indexer-concepten
titlesuffix: Azure Media Services
description: Dit onderwerp worden enkele concepten van de Video Indexer-service.
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.topic: article
ms.date: 02/10/2019
ms.author: juliako
ms.openlocfilehash: c7bbe8c6b2ad51ed7272cd215552807c7cea3aee
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/11/2019
ms.locfileid: "55991504"
---
# <a name="video-indexer-concepts"></a>Video Indexer-concepten
 
Dit artikel worden enkele concepten van de Video Indexer-service.
    
## <a name="summarized-insights"></a>Samengevatte insights

Samengevatte insights bevat een samengevoegde weergave van de gegevens: gezichten, onderwerpen en emoties. Bijvoorbeeld, in plaats van gaan over elk van de duizenden tijdsbereik en waarin gezichten worden weergegeven in het te controleren, de samengevatte inzichten bevat alle gezichten en voor elk adres, het tijdsbereik dat deze wordt weergegeven in en het percentage van de tijd die wordt weergegeven.

## <a name="time-range-vs-adjusted-time-range"></a>tijdsbereik versus aangepast tijdsbereik

TimeRange is het tijdsbereik in de oorspronkelijke video. AdjustedTimeRange is het tijdsbereik ten opzichte van de huidige afspeellijst. Omdat u een afspeellijst van verschillende regels van verschillende video's maken kunt, kunt u een video van 1 uur duren en slechts 1 regel van het, bijvoorbeeld, 10:00-10:15 gebruikt. In dat geval hebt u een afspeellijst met 1 regel, waar het tijdsbereik 10:00:15-10 is, maar de adjustedTimeRange is 00:00-00:15.
 
## <a name="blocks"></a>blokken

Blokken zijn bedoeld om gemakkelijker te doorlopen van de gegevens. Een blok kan bijvoorbeeld worden opgesplitst op basis van wanneer er tussen sprekers wordt gewisseld of als er een lange pauze plaatsvindt.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over hoe u aan de slag [hoe u zich aanmelden en uw eerste video uploaden](video-indexer-get-started.md).

## <a name="see-also"></a>Zie ook

[Overzicht van Video Indexer](video-indexer-overview.md)
