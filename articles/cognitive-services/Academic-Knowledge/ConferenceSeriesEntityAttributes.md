---
title: Conferentie reeks entiteitskenmerken - Academic Knowledge API
titlesuffix: Azure Cognitive Services
description: Meer informatie over de kenmerken die u met de entiteit Conferentiereeks gebruiken kunt.
services: cognitive-services
author: alch-msft
manager: cgronlun
ms.service: cognitive-services
ms.component: academic-knowledge
ms.topic: conceptual
ms.date: 03/23/2017
ms.author: alch
ms.openlocfilehash: f9f28afd7005d7a61aa0d2f4dba69ca598034b52
ms.sourcegitcommit: 7824e973908fa2edd37d666026dd7c03dc0bafd0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/10/2018
ms.locfileid: "48900745"
---
# <a name="conference-series-entity"></a>Conferentie reeks entiteit

<sub> * De volgende kenmerken zijn specifiek voor de Conferentie reeks entiteit. (Ty = "3") </sub>

Naam    |Beschrijving                            |Type       | Bewerkingen
------- | ------------------------------------- | --------- | ----------------------------
Id      |Entiteits-id                              |Int64      |Is gelijk aan
ALGEMENE NAAM      |Conferentiereeks genormaliseerd naam      |Reeks     |Is gelijk aan
DCN     |Weergavenaam conferentie-serie         |Reeks     |geen
CC      |Conferentiereeks totaal aantal citaat         |Int32      |geen  
ECC     |Conferentie reeks totale geschatte citaat tellen   |Int32      |geen
F.FId   |Veld van het onderzoek entiteit-ID die is gekoppeld aan de conferentiereeks |Int64  | Is gelijk aan
F.FN    |Veld van het onderzoek naar de naam die is gekoppeld aan de conferentiereeks  | Is gelijk aan,<br/>StartsWith