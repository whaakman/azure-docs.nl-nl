---
title: Herkenning van entiteit met de Tekstanalyse-API gebruiken
titleSuffix: Azure Cognitive Services
description: Leer hoe u voor het herkennen van entiteiten met behulp van de Text Analytics REST-API.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: article
ms.date: 02/13/2019
ms.author: aahi
ms.openlocfilehash: b2330d322c6939ba6d9581c125c512fcea9f924b
ms.sourcegitcommit: b3d74ce0a4acea922eadd96abfb7710ae79356e0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/14/2019
ms.locfileid: "56242744"
---
# <a name="how-to-use-named-entity-recognition-in-text-analytics-preview"></a>Herkenning van entiteit met de naam gebruiken in Text Analytics (Preview)

De [entiteit Recognition-API](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-V2-1-Preview/operations/5ac4251d5b4ccd1554da7634) neemt ongestructureerde tekst en voor elk JSON-document, resulteert in een lijst van disambiguated entiteiten met koppelingen naar meer informatie op het web (Wikipedia en Bing). 

## <a name="entity-linking-and-named-entity-recognition"></a>Entiteiten koppelen en herkenning van benoemde entiteiten

De Text Analytics `entities` eindpunt supprts, beide met de naam van entiteit recognition (NER) en het koppelen van de entiteit.

