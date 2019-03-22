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
ms.date: 03/21/2019
ms.author: diberry
ms.openlocfilehash: 739ae64c6b32958271260bcbd01b339c1b108f11
ms.sourcegitcommit: 02d17ef9aff49423bef5b322a9315f7eab86d8ff
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/21/2019
ms.locfileid: "58337422"
---
# <a name="use-active-learning-to-improve-your-knowledge-base"></a>Actief leren gebruiken voor het verbeteren van uw knowledge base

Actief leren, kunt u de kwaliteit van uw knowledge base verbeteren door het alternatieve vragen, op basis van gebruiker-inzendingen aan uw vraag en antwoord voorstellen. U bekijken deze suggesties op deze toevoegen aan bestaande vragen of weigert deze. 

Uw knowledge base verandert niet automatisch. U moet de suggesties in volgorde van voor elke wijziging van kracht accepteren. Deze suggesties vragen toevoegen, maar niet wijzigen of verwijderen van bestaande vragen.

## <a name="what-is-active-learning"></a>Wat is actief leren?

QnA Maker leert nieuwe vraag variaties met impliciete en expliciete feedback.
 
* Impliciete feedback – de kerntechnologie begrijpt wanneer een gebruiker vraag vindt u meerdere antwoorden met scores die bijna zijn en wordt dit gezien als feedback. 
* Expliciete feedback – wanneer u meerdere antwoorden met weinig variatie in scores zijn geretourneerd door de knowledge base, de clienttoepassing wordt de gebruiker gevraagd welke vraag is de juiste vraag. Expliciete feedback van de gebruiker wordt verzonden naar de QnA Maker met de API van de trein. 

Een van beide methoden biedt de kerntechnologie met vergelijkbare query's die zijn geclusterd.

## <a name="how-active-learning-works"></a>Hoe actief leren werken

Actief leren wordt geactiveerd op basis van de scores van hoogste aantal antwoorden die door de QnA Maker wordt geretourneerd voor een bepaalde query. Als de score verschillen binnen een kleine bereik liggen, wordt de query wordt beschouwd als een mogelijke _suggestie_ voor elk van de mogelijke antwoorden. 

Alle suggesties samen die vergelijkbaar zijn geclusterd en bovenste suggesties voor alternatieve vragen worden weergegeven op basis van de frequentie van de bepaalde query's door eindgebruikers. Actief leren biedt de best mogelijke suggesties in gevallen waar de eindpunten zijn aan een redelijk aantal en de verschillende query's voor gebruik.

Wanneer 5 of meer vergelijkbaar query's zijn geclusterd, stelt om de 30 minuten, QnA Maker de op basis van gebruiker vragen naar de knowledge base-ontwerpfunctie te accepteren of weigeren.

Als u vragen worden voorgesteld in de portal voor QnA Maker, moet u beoordeelt en accepteert of weigert die suggesties. 

## <a name="upgrade-your-version-to-use-active-learning"></a>Werk uw versie voor het gebruik van actief leren

