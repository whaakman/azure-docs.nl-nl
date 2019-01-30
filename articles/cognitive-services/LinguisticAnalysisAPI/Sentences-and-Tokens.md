---
title: Zinnen en tokens - linguïstische analyse-API
titlesuffix: Azure Cognitive Services
description: Meer informatie over de scheiding van zinnen en tokeniseren in de linguïstische analyse-API.
services: cognitive-services
author: DavidLiCIG
manager: cgronlun
ms.service: cognitive-services
ms.subservice: linguistic-analysis
ms.topic: conceptual
ms.date: 03/21/2016
ms.author: davl
ROBOTS: NOINDEX
ms.openlocfilehash: 3b88336866909f527aa17cf77514a5ca4a4830b5
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55213443"
---
# <a name="sentence-separation-and-tokenization"></a>Scheiding van zinnen en tokeniseren

> [!IMPORTANT]
> De preview van Linguistic Analysis is op 9 augustus 2018 uit gebruik genomen. We raden u aan [Azure Machine Learning-tekstanalysemodules](https://docs.microsoft.com/azure/machine-learning/studio-module-reference/text-analytics) te gebruiken voor tekstverwerking en -analyse.

## <a name="background-and-motivation"></a>Achtergrond en motivatie

Een stuk tekst, is de eerste stap van de linguïstische analyse opsplitsen in zinnen en tokens.

### <a name="sentence-separation"></a>Scheiding van zinnen

Op de eerste blik het lijkt erop dat belangrijke tekst in zinnen eenvoudig is: alleen de markeringen end van een zin zoeken en er zinnen verbreken.
Deze markeringen worden echter vaak complexe en niet-eenduidige.

U kunt bijvoorbeeld de volgende tekst:

> Wat zei je?!? Ik hebt niet gereageerd over van de directeur 'nieuwe voorstel." Het is belangrijk voor Dhr. en Mevr Smith.

Deze tekst bevat drie zinnen:

- Wat zei je?!?
- Ik hebt niet gereageerd over van de directeur 'nieuwe voorstel."
- Het is belangrijk voor Dhr. en Mevr Smith.

Houd er rekening mee hoe het einde van zinnen op heel verschillende manieren worden gemarkeerd.
De eerste eindigt in een combinatie van vraagtekens en uitroeptekens (ook wel een interrobang genoemd).
De tweede eindigt met een punt of volledige stoppen, maar de volgende aanhalingsteken moeten worden opgehaald in de voorgaande zin.
In de derde zin, kunt u zien hoe dat dezelfde periode teken markeren afkortingen ook kan worden gebruikt.
Biedt een goede kandidaat alleen kijken naar interpunctie, maar verdere werk is vereist voor het identificeren van de grenzen van de zin ' True '.

### <a name="tokenization"></a>Tokeniseren

De volgende taak bestaat uit om deze zinnen in tokens.
Voor het overgrote deel worden Engelse tokens gescheiden door een spatie.
(Zoeken naar woorden of tokens is veel eenvoudiger in het Engels dan in het Chinees, waarbij spaties tussen woorden voornamelijk niet worden gebruikt.
De eerste zin kan worden geschreven als "Whatdidyousay?")

Er zijn een paar moeilijk gevallen.
Eerst, interpunctie vaak (maar niet altijd) moet worden gesplitst vandaan lopen rond context.
Ten tweede Engels is *Samentrekkingen*, zoals "niet" of "het is", waarbij de woorden zijn gecomprimeerd en afgekort in kleinere delen.
Het doel van het tokenizer is om de tekenreeks in woorden.

We gaan terug naar de voorbeeldzinnen hierboven.
Nu we een 'center-punt' hebt geplaatst (&middot;) tussen elke afzonderlijke token.

- Wat &middot; heeft &middot; u &middot; zeg &middot; gemikt?
- Ik &middot; heeft &middot; &middot; horen &middot; over &middot; de &middot; director &middot; van &middot; " &middot; nieuwe &middot; voorstel &middot; . &middot; "
- Deze &middot; van &middot; belangrijk &middot; naar &middot; Dhr. &middot; en &middot; Mevr. &middot; Smith &middot; .

Houd er rekening mee hoe de meeste tokens zijn woorden die u in de woordenlijst vinden wilt (bijvoorbeeld *belangrijk*, *director*).
Anderen bestaat uitsluitend uit leestekens.
Ten slotte zijn er meer ongebruikelijke tokens voor voornaamwoorden, *zoals* voor *niet*, en bezittelijke voornaamwoorden, zoals *van*.
Deze tokeniseren kan we voor het afhandelen van het woord *niet* en de woorden *niet* op een consistente manier.

## <a name="specification"></a>Specificatie

Het is belangrijk om consistente beslissingen over welke bestaat uit een willekeurige zin en een token.
We zijn afhankelijk van de specificatie van de [Penn Treebank](https://catalog.ldc.upenn.edu/LDC99T42) (meer gegevens zijn beschikbaar op ftp://ftp.cis.upenn.edu/pub/treebank/public_html/tokenization.html).
