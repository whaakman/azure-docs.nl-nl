---
title: Ondersteunde talen in Text Analytics API - cognitieve Azure-Services | Microsoft Docs
description: Lijst met algemeen beschikbaar en preview taal ondersteuning voor tekst Analytics API-bewerkingen. Van toepassing op gevoel analyse, uitpakken van sleutel woordgroep en taal wordt gedetecteerd.
services: cognitive-services
author: ashmaka
manager: cgronlun
ms.service: cognitive-services
ms.technology: text-analytics
ms.topic: conceptual
ms.date: 05/02/2018
ms.author: ashmaka
ms.openlocfilehash: 2d341cfaf261bea6367bb55dd5d322f419e22d34
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345675"
---
# <a name="supported-languages-in-the-text-analytics-api"></a>Ondersteunde talen in de tekst Analytics-API

Dit artikel wordt uitgelegd welke talen worden ondersteund voor elke bewerking: gevoel analyse, uitpakken van sleutel woordgroep en taal wordt gedetecteerd.

## <a name="language-detection"></a>Taaldetectie

De tekst Analytics API kan maximaal 120 verschillende talen detecteren. Taaldetectie retourneert 'script' van een taal. Bijvoorbeeld: voor de zinsnede 'Ik heb een aquaduct' wordt geretourneerd `en` in plaats van `en-US`. De enige speciale wordt Chinees, waarbij de mogelijkheid van de detectie taal retourneert `zh_CHS` of `zh_CHT` als het script dat wordt gegeven van de opgegeven tekst kan worden bepaald. In situaties waarin een specifiek script kan niet worden vastgesteld voor een Chinees document wordt geretourneerd gewoon `zh`.

## <a name="sentiment-analysis-key-phrase-extraction-and-entity-linking"></a>Gevoel analyse, uitpakken van sleutel woordgroep en entiteit koppelen

Voor gevoel analyse, uitpakken van sleutel woordgroep en entiteit koppelen, is de lijst met ondersteunde talen meer selectieve omdat de analyzers verfijnd om de taalkundige regels van extra talen onder te brengen.

## <a name="language-list-and-status"></a>Taallijst en status

Taalondersteuning is in eerste instantie uitgerold in preview opwaarderen naar algemeen beschikbaar (GA) status, onafhankelijk van elkaar en van de algehele Text Analytics-service. Het is mogelijk voor talen in preview, zelfs wanneer de tekst Analytics API overgangen naar algemeen beschikbaar blijven.

| Taal    | Taalcode | Stemming | Sleutelfrasen | Entiteiten koppelen |   Opmerkingen  |
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
| Noors (Bokmål) | `no`          | ✔ \*     |  ✔          |             |     |
| Pools      | `pl`          | ✔ \*     |  ✔          |             |     |
| Portugees (Portugal) | `pt-PT`| ✔        |  ✔          |       |`pt` ook geaccepteerd|
| Portugees (Brazilië)   | `pt-BR`|          |  ✔   |         |     |
| Russisch     | `ru`          | ✔ \*     | ✔           |             |     |
| Spaans     | `es`          | ✔        | ✔           |     |     |
| Zweeds     | `sv`          | ✔ \*     | ✔           |             |     |
| Turks     | `tr`          | ✔ \*     |             |             |     |

\* Hiermee wordt aangegeven taalondersteuning in preview

## <a name="see-also"></a>Zie ook

[Cognitieve Services-documentatie pagina](https://docs.microsoft.com/azure/cognitive-services/)   
[Cognitieve Services productpagina](https://azure.microsoft.com/services/cognitive-services/)
