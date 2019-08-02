---
title: Entiteits kenmerken voor academische grafiek-Academic Knowledge API
titlesuffix: Azure Cognitive Services
description: Meer informatie over de entiteits kenmerken die u kunt gebruiken met de academische grafiek in de Academic Knowledge API.
services: cognitive-services
author: alch-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/27/2017
ms.author: alch
ROBOTS: NOINDEX
ms.openlocfilehash: 596ce35a0d744691edd75d6071d4758691e0031b
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2019
ms.locfileid: "68705021"
---
# <a name="entity-attributes"></a>Entiteits kenmerken

Het academische diagram bestaat uit 7 typen entiteit. Alle entiteiten hebben een entiteit-ID en een entiteits type.

## <a name="common-entity-attributes"></a>Algemene entiteits kenmerken
Name    |Description                |type       | Bewerkingen
------- | ------------------------- | --------- | ----------------------------
ID      |Entiteits-id                  |Int64      |Is gelijk aan
Ty      |Entiteitstype                |enum   |Is gelijk aan

## <a name="entity-type-enum"></a>Enum van entiteits type
Name                                                            |value
----------------------------------------------------------------|-----
[Publicatie](PaperEntityAttributes.md)                               |0
[Auteur](AuthorEntityAttributes.md)                             |1
[Tijdschrift](JournalEntityAttributes.md)                           |2
[Reeks vergaderingen](JournalEntityAttributes.md)                 |3
[Conference-instantie](ConferenceInstanceEntityAttributes.md)    |4
[Relatie](AffiliationEntityAttributes.md)                   |5
[Veld van studie](FieldsOfStudyEntityAttributes.md)                      |6

