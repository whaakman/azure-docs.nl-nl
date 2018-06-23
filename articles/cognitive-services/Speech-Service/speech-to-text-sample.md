---
title: Voorbeeld voor spraak-naar-tekst | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: Hier volgt een voorbeeld voor spraak-naar-tekst.
services: cognitive-services
author: wolfma61
manager: onano
ms.service: cognitive-services
ms.technology: Speech
ms.topic: article
ms.date: 05/07/2018
ms.author: wolfma
ms.openlocfilehash: 9e0b82cc9a034f1e25ef0cc656e3770a166a6ad7
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345834"
---
# <a name="sample-for-speech-to-text"></a>Voorbeeld voor spraak-naar-tekst

> [!NOTE]
> Zie voor instructies voor het downloaden van dit voorbeeld en andere [voorbeelden voor spraak-SDK](samples.md).

[!include[Get a Subscription Key](includes/get-subscription-key.md)]

> [!NOTE]
> Voor alle voorbeelden hieronder, moeten de volgende op het hoogste niveau declaraties zijn voldaan:
>
> [!code-csharp[](~/samples-cognitive-services-speech-sdk/Windows/csharp_samples/speech_recognition_samples.cs#toplevel)]
>
> [!code-cpp[](~/samples-cognitive-services-speech-sdk/Windows/cxx_samples/speech_recognition_samples.cpp#toplevel)]
>
> - - -

## <a name="speech-recognition-using-the-microphone"></a>Spraakherkenning met behulp van de microfoon

Het onderstaande codefragment ziet u hoe herkennen spraakinvoer van de microfoon in de standaardtaal (`en-US`).

[!code-csharp[Speech Recognition Using Microphone](~/samples-cognitive-services-speech-sdk/Windows/csharp_samples/speech_recognition_samples.cs#recognitionWithMicrophone)]

[!code-cpp[Speech Recognition Using Microphone](~/samples-cognitive-services-speech-sdk/Windows/cxx_samples/speech_recognition_samples.cpp#SpeechRecognitionWithMicrophone)]

- - -

## <a name="speech-recognition-from-a-file"></a>Spraakherkenning uit een bestand

Het volgende codefragment herkent spraakinvoer uit een audio-bestand in de standaardtaal (`en-US`), de ondersteunde indeling is één-kanaal (mono) WAV / PCM met een samplefrequentie van 16 KHz.

[!include[Sample Audio](includes/sample-audio.md)]

[!code-csharp[Speech Recognition From a File](~/samples-cognitive-services-speech-sdk/Windows/csharp_samples/speech_recognition_samples.cs?name=recognitionFromFile)]

[!code-cpp[Speech Recognition From a File](~/samples-cognitive-services-speech-sdk/Windows/cxx_samples/speech_recognition_samples.cpp?name=SpeechRecognitionWithFile)]

- - -

## <a name="speech-recognition-using-a-customized-model"></a>Spraakherkenning gebruikmaken van een aangepaste model

De [aangepaste spraak Service (cri's)](https://www.cris.ai/) kunt u de aanpassing van de Microsoft spraak-naar-tekst-engine voor uw toepassing. Het onderstaande codefragment toont het herkennen van spraak van een microfoon met behulp van uw model cri's; Vul uw abonnementssleutel cri's en uw eigen implementatie-id voordat het wordt uitgevoerd.

[!code-csharp[Speech Recognition Using a Customized Model](~/samples-cognitive-services-speech-sdk/Windows/csharp_samples/speech_recognition_samples.cs#recognitionCustomized)]

[!code-cpp[Speech Recognition Using a Customized Model](~/samples-cognitive-services-speech-sdk/Windows/cxx_samples/speech_recognition_samples.cpp#SpeechRecognitionUsingCustomizedModel)]

- - -

## <a name="continuous-speech-recognition"></a>Continue spraakherkenning

[!code-csharp[Continuous Speech Recognition](~/samples-cognitive-services-speech-sdk/Windows/csharp_samples/speech_recognition_samples.cs#recognitionContinuous)]

[!code-cpp[Continuous Speech Recognition](~/samples-cognitive-services-speech-sdk/Windows/cxx_samples/speech_recognition_samples.cpp#SpeechContinuousRecognitionUsingEvents)]

- - -

## <a name="sample-source-code"></a>Voorbeeldcode

De nieuwste versie van de voorbeelden en zelfs meer geavanceerde voorbeelden zijn in een speciaal [GitHub-opslagplaats](https://github.com/Azure-Samples/cognitive-services-speech-sdk).

## <a name="next-steps"></a>Volgende stappen

- [Opzet erkenning](./intent.md)

- [De vertaling](./translation.md)
