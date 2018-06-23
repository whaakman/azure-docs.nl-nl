---
title: De methode overeenkomsten in de Academic Knowledge API | Microsoft Docs
description: Gebruik de methode gelijkenis voor het berekenen van de academic overeenkomsten van twee tekenreeksen in cognitieve Microsoft-Services.
services: cognitive-services
author: alch-msft
manager: kuansanw
ms.service: cognitive-services
ms.component: academic-knowledge
ms.topic: article
ms.date: 01/18/2017
ms.author: alch
ms.openlocfilehash: 472498d6bfe06ae4477a30f892d44e79c901acf5
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344411"
---
# <a name="similarity-method"></a>De methode overeenkomsten

De **gelijkenis** REST-API wordt gebruikt voor het berekenen van de academic overeenkomsten tussen de twee tekenreeksen. 
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
**SimilarityScore**        |Een drijvende-kommawaarde die de cosinus overeenkomsten van s1 en s2 met waarden dichter 1.0 zin meer lijken en -waarden dichter naar-1.0 betekenis kleiner vertegenwoordigt
<br>

## <a name="successerror-conditions"></a>Geslaagde of mislukte voorwaarden
HTTP-Status | Reden | Antwoord
-----------|----------|--------
**200**         |Geslaagd | Drijvendekommagetal
**400**         | Onjuiste aanvraag of de aanvraag ongeldig | Foutbericht      
**500**         |Interne serverfout | Foutbericht
**Time-out**     | Er is een time-out opgetreden voor de aanvraag.  | Foutbericht
<br>
## <a name="example-calculate-similarity-of-two-partial-abstracts"></a>Voorbeeld: Overeenkomsten van twee gedeeltelijke samenvattingen berekenen
#### <a name="request"></a>Aanvraag:
```
https://westus.api.cognitive.microsoft.com/academic/v1.0/similarity?s1=Using complementary priors, we derive a fast greedy algorithm that can learn deep directed belief networks one layer at a time, provided the top two layers form an undirected associative memory
&s2=Deepneural nets with a large number of parameters are very powerful machine learning systems. However, overfitting is a serious problem in such networks
```
In dit voorbeeld wordt de score overeenkomsten tussen twee gedeeltelijke samenvattingen met genereren de **gelijkenis** API.
#### <a name="response"></a>Antwoord:
```
0.520
```
#### <a name="remarks"></a>Opmerkingen:
De score gelijkenis wordt bepaald door de beoordeling van de academic concepten via word insluiten. In dit voorbeeld betekent 0.52 dat de twee gedeeltelijke samenvattingen enigszins vergelijkbaar zijn.
<br>