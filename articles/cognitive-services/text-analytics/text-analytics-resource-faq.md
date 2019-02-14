---
title: Veelgestelde vragen over de Tekstanalyse-API
titleSuffix: Azure Cognitive Services
description: Vind antwoorden op veelgestelde vragen over de Tekstanalyse-API.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: conceptual
ms.date: 02/13/2019
ms.author: aahi
ms.openlocfilehash: bdc8549f5f72d6098bf132d8395c9892170be9b0
ms.sourcegitcommit: b3d74ce0a4acea922eadd96abfb7710ae79356e0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/14/2019
ms.locfileid: "56245453"
---
# <a name="frequently-asked-questions-faq-about-the-text-analytics-cognitive-service"></a>Veelgestelde vragen (FAQ) over de Text Analytics-Cognitive Service

 Vind antwoorden op veelgestelde vragen over concepten, code en scenario's met betrekking tot de Tekstanalyse-API voor Microsoft Cognitive Services op Azure.

## <a name="can-text-analytics-identify-sarcasm"></a>Kan de Text Analytics herkennen sarcasme?

Analyse is voor positieve negatief gevoel in plaats van stemming detectie.

Er is altijd een zekere mate van imprecisie in sentimentanalyse, maar het model is vooral nuttig zijn wanneer er geen verborgen betekenis of tekstonderdeel tot de inhoud is. Irony, sarcasme humor en op dezelfde manier genuanceerd inhoud, is afhankelijk van culturele context en normen bedoeling duidelijk. Dit type inhoud is tussen de meest uitdagende te analyseren. Normaal gesproken is het grootste verschil tussen een bepaald scorebereik die worden geproduceerd door de analyzer en een subjectief beoordeling door een mens voor inhoud met genuanceerd betekenis.

## <a name="can-i-add-my-own-training-data-or-models"></a>Kan ik mijn eigen trainingsgegevens of modellen toevoegen?

Nee, de modellen zijn dat. De enige bewerkingen op de geüploade gegevens worden scoren, sleuteltermextractie en taaldetectie. We aangepaste modellen gehost. Als u wilt maken en hosten van aangepaste machine learning-modellen, houd rekening met de [machine learning-mogelijkheden in Microsoft R Server](https://docs.microsoft.com/r-server/r/concept-what-is-the-microsoftml-package).

## <a name="can-i-request-additional-languages"></a>Kan ik vragen om extra talen?

Sentimentanalyse en sleuteltermextractie zijn beschikbaar voor een [selecteert u het aantal talen](text-analytics-supported-languages.md). Verwerking van natuurlijke taal is complex en vereist aanzienlijk testen voordat de nieuwe functionaliteit kan worden vrijgegeven. Om deze reden vermijden we ondersteuning vooraf aankondiging zodat niemand wordt afhankelijk van functionaliteit die nodig heeft meer tijd om te vervallen. 

Zodat we prioriteit geven aan welke talen om te werken op volgende stemmen voor specifieke talen op [User Voice](https://cognitive.uservoice.com/forums/555922-text-analytics). 

## <a name="why-does-key-phrase-extraction-return-some-words-but-not-others"></a>Waarom retourneert sleuteltermextractie sommige woorden, maar niet voor andere?

Sleuteltermextractie wordt voorkomen dat niet-essentiële woorden en bijvoeglijke van zelfstandige naamwoorden. Bijvoeglijk naamwoord-zelfstandig naamwoord combinaties, zoals "spectaculaire weergaven" of "duidelijk weer" worden samen geretourneerd.

Over het algemeen bestaat de uitvoer van zelfstandige naamwoorden en objecten van de zin. Uitvoer wordt weergegeven in de volgorde van prioriteit, met de eerste woordgroep wordt de belangrijkste. Urgentie wordt gemeten door het aantal keren dat die een bepaald concept wordt vermeld of de relatie van dat element op andere elementen in de tekst.

## <a name="why-does-output-vary-given-identical-inputs"></a>Waarom uitvoer verschilt, opgegeven identieke invoer?

Verbeteringen van modellen en algoritmes zijn als de wijziging is belangrijke of stil bijbehorende in de service als de update secundaire is aangekondigd. Na verloop van tijd vindt u misschien dat dezelfde tekst invoer resultaten in een andere gevoelsscore of sleuteluitdrukkingen uitvoer. Dit is normaal en opzettelijke gevolg van het gebruik van beheerde machine learning-resources in de cloud.

## <a name="next-steps"></a>Volgende stappen

Uw vraag over een ontbrekende functie of functionaliteit is? Houd rekening met aanvragen of uw stem op deze op onze [UserVoice-website](https://cognitive.uservoice.com/forums/555922-text-analytics).

## <a name="see-also"></a>Zie ook

 [StackOverflow: Text Analytics-API   
 [StackOverflow: Cognitive Services](http://stackoverflow.com/questions/tagged/microsoft-cognitive)
