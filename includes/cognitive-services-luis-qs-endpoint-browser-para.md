---
title: bestand opnemen
description: bestand opnemen
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: luis
ms.topic: include
ms.custom: include file
ms.date: 08/16/2018
ms.author: diberry
ms.openlocfilehash: dae56e05f01e83f05e75fdf378c0c50679d18728
ms.sourcegitcommit: 58c5cd866ade5aac4354ea1fe8705cee2b50ba9f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/24/2018
ms.locfileid: "42819887"
---
Om te begrijpen wat een LUIS-eindpunt voor voorspelling retourneert, het resultaat van een voorspelling te bekijken in een webbrowser. Als u wilt een openbare app een query uitvoert, moet u uw eigen sleutel en de app-ID. De openbare app-ID IoT `df67dcdb-c37d-46af-88e1-8b97951ca1c2`, wordt geleverd als onderdeel van de URL in stap 1.

De indeling van de URL voor een **ophalen** endpoint-aanvraag is:

```JSON
https://<region>.api.cognitive.microsoft.com/luis/v2.0/apps/<appID>?subscription-key=<YOUR-KEY>&q=<user-utterance>
```

1. Het eindpunt van de openbare IoT-app is volgens de volgende notatie: `https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/df67dcdb-c37d-46af-88e1-8b97951ca1c2?subscription-key=<YOUR_KEY>&q=turn on the bedroom light`

    Kopieer de URL en vervangen door uw sleutel voor de waarde van `<YOUR_KEY>`.

2. Plak de URL in een browservenster en druk op Enter. De browser wordt een JSON-resultaat die aangeeft dat LUIS detecteert de `HomeAutomation.TurnOn` intentie als het belangrijkste doel en de `HomeAutomation.Room` entiteit met de waarde `bedroom`.

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

3. Wijzig de waarde van de parameter `q=` in de URL in `turn off the living room light` en druk op Enter. Het resultaat geeft nu aan dat wordt gedetecteerd door LUIS de `HomeAutomation.TurnOff` intentie als het belangrijkste doel en de `HomeAutomation.Room` entiteit met de waarde `living room`. 

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
