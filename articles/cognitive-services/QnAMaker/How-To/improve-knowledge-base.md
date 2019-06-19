---
title: Verbetering van knowledge base - QnA Maker
titleSuffix: Azure Cognitive Services
description: Actief leren, kunt u de kwaliteit van uw knowledge base verbeteren door het alternatieve vragen, op basis van gebruiker-inzendingen aan uw vraag en antwoord voorstellen. U bekijken deze suggesties op deze toevoegen aan bestaande vragen of weigert deze. Uw knowledge base verandert niet automatisch. U moet de suggesties voor elke wijziging van kracht accepteren. Deze suggesties vragen toevoegen, maar niet wijzigen of verwijderen van bestaande vragen.
author: diberry
manager: nitinme
services: cognitive-services
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 06/19/2019
ms.author: diberry
ms.openlocfilehash: b73884e544ea1b8ee76c8a891048e6a8e17d6ab3
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67204083"
---
# <a name="use-active-learning-to-improve-your-knowledge-base"></a>Actief leren gebruiken voor het verbeteren van uw knowledge base

Actief leren, kunt u de kwaliteit van uw knowledge base verbeteren door het alternatieve vragen, op basis van gebruiker-inzendingen aan uw vraag en antwoord voorstellen. U bekijken deze suggesties op deze toevoegen aan bestaande vragen of weigert deze. 

Uw knowledge base verandert niet automatisch. In de volgorde voor elke wijziging door te voeren, moet u de suggesties accepteren. Deze suggesties vragen toevoegen, maar niet wijzigen of verwijderen van bestaande vragen.

## <a name="what-is-active-learning"></a>Wat is actief leren?

QnA Maker leert nieuwe vraag variaties met impliciete en expliciete feedback.
 
