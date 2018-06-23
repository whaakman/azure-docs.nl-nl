---
title: Azure Video indexeerfunctie concepten | Microsoft Docs
description: Dit onderwerp worden enkele concepten die van de indexeerfunctie Video-service.
services: cognitive services
documentationcenter: ''
author: juliako
manager: erikre
ms.service: cognitive-services
ms.topic: article
ms.date: 04/17/2017
ms.author: juliako
ms.openlocfilehash: 01d92a6b55d2fb2c09cee333f482d79d2cdf763c
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345471"
---
# <a name="video-indexer-concepts"></a>Video indexeerfunctie-concepten
 
Dit onderwerp worden enkele concepten die van de indexeerfunctie Video-service.
    
## <a name="summarized-insights"></a>Samengevatte insights

Samengevatte insights een samengevoegde weergave van de gegevens bevatten: vlakken, trefwoorden, patronen. Bijvoorbeeld, in plaats van wat over elk van de duizenden tijdsbereik en controleren welke vlakken erin, de samengevatte inzichten bevat alle vlakken en voor elk adres, de tijd bereiken die wordt weergegeven in en het percentage van de tijd wordt weergegeven.

## <a name="topicskeywords"></a>Onderwerpen over/trefwoorden

Onderwerpen over/trefwoorden zijn in de lijst met belangrijke termen die Video indexeerfunctie uit de tekst ophaalt. Een video van Scott Guthrie bevat bijvoorbeeld mogelijk de volgende onderwerpen/sleutelwoorden: beveiliging, Azure, Microsoft Cloud, omzet.

## <a name="sentiments"></a>Zie

Wanneer Video indexeerfunctie transcripties analyseert, wordt ook patronen gedetecteerd. Bijvoorbeeld, is een positieve gevoel 'is een bijzondere gebeurtenis'.

## <a name="time-range-vs-adjusted-time-range"></a>tijdsbereik versus gecorrigeerde tijdsbereik

TimeRange is het tijdsbereik in de oorspronkelijke video. AdjustedTimeRange is het tijdsbereik ten opzichte van de huidige afspeellijst. Omdat u een afspeellijst van andere regels van verschillende video's maken kunt, kunt u een video van 1 uur duren en slechts 1 regel af, bijvoorbeeld: 10:00-10:15. In dat geval hebt u een afspeellijst met 1 regel, waar het tijdsbereik 10:00-10:15 is, maar de adjustedTimeRange is 00:00-00:15.
 
## <a name="blocks"></a>Blokken

Blokken zijn bedoeld om eenvoudiger te doorlopen van de gegevens. Bijvoorbeeld, kan blok worden onderverdeeld op basis van wanneer luidsprekers wijzigt of er een lange pauze is.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het aan de slag [het aanmelden en het uploaden van uw eerste video](video-indexer-get-started.md).

## <a name="see-also"></a>Zie ook

[Video indexeerfunctie-overzicht](video-indexer-overview.md)

