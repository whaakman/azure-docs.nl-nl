---
title: 'Snelstart: Spraak herkennen, Node.js - Spraakservices'
titleSuffix: Azure Cognitive Services
description: Gebruik deze handleiding om een consoletoepassing voor spraak-naar-tekst te maken met de Speech-SDK voor Node.js. Als u klaar bent, kunt u de microfoon van de computer gebruiken om in realtime spraak naar tekst te transcriberen.
services: cognitive-services
author: fmegen
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: quickstart
ms.date: 1/16/2019
ms.author: fmegen
ms.openlocfilehash: e0ae916687ca32835dd8daf6e5059b8f6eea0ff6
ms.sourcegitcommit: ba9f95cf821c5af8e24425fd8ce6985b998c2982
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/17/2019
ms.locfileid: "54382171"
---
# <a name="quickstart-recognize-speech-with-the-speech-sdk-for-nodejs"></a>Quickstart: Spraak herkennen met de Speech-SDK voor Node.js

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

In dit artikel leert u hoe u een Node.js-project maakt met behulp van de JavaScript-binding van de Cognitive Services Speech-SDK om spraak-naar-tekst te transcriberen.
De toepassing is gebaseerd op de Microsoft [Cognitive Services Speech-SDK](https://aka.ms/csspeech/npmpackage).

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnementssleutel voor de Spraakservice. [Gratis downloaden](get-started.md).
* Een huidige versie van [Node.js](https://nodejs.org).

## <a name="create-a-new-project"></a>Een nieuw project maken

Maak een nieuwe map en initialiseer het project.

```sh
npm init -f
```

Hiermee worden de package.json-bestanden geïnitialiseerd met standaardwaarden. Waarschijnlijk wilt u dit bestand later bewerken.

## <a name="install-the-speech-sdk"></a>De Speech-SDK installeren

Voeg de Speech-SDK toe aan uw Node.js-project.

```
npm install microsoft-cognitiveservices-speech-sdk
```

Hiermee worden de meest recente versie van de Speech-SDK en alle vereiste onderdelen van npmjs gedownload en geïnstalleerd. De SDK wordt geïnstalleerd in de map `node_modules` van de projectmap.

## <a name="use-the-speech-sdk"></a>De Speech-SDK gebruiken

Maak een nieuw bestand in de map met de naam `index.js` en open dit bestand met een teksteditor.

> [!NOTE]
> In Node.js biedt de Speech Service-SDK geen ondersteuning voor de microfoon of het gegevenstype Bestand. Beide worden alleen ondersteund in browsers. Gebruik in plaats daarvan de Stream-interface met de Speech Service-SDK, via `AudioInputStream.createPushStream()` of via `AudioInputStream.createPullStream()`.

In dit voorbeeld gebruiken we de `PushAudioInputStream`-interface.

Voeg de volgende JavaScript-code toe:

[!code-javascript[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/js-node/index.js#code)]

## <a name="run-the-sample"></a>De voorbeeldtoepassing uitvoeren

Als u de app wilt starten, past u `YourSubscriptionKey`, `YourServiceRegion` en `YourAudioFile.wav` aan uw configuratie aan. Daarna kunt u de app uitvoeren door de volgende opdracht aan te roepen:

```sh
node index.js
```

Hiermee wordt een herkenning geactiveerd met behulp van de opgegeven bestandsnaam en wordt de uitvoer op de console weergegeven.

Hier volgt een voorbeeld van de uitvoering van `index.js` na het bijwerken van de abonnementssleutel en het gebruik van het bestand `whatstheweatherlike.wav`.

```json
SpeechRecognitionResult {
  "privResultId": "9E30EEBD41AC4571BB77CF9164441F46",
  "privReason": 3,
  "privText": "What's the weather like?",
  "privDuration": 15900000,
  "privOffset": 300000,
  "privErrorDetails": null,
  "privJson": {
    "RecognitionStatus": "Success",
    "DisplayText": "What's the weather like?",
    "Offset": 300000,
    "Duration": 15900000
  },
  "privProperties": null
}
```

## <a name="install-and-use-the-speech-sdk-with-visual-studio-code"></a>De Speech-SDK installeren en gebruiken met Visual Studio Code

U kunt het voorbeeld ook uitvoeren vanuit Visual Studio Code. Volg deze stappen om de quickstart te installeren, te openen en uit te voeren:

1. Start Visual Studio Code, klik op Map openen en ga naar de map met de quickstart

   ![Schermafbeelding van Map openen](media/sdk/qs-js-node-01-open_project.png)

1. Open een terminalvenster in Visual Studio Code

   ![Schermafbeelding van het terminalvenster](media/sdk/qs-js-node-02_open_terminal.png)

1. Voer npm uit om de afhankelijkheden te installeren

   ![Schermafbeelding van npm-installatie](media/sdk/qs-js-node-03-npm_install.png)

1. Nu bent u klaar om `index.js` te openen en een onderbrekingspunt in te stellen

   ![Schermafbeelding van index.js met een onderbrekingspunt op regel 16](media/sdk/qs-js-node-04-setup_breakpoint.png)

1. Als u de foutopsporing wilt starten, drukt u op F5 of selecteert u Fouten opsporen/Foutopsporing starten in het menu

   ![Schermafbeelding van het foutopsporingsmenu](media/sdk/qs-js-node-05-start_debugging.png)

1. Wanneer een onderbrekingspunt is bereikt, kunt u de aanroepstack en variabelen inspecteren

   ![Schermafbeelding van foutopsporingsprogramma](media/sdk/qs-js-node-06-hit_breakpoint.png)

1. Alle uitvoer wordt weergegeven in het consolevenster voor foutopsporing

   ![Schermafbeelding van console voor foutopsporing](media/sdk/qs-js-node-07-debug_output.png)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Node.js-voorbeelden op GitHub verkennen](https://aka.ms/csspeech/samples)
