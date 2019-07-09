---
title: Application Insights, C#
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
ms.openlocfilehash: 720352403fd5f5937669f9838f3974cb0d3f8797
ms.sourcegitcommit: cf438e4b4e351b64fd0320bf17cc02489e61406a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/08/2019
ms.locfileid: "67657776"
---
# <a name="add-luis-results-to-application-insights-from-a-bot-in-c"></a>LUIS resultaten naar Application Insights toevoegen van een Bot inC#

In deze zelfstudie voegt bot en Language Understanding informatie [Application Insights](https://azure.microsoft.com/services/application-insights/) telemetrie-gegevensopslag. Zodra u deze gegevens hebt, kunt u deze met de Kusto-taal of Power BI analyseren, samenvoegen, opvragen en te rapporteren intenties en entiteiten van de utterance in realtime. Met deze analyse kunt u bepalen of u moet toevoegen of bewerken van de intenties en entiteiten van uw LUIS-app.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Bot en Language understanding-gegevens in Application Insights vastleggen
> * Application Insights op te vragen voor Language Understanding-gegevens

## <a name="prerequisites"></a>Vereisten

* Een Azure bot service-bot, die zijn gemaakt met Application Insights is ingeschakeld.
* Bot-code van de vorige bot gedownload  **[zelfstudie](luis-csharp-tutorial-bf-v4.md)** . 
* [Botemulator](https://aka.ms/abs/build/emulatordownload)
* [Visual Studio Code](https://code.visualstudio.com/Download)

Alle code in deze zelfstudie vindt u op de [Azure-Samples Language Understanding GitHub-opslagplaats](https://github.com/Azure-Samples/cognitive-services-language-understanding/tree/master/documentation-samples/tutorial-web-app-bot-application-insights/v4/luis-csharp-bot-johnsmith-src-telemetry). 

## <a name="add-application-insights-to-web-app-bot-project"></a>Application Insights toevoegen aan de web-app-bot-project

Op dit moment verzamelt de Application Insights-service, die wordt gebruikt in deze web-app-bot, telemetrie van de algemene status voor de bot. Deze verzamelt geen informatie van LUIS. 

Als u wilt de LUIS-gegevens vastleggen, de web-app-bot moet de **[Microsoft.ApplicationInsights](https://www.nuget.org/packages/Microsoft.ApplicationInsights/)** NuGet-pakket geïnstalleerd en geconfigureerd.  

1. Vanuit Visual Studio, voeg de afhankelijkheden toe aan de oplossing. In de **Solution Explorer**, met de rechtermuisknop op de projectnaam en selecteer **NuGet-pakketten beheren...** . De NuGet Package manager geeft een lijst van geïnstalleerde pakketten. 
1. Selecteer **Bladeren** Zoek **Microsoft.ApplicationInsights**.
1. Installeer het pakket. 

## <a name="capture-and-send-luis-query-results-to-application-insights"></a>Vastleggen en LUIS queryresultaten verzenden naar Application Insights

1. Open de `LuisHelper.cs` -bestand en vervang de inhoud door de volgende code. De **LogToApplicationInsights** methode de bot en LUIS gegevens vastleggen en verzendt dit naar Application Insights als een traceringsgebeurtenis met de naam `LUIS`.

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
    using Microsoft.ApplicationInsights;
    using System.Collections.Generic;
    
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
    
                    LuisHelper.LogToApplicationInsights(configuration, turnContext, recognizerResult);
    
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
            public static void LogToApplicationInsights(IConfiguration configuration, ITurnContext turnContext, RecognizerResult result)
            {
                // Create Application Insights object
                TelemetryClient telemetry = new TelemetryClient();
    
                // Set Application Insights Instrumentation Key from App Settings
                telemetry.InstrumentationKey = configuration["BotDevAppInsightsKey"];
    
                // Collect information to send to Application Insights
                Dictionary<string, string> logProperties = new Dictionary<string, string>();
    
                logProperties.Add("BotConversation", turnContext.Activity.Conversation.Name);
                logProperties.Add("Bot_userId", turnContext.Activity.Conversation.Id);
    
                logProperties.Add("LUIS_query", result.Text);
                logProperties.Add("LUIS_topScoringIntent_Name", result.GetTopScoringIntent().intent);
                logProperties.Add("LUIS_topScoringIntentScore", result.GetTopScoringIntent().score.ToString());
    
    
                // Add entities to collected information
                int i = 1;
                if (result.Entities.Count > 0)
                {
                    foreach (var item in result.Entities)
                    {
                        logProperties.Add("LUIS_entities_" + i++ + "_" + item.Key, item.Value.ToString());
                    }
                }
    
                // Send to Application Insights
                telemetry.TrackTrace("LUIS", ApplicationInsights.DataContracts.SeverityLevel.Information, logProperties);
    
            }
        }
    }
    ```

## <a name="add-application-insights-instrumentation-key"></a>Application Insights-instrumentatiesleutel toevoegen 

Gegevens naar application insights toevoegt, moet u de instrumentatiesleutel.

1. In een browser, in de [Azure-portal](https://portal.azure.com), vinden van uw bot **Application Insights** resource. De naam heeft de meeste van de naam van de bot, en vervolgens willekeurige tekens aan het einde van de naam, zoals `luis-csharp-bot-johnsmithxqowom`. 
1. Op de Application Insights-resource op de **overzicht** pagina-, Kopieer de **Instrumentatiesleutel**.
1. Open in Visual Studio, de **appsettings.json** bestand in de hoofdmap van het project bot. Dit bestand bevat alle omgevingsvariabelen.
1. Voeg een nieuwe variabele `BotDevAppInsightsKey` met de waarde van de instrumentatiesleutel. De waarde in moet worden opgegeven tussen aanhalingstekens. 

## <a name="build-and-start-the-bot"></a>Bouwen en de bot te starten

1. In Visual Studio, bouwen en uitvoeren van de bot. 
1. Start de bot-emulator en opent de bot. Dit [stap](luis-csharp-tutorial-bf-v4.md#use-the-bot-emulator-to-test-the-bot) is opgegeven in de vorige zelfstudie.

1. De bot een vraag stellen. Dit [stap](luis-csharp-tutorial-bf-v4.md#ask-bot-a-question-for-the-book-flight-intent) is opgegeven in de vorige zelfstudie.

## <a name="view-luis-entries-in-application-insights"></a>LUIS-items weergeven in Application Insights

Open Application Insights om te zien van de LUIS-vermeldingen. Het kan enkele minuten voor de gegevens worden weergegeven in Application Insights duren.

1. In de [Azure-portal](https://portal.azure.com), opent u de Application Insights-resource van de bot. 
1. Wanneer de resource wordt geopend, selecteert u **zoeken** en zoek naar alle gegevens in de laatste **30 minuten** met het gebeurtenistype van **Trace**. Selecteer de tracering met de naam **LUIS**. 
1. De bot en LUIS informatie vindt u onder **aangepaste eigenschappen**. 

    ![LUIS aangepaste eigenschappen die zijn opgeslagen in Application Insights bekijken](./media/luis-tutorial-appinsights/application-insights-luis-trace-custom-properties-csharp.png)

## <a name="query-application-insights-for-intent-score-and-utterance"></a>Application Insights-query voor het doel, score en utterance
Application Insights biedt u de power query uitvoeren op gegevens die met de [Kusto](https://docs.microsoft.com/azure/azure-monitor/log-query/log-query-overview#what-language-do-log-queries-use) taal, alsmede exporteren naar [Power BI](https://powerbi.microsoft.com). 

1. Selecteer **logboek (Analytics)** . Er wordt een nieuw venster geopend met een query aan de bovenkant en een venster van de tabel gegevens hieronder die. Als u databases hebt gebruikt, wordt deze rangschikking bekend is. De query geeft uw vorige gefilterde gegevens. De **CustomDimensions** kolom heeft de bot en LUIS-informatie.
1. Om op te halen van de belangrijkste doel, score en utterance, voeg de volgende net boven de laatste regel (de `|top...` regel) in het queryvenster:

    ```kusto
    | extend topIntent = tostring(customDimensions.LUIS_topScoringIntent_Name)
    | extend score = todouble(customDimensions.LUIS_topScoringIntentScore)
    | extend utterance = tostring(customDimensions.LUIS_query)
    ```

1. Voer de query uit. De nieuwe kolommen topIntent, score en utterance zijn beschikbaar. Selecteer topIntent kolom te sorteren.

Meer informatie over de [Kusto-querytaal](https://docs.microsoft.com/azure/log-analytics/query-language/get-started-queries) of [de gegevens exporteren naar Power BI](https://docs.microsoft.com/azure/application-insights/app-insights-export-power-bi). 


## <a name="learn-more-about-bot-framework"></a>Meer informatie over Bot Framework

Meer informatie over [Bot Framework](https://dev.botframework.com/).

## <a name="next-steps"></a>Volgende stappen

Andere informatie die u mogelijk wilt toevoegen aan de application insights-gegevens omvatten app-ID, versie-ID, datum laatste model, laatste datum van de trein, de laatste publicatiedatum. Deze waarden kunnen worden opgehaald uit de eindpunt-URL (app-ID en versie-ID) of van een authoring API-aanroep en vervolgens ingesteld in de bot-instellingen van de web-app en van daaruit opgehaald.  

Als u van hetzelfde abonnement endpoint voor meer dan één LUIS-app gebruikmaakt, moet u ook de abonnements-ID en een eigenschap met de mededeling dat het een gedeelde sleutel opnemen.

> [!div class="nextstepaction"]
> [Meer informatie over de voorbeeld-uitingen](luis-how-to-add-example-utterances.md)
