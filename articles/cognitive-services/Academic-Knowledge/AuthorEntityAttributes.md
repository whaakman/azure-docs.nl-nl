---
title: De auteur van entiteitskenmerken - Academic Knowledge API
titlesuffix: Azure Cognitive Services
description: Meer informatie over de kenmerken die u met de auteur van entiteit in de Academic Knowledge API gebruiken kunt.
services: cognitive-services
author: alch-msft
manager: cgronlun
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/23/2017
ms.author: alch
ms.openlocfilehash: 48758ac9ec8c993bbdb490229ae20fcce1fb0a49
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55175176"
---
# <a name="author-entity"></a>De auteur van entiteit
<sub> * De volgende kenmerken zijn specifiek voor de auteur van entiteit. (Ty = '1') </sub>

Name    |Description                            |Type       | Bewerkingen
------- | ------------------------------------- | --------- | ----------------------------
Id      |Entiteits-id                              |Int64      |Is gelijk aan
AuN     |Genormaliseerde naam van auteur                 |Reeks     |Is gelijk aan
DAuN    |Weergavenaam van de auteur                    |Reeks     |geen
CC      |Totale citaat aantal van auteur            |Int32      |geen  
ECC     |Totale geschatte citaat aantal van auteur  |Int32      |geen
E       |Uitgebreide metagegevens (Zie tabel "Uitgebreide metagegevens kenmerken")  |Reeks     |geen  


## <a name="extended-metadata-attributes"></a>Uitgebreide metagegevens kenmerken ##

Name    | Beschrijving               
--------|---------------------------    
LKA. Afn     | de weergavenaam van de relatie die is gekoppeld aan de auteur van de  
LKA.AfId        | van de relatie die is gekoppeld aan de auteur van de entiteit-ID
