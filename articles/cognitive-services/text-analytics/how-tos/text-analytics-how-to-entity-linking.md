---
title: Herkenning van entiteit met de Tekstanalyse-API gebruiken
titleSuffix: Azure Cognitive Services
description: Leer hoe u voor het herkennen van entiteiten met behulp van de Text Analytics REST-API.
services: cognitive-services
author: ashmaka
manager: cgronlun
ms.service: cognitive-services
ms.component: text-analytics
ms.topic: article
ms.date: 10/01/2018
ms.author: ashmaka
ms.openlocfilehash: b2916e5c414562c55c35c9c5e7ab378963e004be
ms.sourcegitcommit: 609c85e433150e7c27abd3b373d56ee9cf95179a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/03/2018
ms.locfileid: "48248064"
---
# <a name="how-to-use-named-entity-recognition-in-text-analytics-preview"></a>Herkenning van entiteit met de naam gebruiken in Text Analytics (Preview)

De [entiteit Recognition-API](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-V2-1-Preview/operations/5ac4251d5b4ccd1554da7634) neemt ongestructureerde tekst en voor elk JSON-document, resulteert in een lijst van disambiguated entiteiten met koppelingen naar meer informatie op het web (Wikipedia en Bing). 

## <a name="entity-linking-and-named-entity-recognition"></a>Entiteiten koppelen en herkenning van benoemde entiteiten

De Text Analytics `entities` eindpunt supprts, beide met de naam van entiteit recognition (NER) en het koppelen van de entiteit.

