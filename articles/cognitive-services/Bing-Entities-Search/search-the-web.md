---
title: Wat is Bing entiteit zoeken? | Microsoft Docs
description: Informatie over het gebruik van de API van Bing entiteit zoeken om te zoeken op het web voor entiteiten en plaatsen.
services: cognitive-services
author: swhite-msft
manager: ehansen
ms.assetid: 0B54E747-61BF-42AA-8788-E25D63F625FC
ms.service: cognitive-services
ms.component: bing-entity-search
ms.topic: article
ms.date: 07/06/2016
ms.author: scottwhi
ms.openlocfilehash: f1b87c07d5b56307fd6b3fc68999598aeab6eb82
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345579"
---
# <a name="what-is-bing-entity-search"></a>Wat is Bing entiteit zoeken?

De Bing-API van zoekservice entiteit stuurt een zoekquery naar Bing en entiteiten en locaties omvatten de resultaten opgehaald. Plaats resultaten bevatten restaurant, hotel of andere lokale bedrijven. Bing retourneert plaatsen als de query de naam van de lokale business geeft of om een type of-business (bijvoorbeeld restaurant dichtbij vraagt). Bing retourneert entiteiten, als de query bekende mensen, plaatsen (sport attracties, statussen, landen, enz.) of dingen geeft.

## <a name="suggesting--using-search-terms"></a>Gebruik zoektermen van & opmaken

Als u een zoekvak waarin de gebruiker de zoekterm invoert, kunt u met de [voor Automatische suggestie van Bing-API](../bing-autosuggest/get-suggested-search-terms.md) om de ervaring te verbeteren. De API retourneert voorgestelde query tekenreeksen op basis van gedeeltelijke zoektermen als de gebruiker typt.

