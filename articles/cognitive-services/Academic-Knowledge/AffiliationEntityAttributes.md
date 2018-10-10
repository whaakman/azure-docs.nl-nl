---
title: Kenmerken van de relatie-entiteit in de Academic Knowledge API
titlesuffix: Azure Cognitive Services
description: Meer informatie over de kenmerken die u met de relatie-entiteit in de Academic Knowledge API gebruiken kunt.
services: cognitive-services
author: alch-msft
manager: cgronlun
ms.service: cognitive-services
ms.component: academic-knowledge
ms.topic: conceptual
ms.date: 03/23/2017
ms.author: alch
ms.openlocfilehash: 177fe9da8bbe821a69eae02d89a225e5d4009331
ms.sourcegitcommit: 7824e973908fa2edd37d666026dd7c03dc0bafd0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/10/2018
ms.locfileid: "48900475"
---
# <a name="affiliation-entity"></a>Relatie-entiteit

<sub> * De volgende kenmerken zijn specifiek voor de relatie-entiteit. (Ty = '5') </sub>

Naam    |Beschrijving                            |Type       | Bewerkingen
------- | ------------------------------------- | --------- | ----------------------------
Id      |Entiteits-id                              |Int64      |Is gelijk aan
AfN     |Relatie genormaliseerde naam        |Reeks     |Is gelijk aan
DAfN    |Weergavenaam van de relatie       |Reeks     |geen
CC      |Relatie totale citaat tellen           |Int32      |geen  
ECC     |Relatie totale geschatte citaat tellen |Int32      |geen

## <a name="extended-metadata-attributes"></a>Uitgebreide metagegevens kenmerken ##

Naam    | Beschrijving               
--------|---------------------------    
Pc      |Lidmaatschap van papier tellen