---
title: Gevoel cognitieve zoeken kwalificatie (Azure Search) | Microsoft Docs
description: Gevoel extraheren uit tekst in een Azure Search verrijking-pijplijn.
services: search
manager: pablocas
author: luiscabrer
documentationcenter: ''
ms.assetid: ''
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.date: 05/01/2018
ms.author: luisca
ms.openlocfilehash: 1ddbba5b881cd05a997cd24a9396d5b722376e6f
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
ms.locfileid: "33790998"
---
#   <a name="sentiment-cognitive-skill"></a>Gevoel cognitieve kwalificatie

De **gevoel** kwalificatie evalueert ongestructureerde tekst langs een positieve-negatief geheel worden beschouwd en voor elke record, een numerieke score tussen 0 en 1 geretourneerd. Scores dicht bij 1 geven een positieve stemming aan en scores dicht bij 0 geven een negatieve stemming aan.

## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Text.SentimentSkill

## <a name="data-limits"></a>Gegevenslimieten
De maximale grootte van een record moet 5000 tekens gemeten door `String.Length`. Als u opsplitsen van uw gegevens wilt voordat deze wordt verzonden naar de gevoel analyzer, gebruik de [tekst Split-kwalificatie](cognitive-search-skill-textsplit.md).


## <a name="skill-parameters"></a>Kwalificatie parameters

Parameters zijn hoofdlettergevoelig.

| Parameternaam |                      |
|----------------|----------------------|
| defaultLanguageCode | (optioneel) De taalcode toegepast op documenten die taal niet expliciet opgeven. <br/> Zie [volledige lijst met ondersteunde talen](../cognitive-services/text-analytics/text-analytics-supported-languages.md) |

## <a name="skill-inputs"></a>Kwalificatie invoer 

| Voer een naam | Beschrijving |
|--------------------|-------------|
| Tekst | De tekst die moet worden geanalyseerd.|
| languageCode  |  (Optioneel) Een tekenreeks die de taal van de records aangeeft. Als deze parameter niet is opgegeven, is de standaardwaarde 'en'. <br/>Zie [volledige lijst met ondersteunde talen](../cognitive-services/text-analytics/text-analytics-supported-languages.md).|

## <a name="skill-outputs"></a>Kwalificatie uitvoer

| Naam voor uitvoer | Beschrijving |
|--------------------|-------------|
| Score | Een waarde tussen 0 en 1 dat het gevoel over van de geanalyseerde tekst vertegenwoordigt. Waarden dicht bij 0 negatieve gevoel hebben, hebben bijna 0,5 neutrale gevoel en waarden bijna 1 positief gevoel hebben.|


##  <a name="sample-definition"></a>Voorbeeld-definitie

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

##  <a name="sample-input"></a>Voorbeeldinvoer

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
Als u niets opgeeft, wordt een score gevoel niet voor deze records geretourneerd.

## <a name="error-cases"></a>Foutgevallen
Als een andere taal wordt niet ondersteund, wordt een fout gegenereerd en geen score gevoel wordt geretourneerd.

## <a name="see-also"></a>Zie ook

+ [Vooraf gedefinieerde vaardigheden](cognitive-search-predefined-skills.md)
+ [Het definiëren van een vaardigheden](cognitive-search-defining-skillset.md)