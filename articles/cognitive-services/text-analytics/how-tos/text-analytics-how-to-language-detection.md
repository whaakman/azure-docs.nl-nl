---
title: Taal detecteren met de Text Analytics REST API | Microsoft Docs
description: Taal detecteren met de Text Analytics REST API van Azure Cognitive Services.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: sample
ms.date: 02/13/2019
ms.author: aahi
ms.openlocfilehash: 8197d091763709282c42379a7ca0ea802e5c6fdf
ms.sourcegitcommit: 24906eb0a6621dfa470cb052a800c4d4fae02787
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/27/2019
ms.locfileid: "56886768"
---
# <a name="example-how-to-detect-language-with-text-analytics"></a>Voorbeeld: Taal detecteren met Text Analytics

De [Taaldetectie-API](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c7) evalueert tekstinvoer voor elk document en retourneert de taal-id's met een score die wijst op de sterkte van de analyse. Text Analytics herkent maximaal 120 talen.

Deze mogelijkheid is handig voor inhoudsarchieven die willekeurige tekst verzamelen, waarin de taal onbekend is. U kunt de resultaten van deze analyse parseren om te bepalen welke taal wordt gebruikt in het ingevoerde document. Het antwoord retourneert ook een score die overeenkomt met het vertrouwen van het model (een waarde tussen 0 en 1).

> [!TIP]
> Text Analytics biedt ook een Docker-containerinstallatiekopie op basis van Linux voor taaldetectie. U kunt de [Text Analytics-container dus dicht bij uw gegevens installeren en uitvoeren](text-analytics-how-to-install-containers.md).

## <a name="preparation"></a>Voorbereiding

U moet JSON-documenten in deze indeling hebben: id, tekst

De documentgrootte moet minder dan 5120 tekens per document bevatten en u kunt maximaal 1000 items (id's) per verzameling hebben. De verzameling is in de hoofdtekst van de aanvraag ingediend. Hier volgt een voorbeeld van de inhoud die u voor taaldetectie kan indienen.

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
                "text": "Этот документ на английском языке."
            }
        ]
    }
```

## <a name="step-1-structure-the-request"></a>Stap 1: Structureer de aanvraag

Meer informatie over de definitie van de aanvraag kunt u vinden in [De Text Analytics-API aanroepen](text-analytics-how-to-call-api.md). De volgende punten zijn voor uw gemak opnieuw geformuleerd:

+ Maak een **POST**-aanvraag. Bekijk de API-documentatie voor deze aanvraag: [API voor taaldetectie](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c7)

+ Stel het HTTP-eindpunt voor taaldetectie in, met behulp van een Text Analytics-resource in Azure of een geïnstantieerde [Text Analytics-container](text-analytics-how-to-install-containers.md). Deze moet de `/languages`-resource: `https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/languages` bevatten

+ Stel een aanvraagheader in om de toegangssleutel voor de Text Analytics-bewerkingen op te nemen. Zie voor meer informatie [Eindpunten en toegangssleutels zoeken](text-analytics-how-to-access-key.md).

+ Verstrek in de hoofdtekst van de aanvraag de JSON-documentenverzameling die u hebt voorbereid voor deze analyse

> [!Tip]
> Gebruik [Postman](text-analytics-how-to-call-api.md) of open de **API-testconsole** in de [documentatie](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c7) om de aanvraag te structureren en POST deze in de service.

## <a name="step-2-post-the-request"></a>Stap 2: Plaats de aanvraag

Analyse wordt uitgevoerd na ontvangst van de aanvraag. De service accepteert maximaal 100 aanvragen per minuut. Elke aanvraag kan maximaal 1 MB zijn.

Terughalen als de service staatloos is. Er worden geen gegevens opgeslagen in uw account. Resultaten worden onmiddellijk in het antwoord geretourneerd.


## <a name="step-3-view-results"></a>Stap 3: Resultaten weergeven

Alle POST-verzoeken retourneren een ingedeeld JSON-antwoord met de id's en gedetecteerde eigenschappen.

Uitvoer wordt onmiddellijk geretourneerd. U kunt de resultaten streamen naar een toepassing die JSON accepteert of u kunt de uitvoer opslaan als lokaal bestand en vervolgens importeren in een toepassing waarmee u kunt sorteren, zoeken en de gegevens kunt manipuleren.

Resultaten voor de voorbeeldaanvraag moeten eruitzien als de volgende JSON. Merk op dat er één document met meerdere items is. Uitvoer is in het Engels. Taal-id's zijn onder andere een beschrijvende naam en een taalcode in [ISO 639-1](https://www.iso.org/standard/22109.html) indeling.

Een positief score van 1.0 staat voor het hoogst mogelijke vertrouwensniveau van de analyse.



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

Als de analyzer de invoer niet kan parseren (bijvoorbeeld, veronderstel dat u een tekstblok hebt ingediend, dat uitsluitend bestaat uit cijfers verzonden), het retourneert het resultaat`(Unknown)`.

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
### <a name="mixed-language-content"></a>Gemengde talen inhoud

Gemengde talen inhoud binnen hetzelfde document retourneert de taal met de grootste weergave in de inhoud, maar met een lagere positieve classificatie, die de marginale sterkte van deze evaluatie weergeeft. De invoer in het volgende voorbeeld is een combinatie van Engels, Spaans en Frans. De analyzer telt tekens in elk segment om te bepalen van de overheersende taal.

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

Resulterende uitvoer bestaat uit de overheersende taal, met een score van minder dan 1,0 die wijst op een zwakkere mate van betrouwbaarheid.

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

In dit artikel hebt u concepten en werkstroom geleerd voor taaldetectie met behulp van de Text Analytics in Cognitive Services. Hier volgt een snelle herinnering van de belangrijkste punten eerder uitgelegd en gedemonstreerd:

+ [Taaldetectie API](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c7) is beschikbaar in 120 talen.
+ JSON-documenten in de aanvraagbody omvatten een id en tekst.
+ POST-aanvraag is een `/languages`-eindpunt die een persoonlijke [toegangssleutel en een eindpunt](text-analytics-how-to-access-key.md) gebruikt die geldig zijn voor uw abonnement.
+ Antwoorduitvoer, die uit de taal-id's voor elk document-ID bestaat, kan worden gestreamd naar alle apps die JSON accepteert, met inbegrip van Excel en Power BI om er maar een paar te noemen.

## <a name="see-also"></a>Zie ook 

 [Overzicht van Text Analytics](../overview.md)  
 [Veelgestelde vragen](../text-analytics-resource-faq.md)</br>
 [Text Analytics-productpagina](//go.microsoft.com/fwlink/?LinkID=759712) 

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Gevoel analyseren](text-analytics-how-to-sentiment-analysis.md)
