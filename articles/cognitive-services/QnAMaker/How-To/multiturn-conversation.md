---
title: Meerdere inschakelen gesprekken
titleSuffix: Azure Cognitive Services
description: Prompts en context gebruiken voor het beheren van de meerdere schakelt, bekend als meerdere inschakelen, voor uw bot in één vraag naar een andere. Meerdere inschakelen is de mogelijkheid om een gesprek en weer schakelen waar de volgende vragen en antwoorden van invloed op de vorige vraag context.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 06/26/2019
ms.author: diberry
ms.openlocfilehash: 10249375922b47a40f71a60938cdd12ffe0f9b54
ms.sourcegitcommit: 79496a96e8bd064e951004d474f05e26bada6fa0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2019
ms.locfileid: "67508137"
---
# <a name="use-follow-up-prompts-to-create-multiple-turns-of-a-conversation"></a>Gebruik follow-up aanwijzingen voor het maken van meerdere wordt van een gesprek

Opvolging prompts en context gebruiken voor het beheren van de meerdere schakelt, ook wel _meerdere inschakelen_, voor uw bot in één vraag naar een andere.

Om te zien hoe meerdere inschakelen werkt, door de volgende demonstratie video te bekijken:

[![De conversatie meerdere inschakelen in de QnA Maker](../media/conversational-context/youtube-video.png)](https://aka.ms/multiturnexample)

## <a name="what-is-a-multi-turn-conversation"></a>Wat is een gesprek met meerdere inschakelen?

Enkele vragen kunnen niet worden beantwoord in een enkel inschakelen. Bij het ontwerpen van uw client-toepassing (chatbot) gesprekken, kan een gebruiker een vraag die moet worden gefilterd of verfijnd om te bepalen het juiste antwoord. Maakt u deze stroom via de vragen mogelijk door middel van de gebruiker met de *opvolging vraagt*.

Wanneer een gebruiker wordt gevraagd een vraag, QnA Maker retourneert het antwoord _en_ follow-up prompts. Dit antwoord kunt u de follow-up vragen als opties presenteren. 

## <a name="example-multi-turn-conversation-with-chat-bot"></a>Voorbeeld van meerdere inschakelen conversatie met chatbot

Een chatbot beheert een gesprek met een gebruiker om te bepalen het definitieve antwoord met meerdere inschakelen, zoals wordt weergegeven in de volgende afbeelding:

![Een dialoogvenster met meerdere inschakelen vragen die een gebruiker via een gesprek](../media/conversational-context/conversation-in-bot.png)

In de vorige afbeelding is een gebruiker een gesprek gestart door in te voeren **Mijn account**. De knowledge base heeft drie gekoppelde paren van vragen en antwoorden. Om het antwoord te zetten, selecteert de gebruiker een van de drie opties in het knowledge base. De vraag (#1), heeft drie follow-up aanwijzingen die worden weergegeven in de chatbot als drie opties (2). 

Wanneer de gebruiker selecteert een optie (3), wordt de volgende lijst met opties (4) verfijnen weergegeven. Deze volgorde blijft (5) totdat de gebruiker de juiste, definitieve antwoord (6 bepaalt).

> [!NOTE]
> In de voorgaande afbeelding is de **inschakelen van meerdere inschakelen** selectievakje in om ervoor te zorgen dat de prompts worden weergegeven is geselecteerd. 

### <a name="use-multi-turn-in-a-bot"></a>Meerdere inschakelen in een bot gebruiken

Voor het beheren van de contextuele conversatie, wijzigt u uw clienttoepassing met [code toevoegen aan uw bot](https://github.com/microsoft/BotBuilder-Samples/tree/master/experimental/qnamaker-prompting). De code toe te voegen, kunnen gebruikers de prompts te zien.  

## <a name="create-a-multi-turn-conversation-from-a-documents-structure"></a>Een gesprek met meerdere inschakelen van de structuur van een document maken

Bij het maken van een kennisdatabase de **vullen uw KB** sectie geeft een **inschakelen van meerdere inschakelen extractie van URL's, .pdf of DOCX-bestanden** selectievakje. 

![Selectievakje in voor het inschakelen van meerdere inschakelen extractie](../media/conversational-context/enable-multi-turn.png)

Wanneer u deze optie voor een geïmporteerde document selecteert, kunt u de conversatie met meerdere inschakelen impliciete van de documentstructuur. Als die structuur bestaat, QnA Maker de follow-up prompt die paren vragen en antwoorden voor u gemaakt als onderdeel van het importproces. 

Meerdere inschakelen structuur kan worden afgeleid van URL's, PDF-bestanden, alleen of DOCX-bestanden. Een afbeelding van weergeven voor een voorbeeld van de structuur, een [handmatige PDF-bestand van Microsoft Surface gebruiker](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/product-manual.pdf). Vanwege de grootte van deze PDF-bestand, de QnA Maker-resource vereist een **zoeken prijscategorie** van **B** (15 indexen) of hoger. 

![! [Voorbeeld van de structuur in een handmatige gebruiker] (.. / media/conversational-context/import-file-with-conversational-structure.png)](../media/conversational-context/import-file-with-conversational-structure.png#lightbox)

Bij het importeren van het PDF-document bepaalt QnA Maker worden opgevolgd vraagt van de structuur om conversatie stroom te maken. 

1. Selecteer in de QnA Maker, **maken van een kennisdatabase**.
1. Maak of gebruik een bestaande QnA Maker-service. In het voorgaande voorbeeld van de Microsoft Surface, omdat het PDF-bestand te groot voor een kleinere laag is, gebruiken een QnA Maker-service met een **Search-service** van **B** (15 indexen) of hoger.
1. Voer een naam op voor uw knowledge base, zoals **Surface handmatig**.
1. Selecteer de **inschakelen van meerdere inschakelen extractie van URL's, .pdf of DOCX-bestanden** selectievakje. 
1. Selecteer de Surface handmatige URL **https://github.com/Azure-Samples/cognitive-services-sample-data-files/raw/master/qna-maker/data-source-formats/product-manual.pdf** .

1. Selecteer de **maken uw KB** knop. 

    Nadat de knowledge base is gemaakt, wordt een overzicht van de vraag en antwoord-paren weergegeven.

## <a name="show-questions-and-answers-with-context"></a>Vragen en antwoorden met context weergeven

De weergegeven paren van vragen en antwoorden beperken tot alleen degenen met contextuele conversaties. 

Selecteer **weergaveopties**, en selecteer vervolgens **context weergeven (PREVIEW)** . De lijst bevat de vraag en antwoord-paren die opvolging prompts bevatten. 

![Filteren van paren van vragen en antwoorden op contextuele gesprekken](../media/conversational-context/filter-question-and-answers-by-context.png)

De context van meerdere inschakelen wordt weergegeven in de eerste kolom.

![![De kolom "Context (PREVIEW)"] (.. / media/conversational-context/surface-manual-pdf-follow-up-prompt.png)](../media/conversational-context/surface-manual-pdf-follow-up-prompt.png#lightbox)

In de voorgaande afbeelding **#1** geeft aan dat vet in de kolom, dat geeft aan de huidige vraag dat. De bovenliggende vraag is het eerste item in de rij. Vragen hieronder zijn de gekoppelde paren van vragen en antwoorden. Deze items zijn geselecteerd, zodat u meteen aan de andere artikelen van de context kunt gaan. 

## <a name="add-an-existing-question-and-answer-pair-as-a-follow-up-prompt"></a>Een sleutelpaar met een bestaande vraag en antwoord toevoegen als een follow-up prompt

De oorspronkelijke vraag **Mijn account**, heeft follow-up aanwijzingen, zoals **Accounts en meldt u zich**. 

![De 'Accounts en meldt u zich' antwoorden en follow-up prompts](../media/conversational-context/detected-and-linked-follow-up-prompts.png)

Een follow-up prompt toevoegen aan een bestaande vraag en antwoord-paar dat momenteel niet is gekoppeld. Omdat de vraag is niet gekoppeld aan een paar vragen en antwoorden, moet de huidige weergave-instelling worden gewijzigd.

1. Om een sleutelpaar met een bestaande vraag en antwoord als een follow-up prompt koppeling, selecteer de rij voor het paar vragen en antwoorden. Zoek op voor de Surface handmatige **Afmelden** te verminderen van de lijst.
1. In de rij voor **die afmelden wordt weergegeven**, in de **antwoord** kolom, selecteer **toevoegen follow-up prompt**.
1. In de velden in de **opvolging prompt (PREVIEW)** pop-upvenster, voer de volgende waarden:

    |Veld|Value|
    |--|--|
    |Tekst weergeven|Voer **uitschakelen op het apparaat**. Dit is de aangepaste tekst die moet worden weergegeven in de follow-up prompt.|
    |Alleen-context| Schakel dit selectievakje in. Alleen als de vraag Hiermee geeft u de context, wordt er een antwoord geretourneerd.|
    |Koppeling om te beantwoorden|Voer **gebruiken het aanmeldingsscherm** het bestaande paar van de vraag en antwoord vinden.|


1.  Een overeenkomst wordt geretourneerd. Dit antwoord selecteren als de follow-up en selecteer vervolgens **opslaan**. 

    ![De pagina "Opvolging prompt (PREVIEW)"](../media/conversational-context/search-follow-up-prompt-for-existing-answer.png)

1. Nadat u de follow-up prompt hebt toegevoegd, selecteert u **opslaan en trainen** in de bovenste navigatiebalk.
  
### <a name="edit-the-display-text"></a>De weergegeven tekst bewerken 

Wanneer een follow-up prompt is gemaakt, en een sleutelpaar met een bestaande vraag en antwoord is ingevoerd als de **koppeling naar antwoord**, kunt u nieuwe **tekst weer te geven**. Deze tekst niet vervangen door de bestaande vraag en een nieuwe alternatieve vraag niet toegevoegd. Dit staat los van deze waarden. 

1. Als u wilt bewerken in de weergegeven tekst, zoek en selecteer de vraag in de **Context** veld.
1. Selecteer de follow-up prompt in de kolom antwoord in de rij voor deze vraag. 
1. Selecteer de weergegeven tekst die u wilt bewerken, en selecteer vervolgens **bewerken**.

    ![De opdracht bewerken voor de weergegeven tekst](../media/conversational-context/edit-existing-display-text.png)

1. In de **opvolging prompt** pop-upvenster wijzigen van de bestaande tekst. 
1. Wanneer u klaar bent bewerken van de tekst weergeven, selecteert u **opslaan**. 
1. In de bovenste navigatiebalk **opslaan en trainen**.


<!--

## To find the best prompt answer, add metadata to follow-up prompts 

If you have several follow-up prompts for a specific question-and-answer pair but you know, as the knowledge base manager, that not all prompts should be returned, use metadata to categorize the prompts in the knowledge base. You can then send the metadata from the client application as part of the GenerateAnswer request.

In the knowledge base, when a question-and-answer pair is linked to follow-up prompts, the metadata filters are applied first, and then the follow-ups are returned.

1. Add metadata to each of the two follow-up question-and-answer pairs:

    |Question|Add metadata|
    |--|--|
    |*Feedback on a QnA Maker service*|"Feature":"all"|
    |*Feedback on an existing feature*|"Feature":"one"|
    
    ![The "Metadata tags" column for adding metadata to a follow-up prompt](../media/conversational-context/add-metadata-feature-to-follow-up-prompt.png) 

1. Select **Save and train**. 

    When you send the question **Give feedback** with the metadata filter **Feature** with a value of **all**, only the question-and-answer pair with that metadata is returned. QnA Maker doesn't return both question-and-answer pairs, because both don't match the filter. 

-->

## <a name="add-a-new-question-and-answer-pair-as-a-follow-up-prompt"></a>Een nieuwe set van de vraag en antwoord toevoegen als een follow-up prompt

Wanneer u een nieuwe set van vraag en antwoord aan de knowledge base toevoegt, moet elk paar zijn gekoppeld aan een bestaande vraag als een follow-up prompt.

1. Zoek op de werkbalk knowledge base en selecteer het bestaande paar vragen en antwoorden voor **Accounts en meldt u zich**. 

1. In de **antwoord** kolom voor deze vraag, selecteer **toevoegen follow-up prompt**. 
1. Onder **opvolging prompt (PREVIEW)** , een nieuwe follow-up prompt maken door te voeren van de volgende waarden: 

    |Veld|Value|
    |--|--|
    |Tekst weergeven|*Een Windows-Account maken*. De aangepaste tekst die moet worden weergegeven in de follow-up prompt.|
    |Alleen-context|Schakel dit selectievakje in. Dit antwoord wordt geretourneerd, alleen als de vraag Hiermee geeft u de context.|
    |Koppeling om te beantwoorden|Voer de volgende tekst als het antwoord:<br>*[Maak](https://account.microsoft.com/) een Windows-account met een nieuwe of bestaande e-mailaccount*.<br>Wanneer u opslaan en de database te trainen, wordt deze tekst wordt geconverteerd. |
    |||

    ![Maak een nieuwe vraag en antwoord](../media/conversational-context/create-child-prompt-from-parent.png)


1. Selecteer **nieuw**, en selecteer vervolgens **opslaan**. 

    Deze actie wordt een nieuwe set van vraag en antwoord en koppelingen de geselecteerde vraag gemaakt als een follow-up prompt. De **Context** kolom voor beide vragen een follow-up vragen relatie aangeeft. 

1. Selecteer **weergaveopties**, en selecteer vervolgens [ **context weergeven (PREVIEW)** ](#show-questions-and-answers-with-context).

    De nieuwe vraag laat zien hoe deze gekoppeld.

    ![Maak een nieuwe follow-up prompt](../media/conversational-context/new-qna-follow-up-prompt.png)

    De bovenliggende vraag wordt een nieuwe vraag weergegeven als een van de opties.

    ![! [De kolom Context voor beide vragen geeft aan dat een follow-up vragen relatie] (.. / media/conversational-context/child-prompt-created.png)](../media/conversational-context/child-prompt-created.png#lightbox)

1. Nadat u de follow-up prompt hebt toegevoegd, selecteert u **opslaan en trainen** in de bovenste navigatiebalk.

## <a name="enable-multi-turn-during-testing-of-follow-up-prompts"></a>Meerdere inschakelen tijdens het testen van malwareproblemen prompts inschakelen

Wanneer u de vraag met opvolging testen wordt gevraagd het **testen** venster **inschakelen van meerdere inschakelen**, en voer vervolgens uw vraag. Het antwoord bevat de follow-up aanwijzingen.

![Het antwoord bevat de follow-up prompts](../media/conversational-context/test-pane-with-question-having-follow-up-prompts.png)

Als u meerdere inschakelen niet inschakelt, het antwoord wordt geretourneerd, maar follow-up prompts worden niet geretourneerd.

## <a name="a-json-request-to-return-an-initial-answer-and-follow-up-prompts"></a>Een JSON-aanvraag om een eerste antwoord en follow-up prompts te retourneren

Gebruik de lege `context` object om te vragen van het antwoord op de vraag van de gebruiker en follow-up prompts bevatten. 

```JSON
{
  "question": "accounts and signing in",
  "top": 10,
  "userId": "Default",
  "isTest": false,
  "context": {}
}
```

## <a name="a-json-response-to-return-an-initial-answer-and-follow-up-prompts"></a>Een JSON-antwoord om terug te keren een eerste antwoord en follow-up prompts

De voorgaande sectie aangevraagd een antwoord en opvolging aanwijzingen voor het **Accounts en meldt u zich**. Het antwoord bevat de prompt informatie bevindt zich onder *antwoorden [0] .context*, en de tekst die moet worden weergegeven voor de gebruiker. 

```JSON
{
    "answers": [
        {
            "questions": [
                "Accounts and signing in"
            ],
            "answer": "**Accounts and signing in**\n\nWhen you set up your Surface, an account is set up for you. You can create additional accounts later for family and friends, so each person using your Surface can set it up just the way he or she likes. For more info, see All about accounts on Surface.com. \n\nThere are several ways to sign in to your Surface Pro 4: ",
            "score": 100.0,
            "id": 15,
            "source": "product-manual.pdf",
            "metadata": [],
            "context": {
                "isContextOnly": true,
                "prompts": [
                    {
                        "displayOrder": 0,
                        "qnaId": 16,
                        "qna": null,
                        "displayText": "Use the sign-in screen"
                    }
                ]
            }
        },
        {
            "questions": [
                "Sign out"
            ],
            "answer": "**Sign out**\n\nHere's how to sign out: \n\n Go to Start, and right-click your name. Then select Sign out. ",
            "score": 38.01,
            "id": 18,
            "source": "product-manual.pdf",
            "metadata": [],
            "context": {
                "isContextOnly": true,
                "prompts": [
                    {
                        "displayOrder": 0,
                        "qnaId": 16,
                        "qna": null,
                        "displayText": "Turn off the device"
                    }
                ]
            }
        },
        {
            "questions": [
                "Use the sign-in screen"
            ],
            "answer": "**Use the sign-in screen**\n\n1.  \n\nTurn on or wake your Surface by pressing the power button. \n\n2.  \n\nSwipe up on the screen or tap a key on the keyboard. \n\n3.  \n\nIf you see your account name and account picture, enter your password and select the right arrow or press Enter on your keyboard. \n\n4.  \n\nIf you see a different account name, select your own account from the list at the left. Then enter your password and select the right arrow or press Enter on your keyboard. ",
            "score": 27.53,
            "id": 16,
            "source": "product-manual.pdf",
            "metadata": [],
            "context": {
                "isContextOnly": true,
                "prompts": []
            }
        }
    ]
}
```

De `prompts` matrix bevat tekst in de `displayText` eigenschap en de `qnaId` waarde. U kunt deze vragen te beantwoorden kunt weergeven, zoals de volgende weergegeven opties in de conversatie flow en verzend de geselecteerde `qnaId` terug naar de QnA Maker in de volgende aanvraag. 

<!--

The `promptsToDelete` array provides the ...

-->

## <a name="a-json-request-to-return-a-non-initial-answer-and-follow-up-prompts"></a>Een JSON-aanvraag om een niet-eerste antwoord en follow-up prompts te retourneren

Vul de `context` object om op te nemen van de vorige context.

In de volgende JSON-aanvraag, is de huidige vraag *gebruik Windows Hello aan te melden bij* en de vorige vraag is *accounts en meldt u zich*. 

```JSON
{
  "question": "Use Windows Hello to sign in",
  "top": 10,
  "userId": "Default",
  "isTest": false,
  "qnaId": 17,
  "context": {
    "previousQnAId": 15,
    "previousUserQuery": "accounts and signing in"
  }
}
``` 

##  <a name="a-json-response-to-return-a-non-initial-answer-and-follow-up-prompts"></a>Een JSON-antwoord om een niet-eerste antwoord en follow-up prompts te retourneren

De QnA Maker _GenerateAnswer_ JSON-antwoord bevat de follow-up aanwijzingen in de `context` eigenschap van het eerste item in de `answers` object:

```JSON
{
    "answers": [
        {
            "questions": [
                "Use Windows Hello to sign in"
            ],
            "answer": "**Use Windows Hello to sign in**\n\nSince Surface Pro 4 has an infrared (IR) camera, you can set up Windows Hello to sign in just by looking at the screen. \n\nIf you have the Surface Pro 4 Type Cover with Fingerprint ID (sold separately), you can set up your Surface sign you in with a touch. \n\nFor more info, see What is Windows Hello? on Windows.com. ",
            "score": 100.0,
            "id": 17,
            "source": "product-manual.pdf",
            "metadata": [],
            "context": {
                "isContextOnly": true,
                "prompts": []
            }
        },
        {
            "questions": [
                "Meet Surface Pro 4"
            ],
            "answer": "**Meet Surface Pro 4**\n\nGet acquainted with the features built in to your Surface Pro 4. \n\nHere’s a quick overview of Surface Pro 4 features: \n\n\n\n\n\n\n\nPower button \n\n\n\n\n\nPress the power button to turn your Surface Pro 4 on. You can also use the power button to put it to sleep and wake it when you’re ready to start working again. \n\n\n\n\n\n\n\nTouchscreen \n\n\n\n\n\nUse the 12.3” display, with its 3:2 aspect ratio and 2736 x 1824 resolution, to watch HD movies, browse the web, and use your favorite apps. \n\nThe new Surface G5 touch processor provides up to twice the touch accuracy of Surface Pro 3 and lets you use your fingers to select items, zoom in, and move things around. For more info, see Surface touchscreen on Surface.com. \n\n\n\n\n\n\n\nSurface Pen \n\n\n\n\n\nEnjoy a natural writing experience with a pen that feels like an actual pen. Use Surface Pen to launch Cortana in Windows or open OneNote and quickly jot down notes or take screenshots. \n\nSee Using Surface Pen (Surface Pro 4 version) on Surface.com for more info. \n\n\n\n\n\n\n\nKickstand \n\n\n\n\n\nFlip out the kickstand and work or play comfortably at your desk, on the couch, or while giving a hands-free presentation. \n\n\n\n\n\n\n\nWi-Fi and Bluetooth® \n\n\n\n\n\nSurface Pro 4 supports standard Wi-Fi protocols (802.11a/b/g/n/ac) and Bluetooth 4.0. Connect to a wireless network and use Bluetooth devices like mice, printers, and headsets. \n\nFor more info, see Add a Bluetooth device and Connect Surface to a wireless network on Surface.com. \n\n\n\n\n\n\n\nCameras \n\n\n\n\n\nSurface Pro 4 has two cameras for taking photos and recording video: an 8-megapixel rear-facing camera with autofocus and a 5-megapixel, high-resolution, front-facing camera. Both cameras record video in 1080p, with a 16:9 aspect ratio. Privacy lights are located on the right side of both cameras. \n\nSurface Pro 4 also has an infrared (IR) face-detection camera so you can sign in to Windows without typing a password. For more info, see Windows Hello on Surface.com. \n\nFor more camera info, see Take photos and videos with Surface and Using autofocus on Surface 3, Surface Pro 4, and Surface Book on Surface.com. \n\n\n\n\n\n\n\nMicrophones \n\n\n\n\n\nSurface Pro 4 has both a front and a back microphone. Use the front microphone for calls and recordings. Its noise-canceling feature is optimized for use with Skype and Cortana. \n\n\n\n\n\n\n\nStereo speakers \n\n\n\n\n\nStereo front speakers provide an immersive music and movie playback experience. To learn more, see Surface sound, volume, and audio accessories on Surface.com. \n\n\n\n\n",
            "score": 21.92,
            "id": 3,
            "source": "product-manual.pdf",
            "metadata": [],
            "context": {
                "isContextOnly": true,
                "prompts": [
                    {
                        "displayOrder": 0,
                        "qnaId": 4,
                        "qna": null,
                        "displayText": "Ports and connectors"
                    }
                ]
            }
        },
        {
            "questions": [
                "Use the sign-in screen"
            ],
            "answer": "**Use the sign-in screen**\n\n1.  \n\nTurn on or wake your Surface by pressing the power button. \n\n2.  \n\nSwipe up on the screen or tap a key on the keyboard. \n\n3.  \n\nIf you see your account name and account picture, enter your password and select the right arrow or press Enter on your keyboard. \n\n4.  \n\nIf you see a different account name, select your own account from the list at the left. Then enter your password and select the right arrow or press Enter on your keyboard. ",
            "score": 19.04,
            "id": 16,
            "source": "product-manual.pdf",
            "metadata": [],
            "context": {
                "isContextOnly": true,
                "prompts": []
            }
        }
    ]
}
```

## <a name="query-the-knowledge-base-with-the-qna-maker-id"></a>De knowledge Base met de QnA Maker-ID

In de eerste vraag-antwoord, follow-up prompts en de bijbehorende `qnaId` wordt geretourneerd. Nu dat u de ID hebt, kunt u dit kunt doorgeven in de aanvraagtekst voor de follow-up prompt. Als hoofdtekst van de aanvraag bevat de `qnaId`, en het contextobject (die de vorige QnA Maker-eigenschappen bevat), en vervolgens GenerateAnswer de exacte vraag wordt geretourneerd door de ID in plaats van het classificatiealgoritme het antwoord vinden door de tekst van de vraag. 

## <a name="display-prompts-and-send-context-in-the-client-application"></a>Prompts worden weergegeven en context in de clienttoepassing verzendt 

U hebt toegevoegd prompts in uw knowledge base en de stroom in het deelvenster getest. Nu moet u deze aanwijzingen gebruiken in de clienttoepassing. Voor Bot Framework, worden niet automatisch de prompts weergegeven in de clienttoepassingen. U kunt de prompts als knoppen of voorgestelde acties als onderdeel van het antwoord op de query van de gebruiker in clienttoepassingen weergeven door dit [Bot Framework voorbeeld](https://aka.ms/qnamakermultiturnsample) in uw code. De clienttoepassing wordt de huidige QnA Maker-ID en de gebruikersquery opslaan en doorvoert in de [context-object van de API GenerateAnswer](#a-json-request-to-return-a-non-initial-answer-and-follow-up-prompts) voor de volgende gebruikersquery. 

## <a name="display-order-is-supported-in-the-update-api"></a>Weergavevolgorde wordt ondersteund in de Update-API

De [tekst weer te geven en de weergavevolgorde](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/update#promptdto), in het JSON-antwoord geretourneerd, wordt ondersteund voor bewerking door de [API Update](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/update). 

<!--

FIX - Need to go to parent, then answer column, then edit answer. 

-->

## <a name="create-knowledge-base-with-multi-turn-prompts-with-the-create-api"></a>Kennisdatabase maken met meerdere inschakelen prompts met de API maken

U kunt een aanvraag kennis maken met meerdere inschakelen prompts met behulp van de [QnA Maker API ' Create '](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/create). Toevoegen van de stappen in de `context` van de eigenschap `prompts` matrix. 


## <a name="add-or-delete-multi-turn-prompts-with-the-update-api"></a>Toevoegen of verwijderen van meerdere inschakelen prompts met de Update-API

U kunt toevoegen of verwijderen van meerdere inschakelen prompts met behulp van de [QnA Maker Update API](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/update).  Toevoegen van de stappen in de `context` van de eigenschap `promptsToAdd` matrix en de `promptsToDelete` matrix. 


## <a name="next-steps"></a>Volgende stappen

Meer informatie over de contextuele gesprekken uit deze [dialoogvenster voorbeeld](https://aka.ms/qnamakermultiturnsample) of lees meer over [conceptuele bot ontwerpen voor meerdere inschakelen gesprekken](https://docs.microsoft.com/azure/bot-service/bot-builder-conversations?view=azure-bot-service-4.0).

> [!div class="nextstepaction"]
> [Een kennisdatabase migreren](../Tutorials/migrate-knowledge-base.md)
