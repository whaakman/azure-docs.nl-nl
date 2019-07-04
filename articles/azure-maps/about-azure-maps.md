---
title: Overzicht van Azure Maps | Microsoft Docs
description: Een inleiding tot Azure Maps
author: walsehgal
ms.author: v-musehg
ms.date: 02/04/2019
ms.topic: overview
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: 5f41263113568cf9f3771119135be8db37119181
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67442943"
---
# <a name="what-is-azure-maps"></a>Wat is Azure Maps?

Azure kaarten is een verzameling van georuimtelijke services die gebruikmaken van nieuwe kaartgegevens voor nauwkeurige geografische context voor web- en mobiele toepassingen. Azure Maps biedt:

* REST-API's voor rendering van kaarten in verschillende stijlen en satelliet beelden.
* Hiermee zoekt u naar adressen, plaatsen en bezienswaardigheden over de hele wereld.
* Routering point-to-point, multipoint, multipoint optimalisatie, isochrone, commerciële voertuigen, verkeer beïnvloed en matrix routering; netwerkverkeer en incidenten weergeven.
* Mobility-services voor het aanvragen van openbare doorvoer en alternatieve modi van transport (zoals bike share, scooter delen en auto-share) en plannen van routes in realtime. 
* Tot stand brengen van de locatie van de gebruiker via geolocatie en locatie omzetten in tijdzones. 
* Services voor geofencing en kaart gegevensopslag, met informatie over de locatie die wordt gehost in Azure. 
* Locatie intelligence via georuimtelijke analyses. 

Azure kaarten-services zijn naast REST-API's beschikbaar via de Web SDK of de Android SDK. Deze hulpprogramma's, zodat ontwikkelaars snel ontwikkelen en schalen van oplossingen waarmee locatie-informatie in de Azure-oplossingen wordt geïntegreerd. 

