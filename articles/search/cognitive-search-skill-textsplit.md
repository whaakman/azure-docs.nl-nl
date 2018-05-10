---
title: Tekst splitsen cognitieve zoeken kwalificatie (Azure Search) | Microsoft Docs
description: Tekst in segmenten of pagina's van de tekst op basis van de lengte in een Azure Search verrijking pijplijn opsplitsen.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: luisca
ms.openlocfilehash: dbb9261cfce0a8437cfe76121fa16aa87c4b3393
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
#   <a name="text-split-cognitive-skill"></a>Tekst cognitieve kwalificatie splitsen

De **tekst gesplitste** kwalificatie tekst opgesplitst in segmenten van tekst. U kunt opgeven of u wilt opsplitsen van de tekst in zinnen of pagina's van een bepaalde lengte. Deze kwalificatie is vooral nuttig als er tekst een maximale van lengtevereisten in andere vaardigheden downstream. 

## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Text.SplitSkill 

## <a name="skill-parameters"></a>Kwalificatie Parameters

Parameters zijn hoofdlettergevoelig.

| Parameternaam     | Beschrijving |
|--------------------|-------------|
| textSplitMode      | 'Pagina's ' of 'zinnen' | 
| maximumPageLength | Als textSplitMode is ingesteld op 'pagina's ', verwijst deze naar de maximale lengte wordt gemeten door `String.Length`. De minimumwaarde is 100. | 
| defaultLanguageCode   | (optioneel) Een van de volgende taalcodes: `da, de, en, es, fi, fr, it, ko, pt`. De standaardwaarde is Engels (en). Enkele overwegingen:<ul><li>Als u een indeling languagecode countrycode doorgeeft, wordt alleen het languagecode deel van de opmaak wordt gebruikt.</li><li>Als de taal niet in de vorige lijst, wordt de tekst, de split-kwalificatie op teken grenzen verbroken.</li><li>Voorzien in een taalcode is handig om te voorkomen dat een woord in de helft voor niet-ruimte talen zoals Chinees en Japans, Koreaans knippen.</li></ul>  |


## <a name="skill-inputs"></a>Kwalificatie invoer

| Parameternaam       | Beschrijving      |
|----------------------|------------------|
| Tekst  | De tekst in subtekenreeks splitsen. |
| languageCode  | (Optioneel) Taalcode voor het document.  |

## <a name="skill-outputs"></a>Kwalificatie uitvoer 

| Parameternaam     | Beschrijving |
|--------------------|-------------|
| textItems | Een matrix met subtekenreeksen die zijn geëxtraheerd. |


##  <a name="sample-definition"></a>Voorbeeld-definitie

```json
{
    "@odata.type": "#Microsoft.Skills.Text.SplitSkill",
    "textSplitMode" : "pages", 
    "maximumPageLength": 1000,
    "defaultLanguageCode": "en",
    "inputs": [
        {
            "name": "text",
            "source": "/document/content"
        },
        {
            "name": "languageCode",
            "source": "/document/language"
        }
    ],
    "outputs": [
        {
            "name": "textItems",
            "targetName": "mypages"
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
                "text": "This is a the loan application for Joe Romero, he is a Microsoft employee who was born in Chile and then moved to Australia…",
                "languageCode": "en"
            }
        },
        {
            "recordId": "2",
            "data": {
                "text": "This is the second document, which will be broken into several pages...",
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
                "textItems": [
                    "This is the loan…",
                    "On the second page we…"
                ]
            }
        },
        {
            "recordId": "2",
            "data": {
                "textItems": [
                    "This is the second document...",
                    "On the second page of the second doc…"
                ]
            }
        }
    ]
}
```

## <a name="error-cases"></a>Foutgevallen
Als een taal wordt niet ondersteund, wordt een waarschuwing wordt gegenereerd en de tekst wordt gesplitst op teken grenzen.

## <a name="see-also"></a>Zie ook

+ [Vooraf gedefinieerde vaardigheden](cognitive-search-predefined-skills.md)
+ [Het definiëren van een vaardigheden](cognitive-search-defining-skillset.md)
