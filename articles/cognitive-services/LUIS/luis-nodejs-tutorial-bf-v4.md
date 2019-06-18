---
title: Language Understanding Bot Node.js v4-processors
titleSuffix: Azure Cognitive Services
description: Bouw met behulp van Node.js een chatbot met ingebouwd taalbegrip (LUIS). Deze chatbot maakt gebruik van de app Human Resources om snel een botoplossing te implementeren. De bot is gebouwd met Bot Framework versie 4 en de Azure-web-app-bot.
services: cognitive-services
author: diberry
ms.custom: seodec18
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: tutorial
ms.date: 06/15/2019
ms.author: diberry
ms.openlocfilehash: 832a62c5cc5440d81f4b92d2463a563f5bb884a3
ms.sourcegitcommit: 6e6813f8e5fa1f6f4661a640a49dc4c864f8a6cb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/17/2019
ms.locfileid: "67150834"
---
# <a name="tutorial-use-a-web-app-bot-enabled-with-language-understanding-in-nodejs"></a>Zelfstudie: Gebruik een Web-App-Bot ingeschakeld met Language Understanding in Node.js 

Node.js gebruiken om u te maken van een chatbot geïntegreerd met taalbegrip (LUIS). De bot is gebouwd met de Azure [Web-app-bot](https://docs.microsoft.com/azure/bot-service/) resource en [Bot Framework-versie](https://github.com/Microsoft/botbuilder-dotnet) V4-processors.

**In deze zelfstudie leert u het volgende:**

> [!div class="checklist"]
> * Een web-app-bot maken. Bij dit proces wordt een nieuwe LUIS-app gemaakt.
> * De bot-project hebt gemaakt door de webservice van de bot gedownload
> * De bot en emulator lokaal op uw computer starten
> * De uitingsresultaten in de bot bekijken

## <a name="prerequisites"></a>Vereisten

* [Botemulator](https://aka.ms/abs/build/emulatordownload)
* [Visual Studio Code](https://code.visualstudio.com/Download)


## <a name="create-a-web-app-bot-resource"></a>Maak een web-app-bot-resource

1. Selecteer in de [Azure-portal](https://portal.azure.com) de optie **Nieuwe resource maken**.

1. Zoek via het zoekvak naar **Web-app-bot** en selecteer deze optie. Selecteer **Maken**.

1. In **Bot Service** geeft u de vereiste informatie op:

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
    |App service-plan/locatie|Niet wijzigen van de opgegeven standaardwaarde.|
    |Application Insights|Niet wijzigen van de opgegeven standaardwaarde.|
    |Microsoft App-ID en wachtwoord|Niet wijzigen van de opgegeven standaardwaarde.|

1. In de **Bot sjabloon**, selecteert u het volgende, en kies vervolgens de **Selecteer** knop onder deze instellingen:

    |Instelling|Doel|Selectie|
    |--|--|--|
    |SDK-versie|Versie van het botframework|**SDK v4**|
    |SDK-taal|Computertaal van de bot|**Node.js**|
    |Bot|Type bot|**Basisbot**|
    
1. Selecteer **Maken**. Hiermee maakt u de botservice en implementeert u deze in Azure. Bij dit proces wordt een LUIS-app gemaakt met de naam `luis-nodejs-bot-XXXX`. Deze naam is gebaseerd op de naam van de /Azure Bot Service-app.

    [![Een web-app-bot maken](./media/bfv4-nodejs/create-web-app-service.png)](./media/bfv4-nodejs/create-web-app-service.png#lightbox)

    Wacht totdat de botservice is gemaakt voordat u doorgaat.

## <a name="the-bot-has-a-language-understanding-model"></a>De bot is een model Language Understanding

Het proces voor het maken van bot service maakt ook een nieuwe LUIS-app met intents en voorbeeld-uitingen. Met de bot kunnen de volgende intenties worden toegewezen aan de nieuwe LUIS-app: 

|LUIS-intenties van de basisbot|voorbeelduiting|
|--|--|
|Vlucht|`Travel to Paris`|
|Annuleren|`bye`|
|Geen|Alles buiten het domein van de app.|

## <a name="test-the-bot-in-web-chat"></a>De bot in Web Chat testen

1. Selecteer terwijl u nog in de Azure-portal voor de nieuwe bot **Test in Web Chat**. 
1. In de **Typ uw bericht** tekstvak, voert u de tekst `hello`. De bot reageert met informatie over de botframework, evenals de voorbeelden van query's voor het specifieke LUIS-model, zoals een vlucht naar Parijs reservering. 

    ![Schermopname van Azure-portal, voer de tekst 'Hallo'.](./media/bfv4-nodejs/ask-bot-question-in-portal-test-in-web-chat.png)

    U kunt de test-functionaliteit gebruiken voor het snel testen van uw bot. Voor meer voltooien testen, inclusief foutopsporing, downloaden de bot-code en Visual Studio gebruiken. 

## <a name="download-the-web-app-bot-source-code"></a>De code van de bot bron voor de web-app downloaden
Als u de code van de web-app-bot verder wilt ontwikkelen, downloadt u de code en gebruikt u deze op uw lokale computer. 

1. In de Azure-portal selecteert u **Bouwen** in het gedeelte **Botbeheer**. 

1. Selecteer **Broncode bot downloaden**. 

    [![Download de broncode van de web-app-bot voor de basisbot](../../../includes/media/cognitive-services-luis/bfv4/download-code.png)](../../../includes/media/cognitive-services-luis/bfv4/download-code.png#lightbox)

1. Wanneer het pop-updialoogvenster vraagt **app-instellingen in het gedownloade zip-bestand opnemen?** , selecteer **Ja**.

1. Als de broncode is ingepakt, ziet u een bericht met een link voor het downloaden van de code. Selecteer de link. 

1. Sla het zip-bestand op uw lokale computer op en pak de bestanden uit. Open het project met Visual Studio. 

## <a name="review-code-to-send-utterance-to-luis-and-get-response"></a>Code utterance naar LUIS verzenden en ophalen van antwoord bekijken

1. Open de **dialoogvensters -> luisHelper.js** bestand. Hier wordt de uiting die de gebruiker heeft ingevoerd in de bot naar LUIS verzonden. De reactie van LUIS wordt geretourneerd vanaf de methode als een **bookDetails** JSON-object. Wanneer u uw eigen bot maakt, moet u ook uw eigen object om de details van LUIS terug te maken. 

    ```nodejs
    // Copyright (c) Microsoft Corporation. All rights reserved.
    // Licensed under the MIT License.
    
    const { LuisRecognizer } = require('botbuilder-ai');
    
    class LuisHelper {
        /**
         * Returns an object with preformatted LUIS results for the bot's dialogs to consume.
         * @param {*} logger
         * @param {TurnContext} context
         */
        static async executeLuisQuery(logger, context) {
            const bookingDetails = {};
    
            try {
                const recognizer = new LuisRecognizer({
                    applicationId: process.env.LuisAppId,
                    endpointKey: process.env.LuisAPIKey,
                    endpoint: `https://${ process.env.LuisAPIHostName }`
                }, {}, true);
    
                const recognizerResult = await recognizer.recognize(context);
    
                const intent = LuisRecognizer.topIntent(recognizerResult);
    
                bookingDetails.intent = intent;
    
                if (intent === 'Book_flight') {
                    // We need to get the result from the LUIS JSON which at every level returns an array
    
                    bookingDetails.destination = LuisHelper.parseCompositeEntity(recognizerResult, 'To', 'Airport');
                    bookingDetails.origin = LuisHelper.parseCompositeEntity(recognizerResult, 'From', 'Airport');
    
                    // This value will be a TIMEX. And we are only interested in a Date so grab the first result and drop the Time part.
                    // TIMEX is a format that represents DateTime expressions that include some ambiguity. e.g. missing a Year.
                    bookingDetails.travelDate = LuisHelper.parseDatetimeEntity(recognizerResult);
                }
            } catch (err) {
                logger.warn(`LUIS Exception: ${ err } Check your LUIS configuration`);
            }
            return bookingDetails;
        }
    
        static parseCompositeEntity(result, compositeName, entityName) {
            const compositeEntity = result.entities[compositeName];
            if (!compositeEntity || !compositeEntity[0]) return undefined;
    
            const entity = compositeEntity[0][entityName];
            if (!entity || !entity[0]) return undefined;
    
            const entityValue = entity[0][0];
            return entityValue;
        }
    
        static parseDatetimeEntity(result) {
            const datetimeEntity = result.entities['datetime'];
            if (!datetimeEntity || !datetimeEntity[0]) return undefined;
    
            const timex = datetimeEntity[0]['timex'];
            if (!timex || !timex[0]) return undefined;
    
            const datetime = timex[0].split('T')[0];
            return datetime;
        }
    }
    
    module.exports.LuisHelper = LuisHelper;
    ```

1. Open **dialoogvensters -> bookingDialog.js** om te begrijpen hoe de BookingDetails-object wordt gebruikt voor het beheren van de conversatie-stroom. Reizen details in stappen wordt gevraagd, wordt de hele boeking is bevestigd en ten slotte terug naar de gebruiker wordt herhaald. 

    ```nodejs
    // Copyright (c) Microsoft Corporation. All rights reserved.
    // Licensed under the MIT License.
    
    const { TimexProperty } = require('@microsoft/recognizers-text-data-types-timex-expression');
    const { ConfirmPrompt, TextPrompt, WaterfallDialog } = require('botbuilder-dialogs');
    const { CancelAndHelpDialog } = require('./cancelAndHelpDialog');
    const { DateResolverDialog } = require('./dateResolverDialog');
    
    const CONFIRM_PROMPT = 'confirmPrompt';
    const DATE_RESOLVER_DIALOG = 'dateResolverDialog';
    const TEXT_PROMPT = 'textPrompt';
    const WATERFALL_DIALOG = 'waterfallDialog';
    
    class BookingDialog extends CancelAndHelpDialog {
        constructor(id) {
            super(id || 'bookingDialog');
    
            this.addDialog(new TextPrompt(TEXT_PROMPT))
                .addDialog(new ConfirmPrompt(CONFIRM_PROMPT))
                .addDialog(new DateResolverDialog(DATE_RESOLVER_DIALOG))
                .addDialog(new WaterfallDialog(WATERFALL_DIALOG, [
                    this.destinationStep.bind(this),
                    this.originStep.bind(this),
                    this.travelDateStep.bind(this),
                    this.confirmStep.bind(this),
                    this.finalStep.bind(this)
                ]));
    
            this.initialDialogId = WATERFALL_DIALOG;
        }
    
        /**
         * If a destination city has not been provided, prompt for one.
         */
        async destinationStep(stepContext) {
            const bookingDetails = stepContext.options;
    
            if (!bookingDetails.destination) {
                return await stepContext.prompt(TEXT_PROMPT, { prompt: 'To what city would you like to travel?' });
            } else {
                return await stepContext.next(bookingDetails.destination);
            }
        }
    
        /**
         * If an origin city has not been provided, prompt for one.
         */
        async originStep(stepContext) {
            const bookingDetails = stepContext.options;
    
            // Capture the response to the previous step's prompt
            bookingDetails.destination = stepContext.result;
            if (!bookingDetails.origin) {
                return await stepContext.prompt(TEXT_PROMPT, { prompt: 'From what city will you be travelling?' });
            } else {
                return await stepContext.next(bookingDetails.origin);
            }
        }
    
        /**
         * If a travel date has not been provided, prompt for one.
         * This will use the DATE_RESOLVER_DIALOG.
         */
        async travelDateStep(stepContext) {
            const bookingDetails = stepContext.options;
    
            // Capture the results of the previous step
            bookingDetails.origin = stepContext.result;
            if (!bookingDetails.travelDate || this.isAmbiguous(bookingDetails.travelDate)) {
                return await stepContext.beginDialog(DATE_RESOLVER_DIALOG, { date: bookingDetails.travelDate });
            } else {
                return await stepContext.next(bookingDetails.travelDate);
            }
        }
    
        /**
         * Confirm the information the user has provided.
         */
        async confirmStep(stepContext) {
            const bookingDetails = stepContext.options;
    
            // Capture the results of the previous step
            bookingDetails.travelDate = stepContext.result;
            const msg = `Please confirm, I have you traveling to: ${ bookingDetails.destination } from: ${ bookingDetails.origin } on: ${ bookingDetails.travelDate }.`;
    
            // Offer a YES/NO prompt.
            return await stepContext.prompt(CONFIRM_PROMPT, { prompt: msg });
        }
    
        /**
         * Complete the interaction and end the dialog.
         */
        async finalStep(stepContext) {
            if (stepContext.result === true) {
                const bookingDetails = stepContext.options;
    
                return await stepContext.endDialog(bookingDetails);
            } else {
                return await stepContext.endDialog();
            }
        }
    
        isAmbiguous(timex) {
            const timexPropery = new TimexProperty(timex);
            return !timexPropery.types.has('definite');
        }
    }
    
    module.exports.BookingDialog = BookingDialog;
    ```


## <a name="install-dependencies-and-start-the-bot-code-in-visual-studio"></a>Afhankelijkheden installeren en starten van de bot-code in Visual Studio

1. Vscode, vanuit de geïntegreerde terminal, afhankelijkheden geïnstalleerd met de opdracht `npm install`.
1. Ook vanuit de geïntegreerde terminal, start u de bot in met de opdracht `npm start`. 


## <a name="use-the-bot-emulator-to-test-the-bot"></a>De bot-emulator gebruikt voor het testen van de bot

1. Beginnen met de Bot-Emulator en selecteer **Open Bot**.
1. In de **openen van een bot** pop-upvenster, voer de URL van uw bot, zoals `http://localhost:3978/api/messages`. De `/api/messages` route is het webadres van de bot.
1. Voer de **Microsoft App-ID** en **Microsoft App-wachtwoord**vindt u in de **.env** bestand in de hoofdmap van de bot-code die u hebt gedownload.

    (Optioneel) u een nieuwe bot kunt maken, configuratie en kopieer de `MicrosoftAppId` en `MicrosoftAppPassword` uit de **.env** bestand in de Visual Studio-project voor de bot. De naam van het configuratiebestand van de bot moet hetzelfde zijn als de botnaam van de. 

    ```json
    {
        "name": "<bot name>",
        "description": "<bot description>",
        "services": [
            {
                "type": "endpoint",
                "appId": "<appId from .env>",
                "appPassword": "<appPassword from .env>",
                "endpoint": "http://localhost:3978/api/messages",
                "id": "<don't change this value>",
                "name": "http://localhost:3978/api/messages"
            }
        ],
        "padlock": "",
        "version": "2.0",
        "overrides": null,
        "path": "<local path to .bot file>"
    }
    ```

1. Voer in de emulator bot `Hello` en hetzelfde antwoord voor de basic-bot als u hebt ontvangen de **Test in Web Chat**.

    [![Antwoord van de basisbot in de emulator](./media/bfv4-nodejs/ask-bot-emulator-a-question-and-get-response.png)](./media/bfv4-nodejs/ask-bot-emulator-a-question-and-get-response.png#lightbox)


## <a name="ask-bot-a-question-for-the-book-flight-intent"></a>Een vraag stellen bot voor het doel vlucht

1. Boek een vlucht door in te voeren van de volgende utterance in de emulator bot: 

    ```bot
    Book a flight from Paris to Berlin on March 22, 2020
    ```

    De bot-emulator wordt gevraagd om te bevestigen. 

1. Selecteer **Ja**. De bot reageert met een overzicht van de acties. 
1. Selecteer in het logboek van de emulator bot, de regel met `Luis Trace`. U ziet nu het JSON-antwoord van LUIS voor het doel en de entiteiten van de utterance.

    [![Antwoord van de basisbot in de emulator](./media/bfv4-nodejs/ask-luis-book-flight-question-get-json-response-in-bot-emulator.png)](./media/bfv4-nodejs/ask-luis-book-flight-question-get-json-response-in-bot-emulator.png#lightbox)

## <a name="learn-more-about-the-web-app-bot-and-framework"></a>Meer informatie over de Web App-Bot en framework

Azure Bot Service maakt gebruik van de Bot Framework-SDK. Meer informatie over de SDK en het botframework:

* [Azure Bot Service](https://docs.microsoft.com/azure/bot-service/bot-service-overview-introduction?view=azure-bot-service-4.0) v4-documentatie
* [Bot Builder-voorbeelden](https://github.com/Microsoft/botbuilder-samples)
* [Bot Builder-SDK voor Node.js](https://github.com/Microsoft/botbuilder-js)
* [Bot Builder-hulpprogramma's](https://github.com/Microsoft/botbuilder-tools):

## <a name="next-steps"></a>Volgende stappen

Bekijk meer [voorbeelden](https://github.com/microsoft/botframework-solutions) met gespreksbots. 

> [!div class="nextstepaction"]
> [Een Language Understanding bouwen met het domein van een aangepast onderwerp](luis-quickstart-intents-only.md)