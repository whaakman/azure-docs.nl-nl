---
title: 'Voorbeeld: Sleuteltermen extraheren in de Text Analytics'
titleSuffix: Azure Cognitive Services
description: Leer hoe u sleuteltermen extraheert met de Text Analytics REST-API.
services: cognitive-services
author: HeidiSteen
manager: cgronlun
ms.service: cognitive-services
ms.component: text-analytics
ms.topic: sample
ms.date: 09/12/2018
ms.author: heidist
ms.openlocfilehash: 62c078a8a72cd0a3633b7dd5fda1545f01067dbc
ms.sourcegitcommit: 616e63d6258f036a2863acd96b73770e35ff54f8
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/14/2018
ms.locfileid: "45605484"
---
# <a name="example-how-to-extract-key-phrases-in-text-analytics"></a>Voorbeeld: Sleuteltermen extraheren in de Text Analytics

De [Sleuteltermextractie API](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c6) evalueert ongestructureerde tekst en retourneert voor elk JSON-document een lijst met sleuteltermen. 

Deze functie is handig als u snel de belangrijkste punten moet identificeren van een verzameling documenten. Bijvoorbeeld, bij de invoertekst "het eten was heerlijk en de bediening fantastisch", retourneert de service de belangrijkste gespreksonderwerpen: 'eten' en "bediening fantastisch".

Op dit moment ondersteunt Sleuteltermextractie Engels, Duits, Spaans en Japans. Andere talen bevinden zich in preview-fase. Zie voor meer informatie [Ondersteunde talen](../text-analytics-supported-languages.md).

## <a name="preparation"></a>Voorbereiding

Sleuteltermextractie werkt het beste wanneer u grotere segmenten tekst geeft. Dit is het tegenovergestelde van sentimentanalyse, wat beter presteert op kleinere blokken tekst. Overweeg dienovereenkomstig herstructurering van de invoer voor de beste resultaten uit beide bewerkingen.

U moet JSON-documenten in deze indeling hebben: id, tekst, taal

De documentgrootte moet onder maximaal 5000 tekens per document zijn, en u kunt maximaal 1000 items (id's) per verzameling hebben. De verzameling is in de hoofdtekst van de aanvraag ingediend. Het volgende voorbeeld geeft de inhoud die u kunt indienen voor sleuteltermextractie.

```
    {
        "documents": [
            {
                "language": "en",
                "id": "1",
                "text": "We love this trail and make the trip every year. The views are breathtaking and well worth the hike!"
            },
            {
                "language": "en",
                "id": "2",
                "text": "Poorly marked trails! I thought we were goners. Worst hike ever."
            },
            {
                "language": "en",
                "id": "3",
                "text": "Everyone in my family liked the trail but thought it was too challenging for the less athletic among us. Not necessarily recommended for small children."
            },
            {
                "language": "en",
                "id": "4",
                "text": "It was foggy so we missed the spectacular views, but the trail was ok. Worth checking out if you are in the area."
            },                
            {
                "language": "en",
                "id": "5",
                "text": "This is my favorite trail. It has beautiful views and many places to stop and rest"
            }
        ]
    }
```    
    
## <a name="step-1-structure-the-request"></a>Stap 1: Structuur van de aanvraag

Meer informatie over de definitie van de aanvraag kunt u vinden in [De Text Analytics-API aanroepen](text-analytics-how-to-call-api.md). De volgende punten zijn voor uw gemak opnieuw geformuleerd:

+ Maak een **POST**-aanvraag. Bekijk de API-documentatie voor deze aanvraag: [Key Phrases-API](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c6)

+ Stel het HTTP-eindpunt in voor sleuteltermextractie. Deze moet de `/keyphrases`-resource: `https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/keyPhrases` bevatten

+ Stel een aanvraagheader in om de toegangssleutel voor de Text Analytics-bewerkingen op te nemen. Zie voor meer informatie [Eindpunten en toegangssleutels zoeken](text-analytics-how-to-access-key.md).

+ Verstrek in de hoofdtekst van de aanvraag de JSON-documentenverzameling die u hebt voorbereid voor deze analyse

> [!Tip]
> Gebruik [Postman](text-analytics-how-to-call-api.md) of open de **API-testconsole** in de [documentatie](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c6) om de aanvraag te structureren en POST deze in de service.

## <a name="step-2-post-the-request"></a>Stap 2: De aanvraag posten

Analyse wordt uitgevoerd na ontvangst van de aanvraag. De service accepteert maximaal 100 aanvragen per minuut. Elke aanvraag kan maximaal 1 MB zijn.

Terughalen als de service staatloos is. Er worden geen gegevens opgeslagen in uw account. Resultaten worden onmiddellijk in het antwoord geretourneerd.

## <a name="step-3-view-results"></a>Stap 3: Resultaten weergeven

Alle POST-verzoeken retourneren een ingedeeld JSON-antwoord met de id's en gedetecteerde eigenschappen.

Uitvoer wordt onmiddellijk geretourneerd. U kunt de resultaten streamen naar een toepassing die JSON accepteert of u kunt de uitvoer opslaan als lokaal bestand en vervolgens importeren in een toepassing waarmee u kunt sorteren, zoeken en de gegevens kunt manipuleren.

Een voorbeeld van de uitvoer voor sleuteltermextractie:

```
    "documents": [
        {
            "keyPhrases": [
                "year",
                "trail",
                "trip",
                "views"
            ],
            "id": "1"
        },
        {
            "keyPhrases": [
                "marked trails",
                "Worst hike",
                "goners"
            ],
            "id": "2"
        },
        {
            "keyPhrases": [
                "trail",
                "small children",
                "family"
            ],
            "id": "3"
        },
        {
            "keyPhrases": [
                "spectacular views",
                "trail",
                "area"
            ],
            "id": "4"
        },
        {
            "keyPhrases": [
                "places",
                "beautiful views",
                "favorite trail"
            ],
            "id": "5"
        }
```

Zoals is vermeld, de analyzer vindt en verwijdert niet-essentiële woorden, en houdt enkelvoudige termen of zinnen die een onderwerp of het object van een zin lijken te zijn. 

## <a name="summary"></a>Samenvatting

In dit artikel hebt kennis gemaakt met concepten en werkstromen voor sleuteltermextractie met behulp van Text Analytics in Cognitive Services. Samenvatting:

+ [Sleuteltermextractie-API](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c6) is beschikbaar in bepaalde talen.
+ JSON-documenten in de aanvraagbody omvatten een id, tekst en taalcode.
+ POST-aanvraag is een `/keyphrases`-eindpunt die een persoonlijke [toegangssleutel en een eindpunt](text-analytics-how-to-access-key.md) gebruikt die geldig zijn voor uw abonnement.
+ De antwoorduitvoer, die uit de sleutelwoorden en belangrijke woordgroepen voor elk document-ID bestaat, kan worden gestreamd naar alle apps die JSON accepteren, met inbegrip van Excel en Power BI, om er maar een paar te noemen.

## <a name="see-also"></a>Zie ook 

 [Overzicht van Text Analytics](../overview.md)  
 [Veelgestelde vragen](../text-analytics-resource-faq.md)</br>
 [Text Analytics-productpagina](//go.microsoft.com/fwlink/?LinkID=759712) 

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Text Analytics-API](//westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c6)
