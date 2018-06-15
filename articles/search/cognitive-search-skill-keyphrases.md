---
title: Sleutel woordgroep extractie cognitieve zoeken kwalificatie (Azure Search) | Microsoft Docs
description: Niet-gestructureerde tekst geëvalueerd en voor elke record, retourneert een lijst met belangrijke termen in een Azure Search verrijking-pijplijn.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: luisca
ms.openlocfilehash: a12efaa020e626e4a10a0708c9b84d8fe125588c
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
ms.locfileid: "33791033"
---
#   <a name="key-phrase-extraction-cognitive-skill"></a>Uitpakken van sleutel woordgroep cognitieve kwalificatie

De **uitpakken van sleutel woordgroep** kwalificatie ongestructureerde tekst geëvalueerd en voor elke record retourneert een lijst met belangrijke termen.

Deze functie is handig als u hebt de belangrijkste gesproken punten in de record snel kan identificeren. Bijvoorbeeld bepaalde invoertekst 'de voeding delicious is en er zijn prachtige personeel', stuurt de service 'voeding' en 'prachtige personeel'.

## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Text.KeyPhraseExtractionSkill 

## <a name="data-limits"></a>Gegevenslimieten
De maximale grootte van een record moet 50.000 tekens gemeten door `String.Length`. Als u opsplitsen van uw gegevens wilt voordat deze wordt verzonden naar de sleutel woordgroep zelfstandig uitpakken, overweeg dan de [tekst Split-kwalificatie](cognitive-search-skill-textsplit.md).

## <a name="skill-parameters"></a>Kwalificatie parameters

Parameters zijn hoofdlettergevoelig.
| Invoer                | Beschrijving |
|---------------------|-------------|
| defaultLanguageCode | (Optioneel) De taalcode toegepast op documenten die taal niet expliciet opgeven.  Als de standaardtaal is niet opgegeven, Engels (en) gebruikt als de standaard taal. <br/> Zie [volledige lijst met ondersteunde talen](https://docs.microsoft.com/azure/cognitive-services/text-analytics/text-analytics-supported-languages). |
| maxKeyPhraseCount   | (Optioneel) Het maximum aantal belangrijke woordgroepen produceren. |

## <a name="skill-inputs"></a>Kwalificatie invoer
| Invoer     | Beschrijving |
|--------------------|-------------|
| Tekst | De tekst die moet worden geanalyseerd.|
| languageCode  |  Een tekenreeks die de taal van de records aangeeft. Als deze parameter niet is opgegeven, wordt de taalcode van de standaardwaarde gebruikt voor het analyseren van de records. <br/>Zie [volledige lijst met ondersteunde talen](https://docs.microsoft.com/azure/cognitive-services/text-analytics/text-analytics-supported-languages)|

##  <a name="sample-definition"></a>Voorbeeld-definitie

```json
 {
    "@odata.type": "#Microsoft.Skills.Text.KeyPhraseExtractionSkill",
    "inputs": [
      {
        "name": "text",
        "source": "/document/text"
      },
      {
        "name": "languageCode",
        "source": "/document/languagecode" 
      }
    ],
    "outputs": [
      {
        "name": "keyPhrases",
        "targetName": "myKeyPhrases"
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
        "data":
           {
             "text": "Glaciers are huge rivers of ice that ooze their way over land, powered by gravity and their own sheer weight. They accumulate ice from snowfall and lose it through melting. As global temperatures have risen, many of the world’s glaciers have already started to shrink and retreat. Continued warming could see many iconic landscapes – from the Canadian Rockies to the Mount Everest region of the Himalayas – lose almost all their glaciers by the end of the century.",
             "language": "en"
           }
      }
    ]
```


##  <a name="sample-output"></a>Voorbeelduitvoer

```json
{
    "values": [
      {
        "recordId": "1",
        "data":
           {
            "keyPhrases": 
            [
              "world’s glaciers", 
              "huge rivers of ice", 
              "Canadian Rockies", 
              "iconic landscapes",
              "Mount Everest region",
              "Continued warming"
            ]
           }
      }
    ]
}
```


## <a name="errors-and-warnings"></a>Fouten en waarschuwingen
Als u een niet-ondersteunde taalcode opgeeft, wordt een fout gegenereerd en sleutel zinnen zijn niet uitgepakt.
Als uw tekst leeg is, wordt er een waarschuwing geproduceerd.
Als de tekst langer dan 50.000 tekens is, wordt alleen de eerste 50.000 tekens worden geanalyseerd en een waarschuwing worden uitgegeven.

## <a name="see-also"></a>Zie ook

+ [Vooraf gedefinieerde vaardigheden](cognitive-search-predefined-skills.md)
+ [Het definiëren van een vaardigheden](cognitive-search-defining-skillset.md)