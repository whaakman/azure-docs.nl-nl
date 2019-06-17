---
title: Sentiment cognitief zoeken vaardigheid - Azure Search
description: Een positieve negatieve gevoelsscore extraheren uit tekst in een Azure Search verrijking-pijplijn.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.date: 05/02/2019
ms.author: luisca
ms.custom: seodec2018
ms.openlocfilehash: f52f5200f33d11db44d94b5a5f26d246f711e224
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65023806"
---
#   <a name="sentiment-cognitive-skill"></a>Sentiment cognitieve vaardigheden

De **Sentiment** vaardigheid ongestructureerde tekst langs een positieve negatieve zorgcontinuüm, en voor elke record evalueert, retourneert een numerieke score tussen 0 en 1. Scores dicht bij 1 geven een positieve stemming aan en scores dicht bij 0 geven een negatieve stemming aan. Deze vaardigheid maakt gebruik van de machine learning-modellen die worden geleverd door [Tekstanalyse](https://docs.microsoft.com/azure/cognitive-services/text-analytics/overview) in Cognitive Services.

> [!NOTE]
> Als u bereik uitbreiden door het verhogen van de frequentie van de verwerking, meer documenten toe te voegen of toe te voegen meer AI-algoritmen, u moet [een factureerbare Cognitive Services-resource koppelen](cognitive-search-attach-cognitive-services.md). Kosten toenemen bij het aanroepen van API's in Cognitive Services en voor het ophalen van de afbeelding als onderdeel van de fase documenten kraken in Azure Search. Er zijn geen kosten voor het ophalen van de tekst van documenten.
>
> Uitvoering van de ingebouwde vaardigheden wordt in rekening gebracht op de bestaande [Cognitive Services betaalt u go prijs](https://azure.microsoft.com/pricing/details/cognitive-services/). Afbeelding extractie prijzen wordt beschreven op de [Azure Search-pagina met prijzen](https://go.microsoft.com/fwlink/?linkid=2042400).


## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Text.SentimentSkill

## <a name="data-limits"></a>Gegevenslimieten
De maximale grootte van een record moet 5000 tekens wordt gemeten door `String.Length`. Als u splitst u uw gegevens wilt voordat deze worden verzonden naar de analyzer sentiment, gebruik de [tekst splitsen vaardigheid](cognitive-search-skill-textsplit.md).


## <a name="skill-parameters"></a>Kwalificatie parameters

Parameters zijn hoofdlettergevoelig.

| Parameternaam |                      |
|----------------|----------------------|
| defaultLanguageCode | (optioneel) De taalcode om toe te passen op documenten die taal niet expliciet opgeven. <br/> Zie [volledige lijst met ondersteunde talen](../cognitive-services/text-analytics/text-analytics-supported-languages.md) |

## <a name="skill-inputs"></a>Kwalificatie invoer 

| Voer een naam in | Description |
|--------------------|-------------|
| text | De tekst die moet worden geanalyseerd.|
| languageCode  |  (Optioneel) Een tekenreeks die aangeeft van de taal van de records. Als deze parameter niet is opgegeven, is de standaardwaarde 'en'. <br/>Zie [volledige lijst met ondersteunde talen](../cognitive-services/text-analytics/text-analytics-supported-languages.md).|

## <a name="skill-outputs"></a>Kwalificatie uitvoer

| Naam van de uitvoer | Description |
|--------------------|-------------|
| score | Een waarde tussen 0 en 1 waarmee het gevoel van de geanalyseerde tekst. Waarden dicht bij 0 negatief gevoel hebben, hebben dicht bij 0,5 neutrale sentiment en -waarden dicht bij 1 positief gevoel hebben.|


##  <a name="sample-definition"></a>Van voorbeelddefinitie

```json
{
    "@odata.type": "#Microsoft.Skills.Text.SentimentSkill",
    "inputs": [
        {
            "name": "text",
            "source": "/document/content"
        },
        {
            "name": "languageCode",
            "source": "/document/languagecode"
        }
    ],
    "outputs": [
        {
            "name": "score",
            "targetName": "mySentiment"
        }
    ]
}
```

##  <a name="sample-input"></a>Van Voorbeeldinvoer

```json
{
    "values": [
        {
            "recordId": "1",
            "data": {
                "text": "I had a terrible time at the hotel. The staff was rude and the food was awful.",
                "languageCode": "en"
            }
        }
    ]
}
```


##  <a name="sample-output"></a>Voorbeelduitvoer

```json
{
    "values": [
        {
            "recordId": "1",
            "data": {
                "score": 0.01
            }
        }
    ]
}
```

## <a name="notes"></a>Opmerkingen
Als dit leeg is, wordt een gevoelsscore wordt niet geretourneerd voor de records.

## <a name="error-cases"></a>Foutgevallen
Als een taal wordt niet ondersteund, wordt een fout gegenereerd en er is geen gevoelsscore wordt geretourneerd.

## <a name="see-also"></a>Zie ook

+ [Vooraf gedefinieerde vaardigheden](cognitive-search-predefined-skills.md)
+ [Hoe u een set vaardigheden definiëren](cognitive-search-defining-skillset.md)
