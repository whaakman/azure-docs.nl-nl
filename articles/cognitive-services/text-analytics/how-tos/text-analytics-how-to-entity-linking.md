---
title: Instructies gebruiken entiteit koppelen in Text Analytics REST-API (Microsoft cognitieve Services in Azure) | Microsoft Docs
description: Informatie over het identificeren en oplossen van entiteiten met de tekst Analytics REST-API in Microsoft cognitieve Services op Azure in deze zelfstudie scenario.
services: cognitive-services
author: ashmaka
manager: cgronlun
ms.service: cognitive-services
ms.technology: text-analytics
ms.topic: article
ms.date: 5/02/2018
ms.author: ashmaka
ms.openlocfilehash: 55bec1a0223b70749a97a30e2da92ef15128038c
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344851"
---
# <a name="how-to-identify-linked-entities-in-text-analytics-preview"></a>Het identificeren van de gekoppelde entiteiten in Text Analytics (Preview)

De [entiteit koppelen API](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/5ac4251d5b4ccd1554da7634) ongestructureerde tekst neemt, en voor elk JSON-document, retourneert u een lijst met entiteiten ondubbelzinnig gemaakt met koppelingen naar meer informatie op het web (Wikipedia (Engelstalig) en Bing). 

## <a name="entity-linking-vs-named-entity-recognition"></a>Entiteit Linking vs. Herkenning van benoemde entiteiten

Bij het verwerken van natuurlijke taal, kunnen u gemakkelijk de concepten van entiteit koppelen en benoemde entiteit herkenning (NER) verward. In de preview-versie van Text-Analytics `entities` eindpunt, alleen Entiteitstypen koppelen wordt ondersteund.

Entiteit koppelen is de mogelijkheid om te bepalen en op te heffen de identiteit van een entiteit die is gevonden in tekst (bijvoorbeeld bepalen of de ' Mars ' wordt gebruikt als wereld of als de Romeinse god van war). Dit proces vereist de aanwezigheid van een knowledge base die herkend entiteiten zijn gekoppeld - Wikipedia (Engelstalig) wordt gebruikt als de knowledge base voor de `entities` eindpunt Text Analytics.

### <a name="language-support"></a>Taalondersteuning

Entiteit koppelen in diverse talen, moet een bijbehorende knowledge base gebruiken in elke taal. Voor de entiteit koppelen in Text Analytics, betekent dit dat elke taal die wordt ondersteund door de `entities` eindpunt een koppeling naar de bijbehorende corpus Wikipedia (Engelstalig) in die taal. Omdat de grootte van vennootschappen tussen talen varieert, wordt verwacht dat de entiteit van de functionaliteit intrekken koppelen ook variëren.


## <a name="preparation"></a>Voorbereiding

Moet u de JSON-documenten in deze indeling hebben: id, de tekst, de taal

Zie voor ondersteunde talen [deze lijst](../text-analytics-supported-languages.md).

De documentgrootte van het moet onder 5000 tekens per document en u kunt maximaal 1000 hebben items (id's) per verzameling. De verzameling is in de hoofdtekst van de aanvraag verzonden. Het volgende voorbeeld wordt een afbeelding van inhoud die u aanvragen bij het koppelen einde van de entiteit indienen mogelijk.

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
    
## <a name="step-1-structure-the-request"></a>Stap 1: De aanvraag structureren

Meer informatie over de definitie van de aanvraag kunnen worden gevonden in [hoe de tekst Analytics-API aan te roepen](text-analytics-how-to-call-api.md). De volgende punten worden aangepast voor het gemak:

+ Maak een **POST** aanvraag. Bekijk de API-documentatie voor deze aanvraag: [entiteit koppelen API](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/5ac4251d5b4ccd1554da7634)

+ Het HTTP-eindpunt voor het ophalen van de belangrijkste woordgroep ingesteld. Het omvat de `/entities` resource: `https://westus.api.cognitive.microsoft.com/text/analytics/v2.0/entities`

+ Stel een aanvraagheader om op te nemen van de toegangssleutel voor Text Analytics-bewerkingen. Zie voor meer informatie [eindpunten zoeken en toegangssleutels](text-analytics-how-to-access-key.md).

+ Geef de JSON-documenten verzameling die u hebt voorbereid voor deze analyse in de aanvraagtekst.

> [!Tip]
> Gebruik [Postman](text-analytics-how-to-call-api.md) of open de **API testen console** in de [documentatie](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/5ac4251d5b4ccd1554da7634) aan de structuur van een aanvraag en dit bericht naar de service.

## <a name="step-2-post-the-request"></a>Stap 2: De aanvraag posten

Analyse wordt na ontvangst van de aanvraag uitgevoerd. De service accepteert maximaal 100 aanvragen per minuut. Elke aanvraag mag maximaal 1 MB.

Intrekken van de service staatloze is. Er zijn geen gegevens worden opgeslagen in uw account. Resultaten worden direct in het antwoord geretourneerd.

## <a name="step-3-view-results"></a>Stap 3: Resultaten weergeven

Alle POST-verzoeken retourneren een JSON-indeling van antwoord met de id's en eigenschappen gedetecteerd.

Uitvoer wordt onmiddellijk geretourneerd. U kunt de resultaten naar een toepassing die JSON accepteert stream of sla de uitvoer naar een bestand op het lokale systeem en vervolgens importeren in een toepassing waarmee u kunt sorteren en manipuleren van de gegevens zoeken.

Een voorbeeld van de uitvoer voor entiteit koppelen wordt volgende weergegeven:

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

Indien beschikbaar, bevat het antwoord de Wikipedia (Engelstalig)-ID, de URL van de Wikipedia (Engelstalig) en de Bing-ID voor elke gedetecteerde entiteit. Deze kunnen worden gebruikt voor het verder verbeteren van uw toepassing met informatie over de gekoppelde entiteit.


## <a name="summary"></a>Samenvatting

In dit artikel hebt u geleerd concepten en werkstroom voor het koppelen van entiteit Text Analytics gebruiken in cognitieve Services. Samengevat:

+ [Entiteit koppelen API](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/5ac4251d5b4ccd1554da7634) is beschikbaar voor de geselecteerde talen.
+ JSON-documenten in de aanvraagtekst bevatten een code-id, tekst en taal.
+ POST-aanvraag is een `/entities` eindpunt, met een persoonlijk [toegang krijgen tot sleutel en een eindpunt](text-analytics-how-to-access-key.md) die geldig is voor uw abonnement.
+ Antwoorduitvoer, die uit de gekoppelde entiteiten bestaat (inclusief vertrouwen scores verschuivingen en webkoppelingen, voor elk document-ID) kan in elke toepassing worden gebruikt

## <a name="see-also"></a>Zie ook 

 [Overzicht van tekst Analytics](../overview.md)  
 [Veelgestelde vragen (FAQ)](../text-analytics-resource-faq.md)</br>
 [Tekst Analytics productpagina](//go.microsoft.com/fwlink/?LinkID=759712) 

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Tekstanalyse API](//westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c6)
