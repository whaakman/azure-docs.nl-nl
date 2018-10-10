---
title: Methode gelijkenis - Academic Knowledge API
titlesuffix: Azure Cognitive Services
description: Gebruik de methode overeenkomsten voor het berekenen van de academische gelijkenis van twee tekenreeksen.
services: cognitive-services
author: alch-msft
manager: cgronlun
ms.service: cognitive-services
ms.component: academic-knowledge
ms.topic: conceptual
ms.date: 01/18/2017
ms.author: alch
ms.openlocfilehash: 76e86eb78a06d98e3d5c6c54b244add3c0c245d2
ms.sourcegitcommit: 7824e973908fa2edd37d666026dd7c03dc0bafd0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/10/2018
ms.locfileid: "48900458"
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
**S1**        |Reeks   |Ja  |Tekenreeks * moet worden vergeleken
**S2**        |Reeks   |Ja  |Tekenreeks * moet worden vergeleken
<sub> * Tekenreeksen vergelijken, hebben een maximum lengte van 1MB. </sub>
<br>
## <a name="response"></a>Antwoord
Naam | Beschrijving
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