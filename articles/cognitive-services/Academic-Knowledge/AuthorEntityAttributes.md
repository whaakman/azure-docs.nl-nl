---
title: De auteur van entiteitskenmerken - Academic Knowledge API
titlesuffix: Azure Cognitive Services
description: Meer informatie over de kenmerken die u met de auteur van entiteit in de Academic Knowledge API gebruiken kunt.
services: cognitive-services
author: alch-msft
manager: cgronlun
ms.service: cognitive-services
ms.component: academic-knowledge
ms.topic: conceptual
ms.date: 03/23/2017
ms.author: alch
ms.openlocfilehash: 62e6da6d558a0494fb83115b1e307081099399d4
ms.sourcegitcommit: 7824e973908fa2edd37d666026dd7c03dc0bafd0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/10/2018
ms.locfileid: "48900683"
---
# <a name="author-entity"></a>De auteur van entiteit
<sub> * De volgende kenmerken zijn specifiek voor de auteur van entiteit. (Ty = '1') </sub>

Naam    |Beschrijving                            |Type       | Bewerkingen
------- | ------------------------------------- | --------- | ----------------------------
Id      |Entiteits-id                              |Int64      |Is gelijk aan
AuN     |Genormaliseerde naam van auteur                 |Reeks     |Is gelijk aan
DAuN    |Weergavenaam van de auteur                    |Reeks     |geen
CC      |Totale citaat aantal van auteur            |Int32      |geen  
ECC     |Totale geschatte citaat aantal van auteur  |Int32      |geen
E       |Uitgebreide metagegevens (Zie tabel "Uitgebreide metagegevens kenmerken")  |Reeks     |geen  


## <a name="extended-metadata-attributes"></a>Uitgebreide metagegevens kenmerken ##

Naam    | Beschrijving               
--------|---------------------------    
LKA. Afn     | de weergavenaam van de relatie die is gekoppeld aan de auteur van de  
LKA. AfId        | van de relatie die is gekoppeld aan de auteur van de entiteit-ID