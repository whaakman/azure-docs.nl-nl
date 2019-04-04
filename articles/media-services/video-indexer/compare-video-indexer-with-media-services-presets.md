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
ms.date: 02/10/2019
ms.author: juliako
ms.openlocfilehash: 041e76ccecb4dd0fe9c060681609dfb92c03ec5a
ms.sourcegitcommit: 0a3efe5dcf56498010f4733a1600c8fe51eb7701
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58893143"
---
# <a name="compare-azure-media-services-v3-presets-and-video-indexer"></a>Voorinstellingen voor Azure Media Services v3 en Video Indexer vergelijken 

In dit artikel vergelijkt de mogelijkheden van **Video Indexer-API's** en **Media Services v3 API's**. 

Er is momenteel een overlapping tussen de functies die worden aangeboden door de [Video Indexer-API's](https://api-portal.videoindexer.ai/) en de [Media Services v3 API's](https://github.com/Azure/azure-rest-api-specs/blob/master/specification/mediaservices/resource-manager/Microsoft.Media/stable/2018-07-01/Encoding.json). De volgende tabel bevat de huidige richtlijn voor het begrijpen van de verschillen en overeenkomsten. 

## <a name="compare"></a>Vergelijken

|Functie|Video Indexer-API 's |Analyzer video en Audio Analyzer voorinstellingen<br/>in Media Services v3 API's|
|---|---|---|
|Media-inzichten|[Verbeterd](video-indexer-output-json-v2.md) |[Basisprincipes](../latest/intelligence-concept.md)|
|Ervaringen|Zie de volledige lijst van ondersteunde functies: <br/> [Overzicht](video-indexer-overview.md)|Retourneert alleen inzichten in video 's|
|Billing|[Prijzen van Media Services](https://azure.microsoft.com/pricing/details/media-services/#analytics)|[Prijzen van Media Services](https://azure.microsoft.com/pricing/details/media-services/#analytics)|
|Naleving|[Compliance met Azure](https://aka.ms/AzureCompliance)|Media Services is compatibel met veel certificeringen. Bekijk [Azure naleving Offerings.pdf](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942/file/178110/23/Microsoft%20Azure%20Compliance%20Offerings.pdf) en zoek naar 'Media Services' om te zien als deze aan een certificaat van belang zijn voldoet.|
|Gratis proefversie|US - oost|Niet beschikbaar|
|Beschikbaarheid |VS-West, Zuidoost-Azië, Noord-Europa|Zie [Azure status](https://azure.microsoft.com/global-infrastructure/services/?products=media-services).|

## <a name="next-steps"></a>Volgende stappen

[Video Indexer-overzicht](video-indexer-overview.md)

[Overzicht van Media Services v3](../latest/media-services-overview.md)
