---
title: Academic grafiek entiteitskenmerken voor de Academic Knowledge API | Microsoft Docs
description: Meer informatie over de entiteitskenmerken die u met de Academic grafiek in de Academic Knowledge API gebruiken kunt.
services: cognitive-services
author: alch-msft
manager: kuansanw
ms.service: cognitive-services
ms.component: academic-knowledge
ms.topic: article
ms.date: 03/27/2017
ms.author: alch
ms.openlocfilehash: f771969c3c6f05e5d2c99b1fbf593ae039ccb12c
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344396"
---
# <a name="entity-attributes"></a>Entiteitskenmerken

De grafiek academic bestaat uit 7 soorten entiteit. Alle entiteiten hebben een entiteit-ID en een entiteitstype.

## <a name="common-entity-attributes"></a>Gangbare entiteitskenmerken
Naam    |Beschrijving                |Type       | Bewerkingen
------- | ------------------------- | --------- | ----------------------------
Id      |Entiteits-id                  |Int64      |Is gelijk aan
Ty      |Entiteitstype                |Enum   |Is gelijk aan

## <a name="entity-type-enum"></a>Entiteit type enum
Naam                                                            |waarde
----------------------------------------------------------------|-----
[Publicatie](PaperEntityAttributes.md)                               |0
[Auteur](AuthorEntityAttributes.md)                             |1
[Tijdschrift](JournalEntityAttributes.md)                           |2
[Telefonische vergaderingen reeks](JournalEntityAttributes.md)                 |3
[Telefonische vergaderingen exemplaar](ConferenceInstanceEntityAttributes.md)    |4
[Relatie](AffiliationEntityAttributes.md)                   |5
[Veld van onderzoek](FieldsOfStudyEntityAttributes.md)                      |6

