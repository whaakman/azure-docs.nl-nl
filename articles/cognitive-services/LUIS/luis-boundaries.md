---
title: Language Understanding (LUIS) grenzen | Microsoft Docs
titleSuffix: Azure
description: Dit artikel bevat bekende beperkingen van LUIS.
services: cognitive-services
author: v-geberr
manager: kamran.iqbal
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 05/07/2018
ms.author: v-geberr
ms.openlocfilehash: 7f46e55e11c4eb68b515a743b0f51392ffc1269e
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/19/2018
ms.locfileid: "36266801"
---
# <a name="luis-boundaries"></a>LUIS grenzen
LUIS heeft verschillende gebieden van de grens. De eerste is de [model grens](#model-boundaries), welke besturingselementen intents entiteiten en functies in LUIS. Het tweede gebied is [quotalimieten](#key-limits) op basis van het type sleutel. Een derde gebied van grenzen is de [combinatie toetsenbord](#keyboard-controls) voor het beheren van de website LUIS. Een vierde gebied is de [toewijzing van world regio](luis-reference-regions.md) tussen de LUIS website ontwerpen en de LUIS [eindpunt](luis-glossary.md#endpoint) API's. 


## <a name="model-boundaries"></a>Model grenzen

|Onderwerp|Limiet|
|--|:--|--|
| [App-naam][luis-get-started-create-app] | * Standaard teken max |
| [Batch testen][batch-testing]| 10 gegevenssets, 1000 utterances per gegevensset|
| **[Samengestelde](./luis-concept-entity-types.md)|100 met maximaal 10 onderliggende objecten |
| Expliciete lijst | 50 per toepassing|
| **[Hiërarchische](./luis-concept-entity-types.md) |100 met maximaal 10 onderliggende objecten |
| [Intents][intents]|500 per toepassing<br>[Op basis van verzending](https://github.com/Microsoft/botbuilder-tools/tree/master/Dispatch) toepassing heeft een bijbehorende 500 dispatch-bronnen|
| [Lijst entiteiten](./luis-concept-entity-types.md) | Bovenliggende: 50, onderliggende: 20.000 items. Canonieke naam * standaard teken max. Synoniemen hebben geen lengtebeperking. |
| [Patronen](luis-concept-patterns.md)|500 patronen per toepassing.<br>Maximale lengte van het patroon is 400 tekens.<br>3 Pattern.any entiteiten per patroon<br>Maximaal 2 geneste optionele teksten in patroon|
| [Pattern.any](./luis-concept-entity-types.md)|100 per toepassing, 3 pattern.any entiteiten per patroon |
| [Lijst van de wachtwoordzin op te geven][phrase-list]|10 woordgroep lijsten, 5.000 items per lijst|
| [Vooraf gedefinieerde entiteiten](./Pre-builtEntities.md) | Geen limiet|
| [Reguliere expressie entiteiten](./luis-concept-entity-types.md)|20 entiteiten<br>maximaal 500 tekens. per reguliere expressie entiteit patroon|
| [Rollen](luis-concept-roles.md)|300 rollen per toepassing. 10 rollen per entiteit|
| **[Eenvoudige](./luis-concept-entity-types.md)| 100 entiteiten|
| [Utterance][utterances] | 500 tekens bevatten|
| [Utterances][utterances] | 15.000 per toepassing|
| [De versienaam van de][luis-how-to-manage-versions] | beperkt tot alfanumerieke en periode 10 tekens (.) |

* Standaard teken max is 50 tekens. 

** Het totale aantal entiteiten eenvoudige, hiërarchische en samengestelde kan niet groter zijn dan 100. Het totale aantal hiërarchische entiteiten, samengestelde entiteiten, eenvoudige entiteiten en hiërarchische kinderen entiteiten kan 330 niet overschrijden. 

## <a name="intent-and-entity-naming"></a>Doel en de naamgeving van entiteit
De volgende tekens niet gebruiken in doel en de entiteit:

|Teken|Naam|
|--|--|
|`{`|Accolade links|
|`}`|Accolade rechts|
|`[`|Haakje openen|
|`]`|Haakje sluiten|
|`\`|backslash|

## <a name="key-limits"></a>Limieten
De authoring sleutel heeft verschillende beperkingen voor ontwerp- en eindpunt. De sleutel LUIS service-abonnement is alleen geldig voor het eindpunt query's.

|Sleutel|Ontwerpen|Eindpunt|Doel|
|--|--|--|--|
|Ontwerpen/Starter|1 miljoen/maand, 5/seconde|1-1000/maand, 5/seconde|Uw app LUIS ontwerpen|
|[Abonnement] [ pricing] - F0 - gratis laag |ongeldig|10 duizend/maand, 5/seconde|Uw eindpunt LUIS opvragen|
|[Abonnement] [ pricing] - S0 - basisstaffel|ongeldig|50/seconde|Uw eindpunt LUIS opvragen|
|[Gevoel Gegevensanalyse-integratie](publishapp.md#enable-sentiment-analysis)|ongeldig|Gratis|Gevoel informatie inclusief uitpakken van sleutel woordgroep gegevens toe te voegen |
|Spraak-integratie|ongeldig|$5,50 USD/1 duizend eindpunt aanvragen|Gesproken utterance converteren naar tekst utterance en het resultaat van de LUIS|

## <a name="keyboard-controls"></a>Toetsenbord

|Toetsenbordinvoer | Beschrijving | 
|--|--|
|Besturingselement + E|Hiermee schakelt u tussen tokens en entiteiten in de lijst utterances|

## <a name="website-sign-in-time-period"></a>Website periode aanmelden

Uw toegang is voor **60 minuten**. Na deze periode, als u krijgt deze fout. U moet zich opnieuw aanmeldt.

[luis-get-started-create-app]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-get-started-create-app
[batch-testing]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-concept-test#batch-testing
[intents]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-concept-intent
[phrase-list]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-concept-feature
[utterances]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-concept-utterance
[luis-how-to-manage-versions]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-how-to-manage-versions
[pricing]: https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/
<!-- TBD: fix this link -->
[speech-to-intent-pricing]: https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/
