---
title: Een bot QnA maken met Azure Bot Service - cognitieve Azure-Services | Microsoft Docs
services: cognitive-services
author: nstulasi
manager: sangitap
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 05/07/2018
ms.author: saneppal
ms.openlocfilehash: fc430bf3aa7cad279d7a93bb6892aa19abee3378
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37109266"
---
# <a name="create-a-qna-bot-with-azure-bot-service"></a>Een QnA Bot maken met Azure Bot-Service
Deze zelfstudie wordt u begeleid bouwen van een bot QnA met Azure Bot-service op de Azure-portal.

## <a name="prerequisite"></a>Vereiste
Voordat u bouwt, volg de stappen in [maken van een knowledge base](../How-To/create-knowledge-base.md) een QnA Maker-service maken met vragen en antwoorden.

De bot reageert op vragen van de kennisdatabase die u hebt gemaakt, via de QnAMakerDialog.

## <a name="create-a-qna-bot"></a>Maken van een QnA Bot
1. In de [Azure-portal](https://portal.azure.com), selecteer **maken** nieuwe resource in de blade menu en selecteer vervolgens **alle**.

    ![maken van de service bot](../media/qnamaker-tutorials-create-bot/bot-service-creation.png)

2. Zoek in het zoekvak op **Web App Bot**.

    ![selectie van bot-service](../media/qnamaker-tutorials-create-bot/bot-service-selection.png)

3. In de **blade Bot Service**, geef de vereiste gegevens en selecteert u **maken**. Dit maakt en implementeert de service bot met QnAMakerDialog naar Azure.

    - Stel **appnaam** aan uw bot-naam. De naam wordt gebruikt als het subdomein wanneer uw bot wordt geïmplementeerd naar de cloud (bijvoorbeeld mynotesbot.azurewebsites.net).
    - Selecteer het abonnement, resourcegroep, App service-abonnement en locatie.
    - Selecteer de **vraag en antwoord** (Node.js of C#)-sjabloon voor het veld Bot-sjabloon.
    - Schakel het selectievakje bevestiging voor de juridische mededeling. De voorwaarden van de juridische kennisgeving zijn lager dan het selectievakje in.

        ![selectie van bot-service](../media/qnamaker-tutorials-create-bot/bot-service-qna-template.PNG)

4. Bevestig dat de bot-service is geïmplementeerd.

    - Selecteer **meldingen** (het belpictogram bevindt zich aan de bovenkant van de Azure portal). De melding wordt gewijzigd van **implementatie gestart** naar **implementatie is voltooid**.
    - Nadat de melding wijzigingen in **implementatie is voltooid**, selecteer **gaat u naar de resource** op deze melding.

## <a name="chat-with-the-bot"></a>Chatten met de Bot
Selecteren **gaat u naar de resource** gaat u naar de resourceblade van de bot.

Zodra de bot is geregistreerd, klikt u op **testen in de Web chatten** om het deelvenster Web chatten te openen. Typ in het Web chatten "hello".

![QnA bot chatten](../media/qnamaker-tutorials-create-bot/qna-bot-web-chat.PNG)

De bot reageert met 'Stel QnAKnowledgebaseId en QnASubscriptionKey in App-instellingen. Lees hoe u ze op https://aka.ms/qnaabssetup'. Dit antwoord wordt bevestigd dat uw QnA Bot het bericht heeft ontvangen, maar er geen QnA Maker kennisdatabase gekoppeld nog is. Dit wordt in de volgende stap.

## <a name="connect-your-qna-maker-knowledge-base-to-the-bot"></a>Verbinding maken met uw kennisdatabase QnA Maker van de bot

1. Open **toepassingsinstellingen** en bewerkt u de **QnAKnowledgebaseId**, **QnAAuthKey**, en de **QnAEndpointHostName** velden bevat de waarden van de QnA Maker knowledge base.

    ![App-instellingen](../media/qnamaker-tutorials-create-bot/application-settings.PNG)

2. Uw knowledge base-ID en host-url voor de eindpuntsleutel ophalen uit het tabblad instellingen van de knowledge base in https://qnamaker.ai.
    - Meld u aan bij [QnA Maker](https://qnamaker.ai)
    - Ga naar de knowledge base
    - Klik op de **instellingen** tabblad
    - **Publiceren** uw kennisdatabase als niet hebt gedaan

    ![QnA Maker waarden](../media/qnamaker-tutorials-create-bot/qnamaker-settings-kbid-key.PNG)

> [!NOTE]
> Als u verbinding maken met de preview-versie van het knowledge base met de bot QnA wilt, stel de waarde van **Ocp-Apim-Subscription-Key** naar **QnAAuthKey**. Laat de **QnAEndpointHostName** leeg.

## <a name="test-the-bot"></a>De bot testen
Klik in de Azure-portal op **testen in Web chatten** voor het testen van de bot. 

![QnA Maker bot](../media/qnamaker-tutorials-create-bot/qna-bot-web-chat-response.PNG)

Uw QnA Bot beantwoordt nu van uw knowledge base.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Integreer QnA Maker en LUIS](./integrate-qnamaker-luis.md)

## <a name="see-also"></a>Zie ook

- [Uw knowledge base beheren](https://qnamaker.ai)
- [Uw bot in verschillende kanalen inschakelen](https://docs.microsoft.com/azure/bot-service/bot-service-manage-channels)
