---
title: Analyzer naming structuur - linguïstische analyse-API
titlesuffix: Azure Cognitive Services
description: Meer informatie over hoe de linguïstische analyse-API Analyzer naamgevingsconventie resulteert zowel flexibiliteit en precisie.
services: cognitive-services
author: RichardSunMS
manager: nitinme
ms.service: cognitive-services
ms.subservice: linguistic-analysis
ms.topic: conceptual
ms.date: 03/23/2016
ms.author: lesun
ROBOTS: NOINDEX
ms.openlocfilehash: b2e956b099022751712f96ab0b15aa57c752b458
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55873373"
---
# <a name="analyzer-names"></a>Analysenamen

> [!IMPORTANT]
> De preview van Linguistic Analysis is op 9 augustus 2018 uit gebruik genomen. We raden u aan [Azure Machine Learning-tekstanalysemodules](https://docs.microsoft.com/azure/machine-learning/studio-module-reference/text-analytics) te gebruiken voor tekstverwerking en -analyse.

Een enigszins gecompliceerd naamgevingsconventie voor analyse gebruiken we beide flexibiliteit voor analyse en precisie begrijpen wat een naam betekent toe te passen.
Analysenamen bestaat uit vier onderdelen: een ID, de soort een specificatie en een van de implementatie.
Onder is de rol van elk onderdeel gedefinieerd.

## <a name="id"></a>Id
Eerst heeft een analyzer een unieke ID; een GUID.
Deze GUID's relatief zelden moeten wijzigen, maar zijn de enige manier om een unieke beschrijving van een bepaalde analyzer.

## <a name="kind"></a>Soort
Vervolgens elke analyzer is een **type**.
Hiermee definieert u in een zeer brede voorwaarden voor het type analyse geretourneerd en de gegevensstructuur die wordt gebruikt om aan te duiden die analyse moet unieke worden gedefinieerd.
Er zijn momenteel drie verschillende typen:
 - [Tokens](Sentences-and-Tokens.md)
 - [POS-Tags](Pos-Tagging.md)
 - [Op basis van ontleding structuur](constituency-parsing.md)

## <a name="specification"></a>Specificatie
Binnen een bepaald type echter verschillende deskundigen mogelijk niet eens bent op de manier waarop een bepaalde verschijnsel moet worden geanalyseerd.
In tegenstelling tot programmeertalen is er geen duidelijke en exacte definitie van hoe dit moet worden gedaan.

Stel bijvoorbeeld dat we zijn proberen te vinden van de tokens in de Engelse zin, 'Hij niet Ga.'
In het bijzonder, houd rekening met de tekenreeks "niet".
Eén mogelijke interpretatie is dat deze moet worden gesplitst in twee tokens: "heeft" en "niet".
De alternatieve zin 'hij niet gebleven"moet vervolgens de dezelfde set van tokens.
Een andere mogelijkheid is om te zeggen dat deze moet worden gesplitst in de tokens "heeft" en "".
Het laatste token zou niet normaal gesproken worden beschouwd als een woord, maar deze methode worden bewaard voor meer informatie over de surface tekenreeks, die soms handig zijn.
Of misschien wel dat inkrimping moet worden beschouwd als één woord.

Ongeacht welke keuze wordt gemaakt, dat keuze consistent moet worden aangebracht.
Dit is precies de rol van een **specificatie**: om te bepalen wat een juiste weergave moet worden.

Analyzer uitvoer kunnen alleen redelijk worden vergeleken met gegevens die aan de dezelfde-specificatie voldoen.

## <a name="implementation"></a>Implementatie

Er zijn vaak meerdere modellen wordt geprobeerd om de dezelfde resultaten te behalen, maar met verschillende prestatiekenmerken.
Een model mogelijk nog sneller echter minder nauwkeurig; een ander mogelijk een andere afweging maken.

De **implementatie** gedeelte van de naam van een analyzer wordt gebruikt voor het identificeren van dit soort informatie, zodat gebruikers de meest geschikte analyzer voor hun behoeften kiezen kunnen.
