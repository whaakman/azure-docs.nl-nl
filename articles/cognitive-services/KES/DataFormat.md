---
title: De indeling van de gegevens in de kennis exploratie Service API | Microsoft Docs
description: Meer informatie over de indeling van de gegevens in het Knowledge exploratie Service (KES) API in cognitieve Services.
services: cognitive-services
author: bojunehsu
manager: stesp
ms.service: cognitive-services
ms.component: knowledge-exploration
ms.topic: article
ms.date: 03/26/2016
ms.author: paulhsu
ms.openlocfilehash: a763505ac6458d68df74ae73e71029b81202ec8b
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344439"
---
# <a name="data-format"></a>Indeling van gegevens
Het gegevensbestand beschrijving van de lijst met objecten om te indexeren.
Elke regel in het bestand geeft de kenmerkwaarden van een object in [JSON-indeling](http://json.org/) met UTF-8-codering.
Naast de kenmerken zijn gedefinieerd de [schema](SchemaFormat.md), elk object heeft een optionele 'logprob'-kenmerk waarmee de kans relatieve logboek tussen de objecten.
Wanneer de service weer objecten in de volgorde van de kans verkleinen, kunnen we 'logprob' gebruiken om aan te geven van de afzender volgorde van de overeenkomende objecten.
Een waarschijnlijkheid gegeven *p* tussen 0 en 1, de bijbehorende logboekbestanden kans worden berekend als logboek (*p*), waarbij log() de functie natuurlijke log.
Wanneer u geen waarde voor logprob is opgegeven, wordt de standaardwaarde 0 gebruikt.

```json
{"logprob":-5.3, "Title":"latent dirichlet allocation", "Year":2003, "Author":{"Name":"david m blei", "Affiliation":"uc berkeley"}, "Author":{"Name":"andrew y ng", "Affiliation":"stanford"}, "Author":{"Name":"michael i jordan", "Affiliation":"uc berkeley"}}
{"logprob":-6.1, "Title":"probabilistic latent semantic indexing", "Year":1999, "Author":{"Name":"thomas hofmann", "Affiliation":"uc berkeley"}}
...
```

De waarde is voor String, GUID en Blob-kenmerken weergegeven als een JSON-tekenreeks tussen aanhalingstekens.  Voor numerieke kenmerken (Int32, Int64, Double), wordt de waarde weergegeven als een JSON-nummer.  Voor samengestelde kenmerken heeft de waarde een JSON-object waarmee de onderliggende kenmerkwaarden.  Post voor snellere index builds de objecten door het logboek kans verkleinen.

In het algemeen heeft een kenmerk 0 of meer waarden.  Als een kenmerk geen waarde heeft, wordt u gewoon van de JSON.  Als een kenmerk 2 of meer waarden heeft, kunnen we de kenmerk-waardepaar in het JSON-object herhaalt.  We kunnen ook een JSON-matrix met de meerdere waarden met het kenmerk toewijzen.

```json
{"logprob":0, "Title":"0 keyword"}
{"logprob":0, "Title":"1 keyword", "Keyword":"foo"}
{"logprob":0, "Title":"2 keywords", "Keyword":"foo", "Keyword":"bar"}
{"logprob":0, "Title":"2 keywords (alt)", "Keyword":["foo", "bar"]}
```
