---
title: Language Understanding (LUIS) grenzen | Microsoft Docs
titleSuffix: Azure
description: In dit artikel bevat een bekende beperkingen van LUIS.
services: cognitive-services
author: v-geberr
manager: kamran.iqbal
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 05/07/2018
ms.author: v-geberr
ms.openlocfilehash: c1f49f88cb4772da636bd1396b09e5efd357fb60
ms.sourcegitcommit: 756f866be058a8223332d91c86139eb7edea80cc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2018
ms.locfileid: "37344118"
---
# <a name="luis-boundaries"></a>LUIS grenzen
LUIS heeft verschillende gebieden van de grens. De eerste is de [model grens](#model-boundaries), welke besturingselementen intents, entiteiten en functies van LUIS. Het tweede gedeelte [quotalimieten](#key-limits) op basis van het type sleutel. Is van een derde deel van de grenzen van de [combinatie op het toetsenbord](#keyboard-controls) voor het beheren van de website van LUIS. Een vierde gebied is de [world regiotoewijzing](luis-reference-regions.md) tussen de LUIS website ontwerpen en de LUIS [eindpunt](luis-glossary.md#endpoint) API's. 


## <a name="model-boundaries"></a>Model grenzen

|Onderwerp|Limiet|
|--|:--|--|
| [App-naam][luis-get-started-create-app] | * Standaard teken max |
| [Batch testen][batch-testing]| 10 gegevenssets, 1000 uitingen per gegevensset|
| **[Samengestelde](./luis-concept-entity-types.md)|100 met maximaal 10 onderliggende items |
| Expliciete lijst | 50 per toepassing|
| **[Hiërarchische](./luis-concept-entity-types.md) |100 met maximaal 10 onderliggende items |
| [Intents][intents]|500 per toepassing<br>[Op basis van verzending](https://github.com/Microsoft/botbuilder-tools/tree/master/Dispatch) toepassing heeft een bijbehorende 500 verzending bronnen|
| [Lijst met entiteiten](./luis-concept-entity-types.md) | Bovenliggende: 50, onderliggende: maximaal 20.000 items. Canonieke naam is * standaard teken max. Synoniemen hebben geen lengtebeperking. |
| [Patronen](luis-concept-patterns.md)|500 patronen per toepassing.<br>Maximale lengte van het patroon is 400 tekens.<br>3 Pattern.any entiteiten per patroon<br>Maximaal 2 geneste optionele tekst in het patroon|
| [Pattern.any](./luis-concept-entity-types.md)|100 per toepassing, 3 pattern.any entiteiten per patroon |
| [Woordgroepenlijst met][phrase-list]|10 woordgroep lijsten, 5000 items per lijst|
| [Vooraf gemaakte entiteiten](./luis-prebuilt-entities.md) | geen limiet|
| [Reguliere expressie entiteiten](./luis-concept-entity-types.md)|20 entiteiten<br>Max. 500 tekens. per entiteit patroon van reguliere expressie|
| [Rollen](luis-concept-roles.md)|300 rollen per toepassing. 10 rollen per entiteit|
| **[Eenvoudige](./luis-concept-entity-types.md)| 100 entiteiten|
| [Utterance][utterances] | 500 tekens bevatten|
| [Uitingen][utterances] | 15.000 per toepassing|
| [Versienaam][luis-how-to-manage-versions] | beperkt tot alleen alfanumerieke tekens en periode 10 tekens (.) |

* Standaard teken maximaal is 50 tekens. 

** Het totale aantal eenvoudige, hiërarchische en samengestelde entiteiten niet langer zijn dan 100. Het totale aantal hiërarchische entiteiten, samengestelde entiteiten, eenvoudige entiteiten en hiërarchische onderliggende entiteiten niet langer zijn dan 330. 

## <a name="intent-and-entity-naming"></a>Doel en de naamgeving van entiteit
Gebruik de volgende tekens niet in de namen van intentie en entiteit:

|Teken|Naam|
|--|--|
|`{`|Accolade links|
|`}`|Accolade rechts|
|`[`|Haakje openen|
|`]`|Haakje sluiten|
|`\`|Backslash|

## <a name="key-limits"></a>Limieten
De ontwerphandleiding sleutel heeft verschillende beperkingen bij het ontwerpen en -eindpunt. De sleutel van LUIS-service-eindpunt is alleen geldig voor eindpunt query's.

|Sleutel|Ontwerpen|Eindpunt|Doel|
|--|--|--|--|
|Ontwerpen/Starter|1 miljoen/maand, 5 per seconde|1-1000/maand, 5 per seconde|Uw LUIS-app ontwerpen|
|[Abonnement] [ pricing] - F0 - gratis-laag |ongeldig|10 duizend/maand, 5 per seconde|Uitvoeren van query's uw LUIS-eindpunt|
|[Abonnement] [ pricing] - S0 - Basic-laag|ongeldig|50 per seconde|Uitvoeren van query's uw LUIS-eindpunt|
|[Integratie van sentiment-analyse](luis-how-to-publish-app.md#enable-sentiment-analysis)|ongeldig|Er zijn geen kosten in rekening gebracht|Sentiment informatie, inclusief gegevens sleuteltermextractie toe te voegen |
|Spraak-integratie|ongeldig|$5,50 USD/1 duizend eindpunt aanvragen|Gesproken utterance converteren naar tekst utterance en LUIS resultaten geretourneerd|

## <a name="keyboard-controls"></a>Toetsenbord

|Toetsenbordinvoer | Beschrijving | 
|--|--|
|Besturingselement + E|Hiermee schakelt u tussen tokens en entiteiten in de lijst met uitingen|

## <a name="website-sign-in-time-period"></a>Website zich in een tijdsperiode

Uw aanmelding toegang wordt toegestaan voor **60 minuten**. Na deze periode ontvangt u deze fout. U moet zich opnieuw aanmeldt.

[luis-get-started-create-app]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-get-started-create-app
[batch-testing]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-concept-test#batch-testing
[intents]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-concept-intent
[phrase-list]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-concept-feature
[utterances]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-concept-utterance
[luis-how-to-manage-versions]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-how-to-manage-versions
[pricing]: https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/
<!-- TBD: fix this link -->
[speech-to-intent-pricing]: https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/
