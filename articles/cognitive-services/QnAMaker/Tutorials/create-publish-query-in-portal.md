---
title: Maken, publiceren en beantwoorden in QnA Maker
titleSuffix: Azure Cognitive Services
description: Maak een nieuwe knowledge base met vragen en antwoorden van een openbare web gebaseerde Veelgestelde vragen. Opslaan, trainen en publiceren van de knowledge base. Nadat de knowledge base is gepubliceerd, een vraag verzenden en ontvangen van een antwoord met een cURL-opdracht. Maak een bot, en testen van de bot met dezelfde vraag.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: tutorial
ms.date: 05/07/2019
ms.author: diberry
ms.openlocfilehash: a13e0cb0e594571344b16d007ef13475b384b73d
ms.sourcegitcommit: 18a0d58358ec860c87961a45d10403079113164d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/05/2019
ms.locfileid: "66692992"
---
# <a name="tutorial-from-the-qna-maker-portal-create-a-knowledge-base"></a>Zelfstudie: Een kennisdatabase maken vanuit de portal QnA Maker

Maak een nieuwe knowledge base met vragen en antwoorden van een openbare web gebaseerde Veelgestelde vragen. Opslaan, trainen en publiceren van de knowledge base. Nadat de knowledge base is gepubliceerd, een vraag verzenden en ontvangen van een antwoord met een cURL-opdracht. Maak een bot, en testen van de bot met dezelfde vraag. 

In deze zelfstudie leert u het volgende: 

> [!div class="checklist"]
> * Een kennisdatabase maken in de portal voor QnA Maker.
> * Bekijk, opslaan en de kennisdatabase trainen.
> * Publiceer de knowledge base.
> * CURL gebruiken om te vragen van de knowledge base.
> * Maak een bot.
 

