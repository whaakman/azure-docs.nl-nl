---
title: Uw LUIS-app publiceren | Microsoft Docs
description: Nadat u bouwen en testen van uw app met behulp van Language Understanding (LUIS), het publiceren als een webservice op Azure.
services: cognitive-services
titleSuffix: Azure
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 05/07/2018
ms.author: v-geberr;
ms.openlocfilehash: 1bd24af0498755b7cdcb170624fd8f9f3b39c85a
ms.sourcegitcommit: 756f866be058a8223332d91c86139eb7edea80cc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2018
ms.locfileid: "37347409"
---
# <a name="publish-your-trained-app"></a>Uw getrainde app publiceren
Wanneer u klaar bent met het bouwen en testen van uw LUIS-app, publiceren. Nadat de app is gepubliceerd, wordt de pagina publiceren weergegeven alle bijbehorende HTTP [eindpunten](luis-glossary.md#endpoint). Deze eindpunten per [regio](luis-reference-regions.md) en per [sleutel](luis-how-to-manage-keys.md), klikt u vervolgens zijn geïntegreerd in elke client, chatbot of back-end-toepassing. 

U kunt altijd [testen](interactive-test.md) uw app voordat het wordt gepubliceerd. 

## <a name="production-and-staging-slots"></a>Productie- als stagingsleuven
U kunt uw app te publiceren de **Staging-site** of de **productiesite**. Met behulp van twee publishing sleuven, kunt hiermee u twee verschillende versies met gepubliceerde eindpunten of dezelfde versie hebben op twee verschillende eindpunten. 

<!-- TBD: what is the technical difference? log files, endpoint quota? -->

## <a name="settings-configuration-requires-publishing-model"></a>Instellingen voor configuratie vereist model publiceren
Publiceren naar het eindpunt na wijzigingen in de volgende instellingen. 

## <a name="external-services-settings"></a>Instellingen voor externe services
Externe service-instellingen omvatten **[Sentimentanalyse](#enable-sentiment-analysis)** en  **[spraak gebeuren](#enable-speech-priming)**.

### <a name="enable-sentiment-analysis"></a>Sentimentanalyse inschakelen
In de **instellingen voor externe services**, wordt de **Sentimentanalyse inschakelen** selectievakje kunt LUIS integreren met [Tekstanalyse](https://azure.microsoft.com/services/cognitive-services/text-analytics/) sentiment en sleuteluitdrukkingen opgeven analyse. U hebt geen een Text Analytics-sleutel opgeven en er zijn geen facturering kosten voor deze service kunt u uw Azure-account. Nadat u deze instelling inschakelt, is het permanent. 

Sentimentsgegevens is een score tussen 1 en 0 waarmee wordt aangegeven welke positieve (dichter bij 1) of een negatieve (dichter bij 0) gevoel van de gegevens.

Zie voor meer informatie over de JSON-eindpunt-antwoord met sentimentanalyse, [sentimentanalyse](luis-concept-data-extraction.md#sentiment-analysis)

### <a name="enable-speech-priming"></a>Inschakelen van spraak voorbereiden 
In de **instellingen voor externe services**, wordt de **inschakelen spraak gebeuren** selectievakje kunt u één eindpunt voor het ophalen van een gesproken utterance van chatbot of toepassingen van LUIS-aanroepen en ontvangen van een LUIS voorspelling van het antwoord. Het voorbereiden van de spraakherkenning gebruikmaakt van de Cognitive service [Speech-API](../Speech-Service/rest-apis.md). 

![Afbeelding van dialoogvenster voor bevestiging van spraak voorbereiden](./media/luis-how-to-publish-app/speech-prime-modal.png)

Zodra deze functie is ingeschakeld, moet u uw app publiceren. Wanneer u uw LUIS-app publiceert, wordt het appmodel van uw naar uw eigen Speech-service verzonden naar het ultieme de Speech-service. De gegevens van uw model **niet** gebruikt buiten uw eigen service. 

Als u wilt het gebruik van spraak voorbereiden hebt voltooid, moet u de volgende informatie om te gebruiken de [spraak SDK](../speech-service/speech-sdk-reference.md):
* De sleutel van een LUIS-eindpunt.
* De LUIS-app-ID.
* Een eindpuntdomein, aangeduid als 'Hostname' in de SDK van de spraakherkenning, zoals "westus.api.cognitive.microsoft.com", waarbij het eerste subdomein is voor de regio waar de app wordt gepubliceerd.

Zie voor meer informatie de [spraak naar intentie](http://aka.ms/speechsdk) voorbeeld.

Wanneer uw LUIS-app wordt verwijderd of de Speech-service is verwijderd, worden gegevens van uw model wordt verwijderd. 

## <a name="endpoint-url-settings"></a>Instellingen voor eindpunt-URL
Instellingen voor eindpunt-URL-services omvatten **[tijdzone](#set-timezone-offset)** verschuiving,  **[alle voorspelde scores intentie](#include-all-predicted-intent-scores)**, en  **[ Bing spellingcontrole](#enable-bing-spell-checker)**.

### <a name="set-timezone-offset"></a>Verschuiving van de tijdzone instellen 
Onderdeel van de keuze van de site is de tijdzoneselectie. Met deze instelling timezone kunt LUIS naar [alter](luis-concept-data-alteration.md#change-time-zone-of-prebuilt-datetimev2-entity) elk gewenst moment vooraf gedefinieerde datetimeV2 waarden tijdens de voorspelling, zodat de geretourneerde entiteitsgegevens volgens de geselecteerde tijdzone klopt. 

### <a name="include-all-predicted-intent-scores"></a>Alle voorspelde intentie scores opnemen
De **inclusief alle voorspelde scores intentie** selectievakje kan het eindpunt query-antwoord om op te nemen van de score voorspelling voor elk doel. 

Deze instelling kunt uw chatbot of LUIS-aanroepen van toepassing op een programmatische beslissing nemen op basis van de scores van de geretourneerde intents. De eerste twee intenties zijn in het algemeen de meest interessante. Als de hoogste score is het geen intentie, dat uw chatbot kunt kiezen om een follow-up vraag te stellen die een definitieve keuze maakt tussen de intentie geen en de andere hoge score intentie. 

De intenties en hun scores zijn ook opgenomen van de eindpunt-Logboeken. U kunt [exporteren](create-new-app.md#export-app) deze logboeken en analyseren van de scores. 

```
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
In de **eindpunt-url-instellingen**, wordt de **inschakelen Bing spellingcontrole** selectievakje kunt LUIS te corrigeren van verkeerd gespelde woorden voordat voorspelling. Hiervoor moet u maakt een  **[sleutel Bing spellingcontrole](https://azure.microsoft.com/try/cognitive-services/?api=spellcheck-api)**. Nadat de sleutel is gemaakt, worden twee querystring-parameters worden toegevoegd aan de eindpunt-URL op de pagina publiceren. 

Als u uw eigen URL's zijn maken voor uw toepassing LUIS-aanroepen, controleert u of de **spellingcontrole = true** querystring-parameter en de **bing-spellingcontrole-controle-subscription-key = {YOUR_BING_KEY_HERE}**. Vervang de `{YOUR_BING_KEY_HERE}` met de Bing spell checker sleutel.

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

## <a name="publish-your-trained-app-to-an-http-endpoint"></a>Uw getrainde app publiceren naar een HTTP-eindpunt
Open uw app door te klikken op de naam van de **mijn Apps** pagina en klik vervolgens op **publiceren** in het bovenste deelvenster. 

![Publiceren van pagina-](./media/luis-how-to-publish-app/publish-to-production.png)
 
Wanneer uw app is gepubliceerd, wordt een groen geslaagd-melding weergegeven aan de bovenkant van de browser. 

* Kies of u wilt publiceren naar **productie** of **fasering** door te selecteren in de vervolgkeuzelijst onder **Selecteer sleuf**. 

## <a name="assign-key"></a>Sleutel toewijzen

Als u wilt een sleutel wordt gebruikt dan de gratis Starter_Key wordt weergegeven, klikt u op de **sleutel toevoegen** knop. Deze actie opent u een dialoogvenster waarin u kunt een bestaande eindpuntsleutel om toe te wijzen aan de app selecteren. Zie voor meer informatie over het maken en eindpunt sleutels toevoegen aan uw LUIS-app [beheren van uw sleutels](luis-how-to-manage-keys.md).

Als u wilt zien van eindpunten en sleutels die zijn gekoppeld aan andere regio's, gebruikt u de keuzerondjes om over te schakelen van regio's. Elke rij in de **Resources en sleutels** tabel geeft een lijst van Azure-resources die zijn gekoppeld aan uw account en de eindpunt-sleutels die zijn gekoppeld aan deze resource.

## <a name="endpoint-url-construction"></a>Eindpunt-URL bouwen
De eindpunt-URL komt overeen met de Azure-regio die is gekoppeld aan de eindpuntsleutel.

Deze tabel bevat de eenvoudig uw publicatie configuratie in het URL-eindpunt met route keuzes en querytekenreekswaarden. Als u uw eindpunt-URL's zijn maken voor uw toepassing LUIS-aanroepen, zorg ervoor dat deze dezelfde routes en de queryreeks-waarden zijn ingesteld voor het eindpunt gebruikt--als u deze wilt instellen.

De URL-route is gemaakt met de regio en de app-ID. Als u in andere regio's of met andere apps publiceert, kan de eindpunt-URL worden samengesteld door de regio en app-ID-waarden te wijzigen. 

* Selecteer de slot Production en vervolgens de knop **Publish**. Zodra het publiceren is voltooid, gebruikt u de weergegeven eindpunt-URL voor toegang tot uw LUIS-app. 

### <a name="optional-query-string-parameters"></a>Optionele queryparameters in tekenreeks
De volgende parameters van de query-tekenreeks kunnen worden gebruikt met de eindpunt-URL:

<!-- TBD: what about speech priming? -->

|Querytekenreeks|Type|Voorbeeldwaarde|Doel|
|--|--|--|--|
|uitgebreide|booleaans|true|Opnemen [alle intentie scores](#include-all-predicted-intent-scores) voor utterance|
|timezoneOffset|getal (eenheid is minuten)|60|Stel [tijdzone offset](luis-concept-data-alteration.md#change-time-zone-of-prebuilt-datetimev2-entity) voor [datetimeV2 vooraf gemaakte entiteiten](luis-reference-prebuilt-datetimev2.md)|
|Spellingcontrole|booleaans|true|[spelling corrigeren](#enable-bing-spell-checker) van utterance--gebruikt in combinatie met bing-spellingcontrole-controle-subscription-key queryreeks-parameter|
|Bing-spellingcontrole-controle-subscription-key|abonnements-ID||gebruikt in combinatie met de queryreeks-parameter spellingcontrole|
|Fasering|booleaans|false|Schakel fasering of productie-eindpunt|
|logboek|booleaans|true|toevoegen van de query en de resultaten aan te melden|


## <a name="test-your-published-endpoint-in-a-browser"></a>Uw gepubliceerde eindpunt testen in een browser
Uw gepubliceerde eindpunt testen door te selecteren van de URL in de **eindpunt** kolom. De standaardbrowser wordt geopend met de gegenereerde URL. Instellen van de URL-parameter "& q ' aan de testquery. Bijvoorbeeld append `&q=Book me a flight to Boston on May 4` toe aan uw URL en druk op Enter. De JSON-antwoord van de HTTP-eindpunt wordt weergegeven in de browser. 

![JSON-antwoord van een gepubliceerde HTTP-eindpunt](./media/luis-how-to-publish-app/luis-publish-app-json-response.png)

## <a name="next-steps"></a>Volgende stappen

* Zie [sleutels beheren](./luis-how-to-manage-keys.md) sleutels toevoegen aan uw LUIS-app, en meer informatie over hoe sleutels worden toegewezen aan regio's.
* Zie [trainen en testen van uw app](interactive-test.md) voor instructies over het testen van de gepubliceerde app in de testconsole.
