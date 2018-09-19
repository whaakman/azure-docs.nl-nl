---
title: De methode voor analyse - Lingistic analyse-API
titlesuffix: Azure Cognitive Services
description: De analyzers REST-API bevat een lijst van de analyzers die momenteel worden ondersteund door de linguïstische analyse-API.
services: cognitive-services
author: RichardSunMS
manager: cgronlun
ms.service: cognitive-services
ms.component: linguistic-analysis
ms.topic: conceptual
ms.date: 06/30/2016
ms.author: lesun
ms.openlocfilehash: b443bbd6377f0720c8be86bbe2b7a3e8ab8cb880
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/18/2018
ms.locfileid: "46129009"
---
# <a name="analyzers-method"></a>Methode voor analyse

De **analyzers** REST-API bevat een lijst van de analyzers die momenteel worden ondersteund door de service.
Het antwoord bevat de [namen](Analyzer-Names.md) en de talen die worden ondersteund door elk (zoals 'en' voor het Engels).

## <a name="request-parameters"></a>Aanvraagparameters
Geen

<br>

## <a name="response-parameters"></a>Antwoord-parameters
Naam | Type | Beschrijving
-----|------|--------------
Talen | lijst met tekenreeksen | lijst met twee letter ISO taalcodes waarvoor deze analyzer kan worden gebruikt.
id   | tekenreeks | unieke ID voor deze analyzer
type | tekenreeks | het uitgebreide type analyzer hier
Specificatie | tekenreeks | de naam van de specificatie die wordt gebruikt voor deze analyzer
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
