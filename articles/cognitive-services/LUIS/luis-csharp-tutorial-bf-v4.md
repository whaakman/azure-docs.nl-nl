---
title: Language Understanding Bot C# v4-processors
titleSuffix: Language Understanding - Azure Cognitive Services
description: Bouw met behulp van C# een chatbot met ingebouwd taalbegrip (LUIS). De bot is gebouwd met de Bot Framework-versie 4 en de Azure-Web-app-bot-service.
services: cognitive-services
author: diberry
ms.custom: seodec18
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: tutorial
ms.date: 06/24/2019
ms.author: diberry
ms.openlocfilehash: 8a03d87441f26d3116aff8af33fd94da0ef9a909
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67438436"
---
# <a name="tutorial-use-a-web-app-bot-enabled-with-language-understanding-in-c"></a>Zelfstudie: Gebruik een Web-App-Bot ingeschakeld met Language Understanding inC#

Gebruik C# een chatbot bouwen die is geïntegreerd met taalbegrip (LUIS). De bot is gebouwd met de Azure [Web-app-bot](https://docs.microsoft.com/azure/bot-service/) resource en [Bot Framework-versie](https://github.com/Microsoft/botbuilder-dotnet) V4-processors.

**In deze zelfstudie leert u het volgende:**

> [!div class="checklist"]
> * Een web-app-bot maken. Bij dit proces wordt een nieuwe LUIS-app gemaakt.
> * De bot-project hebt gemaakt door de webservice van de bot gedownload
> * De bot en emulator lokaal op uw computer starten
> * De uitingsresultaten in de bot bekijken

## <a name="prerequisites"></a>Vereisten

