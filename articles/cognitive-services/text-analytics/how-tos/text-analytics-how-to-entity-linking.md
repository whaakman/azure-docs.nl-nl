---
title: Entiteiten koppelen met de Tekstanalyse-API gebruiken
titleSuffix: Azure Cognitive Services
description: Informatie over het identificeren en oplossen van entiteiten met behulp van de Text Analytics REST-API.
services: cognitive-services
author: ashmaka
manager: cgronlun
ms.service: cognitive-services
ms.component: text-analytics
ms.topic: article
ms.date: 09/12/2018
ms.author: ashmaka
ms.openlocfilehash: ad2168806f9ddd124faf66cdb5a0f51ed13dfadc
ms.sourcegitcommit: 616e63d6258f036a2863acd96b73770e35ff54f8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/14/2018
ms.locfileid: "45604736"
---
# <a name="how-to-identify-linked-entities-in-text-analytics-preview"></a>Het identificeren van gekoppelde entiteiten in Text Analytics (Preview)

De [Entity Linking API](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/5ac4251d5b4ccd1554da7634) neemt ongestructureerde tekst en voor elk JSON-document, resulteert in een lijst van disambiguated entiteiten met koppelingen naar meer informatie op het web (Wikipedia en Bing). 

## <a name="entity-linking-vs-named-entity-recognition"></a>Entity Linking vs. Herkenning van benoemde entiteiten

In de verwerking van natuurlijke taal, worden de concepten van entiteiten koppelen en herkenning van benoemde entiteiten (NER) eenvoudig verward. In de preview-versie van de Text Analytics `entities` eindpunt, alleen entiteiten koppelen wordt ondersteund.

