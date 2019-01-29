---
title: Entiteitkenmerken veld van studie - Academic Knowledge API
titlesuffix: Azure Cognitive Services
description: Meer informatie over de kenmerken die u met de entiteit veld van onderzoek in de Academic Knowledge API gebruiken kunt.
services: cognitive-services
author: alch-msft
manager: cgronlun
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/31/2017
ms.author: alch
ms.openlocfilehash: 793b35d9c6412c40a87f3f91fcd772476d57584f
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55154564"
---
# <a name="field-of-study-entity"></a>Veld van het onderzoek naar entiteit

<sub> * De volgende kenmerken zijn specifiek voor het veld van het onderzoek entiteit. (Ty = '6') </sub>

Name    |Description                            |Type       | Bewerkingen
------- | ------------------------------------- | --------- | ----------------------------
Id      |Entiteits-id                              |Int64      |Is gelijk aan
FN      |Veld van genormaliseerde studie-naam         |Reeks     |Is gelijk aan
DFN     |Veld studie-weergavenaam            |Reeks     |geen
CC      |Veld van het onderzoek totale citaat aantal    |Int32      |geen  
ECC     |Veld van het aantal totale geschatte citaat|Int32      |geen
FL      |Velden van de hiërarchie studie-niveau     |Int32      |Is gelijk aan, <br/>IsBetween
FP.FN   |Bovenliggende veld van de naam van het onderzoek             |Reeks     |Is gelijk aan
FP.FId  |Bovenliggende veld van de ID van het onderzoek               |Int64      |Is gelijk aan
FC.FN   |Onderliggende veld van de naam van het onderzoek              |Reeks     |Is gelijk aan
FC.FId  |Onderliggende veld van de ID van het onderzoek                |Int64      |Is gelijk aan
