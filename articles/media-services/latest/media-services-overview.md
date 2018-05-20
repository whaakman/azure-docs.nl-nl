---
title: Overzicht van Azure Media Services v3 | Microsoft Docs
description: Dit artikel biedt een algemeen overzicht van Media Services en bevat koppelingen naar artikelen voor meer informatie.
services: media-services
documentationcenter: na
author: Juliako
manager: cfowler
editor: ''
tags: ''
keywords: azure media services, stream, uitzenden, live, offline
ms.service: media-services
ms.devlang: multiple
ms.topic: overview
ms.tgt_pltfrm: multiple
ms.workload: media
ms.date: 03/27/2018
ms.author: juliako
ms.custom: mvc
ms.openlocfilehash: c5c719d484684c0efff87671ba7e012d9bd7699e
ms.sourcegitcommit: c52123364e2ba086722bc860f2972642115316ef
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/11/2018
---
# <a name="what-is-azure-media-services-v3"></a>Wat is Azure Media Services v3?

> [!div class="op_single_selector" title1="Select the version of Media Services that you are using:"]
> * [Versie 2 - Algemeen beschikbaar](../previous/media-services-overview.md)
> * [Versie 3 - Preview](media-services-overview.md)

> [!NOTE]
> De nieuwste versie van Azure Media Services is momenteel in preview en wordt ook wel v3 of versie 3 genoemd.

Azure Media Services is een cloudgebaseerd platform waarmee u oplossingen bouwt die videostreaming van broadcast-kwaliteit mogelijk maken, de toegankelijkheid en distributie verbeteren, inhoud analyseren en nog veel meer. Ongeacht of u een ontwikkelaar van toepassingen, een callcenter, een overheidsinstelling of een entertainmentbedrijf bent, is Media Services het perfecte hulpmiddel bij het maken van toepassingen die media-ervaringen van uitzonderlijke kwaliteit willen aanbieden aan grote doelgroepen, op alle populaire mobiele apparaten en in de meestgebruikte browsers van dit moment. 

## <a name="what-can-i-do-with-media-services"></a>Wat kan ik doen met Media Services?

Met Media Services kunt u diverse mediawerkstromen bouwen in de cloud. Hier volgen enkele voorbeelden van wat u kunt doen met Media Services.  

