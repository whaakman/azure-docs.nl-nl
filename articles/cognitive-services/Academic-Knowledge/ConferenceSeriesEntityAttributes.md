---
title: Conferentie reeks entiteitskenmerken - Academic Knowledge API
titlesuffix: Azure Cognitive Services
description: Meer informatie over de kenmerken die u met de entiteit Conferentiereeks gebruiken kunt.
services: cognitive-services
author: alch-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/23/2017
ms.author: alch
ms.openlocfilehash: 38b4aa4c899668a68041f042ce6981ddd8c58219
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55884916"
---
# <a name="conference-series-entity"></a>Conferentie reeks entiteit

<sub> * De volgende kenmerken zijn specifiek voor de Conferentie reeks entiteit. (Ty = '3') </sub>

Name    |Description                            |Type       | Bewerkingen
------- | ------------------------------------- | --------- | ----------------------------
Id      |Entiteits-id                              |Int64      |Is gelijk aan
CN      |Conferentiereeks genormaliseerd naam      |String     |Is gelijk aan
DCN     |Weergavenaam conferentie-serie         |String     |geen
CC      |Conferentiereeks totaal aantal citaat         |Int32      |geen  
ECC     |Conferentie reeks totale geschatte citaat tellen   |Int32      |geen
F.FId   |Veld van het onderzoek entiteit-ID die is gekoppeld aan de conferentiereeks |Int64  | Is gelijk aan
F.FN    |Veld van het onderzoek naar de naam die is gekoppeld aan de conferentiereeks  | Is gelijk aan,<br/>StartsWith
