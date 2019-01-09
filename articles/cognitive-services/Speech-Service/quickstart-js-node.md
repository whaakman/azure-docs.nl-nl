---
title: 'Quickstart: Gesproken tekst in JavaScript herkennen in een browser met behulp van de Speech Service-SDK'
titleSuffix: Azure Cognitive Services
description: Meer informatie over het herkennen van spraak in JavaScript in Node.js met behulp van de Speech Service-SDK
services: cognitive-services
author: fmegen
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: quickstart
ms.date: 12/18/2018
ms.author: fmegen
ms.openlocfilehash: 35652b169067bc545fa0d1fcc977bbaee79ec3aa
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2018
ms.locfileid: "53724422"
---
# <a name="quickstart-recognize-speech-in-javascript-in-nodejs-using-the-speech-service-sdk"></a>Quickstart: Gesproken tekst in JavaScript herkennen in een browser met behulp van de Speech Service-SDK

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

In dit artikel leert u hoe u een Node.js-project maakt met behulp van de JavaScript-binding van de Cognitive Services Speech-SDK om spraak-naar-tekst te transcriberen.
De toepassing is gebaseerd op de Microsoft [Cognitive Services Speech-SDK](https://aka.ms/csspeech/npmpackage).

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnementssleutel voor de Spraakservice. [Gratis downloaden](get-started.md).
* Een huidige versie van [Node.js](https://nodejs.org).

## <a name="create-a-new-project-folder"></a>Een nieuwe projectmap maken

Maak een nieuwe, lege map en initialiseer deze als een nieuw JavaScript- en Node.js-project.

```sh
npm init -f
```

Hiermee worden de package.json-bestanden geïnitialiseerd met standaardwaarden. Waarschijnlijk wilt u dit bestand later bewerken.

## <a name="install-the-speech-sdk-for-javascript-into-that-folder"></a>Installeer de Speech Service-SDK voor JavaScript in deze map

Voeg de Speech Service-SDK via `npm install microsoft-cognitiveservices-speech-sdk` toe aan uw Node.js-project.

Hiermee worden de meest recente versie van de Speech Service-SDK en alle vereiste onderdelen van npmjs gedownload en geïnstalleerd. De SDK wordt geïnstalleerd in de map `node_modules` van de projectmap.

## <a name="using-the-speech-sdk"></a>De Speech Service-SDK gebruiken

Maak een nieuw bestand in de map met de naam `index.js` en open dit bestand met een teksteditor.

> [!NOTE]
> In Node.js biedt de Speech Service-SDK geen ondersteuning voor de microfoon of het gegevenstype Bestand. Beide worden alleen ondersteund in browsers. Gebruik in plaats daarvan de Stream-interface met de Speech Service-SDK, via `AudioInputStream.createPushStream()` of via `AudioInputStream.createPullStream()`.

In dit voorbeeld gebruiken we de `PushAudioInputStream`-interface.

Voeg de volgende JavaScript-code toe:

[!code-javascript[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/js-node/index.js#code)]

## <a name="running-the-sample-from-command-line"></a>Het voorbeeld uitvoeren vanaf de opdrachtregel

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

## <a name="running-the-sample-from-visual-studio-code"></a>Het voorbeeld uitvoeren vanuit Visual Studio Code

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
