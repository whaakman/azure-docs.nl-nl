---
title: Abonnementssleutels
titleSuffix: Language Understanding - Azure Cognitive Services
description: U hoeft niet te maken van abonnementssleutels voor het gebruik van uw gratis eindpunt van de eerste 1000 query's. Als u ontvangt een _buiten het quotum_ fout opgetreden in de vorm van een HTTP 403 of 429, moet u een sleutel maken en toewijzen aan uw app.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 01/29/2019
ms.author: diberry
ms.openlocfilehash: 26954c8f839ff0bfb2da484e4fb535f33d4e07ed
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/30/2019
ms.locfileid: "55239176"
---
# <a name="using-subscription-keys-with-your-luis-app"></a>Abonnementssleutels gebruiken met uw LUIS-app

U hoeft niet te maken van abonnementssleutels voor het gebruik van uw gratis eindpunt van de eerste 1000 query's. Wanneer de eindpunt-query's worden gebruikt, maakt u een Azure-resource in de [Azure-portal](http://portal.azure.com), die resource vervolgens toewijzen aan een LUIS-app in de [LUIS portal](https://www.luis.ai).

Als u ontvangt een _buiten het quotum_ fout opgetreden in de vorm van een HTTP 403 of 429, moet u een sleutel maken en toewijzen aan uw app. 

Gebruik voor testen en prototype alleen de gratis laag van (F0). Gebruik voor productiesystemen, een [betaalde](https://aka.ms/luis-price-tier) laag. Gebruik niet de [ontwerpen sleutel](luis-concept-keys.md#authoring-key) voor eindpunt query's in de productieomgeving.

<a name="create-luis-service"></a>

## <a name="create-language-understanding-endpoint-key-in-the-azure-portal"></a>Language Understanding-eindpuntsleutel maken in Azure portal

Deze procedure maakt u een **Language Understanding** resource. Als u een informatiebron die kan worden gebruikt voor Cognitive Services wilt, de alles-in-één-sleutel maken **[Cognitive Service](../cognitive-services-apis-create-account.md)** in plaats van de Language Understanding-resource. 

Deze sleutel moet alleen worden gebruikt voor eindpunt voorspelling query's. Gebruik deze sleutel niet voor wijzigingen in het model of de app. 

1. Aanmelden bij de  **[Azure-portal](https://ms.portal.azure.com/)**. 
1. Selecteer het groene **+** zich in het bovenste linkerdeelvenster en zoek naar de `Language Understanding` in de marketplace, schakelt u vervolgens op **Language Understanding** en volg de  **aan die conferenties** een LUIS-abonnement-account maken. 

    ![Azure Search](./media/luis-azure-subscription/azure-search.png) 

1. Het abonnement configureren met instellingen die de accountnaam, zoals prijzen van lagen, enzovoort. 

    ![Azure API kiezen](./media/luis-azure-subscription/azure-api-choice.png) 

1. Als u de Language Understanding-resource maakt, kunt u de toegangssleutel die is gegenereerd in bekijken **Resourcemanagement -> sleutels**. De volgende sectie wordt beschreven hoe deze nieuwe resources verbinden met een LUIS-app in de LUIS-portal. U moet de naam van de resource LUIS in stap 3.

    ![Azure-sleutels](./media/luis-azure-subscription/azure-keys.png)

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

1. Aanmelden bij de portal LUIS, kiest u een app om toe te voegen van de nieuwe sleutel en selecteer de **beheren** in het menu rechtsboven en selecteer **sleutels en eindpunten**.

    [ ![De pagina sleutels en eindpunten](./media/luis-manage-keys/keys-and-endpoints.png) ](./media/luis-manage-keys/keys-and-endpoints.png#lightbox)

1. Als u wilt toevoegen de LUIS, selecteert u **Resource toewijzen +**.

    ![Een resource toewijzen aan uw app](./media/luis-manage-keys/assign-key.png)

1. Selecteer een Tenant in het dialoogvenster dat is gekoppeld aan het e-mailbericht adres uw gebruikt om aan te melden met de LUIS-website.  

1. Kies de **abonnementsnaam** die zijn gekoppeld aan de Azure-resource toe te voegen.

1. Selecteer de **LUIS resourcenaam**. 

1. Selecteer **resource toewijzen**. 

1. De nieuwe rij in de tabel zoeken en kopieer de eindpunt-URL. Het is goed samengesteld om te maken van een HTTP GET-aanvraag naar de LUIS-eindpunt voor een voorspelling. 

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

Voeg de **spellingcontrole = true** querystring-parameter en de **bing-spellingcontrole-controle-subscription-key = {YOUR_BING_KEY_HERE}**. Vervang de `{YOUR_BING_KEY_HERE}` met de Bing spell checker sleutel.

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

## <a name="how-to-fix-out-of-quota-errors-when-the-key-exceeds-pricing-tier-usage"></a>Voor het oplossen van fouten voor out-van-quotum wanneer de sleutel is groter dan prijzen laag gebruik
Elke laag kunt eindpunt aanvragen naar uw LUIS-account met een specifieke snelheid. Als het aantal aanvragen hoger dan het toegestane aantal uw naar gebruik-account per minuut of per maand is, aanvragen ontvangen van een HTTP-fout "429: Te veel aanvragen."

Elke laag kan wél cumulatief aanvragen per maand. Als het totaal aantal aanvragen dat hoger is dan de toegestane frequentie, aanvragen ontvangen van een HTTP-fout ' 403: verboden '.  

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
