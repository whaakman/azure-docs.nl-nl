---
title: Hoe kan ik gevoel analyse in Text Analytics REST-API (Microsoft cognitieve Services in Azure) | Microsoft Docs
description: Klik hier voor meer informatie over het detecteren van gevoel met de tekst Analytics REST-API in Microsoft cognitieve Services op Azure in deze zelfstudie scenario.
services: cognitive-services
author: HeidiSteen
manager: cgronlun
ms.service: cognitive-services
ms.component: text-analytics
ms.topic: article
ms.date: 12/11/2017
ms.author: heidist
ms.openlocfilehash: 7ffd8bbe47409b459fdd308cd8d670d32f56649b
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344692"
---
# <a name="how-to-detect-sentiment-in-text-analytics"></a>Het vaststellen van gevoel in Tekstanalyse

De [gevoel Analysis API](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c9) tekstinvoer wordt geëvalueerd en retourneert een score gevoel voor elk document, variërend van 0 (negatief) tot 1 (positief).

Deze functie is nuttig voor het detecteren van positieve en negatieve gevoel in sociale media, klant revisies en discussieforums. Inhoud is die u hebt opgegeven; modellen en trainingsgegevens worden geleverd door de service.

Op dit moment ondersteunt gevoel Analysis Engels, Duits, Spaans en Frans. Andere talen zijn Preview-versie. Zie voor meer informatie [ondersteunde talen](../text-analytics-supported-languages.md).

## <a name="concepts"></a>Concepten

