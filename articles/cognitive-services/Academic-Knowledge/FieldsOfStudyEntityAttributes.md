---
title: Veld van uitgevoerde studie entiteitskenmerken in de Academic Knowledge API | Microsoft Docs
description: Meer informatie over de kenmerken die u met het veld van uitgevoerde studie entiteit in de Academic Knowledge API in cognitieve Services gebruiken kunt.
services: cognitive-services
author: alch-msft
manager: kuansanw
ms.service: cognitive-services
ms.component: academic-knowledge
ms.topic: article
ms.date: 03/31/2017
ms.author: alch
ms.openlocfilehash: a8176370f5b2f63b7741f7e892ed5a8c775f19c1
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344400"
---
# <a name="field-of-study-entity"></a>Veld van uitgevoerde studie entiteit

<sub> * De volgende kenmerken zijn specifiek voor veld van uitgevoerde studie entiteit. (Ty = '6') </sub>

Naam    |Beschrijving                            |Type       | Bewerkingen
------- | ------------------------------------- | --------- | ----------------------------
Id      |Entiteits-id                              |Int64      |Is gelijk aan
FN      |Veld van genormaliseerde naam onderzoek         |Reeks     |Is gelijk aan
DFN     |Veld van uitgevoerde studie weergavenaam            |Reeks     |geen
CC      |Veld van het onderzoek instantiedocumentcitaat totaal aantal    |Int32      |geen  
ECC     |Veld van het aantal totale geschatte instantiedocumentcitaat|Int32      |geen
FL      |Niveau in de velden van uitgevoerde studie hiërarchie     |Int32      |Is gelijk aan, <br/>IsBetween
FRONTPAGE. FN   |Bovenliggende veld van uitgevoerde studie naam             |Reeks     |Is gelijk aan
FRONTPAGE. Bestands-id  |Bovenliggende veld van de ID van het onderzoek               |Int64      |Is gelijk aan
FC. FN   |Onderliggende veld van de naam van de uitgevoerde studie              |Reeks     |Is gelijk aan
FC. Bestands-id  |Onderliggende veld van de ID van het onderzoek                |Int64      |Is gelijk aan