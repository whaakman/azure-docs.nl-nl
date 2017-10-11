---
title: Pogingslogica in de Media Services SDK voor .NET | Microsoft Docs
description: Het onderwerp overzicht een van Pogingslogica in de Media Services SDK voor .NET.
author: Juliako
manager: cfowler
editor: 
services: media-services
documentationcenter: 
ms.assetid: 527b61a6-c862-4bd8-bcbc-b9aea1ffdee3
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: juliako
ms.openlocfilehash: 859dd76db4ba06196a853469a1385703d835fa22
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="retry-logic-in-the-media-services-sdk-for-net"></a>Pogingslogica in de Media Services SDK voor .NET
Als u werkt met Microsoft Azure-services, kunnen tijdelijke fouten optreden. Als een tijdelijke fout in de meeste gevallen optreedt na een paar pogingen de bewerking is geslaagd. De Media Services SDK voor .NET implementeert de Pogingslogica voor het afhandelen van tijdelijke fouten die zijn gekoppeld aan de uitzonderingen en fouten die worden veroorzaakt door webaanvragen, uitvoeren van query's, het opslaan van wijzigingen en opslagbewerkingen.  Standaard wordt de Media Services SDK voor .NET vier pogingen uitgevoerd voordat de uitzondering voor uw toepassing opnieuw te genereren. Deze uitzondering moet vervolgens goed omgaan met de code in uw toepassing.  

 Hier volgt een korte richtlijn webaanvraag, opslag, Query en SaveChanges beleidsregels:  

* Het beleid voor opslag wordt gebruikt voor bewerkingen voor blob-opslag (uploads of downloaden van assetbestanden).  
* Het beleid webaanvraag wordt gebruikt voor algemene webaanvragen (bijvoorbeeld voor een verificatietoken ophalen en het oplossen van het eindpunt van de cluster gebruikers).  
* Het beleid van de Query wordt gebruikt voor het uitvoeren van query's entiteiten van REST (bijvoorbeeld mediaContext.Assets.Where(...)).  
* Het beleid SaveChanges wordt gebruikt voor het uitvoeren van alles wat gegevens in de service (bijvoorbeeld het maken van een entiteit bijwerken van een entiteit, een servicefunctie aanroepen voor een bewerking) wijzigt.  
  
  Dit onderwerp bevat de typen en Pogingslogica foutcodes die worden verwerkt door de Media Services SDK voor .NET.  

## <a name="exception-types"></a>Uitzondering typen
De volgende tabel beschrijft de uitzonderingen die de Media Services SDK voor .NET worden verwerkt of verwerkt niet voor bepaalde bewerkingen, die tijdelijke fouten kunnen veroorzaken.  

| Uitzondering | Webaanvraag | Storage | Query’s uitvoeren | SaveChanges |
| --- | --- | --- | --- | --- |
| WebException<br/>Zie voor meer informatie de [WebException statuscodes](media-services-retry-logic-in-dotnet-sdk.md#WebExceptionStatus) sectie. |Ja |Ja |Ja |Ja |
| DataServiceClientException<br/> Zie voor meer informatie [HTTP-fout statuscodes](media-services-retry-logic-in-dotnet-sdk.md#HTTPStatusCode). |Nee |Ja |Ja |Ja |
| DataServiceQueryException<br/> Zie voor meer informatie [HTTP-fout statuscodes](media-services-retry-logic-in-dotnet-sdk.md#HTTPStatusCode). |Nee |Ja |Ja |Ja |
| DataServiceRequestException<br/> Zie voor meer informatie [HTTP-fout statuscodes](media-services-retry-logic-in-dotnet-sdk.md#HTTPStatusCode). |Nee |Ja |Ja |Ja |
| DataServiceTransportException |Nee |Nee |Ja |Ja |
| TimeoutException |Ja |Ja |Ja |Nee |
| SocketException |Ja |Ja |Ja |Ja |
| StorageException |Nee |Ja |Nee |Nee |
| IOException |Nee |Ja |Nee |Nee |

### <a name="WebExceptionStatus"></a>Statuscodes WebException
De volgende tabel ziet welke foutcodes WebException de Pogingslogica is geïmplementeerd. De [WebExceptionStatus](http://msdn.microsoft.com/library/system.net.webexceptionstatus.aspx) opsomming definieert de statuscodes.  

| Status | Webaanvraag | Storage | Query’s uitvoeren | SaveChanges |
| --- | --- | --- | --- | --- |
| ConnectFailure |Ja |Ja |Ja |Ja |
| NameResolutionFailure |Ja |Ja |Ja |Ja |
| ProxyNameResolutionFailure |Ja |Ja |Ja |Ja |
| SendFailure |Ja |Ja |Ja |Ja |
| PipelineFailure |Ja |Ja |Ja |Nee |
| ConnectionClosed |Ja |Ja |Ja |Nee |
| KeepAliveFailure |Ja |Ja |Ja |Nee |
| UnknownError |Ja |Ja |Ja |Nee |
| ReceiveFailure |Ja |Ja |Ja |Nee |
| RequestCanceled |Ja |Ja |Ja |Nee |
| Time-out |Ja |Ja |Ja |Nee |
| Een protocolfout <br/>De nieuwe poging op een protocolfout wordt bepaald door de verwerking van HTTP-status code. Zie voor meer informatie [HTTP-fout statuscodes](media-services-retry-logic-in-dotnet-sdk.md#HTTPStatusCode). |Ja |Ja |Ja |Ja |

### <a name="HTTPStatusCode"></a>Statuscodes voor HTTP-fout
Wanneer bewerkingen query's en SaveChanges throw DataServiceClientException, DataServiceQueryException of DataServiceQueryException, wordt de status van HTTP-foutcode in de eigenschap StatusCode geretourneerd.  De volgende tabel ziet welke foutcodes de Pogingslogica is geïmplementeerd.  

| Status | Webaanvraag | Storage | Query’s uitvoeren | SaveChanges |
| --- | --- | --- | --- | --- |
| 401 |Nee |Ja |Nee |Nee |
| 403 |Nee |Ja<br/>Verwerking van nieuwe pogingen met langer wachten op. |Nee |Nee |
| 408 |Ja |Ja |Ja |Ja |
| 429 |Ja |Ja |Ja |Ja |
| 500 |Ja |Ja |Ja |Nee |
| 502 |Ja |Ja |Ja |Nee |
| 503 |Ja |Ja |Ja |Ja |
| 504 |Ja |Ja |Ja |Nee |

Als u bekijken op de werkelijke implementatie van de Media Services SDK voor .NET-Pogingslogica wilt, Zie [azure-sdk-voor-media-services](https://github.com/Azure/azure-sdk-for-media-services/tree/dev/src/net/Client/TransientFaultHandling).

## <a name="next-steps"></a>Volgende stappen
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

