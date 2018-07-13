---
title: Wat is Bing Entiteiten zoeken? | Microsoft Docs
description: Informatie over het gebruik van de Bing Entity Search API om te zoeken naar het web voor entiteiten en plaatsen.
services: cognitive-services
author: swhite-msft
manager: ehansen
ms.assetid: 0B54E747-61BF-42AA-8788-E25D63F625FC
ms.service: cognitive-services
ms.component: bing-entity-search
ms.topic: article
ms.date: 07/06/2016
ms.author: scottwhi
ms.openlocfilehash: 275430bc6ee8f935978243e61f68713974648189
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2018
ms.locfileid: "39008107"
---
# <a name="what-is-bing-entity-search"></a>Wat is Bing Entiteiten zoeken?

Bing Entity Search API een zoekopdracht naar Bing verzonden en opgehaald van entiteiten en plaatsen omvatten de resultaten. Plaats resultaten opnemen restaurants, hotels of andere lokale bedrijven. Bing retourneert plaatsen als de query de naam van de lokale bedrijven geeft of voor een type of-business (bijvoorbeeld restaurants in de buurt) vraagt. Bing retourneert entiteiten als de query bekende mensen, plaatsen (toeristische bezienswaardigheden, Staten, landen, enzovoort) of dingen geeft.

## <a name="suggesting--using-search-terms"></a>Zoektermen voorstellen en gebruiken

Als u een zoekvak aanbiedt waarin de gebruiker een zoekterm invoert, kunt u de [Automatische suggestie-API voor Bing](../bing-autosuggest/get-suggested-search-terms.md) gebruiken om de ervaring te verbeteren. De API retourneert voorgestelde queryreeksen op basis van gedeeltelijke zoektermen terwijl de gebruiker typt.

