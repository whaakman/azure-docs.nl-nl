---
title: Categorieën voor de Bing API voor zoeken van lokale bedrijven zoeken | Microsoft Docs
titleSuffix: Azure Cognitive Services
description: Gebruik dit artikel voor informatie over het opgeven van de zoekcategorieën voor de lokale bedrijven Bing zoeken-API-eindpunt.
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-local-business
ms.topic: article
ms.date: 11/01/2018
ms.author: rosh, v-gedod
ms.openlocfilehash: aec45e94e2cbec9093cdb72c896dabc633f118b2
ms.sourcegitcommit: 6678e16c4b273acd3eaf45af310de77090137fa1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/01/2018
ms.locfileid: "50748652"
---
# <a name="search-categories-for-the-bing-local-business-search-api"></a>Zoekcategorieën voor de Bing API voor zoeken van lokale bedrijven

De Bing lokale bedrijven zoeken-API kunt u zoeken naar lokale zakelijke entiteiten in verschillende categorieën met prioriteit gegeven aan sluit resultaten van de locatie van een gebruiker. U kunt deze zoekbewerkingen opnemen in zoekopdrachten samen met de `localCircularView` en `localMapView` [parameters](specify-geographic-search.md).


## <a name="toplevel-categories"></a>TopLevel categorieën 

De volgende typen definiëren categorieën met zoeken.  Meer dan één categorie kan worden opgegeven met behulp van een door komma's gescheiden lijst die wordt toegewezen aan de `localCategories` parameter.  
- EatDrink 
- SeeDo 
- Winkelen 
- HotelsAndMotels 
- BanksAndCreditUnions 
- De vervangende domeinpagina 
- Ziekenhuizen 

## <a name="sub-categories"></a>Subcategorieën
Subcategorieën worden doorgegeven dezelfde manier als `localCategories`. Subcategorieën zijn specifiekere categorieën. Ze zijn onderliggende in de zin dat als u een categorie C en een van de subcategorieën S in de dezelfde lijst met door komma's gescheiden opgeeft, u dezelfde resultaten ontvangt als u alleen C hebt opgegeven.

### <a name="eat-drink"></a>Eten drank 
|  |  |  |  |
| - | - | - | - |
| BreweriesAndBrewPubs | CocktailLounges | AfricanRestaurants |
| AmericanRestaurants | Bagels | BarbecueRestaurants |
| Taverns | SportsBars | Balken |
| BarsGrillsAndPubs | BuffetRestaurants| BelgianRestaurants | 
| BritishRestaurants | CafeRestaurants | CaribbeanRestaurants |
| ChineseRestaurants | CoffeeAndTea | Delicatessens | 
| DeliveryService | Diners | DiscountStores | 
| Donuts | FastFood | FrenchRestaurants | 
| FrozenYogurt | GermanRestaurants | Supermarkten | 
| GreekRestaurants | Grocers | HawaiianRestaurants | 
| HungarianRestaurants | IceCreamAndFrozenDesserts | IndianRestaurants | 
| ItalianRestaurants | JapaneseRestaurants | SAP | 
| KoreanRestaurants | LiquorStores | MexicanRestaurants |
| MiddleEasternRestaurants | pizza | PolishRestaurants | 
| PortugueseRestaurants | Pretzels | Restaurants | 
| RussianAndUkrainianRestaurants | Broodjes | SeafoodRestaurants | 
| SpanishRestaurants | SteakHouseRestaurants | SushiRestaurants | 
| Punt | ThaiRestaurants | TurkishRestaurants | 
| VegetarianAndVeganRestaurants | VietnameseRestaurants|  |
 
### <a name="see-do"></a>Zie doen 
|  |  |  |
| -- | -- | -- |
| AmusementParks | Bezienswaardigheden | Carnivals |
| Casino 's | LandmarksAndHistoricalSites | MiniatureGolfCourses |
| MovieTheaters | Musea | Parken |
| SightseeingTours | TouristInformation | Dierentuinen |
 
### <a name="shop"></a>Winkelen 
|  |  |  |
| -- | -- | -- |
| AntiqueStores | Uw boekwinkel | CDAndRecordStores |
| ChildrensClothingStores | CigarAndTobaccoShops | ComicBookStores |
| DepartmentStores | DiscountStores | FleaMarketsAndBazaars |
| FurnitureStores | HomeImprovementStores | JewelryAndWatchesStores |
| KitchenwareStores | LiquorStores | MallsAndShoppingCenters |
| MensClothingStores | MusicStores | OutletStores |
| PetShops | PetSupplyStores | SchoolAndOfficeSupplyStores |
| ShoeStores | SportingGoodsStores | ToyAndGameStores |
| VitaminAndSupplementStores | WomensClothingStores |  |


## <a name="examples-of-local-categories-search"></a>Voorbeelden van lokale categorieën zoeken

De volgende voorbeelden GET resulteert volgens de `localCategories` parameter:

`https://api.cognitive.microsoft.com/localbusinesses/v7.0/search?&q=&mkt=en-US&localcategories=HotelsAndMotels`

