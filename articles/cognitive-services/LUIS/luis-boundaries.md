---
title: Limieten
titleSuffix: Language Understanding - Azure Cognitive Services
description: In dit artikel bevat de bekende beperkingen van Azure Cognitive Services Language Understanding (LUIS). LUIS heeft verschillende gebieden van de grens. Model grens bepaalt intents, entiteiten en functies van LUIS. De quotalimieten op basis van het type sleutel. Toetscombinatie bepaalt de LUIS-website.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 01/28/2019
ms.author: diberry
ms.custom: seodec18
ms.openlocfilehash: 592b8500f9b032a20bb991326201d45db586980d
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55177511"
---
# <a name="boundaries-for-your-luis-model-and-keys"></a>Grenzen voor uw LUIS-model en de sleutels
LUIS heeft verschillende gebieden van de grens. De eerste is de [model grens](#model-boundaries), welke besturingselementen intents, entiteiten en functies van LUIS. Het tweede gedeelte [quotalimieten](#key-limits) op basis van het type sleutel. Is van een derde deel van de grenzen van de [combinatie op het toetsenbord](#keyboard-controls) voor het beheren van de website van LUIS. Een vierde gebied is de [world regiotoewijzing](luis-reference-regions.md) tussen de LUIS website ontwerpen en de LUIS [eindpunt](luis-glossary.md#endpoint) API's. 


## <a name="model-boundaries"></a>Model grenzen

|Onderwerp|Limiet|
|--|:--|--|
| [App-naam][luis-get-started-create-app] | * Standaard teken max |
| [Batch testen][batch-testing]| 10 gegevenssets, 1000 uitingen per gegevensset|
| Expliciete lijst | 50 per toepassing|
| [Intents][intents]|500 per toepassing: 499 aangepaste intents en de vereiste _geen_ intentie.<br>[Op basis van verzending](https://aka.ms/dispatch-tool) toepassing heeft een bijbehorende 500 verzending bronnen.|
| [Lijst met entiteiten](./luis-concept-entity-types.md) | Bovenliggend item: 50, onderliggende: maximaal 20.000 items. Canonieke naam is * standaard teken max. Synoniem waarden hebben geen lengtebeperking. |
| [Entiteiten machine geleerd](./luis-concept-entity-types.md):<br> Composite,<br>  Hiërarchische<br> Eenvoudig|Een limiet van 100 bovenliggende entiteiten (dit is exclusief hiërarchische kinderen) of 330 entiteiten (inclusief hiërarchische kinderen), afhankelijk van wat beperken de treffers van de gebruiker eerst.<br><br>Een voorbeeld van met hiërarchie is 30 hiërarchieën met 10 kinderen.  De onderliggende objecten Totaal 300 verbruiken en de hiërarchie-elementen de resterende 30 verbruiken. |
| [Patronen](luis-concept-patterns.md)|500 patronen per toepassing.<br>Maximale lengte van het patroon is 400 tekens.<br>3 Pattern.any entiteiten per patroon<br>Maximaal 2 geneste optionele tekst in het patroon|
| [Pattern.any](./luis-concept-entity-types.md)|100 per toepassing, 3 pattern.any entiteiten per patroon |
| [Woordgroepenlijst met][phrase-list]|10 woordgroep lijsten, 5000 items per lijst|
| [Vooraf gemaakte entiteiten](./luis-prebuilt-entities.md) | geen limiet|
| [Reguliere expressie entiteiten](./luis-concept-entity-types.md)|20 entiteiten<br>Max. 500 tekens. per entiteit patroon van reguliere expressie|
| [Rollen](luis-concept-roles.md)|300 rollen per toepassing. 10 rollen per entiteit|
| [Utterance][utterances] | 500 tekens bevatten|
| [Uitingen][utterances] | 15.000 per toepassing|
| [Versies](luis-concept-version.md)| geen limiet |
| [Versienaam][luis-how-to-manage-versions] | beperkt tot alleen alfanumerieke tekens en periode 10 tekens (.) |

* Standaard teken maximaal is 50 tekens. 

## <a name="intent-and-entity-naming"></a>Doel en de naamgeving van entiteit
Gebruik de volgende tekens niet in de namen van intentie en entiteit:

|Teken|Name|
|--|--|
|`{`|Accolade links|
|`}`|Accolade rechts|
|`[`|Haakje openen|
|`]`|Haakje sluiten|
|`\`|Backslash|

## <a name="key-usage"></a>Sleutelgebruik

Taal begrijpen heeft afzonderlijke sleutels, één type voor het ontwerpen en één type voor het uitvoeren van query's het eindpunt van de voorspelling. Zie voor meer informatie over de verschillen tussen sleuteltypen [ontwerpen en query voorspelling endpoint-sleutels in LUIS](luis-concept-keys.md).

## <a name="key-limits"></a>Limieten

De ontwerphandleiding sleutel heeft verschillende beperkingen bij het ontwerpen en -eindpunt. De sleutel van LUIS-service-eindpunt is alleen geldig voor eindpunt query's.


|Sleutel|Ontwerpen|Eindpunt|Doel|
|--|--|--|--|
|Language Understanding ontwerpen/Starter|1 miljoen/maand, 5 per seconde|1-1000/maand, 5 per seconde|Uw LUIS-app ontwerpen|
|Language Understanding [abonnement] [ pricing] - F0 - gratis-laag |ongeldig|10 duizend/maand, 5 per seconde|Uitvoeren van query's uw LUIS-eindpunt|
|Language Understanding [abonnement] [ pricing] - S0 - Basic-laag|ongeldig|50 per seconde|Uitvoeren van query's uw LUIS-eindpunt|
|Cognitive Service [abonnement] [ pricing] - S0 - Standard-laag|ongeldig|50 per seconde|Uitvoeren van query's uw LUIS-eindpunt|
|[Integratie van sentiment-analyse](luis-how-to-publish-app.md#enable-sentiment-analysis)|ongeldig|Er zijn geen kosten in rekening gebracht|Sentiment informatie, inclusief gegevens sleuteltermextractie toe te voegen |
|Spraak-integratie|ongeldig|$5,50 USD/1 duizend eindpunt aanvragen|Gesproken utterance converteren naar tekst utterance en LUIS resultaten geretourneerd|

## <a name="keyboard-controls"></a>Toetsenbord

|Toetsenbordinvoer | Description | 
|--|--|
|Besturingselement + E|Hiermee schakelt u tussen tokens en entiteiten in de lijst met uitingen|

## <a name="website-sign-in-time-period"></a>Website zich in een tijdsperiode

Uw aanmelding toegang wordt toegestaan voor **60 minuten**. Na deze periode ontvangt u deze fout. U moet zich opnieuw aanmelden.

[luis-get-started-create-app]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-get-started-create-app
[batch-testing]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-concept-test#batch-testing
[intents]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-concept-intent
[phrase-list]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-concept-feature
[utterances]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-concept-utterance
[luis-how-to-manage-versions]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-how-to-manage-versions
[pricing]: https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/
<!-- TBD: fix this link -->
[speech-to-intent-pricing]: https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/
