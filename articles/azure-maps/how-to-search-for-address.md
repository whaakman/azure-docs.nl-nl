---
title: Over het zoeken naar een adres met behulp van de service Azure Maps Search | Microsoft Docs
description: Meer informatie over het zoeken naar een adres met behulp van de Azure Maps Search-service
author: dsk-2015
ms.author: dkshir
ms.date: 05/07/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: 8b7d2119e1eef8532c30b0a45ae2684493462277
ms.sourcegitcommit: df50934d52b0b227d7d796e2522f1fd7c6393478
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38990010"
---
# <a name="how-to-find-an-address-using-the-azure-maps-search-service"></a>Het zoeken naar een adres met behulp van de search-service van Azure Maps

De kaarten search-service is een RESTful-set met API's die zijn ontworpen voor ontwikkelaars om te zoeken naar adressen, plaatsen, punten van belang, bedrijfsvermeldingen en andere geografische informatie. De service wijst een breedtegraad/lengtegraad toe aan een specifiek adres, cross straat, geografische functie of nuttige (pijl). Breedtegraad en lengtegraad waarden die zijn geretourneerd door de zoekopdracht kunnen worden gebruikt als parameters in andere kaarten-services zoals route en.

## <a name="prerequisites"></a>Vereisten

Als u wilt dat alle aanroepen naar de Maps-service API's, moet u een Maps-account en een sleutel. Zie voor meer informatie over het maken van een account en het ophalen van een sleutel [over het beheren van uw Azure kaarten-account en sleutels](how-to-manage-account-keys.md).

