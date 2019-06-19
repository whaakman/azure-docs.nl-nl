---
title: Abonnementssleutels
titleSuffix: Language Understanding - Azure Cognitive Services
description: U hoeft niet te maken van abonnementssleutels voor het gebruik van uw gratis eindpunt van de eerste 1000 query's. Als u ontvangt een _buiten het quotum_ fout opgetreden in de vorm van een HTTP 403 of 429, moet u een sleutel maken en toewijzen aan uw app.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 06/18/2019
ms.author: diberry
ms.openlocfilehash: 7f82bf5a40df0554d4f98b2d835fcbd69279be43
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67204162"
---
# <a name="using-subscription-keys-with-your-luis-app"></a>Abonnementssleutels gebruiken met uw LUIS-app

Wanneer u Language Understanding (LUIS) voor het eerst gebruikt, hoeft u niet te abonnementssleutels maken. U begint met, 1000 endpoint-query's wordt verstrekt. 

Gebruik voor testen en prototype alleen de gratis laag van (F0). Gebruik voor productiesystemen, een [betaalde](https://aka.ms/luis-price-tier) laag. Gebruik niet de [ontwerpen sleutel](luis-concept-keys.md#authoring-key) voor eindpunt query's in de productieomgeving.


<a name="create-luis-service"></a>
<a name="create-language-understanding-endpoint-key-in-the-azure-portal"/>

## <a name="create-prediction-endpoint-runtime-resource-in-the-azure-portal"></a>Voorspelling endpoint runtime-resource maken in Azure portal

U maakt de [voorspelling eindpuntresource](get-started-portal-deploy-app.md#create-the-endpoint-resource) in Azure portal. Deze resource moet alleen worden gebruikt voor eindpunt voorspelling query's. Gebruik deze resource niet voor schrijven wijzigingen in de app.

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


## <a name="assign-resource-key-to-luis-app-in-luis-portal"></a>Resource toewijzen aan een LUIS-app in de Portal LUIS

Telkens wanneer u een nieuwe resource van LUIS maakt, moet u [de resource toewijzen aan de LUIS-app](get-started-portal-deploy-app.md#assign-the-resource-key-to-the-luis-app-in-the-luis-portal). Nadat deze toegewezen, hoeft u deze stap opnieuw uitvoert, tenzij u een nieuwe resource maken. U kunt een nieuwe resource om uit te breiden, de regio's van uw app of voor de ondersteuning van een hoger aantal voorspelling query's maken.

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
In de **eindpunt-url-instellingen**, wordt de **Bing spellingcontrole** schakelaar kunt u LUIS te corrigeren van verkeerd gespelde woorden voordat voorspelling. Maak een  **[sleutel Bing spellingcontrole](https://azure.microsoft.com/try/cognitive-services/?api=spellcheck-api)** . 

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

Voor automation-doeleinden, zoals een CI/CD-pijplijn, kunt u de toewijzing van een LUIS-resource om een LUIS-app te automatiseren. Als u wilt doen, moet u de volgende stappen uitvoeren:

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

## <a name="change-pricing-tier"></a>Prijscategorie wijzigen

1.  In [Azure](https://portal.azure.com), zoek uw LUIS-abonnement. Selecteer het abonnement LUIS.
    ![Zoek uw LUIS-abonnement](./media/luis-usage-tiers/find.png)
1.  Selecteer **prijscategorie** om te zien van de beschikbare Prijscategorieën. 
    ![Prijscategorieën weergeven](./media/luis-usage-tiers/subscription.png)
1.  Selecteer de prijscategorie laag en selecteert u **Selecteer** uw wijziging op te slaan. 
    ![De betaling LUIS laag wijzigen](./media/luis-usage-tiers/plans.png)
1.  Wanneer met de prijswijziging voltooid is, controleert de nieuwe prijscategorie of in een pop-upvenster. 
    ![Controleer of uw LUIS betaling-laag](./media/luis-usage-tiers/updated.png)
1. Houd er rekening mee te [toewijzen van deze eindpuntsleutel](#assign-endpoint-key) op de **publiceren** pagina en deze gebruiken in alle endpoint-query's. 

## <a name="fix-http-status-code-403-and-429"></a>HTTP-statuscode 403 en 429 oplossen

U krijgt fout 403 en 429 statuscodes wanneer u de transacties per seconde of transacties per maand voor uw prijscategorie overschrijdt.

### <a name="when-you-receive-an-http-403-error-status-code"></a>Wanneer u de statuscode van een HTTP 403-fout ontvangt

Wanneer u alle deze gratis 1000 eindpunt query's gebruiken of u uw prijscategorie maandelijkse transacties quota overschrijdt, ontvangt u de statuscode van een HTTP 403-fout. 

U kunt deze fout oplossen, moet u een [uw prijscategorie wijzigen](luis-how-to-azure-subscription.md#change-pricing-tier) naar een hogere laag of [Maak een nieuwe resource](get-started-portal-deploy-app.md#create-the-endpoint-resource) en [toewijzen aan uw app](get-started-portal-deploy-app.md#assign-the-resource-key-to-the-luis-app-in-the-luis-portal).

Oplossingen voor deze fout zijn onder andere:

* In de [Azure-portal](https://portal.azure.com), op uw Taalbegrip resource, op de **Resource Management-prijscategorie >** , uw prijscategorie wijzigen naar een hogere TPS-laag. U hoeft te doen in de portal van Language Understanding als uw resource al aan uw app Language Understanding toegewezen is.
*  Als uw gebruik groter is dan de hoogste prijscategorie, moet u meer Language Understanding-resources met een load balancer vóór ze toevoegen. De [Language Understanding container](luis-container-howto.md) met Kubernetes of Docker Compose met dit kan helpen.

### <a name="when-you-receive-an-http-429-error-status-code"></a>Wanneer u een HTTP 429 foutstatuscode ontvangt

Deze statuscode is geretourneerd wanneer de transacties per seconde is groter dan uw prijscategorie.  

Oplossingen omvatten:

* U kunt [verhogen van uw prijscategorie](#change-pricing-tier), als u niet op het hoogste niveau.
* Als uw gebruik groter is dan de hoogste prijscategorie, moet u meer Language Understanding-resources met een load balancer vóór ze toevoegen. De [Language Understanding container](luis-container-howto.md) met Kubernetes of Docker Compose met dit kan helpen.
* U kunt uw toepassingsaanvragen van clients met gate een [beleid voor opnieuw proberen](https://docs.microsoft.com/azure/architecture/best-practices/transient-faults#general-guidelines) u zelf implementeren wanneer u deze statuscode. 

## <a name="viewing-summary-usage"></a>Overzicht gebruik weergeven
U kunt informatie over het gebruik van LUIS weergeven in Azure. De **overzicht** pagina toont recente samenvattende informatie, inclusief aanroepen en fouten. Als u een LUIS-aanvraag voor het claimeindpunt aanbrengt, klikt u vervolgens onmiddellijk volgen het **overzichtspagina**, leiden tot vijf minuten voor het gebruik worden weergegeven.

![Overzicht gebruik weergeven](./media/luis-usage-tiers/overview.png)

## <a name="customizing-usage-charts"></a>Gebruik grafieken aanpassen
Metrische gegevens biedt meer inzicht in de gegevens.

![Standaard metrische gegevens](./media/luis-usage-tiers/metrics-default.png)

U kunt uw grafieken met metrische gegevens voor een bepaalde periode en type van metrische gegevens configureren. 

![Aangepaste metrische gegevens](./media/luis-usage-tiers/metrics-custom.png)

## <a name="total-transactions-threshold-alert"></a>Totaal aantal transacties drempelwaarde voor waarschuwing
Als u wilt weten wanneer u een bepaalde drempelwaarde in de transactie, bijvoorbeeld 10.000 transacties hebt bereikt, kunt u een waarschuwing maken. 

![Standaard-waarschuwingen](./media/luis-usage-tiers/alert-default.png)

Toevoegen van een waarschuwing voor metrische gegevens voor de **totaal aantal aanroepen** metrische gegevens voor een bepaalde periode. Scheid e-mailadressen van alle personen die de waarschuwing moet ontvangen. Voeg webhooks voor alle systemen die de waarschuwing moeten ontvangen. U kunt ook een logische app uitvoeren wanneer de waarschuwing wordt geactiveerd. 

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het gebruik van [versies](luis-how-to-manage-versions.md) voor het beheren van wijzigingen in uw LUIS-app.
