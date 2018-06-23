---
title: De methode Analyzers in de Lingistic Analysis-API | Microsoft Docs
description: De analyzers REST-API bevat een lijst van analyzers die momenteel worden ondersteund door de service in cognitieve Microsoft-Services.
services: cognitive-services
author: RichardSunMS
manager: wkwok
ms.service: cognitive-services
ms.component: linguistic-analysis
ms.topic: article
ms.date: 06/30/2016
ms.author: lesun
ms.openlocfilehash: 3fc243a0da77c5bae9009929f2b82e1353347752
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344434"
---
# <a name="analyzers-method"></a>Analyzers methode

De **analyzers** REST-API bevat een lijst van analyzers momenteel ondersteund door de service.
Het antwoord bevat de [namen](Analyzer-Names.md) en de talen die worden ondersteund door elk (zoals 'en' voor Engels).

## <a name="request-parameters"></a>Aanvraagparameters
Geen

<br>

## <a name="response-parameters"></a>Antwoord parameters
Naam | Type | Beschrijving
-----|------|--------------
Talen | lijst met tekenreeksen | lijst met twee letter ISO taalcodes vermeld waarvoor deze analyzer kan worden gebruikt.
id   | tekenreeks | unieke ID voor deze analyzer
Type | tekenreeks | het algemene type hier analyzer
Specificatie | tekenreeks | de naam van de specificatie die is gebruikt voor deze analyzer
Implementatie | tekenreeks | Beschrijving van het model en/of het algoritme achter deze analyzer

<br>
## <a name="example"></a>Voorbeeld
/Analyzers ophalen

Antwoord: JSON
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
