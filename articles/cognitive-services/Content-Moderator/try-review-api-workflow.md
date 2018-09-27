---
title: Werkstromen voor inhoudstoezicht vanuit de API-console - Content Moderator
titlesuffix: Azure Cognitive Services
description: Informatie over het gebruik van werkstromen voor inhoudstoezicht vanuit de API-console.
services: cognitive-services
author: sanjeev3
manager: cgronlun
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: conceptual
ms.date: 02/05/2018
ms.author: sajagtap
ms.openlocfilehash: 4ef8951b30fa7aede08a1af3c834192b5ed18649
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/26/2018
ms.locfileid: "47223847"
---
# <a name="workflows-from-the-api-console"></a>Werkstromen uit de API-console

Gebruik de [werkstroombewerkingen](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/5813b46b3f9b0711b43c4c59) in Azure Content Moderator maken of bijwerken van een werkstroom of Werkstroomdetails van ophalen met behulp van de API controleren. U kunt eenvoudige, complexe en zelfs geneste expressies definiëren voor uw werkstromen met behulp van deze API. De werkstromen worden weergegeven in het beoordelingsprogramma voor uw team te gebruiken. De werkstromen worden ook gebruikt door de bewerkingen van de taak van de beoordeling-API.

## <a name="prerequisites"></a>Vereisten

