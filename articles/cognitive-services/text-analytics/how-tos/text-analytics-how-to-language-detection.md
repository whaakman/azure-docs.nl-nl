---
title: Hoe kan ik taaldetectie in op Text Analytics REST-API (Microsoft cognitieve Services in Azure) | Microsoft Docs
description: Het vaststellen van de taal met de tekst Analytics REST-API in Microsoft cognitieve Services op Azure in deze zelfstudie scenario.
services: cognitive-services
author: HeidiSteen
manager: cgronlun
ms.service: cognitive-services
ms.component: text-analytics
ms.topic: article
ms.date: 3/07/2018
ms.author: heidist
ms.openlocfilehash: f8e2d9a36533c298addcf42d3cb2061e9c2d1ac7
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344690"
---
# <a name="how-to-detect-language-in-text-analytics"></a>Het vaststellen van de taal in Tekstanalyse

De [taal detectie API](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c7) tekst invoer- en voor elk document dat wordt geëvalueerd en retourneert de taal-id's met een score die aangeeft van de sterkte van de analyse. Tekstanalyse herkent maximaal 120 talen.

Deze functie is nuttig voor inhoud opslaat die verzamelen willekeurige tekst waarin de taal is onbekend. U kunt de resultaten van deze analyse om te bepalen welke taal wordt gebruikt in het invoerdocument parseren. Het antwoord geeft ook een score dat overeenkomt met het vertrouwen van het model (een waarde tussen 0 en 1).

## <a name="preparation"></a>Voorbereiding

Moet u de JSON-documenten in deze indeling hebben: id, de tekst

