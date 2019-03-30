---
title: Overzicht van Azure Media Services v3 | Microsoft Docs
description: Dit artikel biedt een algemeen overzicht van Media Services en bevat koppelingen naar artikelen voor meer informatie.
services: media-services
documentationcenter: na
author: Juliako
manager: femila
editor: ''
tags: ''
keywords: azure media services, stream, uitzenden, live, offline
ms.service: media-services
ms.devlang: multiple
ms.topic: overview
ms.tgt_pltfrm: multiple
ms.workload: media
ms.date: 03/29/2019
ms.author: juliako
ms.custom: mvc
ms.openlocfilehash: 19e94fc65ddc1719c601397adfe77f8f9445e4fa
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58662122"
---
# <a name="what-is-azure-media-services-v3"></a>Wat is Azure Media Services v3?

Azure Media Services is een cloudgebaseerd platform waarmee u oplossingen bouwt die videostreaming van broadcast-kwaliteit mogelijk maken, de toegankelijkheid en distributie verbeteren, inhoud analyseren en nog veel meer. Ongeacht of u een ontwikkelaar van toepassingen, een callcenter, een overheidsinstelling of een entertainmentbedrijf bent, is Media Services het perfecte hulpmiddel bij het maken van toepassingen die media-ervaringen van uitzonderlijke kwaliteit willen aanbieden aan grote doelgroepen, op alle populaire mobiele apparaten en in de meestgebruikte browsers van dit moment. 

