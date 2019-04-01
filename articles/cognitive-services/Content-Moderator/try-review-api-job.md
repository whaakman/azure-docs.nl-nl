---
title: 'Beheer-taken gebruiken met de REST-API-console: Content Moderator'
titlesuffix: Azure Cognitive Services
description: Bewerkingen van de taak van de beoordeling-API gebruiken om end-to-end inhoudstoezicht taken voor de afbeelding of tekst inhoud in Azure Content Moderator.
services: cognitive-services
author: sanjeev3
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: article
ms.date: 03/18/2019
ms.author: sajagtap
ms.openlocfilehash: 7827cee2af2dfc0c1fddc407c1d146dc9a66c514
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/01/2019
ms.locfileid: "58756097"
---
# <a name="define-and-use-moderation-jobs-rest"></a>Definiëren en gebruik toezicht op taken (REST)

Een taak toezicht fungeert als een soort-wrapper voor de functionaliteit van inhoudstoezicht, werkstromen en recensies. Deze handleiding wordt beschreven hoe u met de REST-API's van de taak starten en controleren van inhoudstoezicht taken. Als u inzicht in de structuur van de API's, kunt u eenvoudig deze aanroepen naar elk platform compatibel is met REST poort.

## <a name="prerequisites"></a>Vereisten

