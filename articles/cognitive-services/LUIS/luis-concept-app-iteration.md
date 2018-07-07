---
title: Inzicht in iteratieve LUIS-app ontwerpen - Azure | Microsoft Docs
description: LUIS-apps vereisen ontwerp iteraties met het trainen van LUIS om op te halen van de beste gegevensextractie.
services: cognitive-services
author: v-geberr
manager: kamran.iqbal
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 02/12/2018
ms.author: v-geberr
ms.openlocfilehash: e0467e4c41209c937f548edc0c40c05cae588f4c
ms.sourcegitcommit: 11321f26df5fb047dac5d15e0435fce6c4fde663
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/06/2018
ms.locfileid: "37888245"
---
# <a name="authoring-cycle"></a>Ontwerpcyclus
LUIS leert beste in een iteratief cyclus van wijzigingen in het gegevensmodel, utterance voorbeelden, publiceren en verzamelen van gegevens van eindpunt query's. 

![Ontwerpcyclus](./media/luis-concept-app-iteration/iteration.png)

## <a name="building-a-luis-model"></a>Het bouwen van een LUIS-model
Doel van het model is om te achterhalen wat de gebruiker wordt gevraagd (de bedoeling of kunt u lezen wat) en bepaalt u welke onderdelen van de vraag geven details (entiteiten) die helpen bij het bepalen van het antwoord. 

Het model moet specifiek zijn voor het app-domein om te bepalen van woorden en woordgroepen dat zijn relevant zijn als normale word bestellen. 

Het model bevat doel, entiteiten. 

## <a name="add-training-examples"></a>Voorbeelden van training toevoegen
LUIS moet voorbeeld uitingen in de intents. De voorbeelden moeten voldoende variatie van word keuze en woordvolgorde kunnen om te bepalen op welk doel de utterance is bedoeld voor. Elke voorbeeld-utterance moet alle vereiste gegevens gelabeld als entiteiten bevatten. 

U LUIS negeren uitingen die niet relevant zijn voor het domein van uw app door toe te wijzen de utterance te instrueren de **geen** intentie. Woorden of zinsdelen u niet hoeft opgehaald uit een utterance hoeft te worden met het label. Er is geen label naar woorden of zinsdelen te negeren. 
<!--
## Not just yet
Do not add features such as a [phrase list](luis-concept-feature.md) feature in your first cycle. Phrase lists are phrases that would be specific to your app's subject area.  
-->
## <a name="train-and-publish-the-app"></a>Trainen en publiceren van de app.
Zodra u verschillende uitingen van 10 tot en met 15 in elk doel, met de vereiste entiteiten met het label hebt, u LUIS trainen en publiceren om op te halen van de eindpunten. Zorg ervoor dat u uw app maken en publiceren van uw app zodat deze beschikbaar zijn in de [eindpunt regio's](luis-reference-regions.md) u nodig hebt. 

## <a name="https-endpoint-testing"></a>HTTPS-eindpunt testen
U kunt testen uw LUIS-app van het HTTPS-eindpunt weergegeven op de **[publiceren](luis-how-to-publish-app.md)** pagina. Testen van het eindpunt kunt LUIS alle uitingen met laag-vertrouwen ter beoordeling te kiezen.  

## <a name="recycle"></a>Prullenbak
Wanneer u klaar bent met een cyclus van het ontwerpen, kunt u opnieuw beginnen. Beginnen met het controleren van endpoint-uitingen die LUIS gemarkeerd met weinig vertrouwen. Controleer deze uitingen voor zowel de intentie en de entiteit. Zodra u uitingen bekijkt, moet de beoordeling-lijst niet leeg zijn.  

## <a name="batch-testing"></a>Batchgewijs testen
Testen van de batch is een manier om te zien hoeveel voorbeeld-uitingen door LUIS zijn beoordeeld. De voorbeelden moeten bekend bent met LUIS en correct moeten worden voorzien van intentie en entiteiten die u wilt dat LUIS om te zoeken. De testresultaten geven aan hoe goed LUIS op die set met uitingen zou uitvoeren. 

## <a name="next-steps"></a>Volgende stappen

Kennis met concepten over [samenwerking](luis-concept-collaborator.md).