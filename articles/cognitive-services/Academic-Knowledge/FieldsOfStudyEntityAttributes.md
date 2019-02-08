---
title: Entiteitkenmerken veld van studie - Academic Knowledge API
titlesuffix: Azure Cognitive Services
description: Meer informatie over de kenmerken die u met de entiteit veld van onderzoek in de Academic Knowledge API gebruiken kunt.
services: cognitive-services
author: alch-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/31/2017
ms.author: alch
ms.openlocfilehash: e9d6badf76efd03c0520a728af7b3e47b25f200a
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55878949"
---
# <a name="field-of-study-entity"></a>Veld van het onderzoek naar entiteit

<sub> * De volgende kenmerken zijn specifiek voor het veld van het onderzoek entiteit. (Ty = '6') </sub>

Name    |Description                            |Type       | Bewerkingen
------- | ------------------------------------- | --------- | ----------------------------
Id      |Entiteits-id                              |Int64      |Is gelijk aan
FN      |Veld van genormaliseerde studie-naam         |String     |Is gelijk aan
DFN     |Veld studie-weergavenaam            |String     |geen
CC      |Veld van het onderzoek totale citaat aantal    |Int32      |geen  
ECC     |Veld van het aantal totale geschatte citaat|Int32      |geen
FL      |Velden van de hiërarchie studie-niveau     |Int32      |Is gelijk aan, <br/>IsBetween
FP.FN   |Bovenliggende veld van de naam van het onderzoek             |String     |Is gelijk aan
FP.FId  |Bovenliggende veld van de ID van het onderzoek               |Int64      |Is gelijk aan
FC.FN   |Onderliggende veld van de naam van het onderzoek              |String     |Is gelijk aan
FC.FId  |Onderliggende veld van de ID van het onderzoek                |Int64      |Is gelijk aan
