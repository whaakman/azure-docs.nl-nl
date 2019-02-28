---
title: bestand opnemen
description: bestand opnemen
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.subservice: luis
ms.topic: include
ms.custom: include file
ms.date: 08/16/2018
ms.author: diberry
ms.openlocfilehash: 67c95ffcdbdbcfbb9a86e15c91d984953d7bbffc
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/23/2019
ms.locfileid: "56741306"
---
Om te begrijpen wat een LUIS-voorspellingseindpunt retourneert, kunt u het resultaat van een voorspelling in een webbrowser bekijken. Als u een query voor een openbare app uitvoert, hebt u uw eigen sleutel en de app-ID nodig. De openbare IoT-app-ID, `df67dcdb-c37d-46af-88e1-8b97951ca1c2`, wordt geleverd als onderdeel van de URL in stap 1.

De indeling van de URL voor een **GET**-eindpuntaanvraag is:

```JSON
https://<region>.api.cognitive.microsoft.com/luis/v2.0/apps/<appID>?subscription-key=<YOUR-KEY>&q=<user-utterance>
```

1. Het eindpunt van de openbare IoT-app heeft deze indeling: `https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/df67dcdb-c37d-46af-88e1-8b97951ca1c2?subscription-key=<YOUR_KEY>&q=turn on the bedroom light`.

    Kopieer de URL en vervang uw sleutel door de waarde van `<YOUR_KEY>`.

2. Plak de URL in een browservenster en druk op Enter. In de browser wordt een JSON-resultaat weergegeven dat aangeeft dat LUIS de intent `HomeAutomation.TurnOn` als de belangrijkste intent en de entiteit `HomeAutomation.Room` met de waarde `bedroom` detecteert.

    ```JSON
    {
      "query": "turn on the bedroom light",
      "topScoringIntent": {
        "intent": "HomeAutomation.TurnOn",
        "score": 0.809439957
      },
      "entities": [
        {
          "entity": "bedroom",
          "type": "HomeAutomation.Room",
          "startIndex": 12,
          "endIndex": 18,
          "score": 0.8065475
        }
      ]
    }
    ```

3. Wijzig de waarde van de parameter `q=` in de URL in `turn off the living room light` en druk op Enter. Het resultaat geeft nu aan dat LUIS de intent `HomeAutomation.TurnOff` als de belangrijkste intent en de entiteit `HomeAutomation.Room` met waarde `living room` heeft gedetecteerd. 

    ```JSON
    {
      "query": "turn off the living room light",
      "topScoringIntent": {
        "intent": "HomeAutomation.TurnOff",
        "score": 0.984057844
      },
      "entities": [
        {
          "entity": "living room",
          "type": "HomeAutomation.Room",
          "startIndex": 13,
          "endIndex": 23,
          "score": 0.9619945
        }
      ]
    }
    ```
