---
title: Entiteits kenmerken van auteur-Academic Knowledge API
titlesuffix: Azure Cognitive Services
description: Meer informatie over de kenmerken die u kunt gebruiken met de entiteit auteur in de Academic Knowledge API.
services: cognitive-services
author: alch-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/23/2017
ms.author: alch
ROBOTS: NOINDEX
ms.openlocfilehash: e63e9d3f8f17a2473caf3d31b83e318ddb132b43
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2019
ms.locfileid: "68705095"
---
# <a name="author-entity"></a>Entiteit Auteur
<sub>* De volgende kenmerken zijn specifiek voor de auteur-entiteit. (Ty = ' 1 ')</sub>

Name    |Description                            |type       | Bewerkingen
------- | ------------------------------------- | --------- | ----------------------------
ID      |Entiteits-id                              |Int64      |Is gelijk aan
AuN     |Genormaliseerde naam schrijven                 |Reeks     |Is gelijk aan
DAuN    |Weergave naam van auteur                    |Tekenreeks     |geen
CC      |Totaal aantal bron vermeldingen van auteur            |Int32      |geen  
ECC     |Totaal aantal geschatte bron vermeldingen van auteur  |Int32      |geen
E       |Uitgebreide meta gegevens (Zie de tabel uitgebreide meta-kenmerken)  |Tekenreeks     |geen  


## <a name="extended-metadata-attributes"></a>Uitgebreide meta gegevens kenmerken ##

Name    | Description               
--------|---------------------------    
LKA. Afn     | weergave naam van de relatie die aan de auteur is gekoppeld  
LKA. AfId        | Entiteits-ID van relatie gekoppeld aan de auteur
