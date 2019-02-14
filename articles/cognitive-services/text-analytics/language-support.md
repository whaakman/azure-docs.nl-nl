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
ms.openlocfilehash: a9f48c8cef8d977469bb6c583d0bc363e334f693
ms.sourcegitcommit: b3d74ce0a4acea922eadd96abfb7710ae79356e0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/14/2019
ms.locfileid: "56245317"
---
# <a name="language-and-region-support-for-the-text-analytics-api"></a>Ondersteuning voor taal en regio voor de Tekstanalyse-API

Dit artikel wordt uitgelegd welke talen worden ondersteund voor elke bewerking: sentimentanalyse, sleuteltermextractie en taaldetectie.

## <a name="language-detection"></a>Taaldetectie

De Tekstanalyse-API kan maximaal 120 verschillende talen detecteren. Taaldetectie retourneert "script" van een taal. Bijvoorbeeld, voor de zinsnede 'Ik heb een hond' wordt geretourneerd `en` in plaats van `en-US`. Het alleen speciaal geval is Chinees, waarop de mogelijkheid van de detectie van taal retourneert `zh_CHS` of `zh_CHT` als het script dat de opgegeven tekst kan worden bepaald. In situaties waar een specifiek script voor een Chinese document kan niet worden geïdentificeerd, retourneert deze gewoon `zh`.

## <a name="sentiment-analysis-key-phrase-extraction-and-entity-recognition"></a>Sentimentanalyse, Sleuteltermextractie en herkenning van entiteit

Voor sentimentanalyse, sleuteltermextractie en herkenning entiteit wordt de lijst met ondersteunde talen selectiever omdat de analyzers verfijnd om aan de regels voor linguïstische van extra talen te kunnen zijn.

## <a name="language-list-and-status"></a>Talenlijst met en status

Taalondersteuning wordt in eerste instantie in de Preview-versie opwaarderen naar algemeen beschikbaar (GA) status, onafhankelijk van elkaar en van de algehele Text Analytics-service uitgerold. Het is mogelijk voor talen die moet worden bewaard in de Preview-versie, zelfs terwijl de Tekstanalyse-API-overgangen naar algemeen beschikbaar.

| Taal    | Taalcode | Sentiment | Sleutelfrasen | Herkenning van entiteiten |   Opmerkingen  |
|:----------- |:-------------:|:---------:|:-----------:|:-----------:|:-----------:
| Deens      | `da`          | ✔ \*     | ✔           |             |     |
| Nederlands       | `nl`          | ✔ \*     | ✔          |             |     |
| Engels     | `en`          | ✔        | ✔           |  ✔ \*   |      |
| Fins     | `fi`          | ✔ \*     | ✔           |             |     |
| Frans      | `fr`          | ✔        | ✔           |             |     |
| Duits      | `de`          | ✔ \*     | ✔           |            |     |
| Grieks       | `el`          | ✔ \*     |             |            |     |
| Italiaans     | `it`          | ✔ \*     | ✔           |             |     |
| Japans    | `ja`          |          | ✔           |            |     |
| Koreaans      | `ko`          |          | ✔           |            |     |
| Norwegian  (Bokmål) | `no`          | ✔ \*     |  ✔          |             |     |
| Pools      | `pl`          | ✔ \*     |  ✔          |             |     |
| Portugees (Portugal) | `pt-PT`| ✔        |  ✔          |       |`pt` ook geaccepteerd|
| Portugees (Brazilië)   | `pt-BR`|          |  ✔   |         |     |
| Russisch     | `ru`          | ✔ \*     | ✔           |             |     |
| Spaans     | `es`          | ✔        | ✔           |   ✔ \*\*      |     |
| Zweeds     | `sv`          | ✔ \*     | ✔           |             |     |
| Turks     | `tr`          | ✔ \*     |             |             |  |

\* Geeft aan dat taalondersteuning in de Preview-versie

\*\* Entiteiten extraheren voor Spaans is alleen beschikbaar in [(versie 2.1-preview)](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-V2-1-Preview/operations/5ac4251d5b4ccd1554da7634)

## <a name="see-also"></a>Zie ook

[Pagina met cognitive Services-documentatie](https://docs.microsoft.com/azure/cognitive-services/)   
[Productpagina van Cognitive Services](https://azure.microsoft.com/services/cognitive-services/)
