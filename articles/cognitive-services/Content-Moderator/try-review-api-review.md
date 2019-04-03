---
title: 'Toezicht op beoordelingen maken met REST-API-console: Content Moderator'
titlesuffix: Azure Cognitive Services
description: Gebruik de Azure Content Moderator revisie API's te maken van de afbeelding of tekst beoordelingen voor menselijk toezicht.
services: cognitive-services
author: sanjeev3
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: article
ms.date: 03/18/2019
ms.author: sajagtap
ms.openlocfilehash: 254269ccedc92b9dfc164cc4665a8a8513682773
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58882007"
---
# <a name="create-human-reviews-rest"></a>Create human reviews (REST)

[Beoordelingen](./review-api.md#reviews) opslaan en weergeven van inhoud voor menselijke moderators om vast te stellen. Wanneer een gebruiker is een beoordeling voltooid, worden de resultaten verzonden naar een opgegeven retouraanroep-eindpunt. In deze handleiding leert u over het instellen van beoordelingen met behulp van de beoordeling REST-API's via de API-console. Als u inzicht in de structuur van de API's, kunt u eenvoudig deze aanroepen naar elk platform compatibel is met REST poort.

## <a name="prerequisites"></a>Vereisten

- Meld u aan of maak een account op de Content Moderator [beoordelingsprogramma](https://contentmoderator.cognitive.microsoft.com/) site.

## <a name="create-a-review"></a>Maken van een beoordeling

Voor het maken van een beoordeling, gaat u naar de **[controleren - maken](https://westus2.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c4)** API verwijzen naar pagina en selecteer de knop voor uw belangrijkste regio (u kunt dit vinden in de eindpunt-URL op de **referenties** pagina van de [beoordelingsprogramma](https://contentmoderator.cognitive.microsoft.com/)). Hiermee start u de API-console, kunt u eenvoudig maken en uitvoeren van de REST API-aanroepen.

![Evaluatie - Get-regio selecteren](images/test-drive-region.png)

### <a name="enter-rest-call-parameters"></a>REST-aanroep parameters invoeren

Voer waarden in voor **teamName**, en **Ocp-Apim-Subscription-Key**:

- **teamName**: Het team-ID die u hebt gemaakt bij het instellen van uw [beoordelingsprogramma](https://contentmoderator.cognitive.microsoft.com/) account (gevonden in de **Id** veld op het scherm van uw beoordelingsprogramma referenties).
- **Ocp-Apim-Subscription-Key**: Uw Content Moderator-sleutel. U kunt dit vinden op de **instellingen** tabblad van de [beoordelingsprogramma](https://contentmoderator.cognitive.microsoft.com).

### <a name="enter-a-review-definition"></a>Geef een definitie controleren

Bewerk de **aanvraagtekst** vak in te voeren van de JSON-aanvraag met de volgende velden:

- **Metadata**: Aangepaste sleutel / waarde-paren moeten worden geretourneerd naar de callback-eindpunt. Als de sleutel een korte code die is gedefinieerd is in de [beoordelingsprogramma](https://contentmoderator.cognitive.microsoft.com), wordt dit weergegeven als een label.
- **Inhoud**: Dit is een URL-tekenreeks die verwijst naar de inhoud in het geval van afbeeldings- en Video-inhoud. Dit is de daadwerkelijke tekenreeks voor tekstinhoud.
- **ContentId**: Een aangepaste id-reeks. Deze reeks wordt doorgegeven aan de API en die zijn geretourneerd via de callback. Dit is handig voor het koppelen van interne id's of metagegevens met de resultaten van een taak voor beheer.
- **CallbackEndpoint**: (Optioneel) De URL voor terugbellen informatie ontvangen wanneer de beoordeling is voltooid.

De aanvraagtekst standaard ziet u voorbeelden van de verschillende typen beoordelingen die u kunt maken:

```json
[Image]
[
  {
    "Metadata": [
      {
        "Key": "string",
        "Value": "string"
      }
    ],
    "Type": "Image",
    "Content": "<Content Url>",
    "ContentId": "<Your identifier for this content>",
    "CallbackEndpoint": "<Url where you would receive callbacks>"
  }
]
[Text]
[
  {
    "Metadata": [
      {
        "Key": "string",
        "Value": "string"
      }
    ],
    "Type": "Text",
    "Content": "<Your Text Content>",
    "ContentId": "<Your identifier for this content>",
    "CallbackEndpoint": "<Url where you would receive callbacks>"
  }
]
[Video]
[
  {
    "VideoFrames":[
      {
          "Id": "<Frame Id>",
          "Timestamp": "<Frame Timestamp",
          "FrameImage":"<Frame Image URL",
          "Metadata": [
            {
              "Key": "<Key>",
              "Value": "<Value"
            }
          ],
          "ReviewerResultTags": [
          ]
    ], 
    "Metadata": [
      {
        "Key": "string",
        "Value": "string"
      },
      //For encrypted Videos
        {
          "Key": "protectedType",
          "Value": "AES or FairPlay or Widevine or Playready"
        },
        {
          "Key": "authenticationToken",
          "Value": "your viewtoken(In case of Video Indexer AES encryption type, this value is viewtoken from breakdown json)"
        },
      //For FairPlay encrypted type video include certificateUrl as well
        {
          "Key": "certificateUrl",
          "Value": "your certificate url"
        }
    ],
    "Type": "Video",
    "Content": "<Stream Url>",
    "ContentId": "<Your identifier for this content>",
    "CallbackEndpoint": "<Url where you would receive callbacks>",
    [Optional]
    "Timescale": "<Timescale of the video>
  }
]
```

### <a name="submit-your-request"></a>Uw aanvraag hebt ingediend
  
Selecteer **Verzenden**. Als de bewerking is geslaagd, de **antwoordstatus** is `200 OK`, en de **antwoordinhoud** verschijnt een ID voor de beoordeling. Kopieer deze ID in de volgende stappen gebruiken.

![Controleren - console antwoord inhoud vak geeft de revisie-ID maken](images/test-drive-review-2.PNG)

### <a name="examine-the-new-review"></a>Bekijk de nieuwe revisie

In de [beoordelingsprogramma](https://contentmoderator.cognitive.microsoft.com), selecteer **revisie** > **installatiekopie**/**tekst** / **Video** (afhankelijk van welke inhoud u gebruikt). De inhoud die u hebt geüpload wordt weergegeven, gereed voor menselijke beoordeling.

![Controleren of hulpprogramma installatiekopie van een voetbal](images/test-drive-review-5.PNG)

## <a name="get-review-details"></a>Lees meer informatie

Als u wilt meer informatie over een bestaande revisie ophalen, gaat u naar de [bekijken: ophalen](https://westus2.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c2) API verwijzen naar pagina en selecteer de knop voor uw regio (de regio waarin uw sleutel wordt beheerd).

![Werkstroom - Get-regio selecteren](images/test-drive-region.png)

Voer de REST-aanroep parameters zoals in de bovenstaande sectie. Voor deze stap **reviewId** is de unieke id-tekenreeks die u hebt ontvangen tijdens het maken van de beoordeling.

![Controleren - console Get resultaten maken](images/test-drive-review-3.PNG)
  
Selecteer **Verzenden**. Als de bewerking is geslaagd, de **antwoordstatus** is `200 OK`, en de **antwoordinhoud** in de details van toegangsbeoordeling in JSON-indeling, zoals hieronder weergegeven:

```json
{  
  "reviewId":"201712i46950138c61a4740b118a43cac33f434",
  "subTeam":"public",
  "status":"Complete",
  "reviewerResultTags":[  
    {  
      "key":"a",
      "value":"False"
    },
    {  
      "key":"r",
      "value":"True"
    },
    {  
      "key":"sc",
      "value":"True"
    }
  ],
  "createdBy":"<teamname>",
  "metadata":[  
    {  
      "key":"sc",
      "value":"true"
    }
  ],
  "type":"Image",
  "content":"https://reviewcontentprod.blob.core.windows.net/<teamname>/IMG_201712i46950138c61a4740b118a43cac33f434",
  "contentId":"0",
  "callbackEndpoint":"<callbackUrl>"
}
```

Let op de volgende velden in het antwoord:

- **status**
- **reviewerResultTags**: Dit wordt weergegeven als labels zijn handmatig toegevoegd door het team voor menselijke beoordeling (die wordt weergegeven de **createdBy** veld).
- **metadata**: U ziet de labels die oorspronkelijk zijn toegevoegd aan de beoordeling, voordat de wijzigingen worden aangebracht team menselijke beoordeling.

## <a name="next-steps"></a>Volgende stappen

In deze handleiding, hebt u geleerd over het maken van inhoudstoezicht beoordelingen met behulp van de REST-API. Vervolgens worden geïntegreerd beoordelingen in een scenario voor end-to-end-beheer, zoals de [toezicht op E-commerce](./ecommerce-retail-catalog-moderation.md) zelfstudie.