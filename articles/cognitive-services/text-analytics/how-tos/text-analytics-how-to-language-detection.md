---
title: Taal met de Text Analytics REST-API detecteren | Microsoft Docs
description: Taal detecteren met behulp van de Text Analytics REST-API van Azure Cognitive Services.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: sample
ms.date: 02/26/2019
ms.author: aahi
ms.openlocfilehash: e1adeb34cf999f471bb183e4d7de9c65427252bb
ms.sourcegitcommit: 6b41522dae07961f141b0a6a5d46fd1a0c43e6b2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/15/2019
ms.locfileid: "67986512"
---
# <a name="example-detect-language-with-text-analytics"></a>Voorbeeld: Taal met Tekstanalyse detecteren

De [taaldetectie](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c7) functie van de REST-API van Azure Text Analytics tekstinvoer voor elk document evalueert en retourneert de taal-id's met een score die aangeeft van de kracht van de analyse.

Deze mogelijkheid is handig voor inhoudsarchieven die willekeurige tekst verzamelen, waarin de taal onbekend is. U kunt de resultaten van deze analyse parseren om te bepalen welke taal wordt gebruikt in het ingevoerde document. Het antwoord retourneert ook een score die overeenkomt met het vertrouwen van het model. De score-waarde ligt tussen 0 en 1.

De functie taaldetectie kan een groot aantal talen, varianten dialecten en sommige talen regionale of culturele detecteren. De exacte lijst met talen voor deze functie wordt niet gepubliceerd.

Als u inhoud, uitgedrukt in een minder vaak gebruikte taal hebt, kunt u de functie taaldetectie om te zien als het resultaat een code proberen. Het antwoord voor talen die niet kunnen worden gedetecteerd, is `unknown`.

> [!TIP]
> Text Analytics biedt ook een Docker-containerinstallatiekopie op basis van Linux voor taaldetectie. U kunt de [Text Analytics-container dus dicht bij uw gegevens installeren en uitvoeren](text-analytics-how-to-install-containers.md).

## <a name="preparation"></a>Voorbereiding

U moet de JSON-documenten in deze indeling hebben: ID en de tekst.

Grootte van het document moet onder 5.120 tekens per document. U kunt maximaal 1000 hebben items (id's) per verzameling. De verzameling is in de hoofdtekst van de aanvraag ingediend. Het volgende voorbeeld wordt een voorbeeld van de inhoud die u voor taaldetectie indienen mogelijk:

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

Zie voor meer informatie over de definitie van aanvraag [de Tekstanalyse-API aanroepen](text-analytics-how-to-call-api.md). De volgende punten zijn voor uw gemak opnieuw geformuleerd:

+ Maak een POST-aanvraag. De API-documentatie voor deze aanvraag, Zie de [Language Detection API](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c7).

+ Het HTTP-eindpunt voor taaldetectie instellen. Een van beide een Text Analytics-resource gebruiken op Azure of een geïnstantieerde [Text Analytics-container](text-analytics-how-to-install-containers.md). Het omvat de `/languages` resource: `https://westus.api.cognitive.microsoft.com/text/analytics/v2.1/languages`.

+ Stel een aanvraagheader in om de toegangssleutel voor de Text Analytics-bewerkingen op te nemen. Zie voor meer informatie, [eindpunten vinden en toegangssleutels](text-analytics-how-to-access-key.md).

+ Verstrek in de hoofdtekst van de aanvraag de JSON-documentenverzameling die u hebt voorbereid voor deze analyse.

> [!Tip]
> Gebruik [Postman](text-analytics-how-to-call-api.md) of open de **API-testconsole** in de [documentatie](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c7) om de aanvraag te structureren en POST deze in de service.

## <a name="step-2-post-the-request"></a>Stap 2: NA de aanvraag

Analyse wordt uitgevoerd na ontvangst van de aanvraag. Voor meer informatie over de grootte en het aantal aanvragen die u kunt verzenden per minuut en seconde, Zie de [gegevenslimieten](../overview.md#data-limits) sectie in het overzicht.

Terughalen als de service staatloos is. Er worden geen gegevens opgeslagen in uw account. Resultaten worden onmiddellijk in het antwoord geretourneerd.


## <a name="step-3-view-the-results"></a>Stap 3: De resultaten bekijken

Alle POST-verzoeken een JSON-indeling-antwoord met de id's retourneren en eigenschappen gedetecteerd.

Uitvoer wordt onmiddellijk geretourneerd. U kunt de resultaten naar een toepassing die JSON accepteert streamen of sla de uitvoer naar een bestand op het lokale systeem. Importeer de uitvoer in een toepassing die u gebruiken kunt om te sorteren, zoeken en de gegevens te manipuleren.

Resultaten voor de voorbeeldaanvraag moeten eruitzien als de volgende JSON. Merk op dat er één document met meerdere items. Uitvoer is in het Engels. Taal-id's zijn onder andere een beschrijvende naam en een taalcode in [ISO 639-1](https://www.iso.org/standard/22109.html) indeling.

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

Als de analyzer de invoer niet parseren kan, die het resultaat `(Unknown)`. Een voorbeeld is als u een tekstblok, die uitsluitend uit cijfers bestaat indienen.

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
### <a name="mixed-language-content"></a>Verschillende talen inhoud

Verschillende talen inhoud binnen hetzelfde document retourneert de taal met de grootste weergave in de inhoud, maar met een lagere classificatie van positief. De classificatie is inclusief de marginale sterkte van de evaluatie. De invoer in het volgende voorbeeld is een combinatie van Engels, Spaans en Frans. De analyzer telt tekens in elk segment om te bepalen van de overheersende taal.

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

De resulterende uitvoer bestaat uit de overheersende taal, met een score van minder dan 1,0, waarmee wordt aangegeven een zwakkere mate van betrouwbaarheid.

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

In dit artikel hebt u geleerd werkstroom voor de taaldetectie en concepten met behulp van de Text Analytics in Azure Cognitive Services. De volgende punten zijn uitgelegd en gedemonstreerd:

+ [Taaldetectie](https://westcentralus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c7) is beschikbaar voor een breed scala aan talen, varianten dialecten en sommige talen regionale of culturele.
+ JSON-documenten in de hoofdtekst van de aanvraag bevatten een ID en de tekst.
+ De POST-aanvraag is een `/languages` eindpunt met behulp van een persoonlijk [toegang krijgen tot de sleutel en een eindpunt](text-analytics-how-to-access-key.md) die geldig is voor uw abonnement.
+ Antwoorduitvoer bestaat uit de taal-id's voor elk document-ID. De uitvoer kan worden gestreamd naar alle Apps die JSON accepteert. Voorbeeld-apps zijn Excel en Power BI, als u wilt een paar te noemen.

## <a name="see-also"></a>Zie ook 

 [Overzicht van Text Analytics](../overview.md)  
 [Veelgestelde vragen](../text-analytics-resource-faq.md)</br>
 [Text Analytics-productpagina](//go.microsoft.com/fwlink/?LinkID=759712) 

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Gevoel analyseren](text-analytics-how-to-sentiment-analysis.md)
