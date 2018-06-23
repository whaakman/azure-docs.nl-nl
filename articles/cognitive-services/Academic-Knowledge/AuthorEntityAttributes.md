---
title: Schrijven van entiteitskenmerken in de Academic Knowledge API | Microsoft Docs
description: Meer informatie over de kenmerken die u met de auteur entiteit in de Academic Knowledge API in cognitieve Services gebruiken kunt.
services: cognitive-services
author: alch-msft
manager: kuansanw
ms.service: cognitive-services
ms.component: academic-knowledge
ms.topic: article
ms.date: 03/23/2017
ms.author: alch
ms.openlocfilehash: afcc3257c49522a4631c4aae0e0c2b88373f9af1
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344398"
---
# <a name="author-entity"></a>De auteur van entiteit
<sub> * De volgende kenmerken zijn specifiek voor de auteur van entiteit. (Ty = '1') </sub>

Naam    |Beschrijving                            |Type       | Bewerkingen
------- | ------------------------------------- | --------- | ----------------------------
Id      |Entiteits-id                              |Int64      |Is gelijk aan
AuN     |De genormaliseerde auteursnaam                 |Reeks     |Is gelijk aan
Daum    |Weergavenaam van de auteur                    |Reeks     |geen
CC      |Telling van de auteur van totale instantiedocumentcitaat            |Int32      |geen  
ECC     |Aantal van de auteur van totale geschatte instantiedocumentcitaat  |Int32      |geen
E       |Uitgebreide metagegevens (Zie de tabel "Uitgebreide Meta kenmerken")  |Reeks     |geen  


## <a name="extended-metadata-attributes"></a>Uitgebreide Metagegevenskenmerken ##

Naam    | Beschrijving               
--------|---------------------------    
LKA. Afn     | weergavenaam van de relatie die zijn gekoppeld aan de auteur  
LKA. AfId        | lidmaatschap van de entiteit-ID die is gekoppeld aan de auteur