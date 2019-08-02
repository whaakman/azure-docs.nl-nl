---
title: Methode Evaluate-Academic Knowledge API
titlesuffix: Azure Cognitive Services
description: Gebruik de methode Evaluate om een set academische entiteiten te retour neren op basis van een query-expressie.
services: cognitive-services
author: alch-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/27/2017
ms.author: alch
ROBOTS: NOINDEX
ms.openlocfilehash: 69e701d6727e5410b71e6cf8fbe20a1cd038ddb0
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2019
ms.locfileid: "68705012"
---
# <a name="evaluate-method"></a>Methode Evaluate

De **evaluate** rest API wordt gebruikt om een set academische entiteiten te retour neren op basis van een query-expressie.
<br>

**REST-eind punt:**  
```
https://westus.api.cognitive.microsoft.com/academic/v1.0/evaluate? 
```   
<br>

## <a name="request-parameters"></a>Aanvraagparameters  

Name     | Value | Vereist?  | Description
-----------|-----------|---------|--------
**expr**       | Tekst teken reeks | Ja | Een query-expressie waarmee wordt opgegeven welke entiteiten moeten worden geretourneerd.
**model**      | Tekst teken reeks | Nee  | De naam van het model dat u wilt opvragen.  Op dit moment wordt de waarde standaard ingesteld op *meest recente*.        
**eigenschappen** | Tekst teken reeks | Nee<br>prijs ID | Een door komma's gescheiden lijst waarin de kenmerk waarden worden opgegeven die in het antwoord zijn opgenomen. Kenmerk namen zijn hoofdletter gevoelig.
**count**        | Number | Nee<br>Standaard: 10 | Het aantal resultaten dat moet worden geretourneerd.
**offset**     | Number |   Nee<br>Standaard: 0    | Index van het eerste resultaat dat moet worden geretourneerd.
**orderby** |   Tekst teken reeks | Nee<br>Standaard: door de probe verminderen | De naam van een kenmerk dat wordt gebruikt voor het sorteren van de entiteiten. Optioneel kan oplopend/aflopend worden opgegeven. De indeling is: *naam: ASC* of *naam: desc*.
  
 <br>

## <a name="response-json"></a>Antwoord (JSON)

Name | Description
-------|-----   
**expr** |  De *expr* para meter van de aanvraag.
**Rijg** |  Een matrix van 0 of meer entiteiten die overeenkomen met de query-expressie. Elke entiteit bevat een natuurlijke logboek waarschijnlijkheids waarde en de waarden van andere aangevraagde kenmerken.
**afgebroken** | Waar als er een time-out is opgetreden voor de aanvraag.

<br>

#### <a name="example"></a>Voorbeeld:
```
https://westus.api.cognitive.microsoft.com/academic/v1.0/evaluate?expr=
Composite(AA.AuN=='jaime teevan')&count=2&attributes=Ti,Y,CC,AA.AuN,AA.AuId
```
<br>Normaal gesp roken wordt een expressie verkregen van een reactie op de methode interpreteren.  Maar u kunt ook zelf query-expressies samen stellen (Zie [syntaxis van query-expressie](QueryExpressionSyntax.md)).  
  
Als u de para meters *Count* en *Offset* gebruikt, kan er een groot aantal resultaten worden verkregen zonder dat er een enkele aanvraag wordt verzonden die resulteert in een enorme (en mogelijk trage) reactie.  In dit voor beeld heeft de aanvraag de expressie gebruikt voor de eerste interpretatie van de API-reactie interpreteren als de *expr* waarde. De para meter *Count = 2* geeft aan dat er twee entiteits resultaten worden aangevraagd. En de *kenmerken = ti, Y, CC, AA. AuN, AA.* De para meter AuId geeft aan dat voor elk resultaat de titel, het jaar, het aantal citaten, de naam van de auteur en de auteur-id zijn aangevraagd.  Zie [entiteits kenmerken](EntityAttributes.md) voor een lijst met kenmerken.
  
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
 
