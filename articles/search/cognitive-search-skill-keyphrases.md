---
title: Belangrijke woordgroepen extractie cognitief zoeken vaardigheid (Azure Search) | Microsoft Docs
description: Evalueert ongestructureerde tekst en voor elke record geeft als resultaat een lijst met belangrijke zinnen in een Azure Search verrijking-pijplijn.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: luisca
ms.openlocfilehash: 694271115626c652523be34160ad6a07053f6387
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/17/2018
ms.locfileid: "45735795"
---
#   <a name="key-phrase-extraction-cognitive-skill"></a>Sleutel vindt er sleuteltermextractie plaats cognitieve vaardigheden

De **sleutel vindt er sleuteltermextractie plaats** vaardigheid evalueert ongestructureerde tekst en voor elke record retourneert een lijst met sleuteltermen.

Deze functie is handig als u snel identificeren van de belangrijkste gespreksonderwerpen in de record. Bijvoorbeeld bepaalde invoertekst 'de voedselvoorzieningsketen delicious is en er waren prachtige personeel', de service 'voeding' en "personeel prachtige" retourneert.

> [!NOTE]
> Cognitief zoeken is een openbare preview. Uitvoering van vaardigheden en uitpakken van de installatiekopie en normalisering worden momenteel gratis aangeboden. Op een later tijdstip, worden de prijzen van deze mogelijkheden aangekondigd. 

## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Text.KeyPhraseExtractionSkill 

## <a name="data-limits"></a>Limieten voor gegevens
De maximale grootte van een record moet tussen de 50.000 tekens wordt gemeten door `String.Length`. Als u moet het opsplitsen van uw gegevens voordat deze naar de extractor sleuteluitdrukkingen verzonden, kunt u overwegen de [tekst splitsen vaardigheid](cognitive-search-skill-textsplit.md).

## <a name="skill-parameters"></a>Kwalificatie parameters

Parameters zijn hoofdlettergevoelig.
| Invoer                | Beschrijving |
|---------------------|-------------|
| defaultLanguageCode | (Optioneel) De taalcode om toe te passen op documenten die taal niet expliciet opgeven.  Als de standaardtaalcode niet opgegeven, Engels is (en) gebruikt als de standaard taal. <br/> Zie [volledige lijst met ondersteunde talen](https://docs.microsoft.com/azure/cognitive-services/text-analytics/text-analytics-supported-languages). |
| maxKeyPhraseCount   | (Optioneel) Het maximum aantal sleuteltermen te produceren. |

## <a name="skill-inputs"></a>Kwalificatie invoer
| Invoer     | Beschrijving |
|--------------------|-------------|
| tekst | De tekst die moet worden geanalyseerd.|
| languageCode  |  Een tekenreeks die aangeeft van de taal van de records. Als deze parameter niet is opgegeven, wordt de standaardtaalcode worden gebruikt voor het analyseren van de records. <br/>Zie [volledige lijst met ondersteunde talen](https://docs.microsoft.com/azure/cognitive-services/text-analytics/text-analytics-supported-languages)|

##  <a name="sample-definition"></a>Van voorbeelddefinitie

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

##  <a name="sample-input"></a>Van Voorbeeldinvoer

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
Als u een niet-ondersteunde taalcode opgeeft, wordt een fout gegenereerd en belangrijke zinnen zijn niet uitgepakt.
Als uw tekst leeg is, kunt u een waarschuwing wordt geproduceerd.
Als de tekst groter dan 50.000 tekens is, worden alleen de eerste 50.000 tekens worden geanalyseerd en wordt er een waarschuwing worden weergegeven.

## <a name="see-also"></a>Zie ook

+ [Vooraf gedefinieerde vaardigheden](cognitive-search-predefined-skills.md)
+ [Hoe u een set vaardigheden definiëren](cognitive-search-defining-skillset.md)