Nadat de gebruiker de zoekterm heeft ingevoerd, past u URL-codering toe op de term voordat u de queryparameter [q](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#query) instelt. Bijvoorbeeld, als de gebruiker voert *Marcus Appel*, stel `q` naar *Marcus + Appel* of *Marcus % 20Appel*.

Als de zoekterm een fout spelling bevat, antwoord van de zoekactie bevat een [QueryContext](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#querycontext) object. Het object bevat de oorspronkelijke spelling en de gecorrigeerde spelling die Bing voor de zoekopdracht gebruikt.

```json
"queryContext": {
    "originalQuery": "hollo wrld",
    "alteredQuery": "hello world",
    "alterationOverrideQuery": "+hollo wrld",
    "adultIntent": false
}
```

## <a name="requesting-entities"></a>Aanvragen van entiteiten

Zie voor een van de voorbeeldaanvraag, [maken van uw eerste aanvraag](./quick-start.md).

## <a name="the-response"></a>Het antwoord

Het antwoord bevat een [SearchResponse](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#searchresponse) object. Als u Bing vindt een entiteit of een plaats die relevant is, het object bevat de `entities` veld `places` , of beide. De antwoordobject bevat anders geen een veld.
> [!NOTE]
> Entiteit antwoorden ondersteunen meerdere markten, maar het antwoord plaatsen ondersteunt alleen Amerikaanse zakelijke locaties. 

De `entities` veld is een [EntityAnswer](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#entityanswer) object met een lijst met [entiteit](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#entity) objecten (Zie de `value` veld). De lijst kan bevatten één dominante entiteit of entiteiten van meerdere oplossen van ambiguïteit. 

Een dominante entiteit is een entiteit die Bing is van mening dat is de enige entiteit die voldoet aan de aanvraag (Er is geen dubbelzinnigheid in welke entiteit voldoet aan de aanvraag). Als meerdere entiteiten kunnen voldoen aan de aanvraag, bevat de lijst met meer dan één entiteit van het oplossen van ambiguïteit. Bijvoorbeeld, als de aanvraag wordt de algemene naam van een film franchise gebruikt, bevat de lijst met waarschijnlijk ondubbelzinnigheid entiteiten. Maar als de aanvraag een bepaalde titel van de winkel bevat, de lijst met waarschijnlijk één dominante entiteit bevat.

Entiteiten bevatten bekende persoonlijkheden zoals zang, actors, atleten, modellen, enzovoort.; locaties en monumenten zoals koppelpunt Groningen of Lincoln Memorial; en, zoals een titel bananen, goldendoodle, rapport of film. De [entityPresentationInfo](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#entitypresentationinfo) veld bevat hints die van de entiteit type identificeren. Bijvoorbeeld, als het is een persoon, films, dieren of voordelen. Zie voor een lijst van mogelijke typen, [Entiteitstypen](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#entity-types)

```json
"entityPresentationInfo": {
    "entityScenario": "DominantEntity",
    "entityTypeHints": ["Attraction"],
    "entityTypeDisplayHint": "Mountain"
}, ...
```

Hieronder ziet u een antwoord met een entiteit dominante en oplossen van ambiguïteit.

```json
{
    "_type": "SearchResponse",
    "queryContext": {
        "originalQuery": "Mount Rainier"
    },
    "entities": {
        "value": [{
            "contractualRules": [{
                "_type": "ContractualRules/LicenseAttribution",
                "targetPropertyName": "description",
                "mustBeCloseToContent": true,
                "license": {
                    "name": "CC-BY-SA",
                    "url": "http://creativecommons.org/licenses/by-sa/3.0/"
                },
                "licenseNotice": "Text under CC-BY-SA license"
            },
            {
                "_type": "ContractualRules/LinkAttribution",
                "targetPropertyName": "description",
                "mustBeCloseToContent": true,
                "text": "contoso.com",
                "url": "http://contoso.com/mount_rainier"
            },
            {
                "_type": "ContractualRules/MediaAttribution",
                "targetPropertyName": "image",
                "mustBeCloseToContent": true,
                "url": "http://contoso.com/mount-rainier"
            }],
            "webSearchUrl": "https://www.bing.com/search?q=Mount%20Rainier...",
            "name": "Mount Rainier",
            "url": "http://www.northwindtraders.com/",
            "image": {
                "name": "Mount Rainier",
                "thumbnailUrl": "https://www.bing.com/th?id=A4ae343983daa4...",
                "provider": [{
                    "_type": "Organization",
                    "url": "http://contoso.com/mount_rainier"
                }],
                "hostPageUrl": "http://contoso.com/commons/7/72/mount_rain...",
                "width": 110,
                "height": 110
            },
            "description": "Mount Rainier is 14,411 ft tall and the highest mountain...",
            "entityPresentationInfo": {
                "entityScenario": "DominantEntity",
                "entityTypeHints": ["Attraction"]
            },
            "bingId": "38b9431e-cf91-93be-0584-c42a3ecbfdc7"
        },
        {
            "contractualRules": [{
                "_type": "ContractualRules/MediaAttribution",
                "targetPropertyName": "image",
                "mustBeCloseToContent": true,
                "url": "http://contoso.com/mount_rainier_national_park"
            }],
            "webSearchUrl": "https://www.bing.com/search?q=Mount%20Rainier%20National...",
            "name": "Mount Rainier National Park",
            "url": "http://worldwideimporters.com/",
            "image": {
                "name": "Mount Rainier National Park",
                "thumbnailUrl": "https://www.bing.com/th?id=A91bdc5a1b648a695a39...",
                "provider": [{
                    "_type": "Organization",
                    "url": "http://contoso.com/mount_rainier_national_park"
                }],
                "hostPageUrl": "http://contoso.com/en/7/7a...",
                "width": 50,
                "height": 50
            },
            "description": "Mount Rainier National Park is a United States National Park...",
            "entityPresentationInfo": {
                "entityScenario": "DisambiguationItem",
                "entityTypeHints": ["Organization"]
            },
            "bingId": "29d4b681-227a-3924-7bb1-8a54e8666b8c"
        }]
    }
}
```

De entiteit bevat een `name`, `description`, en `image` veld. Wanneer u deze velden in uw gebruikerservaring weergeeft, moet u ze het kenmerk. De `contractualRules` veld bevat een lijst met afschrijvingen die u moet toepassen. De contractuele regel geeft het veld dat de toekenning is van toepassing op. Zie voor meer informatie over het toepassen van attribution [Attribution](#data-attribution).

```json
"contractualRules": [{
    "_type": "ContractualRules/LicenseAttribution",
    "targetPropertyName": "description",
    "mustBeCloseToContent": true,
    "license": {
        "name": "CC-BY-SA",
        "url": "http://creativecommons.org/licenses/by-sa/3.0/"
    },
    "licenseNotice": "Text under CC-BY-SA license"
},
{
    "_type": "ContractualRules/LinkAttribution",
    "targetPropertyName": "description",
    "mustBeCloseToContent": true,
    "text": "contoso.com",
    "url": "http://contoso.com/wiki/Mount_Rainier"
},
{
    "_type": "ContractualRules/MediaAttribution",
    "targetPropertyName": "image",
    "mustBeCloseToContent": true,
    "url": "http://contoso.com/wiki/Mount_Rainier"
}], ...
```

Wanneer u de entiteitgegevens (naam, beschrijving en afbeelding), moet u ook de URL in gebruiken de `webSearchUrl` om een koppeling naar de Bing zoeken resultatenpagina die de entiteit bevat.


De `places` veld is een [LocalEntityAnswer](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#localentityanswer) object met een lijst met [plaats](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#place) objecten (Zie de `value` veld). De lijst bevat een of meer lokale entiteiten die voldoen aan de aanvraag.

Locaties zijn restaurant, hotels of lokale bedrijven. De [entityPresentationInfo](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#entitypresentationinfo) veld bevat hints die de lokale entiteitstype identificeren. De lijst bevat een lijst met hints, zoals locatie, LocalBusiness, Restaurant. Elke opeenvolgende hint in de matrix beperkt type van de entiteit. Zie voor een lijst van mogelijke typen, [Entiteitstypen](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#entity-types)

```json
"entityPresentationInfo": {
    "entityScenario": "ListItem",
    "entityTypeHints": ["Place",
    "LocalBusiness",
    "Restaurant"]
}, ...
```
> [!NOTE]
> Entiteit antwoorden ondersteunen meerdere markten, maar het antwoord plaatsen ondersteunt alleen Amerikaanse zakelijke locaties. 

Lokaal op de hoogte entiteit, zoals query's *restaurant dichtbij* vereisen dat de locatie van de gebruiker om nauwkeurig resultaten te bieden. Uw aanvragen moeten altijd de X-Search-locatie en de X-MSEdge-client-IP-headers gebruiken om op te geven van de locatie van de gebruiker. Als u Bing denkt dat de query veel voordeel hebben van de locatie van de gebruiker, wordt de `askUserForLocation` veld van [QueryContext](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#querycontext) naar **waar**. 

```json
{
    "_type": "SearchResponse",
    "queryContext": {
        "originalQuery": "Sinful Bakery and Cafe",
        "askUserForLocation": true
    },
    ...
}
```

Het resultaat van een plaats bevat de plaats waar u de naam, adres, telefoonnummer en URL naar de website van de entiteit. Wanneer u de informatie van entiteit wordt weergegeven, moet u ook de URL in gebruiken de `webSearchUrl` om een koppeling naar de Bing zoeken resultatenpagina die de entiteit bevat.

```json
"places": {
    "value": [{
        "_type": "Restaurant",
        "webSearchUrl": "https://www.bing.com/search?q=Sinful%20Bakery...",
        "name": "Liberty's Delightful Sinful Bakery & Cafe",
        "url": "http://libertysdelightfulsinfulbakeryandcafe.com/",
        "entityPresentationInfo": {
            "entityScenario": "ListItem",
            "entityTypeHints": ["Place",
            "LocalBusiness",
            "Restaurant"]
        },
        "address": {
            "addressLocality": "Seattle",
            "addressRegion": "WA",
            "postalCode": "98112",
            "addressCountry": "US",
            "neighborhood": "Madison Park"
        },
        "telephone": "(800) 555-1212"
    }]
}
```

> [!NOTE]
> U of een derde partij namens u mogelijk niet gebruiken, behouden, opslaan, in de cache, delen, of distribueren van alle gegevens van de API-entiteiten voor het testen, ontwikkelen, training, distribueren of het beschikbaar maken van een niet-Microsoft-service of functie.  

## <a name="data-attribution"></a>Gegevens attribution

API voor Bing entiteit antwoorden bevatten informatie die eigendom zijn van derde partijen. U bent verantwoordelijk om ervoor te zorgen dat uw gebruik is dat mogelijk is, bijvoorbeeld dat voldoet aan alle uw gebruikerservaring mogelijk afhankelijk van creative commons-licentie.

Als een antwoord of het resultaat bevat de `contractualRules`, `attributions`, of `provider` velden, moet u de gegevens van het kenmerk. Als het antwoord niet onder een van deze velden, is er geen attribution vereist. Als het antwoord bevat de `contractualRules` veld en de `attributions` en/of `provider` velden, moet u de contractuele regels gebruiken om de gegevens van het kenmerk.

Het volgende voorbeeld ziet u een entiteit met een MediaAttribution contractuele regel en een afbeelding met een `provider` veld. De regel MediaAttribution identificeert de afbeelding als het doel van de regel, zodat u van de installatiekopie zou negeren `provider` veld en gebruiken in plaats daarvan de regel MediaAttribution attribution opgeven.  

```json
"value": [{
    "contractualRules": [
        ...
        {
            "_type": "ContractualRules/MediaAttribution",
            "targetPropertyName": "image",
            "mustBeCloseToContent": true,
            "url": "http://contoso.com/mount_rainier"
        }
    ],
    ...
    "image": {
        "name": "Mount Rainier",
        "thumbnailUrl": "https://www.bing.com/th?id=A46378861201...",
        "provider": [{
            "_type": "Organization",
            "url": "http://contoso.com/mount_rainier"
        }],
        "hostPageUrl": "http://www.graphicdesigninstitute.com/Uploaded...",
        "width": 110,
        "height": 110
    },
    ...
}]
```

Als een contractuele regel bevat de `targetPropertyName` veld, de regel geldt alleen voor het betreffende veld. Anders wordt de regel van toepassing op het bovenliggende object met de `contractualRules` veld.

In het volgende voorbeeld wordt de `LinkAttribution` regel bevat de `targetPropertyName` veld, zodat de regel geldt voor de `description` veld. Voor regels die betrekking hebben op specifieke velden, moet u een regel direct na de betreffende gegevens met een hyperlink naar de website van de provider bevatten. Als u wilt de beschrijving van het kenmerk, bijvoorbeeld een regel onmiddellijk na de beschrijvende tekst die een hyperlink naar de gegevens op de website van de provider, bevat een koppeling naar contoso.com in dit geval maken.

```json
"entities": {
    "value": [{
            ...
            "description": "Marcus Appel is a former American....",
            ...
            "contractualRules": [{
                    "_type": "ContractualRules/LinkAttribution",
                    "targetPropertyName": "description",
                    "mustBeCloseToContent": true,
                    "text": "contoso.com",
                    "url": "http://contoso.com/cr?IG=B8AD73..."
                 },
            ...
  
```

### <a name="license-attribution"></a>Licentie attribution

Als de lijst met regels voor contractuele bevat een [LicenseAttribution](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#licenseattribution) regel, moet u de aankondiging op de regel direct na de inhoud die de licentie is van toepassing op weergeven. De `LicenseAttribution` regel maakt gebruik van de `targetPropertyName` veld voor het identificeren van de eigenschap die de licentie is van toepassing op.

Hieronder ziet u een voorbeeld waarin een `LicenseAttribution` regel.

![Licentie attribution](./media/cognitive-services-bing-entities-api/licenseattribution.png)

De licentie-ziet u dat u moet een hyperlink naar de website die informatie over de licentie bevat bevatten. Normaal gesproken maken u de naam van de gebruiksrechtovereenkomst voor een hyperlink. Bijvoorbeeld, als de kennisgeving is **tekst onder licentie CC door SA** en CC door Software Assurance is de naam van de licentie, brengt u CC door SA een hyperlink.

### <a name="link-and-text-attribution"></a>Koppeling en tekst attribution

De [LinkAttribution](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#linkattribution) en [TextAttribution](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#textattribution) regels worden meestal gebruikt voor identificatie van de provider van de gegevens. De `targetPropertyName` veld geeft het veld dat de regel van toepassing op.

Als u wilt de providers van het kenmerk, bevatten een regel direct na de inhoud die de afschrijvingen van toepassing op (bijvoorbeeld het betreffende veld). De regel moet duidelijk om aan te geven dat de leveranciers de bron van de gegevens zijn worden gelabeld. Bijvoorbeeld, "gegevens uit: contoso.com '. Voor `LinkAttribution` regels, moet u een hyperlink naar de website van de provider maken.

Hieronder ziet u een voorbeeld waarin `LinkAttribution` en `TextAttribution` regels.

![Koppeling tekst attribution](./media/cognitive-services-bing-entities-api/linktextattribution.png)

### <a name="media-attribution"></a>Media attribution

Als u deze weergeven nadat de entiteit bevat een afbeelding, kunt u een koppeling doorklikken naar de website van de provider moet opgeven. Als de entiteit bevat een [MediaAttribution](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#mediaattribution) regel, gebruikt u de URL van de regel om de koppeling doorklikken te maken. Gebruik anders de URL die is opgenomen in de afbeelding `provider` veld om de koppeling doorklikken te maken.

Hieronder ziet u een voorbeeld van een installatiekopie met `provider` veld en contractuele regels. Omdat in het voorbeeld de contractuele regel omvat, negeert u van de afbeelding `provider` veld en toe te passen de `MediaAttribution` regel.

![Media attribution](./media/cognitive-services-bing-entities-api/mediaattribution.png)

### <a name="search-or-search-like-experience"></a>Zoeken of zoeken-achtige ervaring

Net als met de Bing webzoekopdrachten-API, kunnen Bing Entity Search API alleen worden gebruikt als gevolg van een directe gebruikersquery of zoekopdracht, of als gevolg van een actie in een app of -ervaring die logisch kan worden geïnterpreteerd als een gebruikersaanvraag zoeken. Ter illustratie wordt volgen hieronder enkele voorbeelden van acceptabele zoeken of uw search-achtige ervaring.

- Gebruiker voert een query rechtstreeks in het zoekvak in een app
- Gebruiker selecteert de specifieke tekst of een afbeelding en aanvragen 'meer informatie' of "Als u meer informatie"
- Gebruiker vraagt een search-bot over een bepaald onderwerp
- Gebruiker dwells op een bepaald object of een entiteit in een scenario voor het type visuele zoekopdrachten

Als u niet zeker weet of uw ervaring kan worden beschouwd als een search-achtige ervaring, is het raadzaam dat u contact op met Microsoft.

## <a name="throttling-requests"></a>Aanvraagbeperkingen

[!INCLUDE [cognitive-services-bing-throttling-requests](../../../includes/cognitive-services-bing-throttling-requests.md)]

## <a name="next-steps"></a>Volgende stappen

Als u wilt snel aan de slag met uw eerste aanvraag, Zie [maken van uw eerste aanvraag](./quick-start.md).

Maak uzelf vertrouwd met de [Bing Entity Search API voor Bing versie 7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference) verwijzing. De verwijzing bevat de headers en de queryparameters die u gebruikt om aan te vragen van de lijst met zoekresultaten. Daarnaast vindt u hier definities van de responsobjecten. 

Zie [Automatische suggestie-API voor Bing](../bing-autosuggest/get-suggested-search-terms.md) om de gebruikerservaring van het zoekvak te verbeteren. Als de gebruiker een zoekterm invoert, kunt u deze API aanroepen om relevante querytermen weer te geven die door anderen zijn gebruikt.

Lees [Gebruiks- en weergavevereisten voor Bing](./use-display-requirements.md) om er zeker van te zijn dat u alle regels voor het gebruik van de zoekresultaten volgt.