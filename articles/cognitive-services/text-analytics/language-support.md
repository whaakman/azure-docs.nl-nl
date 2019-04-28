---
title: Taalondersteuning - Tekstanalyse-API
titleSuffix: Azure Cognitive Services
description: 'Een lijst van natuurlijke talen die worden ondersteund door de Tekstanalyse-API. Dit artikel wordt uitgelegd welke talen worden ondersteund voor elke bewerking: sentimentanalyse, sleuteltermextractie, taaldetectie en herkenning van de entiteit.'
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: conceptual
ms.date: 02/13/2019
ms.author: aahi
ms.openlocfilehash: 4f1ce8fd44a501f594f3093789d1ef03e664d018
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60829545"
---
# <a name="language-and-region-support-for-the-text-analytics-api"></a>Ondersteuning voor taal en regio voor de Tekstanalyse-API

Dit artikel wordt uitgelegd welke talen worden ondersteund voor elke bewerking: sentimentanalyse, sleuteltermextractie, taaldetectie en herkenning van benoemde entiteiten.

## <a name="language-detection"></a>Taaldetectie

De Tekstanalyse-API kan maximaal 120 verschillende talen detecteren. Taaldetectie retourneert "script" van een taal. Bijvoorbeeld, voor de zinsnede 'Ik heb een hond' wordt geretourneerd `en` in plaats van `en-US`. Het alleen speciaal geval is Chinees, waarop de mogelijkheid van de detectie van taal retourneert `zh_CHS` of `zh_CHT` als het script dat de opgegeven tekst kan worden bepaald. In situaties waar een specifiek script voor een Chinese document kan niet worden geïdentificeerd, retourneert deze gewoon `zh`.

## <a name="sentiment-analysis-key-phrase-extraction-and-named-entity-recognition"></a>Sentimentanalyse, Sleuteltermextractie en herkenning van benoemde entiteiten

Voor sentimentanalyse, sleuteltermextractie en herkenning entiteit wordt de lijst met ondersteunde talen selectiever omdat de analyzers verfijnd om aan de regels voor linguïstische van extra talen te kunnen zijn.

## <a name="language-list-and-status"></a>Talenlijst met en status

Taalondersteuning wordt in eerste instantie in de Preview-versie opwaarderen naar algemeen beschikbaar (GA) status, onafhankelijk van elkaar en van de algehele Text Analytics-service uitgerold. Het is mogelijk voor talen die moet worden bewaard in de Preview-versie, zelfs terwijl de Tekstanalyse-API-overgangen naar algemeen beschikbaar.

| Taal    | Taalcode | Sentiment | Sleutelfrasen | Herkenning van benoemde entiteiten |   Opmerkingen  |
|:----------- |:-------------:|:---------:|:-----------:|:-----------:|:-----------:
| Arabisch      | `ar`          |           |             | ✔ \*                     | |
| Tsjechisch       | `cs`          |           |             | ✔ \*                     | |
| Chinees (Vereenvoudigd) | `zh-CN`|           |             | ✔ \*        |    |
| Deens      | `da`          | ✔ \*     | ✔           | ✔ \*            |     |
| Nederlands       | `nl`          | ✔ \*     | ✔          |  ✔ \*           |     |
| Engels     | `en`          | ✔        | ✔           |  ✔ \*\*     |      |
| Fins     | `fi`          | ✔ \*     | ✔           |  ✔ \*           |     |
| Frans      | `fr`          | ✔        | ✔           |  ✔ \*           |     |
| Duits      | `de`          | ✔ \*     | ✔           |  ✔ \*          |     |
| Grieks       | `el`          | ✔ \*     |             |            |     |
| Hongaars   | `hu`          |           |             |  ✔ \*          |     | 
| Italiaans     | `it`          | ✔ \*     | ✔           |  ✔ \*           |     |
| Japans    | `ja`          |          | ✔           |  ✔ \*          |     |
| Koreaans      | `ko`          |          | ✔           |  ✔ \*          |     |
| Norwegian  (Bokmål) | `no`  | ✔ \*     |  ✔          | ✔ \*            |     |
| Pools      | `pl`          | ✔ \*     |  ✔          |  ✔ \*           |     |
| Portugees (Portugal) | `pt-PT`| ✔        |  ✔          | ✔ \*      |`pt` ook geaccepteerd|
| Portugees (Brazilië)   | `pt-BR`|          |  ✔   |  ✔ \*       |     |
| Russisch     | `ru`          | ✔ \*     | ✔           |  ✔ \*           |     |
| Spaans     | `es`          | ✔        |            |   ✔ \*\*      |     | 
| Zweeds     | `sv`          | ✔ \*     | ✔           |   ✔ \*          |     |
| Turks     | `tr`          | ✔ \*     |             |   ✔ \*          |  |

\* Ondersteuning is beschikbaar als preview

\*\* Met de naam van entiteit erkenning en [entiteiten koppelen](how-tos/text-analytics-how-to-entity-linking.md) zijn beide beschikbaar voor deze taal.    

## <a name="see-also"></a>Zie ook

[Pagina met cognitive Services-documentatie](https://docs.microsoft.com/azure/cognitive-services/)   
[Productpagina van Cognitive Services](https://azure.microsoft.com/services/cognitive-services/)
