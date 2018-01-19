---
title: Overzicht van Azure Location Based Services | Microsoft Docs
description: Een inleiding tot Azure Location Based Services (preview)
services: location-based-services
keywords: 
author: dsk-2015
ms.author: dkshir
ms.date: 11/28/2017
ms.topic: overview
ms.service: location-based-services
documentationcenter: 
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 6871f174eb9bae57d9b4767520d0fb2d8d9631d3
ms.sourcegitcommit: 562a537ed9b96c9116c504738414e5d8c0fd53b1
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/12/2018
---
# <a name="an-introduction-to-azure-location-based-services-preview"></a>Een inleiding tot Azure Location Based Services (preview)
Azure Location Based Services is een portfolio met georuimtelijke services die onder andere service-API's omvat voor kaarten, zoeken, routes, verkeer en tijdzones. Met behulp van de portfolio met Azure OneAPI-compatibele services kunt u gebruikmaken van vertrouwde hulpprogramma’s om snel oplossingen te ontwikkelen en schalen waarmee locatiegegevens worden geïntegreerd in Azure-oplossingen. Azure Location Based Services biedt ontwikkelaars uit alle branches krachtige georuimtelijke mogelijkheden, vol nieuwe kaartgegevens die onmisbaar zijn wanneer u webtoepassingen en mobiele toepassingen wilt voorzien van geografische context. Azure Location Based Services bestaat uit een Azure One API-compatibele set REST API's en een JavaScript-besturingselement op basis van het web, waarmee ontwikkelen voor meerdere mediums heel gemakkelijk, flexibel en mobiel wordt. 

Azure Location Based Services bestaat uit vijf primaire services ter ondersteuning van Azure-toepassingen waarvoor geografische context is vereist. Elk van de services wordt hieronder gedetailleerd beschreven.

**Render Service**: de Render Service is ontworpen voor ontwikkelaars voor het maken van webtoepassingen en mobiele toepassingen met betrekking tot kaarten. De service maakt gebruik van een raster met grafische afbeeldingen van hoge kwaliteit, beschikbaar in 19 zoomniveaus, of volledig aanpasbare kaartafbeeldingen in een vectorindeling.

![Azure Location Based Services Map.png](media/about-location-based-services/Introduction_Map.png)

**Route Service**: de Route Service is gebouwd met een robuuste infrastructuur voor echte geometrische berekeningen en routebeschrijvingen voor meerdere routetypen. Met deze service kunnen ontwikkelaars routebeschrijvingen berekenen voor een aantal routetypen, zoals auto, vrachtwagen, fiets of te voet. Ook biedt de service informatie over zaken zoals verkeersomstandigheden, gewichtsbeperkingen of vervoer van gevaarlijke stoffen.

![Azure Location Based Services Route.png](media/about-location-based-services/Introduction_Route.png)

