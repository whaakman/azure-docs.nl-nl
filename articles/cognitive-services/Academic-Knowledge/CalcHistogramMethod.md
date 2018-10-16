---
title: Methode CalcHistogram - Academic Knowledge API
titlesuffix: Azure Cognitive Services
description: Gebruik de methode CalcHistogram voor het berekenen van de verdeling van kenmerkwaarden voor een set met document-entiteiten.
services: cognitive-services
author: alch-msft
manager: cgronlun
ms.service: cognitive-services
ms.component: academic-knowledge
ms.topic: conceptual
ms.date: 03/27/2017
ms.author: alch
ms.openlocfilehash: 915e2e5a67d068c418ce50eee9d84dc66e61ee00
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/15/2018
ms.locfileid: "49321288"
---
# <a name="calchistogram-method"></a>Methode CalcHistogram

De **calchistogram** REST-API wordt gebruikt voor het berekenen van de verdeling van kenmerkwaarden voor een set met document-entiteiten.          


**REST-eindpunt:**
```
https:// westus.api.cognitive.microsoft.com/academic/v1.0/calchistogram?
``` 
<br>
  
## <a name="request-parameters"></a>Aanvraagparameters

Naam  |Waarde | Vereist?  |Beschrijving
-----------|----------|--------|----------
**markering**    |Tekenreeks met tekst | Ja  |Een query-expressie waarmee de entiteiten die voor het berekenen van histogrammen.
**Model** |Tekenreeks met tekst | Nee |Selecteer de naam van het model dat u wilt zoeken.  Op dit moment wordt de standaardwaarde aan *nieuwste*.
**Kenmerken** | Tekenreeks met tekst | Nee<br>Standaard: | Een door komma's gescheiden lijst die Hiermee geeft u de kenmerkwaarden weer die zijn opgenomen in het antwoord. Kenmerknamen zijn hoofdlettergevoelig.
**count** |Aantal | Nee<br>Standaard: 10 |Het aantal resultaten dat moet worden geretourneerd.
**offset**  |Aantal | Nee<br>Standaard: 0 |De index van het eerste resultaat om terug te keren.
**Time-out**  |Aantal | Nee<br>Standaard: 1000 |Time-out in milliseconden. Alleen interpretaties gevonden voordat de time-out is verstreken, worden geretourneerd.

## <a name="response-json"></a>Antwoord (JSON)

Naam | Beschrijving
--------|---------
**markering**  |De markering-parameter van de aanvraag.
**num_entities** | Totaal aantal overeenkomende entiteiten.
**histogrammen** |  Een matrix van histogrammen, één voor elk kenmerk dat is opgegeven in de aanvraag.
**.attribute histogrammen [x]** | De naam van het kenmerk waarover het histogram is berekend.
**.distinct_values histogrammen [x]** | Het aantal distinctieve waarden tussen die overeenkomen met entiteiten voor dit kenmerk.
**.total_count histogrammen [x]** | Totaal aantal instanties van de waarde tussen die overeenkomen met entiteiten voor dit kenmerk.
**.histogram histogrammen [x]** | Histogramgegevens voor dit kenmerk.
**histogrammen [.histogram [j] .value x]** |  Een waarde voor het kenmerk.
**histogrammen [.logprob .histogram [j] x]**  |Totaal aantal natuurlijke logboek waarschijnlijkheid die overeenkomen met entiteiten met de waarde van dit kenmerk.
**histogrammen [.count .histogram [j] x]**  |Het aantal overeenkomende entiteiten met de waarde van dit kenmerk.
**afgebroken** | Waar, als de aanvraag is een time-out.


#### <a name="example"></a>Voorbeeld:
```
https:// westus.api.cognitive.microsoft.com/academic/v1.0/calchistogram?expr=And(Composite(AA.AuN=='jaime teevan'),Y>2012)&attributes=Y,F.FN&count=4
```
<br>In dit voorbeeld om een histogram van de telling van publicaties lange per jaar van een bepaalde schrijver sinds 2010, we kunnen eerst genereren de query-expressie met de **interpreteren** API met query-tekenreeks: *papers door Jaime teevan na 2012*.

```
https:// westus.api.cognitive.microsoft.com/academic/v1.0/interpret?query=papers by jaime teevan after 2012
```
<br>De expressie in de eerste interpretatie die wordt geretourneerd vanaf de interpreteren API is *en (samengestelde (AA. AuN == 'jaime teevan'), Y > 2012)*.
<br>Deze waarde van de expressie wordt vervolgens doorgegeven aan de **calchistogram** API. De *attributes=Y,F.FN* parameter geeft aan dat de verdeling van document telt bijvoorbeeld op jaar en veld onderzoek moeten zijn:
```
https:// westus.api.cognitive.microsoft.com/academic/v1.0/calchistogram?expr=And(Composite(AA.AuN=='jaime teevan'),Y>2012)&attributes=Y,F.FN&count=4
```
<br>Het antwoord op deze aanvraag eerst geeft aan dat er 37 documenten die overeenkomen met de query-expressie.  Voor de *jaar* kenmerk, er zijn 3 verschillende waarden, één voor elk jaar na 2012 (dat wil zeggen 2013, 2014 en 2015) zoals opgegeven in de query.  Het totale aantal gedurende de 3 afzonderlijke waarden is 37.  Voor elk *jaar*, het histogram geeft de waarde, totale natuurlijke logboek kans en aantal die overeenkomen met entiteiten.     

Het histogram voor *veld bestuderen* geeft aan dat 34 verschillende velden van het onderzoek. Als een document gekoppeld aan meerdere velden van het onderzoek worden kan, kan het totale aantal (53) groter zijn dan het aantal overeenkomende entiteiten.  Hoewel er afzonderlijke waarden 34 zijn, bevat het antwoord alleen de top 4 vanwege de *count = 4* parameter.

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
