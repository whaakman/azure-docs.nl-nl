---
title: Kenmerken van de relatie-entiteit in de Academic Knowledge API
titlesuffix: Azure Cognitive Services
description: Meer informatie over de kenmerken die u met de relatie-entiteit in de Academic Knowledge API gebruiken kunt.
services: cognitive-services
author: alch-msft
manager: cgronlun
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/23/2017
ms.author: alch
ms.openlocfilehash: 344b26b16f74cd44982e3c93fa69295792daa9a0
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55190624"
---
# <a name="affiliation-entity"></a>Relatie-entiteit

<sub> * De volgende kenmerken zijn specifiek voor de relatie-entiteit. (Ty = '5') </sub>

Name    |Description                            |Type       | Bewerkingen
------- | ------------------------------------- | --------- | ----------------------------
Id      |Entiteits-id                              |Int64      |Is gelijk aan
AfN     |Relatie genormaliseerde naam        |Reeks     |Is gelijk aan
DAfN    |Weergavenaam van de relatie       |Reeks     |geen
CC      |Relatie totale citaat tellen           |Int32      |geen  
ECC     |Relatie totale geschatte citaat tellen |Int32      |geen

## <a name="extended-metadata-attributes"></a>Uitgebreide metagegevens kenmerken ##

Name    | Description               
--------|---------------------------    
Pc      |Lidmaatschap van papier tellen
