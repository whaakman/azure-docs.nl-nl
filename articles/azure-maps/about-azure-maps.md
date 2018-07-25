---
title: Overzicht van Azure Maps | Microsoft Docs
description: Een inleiding tot Azure Maps
author: dsk-2015
ms.author: dkshir
ms.date: 07/12/2018
ms.topic: overview
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: 93fe8dc3f8ff991cd6c48923d9e2073e4e93f1ad
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39040844"
---
# <a name="what-is-azure-maps"></a>Wat is Azure Maps?
Azure Maps is een verzameling georuimtelijke services die wordt ondersteund door nieuwe kaartgegevens, waarmee u uw webtoepassingen en mobiele toepassingen kunt voorzien van een nauwkeurige geografische context. Het bevat REST API’s voor het renderen van kaarten en het zoeken van nuttige plaatsen, routes naar nuttige plaatsen, verkeersomstandigheden, tijdzones en services voor IP-adressen naar locatie. U kunt deze API’s gebruiken met vertrouwde hulpprogramma's om snel oplossingen te ontwikkelen en te schalen waarmee locatiegegevens worden geïntegreerd in Azure-oplossingen. Samen met de REST API’s biedt het een webgebaseerd JavaScript-besturingselement om ontwikkeltaken voor meerdere mediums gemakkelijk, flexibel en mobiel te maken. 

In de volgende video wordt Azure Maps in detail uitgelegd:

<iframe src="https://channel9.msdn.com/Shows/Azure-Friday/Azure-Location-Based-Services/player" width="960" height="540" allowFullScreen frameBorder="0"></iframe>

## <a name="services-in-azure-maps"></a>Services in Azure Maps

Azure Maps bestaat uit de volgende zes services die uw Azure-toepassingen van geografische context kunnen voorzien. 

### <a name="render-service"></a>Service voor rendering

De Render Service is ontworpen voor ontwikkelaars zodat zij webtoepassingen en mobiele toepassingen kunnen maken voor het maken van kaarten. De service maakt gebruik van een raster met grafische afbeeldingen van hoge kwaliteit, beschikbaar in 19 zoomniveaus, of volledig aanpasbare kaartafbeeldingen in een vectorindeling.

![Azure Maps Map.png](media/about-azure-maps/Introduction_Map.png)

De Render Service biedt nu preview-API's die ontwikkelaars met satellietbeelden laten werken. Lees de pagina [Render-API's voor Azure Maps](https://docs.microsoft.com/rest/api/maps/render) voor meer informatie.


### <a name="route-service"></a>Routeservice 

De Route Service bevat robuuste geometrische berekeningen voor de infrastructuur in de echte wereld en routebeschrijvingen voor meerdere manieren van reizen. Met de service kunnen ontwikkelaars richtingen berekenen voor een aantal manieren van reizen, zoals met de auto, vrachtwagen, fiets of wandelen. In de service kan ook rekening worden gehouden met invoerwaarden voor verkeersomstandigheden, gewichtsbeperkingen of transport van gevaarlijke stoffen.

![Azure Maps Route.png](media/about-azure-maps/Introduction_Route.png)

De Route Service biedt nu een preview voor geavanceerde functies, zoals batchverwerking van meerdere aanvragen voor routes, matrices voor de reistijd en afstand tussen een set met herkomsten en bestemmingen, en het vinden van routes of afstanden die u kunt afleggen op basis van de tijd die u hebt of van de benodigde hoeveelheid brandstof. Lees de pagina [Routering-API’s voor Azure Maps](https://docs.microsoft.com/rest/api/maps/route) voor meer informatie over de routeringsmogelijkheden.


### <a name="search-service"></a>Zoekservice

De Search Service is ontworpen voor ontwikkelaars zodat zij naar adressen, plaatsen, bedrijfsvermeldingen op naam of categorie, en andere geografische informatie kunnen zoeken. Met de Search Service kan ook [de geocode worden omgekeerd](https://en.wikipedia.org/wiki/Reverse_geocoding) voor adressen en dwarsstraten op basis van breedte- en lengtegraden. 

![Azure Maps Search.png](media/about-azure-maps/Introduction_Search.png)

De Search Service biedt ook geavanceerde functies, zoals zoeken langs een route, zoeken binnen een groter gebied, een groep zoekaanvragen samenvoegen tot een batch en zoeken naar een groter gebied in plaats van naar een specifieke locatie. Er zijn momenteel preview-versies beschikbaar van API’s voor zoeken in batches en zoeken naar gebieden. Lees de pagina [Zoek-API's voor Azure Maps](https://docs.microsoft.com/rest/api/maps/search) voor meer informatie over de zoekmogelijkheden.


### <a name="time-zone-service"></a>Time Zone Service

Met de Time Zone Service kunt u actuele, historische en toekomstige tijdzonegegevens opvragen met behulp van breedtegraad/lengtegraad-combinaties of een [IANA-id](http://www.iana.org/). Met de Time Zone service kunnen ook id’s voor Microsoft Windows-tijdzones worden geconverteerd naar IANA-tijdzones, door een tijdzoneverschil naar UTC op te halen en de huidige tijd op te halen in een respectieve tijdzone. Een typisch JSON-antwoord voor een query in de Time Zone service ziet er als volgt uit:

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

Ga naar de pagina [Tijdzone-API’s voor Azure Maps](https://docs.microsoft.com/rest/api/maps/timezone) voor meer informatie over deze service.

### <a name="traffic-service"></a>Verkeersservice

De Traffic Service is een suite met webservices die is ontworpen voor ontwikkelaars zodat zij webtoepassingen en mobiele toepassingen kunnen maken die te maken hebben met verkeer. De service omvat twee gegevenstypen:
    * Verkeersstroom - waargenomen snelheden en reistijden in realtime voor alle belangrijke wegen in het netwerk. 
    * Verkeersincidenten - een nauwkeurige weergave van files en incidenten op het wegennet.

![Azure Maps Traffic](media/about-azure-maps/Introduction_Traffic.png)

Ga naar de pagina [Verkeers-API’s voor Azure Maps](https://docs.microsoft.com/rest/api/maps/traffic) voor meer informatie.

### <a name="ip-to-location"></a>IP-adres naar locatie

IP-adres naar locatie is een preview-service waarmee u de tweeletterige landcode voor een bepaald IP-adres kunt ophalen. Met deze service kunt u uw toepassing aanpassen om aan speciale geopolitieke beperkingen te voldoen en om de gebruikerservaring te verbeteren doordat de inhoud van de toepassing wordt gewijzigd op basis van de geografische locatie. 


## <a name="programming-model"></a>Programmeermodel

Azure Maps is gebouwd voor mobiliteit en kan platformoverschrijdende toepassingen aansturen. Het maakt gebruik van een programmeermodel dat taalonafhankelijk is en dat JSON-uitvoer ondersteunt door middel van [REST API's](https://docs.microsoft.com/rest/api/maps/). 

Bovendien biedt Azure Maps een handig [kaartbesturingselement in JavaScript](https://docs.microsoft.com/javascript/api/azure-maps-javascript/?view=azure-iot-typescript-latest) met een eenvoudig programmeermodel voor het snel en gemakkelijk ontwikkelen van webtoepassingen en mobiele toepassingen. 


## <a name="usage"></a>Gebruik

Voor toegang tot de services van Azure Maps gaat u naar [Azure Portal](http://portal.azure.com) en maakt u een Azure Maps-account. 

Azure Maps maakt gebruik van een verificatieschema op basis van sleutels. Uw account wordt geleverd met twee sleutels die vooraf voor u zijn gegenereerd. U kunt deze locatiemogelijkheden direct integreren in uw toepassingen met behulp van een van de sleutels in de aanvragen voor de Azure Maps-service.

## <a name="supported-regions"></a>Ondersteunde regio’s
De Azure Maps-API is momenteel wereldwijd beschikbaar, met uitzondering van de volgende landen: 

* Argentinië
* China
* India
* Marokko
* Pakistan
* Zuid-Korea

Controleer uw huidige IP-adres en verifieer of de locatie van uw IP-adres niet in een van de bovenstaande landen is, die niet worden ondersteund.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over de nieuwe functies van Azure Maps: 
    - [Routematrix, isochronen, IP-adres zoeken en meer](https://azure.microsoft.com/blog/route-matrix-isochrones-ip-lookup-and-more-added-to-azure-maps/). 
- U kunt een voorbeeld-app met de service uitproberen
    - [Demo van interactief zoeken naar kaarten starten](quick-demo-map-app.md)
