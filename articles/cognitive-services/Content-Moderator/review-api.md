---
title: Azure Content beheerder - controle taken en human in de lus beoordelingen | Microsoft Docs
description: Menselijke toezicht van toepassing op de machine-ondersteunde controle voor de beste resultaten.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 1/21/2018
ms.author: sajagtap
ms.openlocfilehash: 35b3cdaa410712c3fd08d3df4ebe4c83e3955d50
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344536"
---
# <a name="moderation-jobs-and-reviews"></a>Controle-taken en beoordelingen

Toezicht machine ondersteund met mogelijkheden voor human in de lus combineren met behulp van de beheerder van Azure-inhoud [revisie API](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c5) ophalen van de beste resultaten voor uw bedrijf.

De controle-API biedt de volgende manieren menselijke toezicht opnemen in uw inhoud toezichtproces:

* `Job` bewerkingen worden gebruikt voor toezicht machine ondersteund en de aanmaak van menselijke revisie start als één stap.
* `Review` bewerkingen worden gebruikt voor het maken van menselijke beoordeling buiten de controle-stap.
* `Workflow` bewerkingen worden gebruikt voor het beheren van werkstromen automatiseren scannen met drempelwaarden voor het maken van de evaluatie.

De `Job` en `Review` operations accepteert uw eindpunten retouraanroep voor het ontvangen van de status en resultaten.

In dit artikel bevat informatie over de `Job` en `Review` bewerkingen. Lees de [werkstromen overzicht](workflow-api.md) voor informatie over het maken, bewerken en werkstroomdefinities ophalen.

## <a name="job-operations"></a>Taakbewerkingen

### <a name="start-a-job"></a>Een taak starten
Gebruik de `Job.Create` opnieuw starten van een controle en de taak voor het maken van menselijke controleren. Inhoud beheerder scant de inhoud en de aangewezen werkstroom evalueert. Op basis van de resultaten van de werkstroom, deze beoordelingen maakt of wordt de stap overgeslagen. De codes na controle en na bekijken voor uw eindpunt retouraanroep worden ook verzonden.

De invoer zijn onder andere de volgende informatie:

- De id van de evaluatie-team.
- De inhoud die moet worden gecontroleerd.
- De naam van de werkstroom. (De standaardwaarde is de werkstroom 'standaard').
- De API-retouraanroep wijst voor meldingen.
 
Het volgende antwoord bevat de id van de taak die is gestart. U kunt de taak-id ophalen van de taakstatus en ontvangt gedetailleerde informatie.

    {
        "JobId": "2018014caceddebfe9446fab29056fd8d31ffe"
    }

### <a name="get-job-status"></a>Taakstatus ophalen

Gebruik de `Job.Get` bewerking en de taak-id ophalen van de details van een taak uitgevoerd of voltooid. De bewerking retourneert onmiddellijk terwijl de controle-taak wordt asynchroon uitgevoerd. De resultaten worden geretourneerd via de callback-eindpunt.

De ingevoerde gegevens omvatten de volgende informatie:

- De ID van het controle-team: de taak-id die is geretourneerd door de vorige bewerking

Het antwoord bevat de volgende informatie:

