---
title: Overzicht van Azure-locatie op basis van Services | Microsoft Docs
description: Een inleiding tot Azure locatie op basis van Services (preview)
services: location-based-services
keywords: 
author: dsk-2015
ms.author: dkshir
ms.date: 11/28/2017
ms.topic: article
ms.service: location-based-services
documentationcenter: 
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: c8ebce06a72bcaf769a11ec954702463d7489aa0
ms.sourcegitcommit: 310748b6d66dc0445e682c8c904ae4c71352fef2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2017
---
# <a name="an-introduction-to-azure-location-based-services-preview"></a>Een inleiding tot Azure locatie op basis van Services (preview)
Services voor Azure op basis van locatie is een portfolio van georuimtelijke-services die service API's voor kaarten, Search, routering, verkeer en tijdzones bevatten. De portfolio van compatibele OneAPI Azure-services kunt u bekend ontwikkelhulpprogramma's gebruiken voor het snel ontwikkelen en scale-oplossingen die informatie over de locatie in uw Azure-oplossingen integreren. Services voor Azure op basis van locatie biedt ontwikkelaars van alle branches georuimtelijke krachtige mogelijkheden die zijn verpakt met nieuwe toewijzing gegevens imperatieve te bieden geografische context voor webtoepassingen en mobiele toepassingen. Services voor Azure op basis van locatie is een Azure één API compatibele REST-API's samen met een JavaScript-web gebaseerde besturingselement aanbrengen ontwikkeling super eenvoudig en flexibel draagbare over meerdere media set. 

Services voor Azure op basis van locatie bestaat uit vijf belangrijke services aan het Azure-toepassingen vereisen geografische context vergroot. Elk van de services wordt hieronder in detail uitgelegd.

**Renderen Service** – de renderen-Service is ontworpen voor ontwikkelaars voor het maken van webtoepassingen en mobiele toepassingen om de toewijzing. De service maakt gebruik van hoge kwaliteit raster grafische afbeeldingen, beschikbaar in 19 zoomniveaus of Kaartafbeeldingen volledig aanpasbare vector-indeling.

![Azure-locatie op basis van Services Map.png](media/about-location-based-services/Introduction_Map.png)

**Route Service** – de Route-Service is gebouwd met een robuuste infrastructuur voor echte geometrie berekeningen en meerdere vervoer modus richtingen. De service worden toegestaan voor ontwikkelaars voor het berekenen van aanwijzingen over een aantal reizen modi zoals auto, vrachtwagen, fiets of lopen; evenals, een aantal invoerwaarden zoals verkeer voorwaarden, gewichtsbeperkingen of gevaarlijke materiaal transport.

![Azure-locatie op basis van Services Route.png](media/about-location-based-services/Introduction_Route.png)

**Search-Service** – de Search-Service is ontworpen voor ontwikkelaars om te zoeken naar adressen, plaatsen, lijsten met de naam of categorie bedrijven en andere geografische informatie. De Search-Service kan ook [geocode reverse](https://en.wikipedia.org/wiki/Reverse_geocoding) adressen en cross streets op basis van een breedtegraad/lengtegraad. 

![Azure-locatie op basis van Services Search.png](media/about-location-based-services/Introduction_Search.png)

**Tijdzone Service** – de tijdzone-Service kunt u huidige, historische en toekomstige tijdzone-informatie van een query met behulp van de breedtegraad lengtegraad paren of een [IANA-ID](http://www.iana.org/). De tijdzone-service staat ook tijdzone-id's van Microsoft Windows converteren naar IANA tijdzones, een tijdzoneverschil naar UTC ophalen en ophalen van de huidige tijd in een bijbehorende tijdzone. Een typische JSON-antwoord voor een query met de tijdzone-Service ziet er als volgt:

```JSON
{
    "Version": "2017c",
    "ReferenceUtcTimestamp": "2017-11-20T23:09:48.686173Z",
    "TimeZones": [{
        "Id": "America/Los_Angeles",
        "ReferenceTime": {
            "Tag": "PST",
            "StandardOffset": "-08:00:00",
            "DaylightSavings": "00:00:00",
            "WallTime": "2017-11-20T15:09:48.686173-08:00",
            "PosixTzValidYear": 2017,
            "PosixTz": "PST+8PDT,M3.2.0,M11.1.0"
        }
    }]
}
```

**Verkeer van de Service** – de verkeer-Service is een reeks webservices die zijn ontworpen voor ontwikkelaars voor het maken van webtoepassingen en mobiele toepassingen vereisen van verkeer. De aanbieding is onderverdeeld in het volgende:
1. Verkeer van de stroom - biedt realtime waargenomen snelheden en reizen tijden voor alle belangrijke wegen in het netwerk. en, 
2. Verkeer van incidenten - biedt een nauwkeurige weergave over de Files en incidenten rond het netwerk weg.

![Azure-locatie op basis van verkeer](media/about-location-based-services/Introduction_Traffic.png)

Services voor Azure op basis van locatie is gebouwd voor mobiliteit en toepassingen voor meerdere platforms kunt inschakelen omdat het programmeermodel agnostisch is en JSON-uitvoer via REST API's ondersteunt. Daarnaast biedt Azure LBS een handige JavaScript-Kaartbesturingselement met een eenvoudige programmeermodel voor snelle eenvoudig ontwikkeling van web- en mobiele toepassingen. 

Services voor Azure op basis van locatie maakt gebruik van een schema voor verificatie op basis van sleutels, zodat toegang tot de services is een kwestie van het navigeren naar de [Azure-portal](http://portal.azure.com) en het maken van een locatie op basis van Azure-Services-account. Uw account wordt geleverd met twee sleutels vooraf voor u gegenereerd. Start deze mogelijkheden locatie rechtstreeks in uw toepassingen integreren met een van uw sleutels in de aanvragen naar de locatie op basis van Azure-Services-service.

Aanmelden voor een [Services van Azure-locatie op basis van account vandaag!](http://aka.ms/azurelbsportal)

## <a name="next-steps"></a>Volgende stappen

U hebt nu een overzicht van de Azure op basis van Locatiediensten (preview). De volgende stap is het uitproberen van een voorbeeld-app met de Services op basis van locatie, evenals een end-to-end-scenario in uw webtoepassing te maken.

> [!div class="nextstepaction"]
> [Een demo interactieve kaart search met behulp van Azure op basis van Locatieservices (preview) starten](quick-demo-map-app.md)
> [zoeken in de buurt interessante met Azure locatie op basis van Services](tutorial-search-location.md)