* [Impliciete feedback](#how-qna-makers-implicit-feedback-works) – de kerntechnologie begrijpt wanneer een gebruiker vraag vindt u meerdere antwoorden met scores die bijna zijn en wordt dit gezien als feedback. U hoeft te doen voor dit mag gebeuren.
* [Expliciete feedback](#how-you-give-explicit-feedback-with-the-train-api) – wanneer u meerdere antwoorden met weinig variatie in scores zijn geretourneerd door de knowledge base, de clienttoepassing vraagt de gebruiker die vraag is de juiste vraag. Expliciete feedback van de gebruiker wordt verzonden naar de QnA Maker met de [API van de trein](#train-api). 

Beide methoden bieden de kerntechnologie met vergelijkbare query's die zijn geclusterd.

## <a name="how-active-learning-works"></a>Hoe actief leren werken

Actief leren wordt geactiveerd op basis van de scores van de eerste paar antwoorden die wordt geretourneerd door de QnA Maker. Als de score verschillen binnen een kleine bereik liggen, klikt u vervolgens de query wordt beschouwd als een mogelijke suggestie (als een alternatieve vraag) voor elk van de mogelijke QnA-paren. Zodra u de vraag voor een specifieke QnA paar akkoord gaat, wordt deze geweigerd voor de paren met een andere. U moet onthouden op te slaan en te trainen, nadat u hebt geaccepteerd suggesties.

Actief leren biedt de best mogelijke suggesties in gevallen waar de eindpunten zijn aan een redelijk aantal en de verschillende query's voor gebruik. Wanneer 5 of meer vergelijkbaar query's zijn geclusterd, stelt om de 30 minuten, QnA Maker de op basis van gebruiker vragen naar de knowledge base-ontwerpfunctie te accepteren of weigeren. Alle suggesties samen die vergelijkbaar zijn geclusterd en bovenste suggesties voor alternatieve vragen worden weergegeven op basis van de frequentie van de bepaalde query's door eindgebruikers.

Als u vragen worden voorgesteld in de portal voor QnA Maker, moet u beoordeelt en accepteert of weigert die suggesties. Er is een API voor het beheren van suggesties niet.

## <a name="how-qna-makers-implicit-feedback-works"></a>De werking van de QnA Maker impliciete feedback

Impliciete feedback van QnA Maker maakt gebruik van een algoritme om te bepalen score nabijheid vervolgens actief leren suggesties doen. De algoritme om te bepalen van de service is niet een eenvoudige berekening. De bereiken in het volgende voorbeeld zijn niet bedoeld om te worden opgelost, maar moeten worden gebruikt als richtlijn voor het begrijp de gevolgen van het algoritme alleen.

Wanneer de score van een vraag maximaal vertrouwen, zoals 80% is, wordt het bereik van de scores die worden beschouwd voor actief leren breed, ongeveer binnen 10% zijn. Als de betrouwbaarheidsscore afneemt, zoals 40%, vermindert het bereik van de scores, ongeveer binnen % 4. 

## <a name="how-you-give-explicit-feedback-with-the-train-api"></a>Hoe u expliciete feedback geven met de API van de trein

Het is belangrijk dat QnA Maker expliciete feedback die over welke van de antwoorden het beste antwoord is ontvangt. Hoe het beste antwoord wordt bepaald aan u is en kunt opnemen:

* Feedback van gebruikers, een van de antwoorden te selecteren.
* Zakelijke logica, zoals het bepalen van een acceptabele bereik te beoordelen.  
* Een combinatie van beide gebruikers feedback en bedrijfslogica.

## <a name="upgrade-your-runtime-version-to-use-active-learning"></a>Werk uw runtimeversie voor het gebruik van actief leren

Actief leren wordt ondersteund in runtime-versie 4.4.0 en hoger. Als uw knowledge base is gemaakt op een eerdere versie [uw runtime upgraden](troubleshooting-runtime.md#how-to-get-latest-qnamaker-runtime-updates) deze functie wilt gebruiken. 

## <a name="turn-on-active-learning-to-see-suggestions"></a>Actief leren om te bekijken van suggesties inschakelen

Actief leren is standaard uitgeschakeld. Inschakelen om te zien van de voorgestelde vragen. Nadat u actief leren inschakelt, moet u het verzenden van gegevens vanaf de client-app naar QnA Maker. Zie voor meer informatie, [architectuur stroom voor het gebruik van GenerateAnswer en Train API's van een bot](#architectural-flow-for-using-generateanswer-and-train-apis-from-a-bot).

1. Selecteer **publiceren** voor het publiceren van de knowledge base. Actief leren query's worden verzameld van het alleen GenerateAnswer API voorspelling-eindpunt. De query's naar het deelvenster in de portal voor QnA Maker is niet van invloed op actief leren.

1. Als u wilt inschakelen actief leren op in de QnA Maker portal, gaat u naar de rechterbovenhoek, selecteer uw **naam**, gaat u naar [ **Service-instellingen**](https://www.qnamaker.ai/UserSettings).  

    ![Actief leren vraag alternatieven van de pagina Service-instellingen inschakelen. Selecteer uw gebruikersnaam in het menu rechtsboven en selecteer de Service-instellingen.](../media/improve-knowledge-base/Endpoint-Keys.png)


1. De QnA Maker-service niet vinden en in-/ uitschakelen **actief leren**. 

    [![Op de pagina Service-instellingen in-/ uitschakelen op de functie actief leren. Als u nog niet in de functie-of uitschakelen, moet u mogelijk uw service upgraden.](../media/improve-knowledge-base/turn-active-learning-on-at-service-setting.png)](../media/improve-knowledge-base/turn-active-learning-on-at-service-setting.png#lightbox)

    Eenmaal **actief leren** is ingeschakeld, de knowledge base nieuwe vragen stelt met regelmatige tussenpozen op basis van vragen door de gebruiker verzonden. U kunt uitschakelen **actief leren** door het omschakelen van de instelling voor het opnieuw.

## <a name="accept-an-active-learning-suggestion-in-the-knowledge-base"></a>Een suggestie actief leren in het knowledge base accepteren

1. Als u wilt bekijken van de voorgestelde vragen op de **bewerken** kennisdatabase weergeeft, schakelt **weergaveopties**en selecteer vervolgens **actief leren suggesties weergeven**. 

    [![Selecteer op de sectie bewerken van de portal voor suggesties weergeven om te zien van de actief leren nieuwe vraag alternatieven.](../media/improve-knowledge-base/show-suggestions-button.png)](../media/improve-knowledge-base/show-suggestions-button.png#lightbox)

1. Filteren van de knowledge base met vragen en antwoorden paren alleen suggesties weergeven door te selecteren **filteren op suggesties**.

    [![Gebruik het Filter door suggesties in-/ uitschakelen om alleen de actief leren vraag alternatieven weer te geven.](../media/improve-knowledge-base/filter-by-suggestions.png)](../media/improve-knowledge-base/filter-by-suggestions.png#lightbox)

1. Elk paar QnA stelt de nieuwe vraag alternatieven met een vinkje `✔` , om te accepteren van de vraag of een `x` af te wijzen de suggesties. Klik op het vinkje om toe te voegen van de vraag. 

    [![Selecteer of actief leren vraag alternatieven weigeren door het selecteren van de groen vinkje of rood verwijderen is ingeschakeld.](../media/improve-knowledge-base/accept-active-learning-suggestions.png)](../media/improve-knowledge-base/accept-active-learning-suggestions.png#lightbox)

    U kunt toevoegen of verwijderen _alle suggesties_ door te selecteren **alle** of **alles afwijzen** in de contextuele werkbalk.

1. Selecteer **opslaan en trainen** aan Sla de wijzigingen in de knowledge base.

1. Selecteer **publiceren** om toe te staan de wijzigingen beschikbaar vanuit de [GenerateAnswer API](metadata-generateanswer-usage.md#generateanswer-request-configuration).

    Wanneer u 5 of meer vergelijkbaar query's zijn geclusterd, stelt elke 30 minuten, QnA Maker de alternatieve vragen die u wilt accepteren of weigeren.


<a name="#score-proximity-between-knowledge-base-questions"></a>

### <a name="architectural-flow-for-using-generateanswer-and-train-apis-from-a-bot"></a>Architectuur van de stroom voor het gebruik van GenerateAnswer en Train API's van een bot

Een bot of andere clienttoepassing moet gebruiken de volgende architectuur stroom actief leren gebruiken:

* Bot [opgehaald van het antwoord van de knowledge base](#use-the-top-property-in-the-generateanswer-request-to-get-several-matching-answers) met de API GenerateAnswer met behulp van de `top` eigenschap om een aantal antwoorden.
* Bot bepaalt expliciete feedback:
    * Met uw eigen [aangepaste bedrijfslogica](#use-the-score-property-along-with-business-logic-to-get-list-of-answers-to-show-user), lage scores filteren.
    * In de bot of client-toepassing, lijst met mogelijke antwoorden aan de gebruiker weergeven en ophalen van de geselecteerde antwoord van de gebruiker.
* Bot [stuurt geselecteerde antwoord terug naar de QnA Maker](#bot-framework-sample-code) met de [API van de trein](#train-api).


### <a name="use-the-top-property-in-the-generateanswer-request-to-get-several-matching-answers"></a>Gebruik van de belangrijkste eigenschap in de aanvraag GenerateAnswer meerdere overeenkomende antwoorden krijgen

Bij het indienen van een vraag voor een antwoord, QnA Maker de `top` eigenschap van de JSON-hoofdtekst stelt u het aantal antwoorden om terug te keren. 

```json
{
    "question": "wi-fi",
    "isTest": false,
    "top": 3
}
```

### <a name="use-the-score-property-along-with-business-logic-to-get-list-of-answers-to-show-user"></a>Gebruik van de eigenschap score samen met zakelijke logica lijst ophalen van antwoorden op de gebruiker weergeven

Wanneer de clienttoepassing (zoals een chatbot)-antwoord is ontvangen, worden de meestgestelde vragen 3 geretourneerd. Gebruik de `score` eigenschap voor het analyseren van de afstand tussen de scores. Dit bereik nabijheid wordt bepaald door uw eigen bedrijfslogica. 

```json
{
    "answers": [
        {
            "questions": [
                "Wi-Fi Direct Status Indicator"
            ],
            "answer": "**Wi-Fi Direct Status Indicator**\n\nStatus bar icons indicate your current Wi-Fi Direct connection status:  \n\nWhen your device is connected to another device using Wi-Fi Direct, '$  \n\n+ •+ ' Wi-Fi Direct is displayed in the Status bar.",
            "score": 74.21,
            "id": 607,
            "source": "Bugbash KB.pdf",
            "metadata": []
        },
        {
            "questions": [
                "Wi-Fi - Connections"
            ],
            "answer": "**Wi-Fi**\n\nWi-Fi is a term used for certain types of Wireless Local Area Networks (WLAN). Wi-Fi communication requires access to a wireless Access Point (AP).",
            "score": 74.15,
            "id": 599,
            "source": "Bugbash KB.pdf",
            "metadata": []
        },
        {
            "questions": [
                "Turn Wi-Fi On or Off"
            ],
            "answer": "**Turn Wi-Fi On or Off**\n\nTurning Wi-Fi on makes your device able to discover and connect to compatible in-range wireless APs.  \n\n1.  From a Home screen, tap ::: Apps > e Settings .\n2.  Tap Connections > Wi-Fi , and then tap On/Off to turn Wi-Fi on or off.",
            "score": 69.99,
            "id": 600,
            "source": "Bugbash KB.pdf",
            "metadata": []
        }
    ]
}
```

## <a name="client-application-follow-up-when-questions-have-similar-scores"></a>Client-toepassing worden opgevolgd wanneer vragen vergelijkbare scores hebben

Uw clienttoepassing worden weergegeven de vragen met een optie voor de gebruiker selecteert _de één vraag_ die het beste vertegenwoordigt de bedoeling. 

Zodra de gebruiker een van de bestaande vragen selecteert, verzendt de clienttoepassing keuze van de gebruiker als feedback met behulp van de QnA Maker Train API. Deze feedback is voltooid voor de actieve learning feedback-lus. 

## <a name="train-api"></a>API van de trein

Actief leren feedback is verzonden naar de QnA Maker met de trein API POST-aanvraag. De API-handtekening is:

```http
POST https://<QnA-Maker-resource-name>.azurewebsites.net/qnamaker/knowledgebases/<knowledge-base-ID>/train
Authorization: EndpointKey <endpoint-key>
Content-Type: application/json
{"feedbackRecords": [{"userId": "1","userQuestion": "<question-text>","qnaId": 1}]}
```

|De eigenschap van de HTTP-aanvraag|Name|Type|Doel|
|--|--|--|--|
|URL-parameter voor route|Knowledge base-ID|string|De GUID voor uw knowledge base.|
|Host-subdomein|De naam van de QnAMaker-resource|string|De hostnaam voor de QnA Maker in uw Azure-abonnement. Dit is beschikbaar op de pagina instellingen nadat u de knowledge base hebt gepubliceerd. |
|Header|Content-Type|string|Het mediatype van de hoofdtekst van het verzonden naar de API. Standaardwaarde is: `application/json`|
|Header|Autorisatie|string|De eindpuntsleutel van uw (EndpointKey xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx).|
|Hoofdtekst van bericht|JSON-object|JSON|De feedback training|

De hoofdtekst van de JSON bevat verschillende instellingen:

|JSON-hoofdtekst van de eigenschap|Type|Doel|
|--|--|--|--|
|`feedbackRecords`|array|Lijst van de feedback.|
|`userId`|string|De gebruikers-ID van de persoon die de voorgestelde vragen accepteren. De indeling van de gebruiker-ID is aan u. Een e-mailadres kan bijvoorbeeld een geldige gebruikers-ID in uw architectuur. Optioneel.|
|`userQuestion`|string|Exact overeenkomende tekst van de query van de gebruiker. Vereist.|
|`qnaID`|getal|ID van de vraag, gevonden in de [GenerateAnswer antwoord](metadata-generateanswer-usage.md#generateanswer-response-properties). |

Een voorbeeld-JSON-hoofdtekst ziet eruit zoals:

```json
{
    "feedbackRecords": [
        {
            "userId": "1",
            "userQuestion": "<question-text>",
            "qnaId": 1
        }
    ]
}
```

Een geslaagde respons retourneert een status van 204 en er is geen JSON-antwoordtekst. 

### <a name="batch-many-feedback-records-into-a-single-call"></a>Batch-aantal feedback records in één aanroep

In de client-side '-toepassing, zoals een bot, kunt u de gegevens worden opgeslagen en vervolgens verzenden van meerdere records in een enkele JSON-instantie in de `feedbackRecords` matrix. 

Een voorbeeld-JSON-hoofdtekst ziet eruit zoals:

```json
{
    "feedbackRecords": [
        {
            "userId": "1",
            "userQuestion": "How do I ...",
            "qnaId": 1
        },
        {
            "userId": "2",
            "userQuestion": "Where is ...",
            "qnaId": 40
        },
        {
            "userId": "3",
            "userQuestion": "When do I ...",
            "qnaId": 33
        }
    ]
}
```



<a name="active-learning-is-saved-in-the-exported-apps-tsv-file"></a>

## <a name="bot-framework-sample-code"></a>Voorbeeldcode voor bot framework

De code van uw bot framework moet aanroepen van de trein-API, als de query van de gebruiker moet worden gebruikt voor actief leren. Er zijn twee soorten code te schrijven:

* Als de query moet worden gebruikt voor actief leren bepalen
* Query verzenden naar de QnA Maker Train API voor het actief leren

In de [Azure Bot voorbeeld](https://aka.ms/activelearningsamplebot), beide van deze activiteiten zijn geprogrammeerd. 

### <a name="example-c-code-for-train-api-with-bot-framework-4x"></a>Voorbeeld C# code voor de API van de trein met Bot Framework 4.x

De volgende code laat zien hoe informatie terug naar de QnA Maker met de API van de trein te verzenden. Dit [volledige codevoorbeeld](https://github.com/microsoft/BotBuilder-Samples/tree/master/experimental/qnamaker-activelearning/csharp_dotnetcore) is beschikbaar op GitHub.

```csharp
public class FeedbackRecords
{
    // <summary>
    /// List of feedback records
    /// </summary>
    [JsonProperty("feedbackRecords")]
    public FeedbackRecord[] Records { get; set; }
}

/// <summary>
/// Active learning feedback record
/// </summary>
public class FeedbackRecord
{
    /// <summary>
    /// User id
    /// </summary>
    public string UserId { get; set; }

    /// <summary>
    /// User question
    /// </summary>
    public string UserQuestion { get; set; }

    /// <summary>
    /// QnA Id
    /// </summary>
    public int QnaId { get; set; }
}

/// <summary>
/// Method to call REST-based QnAMaker Train API for Active Learning
/// </summary>
/// <param name="host">Endpoint host of the runtime</param>
/// <param name="FeedbackRecords">Feedback records train API</param>
/// <param name="kbId">Knowledgebase Id</param>
/// <param name="key">Endpoint key</param>
/// <param name="cancellationToken"> Cancellation token</param>
public async static void CallTrain(string host, FeedbackRecords feedbackRecords, string kbId, string key, CancellationToken cancellationToken)
{
    var uri = host + "/knowledgebases/" + kbId + "/train/";

    using (var client = new HttpClient())
    {
        using (var request = new HttpRequestMessage())
        {
            request.Method = HttpMethod.Post;
            request.RequestUri = new Uri(uri);
            request.Content = new StringContent(JsonConvert.SerializeObject(feedbackRecords), Encoding.UTF8, "application/json");
            request.Headers.Add("Authorization", "EndpointKey " + key);

            var response = await client.SendAsync(request, cancellationToken);
            await response.Content.ReadAsStringAsync();
        }
    }
}
```

### <a name="example-nodejs-code-for-train-api-with-bot-framework-4x"></a>Voorbeeld van de Node.js-code voor de API van de trein met Bot Framework 4.x 

De volgende code laat zien hoe informatie terug naar de QnA Maker met de API van de trein te verzenden. Dit [volledige codevoorbeeld](https://github.com/microsoft/BotBuilder-Samples/blob/master/experimental/qnamaker-activelearning/javascript_nodejs) is beschikbaar op GitHub.

```javascript
async callTrain(stepContext){

    var trainResponses = stepContext.values[this.qnaData];
    var currentQuery = stepContext.values[this.currentQuery];

    if(trainResponses.length > 1){
        var reply = stepContext.context.activity.text;
        var qnaResults = trainResponses.filter(r => r.questions[0] == reply);

        if(qnaResults.length > 0){

            stepContext.values[this.qnaData] = qnaResults;

            var feedbackRecords = {
                FeedbackRecords:[
                    {
                        UserId:stepContext.context.activity.id,
                        UserQuestion: currentQuery,
                        QnaId: qnaResults[0].id
                    }
                ]
            };

            // Call Active Learning Train API
            this.activeLearningHelper.callTrain(this.qnaMaker.endpoint.host, feedbackRecords, this.qnaMaker.endpoint.knowledgeBaseId, this.qnaMaker.endpoint.endpointKey);
            
            return await stepContext.next(qnaResults);
        }
        else{

            return await stepContext.endDialog();
        }
    }

    return await stepContext.next(stepContext.result);
}
```

## <a name="active-learning-is-saved-in-the-exported-knowledge-base"></a>Actief leren wordt opgeslagen in de geëxporteerde knowledge base

Wanneer uw app actief leren ingeschakeld is, en u de app exporteren de `SuggestedQuestions` kolom in de tsv-bestand worden de gegevens actief leren bewaard. 

De `SuggestedQuestions` kolom is een JSON-object van informatie van de impliciete, `autosuggested`, en expliciet, `usersuggested` feedback. Een voorbeeld van dit JSON-object voor een enkele gebruiker ingediend vraag van `help` is:

```JSON
[
    {
        "clusterHead": "help",
        "totalAutoSuggestedCount": 1,
        "totalUserSuggestedCount": 0,
        "alternateQuestionList": [
            {
                "question": "help",
                "autoSuggestedCount": 1,
                "userSuggestedCount": 0
            }
        ]
    }
]
```

Wanneer u deze app opnieuw importeren, blijft de actief leren te verzamelen en aan te bevelen suggesties voor uw knowledge base. 

## <a name="best-practices"></a>Aanbevolen procedures

Zie voor aanbevolen procedures als actief leren, [Best practices](../Concepts/best-practices.md#active-learning).

## <a name="next-steps"></a>Volgende stappen
 
> [!div class="nextstepaction"]
> [Metagegevens met GenerateAnswer API gebruiken](metadata-generateanswer-usage.md)
