---
title: 'Quickstart: Bewaak spraak, C++ (Windows)-Speech Service'
titleSuffix: Azure Cognitive Services
description: Meer informatie over het in-en C++ uitspreken van spraak in het Windows-bureau blad met behulp van de Speech SDK
services: cognitive-services
author: yinhew
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 07/05/2019
ms.author: yinhew
ms.openlocfilehash: 6936d8f93221402fca9b250d09a842ce753e0e12
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "68559313"
---
# <a name="quickstart-synthesize-speech-in-c-on-windows-by-using-the-speech-sdk"></a>Quickstart: Spraak maken in C++ Windows met behulp van de Speech SDK

Quick starts zijn ook beschikbaar voor [spraak herkenning](quickstart-cpp-windows.md) en [spraak omzetting](quickstart-translate-speech-cpp-windows.md).

In dit artikel maakt u een C++-consoletoepassing voor Windows. U gebruikt de Cognitive Services [Speech SDK](speech-sdk.md) om spraak van tekst in realtime te synthesizeren en de spraak op de spreker van uw PC af te spelen. De toepassing is gemaakt met het [Speech SDK NuGet-pakket](https://aka.ms/csspeech/nuget) en Microsoft Visual Studio 2017 (willekeurige editie).

De functie die in dit artikel wordt beschreven, is beschikbaar via [Speech SDK 1.5.0](https://www.nuget.org/packages/Microsoft.CognitiveServices.Speech/1.5.0).

Zie [taal ondersteuning](language-support.md#text-to-speech)voor een volledige lijst met talen/stemmen die beschikbaar zijn voor spraak synthese.

## <a name="prerequisites"></a>Vereisten

U hebt een abonnement op spraak services nodig om deze Quick Start te volt ooien. U kunt er gratis een krijgen. Zie [de spraak services gratis uitproberen](get-started.md) voor meer informatie.

## <a name="create-a-visual-studio-project"></a>Een Visual Studio-project maken

[!INCLUDE [Quickstart C++ project](../../../includes/cognitive-services-speech-service-quickstart-cpp-create-proj.md)]

## <a name="add-sample-code"></a>Voorbeeldcode toevoegen

1. Open het bronbestand *helloworld.cpp*. Vervang alle code onder de eerste include-instructie (`#include "stdafx.h"` of `#include "pch.h"`) door het volgende:

   [!code-cpp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/text-to-speech/cpp-windows/helloworld/helloworld.cpp#code)]

1. Vervang in hetzelfde bestand de tekenreeks `YourSubscriptionKey` door uw abonnementssleutel.

1. Vervang de tekenreeks `YourServiceRegion` door de [regio](regions.md) die gekoppeld is aan uw abonnement (bijvoorbeeld `westus` voor het gratis proefabonnement).

1. Sla de wijzigingen in het project op.

## <a name="build-and-run-the-app"></a>De app bouwen en uitvoeren

1. Compileer de toepassing. Kies in de menubalk **Build** > **Build Solution**. De code moet zonder fouten worden gecompileerd.

   ![Schermafbeelding van Visual Studio-toepassing met de optie Build Solution gemarkeerd](media/sdk/qs-cpp-windows-06-build.png)

1. Start de toepassing. Kies in de menubalk **Debug** > **Start Debugging** of druk op **F5**.

   ![Schermafbeelding van Visual Studio-toepassing met de optie Start Debugging gemarkeerd](media/sdk/qs-cpp-windows-07-start-debugging.png)

1. Er wordt een console venster weer gegeven waarin u wordt gevraagd om tekst te typen. Typ enkele woorden of een zin. De tekst die u hebt getypt, wordt verzonden naar de spraak Services en gesynthesizerd op spraak, die op uw spreker wordt afgespeeld.

   ![Scherm opname van de console-uitvoer na een geslaagde synthese](media/sdk/qs-tts-cpp-windows-console-output.png)

## <a name="next-steps"></a>Volgende stappen

Aanvullende voor beelden, zoals het opslaan van spraak naar een audio bestand, zijn beschikbaar op GitHub.

> [!div class="nextstepaction"]
> [C++-voorbeelden op GitHub bekijken](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Zie ook

- [Spraak lettertypen aanpassen](how-to-customize-voice-font.md)
- [Spraak voorbeelden vastleggen](record-custom-voice-samples.md)
