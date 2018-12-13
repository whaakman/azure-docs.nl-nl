---
title: QnA bot - Azure Bot Service - QnA Maker
titleSuffix: Azure Cognitive Services
description: In deze zelfstudie helpt u bij het bouwen van een QnA bot met Azure Bot service v3 in Azure portal.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: qna-maker`
ms.topic: article
ms.date: 10/25/2018
ms.author: tulasim
ms.openlocfilehash: f5587e14a0250e7312f1c95598b481bd052931c3
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2018
ms.locfileid: "53094828"
---
# <a name="tutorial-create-a-qna-bot-with-azure-bot-service-v3"></a>Zelfstudie: Een QnA Bot maken met Azure Bot Service v3

In deze zelfstudie begeleidt u bij het bouwen van een QnA bot met Azure Bot service v3 in de [Azure-portal](https://portal.azure.com) zonder een code te schrijven. Een gepubliceerde knowledge base (KB) verbinding te maken met een bot is net zo eenvoudig als bot toepassingsinstellingen wijzigen. 

> [!Note] 
> In dit onderwerp is van versie 3 van de SDK-Bot. U vindt versie 4 [hier](https://docs.microsoft.com/azure/bot-service/bot-builder-howto-qna?view=azure-bot-service-4.0&tabs=cs). 

**In deze zelfstudie leert u het volgende:**

<!-- green checkmark -->
> [!div class="checklist"]
> * Een Azure Bot Service maken met de QnA Maker-sjabloon
> * Chatten met de bot om te controleren of dat de code werkt 
> * Uw gepubliceerde KB verbinden met de bot
> * Testen van de bot met een vraag

In dit artikel kunt u de gratis QnA Maker [service](../how-to/set-up-qnamaker-service-azure.md).

## <a name="prerequisites"></a>Vereisten

U moet een gepubliceerde knowledge base voor deze zelfstudie. Als u een hebt, volgt u de stappen in [maken van een kennisdatabase](../How-To/create-knowledge-base.md) een QnA Maker-service maken met vragen en antwoorden.

## <a name="create-a-qna-bot"></a>Maken van een QnA Bot

1. Selecteer in Azure Portal **Een resource maken**.

    ![bot-service maken](../media/qnamaker-tutorials-create-bot/bot-service-creation.png)

2. Zoek in het zoekvak **Web App-Bot**.

    ![bot-service selecteren](../media/qnamaker-tutorials-create-bot/bot-service-selection.png)

3. In **Bot Service** geeft u de vereiste informatie op:

    - Stel **appnaam** op de naam van uw bot. De naam wordt gebruikt als het subdomein wanneer uw bot wordt geïmplementeerd naar de cloud (bijvoorbeeld mynotesbot.azurewebsites.net).
    - Selecteer het abonnement, de resourcegroep, de App service-plan en de locatie.

4. Voor het gebruik van de v3-sjablonen, selecteert u de SDK-versie van **SDK v3** en de taal van de SDK van **C#** of **Node.js**.

    ![de instellingen van de bot-sdk](../media/qnamaker-tutorials-create-bot/bot-v3.png)

5. Selecteer de **vraag en antwoord** sjabloon voor het veld van de sjabloon Bot, en vervolgens slaat u de sjabloon door het selecteren van **Selecteer**.

    ![Sla de bot service-sjabloon selecteren](../media/qnamaker-tutorials-create-bot/bot-v3-template.png)

6. Controleer uw instellingen en selecteer vervolgens **maken**. Hiermee maakt en implementeert de bot-service met op Azure.

    ![bot maken](../media/qnamaker-tutorials-create-bot/bot-blade-settings-v3.png)

7. Bevestig dat de botservice is geïmplementeerd.

    - Selecteer **meldingen** (het belpictogram die zich aan de bovenkant van de Azure-portal). De melding wordt gewijzigd van **implementatie is gestart** naar **implementatie is voltooid**.
    - Nadat de melding wordt gewijzigd in **implementatie is voltooid**, selecteer **naar de resource gaan** op waarmee de melding.

## <a name="chat-with-the-bot"></a>Met de Bot chatten

Selecteren **naar de resource gaan** gaat u naar de resource van de bot.

Selecteer **Test in Web Chat** om de Web Chat-deelvenster te openen. Typ 'Hallo' Web Chat.

![QnA bot chatten](../media/qnamaker-tutorials-create-bot/qna-bot-web-chat.PNG)

De bot reageert met 'Stel QnAKnowledgebaseId en QnASubscriptionKey in Appinstellingen. Dit antwoord wordt bevestigd dat uw QnA Bot het bericht heeft ontvangen, maar er geen QnA Maker-kennisdatabase die zijn gekoppeld aan het nog is. 

## <a name="connect-your-qna-maker-knowledge-base-to-the-bot"></a>Uw QnA Maker knowledge base verbinden met de bot

1. Open **toepassingsinstellingen** en bewerk de **QnAKnowledgebaseId**, **QnAAuthKey**, en de **QnAEndpointHostName** velden bevatten de waarden van de QnA Maker knowledge base.

    ![App-instellingen](../media/qnamaker-tutorials-create-bot/application-settings.PNG)

1. Uw knowledge base-ID en host-url voor de eindpuntsleutel ophalen uit het tabblad instellingen van uw knowledge base in de portal voor QnA Maker.

    - Aanmelden bij [QnA Maker](https://qnamaker.ai)
    - Ga naar uw knowledge base
    - Selecteer de **instellingen** tabblad
    - **Publiceren** uw knowledge base, als dit nog niet gedaan

    ![QnA Maker-waarden](../media/qnamaker-tutorials-create-bot/qnamaker-settings-kbid-key.PNG)

> [!NOTE]
> Als u verbinding maken met de preview-versie van de knowledge base met de QnA bot wilt, stel de waarde van **Ocp-Apim-Subscription-Key** naar **QnAAuthKey**. Laat de **QnAEndpointHostName** leeg zijn.

## <a name="test-the-bot"></a>De bot testen

Selecteer in de Azure portal, **testen in Web Chat** voor het testen van de bot. 

![QnA Maker-bot](../media/qnamaker-tutorials-create-bot/qna-bot-web-chat-response.PNG)

Uw QnA Bot antwoorden uit uw knowledge base.

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u klaar bent met de bot van deze zelfstudie, verwijdert u de bot in Azure portal. De bot-services zijn onder andere:

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
