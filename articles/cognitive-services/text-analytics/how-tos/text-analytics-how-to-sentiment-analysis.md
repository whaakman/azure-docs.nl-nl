---
title: Sentimentanalyse in de Text Analytics REST API (Microsoft Cognitive Services in Azure) | Microsoft Docs
description: Leer hoe u sentiment kunt detecteren met behulp van de Text Analytics REST API in Microsoft Cognitive Services in Azure in deze zelfstudie.
services: cognitive-services
author: HeidiSteen
manager: cgronlun
ms.service: cognitive-services
ms.component: text-analytics
ms.topic: sample
ms.date: 09/12/2018
ms.author: heidist
ms.openlocfilehash: bbe9ffd0709157b5f0389ccc68a285b9c3829db9
ms.sourcegitcommit: 0b7fc82f23f0aa105afb1c5fadb74aecf9a7015b
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/14/2018
ms.locfileid: "51632863"
---
# <a name="example-how-to-detect-sentiment-in-text-analytics"></a>Voorbeeld: Gevoel detecteren in Text Analytics

De [Sentimentanalyse-API](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c9) evalueert tekstinvoer en retourneert een gevoelsscore voor elk document, variërend van 0 (negatief) tot 1 (positief).

Deze mogelijkheid is handig voor het detecteren van een positief en negatief gevoel in sociale media, klantbeoordelingen en discussieforums. Inhoud is die u hebt opgegeven; modellen en trainingsgegevens worden geleverd door de service.

Op dit moment ondersteunt Sentimentanalyse Engels, Duits, Spaans en Frans. Andere talen bevinden zich in preview-fase. Zie voor meer informatie [Ondersteunde talen](../text-analytics-supported-languages.md).

> [!TIP]
> Text Analytics biedt ook een Docker-containerinstallatiekopie op basis van Linux voor sentimentanalyse. U kunt de [Text Analytics-container dus dicht bij uw gegevens installeren en uitvoeren](text-analytics-how-to-install-containers.md).

## <a name="concepts"></a>Concepten

