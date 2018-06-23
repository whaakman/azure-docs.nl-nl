---
title: De methode CalcHistogram in de Academic Knowledge API | Microsoft Docs
description: Gebruik de methode CalcHistogram voor het berekenen van de distributie van kenmerkwaarden voor een set van papier entiteiten in cognitieve Microsoft-Services.
services: cognitive-services
author: alch-msft
manager: kuansanw
ms.service: cognitive-services
ms.component: academic-knowledge
ms.topic: article
ms.date: 03/27/2017
ms.author: alch
ms.openlocfilehash: e0b773fb9791ee638c8cfdbbc9dca40543e50ec0
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344422"
---
# <a name="calchistogram-method"></a>CalcHistogram methode

De **calchistogram** REST-API wordt gebruikt voor het berekenen van de distributie van kenmerkwaarden voor een reeks papier entiteiten.          


**REST-eindpunt:**
```
https:// westus.api.cognitive.microsoft.com/academic/v1.0/calchistogram?
``` 
<br>
  
## <a name="request-parameters"></a>Aanvraagparameters

Naam  |Waarde | Vereist?  |Beschrijving
-----------|----------|--------|----------
**expr**    |Tekenreeks | Ja  |Een query-expressie waarmee de entiteiten waarover histogrammen berekenen.
**model** |Tekenreeks | Nee |Selecteer de naam van het model dat u wilt zoeken.  Op dit moment wordt de standaardwaarde *nieuwste*.
**kenmerken** | Tekenreeks | Nee<br>standaardwaarde: | Een door komma's gescheiden lijst geeft de kenmerkwaarden weer die zijn opgenomen in het antwoord. Kenmerknamen zijn hoofdlettergevoelig.
**aantal** |Aantal | Nee<br>Standaard: 10 |Het aantal resultaten te retourneren.
**offset**  |Aantal | Nee<br>Standaard: 0 |De index van het eerste resultaat te retourneren.
<br>
## <a name="response-json"></a>Antwoord (JSON)
Naam | Beschrijving
--------|---------
**expr**  |De parameter expr van de aanvraag.
**num_entities** | Totaal aantal overeenkomende entiteiten.
**histogrammen toevoegen** |  Een matrix van histogrammen, één voor elk kenmerk dat is opgegeven in de aanvraag.
**[x] histogrammen .attribute** | Naam van het kenmerk gedurende welke de histogram is berekend.
**[x] histogrammen .distinct_values** | Het aantal afzonderlijke waarden tussen die overeenkomt met de entiteiten voor dit kenmerk.
**[x] histogrammen .total_count** | Totaal aantal exemplaren van de waarde tussen entiteiten voor dit kenmerk overeenkomen.
**[x] histogrammen .histogram** | Histogramgegevens voor dit kenmerk.
**histogrammen [.value .histogram [-y] x]** |  Een waarde voor het kenmerk.
**histogrammen [.logprob .histogram [-y] x]**  |Totaal aantal natuurlijke logboek waarschijnlijkheid die overeenkomt met de entiteiten met de waarde van dit kenmerk.
**histogrammen [.count .histogram [-y] x]**  |Het aantal overeenkomende entiteiten met de waarde van dit kenmerk.
**afgebroken** | True als de aanvraag is een time-out.

 <br>
#### <a name="example"></a>Voorbeeld:
```
https:// westus.api.cognitive.microsoft.com/academic/v1.0/calchistogram?expr=And(Composite(AA.AuN=='jaime teevan'),Y>2012)&attributes=Y,F.FN&count=4
```
<br>In dit voorbeeld om een histogram van de telling van publicaties per jaar genereren voor een bepaalde auteur sinds 2010 we kunt eerst genereren de query-expressie met de **interpreteren** API met de querytekenreeks: *papers door Jaime teevan na 2012*.

```
https:// westus.api.cognitive.microsoft.com/academic/v1.0/interpret?query=papers by jaime teevan after 2012
```
<br>De expressie in de eerste interpretatie die wordt geretourneerd vanaf de interpret API is *en (samengestelde (AA. AuN == 'jaime teevan'), Y > 2012)*.
<br>Deze expressiewaarde vervolgens doorgegeven aan de **calchistogram** API. De *attributes=Y,F.FN* parameter geeft aan dat de distributies van papier tellingen op jaar en veld onderzoek, bijvoorbeeld moeten:
```
https:// westus.api.cognitive.microsoft.com/academic/v1.0/calchistogram?expr=And(Composite(AA.AuN=='jaime teevan'),Y>2012)&attributes=Y,F.FN&count=4
```
<br>Het antwoord op deze aanvraag eerst geeft aan dat er 37 documenten die overeenkomen met de query-expressie.  Voor de *jaar* kenmerk, zijn er 3 afzonderlijke waarden, één voor elk jaar na 2012 (dat wil zeggen 2013, 2014 en 2015) zoals opgegeven in de query.  Het totale aantal via de 3 afzonderlijke waarden is 37.  Voor elk *jaar*, het histogram ziet u de waarde, de totale natuurlijke logboek kans en de telling van de entiteiten die overeenkomt met.     

Het histogram voor *veld bestuderen* geeft aan dat er 34 verschillende velden van het onderzoek. Als een artikel is mogelijk gekoppeld aan meerdere velden van onderzoek, is het totale aantal (53) kan niet groter zijn dan het aantal overeenkomende entiteiten.  Hoewel er 34 afzonderlijke waarden zijn, het antwoord alleen de eerste 4 omdat bevat de *count = 4* parameter.

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
