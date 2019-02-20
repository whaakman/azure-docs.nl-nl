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
ms.date: 02/07/2019
ms.author: juliako
ms.custom: mvc
ms.openlocfilehash: 1911b851e4e219ec4c6d2d4872b75e9c18706feb
ms.sourcegitcommit: e51e940e1a0d4f6c3439ebe6674a7d0e92cdc152
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/08/2019
ms.locfileid: "55893320"
---
# <a name="what-is-azure-media-services-v3"></a>Wat is Azure Media Services v3?

Azure Media Services is een cloudgebaseerd platform waarmee u oplossingen bouwt die videostreaming van broadcast-kwaliteit mogelijk maken, de toegankelijkheid en distributie verbeteren, inhoud analyseren en nog veel meer. Ongeacht of u een ontwikkelaar van toepassingen, een callcenter, een overheidsinstelling of een entertainmentbedrijf bent, is Media Services het perfecte hulpmiddel bij het maken van toepassingen die media-ervaringen van uitzonderlijke kwaliteit willen aanbieden aan grote doelgroepen, op alle populaire mobiele apparaten en in de meestgebruikte browsers van dit moment. 

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

## <a name="v3-capabilities"></a>Mogelijkheden van v3

v3 is gebaseerd op een geïntegreerde API-gebied dat functionaliteit voor zowel beheer als bewerkingen beschikbaar stelt die is gebouwd op Azure Resource Manager. 

Deze versie biedt de volgende mogelijkheden:  

* **Transformaties** waarmee u eenvoudige werkstromen voor mediaverwerking of analytische taken kunt definiëren. Transformeren is een proces voor het verwerken van uw video- en audiobestanden. U kunt het proces vervolgens herhaaldelijk toepassen om alle bestanden in de inhoudsbibliotheek te verwerken door het verzenden van taken naar de transformatie.
* **Taken** voor het verwerken (coderen of analyseren) van uw video's. De invoerinhoud voor een taak kan worden opgegeven met HTTPS-URL's, SAS-URL's of paden naar bestanden in Azure Blob-opslag. Op dit moment biedt AMS v3 geen ondersteuning voor gesegmenteerde overdrachtscodering via HTTPS-URL's.
* **Meldingen** voor het controleren van de taakvoortgang of taakstatus, of het starten/stoppen van live kanalen en foutgebeurtenissen. Meldingen zijn geïntegreerd met het meldingssysteem van Azure Event Grid. U kunt zich eenvoudig abonneren op gebeurtenissen voor verschillende resources in Azure Media Services. 
* Gebruik sjablonen van **Azure Resource Management** voor het maken en implementeren van bijvoorbeeld transformaties, streaming-eindpunten en kanalen.
* **Toegangsbeheer op basis van rollen** kan worden ingesteld op het niveau van de resource, zodat u de toegang tot specifieke resources zoals transformaties en kanalen kunt bepalen.
* **Client-SDK's** in meerdere talen: .NET, .NET core, Python, Ga, Java en Node.js.

## <a name="naming-conventions"></a>Naamconventies

