---
title: 'Quickstart: Vertaal spraak, C++ (Windows)-Speech Service'
titleSuffix: Azure Cognitive Services
description: In deze quickstart maakt u een eenvoudige C++-toepassing om spraak van gebruikers vast te leggen, deze om te zetten in een andere taal en de tekst uit te voeren naar de opdrachtregel. Deze handleiding is bedoeld voor Windows-gebruikers.
services: cognitive-services
author: wolfma61
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 07/05/2019
ms.author: erhopf
ms.openlocfilehash: 379087ca94eee6ce3d45bfd97b4771c5f08d6333
ms.sourcegitcommit: 6cff17b02b65388ac90ef3757bf04c6d8ed3db03
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/29/2019
ms.locfileid: "68607697"
---
# <a name="quickstart-translate-speech-with-the-speech-sdk-for-c"></a>Quickstart: Spraak omzetten met de Speech SDK voor C++

Quick starts zijn ook beschikbaar voor [spraak herkenning](quickstart-cpp-windows.md) en [tekst naar spraak](quickstart-text-to-speech-cpp-windows.md).

In deze quickstart maakt u een eenvoudige C++-toepassing waarmee spraak van gebruikers via de microfoon van de computer wordt vastgelegd, waarna de spraak wordt omgezet en de omgezette tekst in realtime wordt getranscribeerd naar de opdrachtregel. Deze toepassing is ontworpen om te worden uitgevoerd op 64-bits Windows en is gebouwd met het [Speech SDK NuGet-pakket](https://aka.ms/csspeech/nuget) en micro soft Visual Studio 2017 of hoger.

Zie [Taalondersteuning](language-support.md) voor een volledige lijst met talen die voor spraakomzetting beschikbaar zijn.

## <a name="prerequisites"></a>Vereisten

Voor deze snelstart zijn de volgende zaken vereist:

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) of hoger
* Een Azure-abonnementssleutel voor de Spraakservice. [Gratis downloaden](get-started.md).

## <a name="create-a-visual-studio-project"></a>Een Visual Studio-project maken

[!INCLUDE [Quickstart C++ project](../../../includes/cognitive-services-speech-service-quickstart-cpp-create-proj.md)]

## <a name="add-sample-code"></a>Voorbeeldcode toevoegen

1. Open het bronbestand *helloworld.cpp*. Vervang alle code onder de eerste include-instructie (`#include "stdafx.h"` of `#include "pch.h"`) door het volgende:

    [!code-cpp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/speech-translation/cpp-windows/helloworld/helloworld.cpp#code)]

1. Vervang in hetzelfde bestand de tekenreeks `YourSubscriptionKey` door uw abonnementssleutel.

1. Vervang de tekenreeks `YourServiceRegion` door de [regio](regions.md) die gekoppeld is aan uw abonnement (bijvoorbeeld `westus` voor het gratis proefabonnement).

1. Sla de wijzigingen in het project op.

## <a name="build-and-run-the-app"></a>De app bouwen en uitvoeren

1. Compileer de toepassing. Kies in de menubalk **Build** > **Build Solution**. De code moet zonder fouten worden gecompileerd.

   ![Schermafbeelding van Visual Studio-toepassing met de optie Build Solution gemarkeerd](media/sdk/qs-cpp-windows-06-build.png)

1. Start de toepassing. Kies in de menubalk **Debug** > **Start Debugging** of druk op **F5**.

   ![Schermafbeelding van Visual Studio-toepassing met de optie Start Debugging gemarkeerd](media/sdk/qs-cpp-windows-07-start-debugging.png)

1. Er wordt een consolevenster weergegeven waarin u wordt gevraagd iets te zeggen. Spreek een Engelse woordgroep of zin in. De gesproken tekst wordt verzonden naar de spraakservice, omgezet en getranscribeerd naar tekst, die in hetzelfde venster wordt weergegeven.

   ![Schermafbeelding van console-uitvoer na geslaagde omzetting](media/sdk/qs-translate-cpp-windows-output.png)

## <a name="next-steps"></a>Volgende stappen

Op GitHub vindt u aanvullende voorbeelden, zoals hoe u spraak kunt lezen vanuit een audiobestand en omgezette tekst als kunstmatige spraak kunt uitvoeren.

> [!div class="nextstepaction"]
> [C++-voorbeelden op GitHub bekijken](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Zie ook

- [Akoestische modellen aanpassen](how-to-customize-acoustic-models.md)
- [Taalmodellen aanpassen](how-to-customize-language-model.md)
