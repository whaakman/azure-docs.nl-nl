---
title: Methode CalcHistogram - Knowledge Exploration Service API
titlesuffix: Azure Cognitive Services
description: Informatie over het gebruik van de methode CalcHistogram in de Knowledge Exploration Service KES () API.
services: cognitive-services
author: bojunehsu
manager: nitinme
ms.service: cognitive-services
ms.subservice: knowledge-exploration
ms.topic: conceptual
ms.date: 03/26/2016
ms.author: paulhsu
ms.openlocfilehash: aaa5b3a85c08f11d821557257de451b8ffc8a3fc
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55860453"
---
# <a name="calchistogram-method"></a>calchistogram methode
De *calchistogram* methode berekent de objecten die overeenkomen met een structured query-expressie en berekent de verdeling van de kenmerkwaarden.

## <a name="request"></a>Aanvraag
`http://<host>/calchistogram?expr=<expr>[&options]` 

Name|Value|Description
----|-----|-----------
expr | Tekenreeks met tekst | Gestructureerde query-expressie waarmee de index-entiteiten die voor het berekenen van histogrammen.
Kenmerken | Text string (default="") | Door komma's gescheiden lijst van kenmerk opgenomen in het antwoord.
count   | Getal (standaard = 10) | Het aantal resultaten dat moet worden geretourneerd.
offset  | Getal (standaard = 0) | De index van het eerste resultaat om terug te keren.

## <a name="response-json"></a>Antwoord (JSON)
JSONPath | Description
----|----
$.expr | *markering* parameter van de aanvraag.
$.num_entities | Totaal aantal overeenkomende entiteiten.
$.histograms |  Matrix van histogrammen, één voor elk kenmerk aangevraagd.
$.histograms [\*] .attribute | De naam van het kenmerk waarover het histogram is berekend.
$.histograms [\*] .distinct_values | Het aantal distinctieve waarden tussen die overeenkomen met entiteiten voor dit kenmerk.
$.histograms[\*].total_count | Totaal aantal instanties van de waarde tussen die overeenkomen met entiteiten voor dit kenmerk.
$.histograms [\*] .histogram | Histogramgegevens voor dit kenmerk.
$.histograms [\*] .histogram [\*] .value | Waarde van het kenmerk.
$.histograms [\*] .histogram [\*] .logprob  | Totaal aantal natuurlijke logboek waarschijnlijkheid die overeenkomen met entiteiten met de waarde van dit kenmerk.
$.histograms [\*] .histogram [\*] .count    | Het aantal overeenkomende entiteiten met de waarde van dit kenmerk.
$.aborted | Waar, als de aanvraag is een time-out.

### <a name="example"></a>Voorbeeld
In de academische publicaties worden bijvoorbeeld berekent de volgende een histogram met publicatie aantallen per jaar en door het sleutelwoord voor een bepaalde auteur sinds 2013:

`http://<host>/calchistogram?expr=And(Composite(Author.Name=='jaime teevan'),Year>=2013)&attributes=Year,Keyword&count=4`

Het antwoord geeft aan dat er 37 documenten die overeenkomt met de query-expressie.  Voor de *jaar* kenmerk, er zijn 3 verschillende waarden, één voor elk jaar sinds 2013.  Het totale aantal gedurende de 3 afzonderlijke waarden is 37.  Voor elk *jaar*, het histogram geeft de waarde, totale natuurlijke logboek kans en aantal die overeenkomen met entiteiten.     

Het histogram voor *sleutelwoord* geeft aan dat 34 afzonderlijke trefwoorden. Als een document gekoppeld aan meerdere trefwoorden worden kan, kan het totale aantal (53) groter zijn dan het aantal overeenkomende entiteiten.  Hoewel er afzonderlijke waarden 34 zijn, bevat het antwoord alleen de top 4 vanwege het ' aantal = 4 "parameter.

```json
{
  "expr": "And(Composite(Author.Name=='jaime teevan'),Y>=2013)",
  "num_entities": 37,
  "histograms": [
    {
      "attribute": "Y",
      "distinct_values": 3,
      "total_count": 37,
      "histogram": [
        {
          "value": 2014,
          "logprob": -6.894,
          "count": 15
        },
        {
          "value": 2013,
          "logprob": -6.927,
          "count": 12
        },
        {
          "value": 2015,
          "logprob": -7.082,
          "count": 10
        }
      ]
    },
    {
      "attribute": "Keyword",
      "distinct_values": 34,
      "total_count": 53,
      "histogram": [
        {
          "value": "crowdsourcing",
          "logprob": -7.142,
          "count": 9
        },
        {
          "value": "information retrieval",
          "logprob": -7.389,
          "count": 4
        },
        {
          "value": "personalization",
          "logprob": -7.623,
          "count": 3
        },
        {
          "value": "mobile search",
          "logprob": -7.674,
          "count": 2
        }
      ]
    }
  ]
}
``` 
