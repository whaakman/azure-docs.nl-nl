---
title: Uw app LUIS publiceren | Microsoft Docs
description: Nadat u bouwen en testen van uw app met behulp van de taal Understanding (LUIS), het publiceren als een webservice in Azure.
services: cognitive-services
titleSuffix: Azure
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 05/07/2018
ms.author: v-geberr;
ms.openlocfilehash: 12a63e65a739be08d436f8f1b53df566255b1fb1
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/22/2018
ms.locfileid: "36322056"
---
# <a name="publish-your-trained-app"></a>Uw getraind app publiceren
Wanneer u klaar bent met het maken en testen van uw app LUIS publiceren. Nadat de app is gepubliceerd, ziet u de pagina publiceren alle bijbehorende HTTP [eindpunten](luis-glossary.md#endpoint). Deze eindpunten per [regio](luis-reference-regions.md) en per [sleutel](Manage-Keys.md), vervolgens zijn geïntegreerd in elke client, chatbot of back-end-toepassing. 

U kunt altijd [testen](interactive-test.md) uw app voordat het wordt gepubliceerd. 

## <a name="production-and-staging-slots"></a>Productie- en staging-sleuven
U kunt uw app publiceren de **Staging-sleuf** of de **productiesite**. Met behulp van twee publishing sleuven kunt hiermee u twee verschillende versies met de gepubliceerde eindpunten of dezelfde versie hebben op twee verschillende eindpunten. 

<!-- TBD: what is the technical difference? log files, endpoint quota? -->

## <a name="settings-configuration-requires-publishing-model"></a>Instellingen voor de configuratie moet publishing model
Publiceren naar het eindpunt na wijzigingen in de volgende instellingen. 

## <a name="external-services-settings"></a>Instellingen voor externe services
Externe service-instellingen omvatten **[gevoel Analysis](#enable-sentiment-analysis)** en  **[spraak Priming](#enable-speech-priming)**.

### <a name="enable-sentiment-analysis"></a>Gevoel analysis inschakelen
In de **instellingen voor externe services**, wordt de **inschakelen gevoel Analysis** selectievakje kunt LUIS integreren met [Tekstanalyse](https://azure.microsoft.com/services/cognitive-services/text-analytics/) gevoel en sleutel woordgroep opgeven analyse. U hoeft niet te bieden van een sleutel Text Analytics en er zijn geen facturering kosten voor deze service bij uw Azure-account. Zodra u deze instelling inschakelt, is het permanent. 

Gevoel gegevens is een score tussen 1 en 0 positieve waarmee wordt aangegeven (dichter bij 1) of een negatieve (dichter op 0) gevoel van de gegevens.

Zie voor meer informatie over het antwoord van de JSON-eindpunt met gevoel analysis [gevoel analyse](luis-concept-data-extraction.md#sentiment-analysis)

### <a name="enable-speech-priming"></a>Inschakelen van spraak voorbereiden 
In de **instellingen voor externe services**, wordt de **inschakelen spraak Priming** selectievakje beschikt u over één eindpunt een gesproken utterance ophalen uit een chatbot of LUIS aanroepen toepassing en een LUIS ontvangen reactie van de voorspelling. Het voorbereiden van de spraak gebruikmaakt van de service cognitieve [Speech-API](../Speech-Service/rest-apis.md). 

![Afbeelding van dialoogvenster voor bevestiging van spraak voorbereiden](./media/luis-how-to-publish-app/speech-prime-modal.png)

Zodra deze functie is ingeschakeld, moet u uw app publiceren. Wanneer u uw app LUIS publiceert, wordt het model van uw app naar uw eigen spraak-service verzonden naar de service spraak bewerken. De gegevens van uw model **niet** buiten uw eigen service gebruikt. 

Als u wilt het gebruik van spraak voorbereiden voltooien, moet u de volgende informatie om te gebruiken de [spraak SDK](../speech-service/speech-sdk-reference.md):
* Een sleutel van het abonnement LUIS.
* De LUIS app-ID.
* Een domein eindpunt aangeduid als 'Hostnaam' in de SDK-spraak, zoals 'westus.api.cognitive.microsoft.com', waarbij het eerste subdomein is voor de regio waar de app is gepubliceerd.

Zie voor meer informatie de [spraak bedoeling](http://aka.ms/speechsdk) voorbeeld.

Wanneer uw app LUIS wordt verwijderd of de service spraak wordt verwijderd, wordt de gegevens van uw model verwijderd. 

## <a name="endpoint-url-settings"></a>Instellingen voor eindpunt-URL
Instellingen voor eindpunt-URL services omvatten **[tijdzone](#set-timezone-offset)** verschuiving,  **[alle voorspelde scores opzet](#include-all-predicted-intent-scores)**, en  **[ Spellingcontrole van Bing](#enable-bing-spell-checker)**.

### <a name="set-timezone-offset"></a>Stelt de verschuiving van de tijdzone 
Deel van de keuze sleuf is de tijdzoneselectie. Deze instelling tijdzone kunt LUIS naar [alter](luis-concept-data-alteration.md#change-time-zone-of-prebuilt-datetimev2-entity) elk gewenst moment vooraf gedefinieerde datetimeV2 waarden tijdens de voorspelling zodat de geretourneerde entiteitsgegevens volgens de geselecteerde tijdzone klopt. 

### <a name="include-all-predicted-intent-scores"></a>Alle voorspelde opzet scores opnemen
De **opnemen alle voorspelde scores opzet** selectievakje kan het eindpunt query-antwoord opnemen van de score voorspelling voor elk doel. 

Deze instelling kunt uw chatbot of toepassing een programmatische beslissing neemt op basis van de scores van de geretourneerde intents LUIS aanroepen. De eerste twee intents zijn doorgaans de meest interessante. Als de bovenste score geen opzet, dat uw chatbot stelt een vervolgzelfstudie vraag waarmee een definitieve keuze tussen de opzet geen en de andere scoren van hoge bedoeling kunt kiezen. 

De intents en hun scores zijn ook opgenomen van de endpoint-Logboeken. U kunt [exporteren](create-new-app.md#export-app) deze logboeken en de scores analyseren. 

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

### <a name="enable-bing-spell-checker"></a>Inschakelen van spellingcontrole van Bing 
In de **eindpunt-url instellingen**, wordt de **spellingcontrole van Bing inschakelen** selectievakje kunt LUIS verkeerd gespelde woorden voordat de voorspelling te corrigeren. Hiervoor moet u maakt een  **[spellingcontrole van Bing sleutel](https://azure.microsoft.com/try/cognitive-services/?api=spellcheck-api)**. Zodra de sleutel is gemaakt, worden twee querystring parameters worden toegevoegd aan de eindpunt-URL op de pagina publiceren. 

Als u uw eigen URL's zijn maken voor uw toepassing LUIS aanroepen, controleert u of de **spellingcontrole = true** querystring parameter en de **bing-spellen-controle-subscription-key = {YOUR_BING_KEY_HERE}**. Vervang de `{YOUR_BING_KEY_HERE}` met uw Bing spellen checker sleutel.

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

## <a name="publish-your-trained-app-to-an-http-endpoint"></a>Uw getraind app publiceren naar een HTTP-eindpunt
Uw app openen door te klikken op de naam ervan op de **mijn Apps** pagina en klik vervolgens op **publiceren** in het bovenste deelvenster. 

![Publiceren van pagina-](./media/luis-how-to-publish-app/publish-to-production.png)
 
Wanneer uw app is gepubliceerd, wordt een melding groen succes aan de bovenkant van de browser weergegeven. 

* Kies of u wilt publiceren naar **productie** of **fasering** door te selecteren in de vervolgkeuzelijst onder **Selecteer sleuf**. 

## <a name="assign-key"></a>Sleutel toewijzen

Als u wilt een sleutel wordt gebruikt dan de vrije Starter_Key weergegeven, klikt u op de **sleutel toevoegen** knop. Deze actie wordt een dialoogvenster waarmee u een bestaande eindpuntsleutel toewijzen aan de app selecteren geopend. Zie voor meer informatie over het maken en eindpunt sleutels toevoegen aan uw app LUIS [uw sleutels beheren](Manage-Keys.md).

Als u wilt zien eindpunten en sleutels die zijn gekoppeld aan een andere regio's, gebruik de keuzerondjes overschakelen regio's. Elke rij in de **Resources en sleutels** tabel ziet u Azure-resources die zijn gekoppeld aan uw account en de endpoint-sleutels die zijn gekoppeld aan deze resource.

## <a name="endpoint-url-construction"></a>Eindpunt-URL bouwen
De eindpunt-URL overeenkomt met de Azure-regio die is gekoppeld aan de eindpuntsleutel.

Deze tabel weerspiegelt gemakkelijk uw publishing configuratie in het URL-eindpunt met de route-opties en waarden voor query-tekenreeks. Als u eindpunt-URL's voor uw toepassing LUIS aanroepen samenstellen zijn, controleert u of deze dezelfde routes en de queryreeks waarden worden ingesteld voor het eindpunt gebruikt--als u deze wilt instellen.

De URL-route is opgesteld met de regio en de app-ID. Als u in andere regio's of met andere apps publiceren wilt, kan de eindpunt-URL worden samengesteld door de regio en app-ID-waarden te wijzigen. 

* Selecteer de productiesite en de **publiceren** knop. Als het publiceren is geslaagd, moet u de weergegeven eindpunt-URL gebruiken voor toegang tot uw app LUIS. 

### <a name="optional-query-string-parameters"></a>Optionele van queryreeksparameters
De volgende parameters voor query-tekenreeks kunnen worden gebruikt met de eindpunt-URL:

<!-- TBD: what about speech priming? -->

|Querytekenreeks|Type|Voorbeeldwaarde|Doel|
|--|--|--|--|
|uitgebreide|booleaans|true|Omvatten [alle opzet scores](#include-all-predicted-intent-scores) voor utterance|
|van timezoneoffset is|nummer (eenheid is minuten)|60|Stel [tijdzone offset](luis-concept-data-alteration.md#change-time-zone-of-prebuilt-datetimev2-entity) voor [datetimeV2 vooraf gemaakte entiteiten](luis-reference-prebuilt-datetimev2.md)|
|Spellingcontrole|booleaans|true|[spelling corrigeren](#enable-bing-spell-checker) van utterance--gebruikt in combinatie met bing-spellen-controle-subscription-key querytekenreeksparameter|
|Bing-spellen-controle-subscription-key|abonnements-ID||gebruikt in combinatie met spellingcontrole queryreeksparameter opgeven|
|Fasering|booleaans|false|Selecteer staging- of -eindpunt|
|logboek|booleaans|true|toevoegen van query's en resultaten aan te melden|


## <a name="test-your-published-endpoint-in-a-browser"></a>Testen van uw gepubliceerde eindpunt in een browser
Test uw gepubliceerde eindpunt door te selecteren van de URL in de **eindpunt** kolom. De standaardbrowser geopend met de gegenereerde URL. Instellen van de URL-parameter "& q ' aan de testquery. Bijvoorbeeld toegevoegd `&q=Book me a flight to Boston on May 4` tot uw URL en druk op Enter. De JSON-reactie van uw HTTP-eindpunt wordt weergegeven in de browser. 

![JSON-antwoord van een gepubliceerde HTTP-eindpunt](./media/luis-how-to-publish-app/luis-publish-app-json-response.png)

## <a name="next-steps"></a>Volgende stappen

* Zie [sleutels beheren](./Manage-Keys.md) sleutels toevoegen aan uw app LUIS en meer informatie over hoe sleutels worden toegewezen aan de regio's.
* Zie [Train en uw app testen](interactive-test.md) voor instructies voor het testen van uw gepubliceerde app in de test-console.
