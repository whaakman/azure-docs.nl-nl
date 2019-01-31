---
title: Bot - Node.js - v4-processors
titleSuffix: Azure Cognitive Services
description: Bouw met behulp van Node.js een chatbot met ingebouwd taalbegrip (LUIS). Deze chatbot maakt gebruik van de app Human Resources om snel een botoplossing te implementeren. De bot is gebouwd met Bot Framework versie 4 en de Azure-web-app-bot.
services: cognitive-services
author: diberry
ms.custom: seodec18
manager: cjgronlund
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: tutorial
ms.date: 12/07/2018
ms.author: diberry
ms.openlocfilehash: 292bcf5974f8c51f99a676786c66316e9cde0748
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55215900"
---
# <a name="tutorial-luis-bot-in-nodejs-with-the-bot-framework-4x-and-the-azure-web-app-bot"></a>Zelfstudie: LUIS-bot in Node.js met Bot Framework 4.x en de Azure-web-app-bot
Met Node.js kunt u een chatbot bouwen met ingebouwd taalbegrip (LUIS). Deze bot maakt gebruik van de app HomeAutomation om een botoplossing te implementeren. De bot is gebouwd aan de hand van de Azure-[web-app-bot](https://docs.microsoft.com/azure/bot-service/) en [Bot Framework versie](https://github.com/Microsoft/botbuilder-js) 4.

**In deze zelfstudie leert u het volgende:**

> [!div class="checklist"]
> * Een web-app-bot maken. Bij dit proces wordt een nieuwe LUIS-app gemaakt.
> * Een vooraf gemaakt domein toevoegen aan het nieuwe LUIS-model
> * Het project downloaden dat is gemaakt door de webbotservice
> * De bot en emulator lokaal op uw computer starten
> * De botcode wijzigen voor nieuwe LUIS-intenties
> * De uitingsresultaten in de bot bekijken

## <a name="prerequisites"></a>Vereisten

* [Botemulator](https://aka.ms/abs/build/emulatordownload)
* [Visual Studio Code](https://code.visualstudio.com/Download)


## <a name="create-web-app-bot"></a>Een web-app-bot maken

1. Selecteer in de [Azure-portal](https://portal.azure.com) de optie **Nieuwe resource maken**.

2. Zoek via het zoekvak naar **Web-app-bot** en selecteer deze optie. Selecteer **Maken**.

3. In **Bot Service** geeft u de vereiste informatie op:

    |Instelling|Doel|Aanbevolen instelling|
    |--|--|--|
    |Botnaam|Resourcenaam|`luis-nodejs-bot-` + `<your-name>`, bijvoorbeeld, `luis-nodejs-bot-johnsmith`|
    |Abonnement|Het abonnement waarvoor de bot moet worden gemaakt.|Uw primaire abonnement.
    |Resourcegroep|Logische groep van Azure-resources|Maak een nieuwe groep voor het opslaan van alle resources die worden gebruikt met deze bot. Geef de groep de naam `luis-nodejs-bot-resource-group`.|
    |Locatie|Azure-regio: deze hoeft niet dezelfde te zijn als de LUIS-regio voor maken en publiceren.|`westus`|
    |Prijscategorie|Wordt gebruikt voor serviceaanvraaglimieten en facturatie.|`F0` is de gratis laag.
    |Naam van app|Deze naam wordt gebruikt als subdomein wanneer uw bot wordt geïmplementeerd in de cloud (bijvoorbeeld humanresourcesbot.azurewebsites.net).|`luis-nodejs-bot-` + `<your-name>`, bijvoorbeeld, `luis-nodejs-bot-johnsmith`|
    |Botsjabloon|Instellingen voor het botframework - zie de volgende tabel|
    |Locatie van LUIS-app|Moet hetzelfde zijn als de regio waarin de LUIS-resource zich bevindt|`westus`|

4. In de **botsjablooninstellingen** selecteert u het volgende en kiest u onder deze instellingen de knop **Selecteren**:

    |Instelling|Doel|Selectie|
    |--|--|--|
    |SDK-versie|Versie van het botframework|**SDK v4**|
    |SDK-taal|Computertaal van de bot|**Node.js**|
    |Echo-/basisbot|Type bot|**Basisbot**|
    
5. Selecteer **Maken**. Hiermee maakt u de botservice en implementeert u deze in Azure. Bij dit proces wordt een LUIS-app gemaakt met de naam `luis-nodejs-bot-XXXX`. Deze naam is gebaseerd op de bot en de app-naam uit het vorige gedeelte.

    [ ![Een web-app-bot maken](./media/bfv4-nodejs/create-web-app-service.png) ](./media/bfv4-nodejs/create-web-app-service.png#lightbox)

6. Laat dit browsertabblad open. Voor alle stappen die u in de LUIS-portal moet uitvoeren, opent u een nieuw tabblad in de browser. Ga door naar het volgende gedeelte zodra de nieuwe botservice is geïmplementeerd.

## <a name="add-prebuilt-domain-to-model"></a>Een vooraf gemaakt domein toevoegen aan een model
Als onderdeel van de botservice-implementatie wordt er een nieuwe LUIS-app gemaakt met intenties en voorbeelduitingen. Met de bot kunnen de volgende intenties worden toegewezen aan de nieuwe LUIS-app: 

|LUIS-intenties van de basisbot|voorbeelduiting|
|--|--|
|Annuleren|`stop`|
|Begroeting|`hello`|
|Help|`help`|
|Geen|Alles buiten het domein van de app.|

Voeg de vooraf gemaakte HomeAutomation-app toe aan het model om uitingen als de volgende te kunnen verwerken: `Turn off the living room lights`

1. Ga naar de [LUIS](https://www.luis.ai)-portal en meld u aan.
2. Op de pagina **Mijn apps** selecteert u de kolom **Gemaakt op** om te sorteren op de datum waarop de app is gemaakt. Azure Bot Service heeft in het vorige gedeelte een nieuwe app gemaakt. De naam ervan is `luis-nodejs-bot-` + `<your-name>` plus vier willekeurige tekens.
3. Open de app en selecteer het gedeelte **Bouwen** in de bovenste navigatiebalk.
4. Selecteer **Vooraf gemaakte domeinen** in het navigatiedeelvenster links.
5. Selecteer het domein **HomeAutomation** door op de kaart **Domein toevoegen** te selecteren.
6. Selecteer **Trainen** in het menu rechtsboven.
7. Selecteer **Publiceren** in het menu rechtsboven. 

    De app die is gemaakt door Azure Bot Service, heeft nu nieuwe intenties:

    |Nieuwe basisbotintenties|voorbeelduiting|
    |--|--|
    |HomeAutomation.TurnOn|`turn the fan to high`
    |HomeAutomation.TurnOff|`turn off ac please`|

## <a name="download-the-web-app-bot"></a>De web-app-bot downloaden 
Als u de code van de web-app-bot verder wilt ontwikkelen, downloadt u de code en gebruikt u deze op uw lokale computer. 

1. In de Azure-portal, in de resource van de web-app-bot, selecteert u de **toepassingsinstellingen** en kopieert u de waarden van **botFilePath** en **botFileSecret**. U moet deze later toevoegen aan een omgevingsbestand. 

2. In de Azure-portal selecteert u **Bouwen** in het gedeelte **Botbeheer**. 

3. Selecteer **Broncode bot downloaden**. 

    [ ![Download de broncode van de web-app-bot voor de basisbot](../../../includes/media/cognitive-services-luis/bfv4/download-code.png) ](../../../includes/media/cognitive-services-luis/bfv4/download-code.png#lightbox)

4. Als de broncode is ingepakt, ziet u een bericht met een link voor het downloaden van de code. Selecteer de link. 

5. Sla het zip-bestand op uw lokale computer op en pak de bestanden uit. Open het project. 

6. Open het bestand bot.js en zoek naar `const results = await this.luisRecognizer.recognize(context);`. Hier wordt de uiting die de gebruiker heeft ingevoerd in de bot naar LUIS verzonden.

    ```nodejs
    /**
     * Driver code that does one of the following:
     * 1. Display a welcome card upon startup
     * 2. Use LUIS to recognize intents
     * 3. Start a greeting dialog
     * 4. Optionally handle Cancel or Help interruptions
     *
     * @param {Context} context turn context from the adapter
     */
    async onTurn(context) {
        // Create a dialog context
        const dc = await this.dialogs.createContext(context);

        if(context.activity.type === ActivityTypes.Message) {
            // Perform a call to LUIS to retrieve results for the current activity message.
            const results = await this.luisRecognizer.recognize(context);
            
            const topIntent = LuisRecognizer.topIntent(results);

            // handle conversation interrupts first
            const interrupted = await this.isTurnInterrupted(dc, results);
            if(interrupted) {
                return;
            }

            // Continue the current dialog
            const dialogResult = await dc.continue();

            switch(dialogResult.status) {
                case DialogTurnStatus.empty:
                    switch (topIntent) {
                        case GREETING_INTENT:
                            await dc.begin(GREETING_DIALOG);
                            break;

                        case NONE_INTENT:
                        default:
                            // help or no intent identified, either way, let's provide some help
                            // to the user
                            await dc.context.sendActivity(`I didn't understand what you just said to me. topIntent ${topIntent}`);
                            break;
                    }

                case DialogTurnStatus.waiting:
                    // The active dialog is waiting for a response from the user, so do nothing
                break;

                case DialogTurnStatus.complete:
                    await dc.end();
                    break;

                default:
                    await dc.cancelAll();
                    break;

            }

        } else if (context.activity.type === 'conversationUpdate' && context.activity.membersAdded[0].name === 'Bot') {
            // When activity type is "conversationUpdate" and the member joining the conversation is the bot
            // we will send our Welcome Adaptive Card.  This will only be sent once, when the Bot joins conversation
            // To learn more about Adaptive Cards, see https://aka.ms/msbot-adaptivecards for more details.
            const welcomeCard = CardFactory.adaptiveCard(WelcomeCard);
            await context.sendActivity({ attachments: [welcomeCard] });
        }
    }
    ```

    De bot verzendt de uiting van de gebruiker naar LUIS en haalt de resultaten op. De belangrijkste intentie bepaalt hoe de conversatie verder verloopt. 


## <a name="start-the-bot"></a>De bot starten
Voordat u code of instellingen wijzigt, controleert u of de bot werkt. 

1. Open in Visual Studio Code een terminalvenster. 

2. Installeer de npm-afhankelijkheden voor deze bot. 

    ```bash
    npm install
    ```
3. Maak een bestand voor het opslaan van de omgevingsvariabelen waar de botcode naar op zoek gaat. Noem het bestand `.env`. Voeg de volgende omgevingsvariabelen toe:

    <!--there is no code language that represents an .env file correctly-->
    ```env
    botFilePath=
    botFileSecret=
    ```

    Stel de waarden van de omgevingsvariabelen in op de waarden die u hebt gekopieerd uit de toepassingsinstellingen van Azure Bot Service. Dit is gebeurd in stap 1 van het gedeelte **[De web-app-bot downloaden](#download-the-web-app-bot)**.

4. Start de bot in de controlemodus. Als u na het opstarten nog wijzigingen aanbrengt aan de code, wordt de app automatisch opnieuw opgestart.

    ```bash
    npm run watch
    ```

5. Wanneer de bot wordt opgestart, wordt in het terminalvenster de lokale poort weergegeven die voor de bot wordt gebruikt:

    ```console
    > basic-bot@0.1.0 start C:\Users\pattiowens\repos\BFv4\luis-nodejs-bot-src
    > node ./index.js NODE_ENV=development

    restify listening to http://[::]:3978
    
    Get the Emulator: https://aka.ms/botframework-emulator
    
    To talk to your bot, open the luis-nodejs-bot-pattiowens.bot file in the Emulator
    ```

## <a name="start-the-emulator"></a>De emulator starten

1. Open de botemulator. 

2. In de botemulator selecteert u het *.bot-bestand in de hoofdmap van het project. Het `.bot`-bestand bevat het URL-eindpunt die de bot gebruikt voor berichten:

    [ ![Botemulator versie 4](../../../includes/media/cognitive-services-luis/bfv4/bot-emulator-v4.png) ](../../../includes/media/cognitive-services-luis/bfv4/bot-emulator-v4.png#lightbox)

3. Voer het botgeheim in dat u hebt gekopieerd uit de toepassingsinstellingen van Azure Bot Service. Dit is gebeurd in stap 1 van het gedeelte **[De web-app-bot downloaden](#download-the-web-app-bot)**. Hiermee krijgt de emulator toegang tot alle versleutelde velden in het .bot-bestand.

    ![Geheim in botemulator versie 4](../../../includes/media/cognitive-services-luis/bfv4/bot-secret.png)


4. Voer `Hello` in de botemulator in en haal het juiste antwoord op bij de basisbot.

    [ ![Antwoord van de basisbot in de emulator](../../../includes/media/cognitive-services-luis/bfv4/emulator-test.png) ](../../../includes/media/cognitive-services-luis/bfv4/emulator-test.png#lightbox)

## <a name="modify-bot-code"></a>Botcode wijzigen 

In het bestand `bot.js` voegt u code toe om de nieuwe intenties te verwerken. 

1. Boven in het bestand gaat u naar het gedeelte **Ondersteunde LUIS-intenties** en voegt u constanten toe voor de HomeAutomation-intenties:

    ```nodejs
    // Supported LUIS Intents
    const GREETING_INTENT = 'Greeting';
    const CANCEL_INTENT = 'Cancel';
    const HELP_INTENT = 'Help';
    const NONE_INTENT = 'None';
    const TURNON_INTENT = 'HomeAutomation_TurnOn'; // new intent
    const TURNOFF_INTENT = 'HomeAutomation_TurnOff'; // new intent
    ```

    De punt (`.`) tussen het domein en de intentie uit de LUIS-portal-app is vervangen door een (`_`). 

2. Zoek **isTurnInterrupted** - dit ontvangt de LUIS-voorspelling van de uiting en voegt dan een regel toe om het resultaat in de console te tonen.

    ```nodejs
    /**
     * Look at the LUIS results and determine if we need to handle
     * an interruptions due to a Help or Cancel intent
     *
     * @param {DialogContext} dc - dialog context
     * @param {LuisResults} luisResults - LUIS recognizer results
     */
    async isTurnInterrupted(dc, luisResults) {
        console.log(JSON.stringify(luisResults));
    ...
    ```

    De bot heeft niet precies hetzelfde antwoord als een LUIS REST API-aanvraag. Het is dus belangrijk om de verschillen te leren kennen door naar de JSON-reactie te kijken. De tekst- en intentie-eigenschappen zijn hetzelfde, maar de eigenschapswaarden van de entiteiten zijn gewijzigd. 

    ```json
    {
        "$instance": {
            "HomeAutomation_Device": [
                {
                    "startIndex": 23,
                    "endIndex": 29,
                    "score": 0.9776345,
                    "text": "lights",
                    "type": "HomeAutomation.Device"
                }
            ],
            "HomeAutomation_Room": [
                {
                    "startIndex": 12,
                    "endIndex": 22,
                    "score": 0.9079433,
                    "text": "livingroom",
                    "type": "HomeAutomation.Room"
                }
            ]
        },
        "HomeAutomation_Device": [
            "lights"
        ],
        "HomeAutomation_Room": [
            "livingroom"
        ]
    }
    ```

3. De intenties toevoegen aan de schakelinstructie van de onTurn-methode voor de case `DialogTurnStatus.empty`:

    ```nodejs
    switch (topIntent) {
        case GREETING_INTENT:
            await dc.begin(GREETING_DIALOG);
            break;

        // New HomeAutomation.TurnOn intent
        case TURNON_INTENT: 

            await dc.context.sendActivity(`TurnOn intent found, entities included: ${JSON.stringify(results.entities)}`);
            break;

        // New HomeAutomation.TurnOff intent
        case TURNOFF_INTENT: 

            await dc.context.sendActivity(`TurnOff intent found, entities included: ${JSON.stringify(results.entities)}`);
            break;

        case NONE_INTENT:
        default:
            // help or no intent identified, either way, let's provide some help
            // to the user
            await dc.context.sendActivity(`I didn't understand what you just said to me. topIntent ${topIntent}`);
            break;
    }
    ```

## <a name="view-results-in-bot"></a>Resultaten in de bot bekijken

1. Voer in de botemulator de volgende uiting in: `Turn on the livingroom lights to 50%`

2. De bot reageert met:

    ```json
    TurnOn intent found, entities included: {"$instance":{“HomeAutomation_Device”:[{“startIndex”:23,“endIndex”:29,“score”:0.9776345,“text”:“lights”,“type”:“HomeAutomation.Device”}],“HomeAutomation_Room”:[{“startIndex”:12,“endIndex”:22,“score”:0.9079433,“text”:“livingroom”,“type”:“HomeAutomation.Room”}]},“HomeAutomation_Device”:[“lights”],“HomeAutomation_Room”:[“livingroom”]}
    ```

## <a name="learn-more-about-bot-framework"></a>Meer informatie over Bot Framework
Azure Bot Service maakt gebruik van de Bot Framework-SDK. Meer informatie over de SDK en het botframework:

* [Azure Bot Service](https://docs.microsoft.com/azure/bot-service/bot-service-overview-introduction?view=azure-bot-service-4.0) v4-documentatie
* [Bot Builder-voorbeelden](https://github.com/Microsoft/botbuilder-samples)
* [Bot Builder-SDK](https://docs.microsoft.com/javascript/api/botbuilder-core/?view=botbuilder-ts-latest)
* [Bot Builder-hulpprogramma's](https://github.com/Microsoft/botbuilder-tools):

## <a name="next-steps"></a>Volgende stappen

U hebt een Azure-botservice gemaakt, u hebt het botgeheim en het .bot-bestandspad gekopieerd en u hebt het zip-bestand van de code gedownload. U hebt het vooraf gebouwde HomeAutomation-domein toegevoegd aan de LUIS-app. Deze app is gemaakt als onderdeel van de nieuwe Azure-botservice. Vervolgens id de app getraind en opnieuw gepubliceerd. U hebt het codeproject uitgepakt, een omgevingsbestand gemaakt (`.env`) en het botgeheim en het .bot-bestandspad ingesteld. In het bestand bot.js hebt u code toegevoegd om de twee nieuwe intenties te kunnen verwerken. Vervolgens hebt u de bot in de botemulator getest om de LUIS-reactie op een uiting van een van de nieuwe intenties te bekijken. 


> [!div class="nextstepaction"]
> [Een aangepast domein bouwen in LUIS](luis-quickstart-intents-only.md)
