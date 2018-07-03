---
title: Handleiding API migreren van v1 in v2 | Microsoft Docs
titleSuffix: Azure
description: Informatie over hoe voor migratie naar de meest recente API instellen.
services: cognitive-services
author: v-geberr
manager: kamran.iqbal
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/01/2018
ms.author: v-geberr
ms.openlocfilehash: 7174a78aeb339c864b2eea384b794646c215bc25
ms.sourcegitcommit: 756f866be058a8223332d91c86139eb7edea80cc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2018
ms.locfileid: "37343999"
---
# <a name="api-v2-migration-guide"></a>Migratiehandleiding voor API-versie 2
De versie 1 [eindpunt](https://aka.ms/v1-endpoint-api-docs) en [ontwerpen](https://aka.ms/v1-authoring-api-docs) API's wordt afgeschaft. Gebruik deze handleiding voor meer informatie over het migreren naar versie 2 [eindpunt](https://aka.ms/luis-endpoint-apis) en [ontwerpen](https://aka.ms/luis-authoring-apis) API's. 

## <a name="new-azure-regions"></a>Nieuwe Azure-regio 's
LUIS heeft nieuwe [regio's](https://aka.ms/LUIS-regions) opgegeven voor de APIs LUIS. LUIS, biedt een andere website voor regiogroepen. De toepassing moet worden gemaakt in dezelfde regio die u verwacht te vragen. Toepassingen kunnen de regio's niet automatisch gemigreerd. U exporteert de app vanuit één regio en vervolgens importeren in een andere waarde voor deze beschikbaar zijn in een nieuwe regio.

## <a name="authoring-route-changes"></a>Routewijzigingen ontwerpen
De ontwerphandleiding API-route gewijzigd via de **prog** route voor het gebruik van de **api** route.


| versie | route |
|--|--|
|1|/Luis/V1.0/**prog**/apps|
|2|/Luis/**api**/v2.0/apps|


## <a name="endpoint-route-changes"></a>Eindpunt Routewijzigingen
Het API-eindpunt heeft nieuwe querystring-parameters, evenals een andere reactie. Als de uitgebreide vlag op true is ingesteld, worden alle intents, ongeacht de score, worden geretourneerd in een matrix met de naam intents, naast de topScoringIntent.

| versie | Route ophalen |
|--|--|
|1|/Luis/V1/Application? ID = {appId} & q = {q}|
|2|/ luis/v2.0/apps/{appId}?q={q} [& timezoneOffset] [& uitgebreide] [& spellingcontrole] [& staging] [& bing-spellingcontrole-controle-subscription-key] [& logboek]|


V1-eindpunt geslaagd antwoord:
```JSON
{
  "odata.metadata":"https://dialogice.cloudapp.net/odata/$metadata#domain","value":[
    {
      "id":"bccb84ee-4bd6-4460-a340-0595b12db294","q":"turn on the camera","response":"[{\"intent\":\"OpenCamera\",\"score\":0.976928055},{\"intent\":\"None\",\"score\":0.0230718572}]"
    }
  ]
}
```

v2-eindpunt geslaagd antwoord:
```JSON
{
  "query": "forward to frank 30 dollars through HSBC",
  "topScoringIntent": {
    "intent": "give",
    "score": 0.3964121
  },
  "entities": [
    {
      "entity": "30",
      "type": "builtin.number",
      "startIndex": 17,
      "endIndex": 18,
      "resolution": {
        "value": "30"
      }
    },
    {
      "entity": "frank",
      "type": "frank",
      "startIndex": 11,
      "endIndex": 15,
      "score": 0.935219169
    },
    {
      "entity": "30 dollars",
      "type": "builtin.currency",
      "startIndex": 17,
      "endIndex": 26,
      "resolution": {
        "unit": "Dollar",
        "value": "30"
      }
    },
    {
      "entity": "hsbc",
      "type": "Bank",
      "startIndex": 36,
      "endIndex": 39,
      "resolution": {
        "values": [
          "BankeName"
        ]
      }
    }
  ]
}
```

## <a name="key-management-no-longer-in-api"></a>Sleutelbeheer niet meer in de API
De abonnement-eindpuntsleutel API's zijn afgeschaft, retourneren 410 verwijderd.

| versie | route |
|--|--|
|1|/Luis/V1.0/Prog/Subscriptions|
|1|/ luis/v1.0/prog/subscriptions/{subscriptionKey}|

Azure [eindpunt sleutels](luis-how-to-azure-subscription.md) worden gegenereerd in Azure portal. Toewijzen van de sleutel aan een LUIS-app op de **[publiceren](luis-how-to-manage-keys.md)** pagina. U hoeft niet te weten de werkelijke waarde van de sleutel. LUIS wordt de naam van het abonnement te maken van de toewijzing. 

## <a name="new-versioning-route"></a>Nieuwe versiebeheer route
De v2-model is nu opgenomen in een [versie](luis-how-to-manage-versions.md). De versienaam van een is 10 tekens in de route. De standaardversie is '0.1'.

| versie | route |
|--|--|
|1|/Luis/V1.0/**prog**/apps/ {appId} / entiteiten|
|2|/Luis/**api**/v2.0/apps/{appId}/**versies**/ {versionId} / entiteiten|

## <a name="metadata-renamed"></a>De metagegevens van de naam gewijzigd
Verschillende API's die de metagegevens van LUIS retourneren hebben nieuwe namen.

| V1-routenaam | de routenaam v2 |
|--|--|
|PersonalAssistantApps |assistenten|
|applicationcultures|culturen|
|applicationdomains|domeinen|
|applicationusagescenarios|usagescenarios|


## <a name="sample-renamed-to-suggest"></a>"Voorbeeld" gewijzigd in "voorstellen"
LUIS stelt uitingen van bestaande [eindpunt uitingen](label-suggested-utterances.md) die het model kan verbeteren. In de vorige versie, is dit de naam **voorbeeld**. In de nieuwe versie, de naam wordt gewijzigd van voorbeeld **voorstellen**. Dit heet **[bekijken eindpunt uitingen](https://docs.microsoft.com/azure/cognitive-services/LUIS/label-suggested-utterances)** in de LUIS-website.

| versie | route |
|--|--|
|1|/Luis/V1.0/**prog**/apps/ {appId} /entities/ {entityId} /**voorbeeld**|
|1|/Luis/V1.0/**prog**/apps/ {appId} /intents/ {intentId} /**voorbeeld**|
|2|/Luis/**api**/v2.0/apps/{appId}/**versies**/ {versionId} /entities/ {entityId} /**voorstellen**|
|2|/Luis/**api**/v2.0/apps/{appId}/**versies**/ {versionId} /intents/ {intentId} /**voorstellen**|


## <a name="create-app-from-prebuilt-domains"></a>App maken vanuit de vooraf gemaakte domeinen
[Vooraf gemaakte domeinen](luis-how-to-use-prebuilt-domains.md) bieden een vooraf gedefinieerde domeinmodel. Vooraf gemaakte domeinen kunnen u snel uw LUIS-toepassing voor algemene domeinen kunnen ontwikkelen. Deze API kunt u een nieuwe app op basis van een vooraf gedefinieerde domein maken. Het antwoord is de nieuwe appID.

|v2-route|term|
|--|--|
|/Luis/API/v2.0/Apps/customprebuiltdomains  |Get, post|
|/ luis/api/v2.0/apps/customprebuiltdomains/{culture}  |Toevoegen|

## <a name="importing-1x-app-into-2x"></a>Importeren van 1.x-app in 2.x
De geëxporteerde 1.x van app-JSON heeft bepaalde gebieden die u nodig hebt om te wijzigen voordat u importeert in [LUIS] [ LUIS] 2.0. 

### <a name="prebuilt-entities"></a>Vooraf gemaakte entiteiten 
De [vooraf gemaakte entiteiten](luis-prebuilt-entities.md) zijn gewijzigd. Zorg ervoor dat u met behulp van de V2 vooraf gemaakte entiteiten. Dit omvat het gebruik [datetimeV2](luis-prebuilt-entities.md#use-a-prebuilt-datetimev2-entity), in plaats van datum/tijd. 

### <a name="actions"></a>Acties
De eigenschap actions is niet meer geldig. Deze moet leeg zijn 

### <a name="labeled-utterances"></a>Gelabelde uitingen
V1 toegestaan gelabelde uitingen aan het begin of einde van het woord of zinsdeel uit spaties bestaan. De spaties verwijderd. 

## <a name="common-reasons-for-http-response-status-codes"></a>Veelvoorkomende redenen voor HTTP-responscodes status
Zie [LUIS-API-responscodes](luis-reference-response-codes.md).

## <a name="next-steps"></a>Volgende stappen

Gebruik de v2-API-documentatie voor het bijwerken van bestaande REST-naar LIUS aanroepen [eindpunt](https://aka.ms/luis-endpoint-apis) en [ontwerpen](https://aka.ms/luis-authoring-apis) API's. 

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions