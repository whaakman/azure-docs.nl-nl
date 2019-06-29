---
title: Ondersteuning voor containers
titleSuffix: Azure Cognitive Services
description: Informatie over het controleren van de containerinstantie sentiment-analyse.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 06/26/2019
ms.author: dapine
ms.openlocfilehash: f68d9c7098f2b1ca782e2522c632c2e267b35336
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67455135"
---
## <a name="verify-the-sentiment-analysis-container-instance"></a>Controleer of de containerinstantie Sentimentanalyse

1. Selecteer de **overzicht** tabblad en kopieer het IP-adres.
1. Open een nieuw browsertabblad en gebruikt u het IP-adres bijvoorbeeld `http://<IP-address>:5000 (http://55.55.55.55:5000`). Startpagina van de container wordt weergegeven, zodat u weet de container wordt uitgevoerd.

    ![Bekijk de startpagina van de Container om te controleren of dat deze wordt uitgevoerd](../media/how-tos/container-instance/swagger-docs-on-container.png)

1. Selecteer de **beschrijving van de API-Service** koppeling om te navigeren naar de pagina van de swagger containers.

1. Kies een van de **POST** API's en selecteer **Try it out in**.  De parameters worden weergegeven met inbegrip van het voorbeeld van invoer:

    ```json
    {
      "documents": [
        {
          "language": "en",
          "id": "1",
          "text": "Hello world. This is some input text that I love."
        },
        {
          "language": "fr",
          "id": "2",
          "text": "Bonjour tout le monde"
        },
        {
          "language": "es",
          "id": "3",
          "text": "La carretera estaba atascada. Había mucho tráfico el día de ayer."
        }
      ]
    }
    ```

1. Vervang de invoer door de volgende JSON:

    ```json
    {
      "documents": [
        {
          "language": "en",
          "id": "7",
          "text": "I was fortunate to attend the KubeCon Conference in Barcelona, it is one of the best conferences I have ever attended. Great people, great sessions and I thoroughly enjoyed it!"
        }
      ]
    }
    ```

1. Stel **showStats** op ' True '.

1. Selecteer **Execute** om te bepalen het gevoel van de tekst.

    Het model dat zich in de container genereert een score tussen 0 en 1, waarbij 0 negatief is en 1 positief is.

    De JSON-antwoord geretourneerd bevat gevoel voor de bijgewerkte tekstinvoer:

    ```json
    {
      "documents": [
      {
        "id": "7",
        "score": 0.9826303720474243,
        "statistics": {
          "charactersCount": 176,
            "transactionsCount": 1
          }
        }
      ],
      "errors": [],
      "statistics": {
        "documentsCount": 1,
        "validDocumentsCount": 1,
        "erroneousDocumentsCount": 0,
        "transactionsCount": 1
      }
    }
    ```

We kunnen het document nu correleren `id` van de antwoordpayloads JSON naar het oorspronkelijke aanvraag nettolading document `id`, en Zie dat er een score van is via `.98` die wijzen op een zeer positieve sentiment.