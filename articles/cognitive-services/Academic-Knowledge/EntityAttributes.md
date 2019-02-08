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
ms.openlocfilehash: accc2803895f3892075cdd9877ca98344ab88bd1
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55884814"
---
# <a name="entity-attributes"></a>Entiteitkenmerken

De academic graph van 7 typen entiteit bestaat. Alle entiteiten hebben een entiteit-ID en een entiteitstype.

## <a name="common-entity-attributes"></a>Algemene entiteitskenmerken
Name    |Description                |Type       | Bewerkingen
------- | ------------------------- | --------- | ----------------------------
Id      |Entiteits-id                  |Int64      |Is gelijk aan
Ty      |Entiteitstype                |enum   |Is gelijk aan

## <a name="entity-type-enum"></a>Entiteit type enum
Name                                                            |waarde
----------------------------------------------------------------|-----
[Publicatie](PaperEntityAttributes.md)                               |0
[Auteur](AuthorEntityAttributes.md)                             |1
[Tijdschrift](JournalEntityAttributes.md)                           |2
[Conferentiereeks](JournalEntityAttributes.md)                 |3
[Conferentie](ConferenceInstanceEntityAttributes.md)    |4
[Relatie](AffiliationEntityAttributes.md)                   |5
[Onderzoeksgebied](FieldsOfStudyEntityAttributes.md)                      |6

