---
title: LUIS iteratieve app begrijpen ontwerpen - Azure | Microsoft Docs
description: LUIS apps nodig ontwerp iteraties voor het trainen van LUIS ophalen van de beste ophalen van gegevens.
services: cognitive-services
author: v-geberr
manager: kamran.iqbal
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 02/12/2018
ms.author: v-geberr
ms.openlocfilehash: b7f8dd46dc8289322726934f330761b0f1ab94bd
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/19/2018
ms.locfileid: "36265934"
---
# <a name="authoring-cycle"></a>Cyclus ontwerpen
LUIS leert best in een herhalende cyclus van wijzigingen in het gegevensmodel, utterance voorbeelden, publiceren en verzamelen van gegevens van de endpoint-query's. 

![Cyclus ontwerpen](./media/luis-concept-app-iteration/iteration.png)

## <a name="building-a-luis-model"></a>Een model LUIS bouwen
De model-doel is om te achterhalen wat de gebruiker vraagt om (de bedoeling of bedoeling) en welke onderdelen van de vraag informatie (entiteiten) die helpen bij het bepalen van het antwoord. 

Het model moet specifiek zijn voor het app-domein om te bepalen woorden en woordgroepen dat zijn relevant, evenals een typische word bestellen. 

Het model omvat hetzelfde doel entiteiten. 

## <a name="add-training-examples"></a>Training voorbeelden toevoegen
LUIS moet voorbeeld utterances in de intents. De voorbeelden moeten voldoende variant van word keuze en word volgorde om te bepalen welk doel de utterance is bedoeld voor. Elke utterance voorbeeld moet alle vereiste gegevens entiteiten met het label hebben. 

U LUIS voor het negeren van utterances die niet relevant zijn voor uw app-domein door toe te wijzen de utterance aan de opdracht geven de **geen** bedoeld. De woorden of zinnen u geen hoeft opgehaald uit een utterance hoeft niet te worden gelabeld. Er is geen naam voor de woorden of zinnen te negeren. 
<!--
## Not just yet
Do not add features such as a [phrase list](luis-concept-feature.md) feature in your first cycle. Phrase lists are phrases that would be specific to your app's subject area.  
-->
## <a name="train-and-publish-the-app"></a>Trainen en publiceer de app
Zodra u verschillende utterances 10 tot en met 15 in elk doel met de vereiste entiteiten met het label hebt, u LUIS trainen en vervolgens publiceren om uw eindpunten. Zorg ervoor dat u uw app maken en publiceren van uw app zodat deze beschikbaar zijn in de [eindpunt regio's](luis-reference-regions.md) u nodig hebt. 

## <a name="https-endpoint-testing"></a>HTTPS-eindpunt testen
U kunt testen uw app LUIS van het HTTPS-eindpunt weergegeven op de **[publiceren](publishapp.md)** pagina. Testen van het eindpunt kan LUIS kiezen eventuele utterances met laag-vertrouwen voor revisie.  

## <a name="recycle"></a>Prullenbak
Wanneer u klaar bent met een cyclus van het ontwerpen, kunt u opnieuw beginnen. Beginnen met het controleren van endpoint-utterances die LUIS gemarkeerd met weinig vertrouwen. Controleer deze utterances voor zowel de bedoeling als de entiteit. Zodra u utterances bekijken, moet de revisie-lijst niet leeg zijn.  

## <a name="batch-testing"></a>Batch testen
Testen van de batch is een manier om te zien hoeveel voorbeeld utterances door LUIS zijn berekend. De voorbeelden moeten nieuwe LUIS en correct moeten worden gelabeld met opzet en entiteiten LUIS gewenste vinden. De testresultaten geven aan hoe goed LUIS op die set met utterances wilt uitvoeren. 

## <a name="next-steps"></a>Volgende stappen

Meer informatie over concepten over [samenwerking](luis-concept-collaborator.md).

[luis-reference-prebuilt-domains]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-prebuilt-domains