Actief leren wordt ondersteund in runtime-versie 4.4.0 en hoger. Als uw knowledge base is gemaakt op een eerdere versie [uw runtime upgraden](troubleshooting-runtime.md#how-to-get-latest-qnamaker-runtime-updates) deze functie wilt gebruiken. 

## <a name="best-practices"></a>Aanbevolen procedures

Zie voor aanbevolen procedures als actief leren, [Best practices](../Concepts/best-practices.md#active-learning).

## <a name="score-proximity-between-knowledge-base-questions"></a>Score afstand tussen de knowledge base-vragen

Wanneer de score van een vraag maximaal vertrouwen, zoals 80% is, wordt het bereik van de scores die worden beschouwd voor actief leren breed, ongeveer binnen 10% zijn. Als de betrouwbaarheidsscore afneemt, zoals 40%, vermindert het bereik van de scores, ongeveer binnen % 4. 

De algoritme om te bepalen van de service is niet een eenvoudige berekening. De bereiken in de voorgaande voorbeelden zijn niet bedoeld om te worden opgelost, maar moeten worden gebruikt als richtlijn voor het begrijp de gevolgen van het algoritme alleen.

## <a name="turn-on-active-learning"></a>Actief leren inschakelen

Actief leren is standaard uitgeschakeld. Inschakelen om te zien van de voorgestelde vragen. 

1. Selecteer **publiceren** voor het publiceren van de knowledge base. Actief leren query's worden verzameld van het alleen GenerateAnswer API voorspelling-eindpunt. De query's naar het deelvenster in de portal voor Qna Maker is niet van invloed op actief leren.

1. Als u actieve learning in, klikt u op uw **naam**, gaat u naar [ **Service-instellingen** ](https://www.qnamaker.ai/UserSettings) in de rechterbovenhoek van de portal QnA Maker.  

    ![Actief leren vraag alternatieven van de pagina Service-instellingen inschakelen. Selecteer uw gebruikersnaam in het menu rechtsboven en selecteer de Service-instellingen.](../media/improve-knowledge-base/Endpoint-Keys.png)


1. De QnA Maker-service niet vinden en in-/ uitschakelen **actief leren**. 

    [![Op de pagina Service-instellingen in-/ uitschakelen op de functie actief leren. Als u nog niet in de functie-of uitschakelen, moet u mogelijk uw service upgraden.](../media/improve-knowledge-base/turn-active-learning-on-at-service-setting.png)](../media/improve-knowledge-base/turn-active-learning-on-at-service-setting.png#lightbox)

    Eenmaal **actief leren** is ingeschakeld, de kennis nieuwe vragen stelt met regelmatige tussenpozen op basis van vragen door de gebruiker verzonden. U kunt uitschakelen **actief leren** door het omschakelen van de instelling voor het opnieuw.

## <a name="add-active-learning-suggestion-to-knowledge-base"></a>Actief leren suggestie met knowledge base toevoegen

1. Als u wilt bekijken van de voorgestelde vragen op de **bewerken** kennisdatabase weergeeft, schakelt **suggesties weergeven**. 

    [![Selecteer op de sectie bewerken van de portal voor suggesties weergeven om te zien van de actief leren nieuwe vraag alternatieven.](../media/improve-knowledge-base/show-suggestions-button.png)](../media/improve-knowledge-base/show-suggestions-button.png#lightbox)

1. Filteren van de knowledge base met vragen en antwoorden paren alleen suggesties weergeven door te selecteren **filteren op suggesties**.

    [![Gebruik het Filter door suggesties in-/ uitschakelen om alleen de actief leren vraag alternatieven weer te geven.](../media/improve-knowledge-base/filter-by-suggestions.png)](../media/improve-knowledge-base/filter-by-suggestions.png#lightbox)

1.  Elke sectie vraag met suggesties toont de nieuwe vragen met een vinkje `✔` , om te accepteren van de vraag of een `x` af te wijzen de suggesties. Klik op het vinkje om toe te voegen van de vraag. 

    [![Selecteer of actief leren vraag alternatieven weigeren door het selecteren van de groen vinkje of rood verwijderen is ingeschakeld.](../media/improve-knowledge-base/accept-active-learning-suggestions.png)](../media/improve-knowledge-base/accept-active-learning-suggestions.png#lightbox)

    U kunt toevoegen of verwijderen _alle suggesties_ hiervoor **alle** of **alles afwijzen**.

1. Selecteer **opslaan en trainen** aan Sla de wijzigingen in de knowledge base.

1. Selecteer **publiceren** om toe te staan de wijzigingen die door de API GenerateAnswer beschikbaar zijn.

    Wanneer 5 of meer vergelijkbaar query's zijn geclusterd, stelt om de 30 minuten, QnA Maker de op basis van gebruiker vragen naar de knowledge base-ontwerpfunctie te accepteren of weigeren.

## <a name="determine-best-choice-when-several-questions-have-similar-scores"></a>Beste keuze bepalen wanneer de volgende vragen vergelijkbare scores hebben

Wanneer u een vraag te sluiten in score op andere vragen, kunt ontwikkelaar van de client-toepassing om informatie te vragen.

### <a name="use-the-top-property-in-the-generateanswer-request"></a>Gebruik van de belangrijkste eigenschap in de aanvraag GenerateAnswer

Bij het indienen van een vraag voor een antwoord QnA Maker, kan deel uitmaken van de JSON-hoofdtekst voor het retourneren van meer dan één bovenste antwoord:

```json
{
    "question": "wi-fi",
    "isTest": false,
    "top": 3
}
```

Als de clienttoepassing (zoals een chatbot)-antwoord is ontvangen, worden de meestgestelde vragen 3 geretourneerd:

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

### <a name="client-application-follow-up-when-questions-have-similar-scores"></a>Client-toepassing worden opgevolgd wanneer vragen vergelijkbare scores hebben

De clienttoepassing wordt weergegeven voor alle vragen met een optie voor de gebruiker selecteert de vraag die het beste hun voornemen vertegenwoordigt. 

Zodra de gebruiker een van de bestaande vragen selecteert, verzendt de clienttoepassing keuze van de gebruiker als feedback met behulp van de QnA Maker Train API. Deze feedback is voltooid voor de actieve learning feedback-lus. 

Gebruik de [Azure Bot C# voorbeeld](https://github.com/Microsoft/BotBuilder-Samples/tree/master/experimental/csharp_dotnetcore/qnamaker-activelearning-bot) om te zien van actief leren in een end-to-end-scenario.

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
|`userQuestion`|string|Exact overeenkomende tekst van de vraag. Vereist.|
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

<a name="active-learning-is-saved-in-the-exported-apps-tsv-file"></a>

## <a name="active-learning-is-saved-in-the-exported-knowledge-base"></a>Actief leren wordt opgeslagen in de geëxporteerde knowledge base

Wanneer uw app actief leren ingeschakeld is, en u de app exporteren de `SuggestedQuestions` kolom in de tsv-bestand worden de gegevens actief leren bewaard. 

De `SuggestedQuestions` kolom is een JSON-object van informatie van de impliciete (`autosuggested`) en expliciet (`usersuggested`) [feedback](#active-learning). Een voorbeeld van dit JSON-object voor een enkele gebruiker ingediend vraag van `help` is:

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

## <a name="next-steps"></a>Volgende stappen
 
> [!div class="nextstepaction"]
> [Metagegevens met GenerateAnswer API gebruiken](metadata-generateanswer-usage.md)
