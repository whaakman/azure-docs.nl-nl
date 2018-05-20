---
title: Overzicht van Azure Maps | Microsoft Docs
description: Een inleiding tot Azure Maps
services: azure-maps
keywords: ''
author: kgremban
ms.author: kgremban
ms.date: 05/07/2018
ms.topic: overview
ms.service: azure-maps
documentationcenter: ''
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: e84580c4023be87ebfc1988c631af0b76e213987
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
# <a name="an-introduction-to-azure-maps"></a>Een inleiding tot Azure Maps
Azure Maps is een portfolio met georuimtelijke services die onder andere service-API's omvat voor kaarten, zoeken, routes, verkeer en tijdzones. Met behulp van de portfolio met Azure OneAPI-compatibele services kunt u gebruikmaken van vertrouwde hulpprogramma's om snel oplossingen te ontwikkelen en schalen waarmee locatiegegevens worden geïntegreerd in Azure-oplossingen. Azure Maps biedt ontwikkelaars uit alle branches krachtige georuimtelijke mogelijkheden, vol nieuwe kaartgegevens die onmisbaar zijn om mobiele applicaties en webapplicaties te voorzien van geografische context. Azure Maps bestaat uit een set REST API's en een webgebaseerd JavaScript-besturingselement die ontwikkeltaken voor meerdere mediums gemakkelijk, flexibel en mobiel maken. 

De volgende video introduceert Azure Maps:

<iframe src="https://channel9.msdn.com/Shows/Azure-Friday/Azure-Location-Based-Services/player" width="960" height="540" allowFullScreen frameBorder="0"></iframe>

Azure Maps bestaat uit vijf primaire services ter ondersteuning van Azure-toepassingen waarvoor geografische context is vereist. Elk van de services wordt gedetailleerd beschreven.

De **Render service** is ontworpen voor ontwikkelaars voor het maken van webtoepassingen en mobiele toepassingen met betrekking tot kaarten. De service maakt gebruik van een raster met grafische afbeeldingen van hoge kwaliteit, beschikbaar in 19 zoomniveaus, of volledig aanpasbare kaartafbeeldingen in een vectorindeling.

![Azure Maps Map.png](media/about-azure-maps/Introduction_Map.png)

De **Route service** bevat een robuuste infrastructuur voor echte geometrische berekeningen en routebeschrijvingen voor meerdere manieren van reizen. Met de service kunnen ontwikkelaars richtingen berekenen voor een aantal manieren van reizen, zoals met de auto, vrachtwagen, fiets of wandelen. In de service kan ook rekening worden gehouden met invoerwaarden voor verkeersomstandigheden, gewichtsbeperkingen of transport van gevaarlijke stoffen.

![Azure Maps Route.png](media/about-azure-maps/Introduction_Route.png)