- Meld u aan of maak een account op de Content Moderator [beoordelingsprogramma](https://contentmoderator.cognitive.microsoft.com/) site.
- (Optioneel) [Definiëren van een aangepaste werkstroom](./Review-Tool-User-Guide/Workflows.md) voor gebruik met uw taak; u kunt ook de standaardwerkstroom.

## <a name="create-a-job"></a>Een taak maken

Voor het maken van een taak voor beheer, gaat u naar de [Job - maken](https://westus2.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c5) API verwijzen naar pagina en selecteer de knop voor uw belangrijkste regio (u kunt dit vinden in de eindpunt-URL op de **referenties** pagina van de [controleren hulpprogramma](https://contentmoderator.cognitive.microsoft.com/)). Hiermee start u de API-console, kunt u eenvoudig maken en uitvoeren van de REST API-aanroepen.

![Taak: maken van de selectie van de regio](images/test-drive-job-1.png)

### <a name="enter-rest-call-parameters"></a>REST-aanroep parameters invoeren

Voer de volgende waarden op voor het maken van de REST-aanroep:

- **teamName**: Het team-ID die u hebt gemaakt bij het instellen van uw [beoordelingsprogramma](https://contentmoderator.cognitive.microsoft.com/) account (gevonden in de **Id** veld op het scherm van uw beoordelingsprogramma referenties).
- **ContentType**: Dit kan zijn 'Afbeelding', 'Tekst' of 'Video'.
- **ContentId**: Een aangepaste id-reeks. Deze reeks wordt doorgegeven aan de API en die zijn geretourneerd via de callback. Dit is handig voor het koppelen van interne id's of metagegevens met de resultaten van een taak voor beheer.
- **workflowname**: De naam van de werkstroom die u eerder hebt gemaakt (of 'standaard' voor de standaardwerkstroom).
- **CallbackEndpoint**: (Optioneel) De URL voor terugbellen informatie ontvangen wanneer de beoordeling is voltooid.
- **Ocp-Apim-Subscription-Key**: Uw Content Moderator-sleutel. U kunt dit vinden op de **instellingen** tabblad van de [beoordelingsprogramma](https://contentmoderator.cognitive.microsoft.com).

### <a name="fill-in-the-request-body"></a>Vul in de aanvraagtekst

De hoofdtekst van de REST-aanroep bevat één veld **ContentValue**. Plak de inhoud van de onbewerkte tekst als u toezicht op tekst houden bent of een afbeelding of video-URL invoeren als u toezicht op afbeelding/video houden bent. U kunt het volgende voorbeeld afbeeldings-URL: [https://moderatorsampleimages.blob.core.windows.net/samples/sample2.jpg](https://moderatorsampleimages.blob.core.windows.net/samples/sample2.jpg)

![Taak - console queryparameters, headers en -vak voor hoofdtekst van aanvraag](images/job-api-console-inputs.PNG)

### <a name="submit-your-request"></a>Uw aanvraag hebt ingediend

Selecteer **Verzenden**. Als de bewerking is geslaagd, de **antwoordstatus** is `200 OK`, en de **antwoordinhoud** vak een ID voor de taak wordt weergegeven. Kopieer deze ID in de volgende stappen gebruiken.

![Controleren - console antwoord inhoud vak geeft de revisie-ID maken](images/test-drive-job-3.PNG)

## <a name="get-job-status"></a>Taakstatus

Als u de status en details voor een taak die wordt uitgevoerd of voltooid, gaat u naar de [taak: ophalen](https://westus2.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c3) API verwijzen naar pagina en selecteer de knop voor uw regio (de regio waarin uw sleutel wordt beheerd).

![Taak - Get-regio selecteren](images/test-drive-region.png)

Voer de REST-aanroep parameters zoals in de bovenstaande sectie. Voor deze stap **JobId** is de unieke id-tekenreeks die u hebt ontvangen toen u de taak is gemaakt. Selecteer **Verzenden**. Als de bewerking is geslaagd, de **antwoordstatus** is `200 OK`, en de **antwoordinhoud** in de taak in JSON-indeling, zoals hieronder weergegeven:

```json
{  
  "Id":"2018014caceddebfe9446fab29056fd8d31ffe",
  "TeamName":"some team name",
  "Status":"Complete",
  "WorkflowId":"OCR",
  "Type":"Image",
  "CallBackEndpoint":"",
  "ReviewId":"201801i28fc0f7cbf424447846e509af853ea54",
  "ResultMetaData":[  
    {  
      "Key":"hasText",
      "Value":"True"
    },
    {  
      "Key":"ocrText",
      "Value":"IF WE DID \r\nALL \r\nTHE THINGS \r\nWE ARE \r\nCAPABLE \r\nOF DOING, \r\nWE WOULD \r\nLITERALLY \r\nASTOUND \r\nOURSELVE \r\n"
    }
  ],
  "JobExecutionReport":[  
    {  
      "Ts":"2018-01-07T00:38:29.3238715",
      "Msg":"Posted results to the Callbackendpoint: https://requestb.in/vxke1mvx"
    },
    {  
      "Ts":"2018-01-07T00:38:29.2928416",
      "Msg":"Job marked completed and job content has been removed"
    },
    {  
      "Ts":"2018-01-07T00:38:29.0856472",
      "Msg":"Execution Complete"
    },
    {  
      "Ts":"2018-01-07T00:38:26.7714671",
      "Msg":"Successfully got hasText response from Moderator"
    },
    {  
      "Ts":"2018-01-07T00:38:26.4181346",
      "Msg":"Getting hasText from Moderator"
    },
    {  
      "Ts":"2018-01-07T00:38:25.5122828",
      "Msg":"Starting Execution - Try 1"
    }
  ]
}
```

![Taak: REST-aanroep antwoord ophalen](images/test-drive-job-5.png)

### <a name="examine-the-new-reviews"></a>Bekijk de nieuwe review(s)

Als uw inhoud taak leidde tot het maken van een beoordeling, kunt u bekijken in de [beoordelingsprogramma](https://contentmoderator.cognitive.microsoft.com). Selecteer **revisie** > **installatiekopie**/**tekst**/**Video** (afhankelijk van welke u inhoud wordt gebruikt). De inhoud moet worden weergegeven, gereed voor menselijke beoordeling. Nadat een menselijke moderator de labels automatisch toegewezen en voorspellingsgegevens controleert en een beslissing laatste toezicht verzendt, verzendt de API taken al deze gegevens naar de aangewezen retouraanroep-eindpunt.

## <a name="next-steps"></a>Volgende stappen

In deze handleiding, hebt u geleerd hoe u kunt maken en het opvragen van inhoudstoezicht taken met behulp van de REST-API. Integreer taken vervolgens zoals in een scenario voor end-to-end-beheer, de [toezicht op E-commerce](./ecommerce-retail-catalog-moderation.md) zelfstudie.