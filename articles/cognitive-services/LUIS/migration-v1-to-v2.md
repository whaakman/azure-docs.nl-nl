---
title: Handleiding van v1 API migreren voor v2 | Microsoft Docs
titleSuffix: Azure
description: Meer informatie over hoe voor de migratie naar de nieuwste API instelt.
services: cognitive-services
author: v-geberr
manager: kamran.iqbal
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/01/2018
ms.author: v-geberr
ms.openlocfilehash: 45b6c2eda77668616a7e49ecd5ea2715af3cd3ce
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37111583"
---
# <a name="api-v2-migration-guide"></a>API v2-Migratiehandleiding
De versie 1 [eindpunt](https://aka.ms/v1-endpoint-api-docs) en [ontwerpen](https://aka.ms/v1-authoring-api-docs) API's wordt afgeschaft. Deze handleiding gebruiken om te begrijpen hoe migreren naar versie 2 [eindpunt](https://aka.ms/luis-endpoint-apis) en [ontwerpen](https://aka.ms/luis-authoring-apis) API's. 

## <a name="new-azure-regions"></a>Nieuwe Azure-regio 's
LUIS heeft nieuwe [regio's](https://aka.ms/LUIS-regions) opgegeven voor de LUIS APIs. LUIS biedt een andere website voor de regiogroepen. De toepassing moet worden geschreven in dezelfde regio die u verwacht te zoeken. Toepassingen regio's niet automatisch gemigreerd. U exporteren de app uit één regio en vervolgens importeren in een ander beschikbaar zijn in een nieuw gebied voor.

## <a name="authoring-route-changes"></a>Routewijzigingen ontwerpen
De authoring API route gewijzigd via de **prog** route voor het gebruik van de **api** route.


| versie | route |
|--|--|
|1|/Luis/V1.0/**prog**/apps|
|2|/Luis/**api**/v2.0/apps|


## <a name="endpoint-route-changes"></a>Eindpunt Routewijzigingen
Het API-eindpunt heeft nieuwe querystring parameters, evenals een ander antwoord. Als de uitgebreide vlag true is, worden alle intents, ongeacht de score, in een matrix met de naam intents, naast de topScoringIntent geretourneerd.

| versie | Route ophalen |
|--|--|
|1|/Luis/V1/Application? ID = {appId} & q = {q}|
|2|/ luis/v2.0/apps/{appId}?q={q} [& van timezoneoffset is] [& uitgebreide] [& spellingcontrole] [& staging] [& bing-spellen-controle-subscription-key] [& logboek]|


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

## <a name="key-management-no-longer-in-api"></a>Sleutelbeheer niet langer in de API
Het eindpunt abonnementssleutel API's zijn afgeschaft 410 GONE retourneren.

| versie | route |
|--|--|
|1|/Luis/V1.0/Prog/Subscriptions|
|1|/ luis/v1.0/prog/subscriptions/{subscriptionKey}|

Azure [eindpunt sleutels](luis-how-to-azure-subscription.md) worden gegenereerd in de Azure portal. Toewijzen van de sleutel aan een app LUIS op de **[publiceren](manage-keys.md)** pagina. U hoeft niet te weten de werkelijke waarde van de sleutel. LUIS wordt de naam van het abonnement te maken van de toewijzing. 

## <a name="new-versioning-route"></a>Nieuwe versioning route
Het model v2 is nu opgenomen in een [versie](luis-how-to-manage-versions.md). Een versienaam is 10 tekens in de route. De standaardversie is '0,1'.

| versie | route |
|--|--|
|1|/Luis/V1.0/**prog**/apps/ {appId} / entiteiten|
|2|/Luis/**api**/v2.0/apps/{appId}/**versies**/ {versionId} / entiteiten|

## <a name="metadata-renamed"></a>Metagegevens die zijn gewijzigd
Verschillende API's die LUIS metagegevens retourneren hebben nieuwe namen.

| de routenaam V1 | de routenaam v2 |
|--|--|
|PersonalAssistantApps |assistenten|
|applicationcultures|culturen|
|applicationdomains|domeinen|
|applicationusagescenarios|usagescenarios|


## <a name="sample-renamed-to-suggest"></a>"Voorbeeld" gewijzigd in 'voorstellen'
LUIS wordt voorgesteld utterances uit bestaande [eindpunt utterances](label-suggested-utterances.md) die het model kunnen verbeteren. In de vorige versie dit heette **voorbeeld**. In de nieuwe versie, de naam is gewijzigd van steekproef **voorstellen**. Dit heet **[bekijken eindpunt utterances](https://docs.microsoft.com/azure/cognitive-services/LUIS/label-suggested-utterances)** in de LUIS-website.

| versie | route |
|--|--|
|1|/Luis/V1.0/**prog**/apps/ {appId} /entities/ {id van de entiteit} /**voorbeeld**|
|1|/Luis/V1.0/**prog**/apps/ {appId} /intents/ {intentId} /**voorbeeld**|
|2|/Luis/**api**/v2.0/apps/{appId}/**versies**/ {versionId} /entities/ {id van de entiteit} /**voorstellen**|
|2|/Luis/**api**/v2.0/apps/{appId}/**versies**/ {versionId} /intents/ {intentId} /**voorstellen**|


## <a name="create-app-from-prebuilt-domains"></a>App te maken van vooraf gedefinieerde domeinen
[Vooraf gedefinieerde domeinen](luis-how-to-use-prebuilt-domains.md) bieden een vooraf gedefinieerde domeinmodel. Vooraf gedefinieerde domeinen kunnen u snel ontwikkelen van uw toepassing LUIS voor algemene domeinen. Deze API kunt u een nieuwe app op basis van een vooraf gedefinieerde domein maken. Het antwoord is de nieuwe appID.

|v2-route|Term|
|--|--|
|/Luis/API/v2.0/Apps/customprebuiltdomains  |Get, post|
|/ luis/api/v2.0/apps/customprebuiltdomains/{culture}  |Toevoegen|

## <a name="importing-1x-app-into-2x"></a>1.x-app te importeren in 2.x
De geëxporteerde 1.x van app-JSON heeft bepaalde zaken die u wijzigen wilt voordat u importeert in [LUIS] [ LUIS] 2.0. 

### <a name="prebuilt-entities"></a>Vooraf gedefinieerde entiteiten 
De [vooraf gemaakte entiteiten](Pre-builtEntities.md) zijn gewijzigd. Zorg ervoor dat u gebruikt de V2 vooraf gemaakte entiteiten. Dit omvat het gebruik [datetimeV2](pre-builtentities.md?#use-a-prebuilt-datetimev2-entity), in plaats van de datum/tijd. 

### <a name="actions"></a>Acties
De eigenschap acties is niet meer geldig. Het moet leeg zijn 

### <a name="labeled-utterances"></a>Gelabelde utterances
V1 toegestaan gelabelde utterances aan het begin of einde van woord of woordgroep uit spaties bestaan. De spaties verwijderd. 

## <a name="common-reasons-for-http-response-status-codes"></a>Veelvoorkomende redenen voor HTTP-antwoord statuscodes
Zie [reactiecodes LUIS API](luis-reference-response-codes.md).

## <a name="next-steps"></a>Volgende stappen

Gebruik de v2 API-documentatie voor het bijwerken van bestaande REST naar LIUS aanroepen [eindpunt](https://aka.ms/luis-endpoint-apis) en [ontwerpen](https://aka.ms/luis-authoring-apis) API's. 

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions