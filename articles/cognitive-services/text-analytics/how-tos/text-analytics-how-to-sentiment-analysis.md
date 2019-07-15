---
title: Sentimentanalyse met behulp van de Text Analytics REST-API van Azure Cognitive Services | Microsoft Docs
description: Informatie over het gevoel detecteren met behulp van de Text Analytics REST-API.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: sample
ms.date: 02/26/2019
ms.author: aahi
ms.openlocfilehash: c3004dd3910dd5fdafc933efa213c9f097310e87
ms.sourcegitcommit: 6b41522dae07961f141b0a6a5d46fd1a0c43e6b2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/15/2019
ms.locfileid: "68001709"
---
# <a name="example-detect-sentiment-with-text-analytics"></a>Voorbeeld: Gevoel met Tekstanalyse detecteren

De [analyse-API van Azure-Sentiment](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c9) tekstinvoer evalueert en retourneert een gevoelsscore voor elk document. Scores de variëren van 0 (negatief) tot 1 (positief).

Deze mogelijkheid is handig voor het detecteren van een positief en negatief gevoel in sociale media, klantbeoordelingen en discussieforums. Inhoud is die u hebt opgegeven. Modellen en trainingsgegevens worden geleverd door de service.

De analyse-Gevoels-API ondersteunt momenteel, Nederlands, Duits, Spaans en Frans. Andere talen bevinden zich in preview-fase. Zie voor meer informatie [Ondersteunde talen](../text-analytics-supported-languages.md).

> [!TIP]
> De Azure Tekstanalyse-API biedt ook een Docker op basis van Linux-containerinstallatiekopie voor sentimentanalyse, zodat u deze kunt [installeren en uitvoeren van de Text Analytics-container](text-analytics-how-to-install-containers.md) dicht bij uw gegevens.

## <a name="concepts"></a>Concepten

