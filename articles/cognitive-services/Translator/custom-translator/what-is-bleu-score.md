---
title: Wat is een score BLEU? -Aangepaste Translator
titleSuffix: Azure Cognitive Services
description: BLEU is een meting van de verschillen tussen een automatische vertaling en een of meer mens gemaakte referentie vertalingen van dezelfde bron zin. Het algoritme BLEU vergelijkt opeenvolgende zinnen van de automatische vertaling met de opeenvolgende items in de vertaling verwijzing gezocht en telt het aantal komt overeen met een gewogen besturingsaanvraag.
author: rajdeep-in
manager: christw
ms.service: cognitive-services
ms.component: custom-translator
ms.topic: article
ms.date: 11/13/2018
ms.author: v-rada
ms.openlocfilehash: 887eaeac5d74aafd9839495939a2079b288738e4
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/14/2018
ms.locfileid: "51627397"
---
# <a name="what-is-a-bleu-score"></a>Wat is een score BLEU?

[BLEU (tweetalige evaluatie Understudy)](https://en.wikipedia.org/wiki/BLEU) is een meting van de verschillen tussen een automatische vertaling en een of meer mens gemaakte referentie vertalingen van dezelfde bron zin.

## <a name="scoring-process"></a>Scoring-proces

Het algoritme BLEU vergelijkt opeenvolgende zinnen van de automatische vertaling met de opeenvolgende items in de vertaling verwijzing gezocht en telt het aantal komt overeen met een gewogen besturingsaanvraag. Deze overeenkomsten zijn positie onafhankelijk. Een hogere mate van de overeenkomst geeft aan dat een hogere mate van vergelijkbaar zijn met de verwijzing vertaling en hogere score. Leesbaarheid en grammaticale zijn niet in aanmerking genomen.

## <a name="how-bleu-works"></a>Hoe werkt de BLEU?

BLEU van kracht is dat deze met menselijke beslissing goed correleert door gemiddelde afzonderlijke zin beslissing fouten over een corpus testen, in plaats van bij het ontwerpen van de exacte menselijke beslissing voor elke zin het.

Is een uitgebreidere bespreking van BLEU scores [hier](https://youtu.be/-UqDljMymMg).

BLEU resultaten sterk afhankelijk zijn van de breedte van uw domein, de consistentie van de testgegevens met de training en -gegevens, afstemmen en hoeveel gegevens er beschikbaar zijn om te trainen. Als uw modellen zijn getraind op een klein domein en is uw trainingsgegevens consistent zijn met de testgegevens van uw, kunt u een hoge BLEU score verwachten.

>[!NOTE]
>Een vergelijking tussen BLEU scores is alleen verantwoord BLEU resultaten zijn vergeleken met dezelfde set Test, de dezelfde combinatie van taal en dezelfde MT-engine. Een score BLEU uit een andere test-set is gebonden aan afwijken.