> [!NOTE]
> U kunt de Azure-portal op dit moment niet gebruiken om v3-resources te beheren. Gebruik de [REST-API](https://aka.ms/ams-v3-rest-ref), [CLI](https://aka.ms/ams-v3-cli-ref), of een van de ondersteunde [SDK's](developers-guide.md).

## <a name="what-can-i-do-with-media-services"></a>Wat kan ik doen met Media Services?

Met Media Services kunt u diverse mediawerkstromen bouwen in de cloud. Hier volgen enkele voorbeelden van wat u kunt doen met Media Services.  

* Video's in verschillende indelingen aanbieden, zodat deze kunnen worden afgespeeld in een groot aantal browsers en op diverse apparaten. Voor zowel on demand als live streaming naar verschillende clients (mobiele apparaten, tv, pc, enzovoort) geldt dat de video- en audio-inhoud op de juiste manier moet worden gecodeerd en verpakt. Zie [Snelstart: Bestanden coderen en streamen](stream-files-dotnet-quickstart.md) om te lezen hoe u dergelijke inhoud kunt leveren en streamen.
* Live sportevenementen streamen naar een groot online publiek, zoals voetbalwedstrijden, schaatswedstrijden en grote kampioenschappen. 
* Openbare vergaderingen en evenementen uitzenden, zoals gemeenteraadsvergaderingen, debatten of verkiezingen.
* Opgenomen videobeelden of audio-inhoud analyseren. Zo kunnen organisaties bijvoorbeeld de klanttevredenheid verbeteren door spraak-naar-tekst te extraheren en zoekindexen en dashboards samen te stellen. Vervolgens kunnen ze informatie over veelvoorkomende klachten, bronnen van klachten en andere relevante gegevens verzamelen.
* Een videoservice op abonnementsbasis maken en met DRM beveiligde inhoud streamen wanneer een klant (bijvoorbeeld een filmstudio) de toegang tot en het gebruik van bedrijfseigen, auteursrechtelijk beschermd werk moet beperken.
* Offline inhoud aanbieden voor afspelen in vliegtuigen, treinen en auto's. Klanten willen bijvoorbeeld inhoud downloaden naar hun telefoon of tablet, omdat ze weten dat ze later geen toegang hebben tot het netwerk en dan toch deze inhoud kunnen afspelen.
* Een videoplatform voor e-learning implementeren met Azure Media Services en [API's van Azure Cognitive Servicess](https://docs.microsoft.com/azure/#pivot=products&panel=ai) voor spraak-naar-tekst ondertiteling, vertalen naar meerdere talen, enzovoort. 
* Gebruik Azure Media Services in combinatie met [Azure Cognitive Services API's](https://docs.microsoft.com/azure/#pivot=products&panel=ai) om ondertitels en bijschriften toe te voegen aan video's om een breder publiek te bereiken (bijvoorbeeld mensen met een gehoorbeperking of personen die de video in een andere taal willen volgen).
* Schakel Azure CDN in om grote schaalbaarheid te bieden, zodat piekbelastingen beter kunnen worden verwerkt (bijvoorbeeld bij een productlancering). 

## <a name="naming-conventions"></a>Naamconventies

Namen van Azure Media Services v3-resources (bijvoorbeeld activa, taken, transformaties) zijn onderhevig aan de naamgevingsbeperkingen van Azure Resource Manager. In overeenstemming met Azure Resource Manager zijn de resourcenamen altijd uniek. U kunt dus alle unieke id-strings (bijvoorbeeld GUID's) gebruiken voor uw resourcenamen. 

Namen van Media Services-resources mogen niet de volgende tekens bevatten: '<', '>', '%', '&', ':', '&#92;', '?', '/', '*', '+', '.', enkele aanhalingstekens of besturingstekens. Alle andere tekens zijn toegestaan. De maximale lengte van een resourcenaam is 260 tekens. 

Zie [Naamvereisten](https://github.com/Azure/azure-resource-manager-rpc/blob/master/v1.0/resource-api-reference.md#arguments-for-crud-on-resource) en [Naamconventies](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions) voor meer informatie over Azure Resource Manager-naamgeving.

## <a name="v3-api-design-principles"></a>Ontwerpprincipes voor v3 API

Een van de belangrijkste principes van de API v3 is het beter beveiligen van de API. V3 API's retourneren geen geheimen of referenties naar aanleiding van een**Get**- of **List**-bewerking. De sleutels zijn altijd null, leeg of opgeschoond uit het antwoord. U moet een afzonderlijke actiemethode aanroepen voor het ophalen van geheimen of referenties. Door afzonderlijke acties te gebruiken, kunt u verschillende RBAC-beveiligingsrechten instellen voor het geval een aantal API's wel geheimen ophaalt/weergeeft en andere API's dat niet doen. Zie voor meer informatie over het beheren van toegang met RBAC [RBAC gebruiken om toegang te beheren](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-rest).

Voorbeelden hiervan zijn onder andere 

* Er worden geen waarden voor ContentKey geretourneerd in de Get-bewerking van de StreamingLocator; 
* De beperkingssleutels worden niet Key geretourneerd in de Get-bewerking van de ContentKeyPolicy; 
* Het URL-onderdeel met de queryreeks van de HTTP invoer-URL's van taken wordt niet geretourneerd (om de handtekening te verwijderen).

Bekijk het voorbeeld, [Beleid voor inhoudssleutels ophalen - .NET](get-content-key-policy-dotnet-howto.md).


## <a name="how-can-i-get-started-with-v3"></a>Hoe ga ik aan de slag met v3? 

Informatie over het coderen en inpakken van inhoud, streamen van video's op aanvraag, live uitzenden, analyseren van uw video's met Media Services v3. Zelfstudies, API-verwijzingen en andere documentatie laten zien hoe u veilig on-demand of live video- of audiostromen levert aan miljoenen gebruikers.

### <a name="quickstarts"></a>Snelstartgidsen  

De snelstartgidsen weergeven fundamentele dag-1-instructies voor nieuwe klanten voor het snel uitproberen van Media Services.

* [Stream videobestanden - .NET](stream-files-dotnet-quickstart.md)
* [Stream-video's - CLI](stream-files-cli-quickstart.md)
* [Stream videobestanden - Node.js](stream-files-nodejs-quickstart.md)
    
### <a name="tutorials"></a>Zelfstudies 

De zelfstudies weergeven procedures op basis van een scenario voor een aantal veelgebruikte Media Services-taken

* [Codeer externe bestands- en stream-video-REST](stream-files-tutorial-with-rest.md)
* [Codeer het geüploade bestand en stream-video - .NET](stream-files-tutorial-with-api.md)
* [Stream live - .NET](stream-live-tutorial-with-api.md)
* [Analyseren van uw video - .NET](analyze-videos-tutorial-with-api.md)
* [AES-128 dynamische versleuteling - .NET](protect-with-aes128.md)
    
### <a name="how-to-guides"></a>Handleidingen

Artikelen bevatten voorbeelden van code die laten zien hoe u om een taak te voltooien. In deze sectie vindt u veel voorbeelden, Hier volgen slechts enkele van deze:

* [Een account maken - CLI](create-account-cli-how-to.md)
* [Toegang tot API's - CLI](access-api-cli-how-to.md)
* [Begin te ontwikkelen met SDK 's](developers-guide.md)
* [Coderen met HTTPS als invoer - taak .NET](job-input-from-http-how-to.md)  
* [Monitor voor gebeurtenissen - Portal](monitor-events-portal-how-to.md)
* [Dynamisch versleutelen met multi-DRM - .NET](protect-with-drm.md) 
* [Coderen met een aangepaste transformeren met behulp van CLI](custom-preset-cli-howto.md)

## <a name="next-steps"></a>Volgende stappen

Hoe ga ik aan de slag met v3? 

> [!div class="nextstepaction"]
> [Meer informatie over basisconcepten](concepts-overview.md)<br/>
> [Ontwikkelen met de Media Services v3-API met behulp van SDK 's](developers-guide.md) 