* [Botemulator](https://aka.ms/abs/build/emulatordownload)
* [Visual Studio](https://visualstudio.microsoft.com/downloads/)


## <a name="create-a-web-app-bot-resource"></a>Maak een web-app-bot-resource

1. Selecteer in de [Azure-portal](https://portal.azure.com) de optie **Nieuwe resource maken**.

1. Zoek via het zoekvak naar **Web-app-bot** en selecteer deze optie. Selecteer **Maken**.

1. In **Bot Service** geeft u de vereiste informatie op:

    |Instelling|Doel|Aanbevolen instelling|
    |--|--|--|
    |Botnaam|Resourcenaam|`luis-csharp-bot-` + `<your-name>`, bijvoorbeeld, `luis-csharp-bot-johnsmith`|
    |Abonnement|Het abonnement waarvoor de bot moet worden gemaakt.|Uw primaire abonnement.
    |Resourcegroep|Logische groep van Azure-resources|Maak een nieuwe groep voor het opslaan van alle resources die worden gebruikt met deze bot. Geef de groep de naam `luis-csharp-bot-resource-group`.|
    |Location|Azure-regio: deze hoeft niet dezelfde te zijn als de LUIS-regio voor maken en publiceren.|`westus`|
    |Prijscategorie|Wordt gebruikt voor serviceaanvraaglimieten en facturatie.|`F0` is de gratis laag.
    |Naam van app|Deze naam wordt gebruikt als subdomein wanneer uw bot wordt geïmplementeerd in de cloud (bijvoorbeeld humanresourcesbot.azurewebsites.net).|`luis-csharp-bot-` + `<your-name>`, bijvoorbeeld, `luis-csharp-bot-johnsmith`|
    |Botsjabloon|Instellingen voor het botframework - zie de volgende tabel|
    |Locatie van LUIS-app|Moet hetzelfde zijn als de regio waarin de LUIS-resource zich bevindt|`westus`|
    |App service-plan/locatie|Niet wijzigen van de opgegeven standaardwaarde.|
    |Application Insights|Niet wijzigen van de opgegeven standaardwaarde.|
    |Microsoft App-ID en wachtwoord|Niet wijzigen van de opgegeven standaardwaarde.|

1. In de **Bot sjabloon**, selecteert u het volgende, en kies vervolgens de **Selecteer** knop onder deze instellingen:

    |Instelling|Doel|Selectie|
    |--|--|--|
    |SDK-versie|Versie van het botframework|**SDK v4**|
    |SDK-taal|Computertaal van de bot|**C#**|
    |Bot|Type bot|**Basisbot**|
    
1. Selecteer **Maken**. Hiermee maakt u de botservice en implementeert u deze in Azure. Bij dit proces wordt een LUIS-app gemaakt met de naam `luis-csharp-bot-XXXX`. Deze naam is gebaseerd op de naam van de /Azure Bot Service-app.

    [![Een web-app-bot maken](./media/bfv4-csharp/create-web-app-service.png)](./media/bfv4-csharp/create-web-app-service.png#lightbox)

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

    ![Schermopname van Azure-portal, voer de tekst 'Hallo'.](./media/bfv4-csharp/ask-bot-question-in-portal-test-in-web-chat.png)

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

1. Open de **LuisHelper.cs** bestand. Hier wordt de uiting die de gebruiker heeft ingevoerd in de bot naar LUIS verzonden. De reactie van LUIS wordt geretourneerd vanaf de methode als een **BookDetails** object. Wanneer u uw eigen bot maakt, moet u ook uw eigen object om de details van LUIS terug te maken. 


    ```csharp
    // Copyright (c) Microsoft Corporation. All rights reserved.
    // Licensed under the MIT License.
    
    using System;
    using System.Linq;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.Bot.Builder;
    using Microsoft.Bot.Builder.AI.Luis;
    using Microsoft.Extensions.Configuration;
    using Microsoft.Extensions.Logging;
    
    namespace Microsoft.BotBuilderSamples
    {
        public static class LuisHelper
        {
            public static async Task<BookingDetails> ExecuteLuisQuery(IConfiguration configuration, ILogger logger, ITurnContext turnContext, CancellationToken cancellationToken)
            {
                var bookingDetails = new BookingDetails();
    
                try
                {
                    // Create the LUIS settings from configuration.
                    var luisApplication = new LuisApplication(
                        configuration["LuisAppId"],
                        configuration["LuisAPIKey"],
                        "https://" + configuration["LuisAPIHostName"]
                    );
    
                    var recognizer = new LuisRecognizer(luisApplication);
    
                    // The actual call to LUIS
                    var recognizerResult = await recognizer.RecognizeAsync(turnContext, cancellationToken);
    
                    var (intent, score) = recognizerResult.GetTopScoringIntent();
                    if (intent == "Book_flight")
                    {
                        // We need to get the result from the LUIS JSON which at every level returns an array.
                        bookingDetails.Destination = recognizerResult.Entities["To"]?.FirstOrDefault()?["Airport"]?.FirstOrDefault()?.FirstOrDefault()?.ToString();
                        bookingDetails.Origin = recognizerResult.Entities["From"]?.FirstOrDefault()?["Airport"]?.FirstOrDefault()?.FirstOrDefault()?.ToString();
    
                        // This value will be a TIMEX. And we are only interested in a Date so grab the first result and drop the Time part.
                        // TIMEX is a format that represents DateTime expressions that include some ambiguity. e.g. missing a Year.
                        bookingDetails.TravelDate = recognizerResult.Entities["datetime"]?.FirstOrDefault()?["timex"]?.FirstOrDefault()?.ToString().Split('T')[0];
                    }
                }
                catch (Exception e)
                {
                    logger.LogWarning($"LUIS Exception: {e.Message} Check your LUIS configuration.");
                }
    
                return bookingDetails;
            }
        }
    }
    ```

1. Open **BookingDetails.cs** om weer te geven hoe het object isoleert de LUIS-informatie. 

    ```csharp
    // Copyright (c) Microsoft Corporation. All rights reserved.
    // Licensed under the MIT License.
    
    namespace Microsoft.BotBuilderSamples
    {
        public class BookingDetails
        {
            public string Destination { get; set; }
    
            public string Origin { get; set; }
    
            public string TravelDate { get; set; }
        }
    }
    ```

1. Open **dialoogvensters -> BookingDialog.cs** om te begrijpen hoe de BookingDetails-object wordt gebruikt voor het beheren van de conversatie-stroom. Reizen details in stappen wordt gevraagd, wordt de hele boeking is bevestigd en ten slotte terug naar de gebruiker wordt herhaald. 

    ```csharp
    // Copyright (c) Microsoft Corporation. All rights reserved.
    // Licensed under the MIT License.
    
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.Bot.Builder;
    using Microsoft.Bot.Builder.Dialogs;
    using Microsoft.Recognizers.Text.DataTypes.TimexExpression;
    
    namespace Microsoft.BotBuilderSamples.Dialogs
    {
        public class BookingDialog : CancelAndHelpDialog
        {
            public BookingDialog()
                : base(nameof(BookingDialog))
            {
                AddDialog(new TextPrompt(nameof(TextPrompt)));
                AddDialog(new ConfirmPrompt(nameof(ConfirmPrompt)));
                AddDialog(new DateResolverDialog());
                AddDialog(new WaterfallDialog(nameof(WaterfallDialog), new WaterfallStep[]
                {
                    DestinationStepAsync,
                    OriginStepAsync,
                    TravelDateStepAsync,
                    ConfirmStepAsync,
                    FinalStepAsync,
                }));
    
                // The initial child Dialog to run.
                InitialDialogId = nameof(WaterfallDialog);
            }
    
            private async Task<DialogTurnResult> DestinationStepAsync(WaterfallStepContext stepContext, CancellationToken cancellationToken)
            {
                var bookingDetails = (BookingDetails)stepContext.Options;
    
                if (bookingDetails.Destination == null)
                {
                    return await stepContext.PromptAsync(nameof(TextPrompt), new PromptOptions { Prompt = MessageFactory.Text("Where would you like to travel to?") }, cancellationToken);
                }
                else
                {
                    return await stepContext.NextAsync(bookingDetails.Destination, cancellationToken);
                }
            }
    
            private async Task<DialogTurnResult> OriginStepAsync(WaterfallStepContext stepContext, CancellationToken cancellationToken)
            {
                var bookingDetails = (BookingDetails)stepContext.Options;
    
                bookingDetails.Destination = (string)stepContext.Result;
    
                if (bookingDetails.Origin == null)
                {
                    return await stepContext.PromptAsync(nameof(TextPrompt), new PromptOptions { Prompt = MessageFactory.Text("Where are you traveling from?") }, cancellationToken);
                }
                else
                {
                    return await stepContext.NextAsync(bookingDetails.Origin, cancellationToken);
                }
            }
            private async Task<DialogTurnResult> TravelDateStepAsync(WaterfallStepContext stepContext, CancellationToken cancellationToken)
            {
                var bookingDetails = (BookingDetails)stepContext.Options;
    
                bookingDetails.Origin = (string)stepContext.Result;
    
                if (bookingDetails.TravelDate == null || IsAmbiguous(bookingDetails.TravelDate))
                {
                    return await stepContext.BeginDialogAsync(nameof(DateResolverDialog), bookingDetails.TravelDate, cancellationToken);
                }
                else
                {
                    return await stepContext.NextAsync(bookingDetails.TravelDate, cancellationToken);
                }
            }
    
            private async Task<DialogTurnResult> ConfirmStepAsync(WaterfallStepContext stepContext, CancellationToken cancellationToken)
            {
                var bookingDetails = (BookingDetails)stepContext.Options;
    
                bookingDetails.TravelDate = (string)stepContext.Result;
    
                var msg = $"Please confirm, I have you traveling to: {bookingDetails.Destination} from: {bookingDetails.Origin} on: {bookingDetails.TravelDate}";
    
                return await stepContext.PromptAsync(nameof(ConfirmPrompt), new PromptOptions { Prompt = MessageFactory.Text(msg) }, cancellationToken);
            }
    
            private async Task<DialogTurnResult> FinalStepAsync(WaterfallStepContext stepContext, CancellationToken cancellationToken)
            {
                if ((bool)stepContext.Result)
                {
                    var bookingDetails = (BookingDetails)stepContext.Options;
    
                    return await stepContext.EndDialogAsync(bookingDetails, cancellationToken);
                }
                else
                {
                    return await stepContext.EndDialogAsync(null, cancellationToken);
                }
            }
    
            private static bool IsAmbiguous(string timex)
            {
                var timexProperty = new TimexProperty(timex);
                return !timexProperty.Types.Contains(Constants.TimexTypes.Definite);
            }
        }
    }
    ```


## <a name="start-the-bot-code-in-visual-studio"></a>De bot-code in Visual Studio starten

Start de bot in Visual Studio. Er wordt een browservenster geopend met de website van de web-app-bot op `http://localhost:3978/`. Er wordt een startpagina weergegeven met informatie over uw bot.

![Er wordt een startpagina weergegeven met informatie over uw bot.](./media/bfv4-csharp/running-bot-web-home-page-success.png)

## <a name="use-the-bot-emulator-to-test-the-bot"></a>De bot-emulator gebruikt voor het testen van de bot

1. Beginnen met de Bot-Emulator en selecteer **Open Bot**.
1. In de **openen van een bot** pop-upvenster, voer de URL van uw bot, zoals `http://localhost:3978/api/messages`. De `/api/messages` route is het webadres van de bot.
1. Voer de **Microsoft App-ID** en **Microsoft App-wachtwoord**vindt u in de **appsettings.json** bestand in de hoofdmap van de bot-code die u hebt gedownload.

    (Optioneel) u een nieuwe bot kunt maken, configuratie en kopieer de `appId` en `appPassword` uit de **appsettings.json** bestand in de Visual Studio-project voor de bot. De naam van het configuratiebestand van de bot moet hetzelfde zijn als de botnaam van de. 

    ```json
    {
        "name": "<bot name>",
        "description": "<bot description>",
        "services": [
            {
                "type": "endpoint",
                "appId": "<appId from appsettings.json>",
                "appPassword": "<appPassword from appsettings.json>",
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

    [![Antwoord van de basisbot in de emulator](./media/bfv4-csharp/ask-bot-emulator-a-question-and-get-response.png)](./media/bfv4-csharp/ask-bot-emulator-a-question-and-get-response.png#lightbox)


## <a name="ask-bot-a-question-for-the-book-flight-intent"></a>Een vraag stellen bot voor het doel vlucht

1. Boek een vlucht door in te voeren van de volgende utterance in de emulator bot: 

    ```bot
    Book a flight from Paris to Berlin on March 22, 2020
    ```

    De bot-emulator wordt gevraagd om te bevestigen. 

1. Selecteer **Ja**. De bot reageert met een overzicht van de acties. 
1. Selecteer in het logboek van de emulator bot, de regel met `Luis Trace`. U ziet nu het JSON-antwoord van LUIS voor het doel en de entiteiten van de utterance.

    [![Antwoord van de basisbot in de emulator](./media/bfv4-csharp/ask-luis-book-flight-question-get-json-response-in-bot-emulator.png)](./media/bfv4-csharp/ask-luis-book-flight-question-get-json-response-in-bot-emulator.png#lightbox)

[!INCLUDE [Bot Information](../../../includes/cognitive-services-qnamaker-luis-bot-info.md)]

## <a name="next-steps"></a>Volgende stappen

Bekijk meer [voorbeelden](https://github.com/microsoft/botframework-solutions) met gespreksbots. 

> [!div class="nextstepaction"]
> [Een Language Understanding bouwen met het domein van een aangepast onderwerp](luis-quickstart-intents-only.md)
