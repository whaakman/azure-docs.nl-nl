---
title: Zinnen en tokens op de taalkundige Analysis-API | Microsoft Docs
description: Meer informatie over zin scheiding en tokeniseren in de taalkundige Analysis-API in cognitieve Services.
services: cognitive-services
author: DavidLiCIG
manager: wkwok
ms.service: cognitive-services
ms.component: linguistic-analysis
ms.topic: article
ms.date: 03/21/2016
ms.author: davl
ms.openlocfilehash: 4681098a0e56640e95463272be44f7432be26839
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344494"
---
# <a name="sentence-separation-and-tokenization"></a>Scheiding van zin en tokeniseren

## <a name="background-and-motivation"></a>Achtergrond en doel

Een instantie van de tekst opgegeven, de eerste stap van de taalkundige analyse, is dit in zinnen en tokens te verdelen.

### <a name="sentence-separation"></a>Scheiding van zin

Op de eerste blik lijkt of tekst in zinnen splitsen eenvoudig is: alleen de markeringen end van een zin vinden en er zinnen verbreken.
Deze markeringen zijn echter vaak complexe en niet-eenduidige.

U kunt bijvoorbeeld de volgende tekst:

> Wat zei je?!? Ik hebt niet gereageerd over van de directeur 'nieuw voorstel." Het is belangrijk Dhr. en Mevr Smith.

Deze tekst bevat drie zinnen:

- Wat zei je?!?
- Ik hebt niet gereageerd over van de directeur 'nieuw voorstel."
- Het is belangrijk Dhr. en Mevr Smith.

Houd er rekening mee hoe het einde van zinnen verschillend zijn gemarkeerd.
De eerste eindigt in een combinatie van vraagtekens en uitroeptekens (ook wel een interrobang genoemd).
De tweede eindigt met een punt of volledige stoppen, maar het volgende aanhalingsteken moeten worden opgehaald in de voorgaande zinnen.
In de derde zin ziet u hoe dat dezelfde periode teken markeren afkortingen ook kan worden gebruikt.
Een goede kandidaat set NET kijken naar leestekens biedt, maar verdere werk is vereist voor het identificeren van de grenzen true zin.

### <a name="tokenization"></a>Tokeniseren

De volgende taak is om deze zinnen in tokens.
Voor het grootste deel worden Engelse tokens gescheiden door spaties bevatten.
(-Tokens of woorden zoeken is veel eenvoudiger in het Engels dan in het Chinees, waar spaties tussen woorden voornamelijk niet worden gebruikt.
De eerste zin kan worden geschreven als 'Whatdidyousay?')

Er zijn enkele moeilijk gevallen.
Eerst leestekens vaak (maar niet altijd) moet worden gesplitst weg van het omringende context.
Ten tweede Engels is *Verkortingen*, zoals 'geen' of 'is', waarbij de woorden zijn gecomprimeerd en afgekort in kleinere delen. Het doel van het tokenizer is de tekenreeks om in te breken woorden.

Laten we terug naar de voorbeeldzinnen hierboven.
Nu we een 'center-punt' hebt geplaatst (&middot;) tussen elke afzonderlijke token.

- Wat &middot; heeft &middot; u &middot; spreken &middot; gemikt?
- Ik &middot; heeft &middot; &middot; horen &middot; over &middot; de &middot; directeur &middot; van &middot; ' &middot; nieuwe &middot; voorstel &middot; . &middot; "
- Deze &middot; van &middot; belangrijke &middot; naar &middot; Dhr. &middot; en &middot; Mevr. &middot; Smith &middot; .

Houd er rekening mee hoe de meeste tokens zijn woorden die u in de woordenlijst vinden wilt (bijv, *belangrijke*, *directeur*).
Anderen bestaan alleen uit leestekens.
Ten slotte zijn er meer ongebruikelijke tokens vertegenwoordigt verkorte formuleringen zoals ** voor *niet*, bezittelijke voornaamwoorden, zoals *van*, enzovoort. Deze tokeniseren kan worden uitgevoerd op het woord verwerken *niet* en de woordgroep *niet* op een consistente manier voor exemplaar.

## <a name="specification"></a>Specificatie

Het is belangrijk om consistente beslissingen te nemen over wat bestaat uit een zin en een token.
We zijn afhankelijk van de specificatie van het [Penn Treebank](https://www.cis.upenn.edu/~treebank/) (enkele extra details hier beschikbaar zijn: [https://www.cis.upenn.edu/~treebank/tokenization.html]).
