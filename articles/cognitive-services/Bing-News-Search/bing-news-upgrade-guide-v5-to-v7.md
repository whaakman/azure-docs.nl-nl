---
title: Upgrade van Bing nieuws zoeken-API versie 5 naar versie 7
titlesuffix: Azure Cognitive Services
description: Hiermee geeft u de onderdelen van uw toepassing die u nodig hebt om bij te werken voor het gebruik van versie 7.
services: cognitive-services
author: swhite-msft
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-news-search
ms.topic: conceptual
ms.date: 04/15/2017
ms.author: scottwhi
ms.openlocfilehash: c6ecb7d4c1e5b648373fcaa3f44c6294329d33c2
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/05/2018
ms.locfileid: "48801162"
---
# <a name="news-search-api-upgrade-guide"></a>Upgradehandleiding nieuws zoeken-API

Deze upgrade handleiding identificeert de wijzigingen tussen 5 en versie 7 van de Bing nieuws zoeken-API. Deze handleiding gebruiken om te bepalen van de onderdelen van uw toepassing die u nodig hebt om bij te werken voor het gebruik van versie 7.

## <a name="breaking-changes"></a>Wijzigingen die fouten veroorzaken

### <a name="endpoints"></a>Eindpunten

- Het versienummer van het eindpunt is gewijzigd van versie 5 in versie 7. Bijvoorbeeld, https://api.cognitive.microsoft.com/bing/\ * \*v7.0**/news/search.

### <a name="error-response-objects-and-error-codes"></a>Fout antwoordobjecten en foutcodes

- Alle mislukte aanvragen moeten nu bevatten een `ErrorResponse` object in de hoofdtekst van het antwoord.

- De volgende velden toevoegt aan de `Error` object.  
  - `subCode`&mdash;Indien mogelijk partities van de foutcode in discrete buckets
  - `moreDetails`&mdash;Als u meer informatie over de fout wordt beschreven in de `message` veld
   

- De foutcodes v5 vervangen door de volgende mogelijke `code` en `subCode` waarden.

|Code|De subCode|Beschrijving
|-|-|-
|ServerError|UnexpectedError<br/>ResourceError<br/>Niet geïmplementeerd|Bing retourneert ServerError wanneer een van de voorwaarden van de onderliggende code optreden. Het antwoord bevat deze fouten als de HTTP-statuscode 500.
|InvalidRequest|ParameterMissing<br/>ParameterInvalidValue<br/>HttpNotAllowed<br/>Geblokkeerd|Bing retourneert InvalidRequest wanneer er een deel van de aanvraag niet geldig is. Bijvoorbeeld, een vereiste parameter ontbreekt of een parameterwaarde is niet geldig.<br/><br/>Als de fout ParameterMissing of ParameterInvalidValue is, is de HTTP-statuscode 400.<br/><br/>Als de fout HttpNotAllowed, de HTTP-statuscode 410 is.
|RateLimitExceeded||Bing retourneert RateLimitExceeded wanneer u uw query's per seconde (QPS) of query's per maand (QPM) quotum overschrijdt.<br/><br/>Bing retourneert HTTP-statuscode 429 als u overschreden QPS en 403 als u QPM overschreden.
|InvalidAuthorization|AuthorizationMissing<br/>AuthorizationRedundancy|Bing retourneert InvalidAuthorization wanneer Bing de oproepende functie kan niet worden geverifieerd. Bijvoorbeeld, de `Ocp-Apim-Subscription-Key` koptekst ontbreekt of de abonnementssleutel is niet geldig.<br/><br/>Redundantie treedt op als u meer dan één verificatiemethode opgeven.<br/><br/>Als de fout InvalidAuthorization is, is de HTTP-statuscode 401.
|InsufficientAuthorization|AuthorizationDisabled<br/>AuthorizationExpired|Bing retourneert InsufficientAuthorization wanneer de oproepende functie heeft geen machtigingen voor toegang tot de resource. Dit kan gebeuren als de abonnementssleutel is uitgeschakeld of is verlopen. <br/><br/>Als de fout InsufficientAuthorization is, is de HTTP-statuscode 403.

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

### <a name="object-changes"></a>Wijzigingen van object

- Toegevoegd de `contractualRules` veld de [NewsArticle](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#newsarticle) object. De `contractualRules` veld bevat een lijst met regels die u (bijvoorbeeld artikel attribution volgen moet). U moet het kenmerk dat is opgegeven in toepassen `contractualRules` in plaats van `provider`. Dit artikel bevat een `contractualRules` alleen wanneer de [webzoekopdrachten-API](../bing-web-search/search-the-web.md) antwoord bevat een nieuwsantwoord.

## <a name="non-breaking-changes"></a>Vaste wijzigingen

### <a name="query-parameters"></a>Queryparameters

- Producten toegevoegd als een mogelijke waarde die u mogelijk de [categorie](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#category) parameter om op te vragen. Zie [categorieën op markten](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#categories-by-market).  
    
- Toegevoegd de [SortBy](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#sortby) queryparameter die actuele onderwerpen op met de meest recente eerst gesorteerd op basis van datum als resultaat.  
  
- Toegevoegd de [omdat](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#since) queryparameter, dat wordt geretourneerd van actuele onderwerpen die zijn gedetecteerd door Bing op of na de opgegeven Unix-epoche timestamp.

### <a name="object-changes"></a>Wijzigingen van object

- Toegevoegd de `mentions` veld de [NewsArticle](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#newsarticle) object. De `mentions` veld bevat een lijst met entiteiten (personen of locaties) die zijn gevonden in het artikel.  
  
- Toegevoegd de `video` veld de [NewsArticle](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#newsarticle) object. De `video` veld bevat een video met betrekking tot het nieuwsartikel. De video is een \<iframe\> die u kunt insluiten of een beweging miniatuur.   
  
- Toegevoegd de `sort` veld de [nieuws](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#news) object. De `sort` veld ziet u de sorteervolgorde van de artikelen. Bijvoorbeeld, worden de artikelen gesorteerd op relevantie (standaard) of de datum.  
  
- Toegevoegd de [SortValue](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#sortvalue) object, dat een sorteervolgorde bepaalt. De `isSelected` veld geeft aan of het antwoord de sorteervolgorde gebruikt. Als **waar**, het antwoord gebruikt de sorteervolgorde. Als `isSelected` is **false**, kunt u de URL in de `url` veld om aan te vragen van een andere sorteervolgorde.
