---
title: Controleer de container instantie van de sentiment-analyse
titleSuffix: Azure Cognitive Services
description: Meer informatie over het controleren van het exemplaar van de sentiment Analysis-container.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 06/26/2019
ms.author: dapine
ms.openlocfilehash: 1303d753b1cbfabe7ddd3442e0880b0bffe089b3
ms.sourcegitcommit: b49431b29a53efaa5b82f9be0f8a714f668c38ab
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/22/2019
ms.locfileid: "68377439"
---
## <a name="verify-the-sentiment-analysis-container-instance"></a>Controleer de container instantie van de sentiment-analyse

1. Selecteer het tabblad **overzicht** en kopieer het IP-adres.
1. Open een nieuw browser tabblad en voer het IP-adres in. Voer `http://<IP-address>:5000 (http://55.55.55.55:5000`bijvoorbeeld in). De start pagina van de container wordt weer gegeven, waarin u kunt zien dat de container wordt uitgevoerd.

    ![De start pagina van de container weer geven om te controleren of deze wordt uitgevoerd](../media/how-tos/container-instance/swagger-docs-on-container.png).

1. Selecteer de koppeling **Service-API-beschrijving** om naar de Swagger-pagina van de container te gaan.

1. Kies een van de **post** -api's en selecteer **try-out**. De para meters worden weer gegeven, met inbegrip van de volgende voorbeeld invoer:

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

1. Vervang de invoer door de volgende JSON-inhoud:

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

1. Stel **showStats** in op True.

1. Selecteer **uitvoeren** om de sentiment van de tekst te bepalen.

    Het model dat in de container is verpakt, genereert een Score van 0 tot 1, waarbij 0 negatief is en 1 positief is.

    Het JSON-antwoord dat wordt geretourneerd bevat sentiment voor de bijgewerkte tekst invoer:

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

We kunnen nu het document `id` van de JSON-gegevens van de reactie Payload koppelen aan het oorspronkelijke document `id`met de aanvraag lading. De Score van meer dan `.98` geeft een sterk positieve sentiment aan.