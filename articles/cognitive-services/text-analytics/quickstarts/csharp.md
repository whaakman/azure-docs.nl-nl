---
title: C#-Snelstartgids voor Azure cognitieve Services, Text Analytics API | Microsoft Docs
description: Get-informatie en codevoorbeelden kunt u snel aan de slag met de tekst Analytics-API in Microsoft cognitieve Services in Azure.
services: cognitive-services
documentationcenter: ''
author: luiscabrer
ms.service: cognitive-services
ms.component: text-analytics
ms.topic: article
ms.date: 09/20/2017
ms.author: ashmaka
ms.openlocfilehash: f46c5179fc245d84a72e038fe3870d2e6c990550
ms.sourcegitcommit: 4f9fa86166b50e86cf089f31d85e16155b60559f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2018
ms.locfileid: "35345957"
---
# <a name="quickstart-for-text-analytics-api-with-c"></a>Snelstartgids voor Tekstanalyse API met C# 
<a name="HOLTop"></a>

Dit artikel laat zien hoe taal wordt gedetecteerd, gevoel analyseren en uitpakken van sleutel woordgroepen met behulp van de [Text Analytics-API's](//go.microsoft.com/fwlink/?LinkID=759711) met C#. De code is geschreven om te werken op een .net Core toepassing, met minimale verwijzingen naar externe bibliotheken, zodat u het ook op Linux- of Mac OS uitvoeren kan.

Raadpleeg de [API-definities](//go.microsoft.com/fwlink/?LinkID=759346) voor technische documentatie voor de API's.

## <a name="prerequisites"></a>Vereisten

U moet hebben een [cognitieve Services API account](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) met **Text Analytics API**. U kunt de **gratis laag voor 5000 transacties/maand** voltooien van deze snelstartgids.

U moet ook beschikken over de [eindpunt en de toegangssleutel](../How-tos/text-analytics-how-to-access-key.md) die is gegenereerd voor u tijdens de aanmelding van. 


## <a name="install-the-nuget-sdk-package"></a>Installeer het Nuget-SDK-pakket
1. Maak een nieuwe Console-oplossing in Visual Studio.
1. Klik met de rechtermuisknop op de oplossing en klik op **NuGet-pakketten beheren voor oplossing**
1. Markeer de **omvatten Prerelease** selectievakje.
1. Selecteer de **Bladeren** tabblad en zoek naar **Microsoft.Azure.CognitiveServices.Language**
1. Het Nuget-pakket te selecteren en te installeren.

> [!Tip]
>  Terwijl u de [HTTP-eindpunten](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c6) rechtstreeks vanuit C#, de SDK Microsoft.Azure.CognitiveServices.Language veel gemakkelijker de service aanroepen zonder te hoeven maken over serialiseren en deserialiseren van JSON.
>
> Enkele nuttige koppelingen:
> - [SDK Nuget-pagina](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Language)
> - [SDK-code ](https://github.com/Azure/azure-sdk-for-net/tree/psSdkJson6/src/SDKs/CognitiveServices/dataPlane/Language/Language)


## <a name="call-the-text-analytics-api-using-the-sdk"></a>De tekst Analytics API met behulp van de SDK niet aanroepen
1. Program.cs vervangen door de code hieronder. Dit programma ziet u de mogelijkheden van de tekst Analytics API in de 3 punten (taal extractie, uitpakken van sleutel-zin en gevoel analyse).
1. Vervang de `Ocp-Apim-Subscription-Key` headerwaarde met een geldige toegangssleutel voor uw abonnement.
1. Vervang de locatie in `client.AzureRegion` (momenteel `AzureRegions.Westus`) voor de regio die u zich heeft aangemeld.
1. Voer het programma.

```csharp
using System;
using Microsoft.Azure.CognitiveServices.Language.TextAnalytics;
using Microsoft.Azure.CognitiveServices.Language.TextAnalytics.Models;
using System.Collections.Generic;
using Microsoft.Rest;
using System.Net.Http;
using System.Threading;
using System.Threading.Tasks;

namespace ConsoleApp1
{
    class Program
    {
        /// <summary>
        /// Container for subscription credentials. Make sure to enter your valid key.
        /// </summary>
        class ApiKeyServiceClientCredentials : ServiceClientCredentials
        {
            public override Task ProcessHttpRequestAsync(HttpRequestMessage request, CancellationToken cancellationToken)
            {
                request.Headers.Add("Ocp-Apim-Subscription-Key", "ENTER KEY HERE");
                return base.ProcessHttpRequestAsync(request, cancellationToken);
            }
        }

        static void Main(string[] args)
        {

            // Create a client.
            ITextAnalyticsAPI client = new TextAnalyticsAPI(new ApiKeyServiceClientCredentials());
            client.AzureRegion = AzureRegions.Westus;

            Console.OutputEncoding = System.Text.Encoding.UTF8;

            // Extracting language
            Console.WriteLine("===== LANGUAGE EXTRACTION ======");

            var result =  client.DetectLanguageAsync(new BatchInput(
                    new List<Input>()
                        {
                          new Input("1", "This is a document written in English."),
                          new Input("2", "Este es un document escrito en Español."),
                          new Input("3", "这是一个用中文写的文件")
                    })).Result;

            // Printing language results.
            foreach (var document in result.Documents)
            {
                Console.WriteLine("Document ID: {0} , Language: {1}", document.Id, document.DetectedLanguages[0].Name);
            }

            // Getting key-phrases
            Console.WriteLine("\n\n===== KEY-PHRASE EXTRACTION ======");

            KeyPhraseBatchResult result2 = client.KeyPhrasesAsync(new MultiLanguageBatchInput(
                        new List<MultiLanguageInput>()
                        {
                          new MultiLanguageInput("ja", "1", "猫は幸せ"),
                          new MultiLanguageInput("de", "2", "Fahrt nach Stuttgart und dann zum Hotel zu Fu."),
                          new MultiLanguageInput("en", "3", "My cat is stiff as a rock."),
                          new MultiLanguageInput("es", "4", "A mi me encanta el fútbol!")
                        })).Result;

            // Printing keyphrases
            foreach (var document in result2.Documents)
            {
                Console.WriteLine("Document ID: {0} ", document.Id);

                Console.WriteLine("\t Key phrases:");

                foreach (string keyphrase in document.KeyPhrases)
                {
                    Console.WriteLine("\t\t" + keyphrase);
                }
            }

            // Extracting sentiment
            Console.WriteLine("\n\n===== SENTIMENT ANALYSIS ======");

            SentimentBatchResult result3 = client.SentimentAsync(
                    new MultiLanguageBatchInput(
                        new List<MultiLanguageInput>()
                        {
                          new MultiLanguageInput("en", "0", "I had the best day of my life."),
                          new MultiLanguageInput("en", "1", "This was a waste of my time. The speaker put me to sleep."),
                          new MultiLanguageInput("es", "2", "No tengo dinero ni nada que dar..."),
                          new MultiLanguageInput("it", "3", "L'hotel veneziano era meraviglioso. È un bellissimo pezzo di architettura."),
                        })).Result;


            // Printing sentiment results
            foreach (var document in result3.Documents)
            {
                Console.WriteLine("Document ID: {0} , Sentiment Score: {1:0.00}", document.Id, document.Score);
            }
        }
    }
}
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Tekstanalyse met Power BI](../tutorials/tutorial-power-bi-key-phrases.md)

## <a name="see-also"></a>Zie ook 

 [Overzicht van tekst Analytics](../overview.md)  
 [Veelgestelde vragen (FAQ)](../text-analytics-resource-faq.md)

