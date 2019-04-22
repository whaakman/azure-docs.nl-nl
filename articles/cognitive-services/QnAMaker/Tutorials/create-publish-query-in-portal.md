---
title: Maken, publiceren en antwoorden in QnA Maker
titleSuffix: Azure Cognitive Services
description: Maak een nieuwe knowledge base met vragen en antwoorden van een openbare web gebaseerde Veelgestelde vragen. Opslaan, trainen en publiceren van de knowledge base. Zodra de knowledge base is gepubliceerd, een vraag verzenden en ontvangen van een antwoord met een CURL-opdracht. Maak een bot en testen van de bot met dezelfde vraag.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: tutorial
ms.date: 04/08/2019
ms.author: diberry
ms.openlocfilehash: 299dd61055503f0b5a11cbe97e137e4760edadda
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/18/2019
ms.locfileid: "59266934"
---
# <a name="tutorial-from-qna-maker-portal-create-a-knowledge-base"></a>Zelfstudie: Een kennisdatabase maken vanuit de QnA Maker portal

Maak een nieuwe knowledge base met vragen en antwoorden van een openbare web gebaseerde Veelgestelde vragen. Opslaan, trainen en publiceren van de knowledge base. Zodra de knowledge base is gepubliceerd, een vraag verzenden en ontvangen van een antwoord met een Curl-opdracht. Maak een bot en testen van de bot met dezelfde vraag. 

In deze zelfstudie leert u het volgende: 

