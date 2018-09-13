---
title: Sentiment cognitief zoeken vaardigheid (Azure Search) | Microsoft Docs
description: Sentiment extraheren uit tekst in een Azure Search verrijking-pijplijn.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.date: 05/01/2018
ms.author: luisca
ms.openlocfilehash: f6a0214ce9bc1cdf6aefbd9cde86e72b7cf7b0f2
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/12/2018
ms.locfileid: "35907193"
---
#   <a name="sentiment-cognitive-skill"></a>Sentiment cognitieve vaardigheden

De **Sentiment** vaardigheid ongestructureerde tekst langs een positieve negatieve zorgcontinuüm, en voor elke record evalueert, retourneert een numerieke score tussen 0 en 1. Scores dicht bij 1 geven een positieve stemming aan en scores dicht bij 0 geven een negatieve stemming aan.

## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Text.SentimentSkill

## <a name="data-limits"></a>Limieten voor gegevens
De maximale grootte van een record moet 5000 tekens wordt gemeten door `String.Length`. Als u splitst u uw gegevens wilt voordat deze worden verzonden naar de analyzer sentiment, gebruik de [tekst splitsen vaardigheid](cognitive-search-skill-textsplit.md).


## <a name="skill-parameters"></a>Kwalificatie parameters

Parameters zijn hoofdlettergevoelig.

| Parameternaam |                      |
|----------------|----------------------|
| defaultLanguageCode | (optioneel) De taalcode om toe te passen op documenten die taal niet expliciet opgeven. <br/> Zie [volledige lijst met ondersteunde talen](../cognitive-services/text-analytics/text-analytics-supported-languages.md) |

## <a name="skill-inputs"></a>Kwalificatie invoer 

| Voer een naam in | Beschrijving |
|--------------------|-------------|
| tekst | De tekst die moet worden geanalyseerd.|
| languageCode  |  (Optioneel) Een tekenreeks die aangeeft van de taal van de records. Als deze parameter niet is opgegeven, is de standaardwaarde 'en'. <br/>Zie [volledige lijst met ondersteunde talen](../cognitive-services/text-analytics/text-analytics-supported-languages.md).|

## <a name="skill-outputs"></a>Kwalificatie uitvoer

| Naam van de uitvoer | Beschrijving |
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