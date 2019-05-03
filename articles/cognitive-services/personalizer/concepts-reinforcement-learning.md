---
title: Bekrachtigingen - Personalizer
titleSuffix: Azure Cognitive Services
description: Personalizer gebruikt informatie over acties en de huidige context om betere ranking-suggesties maken. De informatie over deze acties en context zijn kenmerken of eigenschappen die worden aangeduid als functies.
services: cognitive-services
author: edjez
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: overview
ms.date: 05/07/2019
ms.author: edjez
ms.openlocfilehash: 30f009f76c25d80281d748e1e484175380ca9743
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/02/2019
ms.locfileid: "65027171"
---
# <a name="what-is-reinforcement-learning"></a>Wat is Bekrachtigingen?


Bekrachtigingen is een benadering tot machine learning waarmee u leert van problemen met het ophalen van feedback van het gebruik ervan.
 
Bekrachtigingen werkt door:

* Biedt een kans of vrijheidsgraad op te nemen van een probleem -, zoals besluiten of keuzes maken.
* Contextuele informatie over de omgeving en de opties bieden.
* Feedback geven over hoe goed het gedrag van een bepaald doel bereikt.

Er zijn veel subtypen en stijlen van versterkend leren, is dit de werking van het concept in Personalizer:

* Uw toepassing biedt de mogelijkheid om een deel van de inhoud uit een lijst met alternatieven weer te geven.
* Uw toepassing bevat informatie over elke alternatieve en de context van de gebruiker.
* De berekeningen van uw toepassing een _Beloon score_.

In tegenstelling tot enkele manieren om te leren met bekrachtigingen beschikt leren, vereist Personalizer niet de simulatie om in te werken. De learning-algoritmen zijn ontworpen om te reageren op een externe gebruikers (versus beheren) en leer van elk gegevenspunt met een goed begrip is een unieke kans die kosten tijd en geld te maken, en of er een niet-nul spijt (verlies van mogelijke beloning) als suboptimale prestaties gebeurt.

## <a name="what-type-of-reinforcement-learning-algorithms-does-personalizer-use"></a>Welk type leren met bekrachtigingen beschikt learning-algoritmen gebruikt Personalizer?

Maakt gebruik van de huidige versie van Personalizer **contextuele bandieten**, een methode om te leren met bekrachtigingen beschikt dat wil zeggen learning kader rond besluiten of keuzes te bieden tussen de afzonderlijke acties in een bepaalde context maken.

De _geheugen besluit_, het model dat is getraind om vast te leggen van de best mogelijke beslissing gegeven een context, maakt gebruik van een set lineaire modellen. Deze gebleken herhaaldelijk bedrijfsresultaten te verbeteren en een bewezen benadering, zijn, gedeeltelijk omdat ze meer uit de praktijk zeer snel zonder meerdere pass training en gedeeltelijk omdat ze kunnen die aanvullen onder supervisie learning-modellen en deep neural netwerk-modellen.

De toewijzing van verkennen/exploit Guard-verkeer wordt aangebracht in willekeurig na het percentage instellen voor onderzoek en het standaardalgoritme voor verkenning epsilon greedy is.

### <a name="history-of-contextual-bandits"></a>Geschiedenis van contextuele bandieten

John Langford lang geleden bedacht de naam van contextuele bandieten (Langford en Zhang [2007]) om te beschrijven een tractable subset van bekrachtigingen en heeft hij gewerkt aan een half-dozen documenten verbeteren van onze inzicht in hoe u om te leren op deze paradigma:

* Beygelzimer et al. [2011]
* Dudík markt [2011a, b]
* Kemp markt [2014, 2012]
* Beygelzimer en Langford [2009]
* Li markt [2010]

Jeroen heeft ook gegeven verschillende zelfstudies eerder over onderwerpen als gezamenlijke voorspelling (ICML 2015), contextuele bandieten theorie (NIPS 2013), actief leren (ICML 2009) en voorbeeld complexiteit grenzen (ICML 2003)

## <a name="what-machine-learning-frameworks-does-personalizer-use"></a>Welke machine learning-frameworks maakt gebruik van Personalizer?

U gebruikt momenteel personalizer [Vowpal Wabbit](https://github.com/VowpalWabbit/vowpal_wabbit/wiki) als basis voor de machine learning. Dit framework kunt voor maximale doorvoer en laagste latentie bij het maken van persoonlijke instellingen ranks en het model met alle gebeurtenissen te trainen.

## <a name="references"></a>Verwijzingen

* [Contextuele beslissingen met lage technische inspanningen](https://arxiv.org/abs/1606.03966)
* [Een aanpak kortingen op geoorloofd classificatie](https://arxiv.org/abs/1803.02453)
* [Efficiënte contextuele bandieten in niet-stilstand werelden](https://arxiv.org/abs/1708.01799ds)
* [Voorspelling van de resterende verlies: Leren met bekrachtigingen beschikt: leren met geen incrementele Feedback](https://openreview.net/pdf?id=HJNMYceCW)
* [Instructies en visuele waarnemingen toewijzen aan acties met Bekrachtigingen](https://arxiv.org/abs/1704.08795)
* [Betere resultaten dan uw leraar leren om te zoeken](https://arxiv.org/abs/1502.02206)

## <a name="next-steps"></a>Volgende stappen

[Offline-evaluatie](concepts-offline-evaluation.md) 