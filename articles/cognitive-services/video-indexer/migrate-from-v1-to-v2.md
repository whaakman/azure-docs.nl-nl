---
title: Migreren van Azure Video indexeerfunctie API v1 naar v2 | Microsoft Docs
description: In dit onderwerp wordt uitgelegd hoe van de API van Azure Video indexeerfunctie v1 naar v2 te migreren.
services: cognitive services
documentationcenter: ''
author: juliako
manager: erikre
ms.service: cognitive-services
ms.topic: article
ms.date: 05/13/2018
ms.author: juliako
ms.openlocfilehash: 00f5a0d7c9a37c24866cd5e1259800d5e431ccc3
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/11/2018
ms.locfileid: "35350104"
---
# <a name="migrate-from-the-video-indexer-api-v1-to-v2"></a>Migreren van de Video indexeerfunctie API v1 naar v2

> [!Note]
> De Video indexeerfunctie V1 API's zijn gedeprecieerd en wordt verwijderd tijdens het 1e augustus 2018. U moet beginnen met de Video indexeerfunctie v2 API's om te voorkomen van onderbrekingen.
>
> Om te zetten met Video indexeerfunctie v2-API's, raadpleegt u de instructies [hier](https://api-portal.videoindexer.ai/). 

Dit artikel worden de wijzigingen die zijn geïntroduceerd in v2.  

## <a name="api-changes"></a>Wijzigingen van de API

### <a name="authorization-and-operations"></a>Autorisatie en bewerkingen

Video indexeerfunctie gewijzigd in de versie van het v2, het model voor verificatie en autorisatie van de API. Er zijn twee sets van API's: 

* Autorisatie 
* Bewerkingen

De **autorisatie** API wordt gebruikt om op te halen toegangstokens voor aanroepen de **Operations** API. De **Operations** API bevat alle de Video indexeerfunctie API's, zoals het uploaden van video Get insights en andere bewerkingen.

Eenmaal u [abonneren](video-indexer-get-started.md) naar de **autorisatie** API, zal worden toegangstokens te verkrijgen door uw abonnementssleutel (net als in v1.)

## <a name="getting-and-using-the-access-token-for-operations-apis"></a>Ophalen en het gebruik van het toegangstoken voor bewerkingen API 's

Bij het aanroepen van de **Operations** API's, de abonnementssleutel niet meer worden gebruikt. In plaats daarvan geeft u de toegangstokens die zijn verkregen door de **autorisatie** API. 

Elke aanvraag moet een geldig token, die overeenkomt met het toegangsniveau van de API die u aanroept. Bewerkingen voor uw gebruiker, zoals het ophalen van uw accounts, vereisen bijvoorbeeld dat een toegangstoken van de gebruiker. Bewerkingen voor het account niveau, zoals de lijst met alle video's, vereisen een toegangstoken account. Bewerkingen voor video's, zoals opnieuw indexeren video vereisen een toegangstoken account of een video toegangstoken.

Eenvoudiger om, kunt u **autorisatie** API > **GetAccounts** uw accounts zonder het verkrijgen van een gebruiker-token ophalen eerst. U kunt ook vragen om op te halen van de accounts met geldige-tokens, zodat u kunt het overslaan van een extra aanroep voor het ophalen van een account-token.

Zie voor meer informatie over de verschillende toegangstokens [gebruik Azure Video indexeerfunctie API](video-indexer-use-apis.md).

### <a name="locations"></a>Locaties

Elke aanroep van de API moet de locatie van uw account indexeerfunctie Video opnemen. API-aanroepen zonder de locatie of met een verkeerde locatie mislukken.

De waarden in de volgende tabel beschreven toepassing. De **Param waarde** is de waarde die u wanneer doorgeeft met behulp van de API.

|**Naam**|**Waarde van parameter**|**Beschrijving**|
|---|---|---|
|Proefversie|proefversie|Gebruikt voor proefaccounts. Bijvoorbeeld: https://api.videoindexer.ai/trial/Accounts/{accountId}/Videos/{videoId}/Index?language=English.|
|VS - west|westus2|Gebruikt voor de regio Azure West ons 2.  Bijvoorbeeld: https://api.videoindexer.ai/westus2/Accounts/{accountId}/Videos/{videoId}/Index?language=English.|
|Noord-Europa |northeurope|Gebruikt voor de regio Noord-Europa Azure. Bijvoorbeeld https://api.videoindexer.ai/northeurope/Accounts/{accountId}/Videos/{videoId}/Index?language=English. |
|Oost-Azië|eastasia|Gebruikt voor de regio Azure Oost-Azië. Bijvoorbeeld https://api.videoindexer.ai/eastasia/Accounts/{accountId}/Videos/{videoId}/Index?language=English.|

