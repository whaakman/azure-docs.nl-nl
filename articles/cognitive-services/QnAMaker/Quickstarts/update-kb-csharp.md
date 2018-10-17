---
title: 'Quickstart: Knowledge base bijwerken - REST, C# - QnA Maker'
titleSuffix: Azure Cognitive Services
description: In deze quickstart wordt beschreven hoe u uw voorbeeld van een QnA Maker-knowledge base (KB) programmatisch kunt bijwerken. Met de JSON-definitie voor het bijwerken van een KB kunt u paren van vragen en antwoorden toevoegen, wijzigen of verwijderen.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: quickstart
ms.date: 10/01/2018
ms.author: diberry
ms.openlocfilehash: 9fa57a02f11461a3ea9a08b7b1b101b143924037
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/05/2018
ms.locfileid: "48801690"
---
# <a name="quickstart-update-a-qna-maker-knowledge-base-in-c"></a>Quickstart: Een QnA Maker-knowledge base bijwerken in C#

Deze quickstart helpt u bij het programmatisch bijwerken van een bestaande QnA Maker-knowledge base (KB).  Met deze JSON kunt u een KB bijwerken door nieuwe gegevensbronnen toe te voegen, gegevensbronnen te wijzigen of gegevensbronnen te verwijderen.

Deze API is gelijk aan bewerken, waarna u de knop **Save and train** in de QnA Maker-portal gebruikt.

In deze snelstart worden QnA Maker-API's aangeroepen:
* [Update](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da7600): het model voor de knowledge base is gedefinieerd in de JSON die in de hoofdtekst van de API-aanvraag wordt verzonden. 
* [Bewerkingsdetails ophalen](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/operations_getoperationdetails)

## <a name="prerequisites"></a>Vereisten

* De meest recente [**Visual Studio Community edition**](https://www.visualstudio.com/downloads/).
* U moet een [QnA Maker-service ](../How-To/set-up-qnamaker-service-azure.md) hebben. Selecteer onder **Resourcebeheer** de optie **Sleutel** om uw sleutel op te halen.
* Id voor knowledge base (KB) in QnA Maker gevonden in de URL in de parameter voor de kbid-queryreeks zoals hieronder wordt weergegeven.

    ![Id voor knowledge base in QnA Maker](../media/qnamaker-quickstart-kb/qna-maker-id.png)

Als u nog geen knowledge base hebt, kunt u een voorbeeldexemplaar maken om te gebruiken met deze snelstart: [Een nieuwe knowledge base maken](create-new-kb-csharp.md).

[!INCLUDE [Code is available in Azure-Samples Github repo](../../../../includes/cognitive-services-qnamaker-csharp-repo-note.md)]

## <a name="create-knowledge-base-project"></a>Een project met knowledge base maken

[!INCLUDE [Create Visual Studio Project](../../../../includes/cognitive-services-qnamaker-quickstart-csharp-create-project.md)] 

## <a name="add-required-dependencies"></a>Vereiste afhankelijkheden toevoegen

[!INCLUDE [Add required dependencies to code file](../../../../includes/cognitive-services-qnamaker-quickstart-csharp-required-dependencies.md)] 

## <a name="add-required-constants"></a>Vereiste constanten toevoegen

[!INCLUDE [Add required constants to code file](../../../../includes/cognitive-services-qnamaker-quickstart-csharp-required-constants.md)] 

## <a name="add-knowledge-base-id"></a>Knowledge Base-id toevoegen

[!INCLUDE [Add knowledge base ID as constant](../../../../includes/cognitive-services-qnamaker-quickstart-csharp-kb-id.md)] 

## <a name="add-the-kb-update-definition"></a>De definitie van de KB-update toevoegen

Voeg na de constanten de volgende KB-updatedefinitie toe. De updatedefinitie heeft drie secties:

* add
* update
* delete

Elke sectie kan worden gebruikt in een en dezelfde aanvraag voor de API. 

```csharp
static string new_kb = @"
{
    'add': {
        'qnaList': [
            {
            'id': 1,
            'answer': 'You can change the default message if you use the QnAMakerDialog. See this for details: https://docs.botframework.com/en-us/azure-bot-service/templates/qnamaker/#navtitle',
            'source': 'Custom Editorial',
            'questions': [
                'How can I change the default message from QnA Maker?'
            ],
            'metadata': []
            }
        ],
        'urls': []
    },
    'update' : {
        'name' : 'QnA Maker FAQ from quickstart - updated'
    },
    'delete': {
        'ids': [
            0
        ]
    }
}
";
```

## <a name="add-supporting-functions-and-structures"></a>Ondersteunende functies en structuren toevoegen

[!INCLUDE [Add supporting functions and structures](../../../../includes/cognitive-services-qnamaker-quickstart-csharp-support-functions.md)] 

## <a name="add-patch-request-to-update-kb"></a>PATCH-aanvraag toevoegen om KB bij te werken

De volgende code maakt een HTTPS-aanvraag naar de QnA Maker-API om vraag- en antwoordgroepen in een KB bij te werken en ontvangt het antwoord:

```csharp
async static Task<Response> PatchUpdateKB(string kb, string new_kb)
{
    string uri = host + service + method + kb;
    Console.WriteLine("Calling " + uri + ".");

    using (var client = new HttpClient())
    using (var request = new HttpRequestMessage())
    {
        request.Method = new HttpMethod("PATCH");
        request.RequestUri = new Uri(uri);

        request.Content = new StringContent(new_kb, Encoding.UTF8, "application/json");
        request.Headers.Add("Ocp-Apim-Subscription-Key", key);

        var response = await client.SendAsync(request);
        var responseBody = await response.Content.ReadAsStringAsync();
        return new Response(response.Headers, responseBody);
    }
}
```

## <a name="add-get-request-to-determine-creation-status"></a>GET-aanvraag toevoegen om de maakstatus te bepalen

Met de update van een KB worden vraag- en antwoordgroepen toegevoegd, bijgewerkt en verwijderd. 

```csharp
async static Task<Response> GetStatus(string operation)
{
    string uri = host + service + operation;
    Console.WriteLine("Calling " + uri + ".");

    using (var client = new HttpClient())
    using (var request = new HttpRequestMessage())
    {
        request.Method = HttpMethod.Get;
        request.RequestUri = new Uri(uri);
        request.Headers.Add("Ocp-Apim-Subscription-Key", key);

        var response = await client.SendAsync(request);
        var responseBody = await response.Content.ReadAsStringAsync();
        return new Response(response.Headers, responseBody);
    }
}
```

Deze API-aanroep retourneert een JSON-antwoord dat de bewerkingsstatus bevat: 

```JSON
{
  "operationState": "NotStarted",
  "createdTimestamp": "2018-09-26T05:22:53Z",
  "lastActionTimestamp": "2018-09-26T05:22:53Z",
  "userId": "XXX9549466094e1cb4fd063b646e1ad6",
  "operationId": "177e12ff-5d04-4b73-b594-8575f9787963"
}
```

Herhaal de aanroep totdat deze lukt of mislukt: 

```JSON
{
  "operationState": "Succeeded",
  "createdTimestamp": "2018-09-26T05:22:53Z",
  "lastActionTimestamp": "2018-09-26T05:23:08Z",
  "resourceLocation": "/knowledgebases/XXX7892b-10cf-47e2-a3ae-e40683adb714",
  "userId": "XXX9549466094e1cb4fd063b646e1ad6",
  "operationId": "177e12ff-5d04-4b73-b594-8575f9787963"
}
```

## <a name="add-updatekb-method"></a>UpdateKB-methode toevoegen
Met de volgende methode wordt de KB bijgewerkt en worden controles van de status herhaald. Omdat het maken van de KB enige tijd kan duren, moet u aanroepen herhalen om de status te controleren totdat de status geslaagd of mislukt is.

```csharp
async static void UpdateKB(string kb, string new_kb)
{
    try
    {
        // Starts the QnA Maker operation to update the knowledge base.
        var response = await PatchUpdateKB(kb, new_kb);

        // Retrieves the operation ID, so the operation's status can be
        // checked periodically.
        var operation = response.headers.GetValues("Location").First();

        // Displays the JSON in the HTTP response returned by the 
        // PostUpdateKB(string, string) method.
        Console.WriteLine(PrettyPrint(response.response));

        // Iteratively gets the state of the operation updating the
        // knowledge base. Once the operation state is something other
        // than "Running" or "NotStarted", the loop ends.
        var done = false;
        while (true != done)
        {
            // Gets the status of the operation.
            response = await GetStatus(operation);
            // Displays the JSON in the HTTP response returned by the
            // GetStatus(string) method.
            Console.WriteLine(PrettyPrint(response.response));

            // Deserialize the JSON into key-value pairs, to retrieve the
            // state of the operation.
            var fields = JsonConvert.DeserializeObject<Dictionary<string, string>>(response.response);

            // Gets and checks the state of the operation.
            String state = fields["operationState"];
            if (state.CompareTo("Running") == 0 || state.CompareTo("NotStarted") == 0)
            {
                // QnA Maker is still updating the knowledge base. The thread is
                // paused for a number of seconds equal to the Retry-After
                // header value, and then the loop continues.
                var wait = response.headers.GetValues("Retry-After").First();
                Console.WriteLine("Waiting " + wait + " seconds...");
                Thread.Sleep(Int32.Parse(wait) * 1000);
            }
            else
            {
                // QnA Maker has completed updating the knowledge base.
                done = true;
            }
        }
    }
    catch(Exception ex)
    {
        // An error occurred while updating the knowledge base. Ensure that
        // you included your QnA Maker subscription key and knowledge base ID
        // where directed in the sample.
        Console.WriteLine("An error occurred while updating the knowledge base." + ex.InnerException);
    }
    finally
    {
        Console.WriteLine("Press any key to continue.");
    }
}
```

## <a name="add-the-updatekb-method-to-main"></a>De UpdateKB-methode toevoegen aan Main

Wijzig de Main-methode om de UpdateKB-methode aan te roepen:

```csharp
static void Main(string[] args)
{
    // Invoke the UpdateKB() method to update a knowledge base, periodically
    // checking the status of the QnA Maker operation until the
    // knowledge base is updated.
    UpdateKB(kb, new_kb);

    // The console waits for a key to be pressed before closing.
    Console.ReadLine();
}
```


## <a name="build-and-run-the-program"></a>Het programma bouwen en uitvoeren

Bouw het programma en voer het uit. Hiermee wordt de aanvraag automatisch verzonden naar de QnA Maker-API om de KB bij te werken en vervolgens worden elke 30 seconden de resultaten gecontroleerd. Elk antwoord wordt weergegeven in het consolevenster.

Zodra de knowledge base is bijgewerkt, kunt u deze weergeven in de QnA Maker-portal op de pagina [Mijn knowledge bases](https://www.qnamaker.ai/Home/MyServices). 

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Naslaginformatie over REST API voor QnA Maker (V4)](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75ff)