**Search Service**: de Search Service is ontworpen voor ontwikkelaars om te zoeken naar adressen, plaatsen, bedrijfsvermeldingen op naam of categorie, en andere geografische informatie. Met de Search Service kan ook [de geocode worden omgekeerd](https://en.wikipedia.org/wiki/Reverse_geocoding) voor adressen en dwarsstraten op basis van een breedtegraad/lengtegraad. 

![Azure Location Based Services Search.png](media/about-location-based-services/Introduction_Search.png)

**Time Zone Service**: met de Time Zone Service kunt u huidige, afgelopen en toekomstige tijdzonegegevens opvragen met behulp van breedtegraad/lengtegraad-combinaties of een [IANA-id](http://www.iana.org/). Met de Time Zone service kunnen ook id’s voor Microsoft Windows-tijdzones worden geconverteerd naar IANA-tijdzones, door een tijdzoneverschil naar UTC op te halen en de huidige tijd op te halen in een respectieve tijdzone. Een typisch JSON-antwoord voor een query in de Time Zone Service ziet er als volgt uit:

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

**Traffic Service**: de Traffic Service is een suite met webservices die is ontworpen voor ontwikkelaars voor het maken van webtoepassingen en mobiele toepassingen die te maken hebben met verkeer. De aanbieding is onderverdeeld in het volgende:
1. Verkeersstroom - biedt waargenomen snelheden en reistijden in realtime voor alle belangrijke wegen in het netwerk; en, 
2. Verkeersincidenten - biedt een nauwkeurige weergave van files en incidenten op het wegennet.

![Azure Location Based Services - verkeer](media/about-location-based-services/Introduction_Traffic.png)

Azure Location Based Services is gebouwd voor mobiliteit en kan toepassingen hosten op verschillende platforms omdat het programmeermodel agnostisch is en ondersteuning biedt voor JSON-uitvoer via REST API’s. Daarnaast biedt Azure LBS een handig kaartbesturingselement in JavaScript met een ongecompliceerd programmeermodel voor een snelle en eenvoudige ontwikkeling van webtoepassingen en mobiele toepassingen. 

Azure Location Based Services maakt gebruik van een verificatieschema op basis van sleutels. Voor toegang tot de services gaat u naar [Azure Portal](http://portal.azure.com) en maakt u een Azure Location Based Services-account. Uw account wordt geleverd met twee sleutels die vooraf voor u zijn gegenereerd. U kunt deze locatiemogelijkheden direct integreren in uw toepassingen met behulp van een van de sleutels in de aanvragen voor de Azure Location Based Services-service.

**Relatie met Bing Kaarten** - houd er rekening mee dat Azure Location Based Services, zoals beschreven in dit document, verschilt van de services van Bing Kaarten.  Hoewel veel van de functies overeenkomen, zijn beide services verschillend en niet aan elkaar gerelateerd.  Dit heeft geen invloed op productaanbiedingen of implementatieschema’s van Bing Kaarten met de beschikbaarheid van deze nieuwe service binnen Azure. Dit wordt allemaal afzonderlijk beheerd.

Het doel van Microsoft is om de ontwikkelaarscommunity keuze te bieden op het gebied van locatieserviceaanbiedingen.  Hieronder vindt u snelle hulp voor ontwikkelaars over welke service het beste kan worden gebruikt bij verschillende gebruikscases en klantproblemen.  Let op: deze handleiding is momenteel van toepassing op Azure LBS in de openbare previewfase, en wordt bijgewerkt zodra algemene beschikbaarheid wordt bereikt, later in 2018.

| Klantcriteria | Azure Location Based Services gebruiken wanneer... | Bing Kaarten gebruiken wanneer... |
| ------------- | ------------- | ------------- |
| Ontwikkelomgeving | Andere Azure-services inbouwen of gebruiken | Een cloud van derden of andere ontwikkelomgeving gebruiken |
| Ontwikkelingsfase  | Omdat Azure LBS momenteel in de openbare preview is, is deze versie geoptimaliseerd voor vroege testdoeleinden en Testen-van-concept-ontwikkeling | Voor een productieomgeving is een SLA op zakelijk niveau vereist |
| Prijsopties | Voorlopige prijsopties voor ontwikkelaars zijn voldoende | Voor zakelijk gebruik zijn aangepaste prijzen vereist |
| Caseomgeving gebruiken | In-vehicle-gebruik is vereist | In-vehicle-gebruik is niet vereist |
| Geografische dekking | India, China, Japan en Zuid-Korea zijn niet vereist | Kaarten voor India, China, Japan en Zuid-Korea zijn vereist |
| Inhoud toewijzen | Standaardkaarten van Surface zijn toereikend | Satelliet- en straatbeelden zijn vereist |
| Onderliggende kaartbron | TomTom-kaartgegevens hebben de voorkeur | HERE-kaartgegevens hebben de voorkeur |

Registreer u vandaag nog voor een [Azure Location Based Services-account!](http://aka.ms/azurelbsportal)

## <a name="next-steps"></a>Volgende stappen

U hebt nu een overzicht van de Azure Location Based Services (preview). De volgende stap is het uitproberen van een voorbeeld-app met Location Based Services, evenals het maken van een end-to-endscenario in uw webtoepassing.

> [!div class="nextstepaction"]
> [Een demo van een interactieve kaartzoekopdracht starten met behulp van Azure Location Based Services (preview)](quick-demo-map-app.md)
> [Een nabijgelegen nuttige plaats zoeken met behulp van Azure Location Based Services](tutorial-search-location.md)
