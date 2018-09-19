---
title: Methode - Knowledge Exploration Service API evalueren
titlesuffix: Azure Cognitive Services
description: Informatie over het gebruik van de methode Evaluate in de Knowledge Exploration Service KES () API.
services: cognitive-services
author: bojunehsu
manager: cgronlun
ms.service: cognitive-services
ms.component: knowledge-exploration
ms.topic: conceptual
ms.date: 03/26/2016
ms.author: paulhsu
ms.openlocfilehash: 45b25ec5cfc6e198b9b125675f4942463cef247a
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/18/2018
ms.locfileid: "46128261"
---
# <a name="evaluate-method"></a>Methode evalueren

De *evalueren* methode evalueert en retourneert de uitvoer van een gestructureerde query-expressie op basis van de indexgegevens.

Normaal gesproken worden een expressie opgehaald uit een antwoord aan de methode interpreteren.  Maar u kunt ook samenstellen query-expressies zelf (Zie [gestructureerde Query-expressie](Expressions.md)).  

## <a name="request"></a>Aanvraag 

`http://<host>/evaluate?expr=<expr>&attributes=<attrs>[&<options>]`   

Naam|Waarde|Beschrijving
----|----|----
markering       | Tekenreeks met tekst | Gestructureerde query-expressie die een subset van entiteiten van de index geselecteerd.
Kenmerken | Tekenreeks met tekst | Met door komma's gescheiden lijst van kenmerken om op te nemen in de reactie.
count      | Getal (standaard = 10) | Maximum aantal resultaten dat moet worden geretourneerd.
offset     | Getal (standaard = 0) | De index van het eerste resultaat om terug te keren.
sorteren op |   Tekenreeks met tekst | Naam van het kenmerk dat wordt gebruikt om de resultaten, gevolgd door de optionele sorteervolgorde te sorteren (standaard = asc): "*attrname*[: (asc&#124;desc)] '.  Indien niet opgegeven, worden de resultaten worden geretourneerd door het natuurlijke logboek kans te verkleinen.
timeout  | Getal (standaard = 1000) | Time-out in milliseconden. Alleen de resultaten berekend voordat de time-out is verstreken, worden geretourneerd.

Met behulp van de *aantal* en *offset* parameters, een groot aantal resultaten kan worden verkregen incrementeel ten opzichte van meerdere aanvragen.
  
## <a name="response-json"></a>Antwoord (JSON)
JSONPath|Beschrijving
----|----
$.expr | *markering* parameter van de aanvraag.
$.entities | Matrix van 0 of meer object entiteiten die overeenkomen met de structured query-expressie. 
$.aborted | Waar, als de aanvraag is een time-out.

Elke entiteit bevat een *logprob* waarde en de waarden van de aangevraagde kenmerken.

## <a name="example"></a>Voorbeeld
In het voorbeeld academische publicaties, geeft de volgende aanvraag een gestructureerde query-expressie (mogelijk afkomstig zijn van de uitvoer van een *interpreteren* aanvraag) en worden enkele kenmerken voor de bovenste 2 die overeenkomen met entiteiten opgehaald:

`http://<host>/evaluate?expr=Composite(Author.Name=='jaime teevan')&attributes=Title,Y,Author.Name,Author.Id&count=2`

Het antwoord bevat de top 2 ("count = 2") waarschijnlijk die overeenkomen met entiteiten.  Voor elke entiteit maakt, worden de titel, jaar, de naam van auteur en de auteur van id-kenmerken geretourneerd.  Houd er rekening mee hoe de waarden voor het kenmerken van de structuur van samengestelde overeenkomt met de manier waarop die deze zijn opgegeven in het gegevensbestand. 

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
