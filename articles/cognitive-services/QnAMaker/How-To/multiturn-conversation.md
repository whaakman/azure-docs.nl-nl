---
title: Meerdere inschakelen gesprekken
titleSuffix: Azure Cognitive Services
description: Prompts en context gebruiken voor het beheren van de meerdere schakelt, bekend als meerdere inschakelen, voor uw bot in één vraag naar een andere. Meerdere inschakelen is de mogelijkheid om een back en uiteengezet conversatie waar de volgende vragen en antwoorden van invloed op de vorige vraag context.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 06/12/2019
ms.author: diberry
ms.openlocfilehash: 1e46c935d298f2fe7ebfa4bce471288c9ab8a606
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67271944"
---
# <a name="use-follow-up-prompts-to-create-multiple-turns-of-a-conversation"></a>Gebruik follow-up aanwijzingen voor het maken van meerdere wordt van een gesprek

Opvolging prompts en context gebruiken voor het beheren van de meerdere schakelt, ook wel _meerdere inschakelen_, voor uw bot in één vraag naar een andere.

Bekijk de volgende demonstratie video om te zien hoe dit werkt.

[![](../media/conversational-context/youtube-video.png)](https://aka.ms/multiturnexample).

## <a name="what-is-a-multi-turn-conversation"></a>Wat is een gesprek met meerdere inschakelen?

Enkele vragen kunnen niet worden beantwoord in een enkel inschakelen. Bij het ontwerpen van uw client-toepassing (chatbot) gesprekken, kan een gebruiker een vraag die moet worden gefilterd of verfijnd om te bepalen het juiste antwoord. Deze stroom via de vragen is mogelijk door middel van de gebruiker met de **opvolging vraagt**.

Wanneer de gebruiker wordt gevraagd de vraag, QnA Maker retourneert het antwoord _en_ follow-up prompts. Hiermee kunt u de follow-up vragen als opties presenteren. 

## <a name="example-multi-turn-conversation-with-chat-bot"></a>Voorbeeld van meerdere inschakelen conversatie met chatbot

Een chatbot beheert de conversatie met de gebruiker, vraag door vraag om te bepalen het definitieve antwoord.

![In de conversatie flow conversatie-status in een systeem met meerdere inschakelen dialoogvenster te beheren door op te geven van de instructies in de antwoorden weergegeven als opties om door te gaan van de conversatie.](../media/conversational-context/conversation-in-bot.png)

In de voorgaande afbeelding is de gebruiker heeft ingevoerd `My account`. De knowledge base heeft 3 gekoppelde QnA-paren. Selecteer een van de drie opties voor het verfijnen van het antwoord moet de gebruiker. In de knowledge base heeft de vraag (#1), drie follow-up vragen, die zijn gepresenteerd in de chatbot als drie opties (2). 

Wanneer de gebruiker selecteert een keuze (3), wordt de volgende lijst met opties (4) verfijnen weergegeven. Dit kunt (5) blijven totdat het correct en definitieve antwoord (6) wordt bepaald.

De voorgaande afbeelding heeft **inschakelen van meerdere inschakelen** geselecteerd om te kunnen prompts weergegeven. 

### <a name="using-multi-turn-in-a-bot"></a>In een bot met behulp van meerdere inschakelen

U moet de clienttoepassing voor het beheren van de contextuele conversatie wijzigen. U moet toevoegen [code naar uw bot](https://github.com/microsoft/BotBuilder-Samples/tree/master/experimental/qnamaker-prompting) om te zien van de aanwijzingen.  

## <a name="create-a-multi-turn-conversation-from-a-documents-structure"></a>Een gesprek met meerdere inschakelen van de structuur van een document maken

Wanneer u een kennisdatabase maakt, ziet u een optionele selectievakje in om in te schakelen extractie van meerdere inschakelen. 

![Wanneer u een kennisdatabase maakt, ziet u een optionele selectievakje in om in te schakelen extractie van meerdere inschakelen.](../media/conversational-context/enable-multi-turn.png)

Als u deze optie selecteert bij het importeren van een document, kunt u de conversatie met meerdere inschakelen impliciete van de structuur. Als die structuur bestaat, maakt QnA Maker de follow-up vragen QnA paren voor u. 

Meerdere inschakelen structuur kan alleen worden afgeleid van URL's, PDF-bestand of DOCX bestanden. 

De volgende afbeelding van een Microsoft Surface [PDF-bestand](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/product-manual.pdf) is bedoeld om te worden gebruikt als een handmatige. Vanwege de grootte van deze PDF-bestand, de Azure QnA Maker-resource de prijscategorie van B zoekopdracht vereist (15 indexen) of hoger. 

![! [Als u een document importeert, contextuele conversatie kan worden impliciet van de structuur. Als die structuur bestaat, maakt QnA Maker de follow-up vragen QnA-paren voor u, als onderdeel van het document importeren.] (.. / media/conversational-context/import-file-with-conversational-structure.png)](../media/conversational-context/import-file-with-conversational-structure.png#lightbox)

Bij het importeren van het PDF-document, bepaalt de QnA Maker follow-up prompts van de structuur om conversatie stroom te maken. 

1. In **stap 1**, selecteer **maken van een kennisdatabase** via de bovenste navigatiebalk.
1. In **stap 2**, maken of een bestaande Writer-service te gebruiken. Zorg ervoor dat u een Writer-service met een service voor zoeken van B (15 indexen) of hoger omdat het oppervlak handmatig PDF-bestand te groot voor een kleinere laag is.
1. In **stap 3**, voer een naam op voor uw knowledge base, zoals `Surface manual`.
1. In **stap 4**, selecteer **inschakelen van meerdere inschakelen extractie van URL's, .pdf of DOCX-bestanden.** Selecteer de URL voor de Surface handmatige

    ```text
    https://github.com/Azure-Samples/cognitive-services-sample-data-files/raw/master/qna-maker/data-source-formats/product-manual.pdf
    ```

1. Selecteer de **maken uw KB** knop. 

    Zodra de kennis die is gemaakt, wordt een weergave van de vraag en antwoord-paren worden weergegeven.

## <a name="show-questions-and-answers-with-context"></a>Vragen en antwoorden met context weergeven

Verminder de weergegeven voor alleen degene met contextuele gesprekken paren van vragen en antwoorden. 

1. Selecteer **weergaveopties**en selecteer vervolgens **context weergeven (PREVIEW)** . De lijst bevat de vraag en antwoord paren met follow-up prompts. 

    ![Filteren van vraag en antwoord paren door contextuele gesprekken](../media/conversational-context/filter-question-and-answers-by-context.png)

2. De context van meerdere inschakelen wordt weergegeven in de eerste kolom.

    ![! [Bij het importeren van het PDF-document, bepaalt met QnA Maker follow-up prompts van de structuur om conversatie stroom te maken. ](.. / media/conversational-context/surface-manual-pdf-follow-up-prompt.png)](../media/conversational-context/surface-manual-pdf-follow-up-prompt.png#lightbox)

    In de voorgaande afbeelding geeft #1 vet in de kolom, dat geeft aan de huidige vraag dat. De bovenliggende vraag is het eerste item in de rij. Vragen hieronder zijn de gekoppelde paren van vragen en antwoorden. Deze items zijn geselecteerd, zodat u meteen aan de andere artikelen van de context kunt gaan. 

## <a name="add-existing-qna-pair-as-follow-up-prompt"></a>Bestaande QnA paar toevoegen als opvolgingsactiviteit prompt

De oorspronkelijke vraag `My account` heeft follow-up prompts zoals `Accounts and signing in`. 

![De oorspronkelijke vraag van 'Mijn account' correct retourneert de '-Accounts en meldt u zich' beantwoorden en al de follow-up aanwijzingen die is gekoppeld.](../media/conversational-context/detected-and-linked-follow-up-prompts.png)

Een follow-up prompt toevoegen aan een bestaande QnA-paar dat momenteel niet is gekoppeld. Omdat de vraag is niet gekoppeld aan een paar QnA, moet de huidige weergave-instelling te wijzigen.

1. Om een sleutelpaar met een bestaande QnA als een follow-up prompt koppeling, selecteer de rij voor het paar vragen en antwoorden. Zoek op voor de Surface handmatige `Sign out` te verminderen van de lijst.
1. In de rij voor `Signout`, selecteer **toevoegen follow-up prompt** uit de **antwoord** kolom.
1. In de **opvolging prompt (PREVIEW)** pop-upvenster, voert u het volgende:

    |Veld|Value|
    |--|--|
    |Tekst weergeven|`Turn off the device`. Dit is de aangepaste tekst die u wilt weergeven in de follow-up prompt.|
    |Alleen-context|Geselecteerd. Dit antwoord wordt alleen geretourneerd als de vraag Hiermee geeft u de context.|
    |Koppeling naar antwoord|Voer `Use the sign-in screen` te vinden van het bestaande QnA-paar.|


1.  Een overeenkomst wordt geretourneerd. Selecteer dit antwoord als de follow-up **opslaan**. 

    ![Zoeken naar de Follow-up-prompt koppeling antwoord dialoogvenster voor een bestaande antwoord aan met behulp van de tekst van het antwoord.](../media/conversational-context/search-follow-up-prompt-for-existing-answer.png)

1. Nadat u de follow-up prompt hebt toegevoegd, moet u selecteren **opslaan en trainen** in de bovenste navigatiebalk.
  
### <a name="edit-the-display-text"></a>De weergegeven tekst bewerken 

Wanneer een follow-up prompt is gemaakt, en een sleutelpaar met een bestaande QnA is geselecteerd als de **koppeling naar antwoord**, kunt u nieuwe **tekst weer te geven**. Deze tekst is geen vervanging voor de bestaande vraag en een nieuwe alternatieve vraag is niet toegevoegd. Dit staat los van deze waarden. 

1. Als u wilt bewerken in de weergegeven tekst, zoek en selecteer de vraag in de **Context** veld.
1. Selecteer op de rij van deze vraag, de follow-up prompt in de kolom antwoord. 
1. Selecteer de weergegeven tekst die u wilt bewerken, en selecteer vervolgens **bewerken**.

    ![Selecteer de weergegeven tekst die u wilt bewerken, en selecteer vervolgens bewerken.](../media/conversational-context/edit-existing-display-text.png)

1. De **opvolging prompt** pop-upvenster kunt u de bestaande tekst wijzigen. 
1. Wanneer u klaar bent bewerken van de tekst weergeven, selecteert u **opslaan**. 
1. Houd er rekening mee te selecteren **opslaan en trainen** in de bovenste navigatiebalk.


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

## <a name="add-new-qna-pair-as-follow-up-prompt"></a>Nieuwe QnA set toevoegen als opvolgingsactiviteit prompt

Een nieuwe QnA set toevoegen aan de knowledge base. De QnA-paar moet zijn gekoppeld aan een bestaande vraag als een follow-up prompt.

1. Zoek via de werkbalk van de knowledge base, en selecteer het bestaande paar QnA voor `Accounts and Signing In`. 

1. In de **antwoord** kolom voor deze vraag, selecteer **toevoegen follow-up prompt**. 
1. De **opvolging prompt (PREVIEW)** , een nieuwe follow-up prompt maken door te voeren van de volgende waarden: 

    |Tekstveld|Value|
    |--|--|
    |**Tekst weergeven**|`Create a Windows Account`. Dit is de aangepaste tekst die u wilt weergeven in de follow-up prompt.|
    |**Alleen-context**|Geselecteerd. Dit antwoord wordt alleen geretourneerd als de vraag Hiermee geeft u de context.|
    |**Koppeling om te beantwoorden**|Voer de volgende tekst als het antwoord:<br>`[Create](https://account.microsoft.com/) a Windows account with a new or existing email account.`<br>Wanneer u opslaan en de database te trainen, wordt deze tekst geconverteerd naar |
    |||

    ![Nieuwe vragen QnA maken](../media/conversational-context/create-child-prompt-from-parent.png)


1. Selecteer **nieuw** Selecteer **opslaan**. 

    Dit een nieuwe vraag en antwoord-set gemaakt en gekoppeld van de geselecteerde vraag als een follow-up prompt. De **Context** kolom voor beide vragen een follow-up vragen relatie aangeeft. 

1. Wijzig de **opties weergeven** naar [context weergeven](#show-questions-and-answers-with-context).

    De nieuwe vraag laat zien hoe deze is gekoppeld.

    ![Maak een nieuwe follow-up prompt ](../media/conversational-context/new-qna-follow-up-prompt.png)

    De bovenliggende vraag ziet u de nieuwe vraag als een van de opties.

    ![! [De kolom Context voor beide vragen geeft aan dat een follow-up vragen relatie.] (.. / media/conversational-context/child-prompt-created.png)](../media/conversational-context/child-prompt-created.png#lightbox)

1. Nadat u de follow-up prompt hebt toegevoegd, moet u selecteren **opslaan en trainen** in de bovenste navigatiebalk.

## <a name="enable-multi-turn-when-testing-follow-up-prompts"></a>Meerdere inschakelen als testen worden opgevolgd vraagt inschakelen

Wanneer de vraag met opvolging testen vraagt de **Test** venster **inschakelen van meerdere inschakelen**, en voer uw vraag. Het antwoord bevat de follow-up aanwijzingen.

![Bij het testen van de vraag in het deelvenster, bevat het antwoord de follow-up aanwijzingen.](../media/conversational-context/test-pane-with-question-having-follow-up-prompts.png)

Als u meerdere inschakelen niet inschakelt, kan het antwoord wordt geretourneerd, maar follow-up prompts worden niet geretourneerd.

## <a name="json-request-to-return-initial-answer-and-follow-up-prompts"></a>JSON-aanvraag om terug te keren eerste antwoord en follow-up prompts

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

## <a name="json-response-to-return-initial-answer-and-follow-up-prompts"></a>JSON-antwoord om terug te keren eerste antwoord en follow-up prompts

De vorige sectie aangevraagd een antwoord en opvolging aanwijzingen voor het `Accounts and signing in`. Het antwoord bevat de prompt informatie, dat zich bevindt in `answers[0].context`, bevatten de tekst die moet worden weergegeven voor de gebruiker. 

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
                "promptsToAdd": [
                    {
                        "displayOrder": 0,
                        "qnaId": 16,
                        "qna": null,
                        "displayText": "Use the sign-in screen"
                    },
                    {
                        "displayOrder": 1,
                        "qnaId": 17,
                        "qna": null,
                        "displayText": "Use Windows Hello to sign in"
                    },
                    {
                        "displayOrder": 2,
                        "qnaId": 18,
                        "qna": null,
                        "displayText": "Sign out"
                    },
                    {
                        "displayOrder": 0,
                        "qnaId": 79,
                        "qna": null,
                        "displayText": "Create a Windows Account"
                    }
                ],
                "promptsToDelete":[]
            }
        },
        {
            "questions": [
                "Sign out"
            ],
            "answer": "**Sign out**\n\nHere's how to sign out: \n\n Go to Start , and right-click your name. Then select Sign out. ",
            "score": 38.01,
            "id": 18,
            "source": "product-manual.pdf",
            "metadata": [],
            "context": {
                "isContextOnly": true,
                "promptsToAdd": [
                    {
                        "displayOrder": 0,
                        "qnaId": 16,
                        "qna": null,
                        "displayText": "Turn off the device"
                    }
                ],
                "promptsToDelete":[]
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
                "promptsToAdd": [],
                "promptsToDelete":[]
            }
        }
    ]
}
```

De `promptsToAdd` matrix bevat tekst in de `displayText` eigenschap en de `qnaId` flow waarde, zodat u kunt deze vragen te beantwoorden als de volgende weergegeven opties in de conversatie weergeven, klikt u vervolgens verzenden van de geselecteerde waarde voor QnA Maker in de volgende aanvraag. 

<!--

The `promptsToDelete` array provides the ...

-->

## <a name="json-request-to-return-non-initial-answer-and-follow-up-prompts"></a>JSON-aanvraag om niet-eerste antwoord en follow-up prompts te retourneren

Vul de `context` object om op te nemen van vorige context.

In de volgende JSON-aanvraag, is de huidige vraag `Use Windows Hello to sign in` en de vorige vraag is `Accounts and signing in`. 

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

##  <a name="json-response-to-return-non-initial-answer-and-follow-up-prompts"></a>JSON-antwoord om niet-eerste antwoord en follow-up prompts te retourneren

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
                "promptsToAdd": [],
                "promptsToDelete":[]
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
                "promptsToAdd": [
                    {
                        "displayOrder": 0,
                        "qnaId": 4,
                        "qna": null,
                        "displayText": "Ports and connectors"
                    }
                ],
                "promptsToDelete":[]
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
                "promptsToAdd": [],
                "promptsToDelete":[]
            }
        }
    ]
}
```

## <a name="query-the-knowledge-base-with-the-qna-id"></a>De knowledge Base met de QnA-ID

In de eerste vraag-antwoord, follow-up prompts en de bijbehorende `qnaId` wordt geretourneerd. Nu dat u de ID hebt, kunt u dit kunt doorgeven in de aanvraagtekst voor de follow-up prompt. Als hoofdtekst van de aanvraag bevat de `qnaId`, en het contextobject (die de vorige QnA-eigenschappen bevat), en vervolgens GenerateAnswer de exacte vraag wordt geretourneerd door de ID in plaats van het classificatiealgoritme het antwoord vinden door de tekst van de vraag. 

## <a name="displaying-prompts-and-sending-context-in-the-client-application"></a>Prompts worden weergegeven en het verzenden van context in de clienttoepassing 

U hebt toegevoegd prompts in uw knowledge base en de stroom in het deelvenster getest. Nu moet u deze aanwijzingen gebruiken in de clienttoepassing. Voor Bot Framework, de prompts wordt niet automatisch wordt gestart in de clienttoepassingen weergegeven. U kunt de prompts als knoppen of voorgestelde acties als onderdeel van het antwoord op de query van de gebruiker in client toepassingen weergeven door dit [Bot Framework voorbeeld](https://aka.ms/qnamakermultiturnsample) in uw code. De clienttoepassing wordt de huidige QnA-ID en de gebruikersquery opslaan en doorvoert in de [context-object van de API GenerateAnswer](#json-request-to-return-non-initial-answer-and-follow-up-prompts) voor de volgende gebruikersquery. 

## <a name="display-order-supported-in-api"></a>Weergavevolgorde ondersteund in de API

De [tekst weer te geven en de weergavevolgorde](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/update#promptdto), in het JSON-antwoord geretourneerd, wordt ondersteund voor bewerking door de [API Update](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/update). 

<!--

FIX - Need to go to parent, then answer column, then edit answer. 

-->

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de contextuele gesprekken in de [dialoogvenster voorbeeld](https://aka.ms/qnamakermultiturnsample) of meer [conceptuele bot ontwerpen voor meerdere inschakelen gesprekken](https://docs.microsoft.com/azure/bot-service/bot-builder-conversations?view=azure-bot-service-4.0).

> [!div class="nextstepaction"]
> [Een kennisdatabase migreren](../Tutorials/migrate-knowledge-base.md)
