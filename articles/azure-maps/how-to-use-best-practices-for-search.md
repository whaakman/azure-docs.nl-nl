---
title: Hoe u kunt zoeken efficiënt gebruik van de service Azure Maps Search | Microsoft Docs
description: Meer informatie over het gebruik van aanbevolen procedures voor search met behulp van de Azure Maps Search-service
ms.author: v-musehg
ms.date: 04/08/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: f7a14e975a5ca3aee5588f55f43b28081c100074
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/18/2019
ms.locfileid: "59358141"
---
# <a name="best-practices-to-use-azure-maps-search-service"></a>Aanbevolen procedures voor het gebruik van Azure Maps Search-Service

Azure Maps [zoekservice](https://docs.microsoft.com/rest/api/maps/search) voorziet in API's met verschillende mogelijkheden, bijvoorbeeld van zoeken om te zoeken naar gegevens van de punt van belang (pijl) om een specifieke locatie. In dit artikel hebben we de aanbevolen procedures voor het aanroepen van gegevens via Azure Maps Search-Services. U leert het volgende:

* Query's om te retourneren van de relevante overeenkomsten
* Limiet voor zoekresultaten
* Informatie over het verschil tussen verschillende resultaattypen
* Lees de structuur van antwoord zoeken


## <a name="prerequisites"></a>Vereisten

Als u wilt dat alle aanroepen naar de Maps-service API's, moet u een Maps-account en een sleutel. Zie voor meer informatie over het maken van een account en het ophalen van een sleutel [over het beheren van uw Azure kaarten-account en sleutels](how-to-manage-account-keys.md).

> [!Tip]
> Om te vragen de search-service, kunt u de [Postman-app](https://www.getpostman.com/apps) bouwen REST-aanroepen of u een API-ontwikkelomgeving die u kunt gebruiken.


## <a name="best-practices-for-geocoding"></a>Aanbevolen procedures voor Geocodering

Wanneer u een volledige of gedeeltelijke-adres met behulp van Azure Maps Search-Service zoekt, gaat de zoekterm en retourneert de lengtegraad en breedtegraad coördinaten van het adres. Dit proces kan geocodering wordt genoemd. De mogelijkheid om geocode in een land/regio is afhankelijk van de gegevens onderweg dekking en de nauwkeurigheid van de geocodering van de service voor geocodering.

Zie [dekking voor geocodering](https://docs.microsoft.com/azure/azure-maps/geocoding-coverage) voor meer informatie over de mogelijkheden van Azure Maps geocodering per land/regio.

### <a name="limit-search-results"></a>Limiet voor zoekresultaten

   In deze sectie leert u hoe u Azure Maps search API's gebruiken om zoekresultaten te beperken. 

   > [!Note]
   > Niet alle search API's ondersteunen volledig onderstaande parameters

   **Geo-afwijking van zoekresultaten**

   Om geo-afwijking van de resultaten op het desbetreffende gebied voor uw gebruiker, moet u altijd toevoegen de maximaal mogelijke gedetailleerde locatie invoer. Als u wilt beperken de zoekresultaten, houd rekening met de volgende invoertypen toe te voegen:

   1. Stel de `countrySet` parameter, bijvoorbeeld "US, FR". Het standaardgedrag van de zoekopdracht is om te zoeken naar de hele wereld, mogelijk onnodige resultaten retourneren. Als uw query bevat geen `countrySet` parameter, de zoekopdracht kan onnauwkeurig resultaten worden geretourneerd. Zoek bijvoorbeeld naar voor een stad met de naam **Bellevue** worden resultaten geretourneerd uit de Verenigde Staten en Frankrijk, omdat er met de naam steden **Bellevue** in Frankrijk en in de Verenigde Staten.

   2. U kunt de `btmRight` en `topleft` parameters om in te stellen het selectiekader vak voor de zoekopdracht beperken tot een bepaald gebied op de kaart.

   3. Als u wilt van invloed zijn op het gebied van belang zijn voor de resultaten, kunt u de `lat`en `lon` coördineren van parameters en instellen van de radius van de zoekopdracht gebied met de `radius` parameter.


   **Fuzzy zoekopdrachten parameters**

   1. De `minFuzzyLevel` en `maxFuzzyLevel`, op te halen van relevante overeenkomsten, zelfs wanneer de queryparameters niet precies overeenkomen met de gewenste informatie. De meeste zoekopdrachten standaard `minFuzzyLevel=1` en `maxFuzzyLevel=2` om prestaties en onvoorspelbare resultaten te verminderen. Nemen een voorbeeld van een zoekterm "restrant", deze wordt vergeleken met 'restaurant' wanneer de `maxFuzzyLevel` is ingesteld op 2. De fuzzy standaardniveaus kunnen aan de hand van de behoeften van de aanvraag worden genegeerd. 

   2. U kunt ook opgeven de exacte set resultaattypen moeten worden geretourneerd met behulp van de `idxSet` parameter. Voor dit doel kunt u indienen met door komma's gescheiden lijst van indexen, de volgorde van items niet van belang. Hier volgen de ondersteunde indexen:

       * `Addr` - **Adresbereiken**: Voor sommige Streets (straten) zijn er adres punten die worden geïnterpoleerd vanaf het begin en einde van de straat; Deze punten worden weergegeven als de adresbereiken.
       * `Geo` - **Geographies**: Gebieden op een kaart die staan voor administratieve indeling van een land, dat wil zeggen, land, status, plaats.
       * `PAD` - **Adres Point**:  Punten op een kaart waar specifiek adres met een straatnaam en een nummer kan worden gevonden in een index, bijvoorbeeld Soquel Dr 2501. Het is de hoogste mate van nauwkeurigheid is beschikbaar voor adressen.  
       * `POI` - **Punten van belang**: Punten op een kaart die zijn het noemen waard aandacht en die van belang zijn.  [Search-adres ophalen](https://docs.microsoft.com/rest/api/maps/search/getsearchaddress) koeriersbedrijven niet als resultaat.  
       * `Str` - **Streets (straten)**: Weergave van Streets (straten) op de kaart.
       * `XStr` - **Cross-Streets (straten) / snijpunten**:  Weergave van koppelingen; de locaties waar twee Streets (straten) elkaar overlappen.


       **Voorbeelden van het gebruik**:

       * idxSet = POI (alleen search punten) 

       * idxSet PAD, adres = (search-adressen alleen PAD = punt adres, adres = adresbereik)

### <a name="reverse-geocode-and-geography-entity-type-filter"></a>Reverse-geocode en geography entiteit type filter

Bij het uitvoeren van een zoekopdracht omgekeerde geocode met [omgekeerde API voor Search-adres](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse), de service heeft de mogelijkheid om terug te keren veelhoeken voor de administratieve gebieden. Door op te geven van de parameter `entityType` in de aanvraag, beperkt u het zoeken naar Entiteitstypen opgegeven geografische locatie. Het resulterende antwoord bevat de geografische locatie-ID, evenals het entiteitstype is gekoppeld. Als u meer dan één entiteit opgeven, eindpunt retourneert de **kleinste entiteit beschikbaar**. Geretourneerd geometrie-ID kan worden gebruikt om op te halen van de geometrie van die Geografie via [ophalen veelhoek service](https://docs.microsoft.com/rest/api/maps/search/getsearchpolygon).

**Voorbeeld van een aanvraag:**

```HTTP
https://atlas.microsoft.com/search/address/reverse/json?api-version=1.0&subscription-key={subscription-key}&query=47.6394532,-122.1304551&language=en-US&entityType=Municipality
```

**Antwoord:**

```JSON
{
    "summary": {
        "queryTime": 8,
        "numResults": 1
    },
    "addresses": [
        {
            "address": {
                "routeNumbers": [],
                "countryCode": "US",
                "countrySubdivision": "WA",
                "countrySecondarySubdivision": "King",
                "countryTertiarySubdivision": "Seattle East",
                "municipality": "Redmond",
                "country": "United States",
                "countryCodeISO3": "USA",
                "freeformAddress": "Redmond, WA",
                "boundingBox": {
                    "northEast": "47.717105,-122.034537",
                    "southWest": "47.627016,-122.164998",
                    "entity": "position"
                },
                "countrySubdivisionName": "Washington"
            },
            "position": "47.639454,-122.130455",
            "dataSources": {
                "geometry": {
                    "id": "00005557-4100-3c00-0000-0000596ae571"
                }
            },
            "entityType": "Municipality"
        }
    ]
}
```

### <a name="search-results-language"></a>Resultaten zoektaal

De `language` parameter kunt u om in te stellen in welke taal search resultaten moeten worden geretourneerd. Als de taal is niet ingesteld in de aanvraag, standaard de meest voorkomende taal in het land/regio automatisch search-service. Wanneer gegevens in de opgegeven taal niet beschikbaar is, wordt ook de standaardtaal gebruikt. Zie [ondersteunde talen](https://docs.microsoft.com/azure/azure-maps/supported-languages) voor een lijst van ondersteunde talen met betrekking tot Azure kaarten-services per land/regio.


### <a name="predictive-mode-auto-suggest"></a>Voorspellende modus (Automatische suggesties)

Meer overeenkomende waarden voor gedeeltelijke query's, `typeHead` parameter moet worden ingesteld op 'true' zijn. De query wordt geïnterpreteerd als een gedeeltelijke invoer en de zoekopdracht wordt voorspeld modus invoeren. Anders wordt wordt de service uitgegaan van dat alle relevante informatie is doorgegeven.

In het voorbeeld query hieronder u ziet dat de search service voor adres voor "Microsoft" is opgevraagd met de `typehead` parameter ingesteld op **waar**. Als u het antwoord ziet, kunt u zien dat de service voor zoeken in de query als gedeeltelijke query geïnterpreteerd en antwoord bevat de resultaten voor query automatisch is voorgesteld.

**Voorbeeldquery:**

```HTTP
https://atlas.microsoft.com/search/address/json?subscription-key={subscription-key}&api-version=1.0&typeahead=true&countrySet=US&lat=47.6370891183&lon=-122.123736172&query=Microsoft
```

**Antwoord:**

```JSON
{
    "summary": {
        "query": "microsoft",
        "queryType": "NON_NEAR",
        "queryTime": 25,
        "numResults": 6,
        "offset": 0,
        "totalResults": 6,
        "fuzzyLevel": 1,
        "geoBias": {
            "lat": 47.6370891183,
            "lon": -122.123736172
        }
    },
    "results": [
        {
            "type": "Street",
            "id": "US/STR/p0/10294417",
            "score": 2.594,
            "dist": 327.546040632591,
            "address": {
                "streetName": "Microsoft Way",
                "municipalitySubdivision": "Redmond",
                "municipality": "Redmond",
                "countrySecondarySubdivision": "King",
                "countryTertiarySubdivision": "Seattle East",
                "countrySubdivision": "WA",
                "postalCode": "98052",
                "extendedPostalCode": "980526399,980528300",
                "countryCode": "US",
                "country": "United States Of America",
                "countryCodeISO3": "USA",
                "freeformAddress": "Microsoft Way, Redmond, WA 98052",
                "countrySubdivisionName": "Washington"
            },
            "position": {
                "lat": 47.63989,
                "lon": -122.12509
            },
            "viewport": {
                "topLeftPoint": {
                    "lat": 47.63748,
                    "lon": -122.12309
                },
                "btmRightPoint": {
                    "lat": 47.64223,
                    "lon": -122.13061
                }
            }
        },
        ...,
        ...,
        ...,
        ...,
        {
            "type": "Street",
            "id": "US/STR/p0/9063400",
            "score": 2.075,
            "dist": 3655467.6406921702,
            "address": {
                "streetName": "Microsoft Way",
                "municipalitySubdivision": "Yorkmount, Charlotte",
                "municipality": "Charlotte",
                "countrySecondarySubdivision": "Mecklenburg",
                "countryTertiarySubdivision": "Township 1 Charlotte",
                "countrySubdivision": "NC",
                "postalCode": "28217",
                "countryCode": "US",
                "country": "United States Of America",
                "countryCodeISO3": "USA",
                "freeformAddress": "Microsoft Way, Charlotte, NC 28217",
                "countrySubdivisionName": "North Carolina"
            },
            "position": {
                "lat": 35.14279,
                "lon": -80.91814
            },
            "viewport": {
                "topLeftPoint": {
                    "lat": 35.14267,
                    "lon": -80.91814
                },
                "btmRightPoint": {
                    "lat": 35.14279,
                    "lon": -80.91824
                }
            }
        }
    ]
}
```


### <a name="uri-encoding-to-handle-special-characters"></a>URI-codering voor het afhandelen van speciale tekens 

Om te zoeken tussen adressen, dat wil zeggen '1st Avenue & Union straat, Seattle', speciaal teken '&' moet worden gecodeerd voordat de aanvraag wordt verzonden. We raden tekengegevens in een URI-codering waarbij alle tekens zijn gecodeerd met een '%'-teken en hexadecimale waarde overeenkomt met de UTF-8-teken twee tekens.

**Voorbeelden van het gebruik**:

Search-adres ophalen:

```
query=1st Avenue & E 111th St, New York
```

 moeten worden gecodeerd als:

```
query"=1st%20Avenue%20%26%20E%20111th%20St%2C%20New%20York
```


Hier volgen de verschillende methoden te gebruiken voor verschillende talen: 

JavaScript/TypeScript:
```Javascript
encodeURIComponent(query)
```

C#/VB:
```C#
Uri.EscapeDataString(query)
```

Java:
```Java
URLEncoder.encode(query, "UTF-8") 
```

Python:
```Python
import urllib.parse 
urllib.parse.quote(query)
```

C++:
```C++
#include <curl/curl.h>
curl_easy_escape(query)
```

PHP:
```PHP
urlencode(query)
```

Ruby:
```Ruby
CGI::escape(query) 
```

Swift:
```Swift
query.stringByAddingPercentEncodingWithAllowedCharacters(.URLHostAllowedCharacterSet()) 
```

Ga naar:
```Go
import ("net/url") 
url.QueryEscape(query)
```


## <a name="best-practices-for-poi-search"></a>Aanbevolen procedures voor POI zoeken

Punten van belang (pijl) Search kunt u om aan te vragen van resultaten van de route met de naam, bijvoorbeeld, zoeken bedrijven met de naam. Wij raden u aan het gebruik van de `countrySet` parameter om op te geven van landen waar de toepassing nodig dekking heeft, zoals het standaardgedrag is om te zoeken naar de hele wereld, mogelijk onnodige resultaten retourneren en/of leiden tot langere zoektijden.

### <a name="brand-search"></a>Merk zoeken

Ter verbetering van de relevantie van de resultaten en de informatie in het antwoord bevat de punt van belang (pijl) zoeken antwoord de merk-gegevens die verder kan worden gebruikt voor het parseren van het antwoord.

Laten we een [POI categorie zoeken](https://docs.microsoft.com/rest/api/maps/search/getsearchpoicategory) aanvraag voor benzinestations in de buurt van Microsoft-campus (Redmond, WA). Als u ziet dat het antwoord, ziet u merk-informatie voor elke route die geretourneerd.

**Voorbeeldquery:**

```HTTP
https://atlas.microsoft.com/search/poi/json?subscription-key={subscription-key}&api-version=1.0&query=gas%20station&limit=3&lat=47.6413362&lon=-122.1327968
```

**Antwoord:**

```JSON
{
    "summary": {
        "query": "gas station",
        "queryType": "NON_NEAR",
        "queryTime": 206,
        "numResults": 3,
        "offset": 0,
        "totalResults": 742169,
        "fuzzyLevel": 1,
        "geoBias": {
            "lat": 47.6413362,
            "lon": -122.1327968
        }
    },
    "results": [
        {
            "type": "POI",
            "id": "US/POI/p0/245813",
            "score": 5.663,
            "dist": 1037.0280221303253,
            "info": "search:ta:840531000004190-US",
            "poi": {
                "name": "Chevron",
                "phone": "+(1)-(425)-6532200",
                "brands": [
                    {
                        "name": "Chevron"
                    }
                ],
                "url": "www.chevron.com",
                "classifications": [
                    {
                        "code": "PETROL_STATION",
                        "names": [
                            {
                                "nameLocale": "en-US",
                                "name": "petrol station"
                            }
                        ]
                    }
                ]
            },
            "address": {
                "streetNumber": "2444",
                "streetName": "Bel Red Rd",
                "municipalitySubdivision": "Northeast Bellevue, Bellevue",
                "municipality": "Bellevue",
                "countrySecondarySubdivision": "King",
                "countryTertiarySubdivision": "Seattle East",
                "countrySubdivision": "WA",
                "postalCode": "98007",
                "countryCode": "US",
                "country": "United States Of America",
                "countryCodeISO3": "USA",
                "freeformAddress": "2444 Bel Red Rd, Bellevue, WA 98007",
                "countrySubdivisionName": "Washington"
            },
            "position": {
                "lat": 47.63201,
                "lon": -122.13281
            },
            "viewport": {
                "topLeftPoint": {
                    "lat": 47.63291,
                    "lon": -122.13414
                },
                "btmRightPoint": {
                    "lat": 47.63111,
                    "lon": -122.13148
                }
            },
            "entryPoints": [
                {
                    "type": "main",
                    "position": {
                        "lat": 47.63223,
                        "lon": -122.13311
                    }
                }
            ]
        },
        ...,
        {
            "type": "POI",
            "id": "US/POI/p0/7727106",
            "score": 5.662,
            "dist": 1458.645407416307,
            "info": "search:ta:840539000488527-US",
            "poi": {
                "name": "BROWN BEAR CAR WASH",
                "phone": "+(1)-(425)-6442868",
                "brands": [
                    {
                        "name": "Texaco"
                    }
                ],
                "url": "www.texaco.com/",
                "classifications": [
                    {
                        "code": "PETROL_STATION",
                        "names": [
                            {
                                "nameLocale": "en-US",
                                "name": "petrol station"
                            }
                        ]
                    }
                ]
            },
            "address": {
                "streetNumber": "15248",
                "streetName": "Bel Red Rd",
                "municipalitySubdivision": "Redmond",
                "municipality": "Redmond",
                "countrySecondarySubdivision": "King",
                "countryTertiarySubdivision": "Seattle East",
                "countrySubdivision": "WA",
                "postalCode": "98052",
                "extendedPostalCode": "980525511",
                "countryCode": "US",
                "country": "United States Of America",
                "countryCodeISO3": "USA",
                "freeformAddress": "15248 Bel Red Rd, Redmond, WA 98052",
                "countrySubdivisionName": "Washington"
            },
            "position": {
                "lat": 47.62843,
                "lon": -122.13628
            },
            "viewport": {
                "topLeftPoint": {
                    "lat": 47.62933,
                    "lon": -122.13761
                },
                "btmRightPoint": {
                    "lat": 47.62753,
                    "lon": -122.13495
                }
            },
            "entryPoints": [
                {
                    "type": "main",
                    "position": {
                        "lat": 47.62826,
                        "lon": -122.13626
                    }
                }
            ]
        }
    ]
}
```


### <a name="airport-search"></a>Luchthaven zoeken

PIJL Search ondersteunt luchthavens zoeken met behulp van de officiële luchthaven-codes. Bijvoorbeeld, **zee** (het internationale vliegveld van de Seattle-Tacoma). 

```HTTP
https://atlas.microsoft.com/search/poi/json?subscription-key={subscription-key}&api-version=1.0&query=SEA 
```

### <a name="nearby-search"></a>In de buurt zoeken

Om op te halen, alleen POI resultaten om een specifieke locatie, de [in de buurt zoeken-API](https://docs.microsoft.com/rest/api/maps/search/getsearchnearby) mogelijk de juiste keuze. Dit eindpunt, retourneert alleen resultaten POI en neemt niet in een queryparameter zoeken. Als u wilt de resultaten beperken, is het aanbevolen om in te stellen van de radius.

## <a name="understanding-the-responses"></a>Informatie over de antwoorden

Laten we een zoekaanvraag adres naar de Azure-kaarten [search-service](https://docs.microsoft.com/rest/api/maps/search) voor een adres in Seattle. Als u op de onderstaande aanvraag-URL letten, hebben we ingesteld de `countrySet` parameter **VS** om te zoeken naar het adres in de Verenigde Staten van Amerika.

**Voorbeeldquery:**

```HTTP
https://atlas.microsoft.com/search/address/json?subscription-key={subscription-key}&api-version=1&query=400%20Broad%20Street%2C%20Seattle%2C%20WA&countrySet=US
```

We hebben verder een overzicht van de reactie van onderstaande structuur. De resultaattypen van de resultaatobjecten in het antwoord zijn verschillend. Als u zorgvuldig dat u ziet dat er drie verschillende typen resultaatobjecten ziet, die zijn "Punt adres", "Adres" en 'Cross straat'. U ziet dat adres met de zoekactie geen koeriersbedrijven geretourneerd. De `Score` parameter voor elk antwoordobject geeft aan dat de relatieve overeenkomende score scores van andere objecten in hetzelfde antwoord. Zie [Search-adres ophalen](https://docs.microsoft.com/rest/api/maps/search/getsearchaddress) voor meer informatie over parameters voor antwoord-object.

**Ondersteunde typen resultaat:**

* **Punt-adres:** Punten op een kaart met een specifiek adres met een straatnaam en een nummer. Het hoogste niveau van de nauwkeurigheid van de gegevens beschikbaar voor adressen. 

* **Adresbereik:**  Voor sommige Streets (straten) zijn er adres punten die worden geïnterpoleerd vanaf het begin en einde van de straat; Deze punten worden weergegeven als de adresbereiken. 

* **Geografie:** Gebieden op een kaart die staan voor administratieve indeling van een land, dat wil zeggen, land, status, plaats. 

* **PIJL - (punten van belang):** Punten op een kaart die zijn het noemen waard aandacht en die van belang zijn.

* **Adres:** Weergave van Streets (straten) op de kaart. Adressen worden omgezet in de breedtegraad/lengtegraad-coördinaat van de straat die het adres bevat. Het huisnummer kan niet worden verwerkt. 

* **Cross-adres:** Snijpunten. In de vorm van koppelingen; de locaties waar twee Streets (straten) elkaar overlappen.

**Antwoord:**

```JSON
{
    "summary": {
        "query": "400 broad street seattle wa",
        "queryType": "NON_NEAR",
        "queryTime": 129,
        "numResults": 6,
        "offset": 0,
        "totalResults": 6,
        "fuzzyLevel": 1
    },
    "results": [
        {
            "type": "Point Address",
            "id": "US/PAD/p0/43076024",
            "score": 9.894,
            "address": {
                "streetNumber": "400",
                "streetName": "Broad Street",
                "municipalitySubdivision": "Seattle, South Lake Union, Lower Queen Anne",
                "municipality": "Seattle",
                "countrySecondarySubdivision": "King",
                "countryTertiarySubdivision": "Seattle",
                "countrySubdivision": "WA",
                "postalCode": "98109",
                "countryCode": "US",
                "country": "United States Of America",
                "countryCodeISO3": "USA",
                "freeformAddress": "400 Broad Street, Seattle, WA 98109",
                "countrySubdivisionName": "Washington"
            },
            "position": {
                "lat": 47.62039,
                "lon": -122.34928
            },
            "viewport": {
                "topLeftPoint": {
                    "lat": 47.62129,
                    "lon": -122.35061
                },
                "btmRightPoint": {
                    "lat": 47.61949,
                    "lon": -122.34795
                }
            },
            "entryPoints": [
                {
                    "type": "main",
                    "position": {
                        "lat": 47.61982,
                        "lon": -122.34886
                    }
                }
            ]
        },
        {
            "type": "Street",
            "id": "US/STR/p0/2440854",
            "score": 8.129,
            "address": {
                "streetName": "Broad Street",
                "municipalitySubdivision": "Seattle, Westlake, South Lake Union",
                "municipality": "Seattle",
                "countrySecondarySubdivision": "King",
                "countryTertiarySubdivision": "Seattle",
                "countrySubdivision": "WA",
                "postalCode": "98109",
                "extendedPostalCode": "981094347,981094700,981094701,981094702",
                "countryCode": "US",
                "country": "United States Of America",
                "countryCodeISO3": "USA",
                "freeformAddress": "Broad Street, Seattle, WA 98109",
                "countrySubdivisionName": "Washington"
            },
            "position": {
                "lat": 47.62553,
                "lon": -122.33936
            },
            "viewport": {
                "topLeftPoint": {
                    "lat": 47.62545,
                    "lon": -122.33861
                },
                "btmRightPoint": {
                    "lat": 47.62574,
                    "lon": -122.33974
                }
            }
        },
        {
            "type": "Street",
            "id": "US/STR/p0/8450985",
            "score": 8.129,
            "address": {
                "streetName": "Broad Street",
                "municipalitySubdivision": "Seattle, Belltown",
                "municipality": "Seattle",
                "countrySecondarySubdivision": "King",
                "countryTertiarySubdivision": "Seattle",
                "countrySubdivision": "WA",
                "postalCode": "98109,98121",
                "extendedPostalCode": "981094991,981211117,981211237,981213206",
                "countryCode": "US",
                "country": "United States Of America",
                "countryCodeISO3": "USA",
                "freeformAddress": "Broad Street, Seattle, WA",
                "countrySubdivisionName": "Washington"
            },
            "position": {
                "lat": 47.61691,
                "lon": -122.35251
            },
            "viewport": {
                "topLeftPoint": {
                    "lat": 47.61502,
                    "lon": -122.35041
                },
                "btmRightPoint": {
                    "lat": 47.61857,
                    "lon": -122.35484
                }
            }
        },
        ...,
        ...,
        {
            "type": "Cross Street",
            "id": "US/XSTR/p1/3816818",
            "score": 6.759,
            "address": {
                "streetName": "Broad Street & Valley Street",
                "municipalitySubdivision": "South Lake Union, Seattle",
                "municipality": "Seattle",
                "countrySecondarySubdivision": "King",
                "countryTertiarySubdivision": "Seattle",
                "countrySubdivision": "WA",
                "postalCode": "98109",
                "countryCode": "US",
                "country": "United States Of America",
                "countryCodeISO3": "USA",
                "freeformAddress": "Broad Street & Valley Street, Seattle, WA 98109",
                "countrySubdivisionName": "Washington"
            },
            "position": {
                "lat": 47.62574,
                "lon": -122.33861
            },
            "viewport": {
                "topLeftPoint": {
                    "lat": 47.62664,
                    "lon": -122.33994
                },
                "btmRightPoint": {
                    "lat": 47.62484,
                    "lon": -122.33728
                }
            }
        }
    ]
}
```

### <a name="geometry"></a>geometrie

Wanneer het antwoord van het type is **geometrie**, neemt de geometrie-ID die wordt geretourneerd in de **gegevensbronnen** object onder 'geometry' en 'id'. Bijvoorbeeld, [ophalen veelhoek service](https://docs.microsoft.com/rest/api/maps/search/getsearchpolygon) Hiermee kunt u de geometriegegevens in de GeoJSON-indeling, zoals een plaats of luchthaven overzicht voor een set standaardentiteiten aanvragen. U kunt deze gegevens grens voor [Geofencing](https://docs.microsoft.com/azure/azure-maps/tutorial-geofence) of [koeriersbedrijven zoeken binnen de geometrie](https://docs.microsoft.com/rest/api/maps/search/postsearchinsidegeometry).


[Zoeken naar adres](https://docs.microsoft.com/rest/api/maps/search/getsearchaddress) of [Fuzzy zoeken](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy) API antwoorden kunnen opnemen de **geometrie-ID** die wordt geretourneerd in het object gegevensbronnen onder 'geometry' en 'id'.


```JSON 
"dataSources": { 
        "geometry": { 
            "id": "00005557-4100-3c00-0000-000059690938" // The geometry ID is returned in the dataSources object under "geometry" and "id".
        }
} 
```

## <a name="next-steps"></a>Volgende stappen

* Informatie over [over het bouwen van Azure Maps Search serviceaanvragen](https://docs.microsoft.com/azure/azure-maps/how-to-search-for-address).
* De Azure Maps verkennen [zoeken naar documentatie over service-API](https://docs.microsoft.com/rest/api/maps/search). 