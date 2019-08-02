---
title: Herhalend app-ontwerp-LUIS
titleSuffix: Azure Cognitive Services
description: LUIS leert beste in een iteratief cyclus van wijzigingen in het gegevensmodel, utterance voorbeelden, publiceren en verzamelen van gegevens van eindpunt query's.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 07/29/2019
ms.author: diberry
ms.openlocfilehash: 2a540606a6f9cfa790a2244628e7f0b7bef35986
ms.sourcegitcommit: 3877b77e7daae26a5b367a5097b19934eb136350
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/30/2019
ms.locfileid: "68639257"
---
# <a name="authoring-cycle-for-your-luis-app"></a>Ontwerp cyclus voor uw LUIS-app
LUIS leert beste in een iteratief cyclus van wijzigingen in het gegevensmodel, utterance voorbeelden, publiceren en verzamelen van gegevens van eindpunt query's. 

![Ontwerpcyclus](./media/luis-concept-app-iteration/iteration.png)

## <a name="building-a-luis-model"></a>Het bouwen van een LUIS-model
Doel van het model is om te achterhalen wat de gebruiker wordt gevraagd (de bedoeling of kunt u lezen wat) en bepaalt u welke onderdelen van de vraag geven details (entiteiten) die helpen bij het bepalen van het antwoord. 

Het model moet specifiek zijn voor het app-domein om te bepalen van woorden en woordgroepen dat zijn relevant zijn als normale word bestellen. 

Het model vereist intenties en _moet entiteiten hebben_ . 

## <a name="add-training-examples"></a>Voorbeelden van training toevoegen
LUIS moet voorbeeld uitingen in de intents. De voorbeelden moeten voldoende variatie van word keuze en woordvolgorde kunnen om te bepalen op welk doel de utterance is bedoeld voor. Elke voorbeeld-utterance moet alle vereiste gegevens gelabeld als entiteiten bevatten. 

U LUIS negeren uitingen die niet relevant zijn voor het domein van uw app door toe te wijzen de utterance te instrueren de **geen** intentie. Woorden of zinsdelen u niet hoeft opgehaald uit een utterance hoeft te worden met het label. Er is geen label naar woorden of zinsdelen te negeren. 

## <a name="train-and-publish-the-app"></a>De app trainen en publiceren
Wanneer u 15 tot 30 verschillende uitingen in elke intentie hebt, met de vereiste entiteiten met het label, moet u de [training](luis-how-to-train.md) vervolgens [publiceren](luis-how-to-publish-app.md). Gebruik de koppeling om op te halen van de eindpunten van de melding publiceren geslaagd. Zorg ervoor dat u uw app maakt en publiceert zodat deze beschikbaar is in de [eindpunt regio's](luis-reference-regions.md) die u nodig hebt. 

## <a name="https-endpoint-testing"></a>HTTPS-eindpunt testen
U kunt uw LUIS-app van het HTTPS-eindpunt testen. Door te testen vanaf het eind punt kan LUIS elke uitingen met weinig vertrouwen voor [beoordeling](luis-how-to-review-endpoint-utterances.md)kiezen.  

## <a name="recycle"></a>Prullenbak

Wanneer u klaar bent met een cyclus van het ontwerpen, kunt u opnieuw beginnen. Begin met het controleren van het uitingen Luis- [eind punt](luis-how-to-review-endpoint-utterances.md) dat is gemarkeerd met een lage betrouw baarheid. Controleer deze uitingen voor zowel de intentie en de entiteit. Zodra u uitingen bekijkt, moet de beoordeling-lijst niet leeg zijn.  

U [](luis-concept-version.md#clone-a-version) kunt de huidige versie in een nieuwe versie klonen en vervolgens de ontwerp wijzigingen in de nieuwe versie starten. 

## <a name="batch-testing"></a>Batchgewijs testen

[Batch tests](luis-concept-batch-test.md) zijn een manier om te zien hoeveel voor beeld-uitingen worden beoordeeld door Luis. De voorbeelden moeten bekend bent met LUIS en correct moeten worden voorzien van intentie en entiteiten die u wilt dat LUIS om te zoeken. De testresultaten geven aan hoe goed LUIS op die set met uitingen zou uitvoeren. 

## <a name="next-steps"></a>Volgende stappen

Kennis met concepten over [samenwerking](luis-concept-collaborator.md).
