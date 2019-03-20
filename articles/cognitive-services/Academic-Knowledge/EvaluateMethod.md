---
title: Methode - Academic Knowledge API evalueren
titlesuffix: Azure Cognitive Services
description: Gebruik de methode evalueren om te retourneren van een set academische entiteiten op basis van een query-expressie.
services: cognitive-services
author: alch-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/27/2017
ms.author: alch
ms.openlocfilehash: d2e628fb7fc502ef9ba81d20680d66f24fd7d138
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58004923"
---
# <a name="evaluate-method"></a>Methode evalueren

De **evalueren** REST-API wordt gebruikt om te retourneren van een set academische entiteiten op basis van een query-expressie.
<br>

**REST-eindpunt:**  
```
https://westus.api.cognitive.microsoft.com/academic/v1.0/evaluate? 
```   
<br>

## <a name="request-parameters"></a>Aanvraagparameters  

Name     | Value | Vereist?  | Description
-----------|-----------|---------|--------
**expr**       | Tekenreeks met tekst | Ja | Een query-expressie waarmee wordt aangegeven welke entiteiten moeten worden geretourneerd.
**model**      | Tekenreeks met tekst | Nee  | De naam van het model dat u wilt zoeken.  Op dit moment wordt de standaardwaarde aan *nieuwste*.        
**attributes** | Tekenreeks met tekst | Nee<br>Standaard: Id | Een door komma's gescheiden lijst die Hiermee geeft u de kenmerkwaarden weer die zijn opgenomen in het antwoord. Kenmerknamen zijn hoofdlettergevoelig.
**count**        | Aantal | Nee<br>Standaard: 10 | Het aantal resultaten dat moet worden geretourneerd.
**offset**     | Aantal |   Nee<br>Standaard: 0    | De index van het eerste resultaat om terug te keren.
**orderby** |   Tekenreeks met tekst | Nee<br>Standaard: door kans verkleinen | De naam van een kenmerk dat wordt gebruikt voor het sorteren van de entiteiten. U kunt desgewenst kan oplopend/aflopend worden opgegeven. De indeling is: *naam: asc* of *naam: desc*.
  
 <br>

## <a name="response-json"></a>Antwoord (JSON)

Name | Description
-------|-----   
**expr** |  De *expr* parameter van de aanvraag.
**Entiteiten** |  Een matrix met 0 of meer entiteiten die overeenkomen met de query-expressie. Elke entiteit bevat een waarde voor de kans natuurlijke logboek en de waarden van andere aangevraagde kenmerken.
**afgebroken** | Waar, als de aanvraag is een time-out.

<br>

#### <a name="example"></a>Voorbeeld:
```
https://westus.api.cognitive.microsoft.com/academic/v1.0/evaluate?expr=
Composite(AA.AuN=='jaime teevan')&count=2&attributes=Ti,Y,CC,AA.AuN,AA.AuId
```
<br>Normaal gesproken een expressie worden opgehaald van een reactie op de **interpreteren** methode.  Maar u kunt ook samenstellen query-expressies zelf (Zie [Query-expressies](QueryExpressionSyntax.md)).  
  
Met behulp van de *aantal* en *offset* parameters, een groot aantal resultaten kan worden verkregen zonder het verzenden van een enkele aanvraag de resultaten in een enorme (en mogelijk langzaam)-respons.  In dit voorbeeld gebruikt de aanvraag de expressie voor de eerste interpretatie van de **interpreteren** API-reactie als de *expr* waarde. De *count = 2* parameter geeft u op dat 2 entiteitsresultaten worden aangevraagd. En de *kenmerken Ti, Y, CC, AA =. AuN, AA. AuId* parameter geeft aan dat de titel, jaar, citaat aantal, de naam van auteur en auteur-ID voor elk resultaat worden aangevraagd.  Zie [Entiteitkenmerken](EntityAttributes.md) voor een lijst van kenmerken.
  
```JSON
{
  "expr": "Composite(AA.AuN=='jaime teevan')",
  "entities": 
  [
    {
      "logprob": -15.08,
      "Ti": "personalizing search via automated analysis of interests and activities",
      "Y": 2005,
      "CC": 372,
      "AA": [
        {
          "AuN": "jaime teevan",
          "AuId": 1968481722
        },
        {
          "AuN": "susan t dumais",
          "AuId": 676500258
        },
        {
          "AuN": "eric horvitz",
          "AuId": 1470530979
        }
      ]
    },
    {
      "logprob": -15.389,
      "Ti": "the perfect search engine is not enough a study of orienteering behavior in directed search",
      "Y": 2004,
      "CC": 237,
      "AA": [
        {
          "AuN": "jaime teevan",
          "AuId": 1982462162
        },
        {
          "AuN": "christine alvarado",
          "AuId": 2163512453
        },
        {
          "AuN": "mark s ackerman",
          "AuId": 2055132526
        },
        {
          "AuN": "david r karger",
          "AuId": 2012534293
        }
      ]
    }
  ]
}
 ```
 
