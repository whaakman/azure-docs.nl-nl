---
title: De methode voor analyse - Lingistic analyse-API
titlesuffix: Azure Cognitive Services
description: De analyzers REST-API bevat een lijst van de analyzers die momenteel worden ondersteund door de linguïstische analyse-API.
services: cognitive-services
author: RichardSunMS
manager: nitinme
ms.service: cognitive-services
ms.subservice: linguistic-analysis
ms.topic: conceptual
ms.date: 06/30/2016
ms.author: lesun
ROBOTS: NOINDEX
ms.openlocfilehash: 9338e87644554ac8b3121c5341cea6f2bc512a97
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55878184"
---
# <a name="analyzers-method"></a>Methode voor analyse

> [!IMPORTANT]
> De preview van Linguistic Analysis is op 9 augustus 2018 uit gebruik genomen. We raden u aan [Azure Machine Learning-tekstanalysemodules](https://docs.microsoft.com/azure/machine-learning/studio-module-reference/text-analytics) te gebruiken voor tekstverwerking en -analyse.

De **analyzers** REST-API bevat een lijst van de analyzers die momenteel worden ondersteund door de service.
Het antwoord bevat de [namen](Analyzer-Names.md) en de talen die worden ondersteund door elk (zoals 'en' voor het Engels).

## <a name="request-parameters"></a>Aanvraagparameters
Geen

<br>

## <a name="response-parameters"></a>Antwoord-parameters
Name | Type | Description
-----|------|--------------
Talen | lijst met tekenreeksen | lijst met twee letter ISO taalcodes waarvoor deze analyzer kan worden gebruikt.
id   | string | unieke ID voor deze analyzer
type | string | het uitgebreide type analyzer hier
Specificatie | string | de naam van de specificatie die wordt gebruikt voor deze analyzer
Implementatie | string | Beschrijving van het model en/of het algoritme achter deze analyzer

<br>
## <a name="example"></a>Voorbeeld
GET /analyzers

Reactie: JSON
```json
[
    {
        "id": "22A6B758-420F-4745-8A3C-46835A67C0D2",
        "languages": ["en"],
        "kind": "Constituency_Tree",  
        "specification": "PennTreebank3",
        "implementation": "SplitMerge"
    },
    {
        "id" : "4FA79AF1-F22C-408D-98BB-B7D7AEEF7F04",
        "languages": ["en"],
        "kind": "POS_Tags",
        "specification": "PennTreebank3",
        "implementation": "cmm"
    },
    {
        "id" : "08EA174B-BFDB-4E64-987E-602F85DA7F72",
        "languages": ["en"],
        "kind": "Tokens",
        "specification":"PennTreebank3",
        "implementation": "regexes"
    }
]
```
