---
title: Kenmerken van relatie-entiteit in de Academic Knowledge API
titlesuffix: Azure Cognitive Services
description: Meer informatie over de kenmerken die u met de relatie-entiteit kunt gebruiken in de Academic Knowledge API.
services: cognitive-services
author: alch-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/23/2017
ms.author: alch
ROBOTS: NOINDEX
ms.openlocfilehash: 333875472d9b859196c4d828061301b9d32c4d5a
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2019
ms.locfileid: "68705117"
---
# <a name="affiliation-entity"></a>Relatie-entiteit

<sub>* De volgende kenmerken zijn specifiek voor de relatie-entiteit. (Ty = '5') </sub>

Name    |Description                            |type       | Bewerkingen
------- | ------------------------------------- | --------- | ----------------------------
ID      |Entiteits-id                              |Int64      |Is gelijk aan
AfN     |Genormaliseerde naam van relatie        |Tekenreeks     |Is gelijk aan
DAfN    |Weergave naam van relatie       |Tekenreeks     |geen
CC      |Totaal aantal bron vermeldingen van relatie           |Int32      |geen  
ECC     |Totaal aantal geschatte bron vermeldingen van relatie |Int32      |geen

## <a name="extended-metadata-attributes"></a>Uitgebreide meta gegevens kenmerken ##

Name    | Description               
--------|---------------------------    
Pc      |Aantal vellen van relatie
