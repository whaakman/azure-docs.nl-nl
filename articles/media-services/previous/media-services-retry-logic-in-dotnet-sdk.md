---
title: Pogingslogica in de Media Services SDK voor .NET | Microsoft Docs
description: Het onderwerp biedt een overzicht van de logica voor opnieuw proberen in de Media Services SDK voor .NET.
author: Juliako
manager: femila
editor: ''
services: media-services
documentationcenter: ''
ms.assetid: 527b61a6-c862-4bd8-bcbc-b9aea1ffdee3
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/08/2019
ms.author: juliako
ms.openlocfilehash: 909a68ff0fd78fbdd4870506d1ad579392036dbf
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/11/2019
ms.locfileid: "55999195"
---
# <a name="retry-logic-in-the-media-services-sdk-for-net"></a>Pogingslogica in de Media Services SDK voor .NET  

Als u werkt met Microsoft Azure-services, kunnen tijdelijke fouten optreden. Als er een tijdelijke fout optreedt in de meeste gevallen na een paar nieuwe pogingen de bewerking is geslaagd. De Media Services SDK voor .NET implementeert de logica voor nieuwe pogingen voor het afhandelen van tijdelijke fouten die zijn gekoppeld aan de uitzonderingen en fouten die worden veroorzaakt door webaanvragen, uitvoeren van query's, het opslaan van wijzigingen en -opslagbewerkingen.  Standaard wordt de Media Services SDK voor .NET vier pogingen uitgevoerd voordat de uitzondering aan uw toepassing opnieuw te genereren. Deze uitzondering moet vervolgens correct verwerken door de code in uw toepassing.  

 Hier volgt een korte richtlijn webaanvraag, opslag, query's en SaveChanges beleid:  

* Het beleid voor opslag wordt gebruikt voor blob-opslagbewerkingen (uploaden of downloaden van assetbestanden).  
* Het beleid voor Web wordt gebruikt voor algemene webaanvragen (bijvoorbeeld voor een verificatietoken ophalen en het oplossen van het clustereindpunt gebruikers).  
* Het beleid van de Query wordt gebruikt voor het uitvoeren van query's entiteiten op basis van REST (bijvoorbeeld mediaContext.Assets.Where(...)).  
* Het beleid SaveChanges wordt gebruikt voor het uitvoeren van alles wat gegevens in de service (bijvoorbeeld, het maken van een entiteit bijwerken van een entiteit, een door de servicefunctie voor een bewerking aanroepen) wordt gewijzigd.  
  
  In dit onderwerp geeft een lijst van de uitzonderingstypen en foutcodes die worden verwerkt door de Media Services SDK voor .NET-logica voor opnieuw proberen.  

## <a name="exception-types"></a>Uitzonderingstypen
De volgende tabel beschrijft de uitzonderingen die de Media Services SDK voor .NET worden verwerkt of verwerkt niet voor bepaalde bewerkingen dat leiden tijdelijke fouten tot kunnen.  

| Uitzondering | Webaanvraag | Opslag | Query’s uitvoeren | SaveChanges |
| --- | --- | --- | --- | --- |
| WebException<br/>Zie voor meer informatie de [WebException statuscodes](media-services-retry-logic-in-dotnet-sdk.md#WebExceptionStatus) sectie. |Ja |Ja |Ja |Ja |
| DataServiceClientException<br/> Zie voor meer informatie, [statuscodes voor HTTP-fout](media-services-retry-logic-in-dotnet-sdk.md#HTTPStatusCode). |Nee |Ja |Ja |Ja |
| DataServiceQueryException<br/> Zie voor meer informatie, [statuscodes voor HTTP-fout](media-services-retry-logic-in-dotnet-sdk.md#HTTPStatusCode). |Nee |Ja |Ja |Ja |
| DataServiceRequestException<br/> Zie voor meer informatie, [statuscodes voor HTTP-fout](media-services-retry-logic-in-dotnet-sdk.md#HTTPStatusCode). |Nee |Ja |Ja |Ja |
| DataServiceTransportException |Nee |Nee |Ja |Ja |
| TimeoutException |Ja |Ja |Ja |Nee |
| SocketException |Ja |Ja |Ja |Ja |
| StorageException |Nee |Ja |Nee |Nee |
| IOException |Nee |Ja |Nee |Nee |

### <a name="WebExceptionStatus"></a> Statuscodes WebException
De volgende tabel ziet u welke foutcodes WebException logica voor opnieuw proberen is geïmplementeerd. De [WebExceptionStatus](https://msdn.microsoft.com/library/system.net.webexceptionstatus.aspx) opsomming definieert de statuscodes.  

| Status | Webaanvraag | Opslag | Query’s uitvoeren | SaveChanges |
| --- | --- | --- | --- | --- |
| ConnectFailure |Ja |Ja |Ja |Ja |
| NameResolutionFailure |Ja |Ja |Ja |Ja |
| ProxyNameResolutionFailure |Ja |Ja |Ja |Ja |
| SendFailure |Ja |Ja |Ja |Ja |
| PipelineFailure |Ja |Ja |Ja |Nee |
| ConnectionClosed |Ja |Ja |Ja |Nee |
| KeepAliveFailure |Ja |Ja |Ja |Nee |
| Errorcodes.unknownerror |Ja |Ja |Ja |Nee |
| ReceiveFailure |Ja |Ja |Ja |Nee |
| RequestCanceled |Ja |Ja |Ja |Nee |
| Time-out |Ja |Ja |Ja |Nee |
| ProtocolError <br/>De nieuwe poging op een protocolfout wordt bepaald door de verwerking van HTTP-status code. Zie voor meer informatie, [statuscodes voor HTTP-fout](media-services-retry-logic-in-dotnet-sdk.md#HTTPStatusCode). |Ja |Ja |Ja |Ja |

### <a name="HTTPStatusCode"></a> Statuscodes voor HTTP-fout
Wanneer bewerkingen query's en SaveChanges throw DataServiceClientException, DataServiceQueryException of DataServiceQueryException, wordt de statuscode van de HTTP-fout in de eigenschap StatusCode geretourneerd.  De volgende tabel ziet u welke foutcodes logica voor opnieuw proberen is geïmplementeerd.  

| Status | Webaanvraag | Opslag | Query’s uitvoeren | SaveChanges |
| --- | --- | --- | --- | --- |
| 401 |Nee |Ja |Nee |Nee |
| 403 |Nee |Ja<br/>Nieuwe pogingen met langer wachten op verwerking. |Nee |Nee |
| 408 |Ja |Ja |Ja |Ja |
| 429 |Ja |Ja |Ja |Ja |
| 500 |Ja |Ja |Ja |Nee |
| 502 |Ja |Ja |Ja |Nee |
| 503 |Ja |Ja |Ja |Ja |
| 504 |Ja |Ja |Ja |Nee |

Als u bekijken op de werkelijke implementatie van de Media Services SDK voor .NET-logica voor opnieuw proberen wilt, Zie [azure-sdk-voor-media-services](https://github.com/Azure/azure-sdk-for-media-services/tree/dev/src/net/Client/TransientFaultHandling).

## <a name="next-steps"></a>Volgende stappen
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

