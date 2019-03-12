---
title: Gegevensindeling - Knowledge Exploration Service API
titlesuffix: Azure Cognitive Services
description: Meer informatie over de gegevensindeling in de Knowledge Exploration Service KES () API.
services: cognitive-services
author: bojunehsu
manager: nitinme
ms.service: cognitive-services
ms.subservice: knowledge-exploration
ms.topic: conceptual
ms.date: 03/26/2016
ms.author: paulhsu
ms.openlocfilehash: bba257c61509d862bb3161625750f05a86af8770
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57532164"
---
# <a name="data-format"></a>Gegevensindeling

Bestand met de beschrijving van de lijst van objecten om te indexeren.
Elke regel in het bestand bevat de waarden van het kenmerk van een object in [JSON-indeling](https://json.org/) met UTF-8-codering.
Naast de kenmerken die zijn gedefinieerd de [schema](SchemaFormat.md), elk object is een optionele 'logprob'-kenmerk dat Hiermee geeft u de kans dat relatieve log tussen de objecten.
Wanneer de service objecten in de volgorde wordt van de kans te verkleinen, kunnen we 'logprob' gebruiken om aan te geven van de geretourneerde volgorde van de overeenkomende objecten.
De kans gegeven *p* tussen 0 en 1, de bijbehorende logboekbestanden kans wordt berekend als het logboek (*p*), waarbij log() staat voor de functie natuurlijke logboek.
Als er geen waarde is opgegeven voor logprob, wordt de standaardwaarde 0 gebruikt.

```json
{"logprob":-5.3, "Title":"latent dirichlet allocation", "Year":2003, "Author":{"Name":"david m blei", "Affiliation":"uc berkeley"}, "Author":{"Name":"andrew y ng", "Affiliation":"stanford"}, "Author":{"Name":"michael i jordan", "Affiliation":"uc berkeley"}}
{"logprob":-6.1, "Title":"probabilistic latent semantic indexing", "Year":1999, "Author":{"Name":"thomas hofmann", "Affiliation":"uc berkeley"}}
...
```

De waarde is voor String en GUID Blob kenmerken weergegeven als een JSON-tekenreeks tussen aanhalingstekens.  Voor numerieke kenmerken (Int32, Int64, Double), wordt de waarde weergegeven als een JSON-nummer.  Voor samengestelde kenmerken heeft de waarde een JSON-object waarmee de onderliggende kenmerkwaarden.  Post voor snellere builds voor index de objecten door log kans te verkleinen.

In het algemeen kan een kenmerk 0 of meer waarden hebben.  Als een kenmerk geen waarde heeft, verwijderen we deze gewoon uit de JSON.  Als een kenmerk 2 of meer waarden heeft, kunnen we de kenmerk-waardepaar in het JSON-object herhalen.  We kunnen ook een JSON-matrix met de meerdere waarden aan het kenmerk toewijzen.

```json
{"logprob":0, "Title":"0 keyword"}
{"logprob":0, "Title":"1 keyword", "Keyword":"foo"}
{"logprob":0, "Title":"2 keywords", "Keyword":"foo", "Keyword":"bar"}
{"logprob":0, "Title":"2 keywords (alt)", "Keyword":["foo", "bar"]}
```
