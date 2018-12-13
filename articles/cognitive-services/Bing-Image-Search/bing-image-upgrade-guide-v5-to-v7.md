---
title: Een upgrade uitvoeren voor Bing afbeeldingen zoeken-API versie 5 naar versie 7
titleSuffix: Azure Cognitive Services
description: Deze upgrade handleiding beschrijft de wijzigingen tussen 5 en versie 7 van de Bing afbeeldingen zoeken-API. Deze handleiding gebruiken om te bepalen van de onderdelen van uw toepassing die u nodig hebt om bij te werken voor het gebruik van versie 7.
services: cognitive-services
author: swhite-msft
manager: cgronlun
ms.assetid: 7F78B91F-F13B-40A4-B8A7-770FDB793F0F
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: article
ms.date: 04/15/2017
ms.author: scottwhi
ms.openlocfilehash: 2192e3d2a344a31717c59c813123ed0c807a2841
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2018
ms.locfileid: "53256550"
---
# <a name="bing-image-search-api-upgrade-guide"></a>Bing afbeeldingen zoeken-API Upgradehandleiding

Deze upgrade handleiding identificeert de wijzigingen tussen 5 en versie 7 van de Bing afbeeldingen zoeken-API. Deze handleiding gebruiken om te bepalen van de onderdelen van uw toepassing die u nodig hebt om bij te werken voor het gebruik van versie 7.

## <a name="breaking-changes"></a>Wijzigingen die fouten veroorzaken

### <a name="endpoints"></a>Eindpunten

- Het versienummer van het eindpunt is gewijzigd van versie 5 in versie 7. Bijvoorbeeld, https://api.cognitive.microsoft.com/bing/\ * \*v7.0**/images/search.

### <a name="error-response-objects-and-error-codes"></a>Fout antwoordobjecten en foutcodes

- Alle mislukte aanvragen moeten nu bevatten een `ErrorResponse` object in de hoofdtekst van het antwoord.

- De volgende velden toevoegt aan de `Error` object.  
  - `subCode`&mdash;Indien mogelijk partities van de foutcode in discrete buckets
  - `moreDetails`&mdash;Als u meer informatie over de fout wordt beschreven in de `message` veld


- De foutcodes v5 vervangen door de volgende mogelijke `code` en `subCode` waarden.

|Code|De subCode|Description
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



### <a name="query-parameters"></a>Queryparameters