De documentgrootte van het moet onder 5000 tekens per document en u kunt maximaal 1000 hebben items (id's) per verzameling. De verzameling is in de hoofdtekst van de aanvraag verzonden. Hier volgt een voorbeeld van inhoud die u voor de taal wordt gedetecteerd verzenden mogelijk.

   ```
    {
        "documents": [
            {
                "id": "1",
                "text": "This document is in English."
            },
            {
                "id": "2",
                "text": "Este documento está en inglés."
            },
            {
                "id": "3",
                "text": "Ce document est en anglais."
            },
            {
                "id": "4",
                "text": "本文件为英文"
            },                
            {
                "id": "5",
                "text": "Этот документ находится на английском языке."
            }
        ]
    }
```

## <a name="step-1-structure-the-request"></a>Stap 1: De aanvraag structureren

Meer informatie over de definitie van de aanvraag kunnen worden gevonden in [hoe de tekst Analytics-API aan te roepen](text-analytics-how-to-call-api.md). De volgende punten worden aangepast voor het gemak:

+ Maak een **POST** aanvraag. Bekijk de API-documentatie voor deze aanvraag: [taal Detection-API](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c7)

+ Het HTTP-eindpunt voor de taaldetectie instellen. Het omvat de `/languages` resource: `https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/languages`

+ Stel een aanvraagheader om op te nemen van de toegangssleutel voor Text Analytics-bewerkingen. Zie voor meer informatie [eindpunten zoeken en toegangssleutels](text-analytics-how-to-access-key.md).

+ Geef de JSON-documenten verzameling die u hebt voorbereid voor deze analyse in de aanvraagtekst.

> [!Tip]
> Gebruik [Postman](text-analytics-how-to-call-api.md) of open de **API testen console** in de [documentatie](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c7) aan de structuur van een aanvraag en dit bericht naar de service.

## <a name="step-2-post-the-request"></a>Stap 2: De aanvraag posten

Analyse wordt na ontvangst van de aanvraag uitgevoerd. De service accepteert maximaal 100 aanvragen per minuut. Elke aanvraag mag maximaal 1 MB.

Intrekken van de service staatloze is. Er zijn geen gegevens worden opgeslagen in uw account. Resultaten worden direct in het antwoord geretourneerd.


## <a name="step-3-view-results"></a>Stap 3: Resultaten weergeven

Alle POST-verzoeken retourneren een JSON-indeling van antwoord met de id's en eigenschappen gedetecteerd.

Uitvoer wordt onmiddellijk geretourneerd. U kunt de resultaten naar een toepassing die JSON accepteert stream of sla de uitvoer naar een bestand op het lokale systeem en vervolgens importeren in een toepassing waarmee u kunt sorteren en manipuleren van de gegevens zoeken.

Resultaten voor de voorbeeldaanvraag moeten eruitzien als in de volgende JSON. U ziet dat het een document met meerdere items is. Uitvoer is in het Engels. Taal-id's zijn onder andere een beschrijvende naam en een taalcode in [ISO 639 1](https://www.iso.org/standard/22109.html) indeling.

Het hoogst mogelijke vertrouwensniveau van de analyse wordt uitgedrukt in een positieve score van 1.0.



```
{
    "documents": [
        {
            "id": "1",
            "detectedLanguages": [
                {
                    "name": "English",
                    "iso6391Name": "en",
                    "score": 1
                }
            ]
        },
        {
            "id": "2",
            "detectedLanguages": [
                {
                    "name": "Spanish",
                    "iso6391Name": "es",
                    "score": 1
                }
            ]
        },
        {
            "id": "3",
            "detectedLanguages": [
                {
                    "name": "French",
                    "iso6391Name": "fr",
                    "score": 1
                }
            ]
        },
        {
            "id": "4",
            "detectedLanguages": [
                {
                    "name": "Chinese_Simplified",
                    "iso6391Name": "zh_chs",
                    "score": 1
                }
            ]
        },
        {
            "id": "5",
            "detectedLanguages": [
                {
                    "name": "Russian",
                    "iso6391Name": "ru",
                    "score": 1
                }
            ]
        }
    ],
```

### <a name="ambiguous-content"></a>Niet-eenduidige inhoud

Als de analyzer kan de invoer niet parseren (bijvoorbeeld, wordt ervan uitgegaan dat u een bestaande uitsluitend uit cijfers tekstblok verzonden), wordt hiermee `(Unknown)`.

```
    {
      "id": "5",
      "detectedLanguages": [
        {
          "name": "(Unknown)",
          "iso6391Name": "(Unknown)",
          "score": "NaN"
        }
      ]
```
### <a name="mixed-language-content"></a>Gemengde inhoud taal

Taal van gemengde inhoud binnen hetzelfde document retourneert de taal met de grootste weergave in de inhoud, maar met een lagere positief classificatie weerspiegeling van de marginale robuuste van die beoordeling. Invoer is in het volgende voorbeeld wordt een combinatie van Engels, Spaans en Frans. De analyzer telt tekens in elk segment om te bepalen van de belangrijkste taal.

**Invoer**

```
{
  "documents": [
    {
      "id": "1",
      "text": "Hello, I would like to take a class at your University. ¿Se ofrecen clases en español? Es mi primera lengua y más fácil para escribir. Que diriez-vous des cours en français?"
    }
  ]
}
```

**Uitvoer**

Er bestaat een resulterende uitvoer van de belangrijkste taal, met een score van minder dan 1.0, die wijzen op een zwakkere mate van betrouwbaarheid.

```
{
  "documents": [
    {
      "id": "1",
      "detectedLanguages": [
        {
          "name": "Spanish",
          "iso6391Name": "es",
          "score": 0.9375
        }
      ]
    }
  ],
  "errors": []
}
```

## <a name="summary"></a>Samenvatting

In dit artikel hebt u geleerd concepten en werkstroom voor de taaldetectie Text Analytics gebruiken in cognitieve Services. Hieronder vindt u een snelle herinnering van de belangrijkste punten eerder beschreven en gedemonstreerd:

+ [Taaldetectie API](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c7) is 120 talen beschikbaar.
+ JSON-documenten in de aanvraagtekst bevatten een id en de tekst.
+ POST-aanvraag is een `/languages` eindpunt, met een persoonlijk [toegang krijgen tot sleutel en een eindpunt](text-analytics-how-to-access-key.md) die geldig is voor uw abonnement.
+ Antwoorduitvoer, die uit de taal-id's voor elk document-ID bestaat, kan worden gestreamd naar alle Apps die JSON accepteert, zoals Excel en Power BI, enzovoort.

## <a name="see-also"></a>Zie ook 

 [Overzicht van tekst Analytics](../overview.md)  
 [Veelgestelde vragen (FAQ)](../text-analytics-resource-faq.md)</br>
 [Tekst Analytics productpagina](//go.microsoft.com/fwlink/?LinkID=759712) 

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Gevoel analyseren](text-analytics-how-to-sentiment-analysis.md)
