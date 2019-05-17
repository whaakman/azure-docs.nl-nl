---
title: Vergelijking van Video Indexer en Azure Media Services v3 voorinstellingen | Microsoft Docs
description: In dit onderwerp worden de Video Indexer en Azure mediaservices v3 voorinstellingen vergeleken.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.service: media-services
ms.subservice: video-indexer
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2019
ms.author: juliako
ms.openlocfilehash: 275178998948e357a6a72fbe5d0b3c9c01485a3a
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2019
ms.locfileid: "65800161"
---
# <a name="compare-azure-media-services-v3-presets-and-video-indexer"></a>Voorinstellingen voor Azure Media Services v3 en Video Indexer vergelijken 

In dit artikel vergelijkt de mogelijkheden van **Video Indexer-API's** en **Media Services v3 API's**. 

Er is momenteel een overlapping tussen de functies die worden aangeboden door de [Video Indexer-API's](https://api-portal.videoindexer.ai/) en de [Media Services v3 API's](https://github.com/Azure/azure-rest-api-specs/blob/master/specification/mediaservices/resource-manager/Microsoft.Media/stable/2018-07-01/Encoding.json). De volgende tabel bevat de huidige richtlijn voor het begrijpen van de verschillen en overeenkomsten. 

## <a name="compare"></a>Vergelijken

|Functie|Video Indexer-API 's |Analyzer video en Audio Analyzer voorinstellingen<br/>in Media Services v3 API's|
|---|---|---|
|Media-inzichten|[Verbeterde](video-indexer-output-json-v2.md) |[Basisprincipes](../latest/intelligence-concept.md)|
|Ervaringen|Zie de volledige lijst van ondersteunde functies: <br/> [Overzicht](video-indexer-overview.md)|Retourneert alleen inzichten in video 's|
|Facturering|[Prijzen van Media Services](https://azure.microsoft.com/pricing/details/media-services/#analytics)|[Prijzen van Media Services](https://azure.microsoft.com/pricing/details/media-services/#analytics)|
|Naleving|[ISO 27001](https://www.microsoft.com/TrustCenter/Compliance/ISO-IEC-27001), [ISO 27018](https://www.microsoft.com/trustcenter/Compliance/ISO-IEC-27018), [SOC 1,2,3](https://www.microsoft.com/TrustCenter/Compliance/SOC), [HIPAA](https://www.microsoft.com/trustcenter/compliance/hipaa), [FedRAMP](https://www.microsoft.com/TrustCenter/Compliance/fedramp), [PCI](https://www.microsoft.com/trustcenter/compliance/pci), en [ HITRUST](https://www.microsoft.com/TrustCenter/Compliance/hitrust) gecertificeerd. Voor de meest recente updates, gaat u naar [huidige certificeringen status van Video Indexer](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942).|Media Services is compatibel met veel certificeringen. Bekijk [Azure naleving Offerings.pdf](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942/file/178110/23/Microsoft%20Azure%20Compliance%20Offerings.pdf) en zoek naar 'Media Services' om te zien als deze aan een certificaat van belang zijn voldoet.|
|Gratis versie|US - oost|Niet beschikbaar|
|Beschikbaarheid voor de regio|VS-Oost 2, VS Zuid-centraal, VS-West 2, Noord-Europa, West-Europa, Zuidoost-Azië, Oost-Azië en Australië-Oost.  Voor de meest recente updates, gaat u naar de [producten per regio](https://azure.microsoft.com/global-infrastructure/services/?products=cognitive-services) pagina.|Zie [Azure status](https://azure.microsoft.com/global-infrastructure/services/?products=media-services).|

## <a name="next-steps"></a>Volgende stappen

[Overzicht van Video Indexer](video-indexer-overview.md)

[Overzicht van Media Services v3](../latest/media-services-overview.md)
