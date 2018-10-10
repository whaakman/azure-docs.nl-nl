---
title: Entiteitkenmerken veld van studie - Academic Knowledge API
titlesuffix: Azure Cognitive Services
description: Meer informatie over de kenmerken die u met de entiteit veld van onderzoek in de Academic Knowledge API gebruiken kunt.
services: cognitive-services
author: alch-msft
manager: cgronlun
ms.service: cognitive-services
ms.component: academic-knowledge
ms.topic: conceptual
ms.date: 03/31/2017
ms.author: alch
ms.openlocfilehash: 862fd6d506d5f1ca6f7f532f80f53a29200f33db
ms.sourcegitcommit: 7824e973908fa2edd37d666026dd7c03dc0bafd0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/10/2018
ms.locfileid: "48900424"
---
# <a name="field-of-study-entity"></a>Veld van het onderzoek naar entiteit

<sub> * De volgende kenmerken zijn specifiek voor het veld van het onderzoek entiteit. (Ty = '6') </sub>

Naam    |Beschrijving                            |Type       | Bewerkingen
------- | ------------------------------------- | --------- | ----------------------------
Id      |Entiteits-id                              |Int64      |Is gelijk aan
FN      |Veld van genormaliseerde studie-naam         |Reeks     |Is gelijk aan
DFN     |Veld studie-weergavenaam            |Reeks     |geen
CC      |Veld van het onderzoek totale citaat aantal    |Int32      |geen  
ECC     |Veld van het aantal totale geschatte citaat|Int32      |geen
FL      |Velden van de hiërarchie studie-niveau     |Int32      |Is gelijk aan, <br/>IsBetween
FP. FN   |Bovenliggende veld van de naam van het onderzoek             |Reeks     |Is gelijk aan
FP. Bestands-id  |Bovenliggende veld van de ID van het onderzoek               |Int64      |Is gelijk aan
FC. FN   |Onderliggende veld van de naam van het onderzoek              |Reeks     |Is gelijk aan
FC. Bestands-id  |Onderliggende veld van de ID van het onderzoek                |Int64      |Is gelijk aan