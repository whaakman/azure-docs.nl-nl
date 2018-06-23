---
title: Voorbeeld voor vertaling | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: Hier volgt een voorbeeld voor de vertaling van spraak.
services: cognitive-services
author: wolfma61
manager: onano
ms.service: cognitive-services
ms.technology: Speech
ms.topic: article
ms.date: 05/07/2018
ms.author: wolfma
ms.openlocfilehash: a02a89de1d05ec5388a92f14a0fd206e0ee966a6
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345820"
---
# <a name="sample-for-translation"></a>Voorbeeld voor vertaling

> [!NOTE]
> Zie voor instructies voor het downloaden van dit voorbeeld en andere [voorbeelden voor spraak-SDK](samples.md).

[!include[Get a Subscription Key](includes/get-subscription-key.md)]

> [!NOTE]
> Voor alle voorbeelden hieronder moeten de volgende op het hoogste niveau declaraties zijn voldaan:
>
> [!code-csharp [Using Statements](~/samples-cognitive-services-speech-sdk/Windows/csharp_samples/translation_samples.cs#toplevel)]
>
> - - -

## <a name="translation-using-the-microphone"></a>De vertaling van de microfoon gebruiken

Het onderstaande codefragment laat zien hoe Vertaal spraakinvoer van Engels, Duits, en ook de voice-uitvoer van de vertaalde tekst ophalen. De microfoon wordt gebruikt.

[!code-csharp[Translation Using Microphone](~/samples-cognitive-services-speech-sdk/Windows/csharp_samples/translation_samples.cs#TranslationWithMicrophoneAsync)]

- - -

## <a name="translation-using-file-input"></a>De vertaling van bestandsinvoer met

Het onderstaande codefragment laat zien hoe Vertaal spraakinvoer van Engels, Duits en Frans.
Bestand wordt gebruikt als invoer.

[!code-csharp[Translation Using File Input](~/samples-cognitive-services-speech-sdk/Windows/csharp_samples/translation_samples.cs#TranslationWithFileAsync)]

- - -

## <a name="sample-source-code"></a>Voorbeeldcode

De nieuwste versie van de voorbeelden en zelfs meer geavanceerde voorbeelden zijn in een speciaal [GitHub-opslagplaats](https://github.com/Azure-Samples/cognitive-services-speech-sdk).

## <a name="next-steps"></a>Volgende stappen

- [Spraakherkenning](./speech-to-text-sample.md)

- [Opzet erkenning](./intent.md)
