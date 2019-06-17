---
title: Entiteitkenmerken Academic Graph - Academic Knowledge API
titlesuffix: Azure Cognitive Services
description: Meer informatie over de kenmerken van de entiteit die kunt u met de Academic Graph in de Academic Knowledge API.
services: cognitive-services
author: alch-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/27/2017
ms.author: alch
ms.openlocfilehash: 485775660ecfdf2291365ab98c9188295ea2cbde
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "61340176"
---
# <a name="entity-attributes"></a>Entiteitkenmerken

De academic graph van 7 typen entiteit bestaat. Alle entiteiten hebben een entiteits-ID en een entiteitstype.

## <a name="common-entity-attributes"></a>Algemene entiteitskenmerken
Name    |Description                |Type       | Bewerkingen
------- | ------------------------- | --------- | ----------------------------
Id      |Entiteit-ID                  |Int64      |Is gelijk aan
Ty      |Entiteitstype                |enum   |Is gelijk aan

## <a name="entity-type-enum"></a>Entiteit type enum
Name                                                            |value
----------------------------------------------------------------|-----
[Publicatie](PaperEntityAttributes.md)                               |0
[Auteur](AuthorEntityAttributes.md)                             |1
[Tijdschrift](JournalEntityAttributes.md)                           |2
[Conferentiereeks](JournalEntityAttributes.md)                 |3
[Conferentie](ConferenceInstanceEntityAttributes.md)    |4
[Relatie](AffiliationEntityAttributes.md)                   |5
[Onderzoeksgebied](FieldsOfStudyEntityAttributes.md)                      |6

