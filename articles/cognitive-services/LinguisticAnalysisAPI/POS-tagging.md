---
title: Onderdeel van spraak tags in de taalkundige analyse API | Microsoft Docs
description: Meer informatie over hoe deel uit van spraak tags toevoegen in Microsoft cognitieve Services identificeert de categorie of spraak van elk woord van tekst.
services: cognitive-services
author: RichardSunMS
manager: wkwok
ms.service: cognitive-services
ms.component: linguistic-analysis
ms.topic: article
ms.date: 09/27/2016
ms.author: lesun
ms.openlocfilehash: 90fd5b05c2dabdac88c6c8da288ab629177be38d
ms.sourcegitcommit: d1eefa436e434a541e02d938d9cb9fcef4e62604
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2018
ms.locfileid: "37082635"
---
# <a name="part-of-speech-tagging"></a>Onderdeel van spraak Tagging

## <a name="background-and-motivation"></a>Achtergrond en doel

Zodra een tekst is gescheiden in zinnen en tokens, worden de volgende stap van de analyse is het identificeren van de categorie of een deel van spraak van elk woord.
Deze categorieën zoals omvatten *zelfstandig naamwoord* (die doorgaans vertegenwoordigt mensen, plaatsen, dingen, ideeën, etc.) en *term* (die doorgaans vertegenwoordigt acties, verandert van status, enzovoort. Voor sommige woorden, het onderdeel van spraak ondubbelzinnig is (bijvoorbeeld *quagmire* is in feite een zelfstandig naamwoord), maar voor veel andere is het moeilijk te zien.
*Tabel* kan een plaats waar u zich bevinden (of 2D-indeling van de getallen), maar u kunt ook 'tabel een discussie'.

## <a name="list-of-part-of-speech-tags"></a>Lijst met deel uit van spraak labels

| Label | Beschrijving | Voorbeeld woorden |
|-----|-------------|---------------|
| $ | dollar | $ |
| \`\` | aanhalingsteken openen | \` \`\` |
| '' | aanhalingsteken sluiten | ' '' |
| ( | openingshaakje | ( [ { |
| ) | sluithaakje | ) ] } |
| , | door komma 's | , |
| -- | streepje | -- |
| . | zin eindteken | . ! ? |
| : | dubbele punt of weglatingsteken | : ; ... |
| CC | combinatie, coördineren | en maar of nog|
| CD | cijfer kardinaalcurveverbindingslijn | negen 20 1980 ' 96 |
| DT | determiner |een de een alle beide geen van beide|
| EX | Er zijn existentieel: | Er |
| FW | afwijkende word | enfant terrible hoi polloi je ne sais quoi |
| IN | voorzetsel of samen te brengen| in binnenste als bij of |
| JJ | bijvoeglijk naamwoord of een cijfer, rangtelwoord | achtste vrij execrable multimodale |
| JJR | bijvoeglijk naamwoord, vergelijkende | beter sneller goedkoper |
| JJS | bijvoeglijk naamwoord, superlative | aanbevolen snelste goedkoopste | 
| LS | lijst item markering | (a) (b) 1 2 A B A. B. |
| MD | modale assistent | kan mogelijk wordt wordt kan mogelijk moeten zou |
| NN | zelfstandig naamwoord, algemene, enkelvoud of massaopslag | achtergebleven geld schoen |
| NNP | de juiste, enkelvoudige zelfstandig naamwoord, | Kennedy Roosevelt Chicago Weehauken |
| NNPS | de juiste, meervoud zelfstandig naamwoord, | Springfields struiken |
| NNS | zelfstandig naamwoord, algemene, meervoud | stukken muizen velden |
| PDT | vooraf determiner | alle beide half veel behoorlijk dergelijke ervoor dat dit |
| POS | genitive markering | ' de |
| PRP | Overeenkomst, persoonlijke | ze hij deze ik we ze u |
| PRP$ | overeenkomst possessive | haar zijn de mijn onze hun uw |
| RB | bewerkingsparameter | klinisch alleen |
| RBR | bewerkingsparameter vergelijkende | verder versomberden grander graver groter grimmer moeilijker zwaardere gezonder zwaardere hoger echter groter hoger minder complex omslachtiger minder perfect minder lonelier langer harder kleine meer... |
| RESOURCESTRUCTUUR | bewerkingsparameter superlative | aanbevolen grootste bluntest vroegste verst eerste verst moeilijkst heartiest hoogste grootste minst minder meeste dichtstbijzijnde tweede strengste slechtste |
| RP | Partikel | op uitgeschakeld over-omhoog |
| SYM | symbool | % & |
| TOT | 'tot' als voorzetsel of infinitive markering | tot |
| KƆˈNƐKSI | Tussenwerpsel | kɔˈnɛksi Prettige kerstdagen howdy Hallo |
| VB | term baseform | Verleen snel toewijzen |
| VBD | term verleden tijd | gegeven toegewezen flew |
| VBG | term, aanwezig participle of gerundium | Geeft Vliegende toewijzen |
| VBN | term verleden participle | krijgt toegewezen flown |
| VBP | term, aanwezig tijd, niet 3e persoon enkelvoud | Verleen snel toewijzen |
| VBZ | term, aanwezig tijd, 3e persoon enkelvoud | Geeft bestanden wordt toegewezen |
| WDT | W determiner | die welke die |
| WP | W overeenkomst | wie wie |
| WP$ | W-overeenkomst possessive | waarvan |
| WRB | W bewerkingsparameter | hoe echter wanneer waar |

## <a name="specification"></a>Specificatie

Als voor tokeniseren, wij zijn afhankelijk van de specificatie van het [Penn Treebank](https://catalog.ldc.upenn.edu/ldc99t42).
