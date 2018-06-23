---
title: Instructies sleutel woordgroep extractie in Text Analytics REST-API (Microsoft cognitieve Services in Azure) | Microsoft Docs
description: Het uitpakken van sleutel woordgroepen met de tekst Analytics REST-API in Microsoft cognitieve Services op Azure in deze zelfstudie scenario.
services: cognitive-services
author: HeidiSteen
manager: cgronlun
ms.service: cognitive-services
ms.component: text-analytics
ms.topic: article
ms.date: 3/07/2018
ms.author: heidist
ms.openlocfilehash: 78b100e737242fa9f56e50275ef2038d8895349e
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344691"
---
# <a name="how-to-extract-key-phrases-in-text-analytics"></a>Het uitpakken van sleutel zinnen in Tekstanalyse

De [sleutel woordgroep extractie API](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c6) evalueert ongestructureerde tekst is en retourneert een lijst met belangrijke termen voor elk JSON-document. 

Deze mogelijkheid is handig als u nodig hebt om snel te identificeren de belangrijkste punten in een verzameling van documenten. Bijvoorbeeld, bepaalde invoertekst 'de voeding delicious is en er zijn prachtige personeel', stuurt de service te bespreken de belangrijkste punten: 'voeding' en 'prachtige personeel'.

Uitpakken van sleutel woordgroep ondersteunt momenteel, Nederlands, Duits, Engels en Japans. Andere talen zijn Preview-versie. Zie voor meer informatie [ondersteunde talen](../text-analytics-supported-languages.md).

## <a name="preparation"></a>Voorbereiding

Uitpakken van sleutel woordgroep werkt het beste als u hieraan groter segmenten van tekst om te werken op. Dit is het tegenovergestelde van gevoel analyse, die beter presteert op kleinere blokken van tekst. Als u de beste resultaten vanuit beide bewerkingen, overweeg dienovereenkomstig reorganisatie van de invoer.

Moet u de JSON-documenten in deze indeling hebben: id, de tekst, de taal

De documentgrootte van het moet onder 5000 tekens per document en u kunt maximaal 1000 hebben items (id's) per verzameling. De verzameling is in de hoofdtekst van de aanvraag verzonden. Het volgende voorbeeld wordt een afbeelding van inhoud die u voor het ophalen van de belangrijkste woordgroep kan indienen.

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
    
## <a name="step-1-structure-the-request"></a>Stap 1: De aanvraag structureren

Meer informatie over de definitie van de aanvraag kunnen worden gevonden in [hoe de tekst Analytics-API aan te roepen](text-analytics-how-to-call-api.md). De volgende punten worden aangepast voor het gemak:

+ Maak een **POST** aanvraag. Bekijk de API-documentatie voor deze aanvraag: [sleutel zinnen API](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c6)

+ Het HTTP-eindpunt voor het ophalen van de belangrijkste woordgroep ingesteld. Het omvat de `/keyphrases` resource: `https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/keyPhrases`

+ Stel een aanvraagheader om op te nemen van de toegangssleutel voor Text Analytics-bewerkingen. Zie voor meer informatie [eindpunten zoeken en toegangssleutels](text-analytics-how-to-access-key.md).

+ Geef de JSON-documenten verzameling die u hebt voorbereid voor deze analyse in de aanvraagtekst.

> [!Tip]
> Gebruik [Postman](text-analytics-how-to-call-api.md) of open de **API testen console** in de [documentatie](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c6) aan de structuur van een aanvraag en dit bericht naar de service.

## <a name="step-2-post-the-request"></a>Stap 2: De aanvraag posten

Analyse wordt na ontvangst van de aanvraag uitgevoerd. De service accepteert maximaal 100 aanvragen per minuut. Elke aanvraag mag maximaal 1 MB.

Intrekken van de service staatloze is. Er zijn geen gegevens worden opgeslagen in uw account. Resultaten worden direct in het antwoord geretourneerd.

## <a name="step-3-view-results"></a>Stap 3: Resultaten weergeven

Alle POST-verzoeken retourneren een JSON-indeling van antwoord met de id's en eigenschappen gedetecteerd.

Uitvoer wordt onmiddellijk geretourneerd. U kunt de resultaten naar een toepassing die JSON accepteert stream of sla de uitvoer naar een bestand op het lokale systeem en vervolgens importeren in een toepassing waarmee u kunt sorteren en manipuleren van de gegevens zoeken.

Een voorbeeld van de uitvoer voor het ophalen van de belangrijkste woordgroep wordt volgende weergegeven:

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

Zoals vermeld, de analyzer vindt wordt verwijderd van niet-essentiële woorden en houdt enkelvoudige termen of woordgroepen die lijken te zijn van het onderwerp of een object van een zin. 

## <a name="summary"></a>Samenvatting

In dit artikel hebt u geleerd concepten en werkstroom voor het uitpakken van sleutel woordgroep Text Analytics gebruiken in cognitieve Services. Samengevat:

+ [Sleutel woordgroep extractie API](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c6) is beschikbaar voor de geselecteerde talen.
+ JSON-documenten in de aanvraagtekst bevatten een code-id, tekst en taal.
+ POST-aanvraag is een `/keyphrases` eindpunt, met een persoonlijk [toegang krijgen tot sleutel en een eindpunt](text-analytics-how-to-access-key.md) die geldig is voor uw abonnement.
+ Antwoorduitvoer, die uit sleutel woorden en woordgroepen voor elk document-ID bestaat, kan worden gestreamd naar alle Apps die JSON accepteert, zoals Excel en Power BI, enzovoort.

## <a name="see-also"></a>Zie ook 

 [Overzicht van tekst Analytics](../overview.md)  
 [Veelgestelde vragen (FAQ)](../text-analytics-resource-faq.md)</br>
 [Tekst Analytics productpagina](//go.microsoft.com/fwlink/?LinkID=759712) 

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Tekstanalyse API](//westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c6)