Nadat de gebruiker de zoekterm invoert, URL-codering de term voordat u de [q](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#query) queryparameter. Bijvoorbeeld, als de gebruiker invoert *Marcus Appel*stelt `q` naar *Marcus + Appel* of *Marcus % 20Appel*.

Als de zoekterm een fout spelling bevat, het zoeken antwoord bevat een [QueryContext](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#querycontext) object. Het object bevat de oorspronkelijke spelling en de gecorrigeerde spelling die Bing voor de zoekopdracht gebruikt.

```json
"queryContext": {
    "originalQuery": "hollo wrld",
    "alteredQuery": "hello world",
    "alterationOverrideQuery": "+hollo wrld",
    "adultIntent": false
}
```

## <a name="requesting-entities"></a>Aanvragende entiteiten

Zie voor een voorbeeld van de aanvraag, [maken van uw eerste aanvraag](./quick-start.md).

## <a name="the-response"></a>Het antwoord

Het antwoord bevat een [SearchResponse](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#searchresponse) object. Als Bing vindt een entiteit of een plaats die relevant zijn, het object bevat de `entities` veld `places` veld of beide. Het object response omvat anders niet een van de velden.

De `entities` veld is een [EntityAnswer](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#entityanswer) object met een lijst met [entiteit](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#entity) objecten (Zie de `value` veld). De lijst kan één verwerkingsflexibiliteit entiteit of meerdere disambiguation entiteiten bevatten. 

Een verwerkingsflexibiliteit entiteit is een entiteit die Bing meent is de enige entiteit die voldoet aan de aanvraag (Er is geen dubbelzinnigheid wat betreft welke entiteit voldoet aan de aanvraag). Als meerdere entiteiten kunnen voldoen aan de aanvraag, bevat de lijst met meer dan één disambiguation entiteit. Als de aanvraag wordt de algemene naam van een franchise film gebruikt, bevat de lijst waarschijnlijk disambiguation entiteiten. Maar als de aanvraag is opgegeven voor een bepaalde titel van de winkel, de lijst waarschijnlijk één verwerkingsflexibiliteit entiteit bevat.

Entiteiten bevatten bekende wensen zoals zang, actoren, athletes, modellen, enz.; locaties en monumenten zoals Mount Rainier of Lincoln Memorial; en dingen zoals een titel banana, goldendoodle, boek of film. De [entityPresentationInfo](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#entitypresentationinfo) veld hints identificatie van het type bevat. Bijvoorbeeld als het een persoon, films, dieren of voordelen. Zie voor een lijst van mogelijke typen [Entiteitstypen](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#entity-types)

```json
"entityPresentationInfo": {
    "entityScenario": "DominantEntity",
    "entityTypeHints": ["Attraction"],
    "entityTypeDisplayHint": "Mountain"
}, ...
```

Hieronder ziet u een antwoord dat een entiteit dominante en disambiguation bevat.

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

De entiteit bevat een `name`, `description`, en `image` veld. Wanneer u deze velden in de gebruikerservaring weergeven, moet u ze kenmerk. De `contractualRules` veld bevat een lijst met afschrijvingen die u moet toepassen. De regel contractueel identificeert het veld met het kenmerk is van toepassing op. Zie voor meer informatie over het toepassen van afschrijving [toekenning](#data-attribution).

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

Wanneer u de informatie van de entiteit (naam, beschrijving en installatiekopie) wordt weergegeven, moet u ook de URL in gebruiken de `webSearchUrl` veld om te koppelen aan de Bing-zoekopdracht resultatenpagina die de entiteit bevat.


De `places` veld is een [LocalEntityAnswer](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#localentityanswer) object met een lijst met [plaats](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#place) objecten (Zie de `value` veld). De lijst bevat een of meer lokale entiteiten die voldoen aan de aanvraag.

Locaties bevatten restaurant, hotels of lokale bedrijven. De [entityPresentationInfo](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#entitypresentationinfo) veld bevat hints die de lokale entiteitstype identificeren. De lijst bevat een lijst met hints zoals locatie, LocalBusiness, Restaurant. Elke opeenvolgende hint in de matrix wordt beperkt van het type. Zie voor een lijst van mogelijke typen [Entiteitstypen](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#entity-types)

```json
"entityPresentationInfo": {
    "entityScenario": "ListItem",
    "entityTypeHints": ["Place",
    "LocalBusiness",
    "Restaurant"]
}, ...
```

Lokale bewust entiteit, zoals query's *restaurant dichtbij* locatie voor nauwkeurige resultaten van de gebruiker vereist. Uw verzoeken, gebruik altijd de X-Search-locatie en de X-MSEdge-client-IP-headers van de gebruiker de locatie opgeven. Als Bing denkt de query wilt profiteren van de locatie van de gebruiker dat, wordt de `askUserForLocation` veld [QueryContext](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#querycontext) naar **true**. 

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

Een resultaat plaats bevat naam, adres, telefoonnummer en URL in de plaats aan van de entiteit-website. Wanneer u de entiteit-informatie weergeven, moet u ook de URL in gebruiken de `webSearchUrl` veld om te koppelen aan de Bing-zoekopdracht resultatenpagina die de entiteit bevat.

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
> U of een derde partij namens u mogelijk niet gebruiken, behouden, opslaan, cache, delen, of distribueren van alle gegevens van de API-entiteiten voor de toepassing testen, ontwikkelen, training, distribueren of het beschikbaar maken van een niet-Microsoft-service of functie.  

## <a name="data-attribution"></a>Toekenning van gegevens

Bing entiteit API antwoorden bevatten informatie die eigendom zijn van derden. U bent zelf verantwoordelijk om te controleren of uw gebruik nodig, bijvoorbeeld door het voldoen aan eventuele creative commons-licentie die uw gebruikerservaring kan vertrouwen op.

Als een antwoord of het resultaat bevat de `contractualRules`, `attributions`, of `provider` velden, moet u de gegevens van het kenmerk. Als het antwoord niet onder een van deze velden, is geen toekenning vereist. Als het antwoord bevat de `contractualRules` veld en de `attributions` en/of `provider` velden, moet u de contractueel regels gebruiken voor het kenmerk van de gegevens.

Het volgende voorbeeld ziet u een entiteit die bestaat uit een MediaAttribution contractueel regel en een afbeelding met een `provider` veld. De regel MediaAttribution identificeert de afbeelding als het doel van de regel, zodat u de installatiekopie zou negeren `provider` veld en gebruiken in plaats daarvan de regel MediaAttribution voor toekenning.  

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

Als een contractueel regel bevat de `targetPropertyName` veld, de regel geldt alleen voor het betreffende veld. Anders wordt de regel geldt voor het bovenliggende object waarin de `contractualRules` veld.

In het volgende voorbeeld wordt de `LinkAttribution` regel bevat de `targetPropertyName` veld, zodat de regel geldt voor de `description` veld. Voor regels die betrekking hebben op specifieke velden, moet u een regel onmiddellijk na de gerichte gegevens met een hyperlink naar de website van de provider opnemen. Als u wilt de beschrijving van het kenmerk, bijvoorbeeld een regel onmiddellijk na de beschrijvende tekst die een hyperlink naar de gegevens op de website van de provider, bevat een koppeling naar contoso.com in dit geval maken.

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

### <a name="license-attribution"></a>Licentie toekenning

Als de lijst met regels voor contractueel bevat een [LicenseAttribution](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#licenseattribution) regel, moet u de kennisgeving weergeven op de regel onmiddellijk na de inhoud die de licentie is van toepassing op. De `LicenseAttribution` regel maakt gebruik van de `targetPropertyName` veld voor het identificeren van de eigenschap die de licentie is van toepassing op.

Hieronder vindt u een voorbeeld waarin een `LicenseAttribution` regel.

![Licentie toekenning](./media/cognitive-services-bing-entities-api/licenseattribution.png)

De aankondiging van de licentie die u wilt weergeven, moet een hyperlink naar de website die informatie over de licentie bevat bevatten. Normaal gesproken u de naam van de licentie een hyperlink. Bijvoorbeeld, als het bericht is **tekst onder licentie CC door SA** en CC door SA is de naam van de licentie, brengt u CC door SA een hyperlink.

### <a name="link-and-text-attribution"></a>Toekenning koppeling en tekst

De [LinkAttribution](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#linkattribution) en [TextAttribution](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#textattribution) regels worden doorgaans gebruikt voor het identificeren van de provider van de gegevens. De `targetPropertyName` veld identificeert het veld dat de regel van toepassing op.

Als u wilt de providers kenmerk, bevatten een regel onmiddellijk na de inhoud die van toepassing zijn de afschrijvingen (bijvoorbeeld het betreffende veld). De regel moet duidelijk label hebben om aan te geven dat de providers de bron van de gegevens zijn. Bijvoorbeeld, "gegevens van: contoso.com '. Voor `LinkAttribution` regels, moet u een hyperlink naar de website van de provider.

Hieronder vindt u een voorbeeld waarin `LinkAttribution` en `TextAttribution` regels.

![Koppeling tekst toekenning](./media/cognitive-services-bing-entities-api/linktextattribution.png)

### <a name="media-attribution"></a>Toekenning Media

Als de entiteit een afbeelding bevat en u weer te geven, moet u een koppeling doorklikken naar de website van de provider opgeven. Als de entiteit bevat een [MediaAttribution](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#mediaattribution) regel, gebruikt u de URL van de regel om de koppeling doorklikken te maken. Anders gebruikt u de URL die is opgenomen in de installatiekopie `provider` veld om de koppeling doorklikken te maken.

Hieronder vindt u een voorbeeld van een afbeelding `provider` veld en contractueel regels. Omdat in het voorbeeld de contractueel regel omvat, u de installatiekopie negeren `provider` veld en toe te passen de `MediaAttribution` regel.

![Toekenning Media](./media/cognitive-services-bing-entities-api/mediaattribution.png)

### <a name="search-or-search-like-experience"></a>Zoeken of zoeken ervaring

Net als met Bing Web zoeken-API kan de Search-API van Bing entiteit alleen worden gebruikt als gevolg van een directe gebruikersquery of zoeken, of als gevolg van een actie in een app of ervaring die logisch kan worden geïnterpreteerd als een gebruikersaanvraag zoeken. Ter illustratie hier volgen enkele voorbeelden van acceptabele zoeken of zoeken-achtige ervaringen.

- Gebruiker voert een query rechtstreeks in een zoekvak in een app
- Gebruiker selecteert specifieke tekst of afbeelding en aanvragen 'meer informatie' of 'als u meer informatie'
- Gebruiker vraagt een search-bot over een bepaald onderwerp
- Gebruiker dwells op een bepaald object of de entiteit in een scenario voor het type van visual zoeken

Als u niet zeker weet of uw ervaring een search-ervaring kan worden overwogen, kunt u contact opnemen met Microsoft.

## <a name="throttling-requests"></a>Aanvraagbeperkingen

[!INCLUDE [cognitive-services-bing-throttling-requests](../../../includes/cognitive-services-bing-throttling-requests.md)]

## <a name="next-steps"></a>Volgende stappen

Om snel aan de slag met de eerste aanvraag, Zie [maken van uw eerste aanvraag](./quick-start.md).

Vertrouwd raken met de [Bing-API van zoekservice entiteit v7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference) verwijzing. De verwijzing bevat de kopteksten en de queryparameters die u gebruikt om aan te vragen van de zoekresultaten. Dit omvat ook de definities van de antwoord-objecten. 

Ter verbetering van de gebruikerservaring van uw search-vak Zie [voor Automatische suggestie van Bing-API](../bing-autosuggest/get-suggested-search-terms.md). Als de gebruiker de zoekterm invoert, kunt u deze API om relevante querytermen die zijn gebruikt door anderen aanroepen.

Lees [Bing gebruiken en de vereisten van de weergave](./use-display-requirements.md) zodat u de regels over het gebruik van de zoekresultaten niet verbreken.