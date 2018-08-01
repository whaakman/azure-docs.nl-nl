---
title: Migreren van Azure Video Indexer-API v1 in v2 | Microsoft Docs
description: In dit onderwerp wordt uitgelegd over het migreren van de Azure-Video Indexer-API v1 in v2.
services: cognitive services
documentationcenter: ''
author: juliako
manager: erikre
ms.service: cognitive-services
ms.topic: article
ms.date: 07/25/2018
ms.author: juliako
ms.openlocfilehash: 1883c449def44f6c54c6f20317de39ada405643f
ms.sourcegitcommit: e3d5de6d784eb6a8268bd6d51f10b265e0619e47
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2018
ms.locfileid: "39388967"
---
# <a name="migrate-from-the-video-indexer-api-v1-to-v2"></a>Migreren van de Video Indexer-API v1 in v2

> [!Note]
> De Video Indexer-API V1 is op 1 augustus 2018 afgeschaft. U moet nu de v2-API voor Video Indexer gebruiken. <br/>Als u wilt ontwikkelen met Video Indexer v2-API's, Zie de instructies [hier](https://api-portal.videoindexer.ai/). 

Dit artikel beschrijft de wijzigingen die zijn geïntroduceerd in versie 2.  

## <a name="api-changes"></a>API-wijzigingen

### <a name="authorization-and-operations"></a>Autorisatie en bewerkingen

Video Indexer gewijzigd in de v2-versie, het model voor verificatie en autorisatie van de API. Er zijn twee sets met API's: 

* Autorisatie 
* Bewerkingen

De **autorisatie** API wordt gebruikt om op te halen van de toegangstokens voor het aanroepen de **Operations** API. De **Operations** API bevat alle Video Indexer API's, zoals het uploaden van video, verzamel inzichten en andere bewerkingen.

Nadat u [abonneren](video-indexer-get-started.md) naar de **autorisatie** API, kunt u zich aan toegangstokens verkrijgen door door te geven uw abonnementssleutel (net als in v1.)

## <a name="getting-and-using-the-access-token-for-operations-apis"></a>Ophalen van en het gebruik van het toegangstoken voor bewerkingen van API 's

Bij het aanroepen van de **Operations** API's, de abonnementssleutel niet meer worden gebruikt. In plaats daarvan geeft u de toegangstokens te geven die zijn verkregen met de **autorisatie** API. 

Elke aanvraag moet een geldig token, die overeenkomt met het toegangsniveau van de API die u aanroept. Bewerkingen voor uw gebruiker, zoals het ophalen van uw accounts, vereisen bijvoorbeeld een token voor gebruikerstoegang. Bewerkingen voor het account niveau, zoals de lijst met alle video's, een toegangstoken voor account nodig. Bewerkingen voor video's, zoals opnieuw indexeren video, vereist een toegangstoken voor account of een video toegangstoken.

Dingen om eenvoudiger te maken, kunt u **autorisatie** API > **GetAccounts** ophalen van uw accounts zonder het verkrijgen van een gebruiker token eerst. U kunt ook vragen om op te halen van de accounts met behulp van geldige tokens, zodat u kunt het overslaan van een extra aanroep van een account-token ophalen.

Zie voor meer informatie over de verschillende toegangstokens [gebruik Azure Video Indexer-API](video-indexer-use-apis.md).

### <a name="locations"></a>Locaties

Elke aanroep naar de API moet de locatie van uw Video Indexer-account bevatten. API-aanroepen zonder de locatie of met een verkeerde locatie mislukken.

De waarden die worden beschreven in de volgende tabel zijn van toepassing. De **Param waarde** is de waarde die u als doorgeven met behulp van de API.

|**Naam**|**Waarde van parameter**|**Beschrijving**|
|---|---|---|
|Proefversie|proefversie|Voor proefaccounts gebruikt. Bijvoorbeeld: https://api.videoindexer.ai/trial/Accounts/{accountId}/Videos/{videoId}/Index?language=English.|
|US - west|westus2|Gebruikt voor de regio van Azure VS-West 2.  Bijvoorbeeld: https://api.videoindexer.ai/westus2/Accounts/{accountId}/Videos/{videoId}/Index?language=English.|
|Europa - noord |northeurope|Voor de regio Noord-Europa Azure gebruikt. Bijvoorbeeld https://api.videoindexer.ai/northeurope/Accounts/{accountId}/Videos/{videoId}/Index?language=English. |
|Azië - oost|eastasia|Gebruikt voor de regio Oost-Azië van Azure. Bijvoorbeeld https://api.videoindexer.ai/eastasia/Accounts/{accountId}/Videos/{videoId}/Index?language=English.|

