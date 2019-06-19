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
ms.date: 06/18/2019
ms.author: aahi
ms.openlocfilehash: 704a1193eb47f9346900c6c8a003122c30c8ab44
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67203975"
---
# <a name="language-and-region-support-for-the-text-analytics-api"></a>Ondersteuning voor taal en regio voor de Tekstanalyse-API

Dit artikel wordt uitgelegd welke talen worden ondersteund voor elke bewerking: sentimentanalyse, sleuteltermextractie, taaldetectie en herkenning van benoemde entiteiten.

## <a name="language-detection"></a>Taaldetectie

De Tekstanalyse-API kan een groot aantal talen, varianten dialecten en sommige regionale/culturele talen detecteren.  Taaldetectie retourneert "script" van een taal. Bijvoorbeeld, voor de zinsnede 'Ik heb een hond' wordt geretourneerd `en` in plaats van `en-US`. Het alleen speciaal geval is Chinees, waarop de mogelijkheid van de detectie van taal retourneert `zh_CHS` of `zh_CHT` als het script dat de opgegeven tekst kan worden bepaald. In situaties waar een specifiek script voor een Chinese document kan niet worden geïdentificeerd, retourneert deze gewoon `zh`.

We de exacte lijst met talen voor deze functie niet publiceren, maar het kan een groot aantal talen, varianten dialecten en sommige regionale/culturele talen detecteren. 

Als u inhoud, uitgedrukt in een minder vaak gebruikte taal hebt, kunt u proberen taaldetectie om te zien als het resultaat een code. Het antwoord voor talen die niet kunnen worden gedetecteerd, is `unknown`.

## <a name="sentiment-analysis-key-phrase-extraction-and-named-entity-recognition"></a>Sentimentanalyse, Sleuteltermextractie en herkenning van benoemde entiteiten

Voor sentimentanalyse, sleuteltermextractie en herkenning entiteit wordt de lijst met ondersteunde talen selectiever omdat de analyzers verfijnd om aan de regels voor linguïstische van extra talen te kunnen zijn. Ondersteuning voor de volledige set [Entiteitstypen](how-tos/text-analytics-how-to-entity-linking.md#supported-types-for-named-entity-recognition) is momenteel beperkt tot de volgende talen: 
* Nederlands
* Chinees (Vereenvoudigd)
* Frans
* Duits
* Spaans

Alleen de `Person`, `Location` en `Organization` benoemde entiteiten worden geretourneerd voor de andere talen.

## <a name="language-list-and-status"></a>Talenlijst met en status

Taalondersteuning wordt in eerste instantie in de Preview-versie opwaarderen naar algemeen beschikbaar (GA) status, onafhankelijk van elkaar en van de algehele Text Analytics-service uitgerold. Het is mogelijk voor talen die moet worden bewaard in de Preview-versie, zelfs terwijl de Tekstanalyse-API-overgangen naar algemeen beschikbaar.

| Taal    | Taalcode | Sentiment | Sleutelfrasen | Herkenning van benoemde entiteiten |   Opmerkingen  |
|:----------- |:-------------:|:---------:|:-----------:|:-----------:|:-----------:
| Arabisch      | `ar`          |           |             | ✔ \*                     | |
| Tsjechisch       | `cs`          |           |             | ✔ \*                     | |
| Chinees (Vereenvoudigd) | `zh-CN`|           |             | ✔ \*        |    |
| Deens      | `da`          | ✔ \*     | ✔           | ✔ \*            |     |
| Nederlands       | `nl`          | ✔ \*     | ✔          |  ✔ \*           |     |
| Nederlands     | `en`          | ✔        | ✔           |  ✔ \*\*     |      |
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

\*\* [Met de naam van entiteit erkenning](how-tos/text-analytics-how-to-entity-linking.md#named-entity-recognition-ner) en [entiteiten koppelen](how-tos/text-analytics-how-to-entity-linking.md#entity-linking) zijn beide beschikbaar voor deze taal.    

## <a name="see-also"></a>Zie ook

[Pagina met cognitive Services-documentatie](https://docs.microsoft.com/azure/cognitive-services/)   
[Productpagina van Cognitive Services](https://azure.microsoft.com/services/cognitive-services/)
