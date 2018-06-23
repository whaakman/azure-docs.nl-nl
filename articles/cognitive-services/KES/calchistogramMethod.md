---
title: CalcHistogram-methode in kennis exploratie Service API | Microsoft Docs
description: Informatie over het gebruik van de methode CalcHistogram in het Knowledge exploratie Service (KES) API in cognitieve Services.
services: cognitive-services
author: bojunehsu
manager: stesp
ms.service: cognitive-services
ms.component: knowledge-exploration
ms.topic: article
ms.date: 03/26/2016
ms.author: paulhsu
ms.openlocfilehash: 6ed694b0cc9cf41b815cc54b9f6d12adb2b7cd64
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344488"
---
# <a name="calchistogram-method"></a>calchistogram methode
De *calchistogram* methode berekent de objecten die overeenkomt met een structured query-expressie en berekent de verdeling van de kenmerkwaarden.

## <a name="request"></a>Aanvraag
`http://<host>/calchistogram?expr=<expr>[&options]` 

Naam|Waarde|Beschrijving
----|-----|-----------
expr | Tekenreeks | Structured query-expressie waarmee de index entiteiten waarover histogrammen berekenen.
kenmerken | Tekenreeks (standaard = "") | Door komma's gescheiden lijst met kenmerk opgenomen in het antwoord.
count   | Nummer (standaard = 10) | Het aantal resultaten te retourneren.
offset  | Nummer (standaard = 0) | De index van het eerste resultaat te retourneren.

## <a name="response-json"></a>Antwoord (JSON)
JSONPath | Beschrijving
----|----
$.expr | *expr* parameter van de aanvraag.
$.num_entities | Totaal aantal overeenkomende entiteiten.
$.histograms |  Matrix van histogrammen, één voor elk kenmerk aangevraagd.
$.histograms [\*] .attribute | Naam van het kenmerk gedurende welke de histogram is berekend.
$.histograms [\*] .distinct_values | Het aantal afzonderlijke waarden tussen die overeenkomt met de entiteiten voor dit kenmerk.
$.histograms [\*] .total_count | Totaal aantal exemplaren van de waarde tussen entiteiten voor dit kenmerk overeenkomen.
$.histograms [\*] .histogram | Histogramgegevens voor dit kenmerk.
$.histograms [\*] .histogram [\*] .value | Waarde van het kenmerk.
$.histograms [\*] .histogram [\*] .logprob  | Totaal aantal natuurlijke logboek waarschijnlijkheid die overeenkomt met de entiteiten met de waarde van dit kenmerk.
$.histograms [\*] .histogram [\*] .count    | Het aantal overeenkomende entiteiten met de waarde van dit kenmerk.
$.aborted | True als de aanvraag is een time-out.

### <a name="example"></a>Voorbeeld
In het voorbeeld academic publicaties berekend het volgende een histogram van de publicatie aantallen per jaar en op trefwoord voor een bepaalde auteur sinds 2013:

`http://<host>/calchistogram?expr=And(Composite(Author.Name=='jaime teevan'),Year>=2013)&attributes=Year,Keyword&count=4`

Het antwoord geeft aan dat er 37 documenten die overeenkomt met de query-expressie.  Voor de *jaar* kenmerk, zijn er 3 afzonderlijke waarden, één voor elk jaar sinds 2013.  Het totale aantal via de 3 afzonderlijke waarden is 37.  Voor elk *jaar*, het histogram ziet u de waarde, de totale natuurlijke logboek kans en de telling van de entiteiten die overeenkomt met.     

Het histogram voor *sleutelwoord* geeft aan dat er 34 distinct trefwoorden. Als een artikel is mogelijk gekoppeld aan meerdere trefwoorden, is het totale aantal (53) kan niet groter zijn dan het aantal overeenkomende entiteiten.  Hoewel er 34 afzonderlijke waarden zijn, het antwoord alleen bevat de eerste 4 omdat het ' aantal = 4 "parameter.

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
