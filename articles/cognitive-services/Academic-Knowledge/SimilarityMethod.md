---
title: Methode gelijkenis - Academic Knowledge API
titlesuffix: Azure Cognitive Services
description: Gebruik de methode overeenkomsten voor het berekenen van de academische gelijkenis van twee tekenreeksen.
services: cognitive-services
author: alch-msft
manager: cgronlun
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 01/18/2017
ms.author: alch
ms.openlocfilehash: 7484b570784f5f058ebd23b1e3c225c5d858a274
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55183336"
---
# <a name="similarity-method"></a>Methode gelijkenis

De **gelijkenis** REST-API wordt gebruikt voor het berekenen van de academische cosinusgelijkenis tussen twee tekenreeksen. 
<br>

**REST-eindpunt:**
```
https://westus.api.cognitive.microsoft.com/academic/v1.0/similarity?
```

## <a name="request-parameters"></a>Aanvraagparameters
Parameter        |Gegevenstype      |Vereist | Beschrijving
----------|----------|----------|------------
**s1**        |Reeks   |Ja  |Tekenreeks * moet worden vergeleken
**s2**        |Reeks   |Ja  |Tekenreeks * moet worden vergeleken
<sub> * Tekenreeksen vergelijken, hebben een maximum lengte van 1MB. </sub>
<br>
## <a name="response"></a>Antwoord
Name | Beschrijving
--------|---------
**SimilarityScore**        |Een drijvende-kommawaarde vertegenwoordigt de gelijkenis cosinus van s1 en s2 met dichter bij 1.0 betekenis meer vergelijkbaar en waarden dichter naar-1.0 wat betekent dat kleiner
<br>

## <a name="successerror-conditions"></a>Geslaagde of mislukte voorwaarden
HTTP-Status | Reden | Antwoord
-----------|----------|--------
**200**         |Geslaagd | Drijvendekommagetal
**400**         | Ongeldige aanvraag of aanvraag ongeldig | Foutbericht      
**500**         |Interne serverfout | Foutbericht
**Is een time-out**     | Er is een time-out opgetreden voor de aanvraag.  | Foutbericht
<br>
## <a name="example-calculate-similarity-of-two-partial-abstracts"></a>Voorbeeld: Gelijkenis van twee gedeeltelijke samenvattingen berekenen
#### <a name="request"></a>Aanvraag:
```
https://westus.api.cognitive.microsoft.com/academic/v1.0/similarity?s1=Using complementary priors, we derive a fast greedy algorithm that can learn deep directed belief networks one layer at a time, provided the top two layers form an undirected associative memory
&s2=Deepneural nets with a large number of parameters are very powerful machine learning systems. However, overfitting is a serious problem in such networks
```
In dit voorbeeld genereren we de score overeenkomsten tussen de twee gedeeltelijke samenvattingen met behulp van de **gelijkenis** API.
#### <a name="response"></a>Reactie:
```
0.520
```
#### <a name="remarks"></a>Opmerking:
De score gelijkenis wordt bepaald door het beoordelen van de academische concepten via word insluiten. In dit voorbeeld betekent 0.52 dat de twee gedeeltelijke samenvattingen enigszins vergelijkbaar zijn.
<br>
