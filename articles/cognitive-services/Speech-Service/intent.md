---
title: Voorbeeld voor opzet erkenning | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: Hier volgt een voorbeeld voor opzet herkenning.
services: cognitive-services
author: wolfma61
manager: onano
ms.service: cognitive-services
ms.technology: Speech
ms.topic: article
ms.date: 06/07/2018
ms.author: wolfma
ms.openlocfilehash: 1c9c1e2d54ccb200ef009be3566f6da9ced01175
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37111164"
---
# <a name="sample-for-intent-recognition"></a>Voorbeeld voor opzet opname

> [!NOTE]
> Zie voor instructies voor het downloaden van dit voorbeeld en andere [voorbeelden voor spraak-SDK](samples.md).

> [!NOTE]
> Neem eerst een abonnementssleutel ophalen. In tegenstelling tot andere services wordt ondersteund door de SDK cognitieve spraak, vereist de bedoeling herkenning een specifiek abonnement-sleutel. [Hier](https://www.luis.ai) vindt u meer informatie over de opzet opname-technologie, evenals informatie over het verkrijgen van een abonnementssleutel. De abonnementssleutel van uw eigen, vervangen de [regio van uw abonnement](regions.md), en de AppId van uw opzet model op de juiste plaatsen in de voorbeelden.

> [!NOTE]
> Voor alle voorbeelden hieronder moeten de volgende op het hoogste niveau declaraties zijn voldaan:
>
> [!code-cpp[](~/samples-cognitive-services-speech-sdk/Windows/cxx_samples/intent_recognition_samples.cpp#toplevel)]
>
> - - -

## <a name="intent-recognition-using-microphone"></a>Opzet erkenning met behulp van de microfoon

Het onderstaande codefragment toont het herkennen van de intentie van microfoon invoer in de standaardtaal (`en-US`).

[!code-cpp[Intent Recognition Using Microphone](~/samples-cognitive-services-speech-sdk/Windows/cxx_samples/intent_recognition_samples.cpp#IntentRecognitionWithMicrophone)]

- - -

## <a name="intent-recognition-using-microphone-in-a-specified-language"></a>Opzet erkenning met microfoon in een bepaalde taal

Het onderstaande codefragment ziet u het doel van de microfoon invoer in een bepaalde taal in dit geval herkennen in het Duits (`de-de`).

[!code-cpp[Intent Recognition Using Microphone In A Specified Language](~/samples-cognitive-services-speech-sdk/Windows/cxx_samples/intent_recognition_samples.cpp#IntentRecognitionWithLanguage)]

- - -

## <a name="intent-recognition-from-a-file"></a>Opzet herkenning van een bestand

Het volgende codefragment herkent doel van een audio-bestand in de standaardtaal (`en-US`), de ondersteunde indeling is één-kanaal (mono) WAV / PCM met een samplefrequentie van 16 KHz.

[!include[Sample Audio](includes/sample-audio.md)]

[!code-cpp[Intent Recognition From a File](~/samples-cognitive-services-speech-sdk/Windows/cxx_samples/intent_recognition_samples.cpp#IntentRecognitionWithFile)]

- - -

## <a name="intent-recognition-using-events"></a>Opzet voor het gebruik van gebeurtenissen

Het codefragment laat zien hoe doel in een continue manier kan herkennen. Deze code geeft toegang tot aanvullende informatie, zoals tussenliggende resultaten. 

[!code-cpp[Intent Recognition Using Events](~/samples-cognitive-services-speech-sdk/Windows/cxx_samples/intent_recognition_samples.cpp#IntentContinuousRecognitionUsingEvents)]

- - -

## <a name="sample-source-code"></a>Voorbeeldcode

Zie voor de meest recente set steekproeven van het [cognitieve Services spraak SDK voorbeeld GitHub-opslagplaats](https://aka.ms/csspeech/samples).

## <a name="next-steps"></a>Volgende stappen

- [Spraakherkenning](./speech-to-text-sample.md)

- [Omzetting](./translation.md)
