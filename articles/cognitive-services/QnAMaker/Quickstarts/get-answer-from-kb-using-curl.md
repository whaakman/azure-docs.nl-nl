---
title: 'Snelstartgids: Gebruik cURL antwoord ophalen uit knowledge base - QnA Maker'
titleSuffix: Azure Cognitive Services
description: In deze snelstartgids helpt u bij het ophalen van een antwoord uit uw knowledge base met cURL.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 12/04/2018
ms.author: diberry
ms.openlocfilehash: 0cbd25c0ea906c0b0f35b6ac0ae798505863ac8a
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53273252"
---
# <a name="quickstart-get-answer-from-knowledge-base-using-curl"></a>Snelstartgids: Antwoord ophalen uit knowledge base met cURL

In deze snelstartgids op basis van een cURL begeleidt u bij het ophalen van een antwoord uit uw knowledge base.

## <a name="prerequisites"></a>Vereisten

* Meest recente [ **cURL**](https://curl.haxx.se/).
* Hebt u een [QnA Maker-service](../How-To/set-up-qnamaker-service-azure.md) en hebben een [knowledge base met vragen en antwoorden](../Tutorials/create-publish-query-in-portal.md).

## <a name="publish-to-get-endpoint"></a>Publiceren-eindpunt ophalen

Wanneer u klaar bent voor het genereren van een antwoord op een vraag uit uw knowledge base, [publiceren](../How-to/publish-knowledge-base.md) uw knowledge base.

## <a name="use-production-endpoint-with-curl"></a>Productie-eindpunt gebruiken met cURL

Wanneer uw knowledge base wordt gepubliceerd, de **publiceren** pagina vindt u de instellingen van de HTTP-aanvraag voor het genereren van een antwoord. De **CURL** tabblad bevat de instellingen die vereist zijn voor het genereren van een antwoord van het opdrachtregelprogramma [CURL](https://www.getpostman.com).

[![Resultaten publiceren](../media/qnamaker-use-to-generate-answer/curl-command-on-publish-page.png)](../media/qnamaker-use-to-generate-answer/curl-command-on-publish-page.png#lightbox)

Voor het genereren van een antwoord met CURL, voert u de volgende stappen uit:

1. Kopieer de tekst op het tabblad CURL. 
1. Open een opdrachtregel of de terminal en plak de tekst.
1. Bewerk de vraag om te worden relevant zijn voor uw knowledge base. Wees voorzichtig niet te verwijderen van de betreffende JSON rond de vraag.
1. Voer de opdracht. 
1. Het antwoord bevat de relevante informatie over het antwoord. 

    ```bash
    > curl -X POST https://qnamaker-f0.azurewebsites.net/qnamaker/knowledgebases/1111f8c-d01b-4698-a2de-85b0dbf3358c/generateAnswer -H "Authorization: EndpointKey 111841fb-c208-4a72-9412-03b6f3e55ca1" -H "Content-type: application/json" -d "{'question':'How do I programmatically update my Knowledge Base?'}"
    {
      "answers": [
        {
          "questions": [
            "How do I programmatically update my Knowledge Base?"
          ],
          "answer": "You can use our REST APIs to manage your Knowledge Base. See here for details: https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da7600",
          "score": 100.0,
          "id": 18,
          "source": "Custom Editorial",
          "metadata": [
            {
              "name": "category",
              "value": "api"
            }
          ]
        }
      ]
    }
    ```

## <a name="use-staging-endpoint-with-curl"></a>Staging-eindpunt gebruiken met cURL

Als u wilt op een antwoord van de staging-eindpunt, gebruikt u de Booleaanse parameter querystring `isTest` met de waarde van `true`.

`isTest=true`

## <a name="next-steps"></a>Volgende stappen

De pagina publiceren bevat ook informatie om [een antwoord genereren](get-answer-from-kb-using-postman.md) met Postman. 

> [!div class="nextstepaction"]
> [Tijdens het genereren van een antwoord met behulp van metagegevens](../How-to/metadata-generateanswer-usage.md)