* Video's in verschillende indelingen aanbieden, zodat deze kunnen worden afgespeeld in een groot aantal browsers en op diverse apparaten. Voor zowel on demand als live streaming naar verschillende clients (mobiele apparaten, tv, pc, enzovoort) geldt dat de video- en audio-inhoud op de juiste manier moet worden gecodeerd en verpakt. Zie [Snelstartgids: Videobestanden streamen - .NET](stream-files-dotnet-quickstart.md) om te lezen hoe u dergelijke inhoud kunt leveren en streamen.
* Live sportevenementen streamen naar een groot online publiek, zoals voetbalwedstrijden, schaatswedstrijden en grote kampioenschappen. 
* Openbare vergaderingen en evenementen uitzenden, zoals gemeenteraadsvergaderingen, debatten of verkiezingen.
* Opgenomen videobeelden of audio-inhoud analyseren. Zo kunnen organisaties bijvoorbeeld de klanttevredenheid verbeteren door spraak-naar-tekst te extraheren en zoekindexen en dashboards samen te stellen. Vervolgens kunnen ze informatie over veelvoorkomende klachten, bronnen van klachten en andere relevante gegevens verzamelen. 
* Een videoservice op abonnementsbasis maken en met DRM beveiligde inhoud streamen wanneer een klant (bijvoorbeeld een filmstudio) de toegang tot en het gebruik van bedrijfseigen, auteursrechtelijk beschermd werk moet beperken.
* Offline inhoud aanbieden voor afspelen in vliegtuigen, treinen en auto's. Klanten willen bijvoorbeeld inhoud downloaden naar hun telefoon of tablet, omdat ze weten dat ze later geen toegang hebben tot het netwerk en dan toch deze inhoud kunnen afspelen.
* Ondertitels en bijschriften toevoegen aan video's om een breder publiek te bereiken (bijvoorbeeld mensen met een gehoorbeperking of personen die de video in een andere taal willen volgen). 
* Een videoplatform voor e-learning implementeren met Azure Media Services en [API's van Azure Cognitive Servicess](https://docs.microsoft.com/en-us/azure/#pivot=products&panel=ai) voor spraak-naar-tekst ondertiteling, vertalen naar meerdere talen, enzovoort.
* Azure CDN in staat stellen om grote schaalbaarheid te bieden, zodat piekbelastingen beter kunnen worden verwerkt (bijvoorbeeld bij een productlancering). 

## <a name="v3-capabilities"></a>Mogelijkheden van v3

v3 is gebaseerd op een geïntegreerde API-gebied dat functionaliteit voor zowel beheer als bewerkingen beschikbaar stelt die is gebouwd op **Azure Resource Manager**. Deze versie biedt de volgende mogelijkheden:  

* **Transformaties** waarmee u eenvoudige werkstromen voor mediaverwerking of analytische taken kunt definiëren. Transformeren is een proces voor het verwerken van uw video- en audiobestanden. U kunt het proces vervolgens herhaaldelijk toepassen om alle bestanden in de inhoudsbibliotheek te verwerken door het verzenden van taken naar de transformatie.
* **Taken** voor het verwerken (coderen of analyseren) van uw video's. De invoerinhoud voor een taak kan worden opgegeven met HTTP (s) URL's, SAS-URL's of paden naar bestanden in Azure Blob-opslag. 
* **Meldingen** voor het controleren van de taakvoortgang of taakstatus, of het starten/stoppen van live kanalen en foutgebeurtenissen. Meldingen zijn geïntegreerd met het meldingssysteem van Azure Event Grid. U kunt zich eenvoudig abonneren op gebeurtenissen voor verschillende resources in Azure Media Services. 
* Gebruik sjablonen van **Azure Resource Management** voor het maken en implementeren van bijvoorbeeld transformaties, streaming-eindpunten en kanalen.
* **Toegangsbeheer op basis van rollen** kan worden ingesteld op het niveau van de resource, zodat u de toegang tot specifieke resources zoals transformaties en kanalen kunt bepalen.
* **Client-SDK's** in meerdere talen: .NET, .NET core, Python, Ga, Java en Node.js.

## <a name="how-can-i-get-started-with-v3"></a>Hoe ga ik aan de slag met v3?

Als een ontwikkelaar kunt u de [REST-API](https://go.microsoft.com/fwlink/p/?linkid=873030) van Media Services gebruiken, of clientbibliotheken waarmee u kunt communiceren met de REST-API, om eenvoudig aangepaste mediawerkstromen te maken, beheren en onderhouden. Microsoft genereert en ondersteunt de volgende clientbibliotheken: 

* [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)
* [.NET-talen](https://www.nuget.org/packages/Microsoft.Azure.Management.Media/1.0.0)
* [.NET Core](https://www.nuget.org/packages/Microsoft.Azure.Management.Media/1.0.0) (kies het tabblad **.NET CLI**)
* Java

  Voeg de volgende afhankelijkheid toe aan uw project:
  
  ```
  <dependency>
    <groupId>com.microsoft.azure.media-2018-03-30-preview</groupId>
    <artifactId>azure-mgmt- media</artifactId>
    <version>0.0.1-beta</version>
  </dependency> 
  ```
* Node.js 

  Gebruik de volgende opdracht:
  
  ```
  npm install azure-arm-mediaservices
  ```
  
* [Python](https://pypi.org/project/azure-mgmt-media/1.0.0rc1/)
* [Go](https://github.com/Azure/azure-sdk-for-go/tree/master/services/preview/mediaservices/mgmt/2018-03-30-preview/media)

Media Services biedt [Swagger-bestanden](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/mediaservices/resource-manager/Microsoft.Media) die u kunt gebruiken voor het genereren van SDK's voor de gewenste taal/technologie.  

## <a name="next-steps"></a>Volgende stappen

Als u wilt zien hoe eenvoudig het is om videobestanden te coderen en streamen, gaat u naar [Snelstartgids: Videobestanden streamen - .NET](stream-files-dotnet-quickstart.md). 

