---
title: Evalueren van nauwkeurigheid voor aangepaste spraak - spraakservices
titlesuffix: Azure Cognitive Services
description: In dit document leert u hoe u de kwaliteit van de spraak-naar-tekst-model van Microsoft of uw aangepaste kwantitatief te meten. Audio + human etiket transcriptie gegevens is vereist voor het testen van nauwkeurigheid en 30 minuten tot vijf uur representatieve audio moet worden opgegeven.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: erhopf
ms.openlocfilehash: 7c1b3602b09c1a129923180c4b1d4a5f8293de2c
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/02/2019
ms.locfileid: "65025907"
---
# <a name="evaluate-custom-speech-accuracy"></a>Aangepaste spraak nauwkeurigheid evalueren

In dit document leert u hoe u voor het meten van de kwaliteit van de spraak-naar-tekst-model van Microsoft of uw aangepaste kwantitatief. Audio + human etiket transcriptie gegevens is vereist voor het testen van nauwkeurigheid en 30 minuten tot vijf uur representatieve audio moet worden opgegeven.

## <a name="what-is-word-error-rate-wer"></a>Wat is er Word fout tarief (WER)?

De industriestandaard voor het meten van de nauwkeurigheid van model is *Word Foutfrequentie* (Windows Foutrapportage). Windows Foutrapportage telt het aantal onjuiste woorden die zijn geïdentificeerd tijdens de opname, vervolgens wordt gedeeld door het totale aantal woorden die zijn opgegeven in het transcript human-met het label. Dit nummer wordt ten slotte, vermenigvuldigd met 100% voor het berekenen van de Windows Foutrapportage.

![Windows Foutrapportage formule](./media/custom-speech/custom-speech-wer-formula.png)

Onjuist geïdentificeerd woorden kunnen worden onderverdeeld in drie categorieën:

* Invoegen (I): Woorden die onjuist zijn toegevoegd in het transcript hypothese
* Verwijderen (D): Woorden die gevonden in het transcript hypothese zijn
* Vervanging (S): Woorden die zijn vervangen tussen de bron- en hypothese

Hier volgt een voorbeeld:

![Voorbeeld van een onjuist geïdentificeerde woorden](./media/custom-speech/custom-speech-dis-words.png)

## <a name="resolve-errors-and-improve-wer"></a>Los problemen op en verbeteren van Windows Foutrapportage

De Windows Foutrapportage uit de resultaten van de machine-herkenning kunt u de kwaliteit van het model dat u met uw app, een hulpprogramma of een product evalueren. Een Windows Foutrapportage van 5-10% wordt beschouwd als goede kwaliteit en is klaar voor gebruik. Een Windows Foutrapportage van 20% is acceptabel, maar kunt u rekening houden met extra training. Een Windows Foutrapportage van 30% of meer signalen van slechte kwaliteit en aanpassing en training vereist.

Hoe de fouten zijn verdeeld is belangrijk. Als veel verwijderingsfouten optreden, komt dit meestal vanwege zwakke audio signaalsterkte. U lost dit probleem, moet u voor het verzamelen van audio gegevens dichter bij de bron. Invoegen fouten betekenen dat de audio is vastgelegd in een omgeving met veel ruis en aanwezig is, is door elkaar praten mogelijk erkenning problemen veroorzaken. Vervanging fouten vaak optreden wanneer een onvoldoende voorbeeld van domeinspecifieke voorwaarden is opgegeven als een van beide transcripties human etiket of gerelateerde tekst.

Door het analyseren van afzonderlijke bestanden, kunt u bepalen welk type fouten bestaan, en welke fouten zijn uniek is voor een specifiek bestand. Inzicht in problemen op het niveau van ziet u een gericht op verbeteringen.

## <a name="create-a-test"></a>Een test maken

Als u wilt testen van de kwaliteit van de spraak-naar-tekst basislijn-model van Microsoft of een aangepast model dat u hebt getraind, kunt u twee modellen met elkaar om te evalueren nauwkeurigheid kunt vergelijken. De vergelijking neemt de resultaten van Windows Foutrapportage en herkenning. Normaal gesproken wordt een aangepast model vergeleken met de Microsoft baseline model.

Om te evalueren van modellen naast elkaar:

1. Navigeer naar **spraak-naar-tekst > aangepaste spraak > testen**.
2. Klik op **Test toevoegen**.
3. Selecteer **evalueren nauwkeurigheid**. Geef de test een naam en beschrijving, en selecteer uw gegevensset audio + human etiket transcriptie.
4. Selecteer maximaal twee modellen die u wilt testen.
5. Klik op **Create**.

Nadat de test is gemaakt, kunt u de resultaten naast elkaar kunt vergelijken.

## <a name="side-by-side-comparison"></a>Vergelijking van de side-by-side

Zodra de test voltooid is, wordt aangegeven door de status gewijzigd in *geslaagd*, vindt u enkele van de Windows Foutrapportage voor beide modellen die zijn opgenomen in de test. Klik op de testnaam van de om de detailpagina van de tests weer te geven. Deze pagina geeft een lijst van alle uitingen in uw gegevensset, die wijzen op de resultaten van de twee modellen naast de transcriptie uit de opgegeven gegevensset. Om te controleren van de side-by-side-vergelijking, kunt u verschillende fouttypen, inclusief invoegen, verwijderen en vervangen van kunt schakelen. Door te luisteren naar de audio en vergelijken herkenningsresultaten in elke kolom, waarin de transcriptie human-met het label en de resultaten voor de twee modellen voor spraak-naar-tekst, kunt u bepalen welk model u voldoet aan uw behoeften en waarvoor extra training en verbeteringen zijn Vereist.

## <a name="next-steps"></a>Volgende stappen

* [Uw model te trainen](how-to-custom-speech-train-model.md)
* [Uw model implementeren](how-to-custom-speech-deploy-model.md)

## <a name="additional-resources"></a>Aanvullende resources

* [Voorbereiden en testen van uw gegevens](how-to-custom-speech-test-data.md)
* [Controleer uw gegevens](how-to-custom-speech-inspect-data.md)