1. Ga naar de [beoordelingsprogramma](https://contentmoderator.cognitive.microsoft.com/). Meld u als u dat nog niet hebt gedaan. 
2. In het controlehulpprogramma onder **instellingen**, selecteer de **werkstromen** tabblad, zoals wordt weergegeven in het beoordelingsprogramma [werkstroom zelfstudie](Review-Tool-User-Guide/Workflows.md).

### <a name="browse-to-the-workflows-screen"></a>Blader naar het scherm werkstromen

Selecteer op het dashboard Content Moderator **revisie** > **instellingen** > **werkstromen**. U ziet een standaardwerkstroom.

  ![Standaardwerkstroom](images/default-workflow-listed.PNG)

### <a name="get-the-json-definition-of-the-default-workflow"></a>De JSON-definitie van de standaardwerkstroom ophalen

Selecteer de **bewerken** optie voor uw werkstroom, en selecteer vervolgens de **JSON** tabblad. Ziet u de volgende JSON-expressie:

    {
        "Type": "Logic",
        "If": {
            "ConnectorName": "moderator",
            "OutputName": "isAdult",
            "Operator": "eq",
            "Value": "true",
            "Type": "Condition"
            },
        "Then": {
        "Perform": [
        {
            "Name": "createreview",
            "CallbackEndpoint": null,
            "Tags": []
        }
        ],
        "Type": "Actions"
        }
    }

## <a name="get-workflow-details"></a>Werkstroomdetails van ophalen

Gebruik de **Workflow - Get** bewerking voor meer informatie over uw bestaande standaardwerkstroom.

In het controlehulpprogramma, gaat u naar de [referenties](Review-Tool-User-Guide/credentials.md#the-review-tool) sectie.

### <a name="browse-to-the-api-reference"></a>Blader naar de API-verwijzing

1. In de **referenties** weergave, selecteer [API-verwijzing](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/5813b46b3f9b0711b43c4c59). 
2. Wanneer de **Workflow - maken of bijwerken** pagina wordt geopend, gaat u naar de [Workflow - Get](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/5813b44b3f9b0711b43c4c58) verwijzing.

### <a name="select-your-region"></a>Selecteer de regio

Voor **Open API testconsole**, selecteer de regio die het beste past bij uw locatie.

  ![Werkstroom - Get-regio selecteren](images/test-drive-region.png)

  De **Workflow - Get** API-console wordt geopend.

### <a name="enter-parameters"></a>Parameters invoeren

Voer waarden in voor **team**, **workflowname**, en **Ocp-Apim-Subscription-Key** (uw abonnementssleutel):

- **team**: de team-ID die u hebt gemaakt bij het instellen van uw [hulpprogramma account controleren](https://contentmoderator.cognitive.microsoft.com/). 
- **workflowname**: de naam van uw werkstroom. Gebruik `default`.
- **OCP-Apim-Subscription-Key**: zich op de **instellingen** tabblad. Zie voor meer informatie, [overzicht](overview.md).

  ![Query-parameters en headers ophalen](images/workflow-get-default.PNG)

### <a name="submit-your-request"></a>Uw aanvraag hebt ingediend
  
Selecteer **Verzenden**. Als de bewerking is geslaagd, de **antwoordstatus** is `200 OK`, en de **antwoordinhoud** in de volgende JSON-werkstroom weergegeven:

    {
        "Name": "default",
        "Description": "Default",
        "Type": "Image",
        "Expression": {
        "If": {
            "ConnectorName": "moderator",
            "OutputName": "isadult",
            "Operator": "eq",
            "Value": "true",
            "AlternateInput": null,
            "Type": "Condition"
            },
        "Then": {
            "Perform": [{
                "Name": "createreview",
                "Subteam": null,
                "CallbackEndpoint": null,
                "Tags": []
            }],
            "Type": "Actions"
            },
            "Else": null,
            "Type": "Logic"
            }
    }


## <a name="create-a-workflow"></a>Een werkstroom maken

In het controlehulpprogramma, gaat u naar de [referenties](Review-Tool-User-Guide/credentials.md#the-review-tool) sectie.

### <a name="browse-to-the-api-reference"></a>Blader naar de API-verwijzing

In de **referenties** weergave, selecteer [API-verwijzing](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/5813b46b3f9b0711b43c4c59). De **Workflow - maken of bijwerken** pagina wordt geopend.

### <a name="select-your-region"></a>Selecteer de regio

Voor **Open API testconsole**, selecteer de regio die het beste past bij uw locatie.

  ![Werkstroom - of Update de selectie van de regio maken](images/test-drive-region.png)

  De **Workflow - maken of bijwerken** API-console wordt geopend.

### <a name="enter-parameters"></a>Parameters invoeren

Voer waarden in voor **team**, **workflowname**, en **Ocp-Apim-Subscription-Key** (uw abonnementssleutel):

- **team**: de team-ID die u hebt gemaakt bij het instellen van uw [hulpprogramma account controleren](https://contentmoderator.cognitive.microsoft.com/). 
- **workflowname**: de naam van de nieuwe werkstroom.
- **OCP-Apim-Subscription-Key**: zich op de **instellingen** tabblad. Zie voor meer informatie, [overzicht](overview.md).

  ![Werkstroom - of Update-console query-parameters en headers maken](images/workflow-console-parameters.PNG)

### <a name="enter-the-workflow-definition"></a>Voer de werkstroomdefinitie

1. Bewerk de **aanvraagtekst** vak in te voeren van de JSON-aanvraag met details voor **beschrijving** en **Type** (afbeelding of tekst). 
2. Voor **expressie**, kopieert u de standaard werkstroom-expressie uit de voorgaande sectie, zoals hier wordt weergegeven:

        {
            "Description": "Default workflow from API console",
            "Type": "Image",
            "Expression": 
                // Copy the default workflow expression from the preceding section
        }

    De aanvraagtekst lijkt op de volgende JSON-aanvraag:

        {
            "Description": "Default workflow from API console",
            "Type": "Image",
            "Expression": {
                "Type": "Logic",
                "If": {
                    "ConnectorName": "moderator",
                    "OutputName": "isAdult",
                    "Operator": "eq",
                    "Value": "true",
                    "Type": "Condition"
                    },
                "Then": {
                "Perform": [
                {
                    "Name": "createreview",
                    "CallbackEndpoint": null,
                    "Tags": [ ]
                }
                ],
                "Type": "Actions"
                }
            }
        }
 
### <a name="submit-your-request"></a>Uw aanvraag hebt ingediend
  
Selecteer **Verzenden**. Als de bewerking is geslaagd, de **antwoordstatus** is `200 OK`, en de **antwoordinhoud** bevat `true`.

### <a name="check-out-the-new-workflow"></a>Bekijk de nieuwe werkstroom

Selecteer in het beoordelingsprogramma **revisie** > **instellingen** > **werkstromen**. De nieuwe werkstroom wordt weergegeven en is klaar voor gebruik.

  ![Beoordeling hulpprogramma lijst met werkstromen](images/workflow-console-new-workflow.PNG)
  
### <a name="review-your-new-workflow-details"></a>Bekijk de Werkstroomdetails van uw nieuwe

1. Selecteer de **bewerken** optie voor uw werkstroom, en selecteer vervolgens de **Designer** en **JSON** tabbladen.

   ![Tabblad ontwerper voor een geselecteerde werkstroom](images/workflow-console-new-workflow-designer.PNG)

2. De JSON-weergave van de werkstroom, selecteer de **JSON** tabblad.

## <a name="next-steps"></a>Volgende stappen

* Zie voor voorbeelden van meer complexe workflow de [overzicht van werkstromen](workflow-api.md).
* Informatie over het gebruik van werkstromen met [content moderation taken](try-review-api-job.md).
