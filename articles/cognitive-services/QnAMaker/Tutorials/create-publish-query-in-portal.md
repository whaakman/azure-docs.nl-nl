---
title: 'Zelfstudie: Een knowledge base maken, publiceren en vragen erin beantwoorden in QnA Maker-portal'
titleSuffix: Azure Cognitive Services
description: In deze zelfstudie op basis van een portal gaat u via een programma een kennisdatabase maken en publiceren en vervolgens een vraag uit de knowledge base beantwoorden.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.technology: qna-maker
ms.topic: tutorial
ms.date: 10/29/2018
ms.author: diberry
ms.openlocfilehash: 08f708f740b90f27af5443b46c5d03bef688bd45
ms.sourcegitcommit: fbdfcac863385daa0c4377b92995ab547c51dd4f
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/29/2018
ms.locfileid: "50221555"
---
# <a name="tutorial-create-a-knowledge-base-then-answer-question-via-the-qna-maker-portal"></a>Zelfstudie: Een knowledge base maken en vervolgens een vraag beantwoorden via de QnA Maker-portal

In deze zelfstudie gaat u een kennisdatabase maken en publiceren en vervolgens een vraag uit de knowledge base beantwoorden.

In deze zelfstudie leert u het volgende: 

> [!div class="checklist"]
* Een knowledge base maken in de QnA Maker-portal
* De knowledge base controleren, opslaan en trainen
* De knowledge base publiceren
* Curl gebruiken om een query uit te voeren op de knowledge base

> [!NOTE] 
> De programmaversie van deze zelfstudie is beschikbaar met een volledige oplossing in de [**Github-opslagplaats**Azure-Samples/cognitive-services-qnamaker-csharp](https://github.com/Azure-Samples/cognitive-services-qnamaker-csharp/tree/master/documentation-samples/tutorials/create-publish-answer-knowledge-base).

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
    |Microsoft Azure Directory-id|Uw _Microsoft Azure Directory-id_ is gekoppeld aan het account waarmee u zich aanmeldt bij Azure Portal en de QnA Maker-portal. |
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

1. Selecteer het tandwiel vanuit de werkbalk boven de lijst met vragen en antwoorden. Hiermee worden de filters voor elke vraag en antwoord weergegeven. Voor de heen- en weergepraatvragen is het filter **editorial: chit-chat** al ingesteld. Dit filter wordt geretourneerd naar de clienttoepassing, samen met het geselecteerde antwoord. De clienttoepassing, bijvoorbeeld een chatbot, kan dit filter gebruiken om extra verwerking of interactie met de gebruiker te bepalen.

    ![Weergavefilters](../media/qnamaker-tutorial-create-publish-query-in-portal/save-and-train-kb-chit-chat.png)

1. Selecteer **Opslaan en trainen** in de bovenste menubalk.

## <a name="publish-to-get-kb-endpoints"></a>Publiceren om KB-eindpunten te verkrijgen

Selecteer de knop **Publiceren** in het menu bovenaan. Wanneer u op de pagina Publiceren bent, selecteert u **Publiceren**, naast de knop **Annuleren**.

![Publiceren](../media/qnamaker-tutorial-create-publish-query-in-portal/publish-1.png)

Nadat de KB is gepubliceerd, wordt het eindpunt weergegeven

![Publiceren](../media/qnamaker-tutorial-create-publish-query-in-portal/publish-2.png)

## <a name="use-curl-to-query-for-an-faq-answer"></a>Curl gebruiken om naar een antwoord in Veelgestelde vragen te zoeken

1. Selecteer het tabblad **Curl**. 

    ![Curl-opdracht](../media/qnamaker-tutorial-create-publish-query-in-portal/publish-3-curl.png)

1. Kopieer de tekst van het tabblad **Curl** en voer dit uit in een voor Curl ingeschakelde terminal of opdrachtregel. De waarde van de autorisatie-header bevat de tekst `Endpoint ` met een volgspatie en vervolgens de sleutel.

1. Vervang `<Your question>` door `How large can my KB be?`. Dit komt dicht bij de vraag, `How large a knowledge base can I create?`, in de buurt, maar is niet precies hetzelfde. QnA Maker past natuurlijke taalverwerking toe om te bepalen of de twee vragen hetzelfde zijn.     

1. Voer de CURL-opdracht uit en ontvang het JSON-antwoord inclusief de score en het antwoord. 

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

## <a name="use-curl-to-query-for-a-chit-chat-answer"></a>Curl gebruiken om te zoeken naar een heen- en weergepraatantwoord

1. Vervang in de voor Curl ingeschakelde terminal `How large can my KB be?` door een botinstructie van de gebruiker die het gesprek beëindigt, zoals `Thank you`.   

1. Voer de CURL-opdracht uit en ontvang het JSON-antwoord inclusief de score en het antwoord. 

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

    Omdat de vraag `Thank you` precies overeenkwam met een heen- en weergepraatvraag, is QnA Maker volledig zeker met de score van 100. QnA Maker heeft ook alle gerelateerde vragen geretourneerd, evenals de metagegevenseigenschap met de informatie over het heen- en weergepraatfilter.  

## <a name="use-curl-to-query-for-the-default-answer"></a>Curl gebruiken om te zoeken naar het standaardantwoord

Alle vragen waarover QnA Maker niet zeker is over het antwoord ontvangen het standaardantwoord. Dit antwoord wordt geconfigureerd in Azure Portal. 

1. Vervang in de voor Curl ingeschakelde terminal `Thank you` door `x`. 

1. Voer de CURL-opdracht uit en ontvang het JSON-antwoord inclusief de score en het antwoord. 

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
    
    QnA Maker heeft een score van 0 geretourneerd, wat betekent er geen vertrouwen is, maar heeft ook het standaardantwoord geretourneerd. 

## <a name="next-steps"></a>Volgende stappen

Zie [Ondersteunde gegevensbronnen](../Concepts/data-sources-supported.md) voor meer informatie over ondersteunde bestandsindelingen. 

Lees meer over [persoonlijkheden](../Concepts/best-practices.md#chit-chat) voor heen- en weergepraat.

Zie [Geen overeenkomst gevonden](../Concepts/confidence-score.md#no-match-found) voor meer informatie over het standaardantwoord. 

> [!div class="nextstepaction"]
> [Knowledge base-concepten](../Concepts/knowledge-base.md)