---
title: Inhoud toezicht taken uitvoeren in Azure inhoud beheerder | Microsoft Docs
description: Informatie over het toezicht inhoud taken uitvoeren in de API-console.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 08/03/2017
ms.author: sajagtap
ms.openlocfilehash: 6f741be1001ae70d5fdbf6f374204aaad1601abe
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344477"
---
# <a name="start-a-moderation-job-from-the-api-console"></a>Een taak toezicht starten vanuit de API-console

De controle-API gebruiken [operations taak](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c5) end-to-end inhoud toezicht taken van de inhoud van afbeelding of tekst in Azure inhoud beheerder initiëren. 

De taak toezicht scant uw inhoud met behulp van de inhoud beheerder installatiekopie toezicht API of de tekst toezicht API. Vervolgens gebruikt de controle-taak werkstromen (gedefinieerd in het hulpprogramma voor beoordeling) voor het genereren van beoordelingen in het hulpprogramma voor beoordeling. 

Nadat een menselijke beheerder de labels automatisch toegewezen en de gegevens van de voorspelling bekijkt en een beslissing laatste controle verstuurt, verzendt de API Controleer alle gegevens naar uw API-eindpunt.

## <a name="prerequisites"></a>Vereisten

Navigeer naar de [hulpprogramma controleren](https://contentmoderator.cognitive.microsoft.com/). Meld u als u dit nog niet hebt gedaan. In het hulpprogramma revisie [definiëren van een aangepaste werkstroom](Review-Tool-User-Guide/Workflows.md) moet worden gebruikt in deze `Job` bewerking.

## <a name="use-the-api-console"></a>De API-console gebruiken
Als u wilt de API test-drive met behulp van de online-console, moet u enkele waarden invoeren in de console:
    
- `teamName`: Gebruik de `Id` veld van het scherm van uw beoordeling-hulpprogramma referenties. 
- `ContentId`: Deze reeks is doorgegeven aan de API en geretourneerd via de retouraanroep. **ContentId** is handig voor het koppelen van interne id's of metagegevens met de resultaten van een taak toezicht.- `Workflowname`: de naam van de [werkstroom die u hebt gemaakt](Review-Tool-User-Guide/Workflows.md) in de vorige sectie.
- `Ocp-Apim-Subscription-Key`: De gevonden op de **instellingen** tabblad. Zie voor meer informatie [overzicht](overview.md).

Toegang tot de API-console is van de **referenties** venster.

### <a name="navigate-to-the-api-reference"></a>Navigeer naar de API-verwijzing
In de **referenties** Selecteer [API-referentiemateriaal](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c5).

  De `Job.Create` pagina wordt geopend.

### <a name="select-your-region"></a>Selecteer de regio
Voor **Open API testen console**, selecteer de regio die het beste past bij uw locatie.
  ![Taak - pagina Regioselectie maken](images/test-drive-job-1.png)

  De `Job.Create` API-console wordt geopend. 

### <a name="enter-parameters"></a>Parameters invoeren

Geef waarden op voor de vereiste queryparameters en de abonnementssleutel van uw. In de **aanvraagtekst** Geef de locatie van de informatie die u wilt scannen. In dit voorbeeld gaan we gebruiken deze [voorbeeldafbeelding](https://moderatorsampleimages.blob.core.windows.net/samples/sample6.png).

  ![Taak - console queryparameters, kopteksten en aanvraag hoofdtekst vak maken](images/job-api-console-inputs.PNG)

### <a name="submit-your-request"></a>Uw aanvraag verzenden
Selecteer **Verzenden**. Een taak-ID is gemaakt. Kopieer dit moet worden gebruikt in de volgende stappen.

  `"JobId": "2018014caceddebfe9446fab29056fd8d31ffe"`

### <a name="open-the-get-job-details-page"></a>Open de pagina Get Job details
Selecteer **ophalen**, en open vervolgens de API door de knop die overeenkomt met uw regio te selecteren.

  ![Functie - console maken Get-resultaten](images/test-drive-job-4.png)

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

## <a name="navigate-to-the-review-tool"></a>Navigeer naar het hulpprogramma controleren
Selecteer op het Dashboard inhoud beheerder **revisie** > **installatiekopie**. De gescande afbeelding wordt weergegeven, gereed voor menselijke revisie.

  ![Afbeelding van de knop van drie fietsers controleren](images/ocr-sample-image.PNG)

## <a name="next-steps"></a>Volgende stappen

De REST API gebruiken in uw code of starten met de [taken .NET Quick Start](moderation-jobs-quickstart-dotnet.md) integreren met uw toepassing.