Text Analytics maakt gebruik van een machine learning-classificatiealgoritme voor het genereren van een gevoelsscore tussen 0 en 1. Scores dicht bij 1 wijzen op een positief gevoel, terwijl scores dicht bij 0 op een negatief gevoel wijzen. Het model is vooraf getraind met een uitgebreide hoofdtekst met gevoelskoppelingen. Op dit moment is het niet mogelijk om uw eigen trainingsgegevens te bieden. Het model gebruikt een combinatie van technieken tijdens de analyse van tekst. Technieken zijn onder andere tekst verwerking, analyse van part-of-speech, word plaatsing en word koppelingen. Zie voor meer informatie over het algoritme [Introductie Text Analytics](https://blogs.technet.microsoft.com/machinelearning/2015/04/08/introducing-text-analytics-in-the-azure-ml-marketplace/).

Sentimentanalyse wordt uitgevoerd op het hele document, in plaats van een gevoel voor een bepaalde entiteit in de tekst te extraheren. In de praktijk is er een neiging voor het scoren van nauwkeurigheid te verbeteren wanneer documenten één of twee zinnen in plaats van een grote blok tekst bevatten. Tijdens een fase van de evaluatie objectiviteit bepaalt het model of een document als geheel objectief is of gevoel bevat. Een document dat is voornamelijk doel niet voortgang met de fase voor het detecteren van sentiment, wat tot een 0,50 score leidt, met geen verdere verwerking. Voor documenten die verder in de pijplijn, genereert de volgende fase een score boven of onder 0,50. De score is afhankelijk van de mate van sentiment gedetecteerd in het document.

## <a name="preparation"></a>Voorbereiding

Sentimentanalyse produceert een resultaat hogere kwaliteit wanneer u deze kleinere segmenten van de tekst die verleent moet worden gewerkt. Dit is het tegenovergestelde van sleuteltermextractie, wat beter presteert op grotere blokken tekst. Overweeg dienovereenkomstig herstructurering van de invoer voor de beste resultaten uit beide bewerkingen.

U moet de JSON-documenten in deze indeling hebben: -ID, tekst en taal.

Grootte van document moet onder 5.120 tekens per document. U kunt maximaal 1000 hebben items (id's) per verzameling. De verzameling is in de hoofdtekst van de aanvraag ingediend. Het volgende voorbeeld wordt een voorbeeld van de inhoud die u voor sentimentanalyse indienen mogelijk:

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

## <a name="step-1-structure-the-request"></a>Stap 1: Structureer de aanvraag

Zie voor meer informatie over de definitie van aanvraag [de Tekstanalyse-API aanroepen](text-analytics-how-to-call-api.md). De volgende punten zijn voor uw gemak opnieuw geformuleerd:

+ Maak een POST-aanvraag. De API-documentatie voor deze aanvraag, Zie de [Analysis-Gevoels-API](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c9).

+ Stel de HTTP-eindpunt voor sentimentanalyse met behulp van een Text Analytics-resource op Azure of een exemplaar [Text Analytics-container](text-analytics-how-to-install-containers.md). Het omvat de `/sentiment` resource: `https://westus.api.cognitive.microsoft.com/text/analytics/v2.1/sentiment`.

+ Stel een aanvraagheader in om de toegangssleutel voor de Text Analytics-bewerkingen op te nemen. Zie voor meer informatie, [eindpunten vinden en toegangssleutels](text-analytics-how-to-access-key.md).

+ Verstrek in de hoofdtekst van de aanvraag de JSON-documentenverzameling die u hebt voorbereid voor deze analyse.

> [!Tip]
> Gebruik [Postman](text-analytics-how-to-call-api.md) of open de **API testconsole** in de [documentatie](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c9) structuur van de aanvraag en plaatst deze in de service.

## <a name="step-2-post-the-request"></a>Stap 2: Plaats de aanvraag

Analyse wordt uitgevoerd na ontvangst van de aanvraag. Voor meer informatie over de grootte en het aantal aanvragen die u kunt verzenden per minuut en seconde, Zie de [gegevenslimieten](../overview.md#data-limits) sectie in het overzicht.

Terughalen als de service staatloos is. Er worden geen gegevens opgeslagen in uw account. Resultaten worden onmiddellijk in het antwoord geretourneerd.


## <a name="step-3-view-the-results"></a>Stap 3: De resultaten bekijken

De analyzer sentiment classificeert tekst als voornamelijk positief of negatief zijn. Er wordt een score in het bereik van 0 tot 1 toegewezen. Waarden dicht bij 0,5 zijn neutraal of onbepaald. Een score van 0,5 geeft neutraliteit aan. Als een tekenreeks kan niet worden geanalyseerd op gevoelsbeoordelingen of geen sentiment heeft, de score is altijd 0,5 precies. Bijvoorbeeld, als u in een Spaanse tekenreeks doorgeeft met een Engelse taalcode is de score 0,5.

Uitvoer wordt onmiddellijk geretourneerd. U kunt de resultaten naar een toepassing die JSON accepteert streamen of sla de uitvoer naar een bestand op het lokale systeem. Importeer de uitvoer in een toepassing die u gebruiken kunt om te sorteren, zoeken en de gegevens te manipuleren.

Het volgende voorbeeld ziet het antwoord voor de documentenverzameling in dit artikel:

```json
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

## <a name="sentiment-analysis-v3-public-preview"></a>Openbare preview voor v3 sentiment-analyse

De [volgende versie van Sentimentanalyse](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-0-preview/operations/56f30ceeeda5650db055a3c9) is nu beschikbaar voor openbare preview. Het biedt aanzienlijke verbeteringen in de nauwkeurigheid en de details van de API's tekst categorisatie en scoren. 

> [!NOTE]
> * De indeling van de Sentimentanalyse v3-aanvraag en [gegevenslimieten](../overview.md#data-limits) zijn hetzelfde als de vorige versie.
> * Op dit moment Sentimentanalyse v3: 
>    * Ondersteunt momenteel alleen de Engelse taal.  
>    * Is beschikbaar in de volgende regio's: `Central US`, `Central Canada`, en `East Asia`.

|Functie |Description  |
|---------|---------|
|Verbeterde nauwkeurigheid     | Vergeleken met eerdere versies aanzienlijke verbetering voor het detecteren van positieve, neutrale, negatieve en gecombineerde sentimenten in tekstdocumenten.           |
|Document en op zinsniveau gevoelsscore     | Detecteer het sentiment van zowel een document als van de afzonderlijke zinnen in het document. Als het document meerdere zinnen bevat, wordt aan elke zin een sentimentscore toegewezen.         |
|Sentiment categorie en score     | De API retourneert nu sentiment categorieën voor tekst, naast een gevoelsscore. De categorieën zijn `positive`, `negative`, `neutral`, en `mixed`.       |
| Verbeterde uitvoer | Sentimentanalyse retourneert nu informatie voor zowel een document met volledige tekst en de afzonderlijke zinnen. |

### <a name="sentiment-labeling"></a>Sentiment labels

Sentiment-analyse v3 kunt terugkeren scores en labels op het niveau van een zin en het document. De scores en labels zijn `positive`, `negative`, en `neutral`. Op het documentniveau van het, de `mixed` label voor stemming (niet de score) ook kunnen worden geretourneerd. Het gevoel van het document wordt bepaald door het samenvoegen van de scores van de zinnen.

| Zin sentiment                                                        | Documentlabel geretourneerd |
|---------------------------------------------------------------------------|----------------|
| Ten minste één positieve zin en de rest van de zinnen zijn neutraal. | `positive`     |
| Ten minste één negatieve zin en de rest van de zinnen zijn neutraal.  | `negative`     |
| Ten minste één negatieve zin en ten minste één positieve zin.         | `mixed`        |
| Alle zinnen zijn neutraal.                                                 | `neutral`      |

### <a name="sentiment-analysis-v3-example-request"></a>Sentiment-analyse v3 voorbeeldaanvraag

De volgende JSON is een voorbeeld van een aanvraag aan de nieuwe versie van Sentimentanalyse. De opmaak van de aanvraag is hetzelfde als de vorige versie:

```json
{
  "documents": [
    {
      "language": "en",
      "id": "1",
      "text": "Hello world. This is some input text that I love."
    },
    {
      "language": "en",
      "id": "2",
      "text": "It's incredibly sunny outside! I'm so happy."
    }
  ]
}
```

### <a name="sentiment-analysis-v3-example-response"></a>Sentiment-analyse v3 voorbeeld van een antwoord

Indeling van de aanvraag is hetzelfde als de vorige versie, is de indeling van het antwoord gewijzigd. De volgende JSON wordt een voorbeeld van de reactie van de nieuwe versie van de API:

```json
{
    "documents": [
        {
            "id": "1",
            "sentiment": "positive",
            "documentScores": {
                "positive": 0.98570585250854492,
                "neutral": 0.0001625834556762,
                "negative": 0.0141316400840878
            },
            "sentences": [
                {
                    "sentiment": "neutral",
                    "sentenceScores": {
                        "positive": 0.0785155147314072,
                        "neutral": 0.89702343940734863,
                        "negative": 0.0244610067456961
                    },
                    "offset": 0,
                    "length": 12
                },
                {
                    "sentiment": "positive",
                    "sentenceScores": {
                        "positive": 0.98570585250854492,
                        "neutral": 0.0001625834556762,
                        "negative": 0.0141316400840878
                    },
                    "offset": 13,
                    "length": 36
                }
            ]
        },
        {
            "id": "2",
            "sentiment": "positive",
            "documentScores": {
                "positive": 0.89198976755142212,
                "neutral": 0.103382371366024,
                "negative": 0.0046278294175863
            },
            "sentences": [
                {
                    "sentiment": "positive",
                    "sentenceScores": {
                        "positive": 0.78401315212249756,
                        "neutral": 0.2067587077617645,
                        "negative": 0.0092281140387058
                    },
                    "offset": 0,
                    "length": 30
                },
                {
                    "sentiment": "positive",
                    "sentenceScores": {
                        "positive": 0.99996638298034668,
                        "neutral": 0.0000060341349126,
                        "negative": 0.0000275444017461
                    },
                    "offset": 31,
                    "length": 13
                }
            ]
        }
    ],
    "errors": []
}
```

### <a name="example-c-code"></a>Voorbeeld C# code

U vindt een voorbeeld C# toepassing die wordt deze versie van Sentimentanalyse op [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/tree/master/dotnet/Language/SentimentV3.cs).

## <a name="summary"></a>Samenvatting

In dit artikel hebt u geleerd concepten en werkstroom voor sentimentanalyse met behulp van de Text Analytics in Azure Cognitive Services. Samenvatting:

+ De [Analysis-Gevoels-API](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c9) is beschikbaar voor de geselecteerde talen.
+ JSON-documenten in de aanvraagtekst omvatten een code-ID, tekst en taal.
+ De POST-aanvraag is een `/sentiment` eindpunt met behulp van een persoonlijk [toegang krijgen tot de sleutel en een eindpunt](text-analytics-how-to-access-key.md) die geldig is voor uw abonnement.
+ Antwoorduitvoer, die uit een gevoelsscore voor elk document-ID bestaat, kan worden gestreamd naar alle Apps die JSON accepteert. Voorbeeld-apps zijn Excel en Power BI, als u wilt een paar te noemen.

## <a name="see-also"></a>Zie ook 

 [Overzicht van Text Analytics](../overview.md)  
 [Veelgestelde vragen](../text-analytics-resource-faq.md)</br>
 [Text Analytics-productpagina](//go.microsoft.com/fwlink/?LinkID=759712) 

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Sleuteltermen extraheren](text-analytics-how-to-keyword-extraction.md)
