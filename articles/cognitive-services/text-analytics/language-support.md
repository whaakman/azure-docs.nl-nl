---
title: Taalondersteuning - Tekstanalyse-API
titleSuffix: Azure Cognitive Services
description: 'Een lijst van natuurlijke talen die worden ondersteund door de Tekstanalyse-API. Dit artikel wordt uitgelegd welke talen worden ondersteund voor elke bewerking: sentimentanalyse, sleuteltermextractie en taaldetectie.'
services: cognitive-services
author: ashmaka
manager: cgronlun
ms.service: cognitive-services
ms.technology: text-analytics
ms.topic: article
ms.date: 09/25/2018
ms.author: ashmaka
ms.openlocfilehash: e9f466ac6bce98a6a9f2d79a443c9602ca40bb26
ms.sourcegitcommit: 7c4fd6fe267f79e760dc9aa8b432caa03d34615d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/28/2018
ms.locfileid: "47435478"
---
# <a name="language-and-region-support-for-the-text-analytics-api"></a>Ondersteuning voor taal en regio voor de Tekstanalyse-API

Dit artikel wordt uitgelegd welke talen worden ondersteund voor elke bewerking: sentimentanalyse, sleuteltermextractie en taaldetectie.

## <a name="language-detection"></a>Taaldetectie

De Tekstanalyse-API kan maximaal 120 verschillende talen detecteren. Taaldetectie retourneert "script" van een taal. Bijvoorbeeld, voor de zinsnede 'Ik heb een hond' wordt geretourneerd `en` in plaats van `en-US`. Het alleen speciaal geval is Chinees, waarop de mogelijkheid van de detectie van taal retourneert `zh_CHS` of `zh_CHT` als het script dat de opgegeven tekst kan worden bepaald. In situaties waar een specifiek script voor een Chinese document kan niet worden geïdentificeerd, retourneert deze gewoon `zh`.

## <a name="sentiment-analysis-key-phrase-extraction-and-entity-linking"></a>Sentimentanalyse, Sleuteltermextractie en entiteiten koppelen

Voor sentimentanalyse, sleuteltermextractie en entiteiten koppelen, is de lijst met ondersteunde talen selectiever omdat de analyzers verfijnd om aan de regels voor linguïstische van extra talen te kunnen zijn.

## <a name="language-list-and-status"></a>Talenlijst met en status

Taalondersteuning wordt in eerste instantie in de Preview-versie opwaarderen naar algemeen beschikbaar (GA) status, onafhankelijk van elkaar en van de algehele Text Analytics-service uitgerold. Het is mogelijk voor talen die moet worden bewaard in de Preview-versie, zelfs terwijl de Tekstanalyse-API-overgangen naar algemeen beschikbaar.

| Taal    | Taalcode | Sentiment | Sleutelfrasen | Entiteiten koppelen |   Opmerkingen  |
|:----------- |:-------------:|:---------:|:-----------:|:-----------:|:-----------:
| Deens      | `da`          | ✔ \*     | ✔           |             |     |
| Nederlands       | `nl`          | ✔ \*     | ✔          |             |     |
| Nederlands     | `en`          | ✔        | ✔           |  ✔ \*   |      |
| Fins     | `fi`          | ✔ \*     | ✔           |             |     |
| Frans      | `fr`          | ✔        | ✔           |             |     |
| Duits      | `de`          | ✔ \*     | ✔           |            |     |
| Grieks       | `el`          | ✔ \*     |             |            |     |
| Italiaans     | `it`          | ✔ \*     | ✔           |             |     |
| Japans    | `ja`          |          | ✔           |            |     |
| Koreaans      | `ko`          |          | ✔           |            |     |
| Noors (Bokmål) | `no`          | ✔ \*     |  ✔          |             |     |
| Pools      | `pl`          | ✔ \*     |  ✔          |             |     |
| Portugees (Portugal) | `pt-PT`| ✔        |  ✔          |       |`pt` ook geaccepteerd|
| Portugees (Brazilië)   | `pt-BR`|          |  ✔   |         |     |
| Russisch     | `ru`          | ✔ \*     | ✔           |             |     |
| Spaans     | `es`          | ✔        | ✔           |     |     |
| Zweeds     | `sv`          | ✔ \*     | ✔           |             |     |
| Turks     | `tr`          | ✔ \*     |             |             |     |

\* Geeft aan dat taalondersteuning in de Preview-versie

## <a name="see-also"></a>Zie ook

[Pagina met cognitive Services-documentatie](https://docs.microsoft.com/azure/cognitive-services/)   
[Productpagina van Cognitive Services](https://azure.microsoft.com/services/cognitive-services/)
