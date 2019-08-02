---
title: Methode CalcHistogram-Academic Knowledge API
titlesuffix: Azure Cognitive Services
description: Gebruik de methode CalcHistogram om de verdeling van kenmerk waarden voor een set papier entiteiten te berekenen.
services: cognitive-services
author: alch-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/27/2017
ms.author: alch
ROBOTS: NOINDEX
ms.openlocfilehash: 9e84b1ad37b3224ec5553d0a66ba0fc84bc88f55
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2019
ms.locfileid: "68705072"
---
# <a name="calchistogram-method"></a>Methode CalcHistogram

De **calchistogram** -rest API wordt gebruikt om de verdeling van kenmerk waarden voor een set papier entiteiten te berekenen.          


**REST-eind punt:**
```
https:// westus.api.cognitive.microsoft.com/academic/v1.0/calchistogram?
``` 
<br>

## <a name="request-parameters"></a>Aanvraagparameters

Name  |Value | Vereist?  |Description
-----------|----------|--------|----------
**expr**    |Tekst teken reeks | Ja  |Een query-expressie die de entiteiten specificeert waarvoor histogrammen moeten worden berekend.
**model** |Tekst teken reeks | Nee |Selecteer de naam van het model dat u wilt doorzoeken.  Op dit moment wordt de waarde standaard ingesteld op *meest recente*.
**eigenschappen** | Tekst teken reeks | Nee<br>prijs | Een door komma's gescheiden lijst waarin de kenmerk waarden worden opgegeven die in het antwoord zijn opgenomen. Kenmerk namen zijn hoofdletter gevoelig.
**count** |Number | Nee<br>Standaard: 10 |Het aantal resultaten dat moet worden geretourneerd.
**offset**  |Number | Nee<br>Standaard: 0 |Index van het eerste resultaat dat moet worden geretourneerd.
**out**  |Number | Nee<br>Standaard: 1000 |Time-out in milliseconden. Alleen interpretaties die zijn gevonden voordat de time-out is verstreken, worden geretourneerd.

## <a name="response-json"></a>Antwoord (JSON)

Name | Description
--------|---------
**expr**  |De expr para meter van de aanvraag.
**num_entities** | Totaal aantal overeenkomende entiteiten.
**histograms** |  Een matrix met histogrammen, één voor elk kenmerk dat is opgegeven in de aanvraag.
**histogrammen [x]. kenmerk** | De naam van het kenmerk waarover het histogram is berekend.
**histogrammen [x]. distinct_values** | Aantal afzonderlijke waarden tussen overeenkomende entiteiten voor dit kenmerk.
**histograms[x].total_count** | Totaal aantal waarde-instanties tussen overeenkomende entiteiten voor dit kenmerk.
**histogrammen [x]. histogram** | Histogram gegevens voor dit kenmerk.
**histogrammen [x]. histogram [y]. waarde** |  Een waarde voor het kenmerk.
**histogrammen [x]. histogram [y]. logprob**  |Totale natuurlijke logboek kans van overeenkomende entiteiten met deze kenmerk waarde.
**histogrammen [x]. histogram [y]. Count**  |Aantal overeenkomende entiteiten met deze kenmerk waarde.
**afgebroken** | Waar als er een time-out is opgetreden voor de aanvraag.


#### <a name="example"></a>Voorbeeld:
```
https:// westus.api.cognitive.microsoft.com/academic/v1.0/calchistogram?expr=And(Composite(AA.AuN=='jaime teevan'),Y>2012)&attributes=Y,F.FN&count=4
```
<br>In dit voor beeld, om een histogram te genereren van het aantal publicaties per jaar voor een bepaalde auteur sinds 2010, kunnen we eerst de query-expressie genereren met behulp van de API interpreteren met de query reeks: *papers by Jaime teevan na 2012*.

```
https:// westus.api.cognitive.microsoft.com/academic/v1.0/interpret?query=papers by jaime teevan after 2012
```
<br>De expressie in de eerste interpretatie die wordt geretourneerd door de API interpreteren is *en (composiet (AA). AuN = = ' Jaime teevan '), Y > 2012)* .
<br>Deze expressie waarde wordt vervolgens door gegeven aan de **calchistogram** -API. Met de para meter *Attributes = Y, F. FN* wordt aangegeven dat de distributies van het papier moeten worden verdeeld op jaar en veld van studie, bijvoorbeeld:
```
https:// westus.api.cognitive.microsoft.com/academic/v1.0/calchistogram?expr=And(Composite(AA.AuN=='jaime teevan'),Y>2012)&attributes=Y,F.FN&count=4
```
<br>De reactie op deze aanvraag geeft eerst aan dat er 37 documenten zijn die overeenkomen met de query-expressie.  Voor het kenmerk *Year* zijn er drie afzonderlijke waarden: één voor elk jaar na 2012 (bijvoorbeeld 2013, 2014 en 2015) zoals opgegeven in de query.  Het totale aantal vellen voor de 3 afzonderlijke waarden is 37.  In het histogram worden voor elk *jaar*de waarde, de totale natuurlijke logboek kans en het aantal overeenkomende entiteiten weer gegeven.     

In het histogram voor het *veld van onderzoek* blijkt dat er 34 afzonderlijke studie velden zijn. Als een papier kan worden gekoppeld aan meerdere studie velden, kan het totale aantal (53) groter zijn dan het aantal overeenkomende entiteiten.  Hoewel er 34 afzonderlijke waarden zijn, bevat het antwoord alleen de hoogste 4 als gevolg van de para meter *Count = 4* .

```JSON
{
  "expr": "And(Composite(AA.AuN=='jaime teevan'),Y>2012)",
  "num_entities": 37,
  "histograms": [
    {
      "attribute": "Y",
      "distinct_values": 3,
      "total_count": 37,
      "histogram": [
        {
          "value": 2014,
          "logprob": -15.753,
          "count": 15
        },
        {
          "value": 2013,
          "logprob": -15.805,
          "count": 12
        },
        {
          "value": 2015,
          "logprob": -16.035,
          "count": 10
        }
      ]
    },
    {
      "attribute": "F.FN",
      "distinct_values": 34,
      "total_count": 53,
      "histogram": [
        {
          "value": "crowdsourcing",
          "logprob": -15.258,
          "count": 9
        },
        {
          "value": "information retrieval",
          "logprob": -16.002,
          "count": 4
        },
        {
          "value": "personalization",
          "logprob": -16.226,
          "count": 3
        },
        {
          "value": "mobile search",
          "logprob": -17.228,
          "count": 2
        }
      ]
    }
  ]
}
```
