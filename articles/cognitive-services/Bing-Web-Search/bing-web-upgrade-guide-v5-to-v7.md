---
title: Upgrade van de API-versie 5 naar versie 7 - Bing webzoekopdrachten-API
titleSuffix: Azure Cognitive Services
description: Bepalen welke onderdelen van uw toepassing vereisen updates voor de Bing webzoekopdrachten v7 API's gebruiken.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.assetid: E8827BEB-4379-47CE-B67B-6C81AD7DAEB1
ms.service: cognitive-services
ms.subservice: bing-web-search
ms.topic: reference
ms.date: 01/15/2017
ms.author: scottwhi
ms.openlocfilehash: 74afd08aaa2e512ae3bcae302f50585eb8024306
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55864074"
---
# <a name="upgrade-from-bing-web-search-api-v5-to-v7"></a>Een upgrade uitvoeren voor Bing Web Search API versie 5 naar versie 7

Deze upgrade handleiding identificeert de wijzigingen tussen 5 en versie 7 van de Bing webzoekopdrachten-API. Deze handleiding gebruiken om te bepalen van de onderdelen van uw toepassing die u nodig hebt om bij te werken voor het gebruik van versie 7.

## <a name="breaking-changes"></a>Wijzigingen die fouten veroorzaken

### <a name="endpoints"></a>Eindpunten

- Het versienummer van het eindpunt is gewijzigd van versie 5 in versie 7. Bijvoorbeeld: https:\/\/api.cognitive.microsoft.com/bing/**v7.0**  /zoeken.

### <a name="error-response-objects-and-error-codes"></a>Fout antwoordobjecten en foutcodes

- Alle mislukte aanvragen moeten nu bevatten een `ErrorResponse` object in de hoofdtekst van het antwoord.

- De volgende velden toevoegt aan de `Error` object.  
  - `subCode`&mdash;Indien mogelijk partities van de foutcode in discrete buckets
  - `moreDetails`&mdash;Als u meer informatie over de fout wordt beschreven in de `message` veld


- De foutcodes v5 vervangen door de volgende mogelijke `code` en `subCode` waarden.

|Code|SubCode|Description
|-|-|-
|ServerError|UnexpectedError<br/>ResourceError<br/>Niet geïmplementeerd|Bing retourneert ServerError wanneer een van de voorwaarden van de onderliggende code optreden. De reactie omvat deze fouten als de HTTP-statuscode 500.
|InvalidRequest|ParameterMissing<br/>ParameterInvalidValue<br/>HttpNotAllowed<br/>Geblokkeerd|Bing retourneert InvalidRequest wanneer er een deel van de aanvraag niet geldig is. Bijvoorbeeld, een vereiste parameter ontbreekt of een parameterwaarde is niet geldig.<br/><br/>Als de fout ParameterMissing of ParameterInvalidValue is, is de HTTP-statuscode 400.<br/><br/>Als de fout HttpNotAllowed, de HTTP-statuscode 410 is.
|RateLimitExceeded||Bing retourneert RateLimitExceeded wanneer u uw query's per seconde (QPS) of query's per maand (QPM) quotum overschrijdt.<br/><br/>Bing retourneert HTTP-statuscode 429 als u overschreden QPS en 403 als u QPM overschreden.
|InvalidAuthorization|AuthorizationMissing<br/>AuthorizationRedundancy|Bing retourneert InvalidAuthorization wanneer Bing de oproepende functie kan niet worden geverifieerd. Bijvoorbeeld, de `Ocp-Apim-Subscription-Key` koptekst ontbreekt of de abonnementssleutel is niet geldig.<br/><br/>Redundantie treedt op als u meer dan één verificatiemethode opgeven.<br/><br/>Als de fout InvalidAuthorization is, is de HTTP-statuscode 401.
|InsufficientAuthorization|AuthorizationDisabled<br/>AuthorizationExpired|Bing retourneert InsufficientAuthorization wanneer de oproepende functie heeft geen machtigingen voor toegang tot de resource. Deze fout kan optreden als de abonnementssleutel is uitgeschakeld of is verlopen. <br/><br/>Als de fout InsufficientAuthorization is, is de HTTP-statuscode 403.

- Het volgende wordt de vorige foutcodes toegewezen aan de nieuwe codes. Als u een afhankelijkheid hebt gemaakt in de foutcodes versie 5, overeenkomstig uw code bijwerken.

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


## <a name="non-breaking-changes"></a>Niet-belangrijke wijzigingen  

### <a name="headers"></a>Headers

- De optionele toegevoegd [Pragma](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#pragma) aanvraagheader. Bing retourneert standaard cache-inhoud, indien beschikbaar. Om te voorkomen dat Bing inhoud uit de cache retourneert, stelt u de Pragma-header in op no-cache (bijvoorbeeld: Pragma: no-cache).

### <a name="query-parameters"></a>Queryparameters

- Toegevoegd de [answerCount](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#answercount) queryparameter. Gebruik deze parameter om op te geven van het aantal antwoorden die u wilt dat het antwoord om op te nemen. De antwoorden worden gekozen op basis van de positie. Bijvoorbeeld, als u deze parameter instellen op drie (3), het antwoord bevat de top drie gerangschikte antwoorden.  

- Toegevoegd de [bevorderen](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#promote) queryparameter. Gebruik deze parameter samen met `answerCount` om op te nemen expliciet een of meer antwoord typen, ongeacht hun positie. Bijvoorbeeld, als u wilt promoveren video's en afbeeldingen in het antwoord, stelt u verhogen naar *video's, afbeeldingen*. De lijst met antwoorden die u wilt promoveren komt niet in mindering gebracht de `answerCount` limiet. Bijvoorbeeld, als `answerCount` 2 en `promote` is ingesteld op *video's, afbeeldingen*, het antwoord advies inwinnen webpagina's, nieuws, video's en afbeeldingen.

### <a name="object-changes"></a>Wijzigingen van object

- Toegevoegd de `someResultsRemoved` veld de [WebAnswer](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#webanswer) object. Het veld bevat een Booleaanse waarde die aangeeft of het antwoord bepaalde resultaten van de web-antwoord uitgesloten.  