Text Analytics maakt gebruik van een machine learning-classificatiealgoritme voor het genereren van een gevoelsscore tussen 0 en 1. Scores dicht bij 1 wijzen op een positief gevoel, terwijl scores dicht bij 0 op een negatief gevoel wijzen. Het model is vooraf getraind met een uitgebreide hoofdtekst met gevoelskoppelingen. Het is momenteel niet mogelijk om uw eigen trainingsgegevens te verstrekken. Het model gebruikt een combinatie van technieken tijdens de tekstanalyse, met inbegrip van de verwerking van tekst, analyse van woordsoorten, woordplaatsing en woordkoppelingen. Zie voor meer informatie over het algoritme [Introductie Text Analytics](https://blogs.technet.microsoft.com/machinelearning/2015/04/08/introducing-text-analytics-in-the-azure-ml-marketplace/).

Sentimentanalyse wordt uitgevoerd op het hele document, in plaats van een gevoel voor een bepaalde entiteit in de tekst te extraheren. In de praktijk is er een neiging voor het scoren van nauwkeurigheid te verbeteren wanneer documenten één of twee zinnen bevatten in plaats van een grote blok tekst. Tijdens een fase van de evaluatie objectiviteit bepaalt het model of een document als geheel objectief is of gevoel bevat. Een document dat voornamelijk objectief is, gaat niet in de richting van de gevoelsdetectiefrase, wat resulteert in een .50 score, met geen verdere verwerking. Voor de documenten die doorgaan in de pijplijn, genereert de volgende fase een score boven of onder de .50, afhankelijk van de mate van gevoel gedetecteerd in het document.

## <a name="preparation"></a>Voorbereiding

Gevoelsanalyse produceert een hoger kwaliteitsresultaat wanneer u ze kleinere segmenten van tekst aanbiedt om op te werken. Dit is het tegenovergestelde van sleuteltermextractie, wat beter presteert op grotere blokken tekst. Overweeg dienovereenkomstig herstructurering van de invoer voor de beste resultaten uit beide bewerkingen.

U moet JSON-documenten in deze indeling hebben: id, tekst, taal

De documentgrootte moet onder maximaal 5000 tekens per document zijn, en u kunt maximaal 1000 items (id's) per verzameling hebben. De verzameling is in de hoofdtekst van de aanvraag ingediend. Hier volgt een voorbeeld van de inhoud die u voor gevoelsanalyse kan indienen.

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

+ Maak een **POST**-aanvraag. Controleer de API-documentatie voor deze aanvraag: [Sentimentanalyse-API](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c9)

+ Stel het HTTP-eindpunt in voor sentimentanalyse, met behulp van een Text Analytics-resource in Azure of een geïnstantieerde [Text Analytics-container](text-analytics-how-to-install-containers.md). Deze moet de `/sentiment`-resource: `https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/sentiment` bevatten

+ Stel een aanvraagheader in om de toegangssleutel voor de Text Analytics-bewerkingen op te nemen. Zie voor meer informatie [Eindpunten en toegangssleutels zoeken](text-analytics-how-to-access-key.md).

+ Verstrek in de hoofdtekst van de aanvraag de JSON-documentenverzameling die u hebt voorbereid voor deze analyse.

> [!Tip]
> Gebruik [Postman](text-analytics-how-to-call-api.md) of open de **API-testconsole** in de [documentatie](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c9) om de aanvraag te structureren en POST deze in de service.

## <a name="step-2-post-the-request"></a>Stap 2: De aanvraag posten

Analyse wordt uitgevoerd na ontvangst van de aanvraag. De service accepteert maximaal 100 aanvragen per minuut. Elke aanvraag kan maximaal 1 MB zijn.

Terughalen als de service staatloos is. Er worden geen gegevens opgeslagen in uw account. Resultaten worden onmiddellijk in het antwoord geretourneerd.


## <a name="step-3-view-results"></a>Stap 3: Resultaten weergeven

De gevoelsanalyse classificeert tekst als voornamelijk positief of negatief, en wijst een score toe in het bereik van 0 tot 1. Waarden dicht bij 0,5 zijn neutraal of onbepaald. Een score van 0,5 geeft neutraliteit aan. Als een tekenreeks niet kan worden geanalyseerd op gevoel of geen gevoel heeft, is de score altijd precies 0,5. Bijvoorbeeld, als u in een Spaanse tekenreeks doorgeeft met een Engelse taalcode is de score 0,5.

Uitvoer wordt onmiddellijk geretourneerd. U kunt de resultaten streamen naar een toepassing die JSON accepteert of u kunt de uitvoer opslaan als lokaal bestand en vervolgens importeren in een toepassing waarmee u kunt sorteren, zoeken en de gegevens kunt manipuleren.

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

In dit artikel hebt u concepten en de werkstroom geleerd voor gevoelsanalyse met behulp van Text Analytics in Cognitive Services. Samenvatting:

+ [Gevoelsanalyse API](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c9) is beschikbaar voor de geselecteerde talen.
+ JSON-documenten in de aanvraagbody omvatten een id, tekst en taalcode.
+ POST-aanvraag is een `/sentiment`-eindpunt die een persoonlijke [toegangssleutel en een eindpunt](text-analytics-how-to-access-key.md) gebruikt die geldig zijn voor uw abonnement.
+ Antwoorduitvoer, die uit een gevoelsscore voor elk document-ID bestaat, kan worden gestreamd naar alle apps die JSON, met inbegrip van Excel en Power BI om er maar enkele te noemen, accepteren.

## <a name="see-also"></a>Zie ook 

 [Overzicht van Text Analytics](../overview.md)  
 [Veelgestelde vragen](../text-analytics-resource-faq.md)</br>
 [Text Analytics-productpagina](//go.microsoft.com/fwlink/?LinkID=759712) 

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Sleuteltermen extraheren](text-analytics-how-to-keyword-extraction.md)