### <a name="entity-linking"></a>Entiteiten koppelen
Entiteiten koppelen, is de mogelijkheid om te bepalen en dubbelzinnigheid van de identiteit van een entiteit in de tekst (bijvoorbeeld, waarmee wordt bepaald of de "Mars ' wordt gebruikt als de planeet of als de Roman overmacht van war) gevonden. Dit proces vereist de aanwezigheid van een knowledge base die herkend entiteiten zijn gekoppeld: Wikipedia wordt gebruikt als de knowledge base voor de `entities` eindpunt Text Analytics.

In de Text Analytics [versie 2.0](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/5ac4251d5b4ccd1554da7634)alleen entiteiten koppelen is beschikbaar.

### <a name="named-entity-recognition-ner"></a>Herkenning van benoemde entiteiten (NER)
Met de naam van entiteit recognition is (NER) de mogelijkheid om te bepalen van de verschillende entiteiten in de tekst en verdeel ze in categorieën in de vooraf gedefinieerde klassen. De ondersteunde klassen van entiteiten worden hieronder vermeld.

In de Text Analytics [versie 2.1-Preview](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-V2-1-Preview/operations/5ac4251d5b4ccd1554da7634), entiteiten koppelen en herkenning van benoemde entiteiten (NER) beschikbaar zijn.

### <a name="language-support"></a>Taalondersteuning

Entiteiten koppelen in verschillende talen, moet met behulp van een betreffende knowledge base in elke taal. Voor entiteiten koppelen in Text Analytics, betekent dit dat elke taal die wordt ondersteund door de `entities` eindpunt koppelt aan de bijbehorende Wikipedia corpus in die taal. Omdat de grootte van corpora tussen talen varieert, is het waarschijnlijk dat de entiteit koppelen van de functionaliteit intrekken ook variëren.

## <a name="supported-types-for-named-entity-recognition"></a>Ondersteunde typen voor herkenning van benoemde entiteiten

| Type  | SubType | Voorbeeld |
|:-----------   |:------------- |:---------|
| Person        | N.V.T.\*         | "Jeff", "Bill Gates"     |
| Locatie      | N.V.T.\*         | "Redmond, Washington", "Paris"  |
| Organisatie  | N.V.T.\*         | "Microsoft"   |
| Hoeveelheid      | Aantal        | "6", "six"     | 
| Hoeveelheid      | Percentage    | "50%", "50%"| 
| Hoeveelheid      | Volgnummer       | "2", "tweede"     | 
| Hoeveelheid      | NumberRange   | "4-8"     | 
| Hoeveelheid      | Leeftijd           | "90 dagen oud is", "30 jaar oude"    | 
| Hoeveelheid      | Valuta      | "$10.99"     | 
| Hoeveelheid      | Dimensie     | "10 mijl", "40 cm"     | 
| Hoeveelheid      | Temperatuur   | "32 degrees"    |
| DateTime      | N.V.T.\*         | "6:30 PM 4 februari 2012"      | 
| DateTime      | Date          | "Mei 2e 2017", "05/02/2017"   | 
| Datum en tijd     | Time          | "8 am", "8:00"  | 
| DateTime      | DateRange     | "Mei 2e op 5 mei"    | 
| DateTime      | TimeRange     | "18: 00 uur tot 19: 00"     | 
| DateTime      | Duur      | '1 minuut en 45 seconden'   | 
| DateTime      | Instellen           | "elke dinsdag"     | 
| DateTime      | TimeZone      |    | 
| URL           | N.V.T.\*         | "http://www.bing.com"    |
| Email         | N.V.T.\*         | "support@contoso.com" |
\* Afhankelijk van de invoer- en uitgepakte entiteiten, bepaalde entiteiten kunnen laat de `SubType`.



## <a name="preparation"></a>Voorbereiding

U moet JSON-documenten in deze indeling hebben: id, tekst, taal

Zie voor ondersteunde talen, [deze lijst](../text-analytics-supported-languages.md).

De documentgrootte moet onder maximaal 5000 tekens per document zijn, en u kunt maximaal 1000 items (id's) per verzameling hebben. De verzameling is in de hoofdtekst van de aanvraag ingediend. Het volgende voorbeeld wordt een afbeelding van inhoud die u naar de gekoppelde entiteit-end verzenden kan.

```
{"documents": [{"id": "1",
                "language": "en",
                "text": "Jeff bought three dozen eggs because there was a 50% discount."
                },
               {"id": "2",
                "language": "en",
                "text": "The Great Depression began in 1929. By 1933, the GDP in America fell by 25%."
                }
               ]
}
```    
    
## <a name="step-1-structure-the-request"></a>Stap 1: Structureer de aanvraag

Meer informatie over de definitie van de aanvraag kunt u vinden in [De Text Analytics-API aanroepen](text-analytics-how-to-call-api.md). De volgende punten zijn voor uw gemak opnieuw geformuleerd:

+ Maak een **POST**-aanvraag. Bekijk de API-documentatie voor deze aanvraag: [Entity Linking API](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/5ac4251d5b4ccd1554da7634)

+ Stel het HTTP-eindpunt voor het ophalen van de entiteit. Deze moet de `/entities`-resource: `https://[your-region].api.cognitive.microsoft.com/text/analytics/v2.1-preview/entities` bevatten

+ Stel een aanvraagheader in om de toegangssleutel voor de Text Analytics-bewerkingen op te nemen. Zie voor meer informatie [Eindpunten en toegangssleutels zoeken](text-analytics-how-to-access-key.md).

+ Verstrek in de hoofdtekst van de aanvraag de JSON-documentenverzameling die u hebt voorbereid voor deze analyse

> [!Tip]
> Gebruik [Postman](text-analytics-how-to-call-api.md) of open de **API-testconsole** in de [documentatie](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-V2-1-Preview/operations/5ac4251d5b4ccd1554da7634) om de aanvraag te structureren en POST deze in de service.

## <a name="step-2-post-the-request"></a>Stap 2: Plaats de aanvraag

Analyse wordt uitgevoerd na ontvangst van de aanvraag. De service accepteert maximaal 100 aanvragen per minuut. Elke aanvraag kan maximaal 1 MB zijn.

Terughalen als de service staatloos is. Er worden geen gegevens opgeslagen in uw account. Resultaten worden onmiddellijk in het antwoord geretourneerd.

## <a name="step-3-view-results"></a>Stap 3: Resultaten weergeven

Alle POST-verzoeken retourneren een ingedeeld JSON-antwoord met de id's en gedetecteerde eigenschappen.

Uitvoer wordt onmiddellijk geretourneerd. U kunt de resultaten streamen naar een toepassing die JSON accepteert of u kunt de uitvoer opslaan als lokaal bestand en vervolgens importeren in een toepassing waarmee u kunt sorteren, zoeken en de gegevens kunt manipuleren.

Een voorbeeld van de uitvoer voor entiteiten koppelen wordt volgende weergegeven:

```json
{
    "Documents": [
        {
            "Id": "1",
            "Entities": [
                {
                    "Name": "Jeff",
                    "Matches": [
                        {
                            "Text": "Jeff",
                            "Offset": 0,
                            "Length": 4
                        }
                    ],
                    "Type": "Person"
                },
                {
                    "Name": "three dozen",
                    "Matches": [
                        {
                            "Text": "three dozen",
                            "Offset": 12,
                            "Length": 11
                        }
                    ],
                    "Type": "Quantity",
                    "SubType": "Number"
                },
                {
                    "Name": "50",
                    "Matches": [
                        {
                            "Text": "50",
                            "Offset": 49,
                            "Length": 2
                        }
                    ],
                    "Type": "Quantity",
                    "SubType": "Number"
                },
                {
                    "Name": "50%",
                    "Matches": [
                        {
                            "Text": "50%",
                            "Offset": 49,
                            "Length": 3
                        }
                    ],
                    "Type": "Quantity",
                    "SubType": "Percentage"
                }
            ]
        },
        {
            "Id": "2",
            "Entities": [
                {
                    "Name": "Great Depression",
                    "Matches": [
                        {
                            "Text": "The Great Depression",
                            "Offset": 0,
                            "Length": 20
                        }
                    ],
                    "WikipediaLanguage": "en",
                    "WikipediaId": "Great Depression",
                    "WikipediaUrl": "https://en.wikipedia.org/wiki/Great_Depression",
                    "BingId": "d9364681-98ad-1a66-f869-a3f1c8ae8ef8"
                },
                {
                    "Name": "1929",
                    "Matches": [
                        {
                            "Text": "1929",
                            "Offset": 30,
                            "Length": 4
                        }
                    ],
                    "Type": "DateTime",
                    "SubType": "DateRange"
                },
                {
                    "Name": "By 1933",
                    "Matches": [
                        {
                            "Text": "By 1933",
                            "Offset": 36,
                            "Length": 7
                        }
                    ],
                    "Type": "DateTime",
                    "SubType": "DateRange"
                },
                {
                    "Name": "Gross domestic product",
                    "Matches": [
                        {
                            "Text": "GDP",
                            "Offset": 49,
                            "Length": 3
                        }
                    ],
                    "WikipediaLanguage": "en",
                    "WikipediaId": "Gross domestic product",
                    "WikipediaUrl": "https://en.wikipedia.org/wiki/Gross_domestic_product",
                    "BingId": "c859ed84-c0dd-e18f-394a-530cae5468a2"
                },
                {
                    "Name": "United States",
                    "Matches": [
                        {
                            "Text": "America",
                            "Offset": 56,
                            "Length": 7
                        }
                    ],
                    "WikipediaLanguage": "en",
                    "WikipediaId": "United States",
                    "WikipediaUrl": "https://en.wikipedia.org/wiki/United_States",
                    "BingId": "5232ed96-85b1-2edb-12c6-63e6c597a1de",
                    "Type": "Location"
                },
                {
                    "Name": "25",
                    "Matches": [
                        {
                            "Text": "25",
                            "Offset": 72,
                            "Length": 2
                        }
                    ],
                    "Type": "Quantity",
                    "SubType": "Number"
                },
                {
                    "Name": "25%",
                    "Matches": [
                        {
                            "Text": "25%",
                            "Offset": 72,
                            "Length": 3
                        }
                    ],
                    "Type": "Quantity",
                    "SubType": "Percentage"
                }
            ]
        }
    ],
    "Errors": []
}
```


## <a name="summary"></a>Samenvatting

In dit artikel hebt u geleerd werkstroom voor entiteiten koppelen met behulp van de Text Analytics in Cognitive Services en concepten. Samenvatting:

+ [Entiteiten API](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-V2-1-Preview/operations/5ac4251d5b4ccd1554da7634) is beschikbaar voor de geselecteerde talen.
+ JSON-documenten in de aanvraagbody omvatten een id, tekst en taalcode.
+ POST-aanvraag is een `/entities`-eindpunt die een persoonlijke [toegangssleutel en een eindpunt](text-analytics-how-to-access-key.md) gebruikt die geldig zijn voor uw abonnement.
+ Antwoorduitvoer, die uit de gekoppelde entiteiten bestaat (met inbegrip van vertrouwen scores verschuivingen en webkoppelingen, voor elk document-ID) kan worden gebruikt in een toepassing

## <a name="see-also"></a>Zie ook 

 [Overzicht van Text Analytics](../overview.md)  
 [Veelgestelde vragen](../text-analytics-resource-faq.md)</br>
 [Text Analytics-productpagina](//go.microsoft.com/fwlink/?LinkID=759712) 

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Text Analytics-API](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-V2-1-Preview/operations/5ac4251d5b4ccd1554da7634)
