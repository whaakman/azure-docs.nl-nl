---
title: Analyseren methode in de taalkundige Analysis-API | Microsoft Docs
description: Klik hier voor meer informatie over het gebruik van de methode analyseren in taalkundige Analysis-API voor het analyseren van bepaalde invoer natuurlijke taal.
services: cognitive-services
author: RichardSunMS
manager: wkwok
ms.service: cognitive-services
ms.component: linguistic-analysis
ms.topic: article
ms.date: 12/13/2016
ms.author: lesun
ms.openlocfilehash: b17a00f31845bfa05572dff7ca94e9a1ffd69586
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344525"
---
# <a name="analyze-method"></a>Methode analyseren

De **analyseren** REST-API wordt gebruikt voor het analyseren van de invoer van een bepaalde natuurlijke taal.
Die kunnen erbij betrekken NET zoeken naar de [zinnen en tokens](Sentences-and-Tokens.md) binnen die invoer, zoeken de [deel uit van spraak labels](POS-tagging.md), of het vinden van de [constitutency structuur](Constituency-Parsing.md).
U kunt opgeven welke u wilt dat door het verzamelen van de relevante analyzers resultaten.
Als u alle beschikbare analyzers, bekijk de  **[analyzers](AnalyzersMethod.md)**.

Houd er rekening mee dat u moet de taal van de invoerreeks opgeven.

**REST-eindpunt:**
```
https://westus.api.cognitive.microsoft.com/linguistics/v1.0/analyze
```
<br>

## <a name="request-parameters"></a>Aanvraagparameters

Naam | Type | Vereist | Beschrijving
-----|-------|----------|------------
**taal**    | tekenreeks | Ja | De twee letter ISO taalcode moet worden gebruikt voor analyse. Engels is bijvoorbeeld 'en'.
**analyzerIds** | lijst met tekenreeksen | Ja | Lijst met GUID's van analyzers om toe te passen. Zie de documentatie Analyzers voor meer informatie.
**tekst**        | tekenreeks | Ja | Onbewerkte invoer worden geanalyseerd. Dit wordt mogelijk een korte tekenreeks, zoals een woord of woordgroep, een volledige zin, of een volledige alinea of discourse.

<br>
## <a name="response-json"></a>Antwoord (JSON)
Een matrix van analyse levert, één voor elk kenmerk dat is opgegeven in de aanvraag.

De resultaten er als volgt uitzien:

Naam | Type | Beschrijving
-----|------|--------------
analyzerId | tekenreeks | GUID van de opgegeven analyzer
resultaat | object | Analyzer resultaat

Houd er rekening mee dat het type van het resultaat is afhankelijk van het type invoer analyzer.

### <a name="tokens-response-json"></a>Tokens antwoord (JSON)

Naam | Type | Beschrijving
-----|------|-------------
resultaat | lijst met objecten zin | zin grenzen die zijn geïdentificeerd in de tekst |
[x] resultaat. Offset | int | de tekenoffset is begin van elke zin |
[x] resultaat. Len | int | aantal tekens van elke zin |
[x] resultaat. Tokens | lijst met objecten token | token grenzen die zijn geïdentificeerd in de zin |
[x] resultaat. [-Y] tokens. Offset | int | eerste tekenverschuiving van het token |
[x] resultaat. [-Y] tokens. Len | int | aantal tekens van het token |
[x] resultaat. [-Y] tokens. RawToken | tekenreeks | de tekens in dat token, voordat normalisatie |
[x] resultaat. [-Y] tokens. NormalizedToken | tekenreeks | een genormaliseerde vorm van het teken, veilig voor gebruik in een [parseren structuur](Constituency-Parsing.md), bijvoorbeeld een haakje openen ' (' - LRB - wordt |

Voorbeeld van invoer: ' Dit is een test. Hallo.'
Voorbeeld van de JSON-antwoord:
```json
[
  {
    "Len": 15,
    "Offset": 0,
    "Tokens": [
      {
        "Len": 4,
        "NormalizedToken": "This",
        "Offset": 0,
        "RawToken": "This"
      },
      {
        "Len": 2,
        "NormalizedToken": "is",
        "Offset": 5,
        "RawToken": "is"
      },
      {
        "Len": 1,
        "NormalizedToken": "a",
        "Offset": 8,
        "RawToken": "a"
      },
      {
        "Len": 4,
        "NormalizedToken": "test",
        "Offset": 10,
        "RawToken": "test"
      },
      {
        "Len": 1,
        "NormalizedToken": ".",
        "Offset": 14,
        "RawToken": "."
      }
    ]
  },
  {
    "Len": 6,
    "Offset": 16,
    "Tokens": [
      {
        "Len": 5,
        "NormalizedToken": "Hello",
        "Offset": 16,
        "RawToken": "Hello"
      },
      {
        "Len": 1,
        "NormalizedToken": ".",
        "Offset": 21,
        "RawToken": "."
      }
    ]
  }
]
```


### <a name="pos-tags-response-json"></a>POS labels antwoord (JSON)

Het resultaat is een lijst met lijsten met tekenreeksen.
Er is een lijst met POS-tags, één POS-label voor elke token voor elke zin.
Als u wilt zoeken op het token dat overeenkomt met elke tag POS, moet u vragen om een tokeniseren-object.

### <a name="constituency-tree-response-json"></a>Kieskring structuur antwoord (JSON)

Het resultaat is een lijst met tekenreeksen, één parseringsstructuur voor elke zin gevonden in de invoer.
De vertakkingen parse worden weergegeven in een formulier tussen haakjes.

<br>

## <a name="example"></a>Voorbeeld

`POST /analyze`

Aanvraagtekst: JSON-nettolading
```json
{
  "language": "en",
  "analyzerIds": [
    "4FA79AF1-F22C-408D-98BB-B7D7AEEF7F04",
    "22A6B758-420F-4745-8A3C-46835A67C0D2" ],
  "text": "Hi, Tom! How are you today?" 
}
```

Antwoord: JSON
```json
[
  {
    "analyzerId": "4FA79AF1-F22C-408D-98BB-B7D7AEEF7F04", 
    "result": [ ["NNP",",","NNP","."], ["WRB","VBP","PRP","NN","."] ]
  },
  {
    "analyzerId": "22A6B758-420F-4745-8A3C-46835A67C0D2", 
    "result":["(TOP (S (NNP Hi) (, ,) (NNP Tom) (. !)))","(TOP (SBARQ (WHADVP (WRB How)) (SQ (VP (VBP are)) (NP (PRP you)) (NN today) (. ?))))"]
  }
]
```

