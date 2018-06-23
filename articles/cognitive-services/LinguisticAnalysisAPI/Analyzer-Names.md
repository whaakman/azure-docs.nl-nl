---
title: Analyzer naming structuur in de taalkundige Analysis-API | Microsoft Docs
description: Meer informatie over hoe de naamgeving structuur voor analyzers in de taalkundige Analysis-API maakt gebruikt om zowel flexibel en precisie.
services: cognitive-services
author: RichardSunMS
manager: wkwok
ms.service: cognitive-services
ms.component: linguistic-analysis
ms.topic: article
ms.date: 03/23/2016
ms.author: lesun
ms.openlocfilehash: 2729b7126e82862660fc8e1a995cc87ae996ea03
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344470"
---
# <a name="analyzer-names"></a>Namen van Analyzer

We gebruiken een enigszins gecompliceerd naming structuur voor analyzers beide flexibiliteit op analyzers en precisie begrijpen wat een naam betekent toe te passen.
Analyzer namen bestaan uit vier onderdelen: een ID, een soort, een specificatie en een implementatie.
De rol van elk onderdeel is onder gedefinieerd.

## <a name="id"></a>Id
Eerst heeft een analyzer een unieke ID; een GUID.
Deze GUID's relatief zelden moeten wijzigen, maar zijn de enige manier om een unieke beschrijving van een bepaalde analyzer.

## <a name="kind"></a>Soort
Vervolgens moet elke analyzer is een **type**.
Hiermee definieert u in zeer brede voorwaarden het type van de analyse geretourneerd en moet een unieke definiëren de structuur van de gegevens die analyse vertegenwoordigt.
Er zijn drie verschillende typen:
 - [Tokens](Sentences-and-Tokens.md)
 - [POS-labels](Pos-Tagging.md)
 - [Kieskring structuur](constituency-parsing.md)

## <a name="specification"></a>Specificatie
Binnen een bepaald soort echter andere deskundigen mogelijk niet akkoord op hoe een bepaalde verschijnsel moet worden geanalyseerd.
In tegenstelling tot programmeertalen is er geen duidelijke en exacte definitie van hoe dit moet worden gedaan.

Denk bijvoorbeeld aan dat we probeerde te vinden van de tokens in de Engelse zin "Hij niet gaat."
Houd in het bijzonder rekening met de tekenreeks 'geen'.
Eén mogelijke interpretatie is dat deze moet worden gesplitst in twee tokens: 'is' en 'geen'.
De alternatieve zin 'hij is niet uitgevoerd' moet vervolgens dezelfde set van tokens.
Een andere mogelijkheid is om in te spreken dat deze moet worden gesplitst in de tokens 'komt' en ''.
Het laatste token zou niet normaal worden beschouwd als een woord, maar deze benadering behoudt meer informatie over de surface tekenreeks, die soms nuttig kan zijn.
Of misschien die inkrimping moet worden beschouwd als één woord.

Ongeacht welke keuze wordt gemaakt, dat de keuze consistent moet worden aangebracht.
Dit is precies de rol van een **specificatie**: om te bepalen wat een juiste weergave moet zijn.

Analyzer uitvoer kunnen alleen redelijk worden vergeleken met gegevens die aan de dezelfde specificatie voldoet.

## <a name="implementation"></a>Implementatie

Er zijn vaak meerdere modellen die verbinding proberen te dezelfde resultaten bereiken, maar met verschillende prestatiekenmerken.
Het is mogelijk dat één model sneller echter minder nauwkeurig; een ander mogelijk een andere afweging maken.

De **implementatie** naam van een analyzer wordt gebruikt ter identificatie van dit soort informatie, zodat gebruikers het meest geschikte analyzer voor hun behoeften kunnen kiezen.
