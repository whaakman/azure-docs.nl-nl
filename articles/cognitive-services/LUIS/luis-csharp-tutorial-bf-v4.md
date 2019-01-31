---
title: Bot - C# - v4
titleSuffix: Language Understanding - Azure Cognitive Services
description: Bouw met behulp van C# een chatbot met ingebouwd taalbegrip (LUIS). Deze chatbot maakt gebruik van de app Human Resources om snel een botoplossing te implementeren. De bot is gebouwd met Bot Framework versie 4 en de Azure-web-app-bot.
services: cognitive-services
author: diberry
ms.custom: seodec18
manager: cjgronlund
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: tutorial
ms.date: 01/09/2019
ms.author: diberry
ms.openlocfilehash: 98c65b7adfe35c1ca80846ff7619dad69c5ba266
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55219223"
---
# <a name="tutorial-luis-bot-in-c-with-the-bot-framework-4x-and-the-azure-web-app-bot"></a>Zelfstudie: LUIS-bot in C# met Bot Framework 4.x en de Azure-web-app-bot
Met C# kunt u een chatbot bouwen met ingebouwd taalbegrip (LUIS). Deze bot maakt gebruik van de app HomeAutomation om een botoplossing te implementeren. De bot is gebouwd aan de hand van de Azure-[web-app-bot](https://docs.microsoft.com/azure/bot-service/) en [Bot Framework versie](https://github.com/Microsoft/botbuilder-js) 4.

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
* [Visual Studio](https://visualstudio.microsoft.com/downloads/)


## <a name="create-web-app-bot"></a>Een web-app-bot maken

1. Selecteer in de [Azure-portal](https://portal.azure.com) de optie **Nieuwe resource maken**.

2. Zoek via het zoekvak naar **Web-app-bot** en selecteer deze optie. Selecteer **Maken**.

3. In **Bot Service** geeft u de vereiste informatie op:

    |Instelling|Doel|Aanbevolen instelling|
    |--|--|--|
    |Botnaam|Resourcenaam|`luis-csharp-bot-` + `<your-name>`, bijvoorbeeld, `luis-csharp-bot-johnsmith`|
    |Abonnement|Het abonnement waarvoor de bot moet worden gemaakt.|Uw primaire abonnement.
    |Resourcegroep|Logische groep van Azure-resources|Maak een nieuwe groep voor het opslaan van alle resources die worden gebruikt met deze bot. Geef de groep de naam `luis-csharp-bot-resource-group`.|
    |Locatie|Azure-regio: deze hoeft niet dezelfde te zijn als de LUIS-regio voor maken en publiceren.|`westus`|
    |Prijscategorie|Wordt gebruikt voor serviceaanvraaglimieten en facturatie.|`F0` is de gratis laag.
    |Naam van app|Deze naam wordt gebruikt als subdomein wanneer uw bot wordt geïmplementeerd in de cloud (bijvoorbeeld humanresourcesbot.azurewebsites.net).|`luis-csharp-bot-` + `<your-name>`, bijvoorbeeld, `luis-csharp-bot-johnsmith`|
    |Botsjabloon|Instellingen voor het botframework - zie de volgende tabel|
    |Locatie van LUIS-app|Moet hetzelfde zijn als de regio waarin de LUIS-resource zich bevindt|`westus`|

4. In de **botsjablooninstellingen** selecteert u het volgende en kiest u onder deze instellingen de knop **Selecteren**:

    |Instelling|Doel|Selectie|
    |--|--|--|
    |SDK-versie|Versie van het botframework|**SDK v4**|
    |SDK-taal|Computertaal van de bot|**C#**|
    |Echo-/basisbot|Type bot|**Basisbot**|
    
5. Selecteer **Maken**. Hiermee maakt u de botservice en implementeert u deze in Azure. Bij dit proces wordt een LUIS-app gemaakt met de naam `luis-csharp-bot-XXXX`. Deze naam is gebaseerd op de bot en de app-naam uit het vorige gedeelte.

    [ ![Een web-app-bot maken](./media/bfv4-csharp/create-web-app-service.png) ](./media/bfv4-csharp/create-web-app-service.png#lightbox)

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
2. Op de pagina **Mijn apps** selecteert u de kolom **Gemaakt op** om te sorteren op de datum waarop de app is gemaakt. Azure Bot Service heeft in het vorige gedeelte een nieuwe app gemaakt. De naam ervan is `luis-csharp-bot-` + `<your-name>` plus vier willekeurige tekens.
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

6. Open het bestand bot.cs en zoek naar `_services.LuisServices`. Hier wordt de uiting die de gebruiker heeft ingevoerd in de bot naar LUIS verzonden.

    ```csharp
    /// <summary>
    /// Run every turn of the conversation. Handles orchestration of messages.
    /// </summary>
    /// <param name="turnContext">Bot Turn Context.</param>
    /// <param name="cancellationToken">Task CancellationToken.</param>
    /// <returns>A <see cref="Task"/> representing the asynchronous operation.</returns>
    public async Task OnTurnAsync(ITurnContext turnContext, CancellationToken cancellationToken)
    {
        var activity = turnContext.Activity;

        if (activity.Type == ActivityTypes.Message)
        {
            // Perform a call to LUIS to retrieve results for the current activity message.
            var luisResults = await _services.LuisServices[LuisConfiguration].RecognizeAsync(turnContext, cancellationToken).ConfigureAwait(false);

            // If any entities were updated, treat as interruption.
            // For example, "no my name is tony" will manifest as an update of the name to be "tony".
            var topScoringIntent = luisResults?.GetTopScoringIntent();

            var topIntent = topScoringIntent.Value.intent;
            switch (topIntent)
            {
                case GreetingIntent:
                    await turnContext.SendActivityAsync("Hello.");
                    break;
                case HelpIntent:
                    await turnContext.SendActivityAsync("Let me try to provide some help.");
                    await turnContext.SendActivityAsync("I understand greetings, being asked for help, or being asked to cancel what I am doing.");
                    break;
                case CancelIntent:
                    await turnContext.SendActivityAsync("I have nothing to cancel.");
                    break;
                case NoneIntent:
                default:
                    // Help or no intent identified, either way, let's provide some help.
                    // to the user
                    await turnContext.SendActivityAsync("I didn't understand what you just said to me.");
                    break;
            }
        }
        else if (activity.Type == ActivityTypes.ConversationUpdate)
        {
            if (activity.MembersAdded.Any())
            {
                // Iterate over all new members added to the conversation.
                foreach (var member in activity.MembersAdded)
                {
                    // Greet anyone that was not the target (recipient) of this message.
                    // To learn more about Adaptive Cards, see https://aka.ms/msbot-adaptivecards for more details.
                    if (member.Id != activity.Recipient.Id)
                    {
                        var welcomeCard = CreateAdaptiveCardAttachment();
                        var response = CreateResponse(activity, welcomeCard);
                        await turnContext.SendActivityAsync(response).ConfigureAwait(false);
                    }
                }
            }
        }

    }
    ```

    De bot verzendt de uiting van de gebruiker naar LUIS en haalt de resultaten op. De belangrijkste intentie bepaalt hoe de conversatie verder verloopt. 


## <a name="start-the-bot"></a>De bot starten
Voordat u code of instellingen wijzigt, controleert u of de bot werkt. 

1. Open het oplossingenbestand in Visual Studio. 

2. Maak een `appsettings.json`-bestand voor het opslaan van de botvariabelen waar de botcode naar op zoek gaat:

    ```JSON
    {
    "botFileSecret": "",
    "botFilePath": ""

    }
    ```

    Stel de waarden van de variabelen in op de waarden die u hebt gekopieerd uit de toepassingsinstellingen van Azure Bot Service. Dit is gebeurd in stap 1 van het gedeelte **[De web-app-bot downloaden](#download-the-web-app-bot)**.

3. Start de bot in Visual Studio. Er wordt een browservenster geopend met de website van de web-app-bot op `http://localhost:3978/`.

## <a name="start-the-emulator"></a>De emulator starten

1. Open de botemulator.

2. In de botemulator selecteert u het *.bot-bestand in de hoofdmap van het project. Het `.bot`-bestand bevat het URL-eindpunt die de bot gebruikt voor berichten:

    [ ![Botemulator versie 4](../../../includes/media/cognitive-services-luis/bfv4/bot-emulator-v4.png) ](../../../includes/media/cognitive-services-luis/bfv4/bot-emulator-v4.png#lightbox)

3. Voer het botgeheim in dat u hebt gekopieerd uit de toepassingsinstellingen van Azure Bot Service. Dit is gebeurd in stap 1 van het gedeelte **[De web-app-bot downloaden](#download-the-web-app-bot)**. Hiermee krijgt de emulator toegang tot alle versleutelde velden in het `.bot`-bestand.

    ![Geheim in botemulator versie 4](../../../includes/media/cognitive-services-luis/bfv4/bot-secret.png)

4. Voer `Hello` in de botemulator in en haal het juiste antwoord op bij de basisbot.

    [ ![Antwoord van de basisbot in de emulator](../../../includes/media/cognitive-services-luis/bfv4/emulator-test.png) ](../../../includes/media/cognitive-services-luis/bfv4/emulator-test.png#lightbox)

## <a name="modify-bot-code"></a>Botcode wijzigen 

In het bestand `BasicBot.cs` voegt u code toe om de nieuwe intenties te verwerken. 

1. Boven in het bestand gaat u naar het gedeelte **Ondersteunde LUIS-intenties** en voegt u constanten toe voor de HomeAutomation-intenties:

    ```csharp
    // Supported LUIS Intents
    public const string GreetingIntent = "Greeting";
    public const string CancelIntent = "Cancel";
    public const string HelpIntent = "Help";
    public const string NoneIntent = "None";
    public const string TurnOnIntent = "HomeAutomation_TurnOn"; // new intent
    public const string TurnOffIntent = "HomeAutomation_TurnOff"; // new intent
    ```

    De punt (`.`) tussen het domein en de intentie uit de LUIS-portal-app is vervangen door een onderstrepingsteken (`_`). 

2. Zoek de **OnTurnAsync**-methode die de LUIS-voorspelling van de uiting ontvangt. Voeg code toe in de switch-instructie om de LUIS-reactie te retourneren voor de twee HomeAutomation-intenties. 

    ```csharp
    case TurnOnIntent:
        await turnContext.SendActivityAsync("TurnOn intent found, JSON response: " + luisResults?.Entities.ToString());
        break;
    case TurnOffIntent:
        await turnContext.SendActivityAsync("TurnOff intent found, JSON response: " + luisResults?.Entities.ToString());
        break;
    ```

    De bot heeft niet precies hetzelfde antwoord als een LUIS REST API-aanvraag. Het is dus belangrijk om de verschillen te leren kennen door naar de JSON-reactie te kijken. De tekst- en intentie-eigenschappen zijn hetzelfde, maar de eigenschapswaarden van de entiteiten zijn gewijzigd. 

    ```JSON
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



## <a name="view-results-in-bot"></a>Resultaten in de bot bekijken

1. Voer in de botemulator de volgende uiting in: `Turn on the livingroom lights to 50%`

2. De bot reageert met:

    ```JSON
    TurnOn intent found, JSON response: {"$instance":{“HomeAutomation_Device”:[{“startIndex”:23,“endIndex”:29,“score”:0.9776345,“text”:“lights”,“type”:“HomeAutomation.Device”}],“HomeAutomation_Room”:[{“startIndex”:12,“endIndex”:22,“score”:0.9079433,“text”:“livingroom”,“type”:“HomeAutomation.Room”}]},“HomeAutomation_Device”:[“lights”],“HomeAutomation_Room”:[“livingroom”]}
    ```    

## <a name="learn-more-about-bot-framework"></a>Meer informatie over Bot Framework
Azure Bot Service maakt gebruik van de Bot Framework-SDK. Meer informatie over de SDK en het botframework:

* [Azure Bot Service](https://docs.microsoft.com/azure/bot-service/bot-service-overview-introduction?view=azure-bot-service-4.0) v4-documentatie
* [Bot Builder-voorbeelden](https://github.com/Microsoft/botbuilder-samples)
* [Bot Builder-SDK](https://docs.microsoft.com/javascript/api/botbuilder-core/?view=botbuilder-ts-latest)
* [Bot Builder-hulpprogramma's](https://github.com/Microsoft/botbuilder-tools):

## <a name="next-steps"></a>Volgende stappen

U hebt een Azure-botservice gemaakt, u hebt het botgeheim en het `.bot`-bestandspad gekopieerd en u hebt het zip-bestand van de code gedownload. U hebt het vooraf gebouwde HomeAutomation-domein toegevoegd aan de LUIS-app. Deze app is gemaakt als onderdeel van de nieuwe Azure-botservice. Vervolgens id de app getraind en opnieuw gepubliceerd. U hebt het codeproject uitgepakt, een omgevingsbestand (`.env`) gemaakt en het botgeheim en het `.bot`-bestandspad ingesteld. In het bestand bot.js hebt u code toegevoegd om de twee nieuwe intenties te kunnen verwerken. Vervolgens hebt u de bot in de botemulator getest om de LUIS-reactie op een uiting van een van de nieuwe intenties te bekijken. 

Bekijk meer [voorbeelden](https://github.com/Microsoft/AI) met gespreksbots. 

> [!div class="nextstepaction"]
> [Een aangepast domein bouwen in LUIS](luis-quickstart-intents-only.md)