> [!div class="checklist"]
> * Een knowledge base maken in de QnA Maker-portal
> * De knowledge base controleren, opslaan en trainen
> * De knowledge base publiceren
> * Curl gebruiken om een query uit te voeren op de knowledge base
> * Maken van een bot
> 
> [!NOTE]
> De programmaversie van deze zelfstudie is beschikbaar met een volledige oplossing in de GitHub-opslagplaats [**Azure-Samples/cognitive-services-qnamaker-csharp**](https://github.com/Azure-Samples/cognitive-services-qnamaker-csharp/tree/master/documentation-samples/tutorials/create-publish-answer-knowledge-base).

## <a name="prerequisites"></a>Vereisten

Voor deze zelfstudie moet u beschikken over een bestaande [QnA Maker-service](../How-To/set-up-qnamaker-service-azure.md). 

## <a name="create-a-knowledge-base"></a>Een kennisdatabase maken 

1. Aanmelden bij de [QnA Maker](https://www.qnamaker.ai)-portal. 

1. Selecteer **Een knowledge base maken** in het menu bovenaan.

    ![Stap 1 van het proces voor het maken van een KB](../media/qnamaker-tutorial-create-publish-query-in-portal/create-kb-step-1.png)

1. Sla de eerste stap over omdat u uw bestaande QnA Maker-service wilt gebruiken. 

1. Selecteer uw bestaande instellingen in de volgende stap:  

    |Instelling|Doel|
    |--|--|
    |Microsoft Azure Directory-id|Uw _ID voor Microsoft Azure Directory_ is gekoppeld aan het account waarmee u zich aanmelden bij de Azure portal en de QnA Maker-portal. |
    |Azure-abonnements-id|De factureringsrekening waarin u de QnA Maker-resource hebt gemaakt.|
    |Azure QnA Service|Uw bestaande QnA Maker-resource.|

    ![Stap 2 van het proces voor het maken van een KB](../media/qnamaker-tutorial-create-publish-query-in-portal/create-kb-step-2.png)

1. Voer in de volgende stap de naam van uw knowledge base in, `My Tutorial kb`.

    ![Stap 3 van het proces voor het maken van een KB](../media/qnamaker-tutorial-create-publish-query-in-portal/create-kb-step-3.png)

1. Vul uw kb in de volgende stap met de volgende instellingen:  

    |Naam van instelling|Instellingswaarde|Doel|
    |--|--|--|
    |URL|`https://docs.microsoft.com/azure/cognitive-services/qnamaker/faqs` |De inhoud van de veelgestelde vragen bij die URL zijn ingedeeld met een vraag gevolgd door een antwoord. QnA Maker kan deze indeling interpreteren om vragen en de bijbehorende antwoorden te extraheren.|
    |File |_niet gebruikt in deze zelfstudie_|Hiermee worden bestanden voor vragen en antwoorden geüpload. |
    |De persoonlijkheid 'Heen- en weergepraat'|De vriend(in)|Dit biedt een vriendelijke en informele persoonlijkheid voor veelgestelde vragen en antwoorden. U kunt deze vragen en antwoorden later bewerken. |

    ![Stap 4 van het proces voor het maken van een KB](../media/qnamaker-tutorial-create-publish-query-in-portal/create-kb-step-4.png)

1. Selecteer **Uw KB maken** om het proces te voltooien.

    ![Stap 5 van het proces voor het maken van een KB](../media/qnamaker-tutorial-create-publish-query-in-portal/create-kb-step-5.png)

## <a name="review-kb-save-and-train"></a>KB controleren, opslaan en trainen

1. Controleer de vragen en antwoorden. De eerste pagina bestaat uit vragen en antwoorden uit de URL. 

    ![Opslaan en trainen](../media/qnamaker-tutorial-create-publish-query-in-portal/save-and-train-kb.png)

1. Selecteer de laatste pagina van de vragen en antwoorden vanaf de onderkant van de tabel. De pagina toont vragen en antwoorden van de persoonlijkheid 'Heen- en weergepraat'. 

1. Selecteer metagegevenspictogram vanuit de werkbalk boven de lijst met vragen en antwoorden. Hiermee worden de tags met metagegevens voor elke vraag en antwoord weergegeven. Voor de heen- en weergepraatvragen zijn de **editorial: chit-chat**-metagegevens al ingesteld. Deze metagegevens worden geretourneerd naar de clienttoepassing, samen met het geselecteerde antwoord. De clienttoepassing, bijvoorbeeld een chatbot, kan deze gefilterde metagegevens gebruiken om extra verwerking of interactie met de gebruiker te bepalen.

    ![Tags met metagegevens weergeven](../media/qnamaker-tutorial-create-publish-query-in-portal/save-and-train-kb-chit-chat.png)

1. Selecteer **Opslaan en trainen** in de bovenste menubalk.

## <a name="publish-to-get-kb-endpoints"></a>Publiceren om KB-eindpunten te verkrijgen

Selecteer de knop **Publiceren** in het menu bovenaan. Wanneer u op de pagina Publiceren bent, selecteert u **Publiceren**, naast de knop **Annuleren**.

![Publiceren](../media/qnamaker-tutorial-create-publish-query-in-portal/publish-1.png)

Nadat de KB is gepubliceerd, wordt het eindpunt weergegeven

![Eindpuntinstellingen van pagina publiceren](../media/qnamaker-tutorial-create-publish-query-in-portal/publish-2.png)

Sluit dit niet **publiceren** pagina, gebruikt u dit om te maken van een bot later in de zelfstudie. 

## <a name="use-curl-to-query-for-an-faq-answer"></a>Curl gebruiken voor de query voor een antwoord Veelgestelde vragen

1. Selecteer het tabblad **Curl**. 

    ![Curl-opdracht](../media/qnamaker-tutorial-create-publish-query-in-portal/publish-3-curl.png)

1. Kopieer de tekst van het tabblad **Curl** en voer dit uit in een voor Curl ingeschakelde terminal of opdrachtregel. De waarde van de autorisatie-header bevat de tekst `Endpoint` met een volgspatie en vervolgens de sleutel.

1. Vervang `<Your question>` door `How large can my KB be?`. Dit komt dicht bij de vraag, `How large a knowledge base can I create?`, in de buurt, maar is niet precies hetzelfde. QnA Maker past natuurlijke taalverwerking toe om te bepalen of de twee vragen hetzelfde zijn.     

1. Geeft u de opdracht Curl en de JSON-antwoord met inbegrip van de score en antwoord ontvangen. 

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

## <a name="use-curl-to-query-for-a-chit-chat-answer"></a>Curl gebruiken voor de query voor een Chit chat-antwoord

1. Vervang in de terminal Curl ingeschakeld `How large can my KB be?` met een bot einde van de conversatie-instructie van de gebruiker, zoals `Thank you`.   

1. Geeft u de opdracht Curl en de JSON-antwoord met inbegrip van de score en antwoord ontvangen. 

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

    Omdat de vraag `Thank you` precies overeenkwam met een heen- en weergepraatvraag, is QnA Maker volledig zeker met de score van 100. QnA Maker heeft ook alle gerelateerde vragen geretourneerd, evenals de metagegevenseigenschap met de informatie over de tag met heen- en weergepraatmetagegevens.  

## <a name="use-curl-to-query-for-the-default-answer"></a>Curl gebruiken voor de query voor het standaard-antwoord

Alle vragen waarover QnA Maker niet zeker is over het antwoord ontvangen het standaardantwoord. Dit antwoord wordt geconfigureerd in Azure Portal. 

1. Vervang in de voor Curl ingeschakelde terminal `Thank you` door `x`. 

1. Geeft u de opdracht Curl en de JSON-antwoord met inbegrip van de score en antwoord ontvangen. 

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
    
    QnA Maker geretourneerd een score van `0`, wat betekent dat er geen vertrouwen, maar deze ook de standaard-antwoord geretourneerd. 

## <a name="create-a-knowledge-base-bot"></a>Maken van een knowledge base-bot

Zie voor meer informatie, [een chatbot maken met deze kennisdatabase](create-qna-bot.md).

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u klaar bent met het knowledge base-bot, verwijdert u de resourcegroep, `my-tutorial-rg`, te verwijderen van alle de Azure-resources in de bot-proces hebt gemaakt.

Wanneer u klaar bent met de knowledge base, in de portal voor QnA Maker, selecteert u **mijn knowledge bases**, selecteer vervolgens de knowledge base, **mijn zelfstudie kb**, selecteer vervolgens het verwijderpictogram aan de rechterkant in die rij.  

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie, [gegevensbronnen ondersteund](../Concepts/data-sources-supported.md) voor meer informatie over ondersteuning voor bestandsindelingen. 

Lees meer over [persoonlijkheden](../Concepts/best-practices.md#chit-chat) voor heen- en weergepraat.

Zie [Geen overeenkomst gevonden](../Concepts/confidence-score.md#no-match-found) voor meer informatie over het standaardantwoord. 

> [!div class="nextstepaction"]
> [Een chatbot met deze kennisdatabase maken](create-qna-bot.md)