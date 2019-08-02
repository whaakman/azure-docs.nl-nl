---
title: Veld met kenmerken van de studie-entiteit-Academic Knowledge API
titlesuffix: Azure Cognitive Services
description: Meer informatie over de kenmerken die u kunt gebruiken met het veld van de studie entiteit in de Academic Knowledge API.
services: cognitive-services
author: alch-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/31/2017
ms.author: alch
ROBOTS: NOINDEX
ms.openlocfilehash: d54f3c72462e6702b09068092b7c18ea50f12048
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2019
ms.locfileid: "68704997"
---
# <a name="field-of-study-entity"></a>Veld van de studie entiteit

<sub>* De volgende kenmerken zijn specifiek voor het veld van de Study-entiteit. (Ty = '6') </sub>

Name    |Description                            |type       | Bewerkingen
------- | ------------------------------------- | --------- | ----------------------------
ID      |Entiteits-id                              |Int64      |Is gelijk aan
FN      |Veld van genormaliseerde studie naam         |Tekenreeks     |Is gelijk aan
DFN     |Veld met de weergave naam voor het onderzoek            |Tekenreeks     |geen
CC      |Veld van het totale aantal citaten van studie    |Int32      |geen  
ECC     |Veld van totaal aantal geschatte citaten|Int32      |geen
FL      |Niveau in velden van de studie hiërarchie     |Int32      |Gelijk is aan <br/>IsBetween
FP.FN   |Bovenliggend veld van studie naam             |Tekenreeks     |Is gelijk aan
FP.FId  |Bovenliggend veld van studie-ID               |Int64      |Is gelijk aan
FC.FN   |Onderliggend veld van studie naam              |Tekenreeks     |Is gelijk aan
FC.FId  |Onderliggend veld van studie-ID                |Int64      |Is gelijk aan
