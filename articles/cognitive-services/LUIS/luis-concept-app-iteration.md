---
title: Iteratieve app-ontwerp in Language Understanding (LUIS)
titleSuffix: Azure Cognitive Services
description: LUIS leert beste in een iteratief cyclus van wijzigingen in het gegevensmodel, utterance voorbeelden, publiceren en verzamelen van gegevens van eindpunt query's.  LUIS-apps vereisen ontwerp iteraties met het trainen van LUIS om op te halen van de beste gegevensextractie.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: conceptual
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: 2bd30aad995e9d1f334988652477f8b017c187b9
ms.sourcegitcommit: 17633e545a3d03018d3a218ae6a3e4338a92450d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/22/2018
ms.locfileid: "49638287"
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

## <a name="train-and-publish-the-app"></a>De app trainen en publiceren
Zodra u verschillende uitingen van 10 tot en met 15 in elk doel hebt, met de vereiste entiteiten met het label, trainen en publiceren. Gebruik de koppeling om op te halen van de eindpunten van de melding publiceren geslaagd. Zorg ervoor dat u uw app maken en publiceren van uw app zodat deze beschikbaar zijn in de [eindpunt regio's](luis-reference-regions.md) u nodig hebt. 

## <a name="https-endpoint-testing"></a>HTTPS-eindpunt testen
U kunt uw LUIS-app van het HTTPS-eindpunt testen. Testen van het eindpunt kunt LUIS alle uitingen met laag-vertrouwen ter beoordeling te kiezen.  

## <a name="recycle"></a>Prullenbak
Wanneer u klaar bent met een cyclus van het ontwerpen, kunt u opnieuw beginnen. Beginnen met het controleren van endpoint-uitingen die LUIS gemarkeerd met weinig vertrouwen. Controleer deze uitingen voor zowel de intentie en de entiteit. Zodra u uitingen bekijkt, moet de beoordeling-lijst niet leeg zijn.  

## <a name="batch-testing"></a>Batchgewijs testen
Testen van de batch is een manier om te zien hoeveel voorbeeld-uitingen door LUIS zijn beoordeeld. De voorbeelden moeten bekend bent met LUIS en correct moeten worden voorzien van intentie en entiteiten die u wilt dat LUIS om te zoeken. De testresultaten geven aan hoe goed LUIS op die set met uitingen zou uitvoeren. 

## <a name="next-steps"></a>Volgende stappen

Kennis met concepten over [samenwerking](luis-concept-collaborator.md).