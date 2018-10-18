---
title: Vergelijking van Video Indexer en Azure Media Services v3 voorinstellingen | Microsoft Docs
description: In dit onderwerp worden de Video Indexer en Azure mediaservices v3 voorinstellingen vergeleken.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/03/2018
ms.author: juliako
ms.openlocfilehash: e9b66721135456320134f41c614b194d1e22b10f
ms.sourcegitcommit: 3a7c1688d1f64ff7f1e68ec4bb799ba8a29a04a8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/17/2018
ms.locfileid: "49381033"
---
# <a name="compare-azure-media-services-v3-presets-and-video-indexer"></a>Voorinstellingen voor Azure Media Services v3 en Video Indexer vergelijken 

In dit artikel vergelijkt de mogelijkheden van **Video Indexer-API's** en **Media Services v3 API's**. 

Er is momenteel een overlapping tussen de functies die worden aangeboden door de [Video Indexer v2-API's](https://api-portal.videoindexer.ai/) en de [Media Services v3 API's](https://github.com/Azure/azure-rest-api-specs/blob/master/specification/mediaservices/resource-manager/Microsoft.Media/stable/2018-07-01/Encoding.json). De volgende tabel bevat de huidige richtlijn voor het begrijpen van de verschillen en overeenkomsten. 

## <a name="compare"></a>Vergelijken

|Functie|Video Indexer-API 's |Analyzer video en Audio Analyzer voorinstellingen<br/>in Media Services v3 API's|
|---|---|---|
|Media-inzichten|[Verbeterde](../../cognitive-services/video-indexer/video-indexer-output-json-v2.md?toc=/azure/media-services/video-indexer/toc.json&bc=/azure/media-services/video-indexer/breadcrumb/toc.json) |[Basisprincipes](../latest/intelligence-concept.md)|
|Ervaringen|Zie de volledige lijst van ondersteunde functies: <br/> [Overzicht](../../cognitive-services/video-indexer/video-indexer-overview.md?toc=/azure/media-services/video-indexer/toc.json&bc=/azure/media-services/video-indexer/breadcrumb/toc.json)|Retourneert alleen inzichten in video 's|
|Billing|[Prijzen van Media Services](https://azure.microsoft.com/pricing/details/media-services/#analytics)|[Prijzen van Media Services](https://azure.microsoft.com/pricing/details/media-services/#analytics)|
|Naleving|NOG TE BEPALEN|Media Services is compatibel met veel certificeringen. Bekijk [Azure naleving Offerings.pdf](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942/file/178110/23/Microsoft%20Azure%20Compliance%20Offerings.pdf) en zoek naar 'Media Services' om te zien als deze aan een certificaat van belang zijn voldoet.|
|Gratis proefversie|US - oost|Niet beschikbaar|
|Beschikbaarheid |VS-West, Zuidoost-Azië, Noord-Europa|Zie [Azure status](https://azure.microsoft.com/global-infrastructure/services/?products=media-services).|

## <a name="next-steps"></a>Volgende stappen

[Overzicht van Video Indexer](../../cognitive-services/video-indexer/video-indexer-overview.md?toc=/azure/media-services/video-indexer/toc.json&bc=/azure/media-services/video-indexer/breadcrumb/toc.json)

[Overzicht van Media Services v3](../../media-services/latest/media-services-overview.md)
