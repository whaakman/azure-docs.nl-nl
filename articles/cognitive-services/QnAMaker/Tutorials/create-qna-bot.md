---
title: QnA bot met Azure Bot Service - QnA Maker
titleSuffix: Azure Cognitive Services
description: In deze zelfstudie helpt u bij het bouwen van een QnA bot met Azure Bot service v3 in Azure portal.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 09/12/2018
ms.author: tulasim
ms.openlocfilehash: 30400b04ec08d936242b022f10cf1485e009e6d2
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/23/2018
ms.locfileid: "49647320"
---
# <a name="create-a-qna-bot-with-azure-bot-service-v3"></a>Een QnA Bot maken met Azure Bot Service v3
In deze zelfstudie helpt u bij het bouwen van een QnA bot met Azure Bot service v3 in Azure portal.

## <a name="prerequisite"></a>Vereiste
Voordat u bouwt, volg de stappen in [maken van een kennisdatabase](../How-To/create-knowledge-base.md) een QnA Maker-service maken met vragen en antwoorden.

De bot reageert op vragen van de knowledge base die u hebt gemaakt, via de QnAMakerDialog.

## <a name="create-a-qna-bot"></a>Maken van een QnA Bot
1. In de [Azure-portal](https://portal.azure.com), selecteer **maken** nieuwe resource in het menu-blade en selecteer vervolgens **alle**.

    ![bot-service maken](../media/qnamaker-tutorials-create-bot/bot-service-creation.png)

2. Zoek in het zoekvak **Web App-Bot**.

    ![bot-service selecteren](../media/qnamaker-tutorials-create-bot/bot-service-selection.png)

3. In de **Bot Service-blade**, geef de vereiste gegevens:

    - Stel **appnaam** op de naam van uw bot. De naam wordt gebruikt als het subdomein wanneer uw bot wordt geïmplementeerd naar de cloud (bijvoorbeeld mynotesbot.azurewebsites.net).
    - Selecteer het abonnement, de resourcegroep, de App service-plan en de locatie.

4. Zie de instructies voor het maken van een bot QnA met SDK-v4-Zie [QnA v4-bot sjabloon](https://aka.ms/qna-bot-v4). Voor het gebruik van de v3-sjablonen, selecteert u de SDK-versie van **SDK v3** en de taal van de SDK van **C#** of **Node.js**.

    ![de instellingen van de bot-sdk](../media/qnamaker-tutorials-create-bot/bot-v3.png)

5. Selecteer de **vraag en antwoord** sjabloon voor het veld van de sjabloon Bot, en vervolgens slaat u de sjabloon door het selecteren van **Selecteer**.

    ![bot-service selecteren](../media/qnamaker-tutorials-create-bot/bot-v3-template.png)

6. Controleer uw instellingen en selecteer vervolgens **maken**. Hiermee maakt en implementeert de bot-service met QnAMakerDialog naar Azure.

    ![bot-service selecteren](../media/qnamaker-tutorials-create-bot/bot-blade-settings-v3.png)

7. Bevestig dat de botservice is geïmplementeerd.

    - Selecteer **meldingen** (het belpictogram die zich aan de bovenkant van de Azure-portal). De melding wordt gewijzigd van **implementatie is gestart** naar **implementatie is voltooid**.
    - Nadat de melding wordt gewijzigd in **implementatie is voltooid**, selecteer **naar de resource gaan** op waarmee de melding.

## <a name="chat-with-the-bot"></a>Met de Bot chatten
Selecteren **naar de resource gaan** gaat u naar de resourceblade van de bot.

Nadat de bot is geregistreerd, klikt u op **Test in Web Chat** om de Web Chat-deelvenster te openen. Typ "Hallo" in Web Chat.

![QnA bot chatten](../media/qnamaker-tutorials-create-bot/qna-bot-web-chat.PNG)

De bot reageert met 'Stel QnAKnowledgebaseId en QnASubscriptionKey in Appinstellingen. Leer hoe u ze op https://aka.ms/qnaabssetup'. Dit antwoord wordt bevestigd dat uw QnA Bot het bericht heeft ontvangen, maar er geen QnA Maker-kennisdatabase die zijn gekoppeld aan het nog is. Dat doen in de volgende stap.

## <a name="connect-your-qna-maker-knowledge-base-to-the-bot"></a>Uw QnA Maker knowledge base verbinden met de bot

1. Open **toepassingsinstellingen** en bewerk de **QnAKnowledgebaseId**, **QnAAuthKey**, en de **QnAEndpointHostName** velden bevatten de waarden van de QnA Maker knowledge base.

    ![App-instellingen](../media/qnamaker-tutorials-create-bot/application-settings.PNG)

2. Uw knowledge base-ID en host-url voor de eindpuntsleutel ophalen uit het tabblad instellingen van uw knowledge base in https://qnamaker.ai.
    - Aanmelden bij [QnA Maker](https://qnamaker.ai)
    - Ga naar uw knowledge base
    - Klik op de **instellingen** tabblad
    - **Publiceren** uw knowledge base, als dit nog niet gedaan

    ![QnA Maker-waarden](../media/qnamaker-tutorials-create-bot/qnamaker-settings-kbid-key.PNG)

> [!NOTE]
> Als u verbinding maken met de preview-versie van de knowledge base met de QnA bot wilt, stel de waarde van **Ocp-Apim-Subscription-Key** naar **QnAAuthKey**. Laat de **QnAEndpointHostName** leeg zijn.

## <a name="test-the-bot"></a>De bot testen
Klik in de Azure-portal op **testen in Web Chat** voor het testen van de bot. 

![QnA Maker-bot](../media/qnamaker-tutorials-create-bot/qna-bot-web-chat-response.PNG)

Uw QnA Bot is nu antwoorden op uit uw knowledge base.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [QnA Maker en LUIS integreren](./integrate-qnamaker-luis.md)

## <a name="see-also"></a>Zie ook

- [Beheren van uw knowledge base](https://qnamaker.ai)
- [Uw bot in verschillende kanalen](https://docs.microsoft.com/azure/bot-service/bot-service-manage-channels)
