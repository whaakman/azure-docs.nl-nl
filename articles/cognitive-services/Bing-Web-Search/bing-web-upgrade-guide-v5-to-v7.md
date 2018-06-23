---
title: Upgrade van Bing zoeken-API v5 naar v7 Web | Microsoft Docs
description: Identificeert de onderdelen van uw toepassing die u nodig hebt om bij te werken als versie 7 wilt gebruiken.
services: cognitive-services
author: swhite-msft
manager: ehansen
ms.assetid: E8827BEB-4379-47CE-B67B-6C81AD7DAEB1
ms.service: cognitive-services
ms.component: bing-web-search
ms.topic: article
ms.date: 01/15/2017
ms.author: scottwhi
ms.openlocfilehash: 155297f230c0ee02d6fa49d6d35eb24d9941f29b
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345457"
---
# <a name="web-search-api-upgrade-guide"></a>Web-API van zoekservice upgrade guide

Deze upgrade handleiding identificeert de wijzigingen tussen 5 en versie 7 van de API van Bing Web zoeken. Gebruik deze handleiding kunt u de onderdelen van uw toepassing die u nodig hebt om bij te werken voor het gebruik van versie 7 herkennen.

## <a name="breaking-changes"></a>Wijzigingen die fouten veroorzaken

### <a name="endpoints"></a>Eindpunten

- Het versienummer van het eindpunt is gewijzigd van v5 in v7. Bijvoorbeeld: https:\/\/api.cognitive.microsoft.com/bing/**v7.0**  /zoeken.

### <a name="error-response-objects-and-error-codes"></a>Fout antwoord objecten en foutcodes

- Alle mislukte aanvragen moeten omvatten nu een `ErrorResponse` -object in de hoofdtekst van de reactie.

- De volgende velden te toegevoegd de `Error` object.  
  - `subCode`&mdash;De foutcode partities indien mogelijk in discrete buckets
  - `moreDetails`&mdash;Aanvullende informatie over de fout wordt beschreven in de `message` veld
   

- De foutcodes v5 vervangen door de volgende mogelijke `code` en `subCode` waarden.

|Code|SubCode|Beschrijving
|-|-|-
|ServerError|UnexpectedError<br/>ResourceError<br/>Niet geïmplementeerd|Bing retourneert ServerError wanneer een van de onderliggende code voorwaarden zich voordoen. De reactie omvat deze fouten als de HTTP-statuscode 500 is.
|InvalidRequest|ParameterMissing<br/>ParameterInvalidValue<br/>HttpNotAllowed<br/>Geblokkeerd|Bing retourneert InvalidRequest wanneer een deel van de aanvraag niet geldig is. Bijvoorbeeld, een vereiste parameter ontbreekt of een parameterwaarde is niet geldig.<br/><br/>Als de fout ParameterMissing of ParameterInvalidValue is, is de HTTP-statuscode 400.<br/><br/>Als de fout HttpNotAllowed, de HTTP-statuscode 410 is.
|RateLimitExceeded||Bing retourneert RateLimitExceeded wanneer u uw query's per seconde (QPS) of de query's per maand (QPM)-quota overschrijdt.<br/><br/>Bing retourneert HTTP-statuscode 429 als overschreden QPS en 403 als u QPM overschreden.
|InvalidAuthorization|AuthorizationMissing<br/>AuthorizationRedundancy|Bing retourneert InvalidAuthorization wanneer Bing de aanroeper kan niet worden geverifieerd. Bijvoorbeeld, de `Ocp-Apim-Subscription-Key` header ontbreekt of de abonnementssleutel is niet geldig.<br/><br/>Redundantie treedt op als u meer dan één verificatiemethode opgeven.<br/><br/>Als de fout InvalidAuthorization is, is de HTTP-statuscode 401.
|InsufficientAuthorization|AuthorizationDisabled<br/>AuthorizationExpired|Bing retourneert InsufficientAuthorization wanneer de aanroeper is niet gemachtigd voor toegang tot de bron. Deze fout kan optreden als de abonnementssleutel is uitgeschakeld of is verlopen. <br/><br/>Als de fout InsufficientAuthorization is, is de HTTP-statuscode 403.

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


## <a name="non-breaking-changes"></a>Niet-grote wijzigingen  

### <a name="headers"></a>Headers

- De optionele toegevoegd [Pragma](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#pragma) aanvraag-header. Bing retourneert standaard de inhoud in cache, indien beschikbaar. Om te voorkomen Bing retourneert de inhoud in cache, stelt u de header Pragma op no-cache (bijvoorbeeld Pragma: Nee-cache).

### <a name="query-parameters"></a>Queryparameters

- Toegevoegd de [answerCount](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#answercount) queryparameter. Gebruik deze parameter geeft het aantal antwoorden die u wilt dat het antwoord om op te nemen. De antwoorden worden gekozen op basis van de positie. Als u deze parameter instelt op bijvoorbeeld drie (3), het antwoord bevat de bovenste drie gerangschikte antwoorden.  
  
- Toegevoegd de [promoveren](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#promote) queryparameter. Gebruik deze parameter samen met `answerCount` expliciet opnemen van een of meer antwoord typen, ongeacht hun positie. Bijvoorbeeld, als u video's en afbeeldingen in het antwoord verhogen, stelt u promoveren tot *video's, afbeeldingen*. De lijst met antwoorden die u wilt promoveren komt niet in mindering gebracht op de `answerCount` limiet. Bijvoorbeeld, als `answerCount` is 2 en `promote` is ingesteld op *video's, afbeeldingen*, het antwoord kan onder andere webpagina's, nieuws, video's en afbeeldingen.

### <a name="object-changes"></a>Wijzigingen van object

- Toegevoegd de `someResultsRemoved` veld de [WebAnswer](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#webanswer) object. Het veld bevat een Booleaanse waarde die aangeeft of het antwoord bepaalde resultaten van het antwoord van de webserver uitgesloten.  

