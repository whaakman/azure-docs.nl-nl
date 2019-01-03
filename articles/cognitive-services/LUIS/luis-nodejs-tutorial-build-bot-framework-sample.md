---
title: Bot - Node.js - v3
titleSuffix: Azure Cognitive Services
description: Bouw een bot die is geïntegreerd met een LUIS-toepassing met de Bot Framework 3.x en de Azure-Web-app-bot.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/24/2018
ms.author: diberry
ms.openlocfilehash: b64db46da99e0db3d573dac3830c77a0be4f4d0d
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2018
ms.locfileid: "53726164"
---
# <a name="luis-bot-in-nodejs-with-the-bot-framework-3x-and-the-azure-web-app-bot"></a>LUIS-bot in Node.js met Bot Framework 3.x en de Azure-Web-app-bot

Bouw met behulp van Node.js een chatbot met ingebouwd taalbegrip (LUIS). Deze chatbot maakt gebruik van het vooraf gedefinieerde HomeAutomation domein een bot-oplossing snel implementeren. De bot is gebouwd met Bot Framework 3.x en de Azure-Web-app-bot.

## <a name="prerequisite"></a>Vereiste

Voordat u de bot maakt, volg de stappen in [maken van een app](./luis-get-started-create-app.md) om de LUIS-app die wordt gebruikt te maken.

De bot reageert op intents van het domein HomeAutomation die zich in de LUIS-app. Voor elk van deze intents biedt de LUIS-app een doel dat is toegewezen aan deze. De bot biedt een dialoogvenster die verantwoordelijk is voor het doel dat LUIS detecteert.

| Intentie | Voorbeeld van een utterance | Bot-functionaliteit |
|:----:|:----------:|---|
| HomeAutomation.TurnOn | Schakel in het licht. | De bot roept de `TurnOnDialog` wanneer de `HomeAutomation.TurnOn` wordt gedetecteerd. Dit dialoogvenster is waar u een IoT-service om te schakelen op een apparaat en de gebruiker die het apparaat is ingeschakeld zien wilt aanroepen. |
| HomeAutomation.TurnOff | De verlichting slaapkamers uitschakelen. | De bot roept de `TurnOffDialog` wanneer de `HomeAutomation.TurnOff` wordt gedetecteerd. Dit dialoogvenster wanneer u een IoT-service waarmee een apparaat uitschakelen en de gebruiker die het apparaat is uitgeschakeld vertellen wilt aanroepen. |


## <a name="create-a-language-understanding-bot-with-bot-service"></a>Maken van een bot Language Understanding met Bot Service