### <a name="entity-linking"></a>Entiteiten koppelen
Entiteiten koppelen is de mogelijkheid om te bepalen en dubbelzinnigheid van de identiteit van een entiteit in de tekst (bijvoorbeeld bepalen of de "Mars ' wordt gebruikt als de planeet of als de Roman overmacht van war) gevonden. Dit proces vereist de aanwezigheid van een knowledge base die herkend entiteiten zijn gekoppeld: Wikipedia wordt gebruikt als de knowledge base voor de `entities` eindpunt Text Analytics.

In de Text Analytics [versie 2.1-Preview](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-V2-1-Preview/operations/5ac4251d5b4ccd1554da7634)alleen entiteiten koppelen is beschikbaar.

### <a name="named-entity-recognition-ner"></a>Herkenning van benoemde entiteiten (NER)
Met de naam van entiteit recognition is (NER) de mogelijkheid om te bepalen van de verschillende entiteiten in de tekst en verdeel ze in categorieën in de vooraf gedefinieerde klassen. De ondersteunde klassen van entiteiten worden hieronder vermeld.

Text Analytics versie 2.1 Preview-versie (`https://[region].api.cognitive.microsoft.com/text/analytics/v2.1-preview/entities`), entiteiten koppelen en herkenning van benoemde entiteiten (NER) beschikbaar zijn.

### <a name="language-support"></a>Taalondersteuning

Entiteiten koppelen in verschillende talen, moet met behulp van een betreffende knowledge base in elke taal. Voor entiteiten koppelen in Text Analytics, betekent dit dat elke taal die wordt ondersteund door de `entities` eindpunt koppelt aan de bijbehorende Wikipedia corpus in die taal. Omdat de grootte van corpora tussen talen varieert, is het waarschijnlijk dat de entiteit koppelen van de functionaliteit intrekken ook variëren.

## <a name="supported-types-for-named-entity-recognition"></a>Ondersteunde typen voor herkenning van benoemde entiteiten

| Type  | SubType | Voorbeeld |
|:-----------   |:------------- |:---------|
| Person        | N.V.T.\*         | "Jan", "Bill Gates"     |
| Locatie      | N.V.T.\*         | "Redmond, Washington", "Parijs"  |
| Organisatie  | N.V.T.\*         | 'Microsoft'   |
| Hoeveelheid      | Aantal        | '6', "zes"     | 
| Hoeveelheid      | Percentage    | "50%", "50%"| 
| Hoeveelheid      | Volgnummer       | "2", "tweede"     | 
| Hoeveelheid      | NumberRange   | "4-8"     | 
| Hoeveelheid      | Leeftijd           | "90 dagen oud is", "30 jaar oude"    | 
| Hoeveelheid      | Valuta      | "$10,99"     | 
| Hoeveelheid      | Dimensie     | "10 mijl", "40 cm"     | 
| Hoeveelheid      | Temperatuur   | "32 graden"    |
| DateTime      | N.V.T.\*         | "6:30 PM 4 februari 2012"      | 
| DateTime      | Date          | "Mei 2e 2017", "05/02/2017"   | 
| Datum tijd     | Time          | "8 am", "8:00"  | 
| DateTime      | DateRange     | "Mei 2e op 5 mei"    | 
| DateTime      | timeRange     | "18: 00 uur tot 19: 00"     | 
| DateTime      | Duur      | '1 minuut en 45 seconden'   | 
| DateTime      | Instellen           | "elke dinsdag"     | 
| DateTime      | TimeZone      |    | 
| URL           | N.V.T.\*         | "http://www.bing.com"    |
| Email         | N.V.T.\*         | "support@contoso.com" |
\* Afhankelijk van de invoer- en uitgepakte entiteiten, bepaalde entiteiten kunnen laat de `SubType`.



## <a name="preparation"></a>Voorbereiding

Moet u de JSON-documenten in deze indeling hebben:-id, tekst, taal

Zie voor ondersteunde talen, [deze lijst](../text-analytics-supported-languages.md).

De documentgrootte van het moet onder maximaal 5000 tekens per document, en u kunt maximaal 1000 hebben items (id's) per verzameling. De verzameling is in de hoofdtekst van de aanvraag ingediend. Het volgende voorbeeld wordt een afbeelding van inhoud die u naar de gekoppelde entiteit-end verzenden kan.

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
    
## <a name="step-1-structure-the-request"></a>Stap 1: Structuur van de aanvraag

Meer informatie over de definitie van de aanvraag kunnen u vinden in [over het aanroepen van de Tekstanalyse-API](text-analytics-how-to-call-api.md). De volgende punten worden aangepast om u te helpen:

+ Maak een **POST** aanvraag. Controleer de API-documentatie voor deze aanvraag: [Entity Linking-API](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/5ac4251d5b4ccd1554da7634)

+ Stel de HTTP-eindpunt voor de extractie van cruciale frasen. Het omvat de `/entities` resource: `https://westus.api.cognitive.microsoft.com/text/analytics/v2.1-preview/entities`

+ Stel een aanvraagheader om op te nemen van de toegangssleutel voor de Text Analytics-bewerkingen. Zie voor meer informatie, [over het zoeken van eindpunten en toegangssleutels](text-analytics-how-to-access-key.md).

+ In de hoofdtekst van de aanvraag, bieden de JSON-documenten-verzameling die u hebt voorbereid voor deze analyse

> [!Tip]
> Gebruik [Postman](text-analytics-how-to-call-api.md) of open de **API testconsole** in de [documentatie](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-V2-1-Preview/operations/5ac4251d5b4ccd1554da7634) structureren van een aanvraag en PLAATST deze in de service.

## <a name="step-2-post-the-request"></a>Stap 2: De aanvraag plaatsen

Analyse wordt uitgevoerd na ontvangst van de aanvraag. De service accepteert maximaal 100 aanvragen per minuut. Elke aanvraag kan maximaal 1 MB zijn.

Intrekken dat de service staatloos is. Er zijn geen gegevens worden opgeslagen in uw account. Resultaten worden onmiddellijk in het antwoord geretourneerd.

## <a name="step-3-view-results"></a>Stap 3: Resultaten weergeven

Alle POST-verzoeken retourneert een JSON-antwoord met de id's ingedeeld en eigenschappen gedetecteerd.

Uitvoer wordt onmiddellijk geretourneerd. U kunt de resultaten naar een toepassing die JSON accepteert streamen of sla de uitvoer naar een bestand op het lokale systeem en vervolgens importeren in een toepassing waarmee u kunt sorteren, zoeken en de gegevens te manipuleren.

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

In dit artikel hebt u geleerd werkstroom voor entiteiten koppelen met behulp van de Text Analytics in Cognitive Services en concepten. Kortom:

+ [Entiteiten API](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-V2-1-Preview/operations/5ac4251d5b4ccd1554da7634) is beschikbaar voor de geselecteerde talen.
+ JSON-documenten in de aanvraagtekst omvatten een code-id, tekst en taal.
+ POST-aanvraag is een `/entities` eindpunt, met behulp van een persoonlijk [toegang krijgen tot de sleutel en een eindpunt](text-analytics-how-to-access-key.md) die geldig is voor uw abonnement.
+ Antwoorduitvoer, die uit de gekoppelde entiteiten bestaat (met inbegrip van vertrouwen scores verschuivingen en webkoppelingen, voor elk document-ID) kan worden gebruikt in een toepassing

## <a name="see-also"></a>Zie ook 

 [Overzicht van Text Analytics](../overview.md)  
 [Veelgestelde vragen](../text-analytics-resource-faq.md)</br>
 [Text Analytics-productpagina](//go.microsoft.com/fwlink/?LinkID=759712) 

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Tekstanalyse-API](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-V2-1-Preview/operations/5ac4251d5b4ccd1554da7634)
