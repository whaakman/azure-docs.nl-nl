---
title: QnA bot - Azure Bot Service - QnA Maker
titleSuffix: Azure Cognitive Services
description: Maak een chatbot QnA van de pagina publiceren voor een bestaande knowledge base. Deze bot maakt gebruik van de Bot Framework SDK v4-processors. U hoeft geen code voor het bouwen van de bot te schrijven, is alle code bedoeld voor u.
services: cognitive-services
author: tulasim88
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 06/11/2019
ms.author: tulasim
ms.openlocfilehash: b3bae01d65685aa9ea7bfc95d1f1454741d37b5e
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67053234"
---
# <a name="tutorial-create-a-qna-bot-with-azure-bot-service-v4"></a>Zelfstudie: Een QnA Bot maken met Azure Bot Service v4-processors

Maken van een chatbot QnA van de **publiceren** pagina voor een bestaande knowledge base. Deze bot maakt gebruik van de Bot Framework SDK v4-processors. U hoeft geen code voor het bouwen van de bot te schrijven, is alle code bedoeld voor u.

**In deze zelfstudie leert u het volgende:**

<!-- green checkmark -->
> [!div class="checklist"]
> * Een Azure Bot Service van een bestaande kennisdatabase maken
> * Chatten met de bot om te controleren of dat de code werkt 

## <a name="prerequisites"></a>Vereisten

U moet een gepubliceerde knowledge base voor deze zelfstudie. Als u een hebt, volgt u de stappen in [maken en antwoord in KB](create-publish-query-in-portal.md) zelfstudie om te maken van een QnA Maker knowledge base met vragen en antwoorden.

<a name="create-a-knowledge-base-bot"></a>

## <a name="create-a-qna-bot"></a>Maken van een QnA Bot

Maak een bot als een clienttoepassing voor de knowledge base. 

1. In de portal voor QnA Maker, gaat u naar de **publiceren** pagina en publiceren van uw knowledge base. Selecteer **Bot maken**. 

    ![In de portal voor QnA Maker, gaat u naar de pagina publiceren en publiceren van uw knowledge base. Selecteer maken Bot.](../media/qnamaker-tutorials-create-bot/create-bot-from-published-knowledge-base-page.png)

    De Azure-portal wordt geopend met de configuratie van de bot maken.

1.  Geef de instellingen voor het maken van de bot:

    |Instelling|Value|Doel|
    |--|--|--|
    |Botnaam|`my-tutorial-kb-bot`|Dit is de naam van de Azure-resource voor de bot.|
    |Abonnement|Zie doel.|Selecteer hetzelfde abonnement als u hebt gebruikt om de QnA Maker-resources te maken.|
    |Resourcegroep|`my-tutorial-rg`|De resourcegroep die wordt gebruikt voor alle bot-gerelateerde Azure-resources.|
    |Locatie|`west us`|Locatie van de Azure-resource van de bot.|
    |Prijscategorie|`F0`|De gratis laag voor de Azure botservice.|
    |Naam van app|`my-tutorial-kb-bot-app`|Dit is een web-app voor de ondersteuning van uw bot alleen. Dit moet niet dezelfde app-naam als de QnA Maker-service wordt al gebruikt. QnA Maker van web-app delen met een andere resource wordt niet ondersteund.|
    |SDK Language|C#|Dit is de onderliggende programmeertaal die worden gebruikt door de botframework SDK. Uw keuzes zijn [ C# ](https://github.com/Microsoft/botbuilder-dotnet) of [Node.js](https://github.com/Microsoft/botbuilder-js).|
    |QnA verificatiesleutel|**Niet wijzigen**|Deze waarde is ingevuld voor u.|
    |App service-plan/locatie|**Niet wijzigen**|Voor deze zelfstudie is de locatie niet belangrijk.|
    |Azure Storage|**Niet wijzigen**|Conversatie gegevens worden opgeslagen in Azure Storage-tabellen.|
    |Application Insights|**Niet wijzigen**|Logboekregistratie wordt verzonden naar Application Insights.|
    |Microsoft App-ID|**Niet wijzigen**|Active directory-gebruiker en het wachtwoord is vereist.|

    ![De bot knowledge base met deze instellingen maken.](../media/qnamaker-tutorials-create-bot/create-bot-from-published-knowledge-base.png)

    Wacht enkele minuten totdat de bot maken van het proces melding rapporteert geslaagd.

<a name="test-the-bot"></a>

## <a name="chat-with-the-bot"></a>Met de Bot chatten

1. Open de nieuwe bot-resource van de melding in de Azure-portal. 

    ![Open de nieuwe bot-resource van de melding in de Azure-portal.](../media/qnamaker-tutorials-create-bot/azure-portal-notifications.png)

1. Van **Bot management**, selecteer **Test in Web Chat** en voer: `How large can my KB be?`. De bot reageren met: 


    `The size of the knowledge base depends on the SKU of Azure search you choose when creating the QnA Maker service. Read [here](https://docs.microsoft.com/azure/cognitive-services/qnamaker/tutorials/choosing-capacity-qnamaker-deployment)for more details.`


    ![Test de nieuwe knowledge base-bot.](../media/qnamaker-tutorial-create-publish-query-in-portal/test-bot-in-web-chat-in-azure-portal.png)

    Zie voor meer informatie over Azure Bots [gebruik QnA Maker te beantwoorden vragen](https://docs.microsoft.com/azure/bot-service/bot-builder-howto-qna?view=azure-bot-service-4.0&tabs=cs)

## <a name="related-to-qna-maker-bots"></a>Met betrekking tot QnA Maker bots

* De QnA Maker help bot, die wordt gebruikt in de portal voor QnA Maker is beschikbaar als een [bot voorbeeld](https://github.com/Microsoft/BotBuilder-Samples/tree/master/experimental/csharp_dotnetcore/qnamaker-support-bot).
    ![QnA Maker help bot-pictogram is rood robot](../media/qnamaker-tutorials-create-bot/answer-bot-icon.PNG)
* [Gezondheidszorg bots](https://docs.microsoft.com/HealthBot/qna_model_howto) QnA Maker gebruiken als een van hun [taalmodellen](https://docs.microsoft.com/HealthBot/qna_model_howto).

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u klaar bent met de bot van deze zelfstudie, verwijdert u de bot in Azure portal. 

Als u een nieuwe resourcegroep voor de resources van de bot hebt gemaakt, verwijdert u de resourcegroep. 

Als u een nieuwe resourcegroep hebt gemaakt, moet u zoeken naar bronnen die zijn gekoppeld aan de bot. De eenvoudigste manier is om te zoeken door de naam van de bot en bot-app. De bot-resources zijn onder andere:

* De App Service-plan
* De Search-service
* De Cognitive service
* De appservice
* (Optioneel) kan het ook de application insights-service en de opslag voor de application insights-gegevens

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Concept: knowledge base](../concepts/knowledge-base.md)

## <a name="see-also"></a>Zie ook

- [Beheren van uw knowledge base](https://qnamaker.ai)
- [Uw bot in verschillende kanalen](https://docs.microsoft.com/azure/bot-service/bot-service-manage-channels)
