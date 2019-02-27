---
title: 'Snelstartgids: Spraak herkennen, C++ (Windows) - Speech Services'
titleSuffix: Azure Cognitive Services
description: Informatie over het herkennen van gesproken tekst in C++ op Windows Desktop met behulp van de Speech SDK
services: cognitive-services
author: wolfma61
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 12/13/2018
ms.author: wolfma
ms.openlocfilehash: d6504b97bbc6c1ba4bcd02da54a29501c164439f
ms.sourcegitcommit: 6cab3c44aaccbcc86ed5a2011761fa52aa5ee5fa
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/20/2019
ms.locfileid: "56447987"
---
# <a name="quickstart-recognize-speech-in-c-on-windows-by-using-the-speech-sdk"></a>Quickstart: Gesproken tekst herkennen in C++ onder Windows met behulp van de Speech-SDK

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

In dit artikel maakt u een C++-consoletoepassing voor Windows. U gebruikt de [Speech SDK](speech-sdk.md) van Cognitive Services om in realtime spraak te transcriberen naar tekst via de microfoon van uw pc. De toepassing is gemaakt met het [Speech SDK NuGet-pakket](https://aka.ms/csspeech/nuget) en Microsoft Visual Studio 2017 (willekeurige editie).

## <a name="prerequisites"></a>Vereisten

U hebt een abonnementssleutel voor de Speech-service nodig om deze snelstartgids te doorlopen. U kunt er gratis een krijgen. Zie [Speech-service gratis uitproberen](get-started.md) voor meer informatie.

## <a name="create-a-visual-studio-project"></a>Een Visual Studio-project maken

[!INCLUDE [](../../../includes/cognitive-services-speech-service-quickstart-cpp-create-proj.md)]

## <a name="add-sample-code"></a>Voorbeeldcode toevoegen

1. Open het bronbestand *helloworld.cpp*. Vervang alle code onder de eerste include-instructie (`#include "stdafx.h"` of `#include "pch.h"`) door het volgende:

   [!code-cpp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/cpp-windows/helloworld/helloworld.cpp#code)]

1. Vervang in hetzelfde bestand de tekenreeks `YourSubscriptionKey` door uw abonnementssleutel.

1. Vervang de tekenreeks `YourServiceRegion` door de [regio](regions.md) die gekoppeld is aan uw abonnement (bijvoorbeeld `westus` voor het gratis proefabonnement).

1. Sla de wijzigingen in het project op.

## <a name="build-and-run-the-app"></a>De app bouwen en uitvoeren

1. Compileer de toepassing. Kies in de menubalk **Build** > **Build Solution**. De code moet zonder fouten worden gecompileerd.

   ![Schermafbeelding van Visual Studio-toepassing met de optie Build Solution gemarkeerd](media/sdk/qs-cpp-windows-06-build.png)

1. Start de toepassing. Kies in de menubalk **Debug** > **Start Debugging** of druk op **F5**.

   ![Schermafbeelding van Visual Studio-toepassing met de optie Start Debugging gemarkeerd](media/sdk/qs-cpp-windows-07-start-debugging.png)

1. Er wordt een consolevenster weergegeven waarin u wordt gevraagd iets te zeggen. Spreek een Engelse woordgroep of zin in. De gesproken tekst wordt verzonden naar de Speech-service en getranscribeerd naar tekst, die in hetzelfde venster wordt weergegeven.

   ![Schermafbeelding van console-uitvoer na geslaagde herkenning](media/sdk/qs-cpp-windows-08-console-output-release.png)

## <a name="next-steps"></a>Volgende stappen

Op GitHub vindt u aanvullende voorbeelden, zoals hoe u spraak kunt lezen vanuit een audiobestand.

> [!div class="nextstepaction"]
> [C++-voorbeelden op GitHub bekijken](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Zie ook

- [Akoestische modellen aanpassen](how-to-customize-acoustic-models.md)
- [Taalmodellen aanpassen](how-to-customize-language-model.md)