Entiteiten koppelen is de mogelijkheid om te bepalen en dubbelzinnigheid van de identiteit van een entiteit in de tekst (bijvoorbeeld bepalen of de "Mars ' wordt gebruikt als de planeet of als de Roman overmacht van war) gevonden. Dit proces vereist de aanwezigheid van een knowledge base die herkend entiteiten zijn gekoppeld: Wikipedia wordt gebruikt als de knowledge base voor de `entities` eindpunt Text Analytics.

### <a name="language-support"></a>Taalondersteuning

Entiteiten koppelen in verschillende talen, moet met behulp van een betreffende knowledge base in elke taal. Voor entiteiten koppelen in Text Analytics, betekent dit dat elke taal die wordt ondersteund door de `entities` eindpunt koppelt aan de bijbehorende Wikipedia corpus in die taal. Omdat de grootte van corpora tussen talen varieert, is het waarschijnlijk dat de entiteit koppelen van de functionaliteit intrekken ook variëren.


## <a name="preparation"></a>Voorbereiding

Moet u de JSON-documenten in deze indeling hebben:-id, tekst, taal

Zie voor ondersteunde talen, [deze lijst](../text-analytics-supported-languages.md).

De documentgrootte van het moet onder maximaal 5000 tekens per document, en u kunt maximaal 1000 hebben items (id's) per verzameling. De verzameling is in de hoofdtekst van de aanvraag ingediend. Het volgende voorbeeld wordt een afbeelding van inhoud die u naar de gekoppelde entiteit-end verzenden kan.

```
{"documents": [{"id": "1",
                "language": "en",
                "text": "I really enjoy the new XBox One S. It has a clean look, it has 4K/HDR resolution and it is affordable."
                },
               {"id": "2",
                "language": "en",
                "text": "The Seattle Seahawks won the Super Bowl in 2014."
                }
               ]
}
```    
    
## <a name="step-1-structure-the-request"></a>Stap 1: Structuur van de aanvraag

Meer informatie over de definitie van de aanvraag kunnen u vinden in [over het aanroepen van de Tekstanalyse-API](text-analytics-how-to-call-api.md). De volgende punten worden aangepast om u te helpen:

+ Maak een **POST** aanvraag. Controleer de API-documentatie voor deze aanvraag: [Entity Linking-API](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/5ac4251d5b4ccd1554da7634)

+ Stel de HTTP-eindpunt voor de extractie van cruciale frasen. Het omvat de `/entities` resource: `https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/entities`

+ Stel een aanvraagheader om op te nemen van de toegangssleutel voor de Text Analytics-bewerkingen. Zie voor meer informatie, [over het zoeken van eindpunten en toegangssleutels](text-analytics-how-to-access-key.md).

+ In de hoofdtekst van de aanvraag, bieden de JSON-documenten-verzameling die u hebt voorbereid voor deze analyse

> [!Tip]
> Gebruik [Postman](text-analytics-how-to-call-api.md) of open de **API testconsole** in de [documentatie](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/5ac4251d5b4ccd1554da7634) structureren van een aanvraag en PLAATST deze in de service.

## <a name="step-2-post-the-request"></a>Stap 2: De aanvraag plaatsen

Analyse wordt uitgevoerd na ontvangst van de aanvraag. De service accepteert maximaal 100 aanvragen per minuut. Elke aanvraag kan maximaal 1 MB zijn.

Intrekken dat de service staatloos is. Er zijn geen gegevens worden opgeslagen in uw account. Resultaten worden onmiddellijk in het antwoord geretourneerd.

## <a name="step-3-view-results"></a>Stap 3: Resultaten weergeven

Alle POST-verzoeken retourneert een JSON-antwoord met de id's ingedeeld en eigenschappen gedetecteerd.

Uitvoer wordt onmiddellijk geretourneerd. U kunt de resultaten naar een toepassing die JSON accepteert streamen of sla de uitvoer naar een bestand op het lokale systeem en vervolgens importeren in een toepassing waarmee u kunt sorteren, zoeken en de gegevens te manipuleren.

Een voorbeeld van de uitvoer voor entiteiten koppelen wordt volgende weergegeven:

```
{
    "documents": [
        {
            "id": "1",
            "entities": [
                {
                    "name": "Xbox One",
                    "matches": [
                        {
                            "text": "XBox One",
                            "offset": 23,
                            "length": 8
                        }
                    ],
                    "wikipediaLanguage": "en",
                    "wikipediaId": "Xbox One",
                    "wikipediaUrl": "https://en.wikipedia.org/wiki/Xbox_One",
                    "bingId": "446bb4df-4999-4243-84c0-74e0f6c60e75"
                },
                {
                    "name": "Ultra-high-definition television",
                    "matches": [
                        {
                            "text": "4K",
                            "offset": 63,
                            "length": 2
                        }
                    ],
                    "wikipediaLanguage": "en",
                    "wikipediaId": "Ultra-high-definition television",
                    "wikipediaUrl": "https://en.wikipedia.org/wiki/Ultra-high-definition_television",
                    "bingId": "7ee02026-b6ec-878b-f4de-f0bc7b0ab8c4"
                }
            ]
        },
        {
            "id": "2",
            "entities": [
                {
                    "name": "2013 Seattle Seahawks season",
                    "matches": [
                        {
                            "text": "Seattle Seahawks",
                            "offset": 4,
                            "length": 16
                        }
                    ],
                    "wikipediaLanguage": "en",
                    "wikipediaId": "2013 Seattle Seahawks season",
                    "wikipediaUrl": "https://en.wikipedia.org/wiki/2013_Seattle_Seahawks_season",
                    "bingId": "eb637865-4722-4eca-be9e-0ac0c376d361"
                }
            ]
        }
    ],
    "errors": []
}
```

Wanneer deze beschikbaar is, bevat het antwoord de Wikipedia-ID, de Wikipedia-URL en de Bing-ID voor elke gedetecteerde entiteit. Deze kunnen worden gebruikt voor het verder verbeteren van uw toepassing met informatie met betrekking tot de gekoppelde entiteit.


## <a name="summary"></a>Samenvatting

In dit artikel hebt u geleerd werkstroom voor entiteiten koppelen met behulp van de Text Analytics in Cognitive Services en concepten. Kortom:

+ [Entity Linking API](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/5ac4251d5b4ccd1554da7634) is beschikbaar voor de geselecteerde talen.
+ JSON-documenten in de aanvraagtekst omvatten een code-id, tekst en taal.
+ POST-aanvraag is een `/entities` eindpunt, met behulp van een persoonlijk [toegang krijgen tot de sleutel en een eindpunt](text-analytics-how-to-access-key.md) die geldig is voor uw abonnement.
+ Antwoorduitvoer, die uit de gekoppelde entiteiten bestaat (met inbegrip van vertrouwen scores verschuivingen en webkoppelingen, voor elk document-ID) kan worden gebruikt in een toepassing

## <a name="see-also"></a>Zie ook 

 [Overzicht van Text Analytics](../overview.md)  
 [Veelgestelde vragen](../text-analytics-resource-faq.md)</br>
 [Text Analytics-productpagina](//go.microsoft.com/fwlink/?LinkID=759712) 

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Tekstanalyse-API](//westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c6)
