---
title: De auteur van entiteitskenmerken - Academic Knowledge API
titlesuffix: Azure Cognitive Services
description: Meer informatie over de kenmerken die u met de auteur van entiteit in de Academic Knowledge API gebruiken kunt.
services: cognitive-services
author: alch-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/23/2017
ms.author: alch
ms.openlocfilehash: d4b33c06ab023023aadf403cf0ef0b08c2bafc5f
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55878932"
---
# <a name="author-entity"></a>De auteur van entiteit
<sub> * De volgende kenmerken zijn specifiek voor de auteur van entiteit. (Ty = '1') </sub>

Name    |Description                            |Type       | Bewerkingen
------- | ------------------------------------- | --------- | ----------------------------
Id      |Entiteits-id                              |Int64      |Is gelijk aan
AuN     |Genormaliseerde naam van auteur                 |String     |Is gelijk aan
DAuN    |Weergavenaam van de auteur                    |String     |geen
CC      |Totale citaat aantal van auteur            |Int32      |geen  
ECC     |Totale geschatte citaat aantal van auteur  |Int32      |geen
E       |Uitgebreide metagegevens (Zie tabel "Uitgebreide metagegevens kenmerken")  |String     |geen  


## <a name="extended-metadata-attributes"></a>Uitgebreide metagegevens kenmerken ##

Name    | Description               
--------|---------------------------    
LKA. Afn     | de weergavenaam van de relatie die is gekoppeld aan de auteur van de  
LKA.AfId        | van de relatie die is gekoppeld aan de auteur van de entiteit-ID
