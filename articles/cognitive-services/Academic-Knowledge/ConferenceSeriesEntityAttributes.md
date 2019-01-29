---
title: Conferentie reeks entiteitskenmerken - Academic Knowledge API
titlesuffix: Azure Cognitive Services
description: Meer informatie over de kenmerken die u met de entiteit Conferentiereeks gebruiken kunt.
services: cognitive-services
author: alch-msft
manager: cgronlun
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/23/2017
ms.author: alch
ms.openlocfilehash: ff71b489cce01d8d6ea29e09905d7d3ac8429e34
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55155595"
---
# <a name="conference-series-entity"></a>Conferentie reeks entiteit

<sub> * De volgende kenmerken zijn specifiek voor de Conferentie reeks entiteit. (Ty = '3') </sub>

Name    |Description                            |Type       | Bewerkingen
------- | ------------------------------------- | --------- | ----------------------------
Id      |Entiteits-id                              |Int64      |Is gelijk aan
CN      |Conferentiereeks genormaliseerd naam      |Reeks     |Is gelijk aan
DCN     |Weergavenaam conferentie-serie         |Reeks     |geen
CC      |Conferentiereeks totaal aantal citaat         |Int32      |geen  
ECC     |Conferentie reeks totale geschatte citaat tellen   |Int32      |geen
F.FId   |Veld van het onderzoek entiteit-ID die is gekoppeld aan de conferentiereeks |Int64  | Is gelijk aan
F.FN    |Veld van het onderzoek naar de naam die is gekoppeld aan de conferentiereeks  | Is gelijk aan,<br/>StartsWith
