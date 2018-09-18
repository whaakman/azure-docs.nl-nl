---
title: LUIS en QnAMaker - Bot-integratie
titleSuffix: Azure Cognitive Services
description: Een stapsgewijze zelfstudie over QnA Maker en LUIS integreren in een bot.
services: cognitive-services
author: nstulasi
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 09/12/2018
ms.author: nstulasi
ms.openlocfilehash: cfadaa836eef0763c9dd56c71d177995ab658c3e
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/17/2018
ms.locfileid: "45736033"
---
# <a name="integrate-qna-maker-and-luis-to-distribute-your-knowledge-base"></a>QnA Maker en LUIS voor het distribueren van uw knowledge base integreren
Als uw QnA Maker knowledge base grote groeit, wordt het moeilijk te onderhouden als één monolithische ingesteld en er behoefte aan de knowledge base splitsen in kleinere logische segmenten.

Het is eenvoudig te maken van meerdere knowledge bases in QnA Maker, moet u bepaalde logica voor het routeren van de binnenkomende vraag naar de juiste knowledge base. U kunt dit doen met behulp van LUIS.

## <a name="architecture"></a>Architectuur

![QnA Maker luis-architectuur](../media/qnamaker-tutorials-qna-luis/qnamaker-luis-architecture.PNG)

In dit scenario, QnA Maker eerst de bedoeling van de binnenkomende vraag opgehaald uit een LUIS-model, en gebruik vervolgens die aan deze wordt doorgestuurd naar de juiste QnA Maker knowledge base.

## <a name="prerequisites"></a>Vereisten
- Meld u aan bij de [LUIS](https://www.luis.ai/) portal en [maken van een app](https://docs.microsoft.com/azure/cognitive-services/luis/create-new-app).
- [Intents toevoegen](https://docs.microsoft.com/azure/cognitive-services/luis/add-intents) aan de hand van uw scenario.
- [Train](https://docs.microsoft.com/azure/cognitive-services/luis/luis-how-to-train) en [publiceren](https://docs.microsoft.com/azure/cognitive-services/luis/publishapp) uw LUIS-App.
- Meld u aan bij [QnA Maker](https://qnamaker.ai) en [maken](https://www.qnamaker.ai/Create) knowledge bases aan de hand van uw scenario.
- Testen en publiceren van de knowledge bases.

## <a name="qna-maker--luis-bot"></a>QnA Maker + LUIS-Bot
1. Eerst een bot-Web-App maken met de sjabloon LUIS, een koppeling met de LUIS-app die u hierboven hebt gemaakt en de intenties wijzigen. Zie de gedetailleerde stappen [hier](https://docs.microsoft.com/azure/cognitive-services/luis/luis-csharp-tutorial-build-bot-framework-sample).

2. Afhankelijkheden toevoegen aan het begin van het bestand, met de andere afhankelijkheden:

    ```
    using RestSharp;
    using System.Collections.Generic;
    using Newtonsoft.Json;
    ```
3. Voeg de volgende klasse voor het aanroepen van de QnA Maker-service:

    ```
        /// <summary>
        /// QnAMakerService is a wrapper over the QnA Maker REST endpoint
        /// </summary>
        [Serializable]
        public class QnAMakerService
        {
            private string qnaServiceHostName;
            private string knowledgeBaseId;
            private string endpointKey;
    
            /// <summary>
            /// Initialize a particular endpoint with it's details
            /// </summary>
            /// <param name="hostName">Hostname of the endpoint</param>
            /// <param name="kbId">Knowledge base ID</param>
            /// <param name="ek">Endpoint Key</param>
            public QnAMakerService(string hostName, string kbId, string ek)
            {
                qnaServiceHostName = hostName;
                knowledgeBaseId = kbId;
                endpointKey = ek;
            }
    
            /// <summary>
            /// Call the QnA Maker endpoint and get a response
            /// </summary>
            /// <param name="query">User question</param>
            /// <returns></returns>
            public string GetAnswer(string query)
            {
                var client = new RestClient( qnaServiceHostName + "/qnamaker/knowledgebases/" + knowledgeBaseId + "/generateAnswer");
                var request = new RestRequest(Method.POST);
                request.AddHeader("authorization", "EndpointKey " + endpointKey);
                request.AddHeader("content-type", "application/json");
                request.AddParameter("application/json", "{\"question\": \"" + query + "\"}", ParameterType.RequestBody);
                IRestResponse response = client.Execute(request);
    
                // Deserialize the response JSON
                QnAAnswer answer = JsonConvert.DeserializeObject<QnAAnswer>(response.Content);
    
                // Return the answer if present
                if (answer.answers.Count > 0)
                    return answer.answers[0].answer;
                else
                    return "No good match found.";
            }
        }
    
        /* START - QnA Maker Response Class */
        public class Metadata
        {
            public string name { get; set; }
            public string value { get; set; }
        }
    
        public class Answer
        {
            public IList<string> questions { get; set; }
            public string answer { get; set; }
            public double score { get; set; }
            public int id { get; set; }
            public string source { get; set; }
            public IList<object> keywords { get; set; }
            public IList<Metadata> metadata { get; set; }
        }
    
        public class QnAAnswer
        {
            public IList<Answer> answers { get; set; }
        }
        /* END - QnA Maker Response Class */
    ```

3. Ga naar https://qnamaker.ai -> Mijn knowledge bases -> de code weergeven van de bijbehorende knowledge base. Haal de volgende informatie:

    ![QnA Maker HTTP-aanvraag](../media/qnamaker-tutorials-qna-luis/qnamaker-http-request.png)

4. Exemplaar maken van de klasse QnAMakerService voor elk van uw knowledge bases:
    ```
            // Instantiate the knowledge bases
            public QnAMakerService hrQnAService = new QnAMakerService("https://hrkb.azurewebsites.net", "<HR knowledge base id>", "<HR endpoint key>");
            public QnAMakerService payrollQnAService = new QnAMakerService("https://payrollkb.azurewebsites.net", "<Payroll knowledge base id>", "<Payroll endpoint key>");
            public QnAMakerService financeQnAService = new QnAMakerService("https://financekb.azurewebsites.net", "<Finance knowledge base id>", "<Finance endpoint key>");
    ```

5. Het betreffende knowledge base-aanroep voor het doel.
    ```
            // HR Intent
            [LuisIntent("HR")]
            public async Task CancelIntent(IDialogContext context, LuisResult result)
            {
                // Ask the HR knowledge base
                await context.PostAsync(hrQnAService.GetAnswer(result.Query));
            }
    
            // Payroll intent
            [LuisIntent("Payroll")]
            public async Task GreetingIntent(IDialogContext context, LuisResult result)
            {
                // Ask the payroll knowledge base
                await context.PostAsync(payrollQnAService.GetAnswer(result.Query));
            }
    
            // Finance intent
            [LuisIntent("Finance")]
            public async Task HelpIntent(IDialogContext context, LuisResult result)
            {
                // Ask the finance knowledge base
                await context.PostAsync(financeQnAService.GetAnswer(result.Query));
            }
    ```

## <a name="build-the-bot"></a>De bot bouwen
1. In de code-editor met de rechtermuisknop op `build.cmd` en selecteer **uitvoeren vanaf de Console**.

    ![uitvoeren vanaf de console](../media/qnamaker-tutorials-qna-luis/run-from-console.png)

2. De weergave van de code wordt vervangen door een terminalvenster waarin de voortgang en resultaten van de build.

    ![console-build](../media/qnamaker-tutorials-qna-luis/console-build.png)

## <a name="test-the-bot"></a>De bot testen
Selecteer in de Azure portal, **testen in Web Chat** voor het testen van de bot. Typ berichten van verschillende intents om op te halen van het antwoord van het betreffende knowledge base.

![chat WebTest](../media/qnamaker-tutorials-qna-luis/qnamaker-web-chat.png)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Maak een plan voor bedrijfscontinuïteit voor QnA Maker](../How-To/business-continuity-plan.md)