### <a name="data-model"></a>Gegevensmodel

Video indexeerfunctie heeft nu een vereenvoudigde gegevensmodel veel duidelijker inzicht leveren. Zie voor meer informatie over de uitvoer geproduceerd door de API v2 [Bekijk de Video indexeerfunctie uitvoer geproduceerd door de API v2](video-indexer-output-json-v2.md).

### <a name="swagger"></a>Swagger

De Video indexeerfunctie API-definities zijn dienovereenkomstig bijgewerkt en kunnen worden gedownload via de [API portal](https://api-portal.videoindexer.ai/docs/services/authorization/operations/Get-Account-Access-Token).


### <a name="v1-vs-v2-examples"></a>V1-tegenover V2-voorbeelden

De nieuwe V2-API's kost 3 belangrijke parameters:

1. [Locatie] - zoals hierboven is beschreven. Proefversie, westus2, northeurope of Oost-Aziatische.
2. [YOUR_ACCOUNT_ID] - A-Guid-id van uw account. Opgehaald bij het ophalen van alle accounts (hieronder beschreven).
3. [YOUR_VIDEO_ID] - de id van uw video (bijvoorbeeld ' d4fa369abc'). Geretourneerd tijdens het uploaden van een video- of bij het zoeken naar video's.

#### <a name="uploading-a-video-in-v1"></a>Uploaden van een video in V1:

```
https://videobreakdown.azure-api.net/Breakdowns/Api/Partner/Breakdowns?name=some_name&description=some_description&privacy=private&videoUrl=http://URL_TO_YOUR_VIDEO
```

#### <a name="uploading-a-video-in-v2"></a>Uploaden van een video in V2:

1. Een toegangstoken ophalen voor de uploadaanvraag:

  U kunt ofwel alle accounts en de toegangstokens ophalen:

    ```
    https://api.videoindexer.ai/auth/[LOCATION]/Accounts?generateAccessTokens=true&allowEdit=true
    ```

  Of het specifieke account toegangstoken ophalen:
  
  ```
  https://api.videoindexer.ai/auth/[LOCATION]/Accounts/[YOUR_ACCOUNT_ID]/AccessToken?allowEdit=true
  ```
2. Uploaden van een video:

  ```
  POST https://api.videoindexer.ai/[LOCATION]/Accounts/[YOUR_ACCOUNT_ID]/Videos?name=MySample&description=MySampleDescription&videoUrl=[URL_ENCODED_VIDEO_URL]&accessToken=eyJ0eXAiOiJ...
  ```

#### <a name="getting-insights-in-v1"></a>Inzicht krijgen in V1:

```
https://videobreakdown.azure-api.net/Breakdowns/Api/Partner/Breakdowns/[VIDEO_ID]
```
  
#### <a name="getting-insights-in-v2"></a>Inzicht krijgen in V2:

1. Gebruik het toegangstoken account, of een toegangsniveau van de video-token ophalen:

  ```
  https://api.videoindexer.ai/auth/[LOCATION]/Accounts/[YOUR_ACCOUNT_ID]/Videos/[VIDEO_ID]/AccessToken
  ```
  
2. Inzichten verkrijgen:

  ```
  https://api.videoindexer.ai/trial/[LOCATION]/[YOUR_ACCOUNT_ID]/Videos/[VIDEO_ID]/Index?accessToken=eyJ0eXA...
  ```

#### <a name="getting-video-processing-state-in-v1"></a>Video verwerkingsstatus in V1 ophalen:

```
https://videobreakdown.azure-api.net/Breakdowns/Api/Partner/Breakdowns/[VIDEO_ID]/State
```
  
#### <a name="getting-video-processing-state-in-v2"></a>Video verwerkingsstatus in V2 ophalen:

In de API-v2 de verwerkingsstatus geretourneerd als onderdeel van de API Video Index ophalen.

1. Gebruik het toegangstoken account, of een toegangsniveau van de video-token ophalen:

  ```
  https://api.videoindexer.ai/trial/[LOCATION]/[YOUR_ACCOUNT_ID]/Videos/[VIDEO_ID]/Index?accessToken=eyJ0eXA...
  ```
  
2. Inzichten verkrijgen:

  ```
  https://api.videoindexer.ai/trial/[LOCATION]/[YOUR_ACCOUNT_ID]/Videos/[VIDEO_ID]/Index?accessToken=eyJ0eXA...
  ```

## <a name="next-steps"></a>Volgende stappen

[Gebruik Azure Video indexeerfunctie API](video-indexer-use-apis.md)

