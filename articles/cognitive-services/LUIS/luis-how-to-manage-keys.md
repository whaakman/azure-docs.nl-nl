---
title: Sleutels beheren
titleSuffix: Language Understanding - Azure Cognitive Services
description: Nadat u een LUIS-eindpuntsleutel in Azure portal maakt, wordt de sleutel toewijzen aan de LUIS-app en het juiste eindpunt-URL ophalen. Gebruik dit eindpunt-URL om LUIS voorspellingen.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: 0a278ded7ce290347645f345e4eee0b15972787f
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2018
ms.locfileid: "53088099"
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
<a name="assign-resource"></a>


## <a name="assign-resource-in-luis-portal"></a>Resource in de Portal LUIS toewijzen

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

### <a name="unassign-resource"></a>Resource verwijderen
Wanneer u de toewijzing van de eindpuntsleutel, is het niet verwijderd uit Azure. Dit is alleen van LUIS ontkoppeld. 

Wanneer de eindpuntsleutel van een is niet-toegewezen of niet is toegewezen aan de app, een aanvraag naar het eindpunt van de URL wordt een fout geretourneerd: `401 This application cannot be accessed with the current subscription`. 

### <a name="include-all-predicted-intent-scores"></a>Alle voorspelde intentie scores opnemen
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

### <a name="enable-bing-spell-checker"></a>Inschakelen van de spellingcontrole van Bing 
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


### <a name="publishing-regions"></a>Publicatie-regio 's

Meer informatie over publiceren [regio's](luis-reference-regions.md) inclusief publicatie in [Europa](luis-reference-regions.md#publishing-to-europe), en [Australia](luis-reference-regions.md#publishing-to-australia). Publicatie-regio's wijken af van het ontwerpen van regio's. Een app maken in de ontwerphandleiding regio overeenkomt met de publicatie regio die u wilt gebruiken voor het eindpunt van de query.

## <a name="assign-resource-without-luis-portal"></a>Resource zonder LUIS portal toewijzen

Voor automation-doeleinden, zoals een CI/CD-pijplijn, kunt u de toewijzing van een LUIS-resource om een LUIS-app te automatiseren. In de volgorde die moet u de volgende stappen uitvoeren:

1. Een Azure Resource Manager-token ophalen uit deze [website](https://resources.azure.com/api/token?plaintext=true). Dit token verloopt dus direct gebruiken. De aanvraag retourneert een Azure Resource Manager-token.

    ![Azure Resource Manager-token aanvragen en ontvangen van Azure Resource Manager-token](./media/luis-manage-keys/get-arm-token.png)

1. Het token gebruiken om aan te vragen de LUIS-resources tussen abonnementen, vanuit de [ophalen LUIS azure API-accounts](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5be313cec181ae720aa2b26c), uw gebruikersaccount heeft toegang tot. 

    Deze POST-API moet de volgende instellingen:

    |Header|Waarde|
    |--|--|
    |`Authorization`|De waarde van `Authorization` is `Bearer {token}`. U ziet dat de waarde voor de token moet worden voorafgegaan door het woord `Bearer` en een spatie.| 
    |`Ocp-Apim-Subscription-Key`|Uw [ontwerpen sleutel](luis-how-to-account-settings.md).|

    Deze API retourneert een matrix met JSON-objecten van uw abonnements-ID, resourcegroep en resourcenaam, geretourneerd als de accountnaam waaronder LUIS-abonnementen. Het ene item niet vinden in de matrix die is de resource LUIS om toe te wijzen aan de LUIS-app. 

1. Het token toewijzen aan de LUIS-resource met de [een LUIS-azure-accounts toewijzen aan een toepassing](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5be32228e8473de116325515) API. 

    Deze POST-API moet de volgende instellingen:

    |Type|Instelling|Waarde|
    |--|--|--|
    |Header|`Authorization`|De waarde van `Authorization` is `Bearer {token}`. U ziet dat de waarde voor de token moet worden voorafgegaan door het woord `Bearer` en een spatie.|
    |Header|`Ocp-Apim-Subscription-Key`|Uw [ontwerpen sleutel](luis-how-to-account-settings.md).|
    |Header|`Content-type`|`application/json`|
    |QueryString|`appid`|De id van de LUIS-app. 
    |Hoofdtekst||{"AzureSubscriptionId": "ddda2925-af7f-4b05-9ba1-2155c5fe8a8e"<br>'ResourceGroup': 'resourcegroup-2'<br>"AccountName": "luis-uswest-S0-2"}|

    Als deze API geslaagd is, wordt de status van een 201 - gemaakt. 

## <a name="next-steps"></a>Volgende stappen

Gebruikt u uw sleutel voor het publiceren van uw app in de **app publiceren** pagina. Zie voor instructies over het publiceren, [app publiceren](luis-how-to-publish-app.md).

Zie [sleutels in LUIS](luis-concept-keys.md) LUIS ontwerpen en het eindpunt belangrijkste concepten begrijpen.
