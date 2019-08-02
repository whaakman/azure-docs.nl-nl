---
title: Entiteits kenmerken van de vergaderings reeks-Academic Knowledge API
titlesuffix: Azure Cognitive Services
description: Meer informatie over de kenmerken die u kunt gebruiken met de entiteit vergaderingen reeks.
services: cognitive-services
author: alch-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/23/2017
ms.author: alch
ROBOTS: NOINDEX
ms.openlocfilehash: feed324202f6a75ceb7e9089875b899c51cd8ae6
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2019
ms.locfileid: "68705048"
---
# <a name="conference-series-entity"></a>Entiteit van de vergaderings reeks

<sub>* De volgende kenmerken zijn specifiek voor de entiteit van de vergaderings reeks. (Ty = ' 3 ')</sub>

Name    |Description                            |type       | Bewerkingen
------- | ------------------------------------- | --------- | ----------------------------
ID      |Entiteits-id                              |Int64      |Is gelijk aan
CN      |Genormaliseerde naam van conferentie reeks      |Tekenreeks     |Is gelijk aan
DCN     |Weergave naam van de reeks vergaderingen         |Reeks     |geen
CC      |Aantal opgenomen vergaderingen in de vergaderings reeks         |Int32      |geen  
ECC     |Aantal conferentie reeksen aantal geschatte citaten   |Int32      |geen
F.FId   |Veld met de ID van de studie-entiteit die is gekoppeld aan de reeks vergaderingen |Int64  | Is gelijk aan
F.FN    |Veld met de naam van het onderzoek dat is gekoppeld aan de reeks vergaderingen  | Gelijk is aan<br/>StartsWith
