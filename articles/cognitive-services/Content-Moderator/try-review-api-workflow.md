---
title: Azure Content beheerder - werkstromen voor controle van de API-console inhoud | Microsoft Docs
description: Informatie over het gebruiken van inhoud toezicht werkstromen via de API-console.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 02/05/2018
ms.author: sajagtap
ms.openlocfilehash: 700b2bea5e902141659266a94d61ceb810c1b802
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344538"
---
# <a name="workflows-from-the-api-console"></a>Werkstromen via de API-console

Gebruik de [werkstroombewerkingen](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/5813b46b3f9b0711b43c4c59) in Azure inhoud beheerder maken of bijwerken van een werkstroom of Werkstroomdetails van de ophalen met behulp van de evaluatie-API. U kunt eenvoudig, complexe en zelfs geneste expressies definiëren voor uw werkstromen met behulp van deze API. De werkstromen worden weergegeven in het hulpprogramma voor beoordeling voor uw team te gebruiken. De werkstromen worden ook gebruikt door bewerkingen van de taak van de evaluatie-API.

## <a name="prerequisites"></a>Vereisten

1. Ga naar de [revisie hulpprogramma](https://contentmoderator.cognitive.microsoft.com/). Meld u als u dit nog niet hebt gedaan. 
2. In het hulpprogramma voor beoordeling onder **instellingen**, selecteer de **werkstromen** tabblad, zoals wordt weergegeven in het controle-hulpprogramma [werkstroom zelfstudie](Review-Tool-User-Guide/Workflows.md).

### <a name="browse-to-the-workflows-screen"></a>Blader naar het scherm werkstromen

Selecteer op het dashboard inhoud beheerder **revisie** > **instellingen** > **werkstromen**. Er is een standaardwerkstroom.

  ![Standaardwerkstroom](images/default-workflow-listed.PNG)

### <a name="get-the-json-definition-of-the-default-workflow"></a>De JSON-definitie van de standaardwerkstroom ophalen

Selecteer de **bewerken** optie voor de werkstroom en selecteer vervolgens de **JSON** tabblad. Ziet u de volgende JSON-expressie:

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

## <a name="get-workflow-details"></a>Werkstroomdetails van de ophalen

Gebruik de **Workflow - Get** bewerking details van uw bestaande standaardwerkstroom ophalen.

In het hulpprogramma voor beoordeling gaat u naar de [referenties](Review-Tool-User-Guide/credentials.md#the-review-tool) sectie.

### <a name="browse-to-the-api-reference"></a>Blader naar de API-verwijzing

1. In de **referenties** weergave, selecteer [API-referentiemateriaal](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/5813b46b3f9b0711b43c4c59). 
2. Wanneer de **Workflow - maken of bijwerken** pagina wordt geopend, gaat u naar de [Workflow - Get](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/5813b44b3f9b0711b43c4c58) verwijzing.

### <a name="select-your-region"></a>Selecteer de regio

Voor **Open API testen console**, selecteer de regio die het beste past bij uw locatie.

  ![Workflow - Get Regioselectie](images/test-drive-region.png)

  De **Workflow - Get** API-console wordt geopend.

### <a name="enter-parameters"></a>Parameters invoeren

Voer waarden in voor **team**, **workflowname**, en **Ocp-Apim-Subscription-Key** (uw abonnementssleutel):

- **team**: de team-ID die u hebt gemaakt bij het instellen van uw [hulpprogramma account controleren](https://contentmoderator.cognitive.microsoft.com/). 
- **workflowname**: de naam van uw werkstroom. Gebruik `default`.
- **OCP-Apim-Subscription-Key**: zich op de **instellingen** tabblad. Zie voor meer informatie [overzicht](overview.md).

  ![Query-parameters en -koppen ophalen](images/workflow-get-default.PNG)

### <a name="submit-your-request"></a>Uw aanvraag verzenden
  
Selecteer **Verzenden**. Als de bewerking is geslaagd, de **antwoordstatus** is `200 OK`, en de **antwoordinhoud** verschijnt de volgende JSON-werkstroom:

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

In het hulpprogramma voor beoordeling gaat u naar de [referenties](Review-Tool-User-Guide/credentials.md#the-review-tool) sectie.

### <a name="browse-to-the-api-reference"></a>Blader naar de API-verwijzing

In de **referenties** weergave, selecteer [API-referentiemateriaal](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/5813b46b3f9b0711b43c4c59). De **Workflow - maken of bijwerken** pagina wordt geopend.

### <a name="select-your-region"></a>Selecteer de regio

Voor **Open API testen console**, selecteer de regio die het beste past bij uw locatie.

  ![Workflow - maken of Update pagina regio selecteren](images/test-drive-region.png)

  De **Workflow - maken of bijwerken** API-console wordt geopend.

### <a name="enter-parameters"></a>Parameters invoeren

Voer waarden in voor **team**, **workflowname**, en **Ocp-Apim-Subscription-Key** (uw abonnementssleutel):

- **team**: de team-ID die u hebt gemaakt bij het instellen van uw [hulpprogramma account controleren](https://contentmoderator.cognitive.microsoft.com/). 
- **workflowname**: de naam van de nieuwe werkstroom.
- **OCP-Apim-Subscription-Key**: zich op de **instellingen** tabblad. Zie voor meer informatie [overzicht](overview.md).

  ![Workflow - queryparameters of Update-console en -koppen maken](images/workflow-console-parameters.PNG)

### <a name="enter-the-workflow-definition"></a>Voer de werkstroomdefinitie

1. Bewerk de **aanvraagtekst** vak in te voeren van de JSON-aanvraag met details voor **beschrijving** en **Type** (afbeelding of tekst). 
2. Voor **expressie**, Kopieer de standaardexpressie voor de werkstroom van de voorgaande sectie als volgt te werk:

        {
            "Description": "Default workflow from API console",
            "Type": "Image",
            "Expression": 
                // Copy the default workflow expression from the preceding section
        }

    De aanvraagtekst ziet er de volgende JSON-aanvraag:

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
 
### <a name="submit-your-request"></a>Uw aanvraag verzenden
  
Selecteer **Verzenden**. Als de bewerking is geslaagd, de **antwoordstatus** is `200 OK`, en de **antwoordinhoud** bevat `true`.

### <a name="check-out-the-new-workflow"></a>Bekijk de nieuwe werkstroom

Selecteer in het hulpprogramma voor beoordeling **revisie** > **instellingen** > **werkstromen**. De nieuwe werkstroom wordt weergegeven en is klaar voor gebruik.

  ![Bekijk hulpprogramma lijst met werkstromen](images/workflow-console-new-workflow.PNG)
  
### <a name="review-your-new-workflow-details"></a>Bekijk de Werkstroomdetails van uw nieuwe

1. Selecteer de **bewerken** optie voor de werkstroom en selecteer vervolgens de **Designer** en **JSON** tabbladen.

   ![Tabblad ontwerper voor een geselecteerde werkstroom](images/workflow-console-new-workflow-designer.PNG)

2. Overzicht van de JSON-weergave van de werkstroom, selecteer de **JSON** tabblad.

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer complexe workflow voorbeelden de [werkstromen overzicht](workflow-api.md).
* Informatie over het gebruik van werkstromen met [inhoud toezicht taken](try-review-api-job.md).
