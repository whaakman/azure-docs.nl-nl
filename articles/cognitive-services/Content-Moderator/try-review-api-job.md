---
title: 'Inhoudstoezicht taken uitvoeren met de API-console: Content Moderator'
titlesuffix: Azure Cognitive Services
description: Leer hoe u inhoudstoezicht taken uitvoert in de API-console.
services: cognitive-services
author: sanjeev3
manager: cgronlun
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: conceptual
ms.date: 08/03/2017
ms.author: sajagtap
ms.openlocfilehash: 240b26cd86a6985825e3145c5bc43ef31524d7b7
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/26/2018
ms.locfileid: "47227098"
---
# <a name="start-a-moderation-job-from-the-api-console"></a>Een taak toezicht starten vanuit de API-console

De beoordeling-API gebruiken [operations taak](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c5) inhoudstoezicht end-to-end-taken voor de afbeelding of tekst inhoud in Azure Content Moderator initiëren. 

De beheer-taak scant uw inhoud met behulp van de inhoud Moderator installatiekopie toezicht-API of de tekst toezicht-API. Vervolgens gebruikt de taak voor beheer van werkstromen (gedefinieerd in het beoordelingsprogramma) voor het genereren van beoordelingen in het controlehulpprogramma. 

Nadat een menselijke moderator de labels automatisch toegewezen en voorspellingsgegevens controleert en een beslissing laatste toezicht verzendt, verzendt de API controleren alle gegevens naar uw API-eindpunt.

## <a name="prerequisites"></a>Vereisten

Navigeer naar de [controlehulpprogramma](https://contentmoderator.cognitive.microsoft.com/). Meld u als u dit nog niet hebt gedaan. Binnen het beoordelingsprogramma [definiëren van een aangepaste werkstroom](Review-Tool-User-Guide/Workflows.md) te gebruiken in deze `Job` bewerking.

## <a name="use-the-api-console"></a>De API-console gebruiken
Als u wilt Maak een proefrit met de API met behulp van de online-console, moet u enkele waarden invoeren in de console:
    
- `teamName`: Gebruik de `Id` veld van het beoordelingsprogramma referenties scherm. 
- `ContentId`: Deze reeks wordt doorgegeven aan de API en die zijn geretourneerd via de callback. **ContentId** is handig voor het koppelen van interne id's of metagegevens met de resultaten van een taak in het beheer.- `Workflowname`: de naam van de [werkstroom die u hebt gemaakt](Review-Tool-User-Guide/Workflows.md) in de vorige sectie.
- `Ocp-Apim-Subscription-Key`: De zich op de **instellingen** tabblad. Zie voor meer informatie, [overzicht](overview.md).

Toegang tot de API-console is van de **referenties** venster.

### <a name="navigate-to-the-api-reference"></a>Navigeer naar de API-verwijzing
In de **referenties** venster [API-verwijzing](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c5).

  De `Job.Create` pagina wordt geopend.

### <a name="select-your-region"></a>Selecteer de regio
Voor **Open API testconsole**, selecteer de regio die het beste past bij uw locatie.
  ![Taak: maken van de selectie van de regio](images/test-drive-job-1.png)

  De `Job.Create` API-console wordt geopend. 

### <a name="enter-parameters"></a>Parameters invoeren

Geef waarden voor de vereiste parameters en de abonnementssleutel van uw. In de **aanvraagtekst** , geeft u de locatie van de informatie die u wilt scannen. In dit voorbeeld gebruiken we dit [voorbeeldafbeelding](https://moderatorsampleimages.blob.core.windows.net/samples/sample6.png).

  ![Taak - console queryparameters, headers en -vak voor hoofdtekst van aanvraag](images/job-api-console-inputs.PNG)

### <a name="submit-your-request"></a>Uw aanvraag hebt ingediend
Selecteer **Verzenden**. Een taak-ID wordt gemaakt. Kopieer deze in de volgende stappen gebruiken.

  `"JobId": "2018014caceddebfe9446fab29056fd8d31ffe"`

### <a name="open-the-get-job-details-page"></a>Open de pagina met details van de taak ophalen
Selecteer **ophalen**, en open vervolgens de API door de knop die overeenkomt met uw regio te selecteren.

  ![Taak - console maakt Get-resultaten](images/test-drive-job-4.png)

### <a name="review-the-response"></a>Bekijk de reactie

Voer waarden in voor **teamName** en **JobID**. Voer de abonnementssleutel van uw en selecteer vervolgens **verzenden**. Het volgende antwoord bevat details en de status van de voorbeeld-taak.

```
    {
        "Id": "2018014caceddebfe9446fab29056fd8d31ffe",
        "TeamName": "some team name",
        "Status": "InProgress",
        "WorkflowId": "OCR",
        "Type": "Image",
        "CallBackEndpoint": "",
        "ReviewId": "",
        "ResultMetaData": [],
        "JobExecutionReport": 
        [
            {
            "Ts": "2018-01-07T00:38:26.7714671",
                "Msg": "Successfully got hasText response from Moderator"
            },
            {
                "Ts": "2018-01-07T00:38:26.4181346",
                "Msg": "Getting hasText from Moderator"
            },
            {
                "Ts": "2018-01-07T00:38:25.5122828",
                "Msg": "Starting Execution - Try 1"
            }
        ]
    }
```

## <a name="navigate-to-the-review-tool"></a>Navigeer naar het beoordelingsprogramma
Selecteer op het Dashboard met Content Moderator, **revisie** > **installatiekopie**. De installatiekopie die u hebt gescand wordt weergegeven, klaar te maken voor menselijke beoordeling.

  ![Controleren of de installatiekopie hulpprogramma van drie fietsers](images/ocr-sample-image.PNG)

## <a name="next-steps"></a>Volgende stappen

De REST-API in uw code te gebruiken of beginnen met de [Snelstartgids voor .NET-taken](moderation-jobs-quickstart-dotnet.md) om te integreren in uw toepassing.
