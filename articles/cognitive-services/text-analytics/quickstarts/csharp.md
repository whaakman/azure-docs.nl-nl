---
title: C#-Snelstartgids voor Cognitive Services, Text Analytics-API | Microsoft Docs
description: Verkrijg de informatie en voorbeelden van code kunt u snel aan de slag met het gebruik van de Tekstanalyse-API in Microsoft Cognitive Services op Azure.
services: cognitive-services
documentationcenter: ''
author: luiscabrer
ms.service: cognitive-services
ms.component: text-analytics
ms.topic: article
ms.date: 09/20/2017
ms.author: ashmaka
ms.openlocfilehash: 4bf5179ade6f49b847b8b674d33652071e19a769
ms.sourcegitcommit: 8ebcecb837bbfb989728e4667d74e42f7a3a9352
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/21/2018
ms.locfileid: "41987988"
---
# <a name="quickstart-for-the-text-analytics-api-with-c"></a>Snelstartgids voor de Tekstanalyse-API met C# 
<a name="HOLTop"></a>

Dit artikel leest u hoe u taal detecteren, sentiment analyseren en belangrijke woordgroepen met behulp van de [Tekstanalyse-API's](//go.microsoft.com/fwlink/?LinkID=759711) met C#. De code is geschreven om te werken op een .NET Core-toepassing, met minimale verwijzingen naar externe bibliotheken, dus u het ook in Linux of MacOS uitvoeren kunt.

Raadpleeg de [API-definities](//go.microsoft.com/fwlink/?LinkID=759346) voor technische documentatie voor de API's.

## <a name="prerequisites"></a>Vereisten

Hebt u een [Cognitive Services-API-account](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) met de Tekstanalyse-API. U kunt de *gratis laag voor 5000 transacties per maand* om uit te voeren van deze Quick Start.

Ook moet u de [eindpunt en de toegangssleutel](../How-tos/text-analytics-how-to-access-key.md) die voor u zijn gegenereerd tijdens de registratie. 


## <a name="install-the-nuget-sdk-package"></a>Installeer de SDK NuGet-pakket
1. Maak een nieuwe console-oplossing in Visual Studio.
1. Met de rechtermuisknop op de oplossing en selecteer **NuGet-pakketten beheren voor oplossing**.
1. Selecteer de **voorlopige versie opnemen** selectievakje.
1. Selecteer de **Bladeren** tabblad en zoek naar de **Microsoft.Azure.CognitiveServices.Language**.
1. Selecteer het NuGet-pakket en installeer deze.

> [!Tip]
> Hoewel u kunt aanroepen de [HTTP-eindpunten](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c6) rechtstreeks via C#, de SDK Microsoft.Azure.CognitiveServices.Language veel gemakkelijker om aan te roepen van de service zonder te hoeven maken over het serialiseren en deserialiseren van JSON.
>
> Hier vindt u nuttige koppelingen:
> - [SDK NuGet-pagina](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Language.TextAnalytics)
> - [SDK-code](https://github.com/Azure/azure-sdk-for-net/tree/psSdkJson6/src/SDKs/CognitiveServices/dataPlane/Language/TextAnalytics)


## <a name="call-the-text-analytics-api-by-using-the-sdk"></a>De Tekstanalyse-API aanroepen met behulp van de SDK
1. Vervang Program.cs door de volgende code. Dit programma ziet u de mogelijkheden van de Tekstanalyse-API in drie secties (taal uitpakken, sleutel vindt er sleuteltermextractie plaats en sentimentanalyse).
1. Vervang de `Ocp-Apim-Subscription-Key` headerwaarde met een toegangssleutel die geldig is voor uw abonnement.
1. Vervang de locatie in `Endpoint` naar het eindpunt dat u zich heeft aangemeld. U vindt het eindpunt op de Azure portal-resource. Het eindpunt doorgaans begint met "https://[region].api.cognitive.microsoft.com." Alleen de naam protocol en de host bevatten.
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
            ITextAnalyticsClient client = new TextAnalyticsClient(new ApiKeyServiceClientCredentials())
            {
                Endpoint = "https://westus.api.cognitive.microsoft.com"
            };

            Console.OutputEncoding = System.Text.Encoding.UTF8;

            // Extracting language.
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

            // Getting key phrases.
            Console.WriteLine("\n\n===== KEY-PHRASE EXTRACTION ======");

            KeyPhraseBatchResult result2 = client.KeyPhrasesAsync(new MultiLanguageBatchInput(
                        new List<MultiLanguageInput>()
                        {
                          new MultiLanguageInput("ja", "1", "猫は幸せ"),
                          new MultiLanguageInput("de", "2", "Fahrt nach Stuttgart und dann zum Hotel zu Fu."),
                          new MultiLanguageInput("en", "3", "My cat is stiff as a rock."),
                          new MultiLanguageInput("es", "4", "A mi me encanta el fútbol!")
                        })).Result;

            // Printing key phrases.
            foreach (var document in result2.Documents)
            {
                Console.WriteLine("Document ID: {0} ", document.Id);

                Console.WriteLine("\t Key phrases:");

                foreach (string keyphrase in document.KeyPhrases)
                {
                    Console.WriteLine("\t\t" + keyphrase);
                }
            }

            // Extracting sentiment.
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


            // Printing sentiment results.
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
> [Text Analytics met Power BI](../tutorials/tutorial-power-bi-key-phrases.md)

## <a name="see-also"></a>Zie ook 

 [Text Analytics-overzicht](../overview.md)  
 [Veelgestelde vragen](../text-analytics-resource-faq.md)