- De id van de evaluatie gemaakt. (Deze ID gebruiken om de resultaten van de laatste revisie.)
- De status van de taak (voltooide of wordt uitgevoerd): de toegewezen toezicht labels (sleutel / waarde-paren).
- Het rapport van de taak kan worden uitgevoerd.
 
 
        {
            "Id": "2018014caceddebfe9446fab29056fd8d31ffe",
            "TeamName": "some team name",
            "Status": "Complete",
            "WorkflowId": "OCR",
            "Type": "Image",
            "CallBackEndpoint": "",
            "ReviewId": "201801i28fc0f7cbf424447846e509af853ea54",
            "ResultMetaData":[
            {
            "Key": "hasText",
            "Value": "True"
            },
            {
            "Key": "ocrText",
            "Value": "IF WE DID \r\nALL \r\nTHE THINGS \r\nWE ARE \r\nCAPABLE \r\nOF DOING, \r\nWE WOULD \r\nLITERALLY \r\nASTOUND \r\nOURSELVE \r\n"
            }
            ],
            "JobExecutionReport": [
            {
                "Ts": "2018-01-07T00:38:29.3238715",
                "Msg": "Posted results to the Callbackendpoint: https://requestb.in/vxke1mvx"
                },
                {
                "Ts": "2018-01-07T00:38:29.2928416",
                "Msg": "Job marked completed and job content has been removed"
                },
                {
                "Ts": "2018-01-07T00:38:29.0856472",
                "Msg": "Execution Complete"
                },
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
 
![Installatiekopie van een menselijke moderators revisie](images/ocr-sample-image.PNG)

## <a name="review-operations"></a>Bewerkingen controleren

### <a name="create-reviews"></a>Beoordelingen maken

Gebruik de `Review.Create` tijdens het maken van de menselijke onderzoeken. U ze ergens anders vertragen of aangepaste regels gebruiken de controle-labels toe te wijzen.

De ingevoerde gegevens voor deze bewerking zijn onder andere:

- De inhoud die moet worden gecontroleerd.
- De toegewezen tags (sleutelwaarde-paren) voor de controle door menselijke moderators.

Het volgende antwoord ziet u de revisie-id:

    [
        "201712i46950138c61a4740b118a43cac33f434",
    ]


### <a name="get-review-status"></a>Bekijk status ophalen
Gebruik de `Review.Get` bewerking om de resultaten nadat een menselijke beoordeling van de gecontroleerde installatiekopie is voltooid. U ontvangt een bericht via de opgegeven retouraanroep-eindpunt. 

De bewerking retourneert twee sets met tags: 

* De labels die is toegewezen door de controle-service
* De labels nadat de menselijke revisie is voltooid

De ingevoerde gegevens zijn ten minste:

- De naam van de evaluatie-team
- De revisie-id die is geretourneerd door de vorige bewerking

Het antwoord bevat de volgende informatie:

- De status controleren
- De labels (sleutel / waarde-paren) bevestigd door het menselijke revisor
- De labels (sleutel / waarde-paren) toegewezen door de controle-service

U ziet dat zowel de revisor toegewezen labels (**reviewerResultTags**) en de initiële labels (**metagegevens**) in het volgende voorbeeldantwoord:

    {
        "reviewId": "201712i46950138c61a4740b118a43cac33f434",
        "subTeam": "public",
        "status": "Complete",
        "reviewerResultTags": [
        {
            "key": "a",
            "value": "False"
        },
        {
            "key": "r",
            "value": "True"
        },
        {
            "key": "sc",
            "value": "True"
        }
        ],
        "createdBy": "{teamname}",
        "metadata": [
        {
            "key": "sc",
            "value": "true"
        }
        ],
        "type": "Image",
        "content": "https://reviewcontentprod.blob.core.windows.net/{teamname}/IMG_201712i46950138c61a4740b118a43cac33f434",
        "contentId": "0",
        "callbackEndpoint": "{callbackUrl}"
    }

## <a name="next-steps"></a>Volgende stappen

* Uitprobeert de [taak API console](try-review-api-job.md), en de REST-API-codevoorbeelden gebruiken. Als u bekend met Visual Studio en C# bent, Lees ook de [taken .NET Quick Start](moderation-jobs-quickstart-dotnet.md). 
* Voor revisies aan de slag met de [revisie API console](try-review-api-review.md), en de REST-API-codevoorbeelden gebruiken. Vervolgens ziet de [beoordelingen .NET Quick Start](moderation-reviews-quickstart-dotnet.md).
* Voor een video-beoordelingen en gebruiken de [Video revisie Quick Start](video-reviews-quickstart-dotnet.md), en meer informatie over hoe [transcripties toevoegen aan de video revisie](video-transcript-reviews-quickstart-dotnet.md).