Tekstanalyse maakt gebruik van een machine learning-algoritme classificatie voor het genereren van een gevoel score tussen 0 en 1. Scores dichter bij 1 aangeven positief gevoel terwijl scores dichter bij 0 negatieve gevoel geven. Het model is pretrained met een uitgebreide hoofdtekst van de tekst gevoel koppelingen. Het is momenteel niet mogelijk om uw eigen trainingsgegevens te leveren. Het model wordt een combinatie van technieken tijdens de analyse van tekst, met inbegrip van de verwerking van tekst, onderdeel van spraak analyse, word plaatsing en word koppelingen. Zie voor meer informatie over de algoritme [Tekstanalyse Introducing](https://blogs.technet.microsoft.com/machinelearning/2015/04/08/introducing-text-analytics-in-the-azure-ml-marketplace/).

Gevoel analyse wordt uitgevoerd op het hele document, in plaats van het uitpakken van gevoel voor een bepaalde entiteit in de tekst. In de praktijk is er een neiging voor score berekenen voor nauwkeurigheid verbeteren wanneer documenten één of twee zinnen in plaats van een groot blok van tekst bevatten. Tijdens een beoordelingsfase objectiviteit bepaalt het model of een document in zijn geheel doelstelling of gevoel bevat. Een document dat is voornamelijk serviceniveaudoelstelling wordt niet uitgevoerd aan de gevoel detectie woordgroep, wat resulteert in een.50 score, met het niet verder verwerken. Voor documenten die u in de pijplijn is doorgaat, genereert de volgende fase een score boven of onder.50, afhankelijk van de mate van gevoel gedetecteerd in het document.

## <a name="preparation"></a>Voorbereiding

Gevoel analyse levert een hogere kwaliteitsresultaat als u kleinere reeksen tekst om te werken geeft op. Dit is het tegenovergestelde van sleutel woordgroep extraheren die beter presteert op grotere blokken tekst. Als u de beste resultaten vanuit beide bewerkingen, overweeg dienovereenkomstig reorganisatie van de invoer.

Moet u de JSON-documenten in deze indeling hebben: id, de tekst, de taal

De documentgrootte van het moet onder 5000 tekens per document en u kunt maximaal 1000 hebben items (id's) per verzameling. De verzameling is in de hoofdtekst van de aanvraag verzonden. Hier volgt een voorbeeld van inhoud die u voor gevoel analyse indienen mogelijk.

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

+ Maak een **POST** aanvraag. Bekijk de API-documentatie voor deze aanvraag: [gevoel Analysis API](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c9)

+ Het HTTP-eindpunt voor het ophalen van de belangrijkste woordgroep ingesteld. Het omvat de `/sentiment` resource: `https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/sentiment`

+ Stel een aanvraagheader om op te nemen van de toegangssleutel voor Text Analytics-bewerkingen. Zie voor meer informatie [eindpunten zoeken en toegangssleutels](text-analytics-how-to-access-key.md).

+ Geef de JSON-documenten verzameling die u hebt voorbereid voor deze analyse in de aanvraagtekst.

> [!Tip]
> Gebruik [Postman](text-analytics-how-to-call-api.md) of open de **API testen console** in de [documentatie](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c9) aan de structuur van de aanvraag en dit bericht naar de service.

## <a name="step-2-post-the-request"></a>Stap 2: De aanvraag posten

Analyse wordt na ontvangst van de aanvraag uitgevoerd. De service accepteert maximaal 100 aanvragen per minuut. Elke aanvraag mag maximaal 1 MB.

Intrekken van de service staatloze is. Er zijn geen gegevens worden opgeslagen in uw account. Resultaten worden direct in het antwoord geretourneerd.


## <a name="step-3-view-results"></a>Stap 3: Resultaten weergeven

De analyzer gevoel classificeert tekst als het hoofdzakelijk positief of negatief, een score in het bereik van 0 tot en met 1 toewijzen. Waarden in de buurt 0,5 zijn neutrale of onbepaalde. Een score van 0,5 geeft neutraliteit. Als een tekenreeks kan niet worden geanalyseerd voor gevoel of geen gevoel heeft, de score is altijd 0,5 exact. Als u in een Spaanse tekenreeks met een code voor de Engelse taal doorgeeft, is de score 0,5.

Uitvoer wordt onmiddellijk geretourneerd. U kunt de resultaten naar een toepassing die JSON accepteert stream of sla de uitvoer naar een bestand op het lokale systeem en vervolgens importeren in een toepassing waarmee u kunt sorteren en manipuleren van de gegevens zoeken.

Het volgende voorbeeld ziet het antwoord voor de documentenverzameling in dit artikel.

```
{
    "documents": [
        {
            "score": 0.9999237060546875,
            "id": "1"
        },
        {
            "score": 0.0000540316104888916,
            "id": "2"
        },
        {
            "score": 0.99990355968475342,
            "id": "3"
        },
        {
            "score": 0.980544924736023,
            "id": "4"
        },
        {
            "score": 0.99996328353881836,
            "id": "5"
        }
    ],
    "errors": []
}
```

## <a name="summary"></a>Samenvatting

In dit artikel hebt u geleerd concepten en werkstroom voor gevoel analyse Text Analytics gebruiken in cognitieve Services. Samengevat:

+ [Gevoel analysis API](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c9) is beschikbaar voor de geselecteerde talen.
+ JSON-documenten in de aanvraagtekst bevatten een code-id, tekst en taal.
+ POST-aanvraag is een `/sentiment` eindpunt, met een persoonlijk [toegang krijgen tot sleutel en een eindpunt](text-analytics-how-to-access-key.md) die geldig is voor uw abonnement.
+ Antwoorduitvoer, die uit een score gevoel voor elk document-ID bestaat, kan worden gestreamd naar alle Apps die JSON accepteert, zoals Excel en Power BI, enzovoort.

## <a name="see-also"></a>Zie ook 

 [Overzicht van tekst Analytics](../overview.md)  
 [Veelgestelde vragen (FAQ)](../text-analytics-resource-faq.md)</br>
 [Tekst Analytics productpagina](//go.microsoft.com/fwlink/?LinkID=759712) 

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Uitpakken van sleutel zinnen](text-analytics-how-to-keyword-extraction.md)
