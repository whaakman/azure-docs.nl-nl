---
title: Overzicht van de verwerking van Media schalen | Microsoft Docs
description: Dit onderwerp bevat een overzicht van het vergroten/verkleinen Media verwerken met Azure Media Services.
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 780ef5c2-3bd6-4261-8540-6dee77041387
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/04/2017
ms.author: juliako
ms.openlocfilehash: a82481c4995bfb078e88d7096dff37b52312a296
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="scaling-media-processing-overview"></a>Overzicht van de verwerking van Media schalen
Deze pagina biedt een overzicht van hoe en waarom schalen verwerking van media. 

## <a name="overview"></a>Overzicht
Media Services-accounts worden gekoppeld aan een gereserveerde-eenheidstype, waarmee wordt bepaald hoe snel de mediaverwerkingstaken worden verwerkt. U kunt kiezen uit de volgende gereserveerde-eenheidstypen: **S1**, **S2** en **S3**. Een coderingstaak wordt bijvoorbeeld sneller uitgevoerd wanneer u het gereserveerde-eenheidstype **S2** gebruikt (in vergelijking met het type **S1**). Zie voor meer informatie de [gereserveerde eenheidstypen](https://azure.microsoft.com/blog/high-speed-encoding-with-azure-media-services/).

Naast het opgeven van het type gereserveerde eenheid, kunt u opgeven voor het inrichten van uw account met gereserveerde eenheden. Op basis van het aantal ingerichte, gereserveerde eenheden wordt bepaald hoeveel mediataken tegelijk kunnen worden verwerkt voor een bepaald account. Bijvoorbeeld, als uw account vijf gereserveerde eenheden, heeft en vervolgens vijf media taken wordt gelijktijdig zo lang worden uitgevoerd als er zijn taken moeten worden verwerkt. De resterende taken in de wachtrij moeten wachten en wordt ophalen opgepikt sequentieel worden verwerkt wanneer een actieve taak is voltooid. Als een account heeft geen geen gereserveerde eenheden die zijn ingericht, zullen vervolgens taken worden opgepikt sequentieel worden verwerkt. In dit geval afhankelijk de wachttijd tussen één taak voltooid en de volgende begin van de beschikbaarheid van resources in het systeem.

## <a name="choosing-between-different-reserved-unit-types"></a>Kiezen tussen verschillende gereserveerde eenheidstypen
De volgende tabel kunt u besluit bij de keuze tussen verschillende codering snelheid te koppelen. Ook biedt enkele benchmark gevallen en biedt SAS-URL's die u gebruiken kunt voor het downloaden van video's waarop u uw eigen tests kunt uitvoeren:

| Scenario's | **S1** | **S2** | **S3** |
| --- | --- | --- | --- |
| Beoogde gebruiksscenario |Single-bitrate codering. <br/>Bestanden op SD of onder oplossingen, tijd niet gevoelige, lage kosten. |Single-bitrate en meerdere bitrate codering.<br/>Het normale gebruik voor zowel SD en HD codering. |Single-bitrate en meerdere bitrate codering.<br/>Volledige HD en 4K resolutie video's. Tijd die gevoelige en snellere reactietijd ook codering. |
| Benchmark |[Bestand voor invoer: 5 minuten lang 640x360p op 29,97 frames per seconde](https://wamspartners.blob.core.windows.net/for-long-term-share/Whistler_5min_360p30.mp4?sr=c&si=AzureDotComReadOnly&sig=OY0TZ%2BP2jLK7vmcQsCTAWl33GIVCu67I02pgarkCTNw%3D).<br/><br/>Codering naar een single-bitrate MP4-bestand met de resolutie, duurt ongeveer 11 minuten. |[Bestand voor invoer: 5 minuten lang 1280x720p op 29,97 frames per seconde](https://wamspartners.blob.core.windows.net/for-long-term-share/Whistler_5min_720p30.mp4?sr=c&si=AzureDotComReadOnly&sig=OY0TZ%2BP2jLK7vmcQsCTAWl33GIVCu67I02pgarkCTNw%3D)<br/><br/>Codering met 'Standaardinstelling H264 Single-Bitrate 720p' vooraf duurt ongeveer 5 minuten.<br/><br/>Codering met ' standaardinstelling H264 Multiple Bitrate 720p ' voorinstelling duurt ongeveer 11.5 minuten. |[Bestand voor invoer: 5 minuten lang 1920x1080p op 29,97 frames per seconde](https://wamspartners.blob.core.windows.net/for-long-term-share/Whistler_5min_1080p30.mp4?sr=c&si=AzureDotComReadOnly&sig=OY0TZ%2BP2jLK7vmcQsCTAWl33GIVCu67I02pgarkCTNw%3D). <br/><br/>Codering met 'Standaardinstelling H264 Single-Bitrate 1080p' vooraf duurt ongeveer 2.7 minuten.<br/><br/>Codering met ' standaardinstelling H264 Multiple Bitrate 1080p ' voorinstelling duurt ongeveer 5.7 minuten. |

## <a name="considerations"></a>Overwegingen
> [!IMPORTANT]
> Bekijk de overwegingen in deze sectie beschreven.  
> 
> 

* Gereserveerde eenheden werkt voor alle media verwerking, met inbegrip van taken met behulp van Azure Media Indexer indexeren parallelizing.  Indexeringstaken worden echter niet sneller verwerkt met snellere gereserveerde eenheden, terwijl dit bij coderen wel het geval is.
* Als de gedeelde groep wordt gebruikt, dat wil zeggen, hebben zonder eventuele gereserveerde eenheden vervolgens uw taken coderen dezelfde prestaties als met S1 RUs. Er is echter geen bovengrens voor de tijd die uw taken in in de wachtrij staat besteden kunnen, en op elk gewenst maximaal slechts één taak wordt uitgevoerd.
* De volgende datacenters bieden niet de **S2** gereserveerd eenheidstype: Brazilië-Zuid en India-West.
* Het volgende Datacenter biedt niet aan de **S3** eenheidstype gereserveerd: India-West.

## <a name="billing"></a>Facturering

Er worden kosten in rekening gebracht op basis van het werkelijke aantal minuten dat gereserveerde media-eenheden worden gebruikt. Voor een gedetailleerde uitleg, Zie de sectie Veelgestelde vragen over de [Media Services-prijzen](https://azure.microsoft.com/pricing/details/media-services/) pagina.   

## <a name="quotas-and-limitations"></a>Quota en beperkingen
Zie voor meer informatie over quota's en beperkingen en hoe u een ondersteuningsticket opent [quota's en beperkingen](media-services-quotas-and-limitations.md).

## <a name="next-step"></a>Volgende stap
De schaal verwerkingstaak voor media met een van deze technologieën bereiken: 

> [!div class="op_single_selector"]
> * [.NET](media-services-dotnet-encoding-units.md)
> * [Portal](media-services-portal-scale-media-processing.md)
> * [REST](https://docs.microsoft.com/rest/api/media/operations/encodingreservedunittype)
> * [Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
> * [PHP](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)
> 

> [!NOTE]
> Om de nieuwste versie van Java SDK en te beginnen met ontwikkelen met behulp van Java, Zie [aan de slag met de SDK voor Java-clients voor Media Services](https://docs.microsoft.com/azure/media-services/media-services-java-how-to-use). <br/>
> Voor het downloaden van de meest recente PHP SDK voor Media Services, zoekt u versie 0.5.7 van het pakket Microsoft/WindowAzure in de [Packagist opslagplaats](https://packagist.org/packages/microsoft/windowsazure#v0.5.7).  

## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

