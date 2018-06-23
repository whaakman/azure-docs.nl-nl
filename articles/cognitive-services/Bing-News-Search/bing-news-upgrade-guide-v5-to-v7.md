---
title: Bing nieuws zoekactie API bijwerken v5 naar v7 | Microsoft Docs
description: Identificeert de onderdelen van uw toepassing die u nodig hebt om bij te werken als versie 7 wilt gebruiken.
services: cognitive-services
author: swhite-msft
manager: ehansen
ms.assetid: 5334C475-4841-4736-A66E-DC1E07CBCEC9
ms.service: cognitive-services
ms.component: bing-news-search
ms.topic: article
ms.date: 04/15/2017
ms.author: scottwhi
ms.openlocfilehash: baed6f0091ddad40b4802c0fb52dc2ca1818cd03
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344534"
---
# <a name="news-search-api-upgrade-guide"></a>De Upgradehandleiding van nieuws-API van zoekservice

Deze upgrade handleiding identificeert de wijzigingen tussen 5 en versie 7 van de API van Bing nieuws zoeken. Gebruik deze handleiding kunt u de onderdelen van uw toepassing die u nodig hebt om bij te werken voor het gebruik van versie 7 herkennen.

## <a name="breaking-changes"></a>Wijzigingen die fouten veroorzaken

### <a name="endpoints"></a>Eindpunten

- Het versienummer van het eindpunt is gewijzigd van v5 in v7. Bijvoorbeeld: https://api.cognitive.microsoft.com/bing/\ * \*v7.0**/news/search.

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

### <a name="object-changes"></a>Wijzigingen van object

- Toegevoegd de `contractualRules` veld de [NewsArticle](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#newsarticle) object. De `contractualRules` veld bevat een lijst met regels die u (bijvoorbeeld artikel toekenning volgen moet). U moet het kenmerk dat is opgegeven in de toepassen `contractualRules` in plaats van `provider`. Dit artikel bevat `contractualRules` alleen wanneer de [Web Search API](../bing-web-search/search-the-web.md) antwoord bevat een nieuwsantwoord.

## <a name="non-breaking-changes"></a>De wijzigingen niet verbreken

### <a name="query-parameters"></a>Queryparameters

- Producten toegevoegd als een mogelijke waarde die u mogelijk de [categorie](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#category) -parameter voor een query. Zie [categorieën door markten](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#categories-by-market).  
    
- Toegevoegd de [SortBy](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#sortby) queryparameter trends onderwerpen gesorteerd op basis van datum met de meest recente eerste geretourneerd.  
  
- Toegevoegd de [omdat](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#since) queryparameter trends onderwerpen die zijn gedetecteerd door Bing op of na de opgegeven Unix-epoche tijdstempel geretourneerd.

### <a name="object-changes"></a>Wijzigingen van object

- Toegevoegd de `mentions` veld de [NewsArticle](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#newsarticle) object. De `mentions` veld bevat een lijst van entiteiten (personen of plaatsen) die zijn gevonden in het artikel.  
  
- Toegevoegd de `video` veld de [NewsArticle](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#newsarticle) object. De `video` veld bevat een video die gerelateerd is aan het nieuwsartikel. De video is een \<iframe\> die u kunt insluiten of een miniatuur beweging.   
  
- Toegevoegd de `sort` veld de [nieuws](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#news) object. De `sort` veld bevat de sorteervolgorde van de artikelen. Bijvoorbeeld, worden de artikelen gesorteerd op relevantie (standaard) of datum.  
  
- Toegevoegd de [SortValue](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#sortvalue) -object, dat een sorteervolgorde definieert. De `isSelected` veld geeft aan of het antwoord de sorteervolgorde gebruikt. Als **true**, het antwoord gebruikt de sorteervolgorde. Als `isSelected` is **false**, kunt u de URL in de `url` veld om aan te vragen van een andere sorteervolgorde.
