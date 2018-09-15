---
title: De ontwerp- en eindpunt sleutels in LUIS beheren
titleSuffix: Azure Cognitive Services
description: Nadat u een LUIS-eindpuntsleutel in Azure portal maakt, wordt de sleutel toewijzen aan de LUIS-app en het juiste eindpunt-URL ophalen. Gebruik dit eindpunt-URL om LUIS voorspellingen.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: 6d3f487fd64744fa390291d7e23d95cd9632cd23
ms.sourcegitcommit: ab9514485569ce511f2a93260ef71c56d7633343
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/15/2018
ms.locfileid: "45634936"
---
# <a name="add-an-azure-luis-resource-to-app"></a>Een Azure LUIS-resource toevoegen aan app

Nadat u een LUIS-resource in Azure portal maken, de resource toewijzen aan de LUIS-app en het juiste eindpunt-URL ophalen. Gebruik dit eindpunt-URL om LUIS voorspellingen.

<a name="programmatic-key" ></a>
<a name="authoring-key" ></a>
<a name="endpoint-key" ></a>
<a name="use-endpoint-key-in-query" ></a>
<a name="api-usage-of-ocp-apim-subscription-key" ></a>
<a name="key-limits" ></a>
<a name="key-limit-errors" ></a>
<a name="key-concepts"></a>
<a name="authoring-key"></a>
<a name="create-and-use-an-endpoint-key"></a>
<a name="assign-endpoint-key"></a>

## <a name="assign-resource"></a>Resource toewijzen

1. Maak een LUIS-sleutel op de [Azure-portal](https://portal.azure.com). Zie voor verdere instructies [maken van de eindpuntsleutel van een met behulp van Azure](luis-how-to-azure-subscription.md).
 
2. Selecteer **beheren** in het menu rechtsboven, schakelt u vervolgens **sleutels en eindpunten**.

    [ ![De pagina sleutels en eindpunten](./media/luis-manage-keys/keys-and-endpoints.png) ](./media/luis-manage-keys/keys-and-endpoints.png#lightbox)

3. Als u wilt toevoegen de LUIS, selecteert u **Resource toewijzen +**.

    ![Een resource toewijzen aan uw app](./media/luis-manage-keys/assign-key.png)

4. Selecteer een Tenant in het dialoogvenster dat is gekoppeld aan het e-mailadres dat u zich met naar de website van LUIS aanmelden.  

5. Kies de **abonnementsnaam** die zijn gekoppeld aan de Azure-resource toe te voegen.

6. Selecteer de **LUIS resourcenaam**. 

7. Selecteer **resource toewijzen**. 

8. De nieuwe rij in de tabel zoeken en kopieer de eindpunt-URL. Het is goed samengesteld om te maken van een HTTP GET-aanvraag naar de LUIS-eindpunt voor een voorspelling. 

<!-- content moved to luis-reference-regions.md, need replacement links-->
<a name="regions-and-keys"></a>
<a name="publishing-to-europe"></a>
<a name="publishing-to-australia"></a>

## <a name="unassign-resource"></a>Resource verwijderen
Wanneer u de toewijzing van de eindpuntsleutel, is het niet verwijderd uit Azure. Dit is alleen van LUIS ontkoppeld. 

Wanneer de eindpuntsleutel van een is niet-toegewezen of niet is toegewezen aan de app, een aanvraag naar het eindpunt van de URL wordt een fout geretourneerd: `401 This application cannot be accessed with the current subscription`. 

## <a name="include-all-predicted-intent-scores"></a>Alle voorspelde intentie scores opnemen
De **inclusief alle voorspelde scores intentie** selectievakje kan het eindpunt query-antwoord om op te nemen van de score voorspelling voor elk doel. 

Deze instelling kunt uw chatbot of LUIS-aanroepen van toepassing op een programmatische beslissing nemen op basis van de scores van de geretourneerde intents. De eerste twee intenties zijn in het algemeen de meest interessante. Als de hoogste score is het geen intentie, dat uw chatbot kunt kiezen om een follow-up vraag te stellen die een definitieve keuze maakt tussen de intentie geen en de andere hoge score intentie. 

De intenties en hun scores zijn ook opgenomen van de eindpunt-Logboeken. U kunt [exporteren](luis-how-to-start-new-app.md#export-app) deze logboeken en analyseren van de scores. 

```JSON
{
  "query": "book a flight to Cairo",
  "topScoringIntent": {
    "intent": "None",
    "score": 0.5223427
  },
  "intents": [
    {
      "intent": "None",
      "score": 0.5223427
    },
    {
      "intent": "BookFlight",
      "score": 0.372391433
    }
  ],
  "entities": []
}
```

## <a name="enable-bing-spell-checker"></a>Inschakelen van de spellingcontrole van Bing 
In de **eindpunt-url-instellingen**, wordt de **Bing spellingcontrole** schakelaar kunt u LUIS te corrigeren van verkeerd gespelde woorden voordat voorspelling. Maak een  **[sleutel Bing spellingcontrole](https://azure.microsoft.com/try/cognitive-services/?api=spellcheck-api)**. 

Voeg de **spellingcontrole = true** querystring-parameter en de **bing-spellingcontrole-controle-subscription-key = {YOUR_BING_KEY_HERE}** . Vervang de `{YOUR_BING_KEY_HERE}` met de Bing spell checker sleutel.

```JSON
{
  "query": "Book a flite to London?",
  "alteredQuery": "Book a flight to London?",
  "topScoringIntent": {
    "intent": "BookFlight",
    "score": 0.780123
  },
  "entities": []
}
```


## <a name="publishing-regions"></a>Publicatie-regio 's

Meer informatie over publiceren [regio's](luis-reference-regions.md) inclusief publicatie in [Europa](luis-reference-regions.md#publishing-to-europe), en [Australia](luis-reference-regions.md#publishing-to-australia). Publicatie-regio's wijken af van het ontwerpen van regio's. Een app maken in de ontwerphandleiding regio overeenkomt met de publicatie regio die u wilt gebruiken voor het eindpunt van de query.

## <a name="next-steps"></a>Volgende stappen

Gebruikt u uw sleutel voor het publiceren van uw app in de **app publiceren** pagina. Zie voor instructies over het publiceren, [app publiceren](luis-how-to-publish-app.md).

Zie [sleutels in LUIS](luis-concept-keys.md) LUIS ontwerpen en het eindpunt belangrijkste concepten begrijpen.