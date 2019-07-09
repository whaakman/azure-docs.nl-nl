---
title: Application Insights, Node.js
titleSuffix: Azure Cognitive Services
description: In deze zelfstudie wordt bot en Language Understanding informatie toegevoegd aan Application Insights telemetrie-gegevensopslag.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: tutorial
ms.date: 06/16/2019
ms.author: diberry
ms.openlocfilehash: cfed5477df75350f24e77786117e85b9c728c49a
ms.sourcegitcommit: cf438e4b4e351b64fd0320bf17cc02489e61406a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/08/2019
ms.locfileid: "67657767"
---
# <a name="add-luis-results-to-application-insights-from-a-bot-in-nodejs"></a>LUIS resultaten naar Application Insights toevoegen van een Bot in Node.js
In deze zelfstudie voegt bot en Language Understanding informatie [Application Insights](https://azure.microsoft.com/services/application-insights/) telemetrie-gegevensopslag. Zodra u deze gegevens hebt, kunt u deze met de Kusto-taal of Power BI analyseren, samenvoegen, opvragen en te rapporteren intenties en entiteiten van de utterance in realtime. Met deze analyse kunt u bepalen of u moet toevoegen of bewerken van de intenties en entiteiten van uw LUIS-app.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Bot en Language understanding-gegevens in Application Insights vastleggen
> * Application Insights op te vragen voor Language Understanding-gegevens

## <a name="prerequisites"></a>Vereisten

* Een Azure bot service-bot, die zijn gemaakt met Application Insights is ingeschakeld.
* Bot-code van de vorige bot gedownload  **[zelfstudie](luis-nodejs-tutorial-bf-v4.md)** . 
* [Botemulator](https://aka.ms/abs/build/emulatordownload)
* [Visual Studio Code](https://code.visualstudio.com/Download)

Alle code in deze zelfstudie vindt u op de [Azure-Samples Language Understanding GitHub-opslagplaats](https://github.com/Azure-Samples/cognitive-services-language-understanding/tree/master/documentation-samples/tutorial-web-app-bot-application-insights/v4/luis-nodejs-bot-johnsmith-src-telemetry). 

## <a name="add-application-insights-to-web-app-bot-project"></a>Application Insights toevoegen aan de web-app-bot-project
Op dit moment verzamelt de Application Insights-service, die wordt gebruikt in deze web-app-bot, telemetrie van de algemene status voor de bot. Deze verzamelt geen informatie van LUIS. 

Als u wilt de LUIS-gegevens vastleggen, de web-app-bot moet de **[Application Insights](https://www.npmjs.com/package/applicationinsights)** NPM-pakket geïnstalleerd en geconfigureerd.  

1. In de geïntegreerde terminal VSCode in de hoofdmap van het project bot toevoegen de volgende NPM-pakketten met de opdracht die wordt weergegeven: 

    ```console
    npm install applicationinsights && npm install underscore
    ```
    
    De **onderstrepingsteken** pakket wordt gebruikt voor het samenvoegen van de LUIS JSON-structuur zodat het eenvoudiger om te zien en gebruiken in Application Insights.
    


## <a name="capture-and-send-luis-query-results-to-application-insights"></a>Vastleggen en LUIS queryresultaten verzenden naar Application Insights

1. Maak een nieuw bestand in VSCode, **appInsightsLog.js** en voeg de volgende code toe:

    ```javascript
    const appInsights = require('applicationinsights');
    const _ = require("underscore");
    
    // Log LUIS results to Application Insights
    // must flatten as name/value pairs
    var appInsightsLog = (botContext,luisResponse) => {

        appInsights.setup(process.env.MicrosoftApplicationInsightsInstrumentationKey).start();
        const appInsightsClient = appInsights.defaultClient;

        // put bot context and LUIS results into single object
        var data = Object.assign({}, {'botContext': botContext._activity}, {'luisResponse': luisResponse});
    
        // Flatten data into name/value pairs
        flatten = (x, result, prefix) => {
            if(_.isObject(x)) {
                _.each(x, (v, k) => {
                    flatten(v, result, prefix ? prefix + '_' + k : k)
                })
            } else {
                result["LUIS_" + prefix] = x
            }
            return result;
        }
    
        // call fn to flatten data
        var flattenedData = flatten(data, {});
    
        // ApplicationInsights Trace 
        console.log(JSON.stringify(flattenedData));
    
        // send data to Application Insights
        appInsightsClient.trackTrace({message: "LUIS", severity: appInsights.Contracts.SeverityLevel.Information, properties: flattenedData});
    }
    
    module.exports.appInsightsLog = appInsightsLog;
    ```

    Dit bestand heeft de bot context en de reactie van luis, beide objecten worden samengevoegd en voegt ze naar een **Trace** gebeurtenis in application insights. De naam van de gebeurtenis is **LUIS**. 

1. Open de **dialoogvensters** map, dan zal de **luisHelper.js** bestand. Zodat de nieuwe **appInsightsLog.js** als een vereist bestand en de bot context en de reactie van LUIS vast te leggen. De volledige code voor dit bestand is: 

    ```javascript
    // Copyright (c) Microsoft Corporation. All rights reserved.
    // Licensed under the MIT License.
    
    const { LuisRecognizer } = require('botbuilder-ai');
    const { appInsightsLog } = require('../appInsightsLog');
    
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
    
                // APPINSIGHT: Log results to Application Insights
                appInsightsLog(context,recognizerResult);
    
    
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

## <a name="add-application-insights-instrumentation-key"></a>Application Insights-instrumentatiesleutel toevoegen 

Gegevens naar application insights toevoegt, moet u de instrumentatiesleutel.

1. In een browser, in de [Azure-portal](https://portal.azure.com), vinden van uw bot **Application Insights** resource. De naam heeft de meeste van de naam van de bot, en vervolgens willekeurige tekens aan het einde van de naam, zoals `luis-nodejs-bot-johnsmithxqowom`. 
1. Op de Application Insights-resource op de **overzicht** pagina-, Kopieer de **Instrumentatiesleutel**.
1. Open in VSCode, de **.env** bestand in de hoofdmap van het project bot. Dit bestand bevat alle omgevingsvariabelen.  
1. Voeg een nieuwe variabele `MicrosoftApplicationInsightsInstrumentationKey` met de waarde van de instrumentatiesleutel. Dit doet u er geen put de waarde tussen aanhalingstekens. 

## <a name="start-the-bot"></a>De bot starten

1. Start de bot in de geïntegreerde terminal VSCode:
    
    ```console
    npm start
    ```

1. Start de bot-emulator en opent de bot. Dit [stap](luis-nodejs-tutorial-bf-v4.md#use-the-bot-emulator-to-test-the-bot) is opgegeven in de vorige zelfstudie.

1. De bot een vraag stellen. Dit [stap](luis-nodejs-tutorial-bf-v4.md#ask-bot-a-question-for-the-book-flight-intent) is opgegeven in de vorige zelfstudie.

## <a name="view-luis-entries-in-application-insights"></a>LUIS-items weergeven in Application Insights

Open Application Insights om te zien van de LUIS-vermeldingen. Het kan enkele minuten voor de gegevens worden weergegeven in Application Insights duren.

1. In de [Azure-portal](https://portal.azure.com), opent u de Application Insights-resource van de bot. 
1. Wanneer de resource wordt geopend, selecteert u **zoeken** en zoek naar alle gegevens in de laatste **30 minuten** met het gebeurtenistype van **Trace**. Selecteer de tracering met de naam **LUIS**. 
1. De bot en LUIS informatie vindt u onder **aangepaste eigenschappen**. 

    ![LUIS aangepaste eigenschappen die zijn opgeslagen in Application Insights bekijken](./media/luis-tutorial-appinsights/application-insights-luis-trace-custom-properties-nodejs.png)

## <a name="query-application-insights-for-intent-score-and-utterance"></a>Application Insights-query voor het doel, score en utterance
Application Insights biedt u de power query uitvoeren op gegevens die met de [Kusto](https://docs.microsoft.com/azure/azure-monitor/log-query/log-query-overview#what-language-do-log-queries-use) taal, alsmede exporteren naar [Power BI](https://powerbi.microsoft.com). 

1. Selecteer **logboek (Analytics)** . Er wordt een nieuw venster geopend met een query aan de bovenkant en een venster van de tabel gegevens hieronder die. Als u databases hebt gebruikt, wordt deze rangschikking bekend is. De query geeft uw vorige gefilterde gegevens. De **CustomDimensions** kolom heeft de bot en LUIS-informatie.
1. Om op te halen van de belangrijkste doel, score en utterance, voeg de volgende net boven de laatste regel (de `|top...` regel) in het queryvenster:

    ```kusto
    | extend topIntent = tostring(customDimensions.LUIS_luisResponse_luisResult_topScoringIntent_intent)
    | extend score = todouble(customDimensions.LUIS_luisResponse_luisResult_topScoringIntent_score)
    | extend utterance = tostring(customDimensions.LUIS_luisResponse_text)
    ```

1. Voer de query uit. De nieuwe kolommen topIntent, score en utterance zijn beschikbaar. Selecteer topIntent kolom te sorteren.

Meer informatie over de [Kusto-querytaal](https://docs.microsoft.com/azure/log-analytics/query-language/get-started-queries) of [de gegevens exporteren naar Power BI](https://docs.microsoft.com/azure/application-insights/app-insights-export-power-bi). 

## <a name="next-steps"></a>Volgende stappen

Andere informatie die u mogelijk wilt toevoegen aan de application insights-gegevens omvatten app-ID, versie-ID, datum laatste model, laatste datum van de trein, de laatste publicatiedatum. Deze waarden kunnen worden opgehaald uit de eindpunt-URL (app-ID en versie-ID) of van een authoring API-aanroep en vervolgens ingesteld in de bot-instellingen van de web-app en van daaruit opgehaald.  

Als u van hetzelfde abonnement endpoint voor meer dan één LUIS-app gebruikmaakt, moet u ook de abonnements-ID en een eigenschap met de mededeling dat het een gedeelde sleutel opnemen. 

> [!div class="nextstepaction"]
> [Meer informatie over de voorbeeld-uitingen](luis-how-to-add-example-utterances.md)
