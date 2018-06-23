---
title: Veelgestelde vragen over de Tekstanalyse API - cognitieve Azure-Services | Microsoft Docs
description: Vind antwoorden op veelgestelde vragen over Microsoft cognitieve Services Text Analytics API in Azure.
services: cognitive-services
author: HeidiSteen
manager: cgronlun
ms.service: cognitive-services
ms.component: text-analytics
ms.topic: conceptual
ms.date: 3/07/2018
ms.author: heidist
ms.openlocfilehash: bf82899b4317f0f5ce0f6ca5096dccef7cddd931
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345671"
---
# <a name="frequently-asked-questions-faq-about-the-text-analytics-api"></a>Veelgestelde vragen (FAQ) over de Tekstanalyse API

 Antwoorden op veelgestelde vragen over de concepten, code en scenario's die zijn gerelateerd aan de tekst Analytics-API voor cognitieve Microsoft-Services in Azure.

## <a name="can-text-analytics-identify-sarcasm"></a>Kan herkennen Tekstanalyse sarcasm?

Analyse is voor positieve negatieve gevoel in plaats van stemming detectie.

Er is altijd een zekere mate van hoeveelheidsgegevens in gevoel analyse, maar het model is handig wanneer er geen verborgen betekenis of subtekst tot de inhoud is. Irony, sarcasm humor en op dezelfde manier nuanced inhoud, is afhankelijk van culturele context en normen om bedoeling over te brengen. Dit type inhoud behoort tot de meest uitdagende te analyseren. Meestal is de grootste discrepantie is tussen een gegeven score geproduceerd door de analyzer en een subjectieve beoordeling door een persoon voor inhoud met nuanced betekenis.

## <a name="can-i-add-my-own-training-data-or-models"></a>Kan ik mijn eigen trainingsgegevens of modellen toevoegen?

Nee, worden de modellen pretrained. De enige bewerkingen op geüploade gegevens worden score berekenen, uitpakken van sleutel woordgroep en taal wordt gedetecteerd. We host aangepaste modellen. Als u wilt maken en aangepaste machine learning-modellen hosten, overweeg dan de [machine learning-mogelijkheden in Microsoft R Server](https://docs.microsoft.com/r-server/r/concept-what-is-the-microsoftml-package).

## <a name="can-i-request-additional-languages"></a>Kan ik extra talen aanvragen?

Gevoel analyse en uitpakken van sleutel woordgroep zijn beschikbaar voor een [aantal talen selecteren](text-analytics-supported-languages.md). Verwerking van natuurlijke taal is complex en vereist aanzienlijk testen voordat nieuwe functionaliteit kan worden vrijgegeven. Om deze reden vermijden we vooraf ondersteuning wordt aangekondigd zodat niemand een afhankelijkheid op functionaliteit die meer tijd duurt naar volwassen nodig heeft. 

Om te helpen ons prioriteren welke talen om te werken op de volgende, stemmen voor specifieke talen op [User Voice](https://cognitive.uservoice.com/forums/555922-text-analytics). 

## <a name="why-does-key-phrase-extraction-return-some-words-but-not-others"></a>Waarom uitpakken van sleutel woordgroep retourneren sommige woorden maar geen andere?

Uitpakken van sleutel woordgroep wordt voorkomen dat niet-essentiële woorden en zelfstandige bijvoeglijke naamwoorden. Bijvoeglijk naamwoord-zelfstandig naamwoord combinaties, zoals 'spectaculaire weergaven' of 'mistige weer' worden samen geretourneerd.

In het algemeen bestaat de uitvoer van de volgende zelfstandige naamwoorden en objecten van de zin. Uitvoer wordt vermeld in de volgorde van belangrijkheid, met de eerste woordgroep wordt de belangrijkste. Urgentie wordt gemeten door het aantal keren dat een bepaalde concept wordt vermeld of de relatie van dat element op andere elementen in de tekst.

## <a name="why-does-output-vary-given-identical-inputs"></a>Waarom uitvoer variëren gegeven identiek invoer?

Verbeteringen van modellen en algoritmen worden vermeld als de wijziging primaire of stil bijbehorende bij de service als de update secundaire is. Na verloop van tijd wellicht die dezelfde tekst invoer resultaten in een andere gevoel score of sleutel woordgroep uitvoer. Dit is normaal en opzettelijk gevolg van het gebruik van beheerde machine learning-resources in de cloud.

## <a name="next-steps"></a>Volgende stappen

Uw vraag over een ontbrekende functie of functionaliteit is? Houd rekening met aanvragen of uw stem op deze op onze [UserVoice website](https://cognitive.uservoice.com/forums/555922-text-analytics).

## <a name="see-also"></a>Zie ook

 [StackOverflow: Tekstanalyse API](https://stackoverflow.com/questions/tagged/text-analytics-api)   
 [StackOverflow: Cognitieve Services](http://stackoverflow.com/questions/tagged/microsoft-cognitive)