> [!NOTE]
> De programmaversie van deze zelfstudie is beschikbaar met een volledige oplossing in de GitHub-opslagplaats [**Azure-Samples/cognitive-services-qnamaker-csharp**](https://github.com/Azure-Samples/cognitive-services-qnamaker-csharp/tree/master/documentation-samples/tutorials/create-publish-answer-knowledge-base).

## <a name="prerequisites"></a>Vereisten

Voor deze zelfstudie moet u beschikken over een bestaande [QnA Maker-service](../How-To/set-up-qnamaker-service-azure.md). 

## <a name="create-a-knowledge-base"></a>Een kennisdatabase maken 

1. Aanmelden bij de [QnA Maker](https://www.qnamaker.ai)-portal. 

1. Selecteer **Een knowledge base maken** in het menu bovenaan.

    ![Schermafbeelding van de QnA Maker portal](../media/qnamaker-tutorial-create-publish-query-in-portal/create-kb-step-1.png)

1. De eerste stap overslaan omdat u uw bestaande QnA Maker-service wilt gebruiken. 

1. Selecteer de bestaande instellingen:  

    |Instelling|Doel|
    |--|--|
    |Map-ID van Microsoft Azure|Deze ID is gekoppeld aan het account waarmee u zich aanmelden bij de Azure portal en de QnA Maker-portal. |
    |Azure-abonnements-id|De facturering account waarin u de QnA Maker-resource hebt gemaakt.|
    |Azure QnA Service|Uw bestaande QnA Maker-resource.|

    ![Schermafbeelding van de QnA Maker portal](../media/qnamaker-tutorial-create-publish-query-in-portal/create-kb-step-2.png)

1. Voer de naam van uw knowledge base, `My Tutorial kb`.

    ![Schermafbeelding van de QnA Maker portal](../media/qnamaker-tutorial-create-publish-query-in-portal/create-kb-step-3.png)

1. Vul uw knowledge base met de volgende instellingen:  

    |Naam van instelling|Instellingswaarde|Doel|
    |--|--|--|
    |URL|`https://docs.microsoft.com/azure/cognitive-services/qnamaker/faqs` |De inhoud van de veelgestelde vragen bij die URL zijn ingedeeld met een vraag gevolgd door een antwoord. QnA Maker kan deze indeling interpreteren om vragen en de bijbehorende antwoorden te extraheren.|
    |File |_niet gebruikt in deze zelfstudie_|Hiermee worden bestanden voor vragen en antwoorden geüpload. |
    |De persoonlijkheid 'Heen- en weergepraat'|Beschrijvende|Dit biedt een vriendelijke en casual [persoonlijkheid](../Concepts/best-practices.md#chit-chat) op veelgestelde vragen en antwoorden. U kunt deze vragen en antwoorden later bewerken. |

    ![Schermafbeelding van de QnA Maker portal](../media/qnamaker-tutorial-create-publish-query-in-portal/create-kb-step-4.png)

1. Selecteer **Uw KB maken** om het proces te voltooien.

    ![Schermafbeelding van de QnA Maker portal](../media/qnamaker-tutorial-create-publish-query-in-portal/create-kb-step-5.png)

## <a name="review-save-and-train-the-knowledge-base"></a>De knowledge base controleren, opslaan en trainen

1. Controleer de vragen en antwoorden. De eerste pagina bestaat uit vragen en antwoorden uit de URL. 

    ![Schermafbeelding van de QnA Maker portal](../media/qnamaker-tutorial-create-publish-query-in-portal/save-and-train-kb.png)

1. Selecteer de laatste pagina van de vragen en antwoorden vanaf de onderkant van de tabel. De pagina toont vragen en antwoorden van de persoonlijkheid 'Heen- en weergepraat'. 

1. Selecteer in de werkbalk boven de lijst met vragen en antwoorden, de **weergaveopties** pictogram en selecteer vervolgens **metagegevens weergeven**. Hiermee worden de tags met metagegevens voor elke vraag en antwoord weergegeven. Voor de heen- en weergepraatvragen zijn de **editorial: chit-chat**-metagegevens al ingesteld. Deze metagegevens wordt geretourneerd naar de clienttoepassing, samen met het geselecteerde antwoord. De clienttoepassing, bijvoorbeeld een chatbot, kan deze gefilterde metagegevens gebruiken om extra verwerking of interactie met de gebruiker te bepalen.

    ![Schermafbeelding van de QnA Maker portal](../media/qnamaker-tutorial-create-publish-query-in-portal/save-and-train-kb-chit-chat.png)

1. Selecteer **Opslaan en trainen** in de bovenste menubalk.

## <a name="publish-to-get-knowledge-base-endpoints"></a>Als u wilt ophalen van knowledge base-eindpunten publiceren

Selecteer de knop **Publiceren** in het menu bovenaan. Selecteer **Publiceren** op de publicatiepagina.

![Schermafbeelding van de QnA Maker portal](../media/qnamaker-tutorial-create-publish-query-in-portal/publish-1.png)

Nadat de knowledge base is gepubliceerd, wordt het eindpunt wordt weergegeven.

![Schermafbeelding van de instellingen van eindpunten](../media/qnamaker-tutorial-create-publish-query-in-portal/publish-2.png)

Sluit dit niet **publiceren** pagina. U moet deze later in de zelfstudie, het maken van een bot. 

## <a name="use-curl-to-query-for-an-faq-answer"></a>CURL naar query gebruiken voor een antwoord Veelgestelde vragen

1. Selecteer het tabblad **Curl**. 

    ![Schermafbeelding van Curl tabblad](../media/qnamaker-tutorial-create-publish-query-in-portal/publish-3-curl.png)

1. Kopieer de tekst van de **Curl** tabblad en de App uitvoeren in een cURL ingeschakeld terminal of de opdrachtregel. De autorisatie-header-waarde bevat de tekst `Endpoint`, met een spatie en klikt u vervolgens de sleutel.

1. Vervang `<Your question>` door `How large can my KB be?`. Dit komt dicht bij de vraag, `How large a knowledge base can I create?`, in de buurt, maar is niet precies hetzelfde. QnA Maker past natuurlijke taalverwerking toe om te bepalen of de twee vragen hetzelfde zijn.     

1. Voer de opdracht cURL en ontvangen van de JSON-antwoord, met inbegrip van de score en -antwoordsessie. 

    ```TXT
      % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                     Dload  Upload   Total   Spent    Left  Speed
    100   581  100   543  100    38    418     29  0:00:01  0:00:01 --:--:--   447{
      "answers": [
        {
          "questions": [
            "How large a knowledge base can I create?"
          ],
          "answer": "The size of the knowledge base depends on the SKU of Azure search you choose when creating the QnA Maker service. Read [here](https://docs.microsoft.com/azure/cognitive-services/qnamaker/tutorials/choosing-capacity-qnamaker-deployment)for more details.",
          "score": 42.81,
          "id": 2,
          "source": "https://docs.microsoft.com/azure/cognitive-services/qnamaker/faqs",
          "metadata": []
        }
      ]
    }
    
    ```

    QnA Maker is enigszins zeker met de score van 42.81%.  

## <a name="use-curl-to-query-for-a-chit-chat-answer"></a>CURL naar query gebruiken voor een Chit chat-antwoord

1. Vervang in de terminal cURL ingeschakeld `How large can my KB be?` met een bot einde van de conversatie-instructie van de gebruiker, zoals `Thank you`.   

1. Voer de opdracht cURL en ontvangen van de JSON-antwoord, met inbegrip van de score en -antwoordsessie. 

    ```TXT
      % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                     Dload  Upload   Total   Spent    Left  Speed
    100   525  100   501  100    24    525     25 --:--:-- --:--:-- --:--:--   550{
      "answers": [
        {
          "questions": [
            "Thank you",
            "Thanks",
            "Thnx",
            "Kthx",
            "I appreciate it",
            "Thank you so much",
            "I thank you",
            "My sincere thank"
          ],
          "answer": "You're very welcome.",
          "score": 100.0,
          "id": 109,
          "source": "qna_chitchat_the_friend.tsv",
          "metadata": [
            {
              "name": "editorial",
              "value": "chitchat"
            }
          ]
        }
      ]
    }
   
    ```

    Omdat de vraag `Thank you` precies overeenkwam met een heen- en weergepraatvraag, is QnA Maker volledig zeker met de score van 100. QnA Maker worden ook alle gerelateerde vragen, evenals de eigenschap metadata met informatie over de code van de Chit chat metagegevens geretourneerd.  

## <a name="use-curl-to-query-for-the-default-answer"></a>CURL naar query gebruiken voor het standaard-antwoord

Een vraag die QnA Maker niet zeker weet over ontvangt de standaard-antwoord. Dit antwoord wordt geconfigureerd in Azure Portal. 

1. Vervang in de terminal cURL ingeschakeld `Thank you` met `x`. 

1. Voer de opdracht cURL en ontvangen van de JSON-antwoord, met inbegrip van de score en -antwoordsessie. 

    ```TXT
      % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                     Dload  Upload   Total   Spent    Left  Speed
    100   186  100   170  100    16    272     25 --:--:-- --:--:-- --:--:--   297{
      "answers": [
        {
          "questions": [],
          "answer": "No good match found in KB.",
          "score": 0.0,
          "id": -1,
          "metadata": []
        }
      ]
    }
    ```
    
    QnA Maker geretourneerd een score van `0`, wat betekent dat er geen vertrouwen. Het ook de standaard-antwoord geretourneerd. 

## <a name="create-a-knowledge-base-bot"></a>Maken van een knowledge base-bot

Zie voor meer informatie, [een chatbot maken met deze kennisdatabase](create-qna-bot.md).

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u klaar bent met het knowledge base-bot, verwijdert u de resourcegroep, `my-tutorial-rg`, te verwijderen van alle de Azure-resources in de bot-proces hebt gemaakt.

Wanneer u klaar bent met de knowledge base, in de portal voor QnA Maker, selecteert u **mijn knowledge bases**. Selecteer vervolgens de knowledge base, **mijn zelfstudie kb**, en selecteer het verwijderpictogram aan de rechterkant in die rij.  

## <a name="next-steps"></a>Volgende stappen

Zie [Ondersteunde gegevensbronnen](../Concepts/data-sources-supported.md) voor meer informatie over ondersteunde bestandsindelingen. 

Lees meer over [persoonlijkheden](../Concepts/best-practices.md#chit-chat) voor heen- en weergepraat.

Zie [Geen overeenkomst gevonden](../Concepts/confidence-score.md#no-match-found) voor meer informatie over het standaardantwoord. 

> [!div class="nextstepaction"]
> [Een chatbot met deze kennisdatabase maken](create-qna-bot.md)