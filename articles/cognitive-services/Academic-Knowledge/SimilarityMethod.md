---
title: Soort gelijke methode-Academic Knowledge API
titlesuffix: Azure Cognitive Services
description: Gebruik de soort gelijke methode om de academische gelijkenis van twee teken reeksen te berekenen.
services: cognitive-services
author: alch-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 01/18/2017
ms.author: alch
ROBOTS: NOINDEX
ms.openlocfilehash: 855d29d2c55b841bbbe4e9eadce8c29ad85fad90
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2019
ms.locfileid: "68704856"
---
# <a name="similarity-method"></a>Soort gelijke methode

De **soort gelijke** rest API wordt gebruikt voor het berekenen van de vergelijk bare academiciteit tussen twee teken reeksen. 
<br>

**REST-eind punt:**
```
https://westus.api.cognitive.microsoft.com/academic/v1.0/similarity?
```

## <a name="request-parameters"></a>Aanvraagparameters

Parameter        |Gegevenstype      |Vereist | Description
----------|----------|----------|------------
**s1**        |Reeks   |Ja  |Teken reeks * die moet worden vergeleken
**s2**        |Tekenreeks   |Ja  |Teken reeks * die moet worden vergeleken

<sub>* Teken reeksen die moeten worden vergeleken, hebben een maximale lengte van 1 MB.</sub>
<br>

## <a name="response"></a>Antwoord

Name | Description
--------|---------
**SimilarityScore**        |Een drijvende-komma waarde die de cosinus gelijkenis van S1 en S2 vertegenwoordigt, met waarden dichter tot en met 1,0, wat gelijk is aan en waarden dichter bij-1,0 betekent minder

<br>

## <a name="successerror-conditions"></a>Geslaagde/fout voorwaarden

HTTP-status | Reason | Antwoord
-----------|----------|--------
**200**         |Geslaagd | Getal met drijvende komma
**400**         | Ongeldige aanvraag of aanvraag ongeldig | Foutbericht      
**500**         |Er is een interne serverfout opgetreden | Foutbericht
**Time-out opgetreden**     | Er is een time-out opgetreden voor de aanvraag.  | Foutbericht

<br>

## <a name="example-calculate-similarity-of-two-partial-abstracts"></a>Voorbeeld: Gelijkenis van twee gedeeltelijke samen vattingen berekenen
#### <a name="request"></a>Aanvraag:
```
https://westus.api.cognitive.microsoft.com/academic/v1.0/similarity?s1=Using complementary priors, we derive a fast greedy algorithm that can learn deep directed belief networks one layer at a time, provided the top two layers form an undirected associative memory
&s2=Deepneural nets with a large number of parameters are very powerful machine learning systems. However, overfitting is a serious problem in such networks
```
In dit voor beeld genereren we de Score van het soort gelijkeheid tussen twee gedeeltelijke samen vattingen met behulp van de **soort gelijke** API.
#### <a name="response"></a>Antwoord:
```
0.520
```
#### <a name="remarks"></a>Opmerkingen
De Score van het soort gelijkeheid wordt bepaald door de academische concepten te beoordelen door middel van woord insluiting. In dit voor beeld betekent 0,52 dat de twee gedeeltelijke samen vattingen enigszins vergelijkbaar zijn.
<br>