De **Search service** is ontworpen voor ontwikkelaars om te zoeken naar adressen, plaatsen, bedrijfsvermeldingen op naam of categorie, en andere geografische informatie. Met de Search Service kan ook [de geocode worden omgekeerd](https://en.wikipedia.org/wiki/Reverse_geocoding) voor adressen en dwarsstraten op basis van een breedtegraad/lengtegraad. 

![Azure Maps Search.png](media/about-azure-maps/Introduction_Search.png)

Met de **Time Zone service** kunt u huidige, afgelopen en toekomstige tijdzonegegevens opvragen met behulp van breedtegraad/lengtegraad-combinaties of een [IANA-id](http://www.iana.org/). Met de Time Zone service kunnen ook id’s voor Microsoft Windows-tijdzones worden geconverteerd naar IANA-tijdzones, door een tijdzoneverschil naar UTC op te halen en de huidige tijd op te halen in een respectieve tijdzone. Een typisch JSON-antwoord voor een query in de Time Zone service ziet er als volgt uit:

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

De **Traffic service** is een suite met webservices die is ontworpen voor ontwikkelaars voor het maken van webtoepassingen en mobiele toepassingen die te maken hebben met verkeer. De service omvat twee gegevenstypen:
* Verkeersstroom - waargenomen snelheden en reistijden in realtime voor alle belangrijke wegen in het netwerk. 
* Verkeersincidenten - een nauwkeurige weergave van files en incidenten op het wegennet.

![Azure Maps Traffic](media/about-azure-maps/Introduction_Traffic.png)

Azure Maps is gebouwd voor mobiliteit en kan toepassingen hosten op verschillende platforms omdat het programmeermodel agnostisch is en ondersteuning biedt voor JSON-uitvoer via REST API’s. Daarnaast biedt Azure Maps een handig kaartbesturingselement in JavaScript met een ongecompliceerd programmeermodel voor een snelle en eenvoudige ontwikkeling van webtoepassingen en mobiele toepassingen. 

Azure Maps maakt gebruik van een verificatieschema op basis van sleutels. Voor toegang tot de services gaat u naar [Azure Portal](http://portal.azure.com) en maakt u een Azure Maps-account. Uw account wordt geleverd met twee sleutels die vooraf voor u zijn gegenereerd. U kunt deze locatiemogelijkheden direct integreren in uw toepassingen met behulp van een van de sleutels in de aanvragen voor de Azure Maps-service.

## <a name="unsupported-regions"></a>Niet-ondersteunde regio's
De Azure Maps-API is in sommige landen niet beschikbaar. Controleer uw huidige IP-adres en verifieer of de locatie van uw IP-adres niet in een land is dat niet wordt ondersteund:

* Argentinië
* China
* India
* Marokko
* Pakistan
* Zuid-Korea

## <a name="relationship-with-bing-maps"></a>Relatie met Bing Kaarten
De kaarten die worden beschreven in dit document, verschillen van de kaarten van Bing Kaarten. Hoewel veel van de functies overeenkomen, zijn beide services verschillend en niet aan elkaar gerelateerd. Deze Azure-service heeft geen gevolgen voor het productaanbod of wegenkaarten van Bing Kaarten.

Het doel van Microsoft is om de ontwikkelaarscommunity keuze te bieden op het gebied van locatieserviceaanbiedingen. De volgende tabel bevat richtlijnen voor ontwikkelaars voor het kiezen van de juiste service: 

| Scenario | Gebruik Azure Maps wanneer... | Gebruik Bing Kaarten wanneer... |
| ------------- | ------------- | ------------- |
| Ontwikkelomgeving | Bouwen in of coördinatie met andere Azure-services | Een cloud van derden of andere ontwikkelomgeving gebruiken |
| Ontwikkelingsfase  | Omdat Azure Maps momenteel een openbare preview-versie is, is deze geoptimaliseerd voor vroege testdoeleinden en Testen-van-concept-ontwikkeling | Voor een productieomgeving is een SLA op zakelijk niveau vereist |
| Prijsopties | Voorlopige prijsopties voor ontwikkelaars zijn voldoende | Voor zakelijk gebruik zijn aangepaste prijzen vereist |
| Caseomgeving gebruiken | In-vehicle-gebruik is vereist | In-vehicle-gebruik is niet vereist |
| Geografische dekking | India, China, Japan en Zuid-Korea zijn niet vereist | Kaarten voor India, China, Japan en Zuid-Korea zijn vereist |
| Inhoud toewijzen | Standaardkaarten van Surface zijn toereikend | Satelliet- en straatbeelden zijn vereist |
| Onderliggende kaartbron | TomTom-kaartgegevens hebben de voorkeur | HERE-kaartgegevens hebben de voorkeur |

Meld u vandaag nog aan voor een [Azure Maps-account](http://aka.ms/azurelbsportal).

## <a name="next-steps"></a>Volgende stappen

U hebt nu een algemeen beeld van Azure Maps. De volgende stap is een voorbeeld-app met de service uitproberen.

> [!div class="nextstepaction"]
> [Demo van interactief zoeken naar kaarten starten](quick-demo-map-app.md)

