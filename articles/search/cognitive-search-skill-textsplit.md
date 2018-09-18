---
title: Tekst splitsen vaardigheid cognitief zoeken (Azure Search) | Microsoft Docs
description: Tekst opdelen in segmenten of pagina's van de tekst op basis van de lengte in een Azure Search verrijking-pijplijn.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: luisca
ms.openlocfilehash: 583d2ac5a8ac4c236612cdfe78595da1812c56fa
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/17/2018
ms.locfileid: "45730763"
---
#   <a name="text-split-cognitive-skill"></a>Tekst splitsen cognitieve vaardigheden

De **tekst splitsen** vaardigheid tekst opgesplitst in segmenten van de tekst. U kunt opgeven of u wilt verbreken van de tekst in zinnen of pagina's van een bepaalde lengte. Deze kwalificatie is vooral handig als er vereisten voor de lengte in andere vaardigheden downstream maximale tekst zijn. 

> [!NOTE]
> Cognitief zoeken is een openbare preview. Uitvoering van vaardigheden en uitpakken van de installatiekopie en normalisering worden momenteel gratis aangeboden. Op een later tijdstip, worden de prijzen van deze mogelijkheden aangekondigd. 

## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Text.SplitSkill 

## <a name="skill-parameters"></a>Kwalificatie Parameters

Parameters zijn hoofdlettergevoelig.

| Parameternaam     | Beschrijving |
|--------------------|-------------|
| textSplitMode      | 'Pagina's ' of "zinnen" | 
| maximumPageLength | Als textSplitMode is ingesteld op 'pagina's ', verwijst deze naar de maximale lengte wordt gemeten door `String.Length`. De minimumwaarde is 100. | 
| defaultLanguageCode   | (optioneel) Een van de volgende taalcodes: `da, de, en, es, fi, fr, it, ko, pt`. De standaardwaarde is Engels (en). Aantal dingen om te overwegen:<ul><li>Als u een indeling languagecode countrycode doorgeeft, wordt alleen het languagecode deel van de indeling wordt gebruikt.</li><li>De kwalificatie split verbroken als de taal die zich niet in de vorige lijst, de tekst op de grenzen van het teken.</li><li>Biedt een taalcode is handig om te voorkomen dat een woord in de helft voor niet-ruimte talen zoals Chinese, Japans en Koreaanse knippen.</li></ul>  |


## <a name="skill-inputs"></a>Kwalificatie invoer

| Parameternaam       | Beschrijving      |
|----------------------|------------------|
| tekst  | De tekst die moet worden gesplitst in subtekenreeksen. |
| languageCode  | (Optioneel) De taalcode die voor het document.  |

## <a name="skill-outputs"></a>Kwalificatie uitvoer 

| Parameternaam     | Beschrijving |
|--------------------|-------------|
| textItems | Een matrix met subtekenreeksen die zijn opgehaald. |


##  <a name="sample-definition"></a>Van voorbeelddefinitie

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

##  <a name="sample-input"></a>Van Voorbeeldinvoer

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

##  <a name="sample-output"></a>Voorbeeld van uitvoer

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
Als een taal wordt niet ondersteund, wordt een waarschuwing wordt gegenereerd en wordt de tekst is opgesplitst in de grenzen van het teken.

## <a name="see-also"></a>Zie ook

+ [Vooraf gedefinieerde vaardigheden](cognitive-search-predefined-skills.md)
+ [Hoe u een set vaardigheden definiëren](cognitive-search-defining-skillset.md)
