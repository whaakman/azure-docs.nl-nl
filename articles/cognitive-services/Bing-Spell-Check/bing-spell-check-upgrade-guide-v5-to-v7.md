---
title: Upgrade van de Bing Spell Check-API versie 5 naar versie 7
titlesuffix: Azure Cognitive Services
description: Hiermee geeft u de onderdelen van uw toepassing die u nodig hebt om bij te werken voor het gebruik van versie 7.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-spell-check
ms.topic: conceptual
ms.date: 06/21/2016
ms.author: scottwhi
ms.openlocfilehash: 402fabc41908afc35396064ef4e518fc3c231052
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55866352"
---
# <a name="spell-check-api-upgrade-guide"></a>Handleiding voor van Spell Check-API bijwerken

Deze upgrade handleiding identificeert de wijzigingen tussen 5 en versie 7 van de Bing Spell Check-API. Deze handleiding gebruiken om te bepalen van de onderdelen van uw toepassing die u nodig hebt om bij te werken voor het gebruik van versie 7.

## <a name="breaking-changes"></a>Wijzigingen die fouten veroorzaken

### <a name="endpoints"></a>Eindpunten

- Het versienummer van het eindpunt is gewijzigd van versie 5 in versie 7. Bijvoorbeeld `https://api.cognitive.microsoft.com/bing/v7.0/spellcheck`.

### <a name="error-response-objects-and-error-codes"></a>Fout antwoordobjecten en foutcodes

- Alle mislukte aanvragen moeten nu bevatten een `ErrorResponse` object in de hoofdtekst van het antwoord.

- De volgende velden toevoegt aan de `Error` object.  
  - `subCode`&mdash;Indien mogelijk partities van de foutcode in discrete buckets
  - `moreDetails`&mdash;Als u meer informatie over de fout wordt beschreven in de `message` veld
   

- De foutcodes v5 vervangen door de volgende mogelijke `code` en `subCode` waarden.  
  
|Code|SubCode|Description
|-|-|-
|ServerError|UnexpectedError<br/>ResourceError<br/>Niet geïmplementeerd|Bing retourneert ServerError wanneer een van de subcode-voorwaarden optreden. Het antwoord bevat deze fouten als de HTTP-statuscode 500.
|InvalidRequest|ParameterMissing<br/>ParameterInvalidValue<br/>HttpNotAllowed<br/>Geblokkeerd|Bing retourneert InvalidRequest wanneer er een deel van de aanvraag is niet geldig. Bijvoorbeeld, een vereiste parameter ontbreekt of een parameterwaarde is niet geldig.<br/><br/>Als de fout ParameterMissing of ParameterInvalidValue is, is de HTTP-statuscode 400.<br/><br/>Als de fout HttpNotAllowed, de HTTP-statuscode 410 is.
|RateLimitExceeded||Bing retourneert RateLimitExceeded wanneer u uw query's per seconde (QPS) of query's per maand (QPM) quotum overschrijdt.<br/><br/>Bing retourneert HTTP-statuscode 429 als u overschreden QPS en 403 als u QPM overschreden.
|InvalidAuthorization|AuthorizationMissing<br/>AuthorizationRedundancy|Bing retourneert InvalidAuthorization wanneer Bing de oproepende functie kan niet worden geverifieerd. Bijvoorbeeld, de `Ocp-Apim-Subscription-Key` koptekst ontbreekt of de abonnementssleutel is niet geldig.<br/><br/>Redundantie treedt op als u meer dan één verificatiemethode opgeven.<br/><br/>Als de fout InvalidAuthorization is, is de HTTP-statuscode 401.
|InsufficientAuthorization|AuthorizationDisabled<br/>AuthorizationExpired|Bing retourneert InsufficientAuthorization wanneer de aanroeper beschikt niet over machtigingen voor toegang tot de resource. Dit kan gebeuren als de abonnementssleutel is uitgeschakeld of is verlopen. <br/><br/>Als de fout InsufficientAuthorization is, is de HTTP-statuscode 403.

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

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Vereisten voor gebruik en weergave](./UseAndDisplayRequirements.md)
