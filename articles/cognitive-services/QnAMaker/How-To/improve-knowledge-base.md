---
title: Verbetering van knowledge base - QnA Maker
titleSuffix: Azure Cognitive Services
description: ''
author: diberry
manager: nitinme
services: cognitive-services
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 03/05/2019
ms.author: diberry
ms.openlocfilehash: 76005b153d7a7feabdc1b335a23c6aa1f1fa99f3
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57537895"
---
# <a name="use-active-learning-to-improve-knowledge-base"></a>Actief leren gebruiken om uw knowledge base te verbeteren

Actief leren, kunt u de kwaliteit van uw knowledge base verbeteren door het alternatieve vragen, op basis van gebruiker-inzendingen aan uw vraag en antwoord voorstellen. U bekijken deze suggesties op deze toevoegen aan bestaande vragen of weigert deze. 

Uw knowledge base verandert niet automatisch. U moet de suggesties in volgorde van voor elke wijziging van kracht accepteren. Deze suggesties vragen toevoegen, maar niet wijzigen of verwijderen van bestaande vragen.

## <a name="active-learning"></a>Actief leren

QnA Maker leert nieuwe vraag variaties met impliciete en expliciete feedback.
 
* Impliciete feedback – de kerntechnologie begrijpt wanneer een gebruiker vraag vindt u meerdere antwoorden met scores die bijna zijn en wordt dit gezien als feedback. 
* Expliciete feedback – wanneer u meerdere antwoorden met weinig variatie in scores zijn geretourneerd door de knowledge base, de clienttoepassing wordt de gebruiker gevraagd welke vraag is de juiste vraag. Expliciete feedback van de gebruiker wordt verzonden naar de QnA Maker met de API van de trein. 

Een van beide methoden biedt de kerntechnologie met vergelijkbare query's die zijn geclusterd.

## <a name="how-active-learning-works"></a>Hoe actief leren werken

Actief leren wordt geactiveerd op basis van de scores van hoogste aantal antwoorden die door de QnA Maker wordt geretourneerd voor een bepaalde query. Als de score verschillen binnen een kleine bereik liggen, wordt de query wordt beschouwd als een mogelijke _suggestie_ voor elk van de mogelijke antwoorden. 

Alle suggesties samen die vergelijkbaar zijn geclusterd en bovenste suggesties voor alternatieve vragen worden weergegeven op basis van de frequentie van de bepaalde query's door eindgebruikers. Actief leren biedt de best mogelijke suggesties in gevallen waar de eindpunten zijn aan een redelijk aantal en de verschillende query's voor gebruik.

Wanneer 5 of meer vergelijkbaar query's zijn geclusterd, stelt om de 30 minuten, QnA Maker de op basis van gebruiker vragen naar de knowledge base-ontwerpfunctie te accepteren of weigeren.

Als u vragen worden voorgesteld in de portal voor QnA Maker, moet u beoordeelt en accepteert of weigert die suggesties. 

## <a name="upgrade-version-to-use-active-learning"></a>Versie van de upgrade actief leren gebruiken

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

    ![Schakel op de servicepagina-instellingen actief leren](../media/improve-knowledge-base/Endpoint-Keys.png)


1. De QnA Maker-service niet vinden en in-/ uitschakelen **actief leren**. 

    [![Schakel op de servicepagina-instellingen actief leren](../media/improve-knowledge-base/turn-active-learning-on-at-service-setting.png)](../media/improve-knowledge-base/turn-active-learning-on-at-service-setting.png#lightbox)

    Eenmaal **actief leren** is ingeschakeld, de kennis nieuwe vragen stelt met regelmatige tussenpozen op basis van vragen door de gebruiker verzonden. U kunt uitschakelen **actief leren** door het omschakelen van de instelling voor het opnieuw.

## <a name="add-active-learning-suggestion-to-knowledge-base"></a>Actief leren suggestie met knowledge base toevoegen

1. Als u wilt bekijken van de voorgestelde vragen op de **bewerken** kennisdatabase weergeeft, schakelt **suggesties weergeven**. 

    [![Op de servicepagina-instellingen wisselknop suggesties weergeven](../media/improve-knowledge-base/show-suggestions-button.png)](../media/improve-knowledge-base/show-suggestions-button.png#lightbox)

1. Filteren van de knowledge base met vragen en antwoorden paren alleen suggesties weergeven door te selecteren **filteren op suggesties**.

    [![Filteren op de servicepagina-instellingen op suggesties om te zien alleen de objecten vraag/antwoord paren](../media/improve-knowledge-base/filter-by-suggestions.png)](../media/improve-knowledge-base/filter-by-suggestions.png#lightbox)

1.  Elke sectie vraag met suggesties toont de nieuwe vragen met een vinkje `✔` , om te accepteren van de vraag of een `x` af te wijzen de suggesties. Klik op het vinkje om toe te voegen van de vraag. 

    [![Schakel op de servicepagina-instellingen actief leren](../media/improve-knowledge-base/accept-active-learning-suggestions.png)](../media/improve-knowledge-base/accept-active-learning-suggestions.png#lightbox)

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

Zodra de gebruiker een van de bestaande vragen selecteert. De feedback van gebruikers wordt verzonden naar de QnA Maker [Train](https://www.aka.ms/activelearningsamplebot) API om door te gaan van de feedback actief leren in een lus. 

```http
POST https://<QnA-Maker-resource-name>.azurewebsites.net/qnamaker/knowledgebases/<knowledge-base-ID>/train
Authorization: EndpointKey <endpoint-key>
Content-Type: application/json
{"feedbackRecords": [{"userId": "1","userQuestion": "<question-text>","qnaId": 1}]}
```

Meer informatie over het gebruik van actief leren met een [Azure Bot C# voorbeeld](https://github.com/Microsoft/BotBuilder-Samples/tree/master/experimental/csharp_dotnetcore/qnamaker-activelearning-bot)

## <a name="active-learning-is-saved-in-the-exported-apps-tsv-file"></a>Actief leren wordt opgeslagen in de geëxporteerde app tsv-bestand

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
> [Gebruik QnA Maker-API](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75ff)
