---
title: Upgrade van Bing Video API v5 naar v7 | Microsoft Docs
description: Identificeert de onderdelen van uw toepassing die u nodig hebt om bij te werken als versie 7 wilt gebruiken.
services: cognitive-services
author: swhite-msft
manager: ehansen
ms.assetid: FA7DDF07-97AC-4EBE-8C50-A9737D43B38E
ms.service: cognitive-services
ms.component: bing-video-search
ms.topic: article
ms.date: 04/15/2017
ms.author: scottwhi
ms.openlocfilehash: 62646d026e141d0549c68e18f9318fa32d3e00df
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344493"
---
# <a name="video-search-api-upgrade-guide"></a>Video-API van zoekservice upgrade guide

Deze upgrade handleiding identificeert de wijzigingen tussen 5 en versie 7 van de API van Bing Video zoeken. Gebruik deze handleiding kunt u de onderdelen van uw toepassing die u nodig hebt om bij te werken voor het gebruik van versie 7 herkennen.

## <a name="breaking-changes"></a>Wijzigingen die fouten veroorzaken

### <a name="endpoints"></a>Eindpunten

- Het versienummer van het eindpunt is gewijzigd van v5 in v7. Bijvoorbeeld: https://api.cognitive.microsoft.com/bing/\ * \*v7.0**/videos/search.

### <a name="error-response-objects-and-error-codes"></a>Fout antwoord objecten en foutcodes

- Alle mislukte aanvragen moeten omvatten nu een `ErrorResponse` -object in de hoofdtekst van de reactie.

- De volgende velden te toegevoegd de `Error` object.  
  - `subCode`&mdash;De foutcode partities indien mogelijk in discrete buckets
  - `moreDetails`&mdash;Aanvullende informatie over de fout wordt beschreven in de `message` veld
   

- De foutcodes v5 vervangen door de volgende mogelijke `code` en `subCode` waarden.

|Code|SubCode|Beschrijving
|-|-|-
|ServerError|UnexpectedError<br/>ResourceError<br/>Niet geïmplementeerd|Bing retourneert ServerError wanneer een van de onderliggende code voorwaarden zich voordoen. Het antwoord bevat deze fouten als de HTTP-statuscode 500 is.
|InvalidRequest|ParameterMissing<br/>ParameterInvalidValue<br/>HttpNotAllowed<br/>Geblokkeerd|Bing retourneert InvalidRequest wanneer een deel van de aanvraag niet geldig is. Bijvoorbeeld, een vereiste parameter ontbreekt of een parameterwaarde is niet geldig.<br/><br/>Als de fout ParameterMissing of ParameterInvalidValue is, is de HTTP-statuscode 400.<br/><br/>Als de fout HttpNotAllowed, de HTTP-statuscode 410 is.
|RateLimitExceeded||Bing retourneert RateLimitExceeded wanneer u uw query's per seconde (QPS) of de query's per maand (QPM)-quota overschrijdt.<br/><br/>Bing retourneert HTTP-statuscode 429 als overschreden QPS en 403 als u QPM overschreden.
|InvalidAuthorization|AuthorizationMissing<br/>AuthorizationRedundancy|Bing retourneert InvalidAuthorization wanneer Bing de aanroeper kan niet worden geverifieerd. Bijvoorbeeld, de `Ocp-Apim-Subscription-Key` header ontbreekt of de abonnementssleutel is niet geldig.<br/><br/>Redundantie treedt op als u meer dan één verificatiemethode opgeven.<br/><br/>Als de fout InvalidAuthorization is, is de HTTP-statuscode 401.
|InsufficientAuthorization|AuthorizationDisabled<br/>AuthorizationExpired|Bing retourneert InsufficientAuthorization wanneer de aanroeper is niet gemachtigd voor toegang tot de bron. Dit kan gebeuren als de abonnementssleutel is uitgeschakeld of is verlopen. <br/><br/>Als de fout InsufficientAuthorization is, is de HTTP-statuscode 403.

- Het volgende worden de vorige foutcodes toegewezen aan de nieuwe codes. Als u een afhankelijkheid v5 foutcodes hebt genomen, worden uw code dienovereenkomstig bijgewerkt.

|Versie 5-code|Versie 7 code.subCode
|-|-
|RequestParameterMissing|InvalidRequest.ParameterMissing
RequestParameterInvalidValue|InvalidRequest.ParameterInvalidValue
ResourceAccessDenied|InsufficientAuthorization
ExceededVolume|RateLimitExceeded
ExceededQpsLimit|RateLimitExceeded
Uitgeschakeld|InsufficientAuthorization.AuthorizationDisabled
UnexpectedError|ServerError.UnexpectedError
DataSourceErrors|ServerError.ResourceError
AuthorizationMissing|InvalidAuthorization.AuthorizationMissing
HttpNotAllowed|InvalidRequest.HttpNotAllowed
UserAgentMissing|InvalidRequest.ParameterMissing
Niet geïmplementeerd|ServerError.NotImplemented
InvalidAuthorization|InvalidAuthorization
InvalidAuthorizationMethod|InvalidAuthorization
MultipleAuthorizationMethod|InvalidAuthorization.AuthorizationRedundancy
ExpiredAuthorizationToken|InsufficientAuthorization.AuthorizationExpired
InsufficientScope|InsufficientAuthorization
Geblokkeerd|InvalidRequest.Blocked

### <a name="query-parameters"></a>Queryparameters

- Hernoemd de `modulesRequested` queryparameter naar [modules](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#modulesrequested).  

### <a name="object-changes"></a>Wijzigingen van object

- Hernoemd de `nextOffsetAddCount` veld [video's](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#videos) naar `nextOffset`. De manier waarop u de offset is ook gewijzigd. Eerder, stelt u de [offset](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#offset) query-parameter voor de `nextOffset` waarde plus de vorige waarde van de offset en het aantal video's in het resultaat. Nu u gewoon instellen de `offset` query-parameter voor de `nextOffset` waarde.  
  
- Gewijzigd van het gegevenstype van de `relatedVideos` veld `Video[]` naar [VideosModule](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#videosmodule) (Zie [VideoDetails](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#videodetails)).

