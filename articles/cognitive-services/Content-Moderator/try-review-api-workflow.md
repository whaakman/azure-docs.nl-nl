---
title: 'Beheer van werkstromen met de REST-API-console: Content Moderator definiëren'
titlesuffix: Azure Cognitive Services
description: U kunt de Azure-inhoud Moderator revisie API's gebruiken om aangepaste werkstromen en drempelwaarden op basis van uw inhoud beleidsregels te definiëren.
services: cognitive-services
author: sanjeev3
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: article
ms.date: 03/14/2019
ms.author: sajagtap
ms.openlocfilehash: e150b1321f2fbd348e737222c752203281503643
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/01/2019
ms.locfileid: "58756588"
---
# <a name="define-and-use-moderation-workflows-rest"></a>Definiëren en gebruiken van beheer van werkstromen (REST)

Werkstromen zijn cloud-gebaseerde aangepaste filters die u gebruiken kunt voor het afhandelen van inhoud efficiënter. Werkstromen kunnen verbinding maken met tal van services voor het filteren van inhoud op verschillende manieren en vervolgens de juiste actie ondernemen. Deze handleiding wordt beschreven hoe u werkstromen maakt en gebruikt met de werkstroom REST-API's via de API-console. Als u inzicht in de structuur van de API's, kunt u eenvoudig deze aanroepen naar elk platform compatibel is met REST poort.

## <a name="prerequisites"></a>Vereisten

- Meld u aan of maak een account op de Content Moderator [beoordelingsprogramma](https://contentmoderator.cognitive.microsoft.com/) site.

## <a name="create-a-workflow"></a>Een werkstroom maken

Als u wilt maken of bijwerken van een werkstroom, gaat u naar de **[Workflow - maken of bijwerken](https://westus2.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/5813b46b3f9b0711b43c4c59)** API verwijzen naar pagina en selecteer de knop voor uw belangrijkste regio (u kunt dit vinden in de eindpunt-URL op de **referenties**  pagina van de [beoordelingsprogramma](https://contentmoderator.cognitive.microsoft.com/)). Hiermee start u de API-console, kunt u eenvoudig maken en uitvoeren van de REST API-aanroepen.

![Werkstroom - of Update de selectie van de regio maken](images/test-drive-region.png)

### <a name="enter-rest-call-parameters"></a>REST-aanroep parameters invoeren

Voer waarden in voor **team**, **workflowname**, en **Ocp-Apim-Subscription-Key**:

- **team**: Het team-ID die u hebt gemaakt bij het instellen van uw [beoordelingsprogramma](https://contentmoderator.cognitive.microsoft.com/) account (gevonden in de **Id** veld op het scherm van uw beoordelingsprogramma referenties).
- **workflowname**: De naam van een nieuwe werkstroom om toe te voegen (of een bestaande naam, als u wilt bijwerken van een bestaande werkstroom).
- **Ocp-Apim-Subscription-Key**: Uw Content Moderator-sleutel. U kunt dit vinden op de **instellingen** tabblad van de [beoordelingsprogramma](https://contentmoderator.cognitive.microsoft.com).

![Werkstroom - of Update-console query-parameters en headers maken](images/workflow-console-parameters.PNG)

### <a name="enter-a-workflow-definition"></a>Geef een definitie van de werkstroom

1. Bewerken de **aanvraagtekst** vak in te voeren van de JSON-aanvraag met details voor **beschrijving** en **Type** (ofwel `Image` of `Text`).
2. Voor **expressie**, kopieert u de standaardwerkstroom JSON-expressie. Uw laatste JSON-tekenreeks moet er als volgt:

```json
{
  "Description":"<A description for the Workflow>",
  "Type":"Text",
  "Expression":{
    "Type":"Logic",
    "If":{
      "ConnectorName":"moderator",
      "OutputName":"isAdult",
      "Operator":"eq",
      "Value":"true",
      "Type":"Condition"
    },
    "Then":{
      "Perform":[
        {
          "Name":"createreview",
          "CallbackEndpoint":null,
          "Tags":[

          ]
        }
      ],
      "Type":"Actions"
    }
  }
}
```

> [!NOTE]
> U kunt eenvoudige, complexe en zelfs geneste expressies definiëren voor uw werkstromen met behulp van deze API. De [Workflow - maken of bijwerken](https://westus2.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/5813b46b3f9b0711b43c4c59) documentatie bevat voorbeelden van complexere logica.

### <a name="submit-your-request"></a>Uw aanvraag hebt ingediend
  
Selecteer **Verzenden**. Als de bewerking is geslaagd, de **antwoordstatus** is `200 OK`, en de **antwoordinhoud** bevat `true`.

### <a name="examine-the-new-workflow"></a>Bekijk de nieuwe werkstroom

In de [beoordelingsprogramma](https://contentmoderator.cognitive.microsoft.com/), selecteer **instellingen** > **werkstromen**. De nieuwe werkstroom moet worden weergegeven in de lijst.

![Beoordeling hulpprogramma lijst met werkstromen](images/workflow-console-new-workflow.PNG)

Selecteer de **bewerken** optie voor uw werkstroom en Ga naar de **Designer** tabblad. Hier ziet u een intuïtieve weergave van de JSON-logica.

![Tabblad ontwerper voor een geselecteerde werkstroom](images/workflow-console-new-workflow-designer.PNG)

## <a name="get-workflow-details"></a>Werkstroomdetails van ophalen

Als u wilt meer informatie over een bestaande werkstroom ophalen, gaat u naar de **[Workflow - Get](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/5813b44b3f9b0711b43c4c58)** API verwijzen naar pagina en selecteer de knop voor uw regio (de regio waarin uw sleutel wordt beheerd).

![Werkstroom - Get-regio selecteren](images/test-drive-region.png)

Voer de REST-aanroep parameters zoals in de bovenstaande sectie. Zorg ervoor dat deze keer **workflowname** is de naam van een bestaande werkstroom.

![Query-parameters en headers ophalen](images/workflow-get-default.PNG)

Selecteer **Verzenden**. Als de bewerking is geslaagd, de **antwoordstatus** is `200 OK`, en de **antwoordinhoud** in de werkstroom in JSON-indeling, zoals hieronder weergegeven:

```json
{
  "Name":"default",
  "Description":"Default",
  "Type":"Image",
  "Expression":{
    "If":{
      "ConnectorName":"moderator",
      "OutputName":"isadult",
      "Operator":"eq",
      "Value":"true",
      "AlternateInput":null,
      "Type":"Condition"
    },
    "Then":{
      "Perform":[
        {
          "Name":"createreview",
          "Subteam":null,
          "CallbackEndpoint":null,
          "Tags":[

          ]
        }
      ],
      "Type":"Actions"
    },
    "Else":null,
    "Type":"Logic"
  }
}
```

## <a name="next-steps"></a>Volgende stappen

- Informatie over het gebruik van werkstromen met [content moderation taken](try-review-api-job.md).