In dit artikel wordt de [Postman-app](https://www.getpostman.com/apps) REST-aanroepen te bouwen. U kunt elke API-ontwikkelomgeving die u wilt gebruiken. 


## <a name="using-fuzzy-search"></a>Fuzzy zoekopdrachten gebruiken

De standaard-API voor search-service is [fuzzy zoekopdrachten](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy), dat invoer van een combinatie van adres of de pijl-tokens worden verwerkt. Deze zoeken-API is de canonieke één regel zoeken en is handig als u niet wat uw invoer van gebruikers als een zoekquery weet. De fuzzy zoekopdrachten-API is een combinatie van POI zoeken en geocodering. De API kan ook worden gewogen met een contextuele positie (LAT/ion. Koppel), volledig worden beperkt door een coördinaat en de RADIUS-, of deze meer in het algemeen kan worden uitgevoerd zonder een geo bijstelling anker.

De meeste zoekopdrachten standaard ingesteld op ' maxFuzzyLevel = 1' om de prestaties en verminder onvoorspelbare resultaten. Deze standaardinstelling kan worden genegeerd, indien nodig per aanvraag door te geven in de queryparameter ' maxFuzzyLevel = 2' of '3'.

### <a name="search-for-an-address-using-fuzzy-search"></a>Zoeken naar een adres met behulp van Fuzzy zoeken

1. Open de Postman-app, klikt u op Nieuw | Nieuw maken, en selecteer **GET-aanvraag**. Voer de Aanvraagnaam van een van **Fuzzy zoekopdrachten**, selecteert u een verzameling of een map op te slaan en op **opslaan**.

2. Selecteer op het tabblad Builder de **ophalen** HTTP-methode en voer de aanvraag-URL voor uw API-eindpunt.

    ![Fuzzy zoeken ](./media/how-to-search-for-address/fuzzy_search_url.png)

    | Parameter | Voorgestelde waarde |
    |---------------|------------------------------------------------|
    | HTTP-methode | GET |
    | Aanvraag-URL | https://atlas.microsoft.com/search/fuzzy/json? |
    | Autorisatie | Geen autorisatie |

    De **json** kenmerk in het URL-pad bepaalt de indeling van het antwoord. U gebruikt json in dit artikel voor gebruiksgemak en leesbaarheid. U vindt de beschikbare antwoord indelingen in de **zoeken bij benadering ophalen** definitie van de [functionele kaarten-API-referentie] (https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy).

3. Klik op **Params**, en voer de volgende sleutel / waarde-paren te gebruiken als parameters query of het pad in de aanvraag-URL:

    ![Fuzzy zoeken ](./media/how-to-search-for-address/fuzzy_search_params.png)

    | Sleutel | Waarde |
    |------------------|-------------------------|
    | API-versie | 1.0 |
    | abonnement-sleutel | \<de sleutel van uw Azure-kaarten\> |
    | query | pizza |

4. Klik op **verzenden** en bekijk de antwoordtekst. 

    De niet-eenduidige queryreeks van 'pizza' geretourneerd 10 punten van belang (pijl) resultaten met categorieën vallen in "pizza" en 'restaurant'. Elk resultaat retourneert een Straatadres, breedtegraad / lengtegraadwaarden, poort en toegangspunten voor de locatie weergeven.
    
    De resultaten zijn verschillend voor deze query is niet gekoppeld aan een bepaalde referentielocatie. U kunt de **countrySet** parameter om op te geven alleen de landen waarvoor de toepassing nodig dekking heeft, zoals het standaardgedrag is om te zoeken naar de hele wereld, mogelijk onnodige resultaten retourneren.

5. Voeg de volgende sleutel / waarde-paar aan de **Params** sectie en klikt u op **verzenden**:

    | Sleutel | Waarde |
    |------------------|-------------------------|
    | countrySet | VS |
    
    De resultaten zijn nu begrensd door de landcode en de query retourneert pizza restaurants in de Verenigde Staten.
    
    Om resultaten die zijn gericht op een bepaalde locatie te krijgen, kunt u een query en de geretourneerde breedtegraad en lengtegraadwaarden in de aanroep van de service Fuzzy zoekopdrachten gebruiken. In dit geval de Search-service gebruikt om te retourneren van de locatie van de wijzer omheen Seattle-ruimte en de LAT gebruikt / ion. de waarden te zien van de zoekopdracht.
    
4. In-parameters, voer de volgende sleutel / waarde-paren en klikt u op **verzenden**:

    ![Fuzzy zoeken ](./media/how-to-search-for-address/fuzzy_search_latlon.png)
    
    | Sleutel | Waarde |
    |-----|------------|
    | LAT | 47.62039 |
    | ion | -122.34928 |

## <a name="search-for-address-properties-and-coordinates"></a>Eigenschappen voor het adres en coördinaten zoeken 

U kunt een volledige of gedeeltelijke adres doorgeven aan het adres search API en een antwoord met eigenschappen, zoals gemeenteraad of onderverdeling, evenals positionele waarden gedetailleerde adres in breedtegraad en lengtegraad.

1. In Postman, klikt u op **nieuwe aanvraag** | **GET-aanvraag** en noem het **zoeken**.
2. Selecteer op het tabblad Builder de **ophalen** HTTP-methode, voer de aanvraag-URL voor uw API-eindpunt en selecteer een autorisatieprotocol, indien van toepassing.

    ![Adres zoeken ](./media/how-to-search-for-address/address_search_url.png)
    
    | Parameter | Voorgestelde waarde |
    |---------------|------------------------------------------------|
    | HTTP-methode | GET |
    | Aanvraag-URL | https://atlas.microsoft.com/search/address/json? |
    | Autorisatie | Geen autorisatie |

2. Klik op **Params**, en voer de volgende sleutel / waarde-paren te gebruiken als parameters query of het pad in de aanvraag-URL:
    
    ![Adres zoeken ](./media/how-to-search-for-address/address_search_params.png)
    
    | Sleutel | Waarde |
    |------------------|-------------------------|
    | API-versie | 1.0 |
    | abonnement-sleutel | \<de sleutel van uw Azure-kaarten\> |
    | query | 400 brede St, Seattle, WA 98109 |
    
3. Klik op **verzenden** en bekijk de antwoordtekst. 
    
    In dit geval u een query volledige adres opgegeven en een enkelvoudig resultaat wordt verkregen in de hoofdtekst van antwoord ontvangen. 
    
4. In de parameters, bewerkt u de queryreeks toe aan de volgende waarde:
    ```
        400 Broad, Seattle
    ```

5. Voeg de volgende sleutel / waarde-paar aan de **Params** sectie en klikt u op **verzenden**:

    | Sleutel | Waarde |
    |-----|------------|
    | typeahead | true |

    De **typeahead** vlag wordt aan het adres zoeken-API te behandelen de query als een gedeeltelijke invoer een matrix van voorspellende waarden retourneren.

## <a name="search-for-a-street-address-using-reverse-address-search"></a>Zoeken naar een adres met behulp van omgekeerde-adres zoeken
1. In Postman, klikt u op **nieuwe aanvraag** | **GET-aanvraag** en noem het **Reverse-adres zoeken**.

2. Selecteer op het tabblad Builder de **ophalen** HTTP-methode en voer de aanvraag-URL voor uw API-eindpunt.
    
    ![URL van omgekeerde-adres zoeken ](./media/how-to-search-for-address/reverse_address_search_url.png)
    
    | Parameter | Voorgestelde waarde |
    |---------------|------------------------------------------------|
    | HTTP-methode | GET |
    | Aanvraag-URL | https://atlas.microsoft.com/search/address/reverse/json? |
    | Autorisatie | Geen autorisatie |
    
2. Klik op **Params**, en voer de volgende sleutel / waarde-paren te gebruiken als parameters query of het pad in de aanvraag-URL:
    
    ![Reverse-adres zoekparameters ](./media/how-to-search-for-address/reverse_address_search_params.png)
    
    | Sleutel | Waarde |
    |------------------|-------------------------|
    | API-versie | 1.0 |
    | abonnement-sleutel | \<de sleutel van uw Azure-kaarten\> |
    | query | 47.59093,-122.33263 |
    
3. Klik op **verzenden** en bekijk de antwoordtekst. 
    
    Het antwoord bevat de pijl-vermelding voor Safeco Field met een pijl categorie 'stadion'. 
    
4. Voeg de volgende sleutel / waarde-paar aan de **Params** sectie en klikt u op **verzenden**:

    | Sleutel | Waarde |
    |-----|------------|
    | getal | true |

    Als de [getal](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse#search_getsearchaddressreverse_uri_parameters) queryparameter met de aanvraag is verzonden, moet het antwoord kan de kant van de straat (links/rechts) en ook de positie van een offset voor dat getal bevatten.
    
5. Voeg de volgende sleutel / waarde-paar aan de **Params** sectie en klikt u op **verzenden**:

    | Sleutel | Waarde |
    |-----|------------|
    | spatialKeys | true |

    Wanneer de [spatialKeys](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse#search_getsearchaddressreverse_uri_parameters) queryparameter is ingesteld, wordt het antwoord bevat eigen geo-ruimtelijk belangrijke informatie voor een opgegeven locatie.

6. Voeg de volgende sleutel / waarde-paar aan de **Params** sectie en klikt u op **verzenden**:

    | Sleutel | Waarde |
    |-----|------------|
    | returnSpeedLimit | true |
    
    Wanneer de [returnSpeedLimit](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse#search_getsearchaddressreverse_uri_parameters) queryparameter is ingesteld, het antwoord geretourneerd van de limiet voor geboekte snelheid.

7. Voeg de volgende sleutel / waarde-paar aan de **Params** sectie en klikt u op **verzenden**:

    | Sleutel | Waarde |
    |-----|------------|
    | returnRoadUse | true |

    Wanneer de [returnRoadUse](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse#search_getsearchaddressreverse_uri_parameters) queryparameter is ingesteld, wordt het antwoord retourneert de matrix voor het gebruik van weg voor reversegeocodes op straat niveau.

8. Voeg de volgende sleutel / waarde-paar aan de **Params** sectie en klikt u op **verzenden**:

    | Sleutel | Waarde |
    |-----|------------|
    | roadUse | true |

    U kunt de geocode omgekeerde query beperken tot een specifiek type weg gebruiken met behulp van de [roadUse](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse#search_getsearchaddressreverse_uri_parameters) queryparameter.
    
## <a name="search-for-the-cross-street-using-reverse-address-cross-street-search"></a>Zoek de cross straat met omgekeerde adres Cross straat zoeken

1. In Postman, klikt u op **nieuwe aanvraag** | **GET-aanvraag** en noem het **omgekeerde adres Cross-adres zoeken**.

2. Selecteer op het tabblad Builder de **ophalen** HTTP-methode en voer de aanvraag-URL voor uw API-eindpunt.
    
    ![Reverse-adres Cross-adres zoeken ](./media/how-to-search-for-address/reverse_address_search_url.png)
    
    | Parameter | Voorgestelde waarde |
    |---------------|------------------------------------------------|
    | HTTP-methode | GET |
    | Aanvraag-URL | https://atlas.microsoft.com/search/address/reverse/crossstreet/json? |
    | Autorisatie | Geen autorisatie |
    
3. Klik op **Params**, en voer de volgende sleutel / waarde-paren te gebruiken als parameters query of het pad in de aanvraag-URL:
    
    | Sleutel | Waarde |
    |------------------|-------------------------|
    | API-versie | 1.0 |
    | abonnement-sleutel | \<de sleutel van uw Azure-kaarten\> |
    | query | 47.59093,-122.33263 |
    
4. Klik op **verzenden** en bekijk de antwoordtekst. 

## <a name="next-steps"></a>Volgende stappen
- Verken de [search-service van Azure Maps](https://docs.microsoft.com/rest/api/maps/search) API-documentatie 