Namen van Azure Media Services v3-resources (bijvoorbeeld activa, taken, transformaties) zijn onderhevig aan de naamgevingsbeperkingen van Azure Resource Manager. In overeenstemming met Azure Resource Manager zijn de resourcenamen altijd uniek. U kunt dus alle unieke id-strings (bijvoorbeeld GUID's) gebruiken voor uw resourcenamen. 

Namen van Media Services-resources mogen niet de volgende tekens bevatten: '<', '>', '%', '&', ':', '&#92;', '?', '/', '*', '+', '.', enkele aanhalingstekens of besturingstekens. Alle andere tekens zijn toegestaan. De maximale lengte van een resourcenaam is 260 tekens. 

Zie [Naamvereisten](https://github.com/Azure/azure-resource-manager-rpc/blob/master/v1.0/resource-api-reference.md#arguments-for-crud-on-resource) en [Naamconventies](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions) voor meer informatie over Azure Resource Manager-naamgeving.

## <a name="media-services-v3-api-design-principles"></a>Ontwerpprincipes van Media Services v3-API

Een van de belangrijkste principes van de API v3 is het beter beveiligen van de API. V3 API's retourneren geen geheimen of referenties naar aanleiding van een**Get**- of **List**-bewerking. De sleutels zijn altijd null, leeg of opgeschoond uit het antwoord. U moet een afzonderlijke actiemethode aanroepen voor het ophalen van geheimen of referenties. Door afzonderlijke acties te gebruiken, kunt u verschillende RBAC-beveiligingsrechten instellen voor het geval een aantal API's wel geheimen ophaalt/weergeeft en andere API's dat niet doen. Zie voor meer informatie over het beheren van toegang met RBAC [RBAC gebruiken om toegang te beheren](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-rest).

Voorbeelden hiervan zijn onder andere 

* Er worden geen waarden voor ContentKey geretourneerd in de Get-bewerking van de StreamingLocator; 
* De beperkingssleutels worden niet Key geretourneerd in de Get-bewerking van de ContentKeyPolicy; 
* Het URL-onderdeel met de queryreeks van de HTTP invoer-URL's van taken wordt niet geretourneerd (om de handtekening te verwijderen).

Bekijk het voorbeeld, [Beleid voor inhoudssleutels ophalen - .NET](get-content-key-policy-dotnet-howto.md).

## <a name="how-can-i-get-started-with-v3"></a>Hoe ga ik aan de slag met v3?

Als een ontwikkelaar kunt u de [REST-API](https://go.microsoft.com/fwlink/p/?linkid=873030) van Media Services gebruiken, of clientbibliotheken waarmee u kunt communiceren met de REST-API, om eenvoudig aangepaste mediawerkstromen te maken, beheren en onderhouden. De API van Media Services versie 3 is gebaseerd op de [OpenAPI-specificatie](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/mediaservices/resource-manager/Microsoft.Media) (voorheen bekend als een Swagger).

[Azure Media Services Explorer](https://github.com/Azure/Azure-Media-Services-Explorer) (AMSE) is een hulpprogramma beschikbaar voor Windows-klanten die kennis willen maken met Media Services. AMSE is een Winforms/C#-toepassing voor het uploaden, downloaden, coderen en streamen van VOD en live-inhoud met Media Services. Het AMSE-hulpprogramma is voor clients die Media Services willen testen zonder code te schrijven. De AMSE-code wordt geleverd als een bron voor klanten die willen ontwikkelen met Media Services.

AMSE is een Open-Source-project, en dus wordt er ondersteuning geboden door de community (problemen kunnen worden gemeld aan https://github.com/Azure/Azure-Media-Services-Explorer/issues). Op dit project is de [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/) (Microsoft Open Source-gedragscode) van toepassing. Raadpleeg voor meer informatie de [veelgestelde vragen over de gedragscode](https://opensource.microsoft.com/codeofconduct/faq/) of neem contact op met opencode@microsoft.com als u aanvullende vragen of opmerkingen hebt.
 
Azure Media Services ondersteunt de volgende clientbibliotheken: 

|API-referenties|SDK’s/hulpprogramma’s|Voorbeelden|
|---|---|---|---|
|[REST-ref](https://aka.ms/ams-v3-rest-ref)|[REST SDK](https://aka.ms/ams-v3-rest-sdk)|[REST Postman-voorbeelden](https://github.com/Azure-Samples/media-services-v3-rest-postman)<br/>[REST API op basis van Azure Resource Manager](https://github.com/Azure-Samples/media-services-v3-arm-templates)|
|[Azure CLI-ref](https://aka.ms/ams-v3-cli-ref)|[Azure-CLI](https://aka.ms/ams-v3-cli)|[Azure CLI-voorbeelden](https://github.com/Azure/azure-docs-cli-python-samples/tree/master/media-services)||
|[.NET-ref](https://aka.ms/ams-v3-dotnet-ref)|[.NET SDK](https://aka.ms/ams-v3-dotnet-sdk)|[.NET-voorbeelden](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials)||
||[.NET Core SDK](https://aka.ms/ams-v3-dotnet-sdk) (kies het tabblad **.NET CLI**)|[.NET Core-voorbeelden](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials)||
|[Java-ref](https://aka.ms/ams-v3-java-ref)|[Java SDK](https://aka.ms/ams-v3-java-sdk)||
|[Node.js-ref](https://aka.ms/ams-v3-nodejs-ref)|[Node.js SDK](https://aka.ms/ams-v3-nodejs-sdk)|[Node.js-voorbeelden](https://github.com/Azure-Samples/media-services-v3-node-tutorials)||
|[Python-ref](https://aka.ms/ams-v3-python-ref)|[Python SDK](https://aka.ms/ams-v3-python-sdk)||
|[Go-ref](https://aka.ms/ams-v3-go-ref)|[Go SDK](https://aka.ms/ams-v3-go-sdk)||
|Ruby|[Ruby SDK](https://aka.ms/ams-v3-ruby-sdk)||

## <a name="next-steps"></a>Volgende stappen

Als u wilt zien hoe eenvoudig het is om videobestanden te coderen en streamen, gaat u naar [Snelstartgids: Videobestanden streamen - .NET](stream-files-dotnet-quickstart.md). 