- De naam van gewijzigd de `modulesRequested` parameter query [modules](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#modules).  

- De aantekeningen gewijzigd in Tags. Zie [modules](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#modules) queryparameter aan labels.  

- Gewijzigd in de lijst met ondersteunde markten van de waarde voor het filter ShoppingSources alleen en-US. Zie [imageType](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#imagetype).  


### <a name="image-insights-changes"></a>Afbeelding insights wijzigingen

- De naam van gewijzigd de `annotations` veld [ImagesInsights](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#imageinsightsresponse) naar `imageTags`.  

- De naam van gewijzigd de `AnnotationModule` object [ImageTagsModule](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#imagetagsmodule).  

- De naam van gewijzigd de `Annotation` object naar [Tag](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#tag), en verwijderd de `confidence` veld.  

- De naam van gewijzigd de `insightsSourcesSummary` veld van de [installatiekopie](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#image) object toe aan `insightsMetadata`.  

- De naam van gewijzigd de `InsightsSourcesSummary` object [InsightsMetadata](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#insightsmetadata).  

- Toegevoegd de `https://api.cognitive.microsoft.com/bing/v7.0/images/details` eindpunt. Dit eindpunt om aanvraag inzichten in afbeeldingen te gebruiken in plaats van het eindpunt/afbeeldingen/zoeken. Zie [Image Insights](./image-insights.md).

- De volgende queryparameters zijn nu alleen geldig voor de `/images/details` eindpunt.  

    -   [insightsToken](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#insightstoken)  
    -   [Modules](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#modules)  
    -   [imgUrl](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#imgurl)  
    -   [CAB-bestand](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#cab)  
    -   [CAL](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#cal)  
    -   [auto 's](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#car)  
    -   [CAT](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#cat)  
    -   [CT](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#ct)  

- De naam van gewijzigd de `ImageInsightsResponse` object [ImageInsights](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#imageinsights).  

- Gewijzigd van de gegevenstypen van de volgende velden in de [ImageInsights](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#imageinsights) object.  

    -   Het type gewijzigd van de `relatedCollections` veld `ImageGallery[]` naar [RelatedCollectionsModule](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#relatedcollectionsmodule).  

    -   Het type gewijzigd van de `pagesIncluding` veld `Image[]` naar [ImagesModule](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#imagesmodule).  

    -   Het type gewijzigd van de `relatedSearches` veld `Query[]` naar [RelatedSearchesModule](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#relatedsearchesmodule).  

    -   Het type gewijzigd van de `recipes` veld `Recipe[]` naar [RecipesModule](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#recipesmodule).  

    -   Het type gewijzigd van de `visuallySimilarImages` veld `Image[]` naar [ImagesModule](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#imagesmodule).  

    -   Het type gewijzigd van de `visuallySimilarProducts` veld `ProductSummaryImage[]` naar [ImagesModule](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#imagesmodule).  

    -   Verwijderd de `ProductSummaryImage` -object en wordt verplaatst van de product-gerelateerde velden in de [installatiekopie](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#image) object. De `Image` object bevat de product-gerelateerde velden alleen wanneer de installatiekopie die opgenomen als onderdeel van visueel vergelijkbare producten in een afbeelding insight-antwoord is.  

    -   Het type gewijzigd van de `recognizedEntityGroups` veld `RecognizedEntityGroup[]` naar [RecognizedEntitiesModule](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#recognizedentitiesmodule).  

-   Gewijzigd de `categoryClassification` veld [ImageInsights](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#imageinsightsresponse) naar `annotations`, en het type gewijzigd `AnnotationsModule`.  

### <a name="images-answer"></a>Installatiekopieën van antwoord

-   De velden displayShoppingSourcesBadges en displayRecipeSourcesBadges uit verwijderd [installatiekopieën](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#images).  

-   De naam van gewijzigd de `nextOffsetAddCount` veld [installatiekopieën](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#images) naar `nextOffset`. De manier waarop u de offset is ook gewijzigd. Eerder, stelt u de [offset](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#offset) queryparameter naar de `nextOffsetAddCount` waarde plus de vorige waarde intervalverschuiving en het aantal afbeeldingen in het resultaat. Stel nu `offset` naar de `nextOffset` waarde.  


## <a name="non-breaking-changes"></a>Niet-belangrijke wijzigingen

### <a name="query-parameters"></a>Queryparameters

- Transparant toegevoegd als een mogelijke [imageType](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#imagetype) filterwaarde. De transparante filter retourneert alleen afbeeldingen met een transparante achtergrond.

- Toegevoegd een een mogelijk [licentie](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#license) filterwaarde. De een filter retourneert alleen installatiekopieën die onder licentie.

- Toegevoegd de [maxFileSize](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#maxfilesize) en [minFileSize](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#minfilesize) queryparameters. Deze filters gebruiken om terug te keren installatiekopieën binnen een bereik van de bestandsgrootte.  

- Toegevoegd de [maxHeight](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#maxheight), [minHeight](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#minheight), [maxWidth](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#maxwidth), [minWidth](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#minwidth) queryparameters. Deze filters gebruiken om terug te keren installatiekopieën binnen een bereik van de hoogte en breedte.  

### <a name="object-changes"></a>Wijzigingen van object

- Toegevoegd de `description` en `lastUpdated` velden naar de [bieden](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#offer) object.  

- Toegevoegd de `name` veld de [ImageGallery](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#imagegallery) object.  

- Toegevoegd `similarTerms` naar de [installatiekopieën](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#images) object. Dit veld bevat een lijst met voorwaarden die vergelijkbaar in betekenis om query-tekenreeks van de gebruiker zijn.  
