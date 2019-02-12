---
title: Verwerking van Media schalingsoverzicht | Microsoft Docs
description: In dit onderwerp wordt een overzicht van het vergroten/verkleinen Media verwerken met Azure Media Services.
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
ms.date: 02/08/2019
ms.author: juliako
ms.openlocfilehash: a6549c68ec248720c81531d137d45909a4de55e9
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/11/2019
ms.locfileid: "55998396"
---
# <a name="scaling-media-processing-overview"></a>Verwerking van Media schalingsoverzicht 
Deze pagina geeft een overzicht van hoe en waarom mediaverwerking schalen. 

## <a name="overview"></a>Overzicht
Media Services-accounts worden gekoppeld aan een gereserveerde-eenheidstype, waarmee wordt bepaald hoe snel de mediaverwerkingstaken worden verwerkt. U kunt kiezen uit de volgende gereserveerde-eenheidstypen: **S1**, **S2**, of **S3**. Een coderingstaak wordt bijvoorbeeld sneller uitgevoerd wanneer u het gereserveerde-eenheidstype **S2** gebruikt (in vergelijking met het type **S1**). Zie voor meer informatie de [gereserveerde-eenheidstypen](https://azure.microsoft.com/blog/high-speed-encoding-with-azure-media-services/).

Naast het opgeven van de gereserveerde-eenheidstype, kunt u opgeven voor het inrichten van uw account met gereserveerde eenheden. Op basis van het aantal ingerichte, gereserveerde eenheden wordt bepaald hoeveel mediataken tegelijk kunnen worden verwerkt voor een bepaald account. Bijvoorbeeld, als uw account vijf gereserveerde eenheden, heeft en vervolgens vijf mediataken gelijktijdig zo lang zal worden uitgevoerd als er zijn taken die moeten worden verwerkt. De resterende taken in de wachtrij wordt gewacht en wordt u opgehaald voor het verwerken van sequentieel worden verwerkt wanneer een actieve taak is voltooid. Als een account heeft geen alle gereserveerde eenheden worden ingericht, klikt u vervolgens taken opgehaald sequentieel worden verwerkt. In dit geval afhankelijk de wachttijd tussen één taak is voltooid en de volgende begin van de beschikbaarheid van resources in het systeem.

## <a name="choosing-between-different-reserved-unit-types"></a>Kiezen tussen verschillende gereserveerde-eenheidstypen
De volgende tabel kunt u een beslissing te nemen bij de keuze tussen verschillende snelheden voor codering. Het bevat ook enkele benchmark gevallen op [een video die u kunt downloaden](https://nimbuspmteam.blob.core.windows.net/asset-46f1f723-5d76-477e-a153-3fd0f9f90f73/SeattlePikePlaceMarket_7min.ts?sv=2015-07-08&sr=c&si=013ab6a6-5ebf-431e-8243-9983a6b5b01c&sig=YCgEB8DxYKK%2B8W9LnBykzm1ZRUTwQAAH9QFUGw%2BIWuc%3D&se=2118-09-21T19%3A28%3A57Z) het uitvoeren van uw eigen tests:

|RU-type|Scenario|Voorbeeld van resultaten voor de [7 min 1080 p video](https://nimbuspmteam.blob.core.windows.net/asset-46f1f723-5d76-477e-a153-3fd0f9f90f73/SeattlePikePlaceMarket_7min.ts?sv=2015-07-08&sr=c&si=013ab6a6-5ebf-431e-8243-9983a6b5b01c&sig=YCgEB8DxYKK%2B8W9LnBykzm1ZRUTwQAAH9QFUGw%2BIWuc%3D&se=2118-09-21T19%3A28%3A57Z)|
|---|---|---|
| **S1**|Single-bitrate codering. <br/>Bestanden op SD of onder oplossingen, tijd niet gevoelige, lage kosten.|Codering op single bitrate SD resolutie MP4-bestand met behulp van 'H264 Single Bitrate SD 16 x 9' duurt tien minuten.|
| **S2**|Single-bitrate en meerdere bitrate codering.<br/>Normaal gebruik voor zowel SD en HD-codering.|Codering met "H264 Single-Bitrate 720p" vooraf duurt ongeveer 8 minuten.<br/><br/>Codering met ' H264 Multiple Bitrate 720p "vooraf ingestelde duurt ongeveer 16,8 minuten.|
| **S3**|Single-bitrate en meerdere bitrate codering.<br/>Volledige HD en 4K resolutie van video's. Tijd gevoelige en snellere doorlooptijd codering.|Codering met "H264 Single-Bitrate 1080p" vooraf duurt ongeveer vier minuten.<br/><br/>Codering met ' H264 Multiple Bitrate 1080p "vooraf ingestelde duurt ongeveer 8 minuten.|

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

