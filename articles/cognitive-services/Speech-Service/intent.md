---
title: Voorbeeld voor Intentieherkenning | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: Hier volgt een voorbeeld voor intentieherkenning.
services: cognitive-services
author: wolfma61
manager: onano
ms.service: cognitive-services
ms.technology: Speech
ms.topic: article
ms.date: 07/16/2018
ms.author: wolfma
ms.openlocfilehash: 3e9afc990d6bfa73eb045e7ed76dfd194df309c6
ms.sourcegitcommit: 248c2a76b0ab8c3b883326422e33c61bd2735c6c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/23/2018
ms.locfileid: "39213195"
---
# <a name="sample-for-intent-recognition"></a>Voorbeeld voor intentieherkenning

Voer eerst een abonnementssleutel verkrijgen. In tegenstelling tot andere services wordt ondersteund door de SDK Cognitive Speech, vereist de herkenning van intentie de sleutel van een bepaald abonnement. [Hier](https://www.luis.ai) vindt u meer informatie over de intentieherkenning-technologie, evenals informatie over het verkrijgen van een abonnementssleutel. Vervang de sleutel van uw eigen Language Understanding-abonnement, de [regio van uw abonnement](regions.md), en de AppId van de intentie model op de juiste plaatsen in de voorbeelden.

## <a name="top-level-declarations"></a>Op het hoogste niveau declaraties

Voor alle voorbeelden hieronder moeten het volgende op het hoogste niveau declaraties zijn voldaan:

[!code-csharp[Top-level declarations](~/samples-cognitive-services-speech-sdk/samples/csharp/sharedcontent/console/intent_recognition_samples.cs#toplevel)]

[!code-cpp[Top-level declarations](~/samples-cognitive-services-speech-sdk/samples/cpp/windows/console/samples/intent_recognition_samples.cpp#toplevel)]

[!code-java[Top-level declarations](~/samples-cognitive-services-speech-sdk/samples/java/jre/console/src/com/microsoft/cognitiveservices/speech/samples/console/IntentRecognitionSamples.java#toplevel)]

## <a name="intent-recognition-using-microphone"></a>Intentieherkenning met behulp van de microfoon

Het onderstaande codefragment laat zien hoe u voor het herkennen van de intentie van de invoer van de microfoon in de standaardtaal (`en-US`).

[!code-csharp[Intent Recognition Using Microphone](~/samples-cognitive-services-speech-sdk/samples/csharp/sharedcontent/console/intent_recognition_samples.cs#intentRecognitionWithMicrophone)]

[!code-cpp[Intent Recognition Using Microphone](~/samples-cognitive-services-speech-sdk/samples/cpp/windows/console/samples/intent_recognition_samples.cpp#IntentRecognitionWithMicrophone)]

[!code-java[Intent Recognition Using Microphone](~/samples-cognitive-services-speech-sdk/samples/java/jre/console/src/com/microsoft/cognitiveservices/speech/samples/console/IntentRecognitionSamples.java#IntentRecognitionWithMicrophone)]

## <a name="intent-recognition-using-microphone-in-a-specified-language"></a>Met behulp van de microfoon in een opgegeven taal intentieherkenning

Het onderstaande codefragment laat zien hoe u voor het herkennen van de intentie van de invoer van de microfoon in een opgegeven taal, in dit geval in het Duits (`de-de`).

[!code-csharp[Intent Recognition Using Microphone In A Specified Language](~/samples-cognitive-services-speech-sdk/samples/csharp/sharedcontent/console/intent_recognition_samples.cs#intentRecognitionWithLanguage)]

[!code-cpp[Intent Recognition Using Microphone In A Specified Language](~/samples-cognitive-services-speech-sdk/samples/cpp/windows/console/samples/intent_recognition_samples.cpp#IntentRecognitionWithLanguage)]

[!code-java[Intent Recognition Using Microphone In A Specified Language](~/samples-cognitive-services-speech-sdk/samples/java/jre/console/src/com/microsoft/cognitiveservices/speech/samples/console/IntentRecognitionSamples.java#IntentRecognitionWithLanguage)]

## <a name="intent-recognition-from-a-file-using-events"></a>Intentieherkenning uit een bestand, gebruik van gebeurtenissen

Het codefragment laat zien hoe u voor het herkennen van intentie in de standaardtaal (`en-US`) in een continue manier. Deze code geeft toegang tot aanvullende informatie, zoals tussenliggende resultaten genereren. Invoer is afkomstig uit een geluidsbestand, de ondersteunde indeling is één-kanaal (mono) WAV / PCM met een samplefrequentie van 16 KHz.

[!code-csharp[Intent Recognition Using Events From a File](~/samples-cognitive-services-speech-sdk/samples/csharp/sharedcontent/console/intent_recognition_samples.cs#intentContinuousRecognitionWithFile)]

[!code-cpp[Intent Recognition Using Events From a File](~/samples-cognitive-services-speech-sdk/samples/cpp/windows/console/samples/intent_recognition_samples.cpp#IntentContinuousRecognitionWithFile)]

[!code-java[Intent Recognition Using Events From a File](~/samples-cognitive-services-speech-sdk/samples/java/jre/console/src/com/microsoft/cognitiveservices/speech/samples/console/IntentRecognitionSamples.java#IntentContinuousRecognitionWithFile)]

[!include[Download the sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]

## <a name="next-steps"></a>Volgende stappen

- [Spraakherkenning](./speech-to-text-sample.md)

- [Omzetting](./translation.md)
