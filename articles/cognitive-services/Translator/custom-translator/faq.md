---
title: Veelgestelde vragen - aangepaste Translator
titleSuffix: Azure Cognitive Services
description: Vindt u antwoorden op veelgestelde vragen over de Translator aangepast.
author: rajdeep-in
manager: christw
ms.service: cognitive-services
ms.subservice: custom-translator
ms.date: 02/21/2019
ms.author: v-rada
ms.topic: reference
ms.openlocfilehash: b09b2cd3a7a642debb28de7ce71a2b3af4f558a8
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/23/2019
ms.locfileid: "56730254"
---
# <a name="frequently-asked-questions"></a>Veelgestelde vragen

In dit artikel bevat antwoorden op veelgestelde vragen over [aangepaste Translator](https://portal.customtranslator.azure.ai).

## <a name="what-are-the-current-restrictions-in-custom-translator"></a>Wat zijn de huidige beperkingen in aangepaste Translator?

Er zijn beperkingen en beperkingen met betrekking tot de bestandsgrootte, modeltraining en implementeren van modellen. Houd er rekening mee met deze beperkingen bij het instellen van uw training om een model in aangepaste Translator te bouwen.

- Ingediende bestanden moet kleiner zijn dan 100 MB groot zijn.
- Ééntalig gegevens wordt niet ondersteund.

## <a name="when-should-i-request-deployment-for-a-translation-system-that-has-been-trained"></a>Wanneer moet ik implementatie voor een NAT-systeem dat is getraind aanvragen?

Het duurt enkele trainingen te maken van de optimale vertaalsysteem voor uw project. U kunt om te proberen met behulp van meer trainingsgegevens of meer zorgvuldig gefilterde gegevens als de score BLEU en / of de resultaten van de zijn niet voldoende. U moet strikte en zorg ervoor dat bij uw afstemmen instellen en uw test ingesteld, op volledig representatief zijn voor de terminologie en stijl van materiaal dat u wilt converteren. U kunt bij het schrijven van uw trainingsgegevens hoeverre en Experimenteer met verschillende opties. De systeemimplementatie van een aanvragen wanneer u tevreden met de vertalingen in uw testresultaten systeem bent niet meer gegevens toevoegen aan de training voor het verbeteren van uw getrainde systeem hebt en u wilt toegang krijgen tot het getrainde model via API's.

## <a name="how-many-trained-systems-can-be-deployed-in-a-project"></a>Hoeveel getrainde systemen kunnen worden geïmplementeerd in een project?

Slechts één getrainde systeem kan worden geïmplementeerd per project. Het duurt enkele trainingen te maken van een systeem geschikte vertaling voor uw project en we raden u aan de implementatie van een training waarmee u kunt het beste resultaat aanvragen. U kunt de kwaliteit van de training bepalen door de score BLEU (hoger is beter), en door overleg met revisoren voordat u besluit dat de kwaliteit van vertalingen geschikt voor implementatie is.

## <a name="when-can-i-expect-my-trainings-to-be-deployed"></a>Wanneer kan ik verwachten van Mijn trainingen om te worden geïmplementeerd?

De implementatie duurt meestal minder dan een uur.

## <a name="how-do-you-access-a-deployed-system"></a>Hoe opent u een geïmplementeerde systeem?

Geïmplementeerde systemen kunnen worden geopend via de Microsoft Translator Text-API V3 door de categorie-id op te geven. Meer informatie over de Translator Text-API kan worden gevonden de [API-verwijzing](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference) webpagina.

## <a name="how-do-i-skip-alignment-and-sentence-breaking-if-my-data-is-already-sentence-aligned"></a>Hoe ik uitlijning en zin belangrijke als mijn gegevens nog zin uitgelijnd overslaan?

De aangepaste-vertaler wordt overgeslagen zin uitlijning en zin belangrijke voor TMX bestanden en tekstbestanden met de `.align` extensie. `.align` bestanden geeft gebruikers een optie voor het overslaan van aangepaste Translator-zin te analyseren en uitlijning van proces voor de bestanden die zijn perfect uitgelijnd en er is geen verdere verwerking. Wordt u aangeraden `.align` -extensie alleen voor bestanden die zijn perfect uitgelijnd.

Als het aantal opgehaalde zinnen komt niet overeen met de twee bestanden met dezelfde basisnaam, aangepaste Translator nog steeds de zin aligner wordt uitgevoerd op `.align` bestanden.

## <a name="i-tried-uploading-my-tmx-but-it-says-document-processing-failed"></a>Ik heb geprobeerd mijn TMX uploaden, maar de status 'verwerking van documenten is mislukt'.

Zorg ervoor dat de TMX aan de TMX 1.4b voldoet specificatie op <https://www.gala-global.org/tmx-14b>.
