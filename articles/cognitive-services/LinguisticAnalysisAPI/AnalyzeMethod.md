---
title: Analysemethode - linguïstische analyse-API
titlesuffix: Azure Cognitive Services
description: Het gebruik van de methode analyseren in linguïstische analyse-API voor het analyseren van bepaalde invoer van natuurlijke taal.
services: cognitive-services
author: RichardSunMS
manager: cgronlun
ms.service: cognitive-services
ms.component: linguistic-analysis
ms.topic: conceptual
ms.date: 12/13/2016
ms.author: lesun
ms.openlocfilehash: c8d380a23c1bbfca8258ef533453050c72a3abd0
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/18/2018
ms.locfileid: "46129621"
---
# <a name="analyze-method"></a>Analysemethode

De **analyseren** REST-API wordt gebruikt voor het analyseren van de invoer van een bepaalde natuurlijke taal.
Dat kan worden te vinden de [zinnen en tokens](Sentences-and-Tokens.md) binnen die invoer, zoeken de [part-of-speech tags](POS-tagging.md), of het vinden van de [constitutency structuur](Constituency-Parsing.md).
U kunt opgeven welke u wilt dat door het verzamelen van de relevante analyzers resultaten.
Als u alle beschikbare analyzers, bekijk de  **[analyzers](AnalyzersMethod.md)**.

Houd er rekening mee dat u nodig hebt om op te geven van de taal van de ingevoerde tekenreeks.

**REST-eindpunt:**
```
https://westus.api.cognitive.microsoft.com/linguistics/v1.0/analyze
```
<br>

## <a name="request-parameters"></a>Aanvraagparameters

Naam | Type | Vereist | Beschrijving
-----|-------|----------|------------
**Taal**    | tekenreeks | Ja | ISO taalcode die moet worden gebruikt voor analyse van de twee letters. Engels is bijvoorbeeld "en".
**analyzerIds** | lijst met tekenreeksen | Ja | Lijst met GUID's van de analyzers om toe te passen. Zie de Analyzers-documentatie voor meer informatie.
**Tekst**        | tekenreeks | Ja | Onbewerkte invoer kunnen worden geanalyseerd. Dit wordt mogelijk een korte tekenreeks, zoals een woord of woordgroep, een volledige zin, of een volledige alinea of discourse.

## <a name="response-json"></a>Antwoord (JSON)

Een matrix van analysis-uitvoer, één voor elk kenmerk dat is opgegeven in de aanvraag.

De resultaten er als volgt uitzien:

Naam | Type | Beschrijving
-----|------|--------------
analyzerId | tekenreeks | GUID van de opgegeven analyzer
Resultaat | object | resultaat van de Analyzer

Houd er rekening mee dat het type van het resultaat is afhankelijk van het type invoer analyzer.

### <a name="tokens-response-json"></a>Tokens antwoord (JSON)

Naam | Type | Beschrijving
-----|------|-------------
Resultaat | lijst met objecten zin | zin grenzen geïdentificeerd binnen de tekst |
resultaat [x]. Offset | int | tekenverschuiving van de eerste van elke zin |
resultaat [x]. Len | int | de lengte in tekens van elke zin |
resultaat [x]. Tokens | lijst met tokens objecten | token grenzen die zijn geïdentificeerd in de zin |
resultaat [x]. [J]-tokens. Offset | int | vanaf teken offset van het token |
resultaat [x]. [J]-tokens. Len | int | de lengte in tekens van het token |
resultaat [x]. [J]-tokens. RawToken | tekenreeks | de tekens in het token, voordat u normalisering |
resultaat [x]. [J]-tokens. NormalizedToken | tekenreeks | een gestandaardiseerde vorm van het teken, veilige voor gebruik in een [parseren structuur](Constituency-Parsing.md); bijvoorbeeld, een haakje openen ' (' - LRB - wordt |

Voorbeeld van invoer: ' Dit is een test. Hello.'
Voorbeeld van JSON-antwoord:
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


### <a name="pos-tags-response-json"></a>POS-Tags antwoord (JSON)

Het resultaat is een lijst met lijsten met tekenreeksen.
Er is een lijst van POS-tags, één POS-tag voor elk token voor elke zin.
Als u zoekt de token die overeenkomt met elke POS-code, moet u vraagt om een tokeniseren-object.

### <a name="constituency-tree-response-json"></a>Op basis van ontleding structuur antwoord (JSON)

Het resultaat is een lijst met tekenreeksen, één parseringsstructuur voor elke zin die is aangetroffen in de invoer.
Het parseren van structuren worden weergegeven in een formulier tussen haakjes.

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

