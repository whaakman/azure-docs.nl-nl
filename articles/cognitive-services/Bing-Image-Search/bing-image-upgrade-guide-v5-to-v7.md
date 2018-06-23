---
title: Upgrade van Bing installatiekopie Search API v5 naar v7 | Microsoft Docs
description: Identificeert de onderdelen van uw toepassing die u nodig hebt om bij te werken als versie 7 wilt gebruiken.
services: cognitive-services
author: swhite-msft
manager: ehansen
ms.assetid: 7F78B91F-F13B-40A4-B8A7-770FDB793F0F
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: article
ms.date: 04/15/2017
ms.author: scottwhi
ms.openlocfilehash: b4d785eafe9ced6fb12e2dac3282dfd286849eb6
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344556"
---
# <a name="image-search-api-upgrade-guide"></a>Installatiekopie-API van zoekservice upgrade guide

Deze upgrade handleiding identificeert de wijzigingen tussen 5 en versie 7 van de API van Bing installatiekopie zoeken. Gebruik deze handleiding kunt u de onderdelen van uw toepassing die u nodig hebt om bij te werken voor het gebruik van versie 7 herkennen.

## <a name="breaking-changes"></a>Wijzigingen die fouten veroorzaken

### <a name="endpoints"></a>Eindpunten

- Het versienummer van het eindpunt is gewijzigd van v5 in v7. Bijvoorbeeld: https://api.cognitive.microsoft.com/bing/\ * \*v7.0**/images/search.

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

- Hernoemd de `modulesRequested` queryparameter naar [modules](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#modules).  
  
- Naam van de aantekeningen aan Tags gewijzigd. Zie [modules](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#modules) queryparameter aan Tags.  

- De lijst met ondersteunde markten van de filterwaarde ShoppingSources en-us alleen gewijzigd. Zie [imageType](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#imagetype).  
 

### <a name="image-insights-changes"></a>Afbeelding insights wijzigingen
  
- Hernoemd de `annotations` veld [ImagesInsights](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#imageinsightsresponse) naar `imageTags`.  
  
- Hernoemd de `AnnotationModule` object naar de [ImageTagsModule](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#imagetagsmodule).  
  
- Hernoemd de `Annotation` object naar de [Tag](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#tag), en verwijderd de `confidence` veld.  
  
- Hernoemd de `insightsSourcesSummary` veld van de [installatiekopie](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#image) object naar de `insightsMetadata`.  
  
- Hernoemd de `InsightsSourcesSummary` object naar de [InsightsMetadata](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#insightsmetadata).  
  
- Toegevoegd de `https://api.cognitive.microsoft.com/bing/v7.0/images/details` eindpunt. Dit eindpunt naar aanvraag installatiekopie inzicht gebruiken in plaats van het eindpunt installatiekopieën/zoeken. Zie [installatiekopie Insights](./image-insights.md). 
  
- De volgende queryparameters zijn nu alleen geldig voor de `/images/details` eindpunt.  
  
    -   [insightsToken](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#insightstoken)  
    -   [modules](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#modules)  
    -   [imgUrl](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#imgurl)  
    -   [CAB-bestand](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#cab)  
    -   [CAL](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#cal)  
    -   [Auto](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#car)  
    -   [CAT](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#cat)  
    -   [CT](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#ct)  
  
- Hernoemd de `ImageInsightsResponse` object naar de [ImageInsights](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#imageinsights).  
  
- Gewijzigd van de gegevenstypen van de volgende velden in de [ImageInsights](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#imageinsights) object.  
  
    -   Het type gewijzigd van de `relatedCollections` veld `ImageGallery[]` naar [RelatedCollectionsModule](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#relatedcollectionsmodule).  
  
    -   Het type gewijzigd van de `pagesIncluding` veld `Image[]` naar [ImagesModule](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#imagesmodule).  
  
    -   Het type gewijzigd van de `relatedSearches` veld `Query[]` naar [RelatedSearchesModule](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#relatedsearchesmodule).  
  
    -   Het type gewijzigd van de `recipes` veld `Recipe[]` naar [RecipesModule](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#recipesmodule).  
  
    -   Het type gewijzigd van de `visuallySimilarImages` veld `Image[]` naar [ImagesModule](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#imagesmodule).  
  
    -   Het type gewijzigd van de `visuallySimilarProducts` veld `ProductSummaryImage[]` naar [ImagesModule](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#imagesmodule).  
  
    -   Verwijderd de `ProductSummaryImage` object en verplaatst van de product-gerelateerde velden in de [installatiekopie](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#image) object. De `Image` -object bevat de product-gerelateerde velden alleen wanneer de installatiekopie die opgenomen als onderdeel van visueel vergelijkbare producten die in een afbeelding inzicht antwoord is.  
  
    -   Het type gewijzigd van de `recognizedEntityGroups` veld `RecognizedEntityGroup[]` naar [RecognizedEntitiesModule](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#recognizedentitiesmodule).  
  
-   Hernoemd de `categoryClassification` veld [ImageInsights](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#imageinsightsresponse) naar `annotations`, en het type aan gewijzigd `AnnotationsModule`.  

### <a name="images-answer"></a>Installatiekopieën van het antwoord

-   De velden displayShoppingSourcesBadges en displayRecipeSourcesBadges uit verwijderd [installatiekopieën](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#images).  
  
-   Hernoemd de `nextOffsetAddCount` veld [installatiekopieën](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#images) naar `nextOffset`. De manier waarop u de offset is ook gewijzigd. Eerder, stelt u de [offset](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#offset) query-parameter voor de `nextOffsetAddCount` waarde plus de vorige waarde van de offset en het aantal afbeeldingen in het resultaat. Nu u instellen `offset` naar de `nextOffset` waarde.  
    
  
## <a name="non-breaking-changes"></a>Niet-grote wijzigingen

### <a name="query-parameters"></a>Queryparameters
  
- Transparant toegevoegd als een mogelijk [imageType](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#imagetype) filterwaarde. Het transparante filter retourneert alleen de afbeeldingen met een transparante achtergrond.

- Toegevoegd als een mogelijk een [licentie](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#license) filterwaarde. Een filter dat de retourneert alleen de afbeeldingen die onder licentie zijn.
  
- Toegevoegd de [maxFileSize](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#maxfilesize) en [minFileSize](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#minfilesize) queryparameters. Deze filters gebruiken om terug te keren installatiekopieën binnen een bereik van de bestandsgrootte.  
  
- Toegevoegd de [maxHeight](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#maxheight), [minHeight](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#minheight), [maxWidth](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#maxwidth), [minWidth](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#minwidth) queryparameters. Deze filters gebruiken om terug te keren installatiekopieën binnen een bereik van de hoogte en breedte.  

### <a name="object-changes"></a>Wijzigingen van object
  
- Toegevoegd de `description` en `lastUpdated` velden naar de [bieden](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#offer) object.  
  
- Toegevoegd de `name` veld de [ImageGallery](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#imagegallery) object.  
  
- Toegevoegd `similarTerms` naar de [installatiekopieën](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#images) object. Dit veld bevat een lijst met termen met dezelfde betekenis voor de queryreeks van de gebruiker.  
