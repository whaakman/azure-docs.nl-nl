---
title: Verwerking van Media schalingsoverzicht | Microsoft Docs
description: In dit onderwerp wordt een overzicht van het vergroten/verkleinen Media verwerken met Azure Media Services.
services: media-services
documentationcenter: ''
author: juliako
manager: cfowler
editor: ''
ms.assetid: 780ef5c2-3bd6-4261-8540-6dee77041387
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/30/2018
ms.author: juliako
ms.openlocfilehash: a17c08cc66b13a5ec15d32be7e9ec738da73e219
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/18/2018
ms.locfileid: "46129051"
---
# <a name="scaling-media-processing-overview"></a>Verwerking van Media schalingsoverzicht
Deze pagina geeft een overzicht van hoe en waarom mediaverwerking schalen. 

## <a name="overview"></a>Overzicht
Media Services-accounts worden gekoppeld aan een gereserveerde-eenheidstype, waarmee wordt bepaald hoe snel de mediaverwerkingstaken worden verwerkt. U kunt kiezen uit de volgende gereserveerde-eenheidstypen: **S1**, **S2** en **S3**. Een coderingstaak wordt bijvoorbeeld sneller uitgevoerd wanneer u het gereserveerde-eenheidstype **S2** gebruikt (in vergelijking met het type **S1**). Zie voor meer informatie de [gereserveerde-eenheidstypen](https://azure.microsoft.com/blog/high-speed-encoding-with-azure-media-services/).

Naast het opgeven van de gereserveerde-eenheidstype, kunt u opgeven voor het inrichten van uw account met gereserveerde eenheden. Op basis van het aantal ingerichte, gereserveerde eenheden wordt bepaald hoeveel mediataken tegelijk kunnen worden verwerkt voor een bepaald account. Bijvoorbeeld, als uw account vijf gereserveerde eenheden, heeft en vervolgens vijf mediataken gelijktijdig zo lang zal worden uitgevoerd als er zijn taken die moeten worden verwerkt. De resterende taken in de wachtrij wordt gewacht en wordt u opgehaald voor het verwerken van sequentieel worden verwerkt wanneer een actieve taak is voltooid. Als een account heeft geen alle gereserveerde eenheden worden ingericht, klikt u vervolgens taken opgehaald sequentieel worden verwerkt. In dit geval afhankelijk de wachttijd tussen één taak is voltooid en de volgende begin van de beschikbaarheid van resources in het systeem.

## <a name="choosing-between-different-reserved-unit-types"></a>Kiezen tussen verschillende gereserveerde-eenheidstypen
De volgende tabel kunt u besluit bij de keuze tussen verschillende snelheden voor codering. Ook vindt u enkele benchmark gevallen en biedt SAS-URL's die u gebruiken kunt voor het downloaden van video's waarop u uw eigen tests kunt uitvoeren:

| Scenario's | **S1** | **S2** | **S3** |
| --- | --- | --- | --- |
| Beoogde use-case |Single-bitrate codering. <br/>Bestanden op SD of onder oplossingen, tijd niet gevoelige, lage kosten. |Single-bitrate en meerdere bitrate codering.<br/>Normaal gebruik voor zowel SD en HD-codering. |Single-bitrate en meerdere bitrate codering.<br/>Volledige HD en 4K resolutie van video's. Tijd gevoelige en snellere doorlooptijd codering. |
| Benchmark voor video van 5 minuten |Coderen naar een single-bitrate MP4-bestand met de dezelfde resolutie duurt ongeveer 11 minuten. |Codering met "H264 Single-Bitrate 720p" vooraf duurt ongeveer 5 minuten.<br/><br/>Codering met ' H264 Multiple Bitrate 720p "vooraf ingestelde duurt ongeveer 11,5 minuten. |Codering met "H264 Single-Bitrate 1080p" vooraf duurt ongeveer 2.7 minuten.<br/><br/>Codering met ' H264 Multiple Bitrate 1080p "vooraf ingestelde duurt ongeveer 5,7 minuten. |


## <a name="considerations"></a>Overwegingen
> [!IMPORTANT]
> Bekijk de overwegingen in deze sectie beschreven.  
> 
> 

* S3-eenheidstype wordt voor de analyse van Audio en Video Analysis-taken die worden geactiveerd door Media Services v3 of Video Indexer, sterk aanbevolen.
* Als de gedeelde groep wordt gebruikt, dat wil zeggen, zonder eventuele gereserveerde eenheden, klikt u vervolgens uw coderen taken hebben dezelfde prestaties net als bij de S1-ME. Er is geen bovengrens voor de tijd dat uw taken kunnen besteden aan de status in wachtrij echter en op elk moment maximaal slechts één taak wordt uitgevoerd.

## <a name="billing"></a>Billing

Er worden kosten in rekening gebracht op basis van het werkelijke aantal minuten dat gereserveerde media-eenheden worden gebruikt. Voor een gedetailleerde uitleg, Zie de sectie Veelgestelde vragen over van de [prijzen van Media Services](https://azure.microsoft.com/pricing/details/media-services/) pagina.   

## <a name="quotas-and-limitations"></a>Quota en beperkingen
Zie voor meer informatie over quota en beperkingen en hoe u een ondersteuningsticket [quota en beperkingen](media-services-quotas-and-limitations.md).

## <a name="next-step"></a>Volgende stap
Het vergroten/verkleinen verwerkingstaak voor media met een van deze technologieën bereiken: 

> [!div class="op_single_selector"]
> * [.NET](media-services-dotnet-encoding-units.md)
> * [Portal](media-services-portal-scale-media-processing.md)
> * [REST](https://docs.microsoft.com/rest/api/media/operations/encodingreservedunittype)
> * [Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
> * [PHP](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)
> 

> [!NOTE]
> Zie [Aan de slag met de SDK voor Java-clients voor Azure Media Services](https://docs.microsoft.com/azure/media-services/media-services-java-how-to-use) om de nieuwste versie van de Java-SDK op te halen en te ontwikkelen met Java. <br/>
> Als u de nieuwste PHP-SDK voor Media Services wilt downloaden, zoekt u versie 0.5.7 van het Microsoft/WindowAzure-pakket in de [Packagist-opslagplaats](https://packagist.org/packages/microsoft/windowsazure#v0.5.7).  

## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

