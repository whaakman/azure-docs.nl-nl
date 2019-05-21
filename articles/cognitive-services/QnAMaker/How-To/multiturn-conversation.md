---
title: Meerdere inschakelen gesprekken
titleSuffix: Azure Cognitive Services
description: Prompts en context gebruiken voor het beheren van de meerdere schakelt, bekend als meerdere inschakelen, voor uw bot in één vraag naar een andere. Meerdere inschakelen is de mogelijkheid om een back en uiteengezet conversatie waar de volgende vragen en antwoorden van invloed op de vorige vraag context.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.topic: article
ms.date: 05/14/2019
ms.author: diberry
ms.openlocfilehash: f12b55e9b00e933e13f84832b8cc36267a1da05f
ms.sourcegitcommit: 24fd3f9de6c73b01b0cee3bcd587c267898cbbee
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2019
ms.locfileid: "65954852"
---
# <a name="use-follow-up-prompts-to-create-multiple-turns-of-a-conversation"></a>Gebruik follow-up aanwijzingen voor het maken van meerdere wordt van een gesprek

Opvolging prompts en context gebruiken voor het beheren van de meerdere schakelt, ook wel _meerdere inschakelen_, voor uw bot in één vraag naar een andere.

Leer van de [demonstratievideo](https://aka.ms/multiturnexample).

## <a name="what-is-a-multi-turn-conversation"></a>Wat is een gesprek met meerdere inschakelen?

Bepaalde typen conversatie kunnen niet worden voltooid in een enkel inschakelen. Bij het ontwerpen van uw client-toepassing (chatbot) gesprekken, kan een gebruiker een vraag die moet worden gefilterd of verfijnd om te bepalen het juiste antwoord. Deze stroom via de vragen is mogelijk door middel van de gebruiker met de **opvolging vraagt**.

Wanneer de gebruiker wordt gevraagd de vraag, QnA Maker retourneert het antwoord _en_ follow-up prompts. Hiermee kunt u de follow-up vragen als opties presenteren. 

## <a name="example-multi-turn-conversation-with-chat-bot"></a>Voorbeeld van meerdere inschakelen conversatie met chatbot

Een chatbot beheert de conversatie vraag door vraag aan de gebruiker om te bepalen het definitieve antwoord.

![In de conversatie flow conversatie-status in een systeem met meerdere inschakelen dialoogvenster te beheren door op te geven van de instructies in de antwoorden weergegeven als opties om door te gaan van de conversatie.](../media/conversational-context/conversation-in-bot.png)

Vraag van de gebruiker moet worden omgezet voordat het antwoord wordt geretourneerd in de vorige afbeelding. In de knowledge base heeft de vraag (#1), vier follow-up prompts weergegeven in de bot chatten als vier opties (2). 

Wanneer de gebruiker selecteert een keuze (3), wordt de volgende lijst met opties (4) verfijnen weergegeven. Dit kunt (5) blijven totdat het correct en definitieve antwoord (6) wordt bepaald.

U moet de clienttoepassing voor het beheren van de contextuele conversatie wijzigen.

## <a name="create-a-multi-turn-conversation-from-documents-structure"></a>Een gesprek met meerdere inschakelen van de structuur van het document maken
Wanneer u een kennisdatabase maakt, ziet u een optionele selectievakje in om in te schakelen extractie van meerdere inschakelen. Als u deze optie selecteert bij het importeren van een document, kunt u de conversatie met meerdere inschakelen impliciete van de structuur. Als die structuur bestaat, maakt QnA Maker de follow-up vragen QnA paren voor u. Meerdere inschakelen structuur kan alleen worden afgeleid van URL's, PDF-bestand of DOCX bestanden. 

De volgende afbeelding van een Microsoft Surface [PDF-bestand](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/product-manual.pdf) is bedoeld om te worden gebruikt als een handmatige. 

![! [Als u een document importeert, contextuele conversatie kan worden impliciet van de structuur. Als die structuur bestaat, maakt QnA Maker de follow-up vragen QnA-paren voor u, als onderdeel van het document importeren.] (.. / media/conversational-context/import-file-with-conversational-structure.png)](../media/conversational-context/import-file-with-conversational-structure.png#lightbox)

Bij het importeren van het PDF-document, bepaalt de QnA Maker follow-up prompts van de structuur om conversatie stroom te maken. 

![! [Bij het importeren van het PDF-document, bepaalt met QnA Maker follow-up prompts van de structuur om conversatie stroom te maken. ](.. / media/conversational-context/surface-manual-pdf-follow-up-prompt.png)](../media/conversational-context/surface-manual-pdf-follow-up-prompt.png#lightbox)

## <a name="show-questions-and-answers-with-context"></a>Vragen en antwoorden met context weergeven

1. Verminder de weergegeven voor alleen degene met contextuele gesprekken paren van vragen en antwoorden. Selecteer **weergaveopties**en selecteer vervolgens **context weergeven (PREVIEW)**. De lijst zijn leeg totdat u de eerste vraag en antwoord-paar met een follow-up prompt toevoegen. 

    ![Filteren van vraag en antwoord paren door contextuele gesprekken](../media/conversational-context/filter-question-and-answers-by-context.png)

## <a name="add-new-qna-pair-as-follow-up-prompt"></a>Nieuwe QnA set toevoegen als opvolgingsactiviteit prompt

1. Selecteer **QnA toevoegen paar**. 
1. Voer de tekst van de nieuwe vraag, `Give feedback.` met een antwoord van `What kind of feedback do you have?`.

1. In de **antwoord** kolom voor deze vraag, selecteer **toevoegen follow-up prompt**. 
1. De **opvolging prompt (PREVIEW)** pop-upvenster kunt u zoeken naar een bestaande vraag of voer een nieuwe vraag. Maak een nieuwe opdrachtprompt door in te voeren van de volgende waarden: 

    |Tekstveld|Value|
    |--|--|
    |**Tekst weergeven**|`Feedback on an QnA Maker service`|
    |**Koppeling om te beantwoorden**|`How would you rate QnA Maker??`|
    |||

    ![Nieuwe vragen QnA maken](../media/conversational-context/create-child-prompt-from-parent.png)

1. Controleer **Context alleen-lezen**. De **Context alleen-lezen** optie geeft aan dat deze gebruiker tekst wordt worden begrepen _alleen_ als reactie op de vorige vraag. Voor dit scenario, de tekst voor het niet logisch als een zelfstandige vraag, alleen is het handig in de context van de vorige vraag.
1. Selecteer **nieuw** Selecteer **opslaan**. 

    Dit een nieuwe vraag en antwoord-set gemaakt en gekoppeld van de geselecteerde vraag als een follow-up prompt. De **Context** kolom voor beide vragen een follow-up vragen relatie aangeeft. 

    ![! [De kolom Context voor beide vragen geeft aan dat een follow-up vragen relatie.] (.. / media/conversational-context/child-prompt-created.png)](../media/conversational-context/child-prompt-created.png#lightbox)

1. Selecteer **toevoegen follow-up prompt** voor de `Give feedback` vraag om toe te voegen een andere follow-up prompt. Hiermee opent u de **opvolging prompt (PREVIEW)** pop-upvenster.

1. Maak een nieuwe opdrachtprompt door in te voeren van de volgende waarden:

    |Tekstveld|Value|
    |--|--|
    |**Tekst weergeven**|`Feedback on an existing feature`|
    |**Koppeling om te beantwoorden**|`Which feature would you like to give feedback on?`|
    |||

1. Controleer **Context alleen-lezen**. De **Context alleen-lezen** optie geeft aan dat deze gebruiker tekst wordt worden begrepen _alleen_ als reactie op de vorige vraag. Voor dit scenario, de tekst voor het niet logisch als een zelfstandige vraag, alleen is het handig in de context van de vorige vraag.

1. Selecteer **Opslaan**. 

    Dit een nieuwe vraag gemaakt en de vraag gekoppeld als een follow-up vraag aan de `Give feedback` vraag.
    
    Op dit moment de bovenste vraag heeft twee follow-up vragen die zijn gekoppeld aan de vorige vraag, `Give feedback`.

    ![! [Dit de eerste vraag is op dit moment twee follow-up vragen leuk vinden op de vorige vraag, 'Feedback geven'.] (.. / media/conversational-context/all-child-prompts-created.png)](../media/conversational-context/all-child-prompts-created.png#lightbox)

1. Selecteer **opslaan en trainen** met het trainen van de knowledge base met de nieuwe vragen. 

## <a name="add-existing-qna-pair-as-follow-up-prompt"></a>Bestaande QnA paar toevoegen als opvolgingsactiviteit prompt

1. Als u het sleutelpaar met een bestaande QnA als een follow-up prompt koppelen wilt, selecteert u de rij voor het paar vragen en antwoorden.
1. Selecteer **toevoegen follow-up prompt** in die rij.
1. In de **opvolging prompt (PREVIEW)** pop-upvenster de antwoordtekst invoeren in het zoekvak in. Alle resultaten worden geretourneerd. Selecteer het antwoord dat u wilt gebruiken als de follow-up en Controleer **Context alleen-lezen**en selecteer vervolgens **opslaan**. 

    ![Zoeken naar de Follow-up-prompt koppeling antwoord dialoogvenster voor een bestaande antwoord aan met behulp van de tekst van het antwoord.](../media/conversational-context/search-follow-up-prompt-for-existing-answer.png)

    Nadat u de follow-up prompt hebt toegevoegd, moet u selecteren **opslaan en trainen**.
  
<!--

## To find best prompt answer, add metadata to follow-up prompts 

If you have several follow-up prompts for a given QnA pair, but you know as the knowledge base manager, that not all prompts should be returned, use metadata to categorize the prompts in the knowledge base, then send the metadata from the client application as part of the GenerateAnswer request.

In the knowledge base, when a question-and-answer pair is linked to follow-up prompts, the metadata filters are applied first, then the follow-ups are returned.

1. For the two follow-up QnA pairs, add metadata to each one:

    |Question|Add metadata|
    |--|--|
    |`Feedback on an QnA Maker service`|"Feature":"all"|
    |`Feedback on an existing feature`|"Feature":"one"|
    
    ![Add metadata to follow-up prompt so it can be filtered in conversation response from service](../media/conversational-context/add-metadata-feature-to-follow-up-prompt.png) 

1. Save and train. 

    When you send the question `Give feedback` with the metadata filter `Feature` with a value of `all`, only the QnA pair with that metadata will be returned. Both QnA pairs are not returned because they both do not match the filter. 

-->

## <a name="test-the-qna-set-to-get-all-the-follow-up-prompts"></a>Test die de QnA instellen om op te halen van alle opvolging wordt gevraagd

Wanneer de vraag met opvolging testen vraagt de **Test** venster **inschakelen van meerdere inschakelen**, en voer uw vraag. Het antwoord bevat de follow-up aanwijzingen.

![Bij het testen van de vraag in het deelvenster, bevat het antwoord de follow-up aanwijzingen.](../media/conversational-context/test-pane-with-question-having-follow-up-prompts.png)

## <a name="json-request-to-return-initial-answer-and-follow-up-prompts"></a>JSON-aanvraag om terug te keren eerste antwoord en follow-up prompts

Gebruik de lege `context` object om te vragen van het antwoord op de vraag van de gebruiker en follow-up prompts bevatten. 

```JSON
{
  "question": "accounts and signing in",
  "top": 30,
  "userId": "Default",
  "isTest": false,
  "context": {}
}
```

## <a name="json-response-to-return-initial-answer-and-follow-up-prompts"></a>JSON-antwoord om terug te keren eerste antwoord en follow-up prompts

De vorige sectie aangevraagd een antwoord en opvolging aanwijzingen voor het `Accounts and signing in`. Het antwoord bevat de prompt informatie, dat zich bevindt in `answers[0].context`, bevatten de tekst die moet worden weergegeven voor de gebruiker. 

```JSON
{
    "answers": [
        {
            "questions": [
                "Accounts and signing in"
            ],
            "answer": "**Accounts and signing in**\n\nWhen you set up your Surface, an account is set up for you. You can create additional accounts later for family and friends, so each person using your Surface can set it up just the way they like. For more info, see All about accounts on Surface.com. \n\nThere are several ways to sign in to your Surface Pro 4: ",
            "score": 86.96,
            "id": 37,
            "source": "surface-pro-4-user-guide-EN .pdf",
            "metadata": [],
            "context": {
                "isContextOnly": true,
                "prompts": [
                    {
                        "displayOrder": 0,
                        "qnaId": 38,
                        "qna": null,
                        "displayText": "Use the sign-in screen"
                    },
                    {
                        "displayOrder": 1,
                        "qnaId": 39,
                        "qna": null,
                        "displayText": "Use Windows Hello to sign in"
                    },
                    {
                        "displayOrder": 2,
                        "qnaId": 40,
                        "qna": null,
                        "displayText": "Sign out"
                    }
                ]
            }
        },
        {
            "questions": [
                "Use the sign-in screen"
            ],
            "answer": "**Use the sign-in screen**\n\n1.  \n\nTurn on or wake your Surface by pressing the power button. \n\n2.  \n\nSwipe up on the screen or tap a key on the keyboard. \n\n3.  \n\nIf you see your account name and account picture, enter your password and select the right arrow or press Enter on your keyboard. \n\n4.  \n\nIf you see a different account name, select your own account from the list at the left. Then enter your password and select the right arrow or press Enter on your keyboard. ",
            "score": 32.27,
            "id": 38,
            "source": "surface-pro-4-user-guide-EN .pdf",
            "metadata": [],
            "context": {
                "isContextOnly": true,
                "prompts": []
            }
        },
        {
            "questions": [
                "Sign out"
            ],
            "answer": "**Sign out**\n\nHere's how to sign out: \n\n Go to Start , and right-click your name. Then select Sign out. ",
            "score": 27.0,
            "id": 40,
            "source": "surface-pro-4-user-guide-EN .pdf",
            "metadata": [],
            "context": {
                "isContextOnly": true,
                "prompts": []
            }
        }
    ],
    "debugInfo": null
}
```

De `prompts` matrix bevat tekst in de `displayText` eigenschap en de `qnaId` waarde, zodat u deze vragen te beantwoorden als de volgende weergegeven opties in de conversatie-stroom weergeven kunt. 

## <a name="json-request-to-return-non-initial-answer-and-follow-up-prompts"></a>JSON-aanvraag om niet-eerste antwoord en follow-up prompts te retourneren

Vul de `context` object om op te nemen van vorige context.

In de volgende JSON-aanvraag, is de huidige vraag `Use Windows Hello to sign in` en de vorige vraag is `Accounts and signing in`. 

```JSON
{
  "question": "Use Windows Hello to sign in",
  "top": 30,
  "userId": "Default",
  "isTest": false,
  "qnaId": 39,
  "context": {
    "previousQnAId": 37,
    "previousUserQuery": "accounts and signing in"
  }
}
``` 

##  <a name="json-response-to-return-non-initial-answer-and-follow-up-prompts"></a>JSON-antwoord om niet-eerste antwoord en follow-up prompts te retourneren

De QnA Maker _GenerateAnswer_ JSON-antwoord bevat de follow-up aanwijzingen in de `context` eigenschap van het eerste item in de `answers` object:

```JSON
{
    "answers": [
        {
            "questions": [
                "Give feedback"
            ],
            "answer": "What kind of feedback do you have?",
            "score": 100.0,
            "id": 288,
            "source": "Editorial",
            "metadata": [],
            "context": {
                "isContextOnly": true,
                "prompts": [
                    {
                        "displayOrder": 0,
                        "qnaId": 291,
                        "qna": null,
                        "displayText": "Feedback on an QnA Maker service"
                    },
                    {
                        "displayOrder": 0,
                        "qnaId": 292,
                        "qna": null,
                        "displayText": "Feedback on an existing feature"
                    }
                ]
            }
        }
    ]
}
```

## <a name="displaying-prompts-and-sending-context-in-the-client-application"></a>Prompts worden weergegeven en het verzenden van context in de clienttoepassing 

Als u vragen hebt toegevoegd in uw knowledge base en de stroom in het deelvenster hebt getest, de prompts wordt niet automatisch wordt gestart in de clienttoepassingen weergegeven. U kunt de prompts als knoppen of voorgestelde acties als onderdeel van het antwoord op de query van de gebruiker in client toepassingen weergeven door dit [Bot Framework voorbeeld](https://aka.ms/qnamakermultiturnsample) in uw code. De clienttoepassing wordt de huidige QnA-ID en de gebruikersquery opslaan en doorvoert in de [context-object van de API GenerateAnswer](#json-request-to-return-non-initial-answer-and-follow-up-prompts) voor de volgende gebruikersquery.

## <a name="display-order-supported-in-api"></a>Weergavevolgorde ondersteund in de API

De volgorde, in het JSON-antwoord geretourneerd wordt voor het bewerken van door de API alleen ondersteund. 

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de contextuele gesprekken in de [dialoogvenster voorbeeld](https://aka.ms/qnamakermultiturnsample) of meer [conceptuele bot ontwerpen voor meerdere inschakelen gesprekken](https://docs.microsoft.com/azure/bot-service/bot-builder-conversations?view=azure-bot-service-4.0).

> [!div class="nextstepaction"]
> [Een kennisdatabase migreren](../Tutorials/migrate-knowledge-base.md)
