---
title: Een model te trainen voor aangepaste spraak - spraakservices
titlesuffix: Azure Cognitive Services
description: Een spraak-naar-tekst training is nodig voor het verbeteren van nauwkeurigheid voor zowel van Microsoft baseline model of een aangepast model dat u van plan bent om te maken. Een model wordt getraind met behulp van transcriptie human-met het label en bijbehorende tekst. Deze gegevenssets samen met het eerder geüploade audiogegevens worden gebruikt om te verfijnen en het model spraak-naar-tekst voor het herkennen van woorden, zinnen, acroniemen, namen en andere voorwaarden productspecifieke te trainen.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: erhopf
ms.openlocfilehash: 194ae477bb3cba4ac7e3350da6b793c6fea6ecdb
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/02/2019
ms.locfileid: "65025855"
---
# <a name="train-a-model-for-custom-speech"></a>Een model te trainen voor aangepaste spraak

Een spraak-naar-tekst training is nodig voor het verbeteren van nauwkeurigheid voor zowel van Microsoft baseline model of een aangepast model dat u van plan bent om te maken. Een model wordt getraind met behulp van transcriptie human-met het label en bijbehorende tekst. Deze gegevenssets samen met het eerder geüploade audiogegevens worden gebruikt om te verfijnen en het model spraak-naar-tekst voor het herkennen van woorden, zinnen, acroniemen, namen en andere voorwaarden productspecifieke te trainen. De meer domein gegevenssets die u (gegevens die is gerelateerd aan wat gebruikers zeggen en wat u verwacht opgeeft te herkennen) meer nauwkeurige uw model zijn zal, wat tot verbeterde herkenning leidt. Houd er rekening mee dat door deze uit niet-gegevens in uw training gerelateerde, kunt u beperken of de nauwkeurigheid van het model samenvoegen.

## <a name="use-training-to-resolve-accuracy-issues"></a>Training gebruiken nauwkeurigheid problemen oplossen

Als problemen met herkenning van het model ondervindt, met behulp van menselijke etiket worden uitgeschreven en bijbehorende gegevens voor extra training kan helpen om de nauwkeurigheid te. Gebruik deze tabel om te bepalen welke gegevensset gebruiken om de oorzaak van deze:

| Toepassing | Gegevenstype | De hoeveelheid gegevens |
|----------|-----------|---------------|
| Namen van de juiste zijn misrecognized | Relateren tekst (zinnen/uitingen) | 10 MB tot 500 MB |
| Woorden zijn misrecognized vanwege een accent | Tekst (uitspraak) | Geef de misrecognized woorden |
| Veelvoorkomende woorden zijn verwijderd of misrecognized | Audio + human etiket transcripties | 10-1000 transcriptie uur |

> [!IMPORTANT]
> Als u een set gegevens nog niet hebt geüpload, raadpleegt u [voorbereiden en testen van uw gegevens](how-to-custom-speech-test-data.md). Dit document vindt u instructies voor het uploaden van gegevens en richtlijnen voor het maken van hoge kwaliteit gegevenssets.

## <a name="train-and-evaluate-a-model"></a>Trainen en evalueren van een model

De eerste stap bij het trainen van een model is het uploaden van trainingsgegevens. Gebruik [voorbereiden en testen van uw gegevens](how-to-custom-speech-test-data.md) voor stapsgewijze instructies voor het voorbereiden van transcripties human-met het label en bijbehorende tekst (uitingen en uitspraak). Nadat u hebt geüpload trainingsgegevens, volgt u deze instructies om te beginnen met het trainen van uw model:

1. Navigeer naar **spraak-naar-tekst > aangepaste spraak > Training**.
2. Klik op **Train model**.
3. Vervolgens geven uw training een **naam** en **beschrijving**.
4. Uit de **Scenario en de basislijn model** vervolgkeuzelijst, selecteert u het scenario dat het beste past bij uw domein. Als u niet zeker weet welke scenario om te kiezen, selecteert u **algemene**. Het model van de basislijn is het beginpunt voor training. Als u een voorkeur hebt, kunt u de nieuwste versie.
5. Uit de **Selecteer trainingsgegevens** pagina, kies een of meerdere audio + human etiket transcriptie gegevenssets die u wilt gebruiken voor training.
6. Zodra de training voltooid is, kunt u de nauwkeurigheid van de gegevens op het zojuist getrainde model testen uitvoeren. Deze stap is optioneel.
7. Selecteer **maken** om uw aangepaste model te bouwen.

Een nieuwe vermelding die overeenkomt met bevat de Training-tabel naar de zojuist gemaakte model. De tabel wordt de status ook weergegeven:  Verwerking is voltooid, is mislukt.

## <a name="evaluate-the-accuracy-of-a-trained-model"></a>De nauwkeurigheid van een getraind model evalueren

U kunt inspecteren van de gegevens en de nauwkeurigheid van model met behulp van deze documenten evalueren:

* [Controleer uw gegevens](how-to-custom-speech-inspect-data.md)
* [Evalueren van uw gegevens](how-to-custom-speech-evaluate-data.md)


Als u ervoor kiest om juistheid, is het belangrijk dat u selecteert een akoestisch gegevensset die anders is dan het account waarmee u uw model met een realistische beeld van de prestaties van het model.

## <a name="next-steps"></a>Volgende stappen

* [Uw model implementeren](how-to-custom-speech-deploy-model.md)

## <a name="additional-resources"></a>Aanvullende resources

* [Voorbereiden en testen van uw gegevens](how-to-custom-speech-test-data.md)
* [Controleer uw gegevens](how-to-custom-speech-inspect-data.md)
* [Evalueren van uw gegevens](how-to-custom-speech-evaluate-data.md)
* [Uw model te trainen](how-to-custom-speech-train-model.md)