`https://api.cognitive.microsoft.com/localbusinesses/v7.0/search?&q=&mkt=en-US&localcategories=EatDrink`

`https://api.cognitive.microsoft.com/localbusinesses/v7.0/search?&q=&mkt=en-US&localcategories=Shop`

`https://api.cognitive.microsoft.com/localbusinesses/v7.0/search?&q=&mkt=en-US&localcategories=Hospitals`

Het aantal 'ziekenhuis' resultaten in de eerste drie geretourneerd van de Bing API voor zoeken van lokale bedrijven wordt beperkt door de volgende query uit:

`https://api.cognitive.microsoft.com/localbusinesses/v7.0/search?&q=&mkt=en-US&localCategories=Hospitals&count=3&offset=0`

Het volgende voorbeeld-JSON-antwoord bevat drie ziekenhuizen in het gebied met groter Seattle:

````json
BingAPIs-TraceId: 68AFB51807C6485CAB8AAF20E232EFFF
BingAPIs-SessionId: F89E7B8539B34BF58AAF811485E83B20
X-MSEdge-ClientID: 1C44E64DBFAA6BCA1270EADDBE7D6A22
BingAPIs-Market: en-US
X-MSEdge-Ref: Ref A: 68AFB51807C6485CAB8AAF20E232EFFF Ref B: CO1EDGE0108 Ref C: 2018-10-22T18:52:28Z

{
   "_type": "SearchResponse",
   "queryContext": {
      "originalQuery": ""
   },
   "places": {
      "readLink": "https:\/\/www.bingapis.com\/api\/v7\/places\/search?q=",
      "totalEstimatedMatches": 0,
      "value": [
         {
            "_type": "LocalBusiness",
            "id": "https:\/\/www.bingapis.com\/api\/v7\/#Places.0",
            "name": "Overlake Hospital Medical Center",
            "url": "http:\/\/www.overlakehospital.org\/",
            "entityPresentationInfo": {
               "entityScenario": "ListItem",
               "entityTypeHints": [
                  "Place",
                  "LocalBusiness"
               ]
            },
            "geo": {
               "latitude": 47.6204986572266,
               "longitude": -122.185829162598
            },
            "routablePoint": {
               "latitude": 47.6204986572266,
               "longitude": -122.185668945312
            },
            "address": {
               "streetAddress": "1035 116th Ave NE",
               "addressLocality": "Bellevue",
               "addressRegion": "WA",
               "postalCode": "98004",
               "addressCountry": "US",
               "neighborhood": "",
               "text": "1035 116th Ave NE, Bellevue, WA, 98004"
            },
            "telephone": "(425) 688-5000"
         },
         {
            "_type": "LocalBusiness",
            "id": "https:\/\/www.bingapis.com\/api\/v7\/#Places.1",
            "name": "Virginia Mason University Village Medical Center",
            "url": "https:\/\/virginiamason.org\/Seattle",
            "entityPresentationInfo": {
               "entityScenario": "ListItem",
               "entityTypeHints": [
                  "Place",
                  "LocalBusiness"
               ]
            },
            "geo": {
               "latitude": 47.6095390319824,
               "longitude": -122.327941894531
            },
            "routablePoint": {
               "latitude": 47.6093978881836,
               "longitude": -122.328277587891
            },
            "address": {
               "streetAddress": "1100 9th Ave",
               "addressLocality": "Seattle",
               "addressRegion": "WA",
               "postalCode": "98101",
               "addressCountry": "US",
               "neighborhood": "University District",
               "text": "1100 9th Ave, Seattle, WA, 98101"
            },
            "telephone": "(206) 223-6600"
         },
         {
            "_type": "LocalBusiness",
            "id": "https:\/\/www.bingapis.com\/api\/v7\/#Places.2",
            "name": "Seattle Children’s Hospital",
            "url": "http:\/\/www.seattlechildrens.org\/",
            "entityPresentationInfo": {
               "entityScenario": "ListItem",
               "entityTypeHints": [
                  "Place",
                  "LocalBusiness"
               ]
            },
            "geo": {
               "latitude": 47.6625061035156,
               "longitude": -122.283760070801
            },
            "routablePoint": {
               "latitude": 47.6631507873535,
               "longitude": -122.284614562988
            },
            "address": {
               "streetAddress": "4800 Sand Point Way NE",
               "addressLocality": "Seattle",
               "addressRegion": "WA",
               "postalCode": "98105",
               "addressCountry": "US",
               "neighborhood": "Laurelhurst",
               "text": "4800 Sand Point Way NE, Seattle, WA, 98105"
            },
            "telephone": "(206) 987-2000"
         }
      ],
      "searchAction": {

      }
   }
}
````

## <a name="next-steps"></a>Volgende stappen
- [Zoeken in geografische grenzen](specify-geographic-search.md)
- [Query's en antwoorden](local-search-query-response.md)
- [Quick Start inC#](quickstarts/local-quickstart.md)