### <a name="data-model"></a>Gegevensmodel

Video Indexer heeft nu een vereenvoudigde gegevensmodel te veel betere inzichten leveren. Zie voor meer informatie over de uitvoer geproduceerd door de v2-API, [Bekijk de Video Indexer-uitvoer geproduceerd door de API v2](video-indexer-output-json-v2.md).

### <a name="swagger"></a>Swagger

De Video Indexer-API-definities zijn bijgewerkt en kunnen worden gedownload via de [API-portal](https://api-portal.videoindexer.ai/docs/services/authorization/operations/Get-Account-Access-Token).


### <a name="v1-vs-v2-examples"></a>V1 en V2-voorbeelden

De nieuwe V2-API's hebben betrekking op 3 belangrijke parameters:

1. [LOCATION] - zoals hierboven is beschreven. Een proefversie, westus2, northeurope of Oost-Aziatische.
2. [YOUR_ACCOUNT_ID] - een Guid-id van uw account. Bij het ophalen van alle accounts (Zie hieronder) opgehaald.
3. [YOUR_VIDEO_ID] - de id van uw video (bijvoorbeeld ' d4fa369abc'). Geretourneerd tijdens het uploaden van een video of bij het zoeken naar video's.

#### <a name="uploading-a-video-in-v1"></a>Uploaden van video's in V1:

```
https://videobreakdown.azure-api.net/Breakdowns/Api/Partner/Breakdowns?name=some_name&description=some_description&privacy=private&videoUrl=http://URL_TO_YOUR_VIDEO
```

#### <a name="uploading-a-video-in-v2"></a>Uploaden van video's in V2:

1. Een toegangstoken ophalen voor het van uploadaanvraag:

  U kunt ofwel alle accounts en hun toegangstokens ophalen:

    ```
    https://api.videoindexer.ai/auth/[LOCATION]/Accounts?generateAccessTokens=true&allowEdit=true
    ```

  Of het specifieke account toegangstoken ophalen:
  
  ```
  https://api.videoindexer.ai/auth/[LOCATION]/Accounts/[YOUR_ACCOUNT_ID]/AccessToken?allowEdit=true
  ```
2. Een video uploaden:

  ```
  POST https://api.videoindexer.ai/[LOCATION]/Accounts/[YOUR_ACCOUNT_ID]/Videos?name=MySample&description=MySampleDescription&videoUrl=[URL_ENCODED_VIDEO_URL]&accessToken=eyJ0eXAiOiJ...
  ```

#### <a name="getting-insights-in-v1"></a>Inzicht krijgen in V1:

```
https://videobreakdown.azure-api.net/Breakdowns/Api/Partner/Breakdowns/[VIDEO_ID]
```
  
#### <a name="getting-insights-in-v2"></a>Inzicht krijgen in V2:

1. Gebruik het toegangstoken voor account, of een toegangsniveau van de video-token verkrijgen:

  ```
  https://api.videoindexer.ai/auth/[LOCATION]/Accounts/[YOUR_ACCOUNT_ID]/Videos/[VIDEO_ID]/AccessToken
  ```
  
2. Inzichten verkrijgen:

  ```
  https://api.videoindexer.ai/trial/[LOCATION]/[YOUR_ACCOUNT_ID]/Videos/[VIDEO_ID]/Index?accessToken=eyJ0eXA...
  ```

#### <a name="getting-video-processing-state-in-v1"></a>Ophalen van de status in V1 videoverwerking:

```
https://videobreakdown.azure-api.net/Breakdowns/Api/Partner/Breakdowns/[VIDEO_ID]/State
```
  
#### <a name="getting-video-processing-state-in-v2"></a>Ophalen van de status in V2 videoverwerking:

In v2 API, wordt de verwerkingsstatus geretourneerd als onderdeel van de API Video-Index ophalen.

1. Gebruik het toegangstoken voor account, of een toegangsniveau van de video-token verkrijgen:

  ```
  https://api.videoindexer.ai/trial/[LOCATION]/[YOUR_ACCOUNT_ID]/Videos/[VIDEO_ID]/Index?accessToken=eyJ0eXA...
  ```
  
2. Inzichten verkrijgen:

  ```
  https://api.videoindexer.ai/trial/[LOCATION]/[YOUR_ACCOUNT_ID]/Videos/[VIDEO_ID]/Index?accessToken=eyJ0eXA...
  ```

## <a name="next-steps"></a>Volgende stappen

[Gebruik Azure Video Indexer-API](video-indexer-use-apis.md)