U kunt zich aanmelden voor een gratis [Azure kaarten-account](https://azure.microsoft.com/services/azure-maps/) en begin te ontwikkelen.

In de volgende video wordt Azure Maps in detail uitgelegd:

<br/>

<iframe src="https://channel9.msdn.com/Shows/Internet-of-Things-Show/Azure-Maps/player?format=ny" width="960" height="540" allowFullScreen frameBorder="0"></iframe>

## <a name="map-controls"></a>Kaartbesturingselementen

### <a name="web-sdk"></a>Web SDK

Azure Maps Web SDK kunt u interactieve kaarten met uw eigen inhoud en beeldmateriaal om weer te geven in uw web- of mobiele toepassingen aanpassen. Dit besturingselement wordt gebruik gemaakt van WebGL, zodat u grote gegevenssets met hoge prestaties kan weergeven. Ontwikkelen met de SDK met behulp van JavaScript- of TypeScript.

![Van de voorbeeldtoewijzing van het population wijzigen](media/about-azure-maps/Introduction_WebMapControl.png)

### <a name="android-sdk"></a>Android SDK

Gebruik de Android SDK voor Azure-kaarten om de toewijzing van mobiele toepassingen te maken. 

![Voorbeelden van de kaart op een mobiel apparaat](media/about-azure-maps/AndroidSDK.png)

## <a name="services-in-azure-maps"></a>Services in Azure Maps

Azure kaarten bestaat uit de volgende negen services die kunnen voorzien van geografische context aan uw Azure-toepassingen.

### <a name="data-service"></a>Gegevensservice

Gegevens zijn een imperatieve voor kaarten. Gebruik Data Service om te uploaden en opslaan van georuimtelijke gegevens voor gebruik met ruimtelijke operations- of image-samenstelling.  Klantgegevens dichter bij te brengen naar de service Azure Maps wordt Verminder de latentie, productiviteit en nieuwe scenario's maken in uw toepassingen. Zie voor meer informatie over deze service, de [gegevens Service API-documentatie](https://docs.microsoft.com/rest/api/maps/data).

### <a name="mobility-service"></a>Mobility-service

De Azure Maps Mobility-service kunt realtime reis plannen. Deze retourneert de best mogelijke routeopties en voorziet in verschillende modi van reizen. Voor gebieden metro (stad), kunnen deze modi lopen, fietsen en openbare doorvoer bevatten. Ontwikkelaars kunnen reisplan details van de doorvoer, zoals line geometrie, lijsten met gestopt, gepland en realtime ontvangsten en servicewaarschuwingen aanvragen.

De service kunnen ook zoekopdrachten voor bepaalde objecttypen, zoals gedeelde fietsen, alsmede scooters of auto's om een locatie. Gebruikers kunnen aanvragen het aantal beschikbare gedeelde fietsen blijven in de dichtstbijzijnde dock. Ze kunnen zoeken naar beschikbare auto-share voertuigen en meer informatie, zoals toekomstige beschikbaarheid en huidige niveau van de brandstof vinden.

Zie voor meer informatie over de service, de [Mobility API-documentatie](https://docs.microsoft.com/rest/api/maps/mobility).

### <a name="render-service"></a>Service voor rendering

De Render service helpt ontwikkelaars web- en mobiele toepassingen met betrekking tot de toewijzing te maken. De service maakt gebruik van een raster met grafische afbeeldingen van hoge kwaliteit, beschikbaar in 19 zoomniveaus, of volledig aanpasbare kaartafbeeldingen in een vectorindeling.

![Voorbeeld van een toewijzing van de service voor de rendering](media/about-azure-maps/Introduction_Map.png)

De Render Service biedt nu preview-API's die ontwikkelaars met satellietbeelden laten werken. Lees voor meer informatie de [Render-API-documentatie](https://docs.microsoft.com/rest/api/maps/render).

### <a name="route-service"></a>Routeservice

De Route Service bevat robuuste geometrische berekeningen voor de infrastructuur in de echte wereld en routebeschrijvingen voor meerdere manieren van reizen. Met de service kunnen ontwikkelaars richtingen berekenen voor een aantal manieren van reizen, zoals met de auto, vrachtwagen, fiets of wandelen. In de service kan ook rekening worden gehouden met invoerwaarden voor verkeersomstandigheden, gewichtsbeperkingen of transport van gevaarlijke stoffen.

![Voorbeeld van een toewijzing van de routeservice](media/about-azure-maps/Introduction_Route.png)

De routeservice biedt een voorbeeld van geavanceerde functies, zoals: 

* Batchverwerking van meerdere routeren van aanvragen.
* Matrices van reizen tijd en de afstand tussen een set oorsprongen en bestemmingen.
* Routes zoeken of afstanden die gebruikers kunnen op basis van tijd of brandstof vereisten. 

Lees voor meer informatie over de mogelijkheden voor routering, de [Route API-documentatie](https://docs.microsoft.com/rest/api/maps/route).

### <a name="search-service"></a>Zoekservice

De Search-service helpt ontwikkelaars zoeken naar adressen, plaatsen, bedrijfsvermeldingen op naam of categorie, en andere geografische informatie. De Search-service kan [geocode omgekeerde](https://en.wikipedia.org/wiki/Reverse_geocoding) adressen en dwarsstraten op basis van Latitude en lengten.

![Voorbeeld van een zoekopdracht op een kaart](media/about-azure-maps/Introduction_Search.png)

De Search-service biedt ook geavanceerde functies zoals:

* Zoeken op een route.
* Zoeken binnen een bredere gebied.
* Batch een groep zoekaanvragen.
* Zoeken naar meer ruimte in plaats van een punt locatie. 

Er zijn momenteel preview-versies beschikbaar van API’s voor zoeken in batches en zoeken naar gebieden. Lees voor meer informatie over de zoekmogelijkheden voor in de [zoeken-API-documentatie](https://docs.microsoft.com/rest/api/maps/search).

### <a name="spatial-operations-service"></a>Ruimtelijke-service van Operations

De service van Azure Maps ruimtelijke Operations haalt locatiegegevens op en analyseert deze op elk gewenst moment om te informeren over de klanten van actieve gebeurtenissen in de tijd en ruimte. Hiermee kunnen bijna realtime analyse en voorspellende modellen van gebeurtenissen. 

De service kan klanten voor het verbeteren van de intelligence van hun locatie met een bibliotheek met algemene georuimtelijke wiskundige berekeningen, met inbegrip van de dichtstbijzijnde punt, cirkel great afstand en buffers. Lees voor meer informatie over de service en de verschillende onderdelen van de [ruimtelijke Operations API-documentatie](https://docs.microsoft.com/rest/api/maps/spatial).

### <a name="time-zone-service"></a>Time Zone Service

De Time Zone service kunt u informatie over de huidige, afgelopen en toekomstige tijdzone van de query met behulp van een breedtegraad/lengtegraad-combinaties of een [IANA-ID](https://www.iana.org/). De Time Zone service biedt ook mogelijkheden voor:

* Id's van Microsoft Windows-tijdzone geconverteerd naar IANA-tijdzones.
* Ophalen van een tijdzoneverschil naar UTC.
* Ophalen van de huidige tijd in een tijdzone. 

Een typisch JSON-antwoord voor een query naar de Time Zone service lijkt op het volgende voorbeeld:

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

Lees voor meer informatie over deze service, de [tijdzone API-documentatie](https://docs.microsoft.com/rest/api/maps/timezone).

### <a name="traffic-service"></a>Verkeersservice

De Traffic service is een suite met webservices die ontwikkelaars gebruiken kunnen voor het maken van webtoepassingen en mobiele toepassingen waarvoor gegevens over verkeer. De service omvat twee gegevenstypen:

* Netwerkverkeer: Realtime waargenomen snelheden en reistijden voor alle belangrijke wegen in het netwerk.
* Verkeer incidenten: Een actuele weergave van Files en incidenten op het netwerk weg.

![Voorbeeld van een kaart met informatie over gegevensverkeer](media/about-azure-maps/Introduction_Traffic.png)

Zie voor meer informatie de [verkeer API-documentatie](https://docs.microsoft.com/rest/api/maps/traffic).

### <a name="ip-to-location-service"></a>Locatieservice IP

Het IP-adres naar locatie-service gebruiken om een voorbeeld van de opgehaalde twee letters landcode voor een IP-adres. Deze service kunt u aanpassen en de gebruikerservaring te verbeteren door te geven van de aangepaste toepassing inhoud op basis van geografische locatie.

Lees voor meer informatie over de REST-API's voor het IP-adres voor de Locatieservice, de [Geolocatie-API van Azure Maps-documentatie](https://docs.microsoft.com/rest/api/maps/geolocation).

## <a name="programming-model"></a>Programmeermodel

Azure kaarten is gebouwd voor mobiliteit en kan helpen bij het ontwikkelen van toepassingen voor meerdere platforms. Het maakt gebruik van een model voor die taalneutraal en ondersteuning biedt voor JSON-uitvoer via [REST-API's](https://docs.microsoft.com/rest/api/maps/).

Bovendien biedt Azure Maps een handig [kaartbesturingselement in JavaScript](https://docs.microsoft.com/javascript/api/azure-maps-control) met een eenvoudig programmeermodel voor het snel en gemakkelijk ontwikkelen van webtoepassingen en mobiele toepassingen.

## <a name="usage"></a>Gebruik

Toegang tot de Azure-kaarten-services is een kwestie van gaat de [Azure-portal](https://portal.azure.com) en het maken van een Azure kaarten-account.

Azure Maps maakt gebruik van een verificatieschema op basis van sleutels. Uw account wordt geleverd met twee sleutels die al voor u gegenereerd. U kunt deze locatiemogelijkheden direct integreren in uw toepassing met behulp van een van de sleutels en een aanvraag aan de Azure Maps-service te maken.

## <a name="supported-regions"></a>Ondersteunde regio’s

De Azure kaarten-API's zijn momenteel beschikbaar in alle landen/regio's behalve deze:

* Argentinië
* China
* India
* Marokko
* Pakistan
* Zuid-Korea

Controleer of dat de locatie van uw huidige IP-adres niet in een van de niet-ondersteunde landen/regio's.

## <a name="next-steps"></a>Volgende stappen

Probeer een voorbeeld-app die worden gepresenteerd Azure-kaarten:

> [!div class="nextstepaction"]
> [Snelstart: Een web-app maken](quick-demo-map-app.md)

Blijf up-to-date via de Azure-kaarten: 

> [!div class="nextstepaction"]
> [Azure Maps-blog](https://azure.microsoft.com/blog/topics/azure-maps/)
