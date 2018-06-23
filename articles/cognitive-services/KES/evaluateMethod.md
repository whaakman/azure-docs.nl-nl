---
title: Evalueren van de methode in kennis exploratie Service API | Microsoft Docs
description: Informatie over het gebruik van de methode evalueren in het Knowledge exploratie Service (KES) API in cognitieve Services.
services: cognitive-services
author: bojunehsu
manager: stesp
ms.service: cognitive-services
ms.component: knowledge-exploration
ms.topic: article
ms.date: 03/26/2016
ms.author: paulhsu
ms.openlocfilehash: fc3d73b326b565cfe40d1b82cc419357b28a801a
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344501"
---
# <a name="evaluate-method"></a>Methode evalueren
De *evalueren* methode evalueert en retourneert de uitvoer van een structured query-expressie op basis van de index.

Normaal gesproken worden een expressie opgehaald uit een antwoord aan de methode interpret.  Maar u kunt ook samenstellen query-expressies zelf (Zie [Structured Query-expressie](Expressions.md)).  

## <a name="request"></a>Aanvraag 
`http://<host>/evaluate?expr=<expr>&attributes=<attrs>[&<options>]`   

Naam|Waarde|Beschrijving
----|----|----
expr       | Tekenreeks | Structured query-expressie waarmee een subset van de index entiteiten worden geselecteerd.
kenmerken | Tekenreeks | Door komma's gescheiden lijst met kenmerken in reactie op te nemen.
count      | Nummer (standaard = 10) | Maximum aantal resultaten te retourneren.
offset     | Nummer (standaard = 0) | De index van het eerste resultaat te retourneren.
OrderBy |   Tekenreeks | Naam van kenmerk gebruikt om te sorteren van de resultaten, gevolgd door de optionele sorteervolgorde (standaard = asc): '*attrname*[: (asc&#124;desc)] '.  Als niet wordt opgegeven, worden de resultaten geretourneerd door natuurlijke logboek kans verkleinen.
timeout  | Nummer (standaard = 1000) | Time-out in milliseconden. Alleen resultaten berekend voordat de time-out is verstreken, worden geretourneerd.

Met behulp van de *aantal* en *offset* parameters, een groot aantal resultaten kan worden verkregen stapsgewijs voor meerdere aanvragen.
  
## <a name="response-json"></a>Antwoord (JSON)
JSONPath|Beschrijving
----|----
$.expr | *expr* parameter van de aanvraag.
$.entities | Matrix van 0 of meer object entiteiten die overeenkomt met de structured query-expressie. 
$.aborted | True als de aanvraag is een time-out.

Elke entiteit bevat een *logprob* waarde en de waarden van de aangevraagde kenmerken.

## <a name="example"></a>Voorbeeld
In het voorbeeld academic publicaties geeft de volgende aanvraag een structured query-expressie (mogelijk uit de uitvoer van een *interpreteren* aanvraag) en enkele kenmerken voor de bovenste 2 overeenkomende entiteiten worden opgehaald:

`http://<host>/evaluate?expr=Composite(Author.Name=='jaime teevan')&attributes=Title,Y,Author.Name,Author.Id&count=2`

Het antwoord bevat de bovenste 2 ("count = 2 ') waarschijnlijk overeenkomende entiteiten.  De titel, jaar, de naam van auteur en de auteur van id-kenmerken worden voor elke entiteit geretourneerd.  Opmerking hoe de structuur van samengestelde kenmerkwaarden die overeenkomt met de manier waarop die ze worden opgegeven in het gegevensbestand. 

```json
{
  "expr": "Composite(Author.Name=='jaime teevan')",
  "entities": 
  [
    {
      "logprob": -6.645,
      "Ti": "personalizing search via automated analysis of interests and activities",
      "Y": 2005,
      "Author": [
        {
          "Name": "jaime teevan",
          "Id": 1968481722
        },
        {
          "Name": "susan t dumais",
          "Id": 676500258
        },
        {
          "Name": "eric horvitz",
          "Id": 1470530979
        }
      ]
    },
    {
      "logprob": -6.764,
      "Ti": "the perfect search engine is not enough a study of orienteering behavior in directed search",
      "Y": 2004,
      "Author": [
        {
          "Name": "jaime teevan",
          "Id": 1982462162
        },
        {
          "Name": "christine alvarado",
          "Id": 2163512453
        },
        {
          "Name": "mark s ackerman",
          "Id": 2055132526
        },
        {
          "Name": "david r karger",
          "Id": 2012534293
        }
      ]
    }
  ]
}
```