1. In de [Azure-portal](https://portal.azure.com), selecteer **maken van nieuwe resource** in het menu-blade en selecteer **alle**.

    ![Zie alle resources in Azure portal](./media/luis-tutorial-node-bot/bot-service-creation.png)

2. Zoek in het zoekvak **Web App-Bot**. 

    ![Web app-bot om te beginnen met het aanmaakproces resource selecteren](./media/luis-tutorial-node-bot/bot-service-selection.png)

3. In de **Bot Service** blade, geef de vereiste gegevens op en selecteer **maken**. Hiermee maakt en implementeert de botservice en LUIS-app in Azure. Als u wilt gebruiken [spraak voorbereiden](https://docs.microsoft.com/bot-framework/bot-service-manage-speech-priming), Bekijk [regio vereisten](luis-resources-faq.md#what-luis-regions-support-bot-framework-speech-priming) voordat het maken van uw bot. 
    * Stel **appnaam** op de naam van uw bot. De naam wordt gebruikt als het subdomein wanneer uw bot wordt geïmplementeerd naar de cloud (bijvoorbeeld mynotesbot.azurewebsites.net). <!-- This name is also used as the name of the LUIS app associated with your bot. Copy it to use later, to find the LUIS app associated with the bot. -->
    * Selecteer het abonnement [resourcegroep](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview), App service-plan en [locatie](https://azure.microsoft.com/regions/).
    * Voor **Bot sjabloon**selecteert:
        * **SDK-v3**
        * **Node.js**
        * **Taalbegrip**
    * Selecteer de **LUIS-App locatie**. Dit is het ontwerpen van [regio] [ LUIS] in de app is gemaakt.
    * Selecteer het selectievakje bevestiging voor de juridische kennisgeving. De voorwaarden van de juridische kennisgeving staan hieronder het selectievakje in.

    ![Bot-Service-blade](./media/luis-tutorial-node-bot/bot-service-setting-callout-template.png)


4. Bevestig dat de botservice is geïmplementeerd.
    * Schakel meldingen (het belpictogram die zich aan de bovenkant van de Azure-portal). De melding wordt gewijzigd van **implementatie is gestart** naar **implementatie is voltooid**.
    * Nadat de melding wordt gewijzigd in **implementatie is voltooid**, selecteer **naar de resource gaan** op waarmee de melding.

## <a name="try-the-default-bot"></a>Probeer de standaard-bot

Bevestig dat de bot is geïmplementeerd door het controleren van de **meldingen**. De meldingen wordt gewijzigd van **implementatie wordt uitgevoerd...**  naar **implementatie is voltooid**. Selecteer **naar de resource gaan** knop om van de bot resources blade te openen.

<!-- this step isn't supposed to be necessary -->
## <a name="install-npm-resources"></a>Installeer de NPM-resources
NPM-pakketten installeren met de volgende stappen uit:

1. Selecteer **bouwen** uit de **Bot Management** sectie van de Web-App-Bot. 

2. Een nieuwe, tweede browser venster wordt geopend. Selecteer **online code-editor openen**.

3. Selecteer in de bovenste navigatiebalk de naam van de web-app-bot `homeautomationluisbot`. 

4. Selecteer in de vervolgkeuzelijst **Kudu-Console openen**.

5. Er wordt een nieuw browservenster geopend. Voer de volgende opdracht in de console:

    ```console
    cd site\wwwroot && npm install
    ```

    Wacht totdat het installatieproces te voltooien. Ga terug naar het eerste venster van de browser. 

## <a name="test-in-web-chat"></a>Testen in chatten
Zodra de bot is geregistreerd, selecteer **Test in Web Chat** om de Web Chat-deelvenster te openen. Typ "Hallo" in Web Chat.

  ![De bot in Web Chat testen](./media/luis-tutorial-node-bot/bot-service-web-chat.png)

De bot reageert door te spreken 'u hebt begroeting bereikt. U zegt: hello '. Hiermee bevestigt u dat de bot heeft ontvangen van uw bericht en doorgegeven aan een LUIS-app dat is gemaakt van de standaardinstelling. Deze standaardinstelling LUIS-app kunt u lezen wat een begroeting gedetecteerd. In de volgende stap maakt u verbinding met de bot de LUIS-app die u eerder hebt gemaakt in plaats van de standaard LUIS-app.

## <a name="connect-your-luis-app-to-the-bot"></a>Uw LUIS-app verbinden met de bot

Open **toepassingsinstellingen** in de eerste browservenster en bewerk de **LuisAppId** veld bevat de toepassings-ID van uw LUIS-app.

  ![Werk de LUIS-app-ID in Azure](./media/luis-tutorial-node-bot/bot-service-app-id.png)

Als u niet de LUIS-app-ID hebt, meld u aan bij de [LUIS](luis-reference-regions.md) website met hetzelfde account als u zich aanmeldt bij Azure. Selecteer op **mijn apps**. 

1. De LUIS-app die u eerder hebt gemaakt, waarin de intenties en entiteiten uit het domein HomeAutomation vinden.

2. In de **instellingen** pagina voor de app LUIS, zoeken en kopiëren van de app-ID.

3. Als u de app nog niet hebt getraind, selecteert u de **trainen** knop in de rechterbovenhoek om te trainen van uw app.

4. Als u de app nog niet hebt gepubliceerd, selecteert u **publiceren** in de bovenste navigatiebalk om te openen de **publiceren** pagina. Selecteer de slot Production en vervolgens de knop **Publish**.

## <a name="modify-the-bot-code"></a>De bot-code wijzigen

Ga naar het tweede browservenster als deze nog steeds openen of Selecteer in het eerste browservenster **bouwen** en selecteer vervolgens **online code-editor openen**.

   ![Online code-editor openen](./media/luis-tutorial-node-bot/bot-service-build.png)

Open in de code-editor `app.js`. Het bevat de volgende code:

```nodejs
/*-----------------------------------------------------------------------------
A simple Language Understanding (LUIS) bot for the Microsoft Bot Framework. 
-----------------------------------------------------------------------------*/

var restify = require('restify');
var builder = require('botbuilder');
var botbuilder_azure = require("botbuilder-azure");

// Setup Restify Server
var server = restify.createServer();
server.listen(process.env.port || process.env.PORT || 3978, function () {
   console.log('%s listening to %s', server.name, server.url); 
});
  
// Create chat connector for communicating with the Bot Framework Service
var connector = new builder.ChatConnector({
    appId: process.env.MicrosoftAppId,
    appPassword: process.env.MicrosoftAppPassword,
    openIdMetadata: process.env.BotOpenIdMetadata 
});

// Listen for messages from users 
server.post('/api/messages', connector.listen());

/*----------------------------------------------------------------------------------------
* Bot Storage: This is a great spot to register the private state storage for your bot. 
* We provide adapters for Azure Table, CosmosDb, SQL Azure, or you can implement your own!
* For samples and documentation, see: https://github.com/Microsoft/BotBuilder-Azure
* ---------------------------------------------------------------------------------------- */

var tableName = 'botdata';
var azureTableClient = new botbuilder_azure.AzureTableClient(tableName, process.env['AzureWebJobsStorage']);
var tableStorage = new botbuilder_azure.AzureBotStorage({ gzipData: false }, azureTableClient);

// Create your bot with a function to receive messages from the user
// This default message handler is invoked if the user's utterance doesn't
// match any intents handled by other dialogs.
var bot = new builder.UniversalBot(connector, function (session, args) {
    session.send('You reached the default message handler. You said \'%s\'.', session.message.text);
});

bot.set('storage', tableStorage);

// Make sure you add code to validate these fields
var luisAppId = process.env.LuisAppId;
var luisAPIKey = process.env.LuisAPIKey;
var luisAPIHostName = process.env.LuisAPIHostName || 'westus.api.cognitive.microsoft.com';

const LuisModelUrl = 'https://' + luisAPIHostName + '/luis/v2.0/apps/' + luisAppId + '?subscription-key=' + luisAPIKey;

// Create a recognizer that gets intents from LUIS, and add it to the bot
var recognizer = new builder.LuisRecognizer(LuisModelUrl);
bot.recognizer(recognizer);

// Add a dialog for each intent that the LUIS app recognizes.
// See https://docs.microsoft.com/bot-framework/nodejs/bot-builder-nodejs-recognize-intent-luis 
bot.dialog('GreetingDialog',
    (session) => {
        session.send('You reached the Greeting intent. You said \'%s\'.', session.message.text);
        session.endDialog();
    }
).triggerAction({
    matches: 'Greeting'
})

bot.dialog('HelpDialog',
    (session) => {
        session.send('You reached the Help intent. You said \'%s\'.', session.message.text);
        session.endDialog();
    }
).triggerAction({
    matches: 'Help'
})

bot.dialog('CancelDialog',
    (session) => {
        session.send('You reached the Cancel intent. You said \'%s\'.', session.message.text);
        session.endDialog();
    }
).triggerAction({
    matches: 'Cancel'
})
```

De bestaande intenties in het app.js worden genegeerd. U kunt ze laten. 

## <a name="add-a-dialog-that-matches-homeautomationturnon"></a>Een dialoogvenster die overeenkomt met HomeAutomation.TurnOn toevoegen

Kopieer de volgende code en toe te voegen aan `app.js`.

```nodejs
bot.dialog('TurnOn',
    (session) => {
        session.send('You reached the TurnOn intent. You said \'%s\'.', session.message.text);
        session.endDialog();
    }
).triggerAction({
    matches: 'HomeAutomation.TurnOn'
})
```

De [komt overeen met] [ matches] kiezen op de [triggerAction] [ triggerAction] die is gekoppeld aan het dialoogvenster geeft de naam van het doel. De herkenning wordt uitgevoerd telkens wanneer de bot een utterance van de gebruiker ontvangt. Als het hoogste score doel dat wordt gedetecteerd, overeenkomt met een `triggerAction` gebonden aan een dialoogvenster, roept de bot dialoogvenster.

## <a name="add-a-dialog-that-matches-homeautomationturnoff"></a>Een dialoogvenster die overeenkomt met HomeAutomation.TurnOff toevoegen

Kopieer de volgende code en toe te voegen aan `app.js`.

```nodejs
bot.dialog('TurnOff',
    (session) => {
        session.send('You reached the TurnOff intent. You said \'%s\'.', session.message.text);
        session.endDialog();
    }
).triggerAction({
    matches: 'HomeAutomation.TurnOff'
})
```
## <a name="test-the-bot"></a>De bot testen

Selecteer in de Azure-Portal op **testen in Web Chat** voor het testen van de bot. Probeer het type berichten like "inschakelen het licht aan" en 'Mijn brandstof uitschakelen' om aan te roepen van de intenties die u hebt toegevoegd aan.
   ![HomeAutomation bot in Web Chat testen](./media/luis-tutorial-node-bot/bot-service-chat-results.png)

> [!TIP]
> Als u merkt dat uw bot niet altijd wordt herkend door de juiste bedoeling of entiteiten, moet u uw LUIS-app-prestaties verbeteren door meer voorbeeld uitingen service trainen wordt gegeven. U kunt uw LUIS-app zonder wijzigingen in de code van uw bot opnieuw trainen. Zie [voorbeeld utterances toevoegen](https://docs.microsoft.com/azure/cognitive-services/LUIS/add-example-utterances) en [trainen en testen van uw LUIS-app](https://docs.microsoft.com/azure/cognitive-services/LUIS/luis-interactive-test).

## <a name="learn-more-about-bot-framework"></a>Meer informatie over Bot Framework
Meer informatie over [Bot Framework](https://dev.botframework.com/) en de [3.x](https://github.com/Microsoft/BotBuilder) en [4.x](https://github.com/Microsoft/botbuilder-js) SDK's.

## <a name="next-steps"></a>Volgende stappen

<!-- From trying the bot, you can see that the recognizer can trigger interruption of the currently active dialog. Allowing and handling interruptions is a flexible design that accounts for what users really do. Learn more about the various actions you can associate with a recognized intent.--> U kunt andere intents, zoals Help, annuleren en begroeting, toevoegen aan de LUIS-app. Vervolgens dialoogvensters voor de nieuwe intents toevoegen en testen met behulp van de bot. 

<!-- 
> [!NOTE] 
> The default LUIS app that the template created contains example utterances for Cancel, Greeting, and Help intents. In the list of apps, find the app that begins with the name specified in **App name** in the **Bot Service** blade when you created the Bot Service. -->

> [!div class="nextstepaction"]
> [Intents toevoegen](./luis-how-to-add-intents.md)
> [spraak voorbereiden](https://docs.microsoft.com/bot-framework/bot-service-manage-speech-priming)


[triggerAction]: https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.dialog.html#triggeraction

[matches]: https://docs.botframework.com/en-us/node/builder/chat-reference/interfaces/_botbuilder_d_.itriggeractionoptions.html#matches


[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions

