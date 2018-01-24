---
title: Het zoeken naar een adres met behulp van de zoekservice Azure locatie op basis van Services (preview) | Microsoft Docs
description: Meer informatie over het zoeken naar een adres met de Azure locatie op basis van Services (preview) Search-service
services: location-based-services
keywords: Voeg geen of trefwoorden zonder overleg met uw EXPERT in Zoekmachineoptimalisatie bewerken.
author: philmea
ms.author: philmea
ms.date: 11/29/2017
ms.topic: article
ms.service: location-based-services
ms.openlocfilehash: cacaaab869d3a7752b5a750f01bbfbdaf79814f7
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="how-to-find-an-address-using-the-azure-location-based-services-preview-search-service"></a>Het zoeken van een adres met behulp van de zoekservice Azure locatie op basis van Services (preview)
De Search-service is een RESTful reeks API's die zijn ontworpen voor ontwikkelaars om te zoeken naar adressen, locaties, punten van belang, lijsten van bedrijven en andere geografische informatie. Een breedtegraad/lengtegraad wijst de Search-Service naar een specifiek adres, cross straat, geografische functie of interessante (POI). Breedtegraad en lengtegraad waarden geretourneerd door de zoekservice API's kunnen worden gebruikt als parameters in andere Azure locatie op basis van Services zoals de verkeer stromen API's en Route.

## <a name="prerequisites"></a>Vereisten
* Installeer de [Postman app](https://www.getpostman.com/apps).

* Een locatie op basis van Azure-Services-account en een sleutel hebben. Zie voor meer informatie over het maken van een account en bij het ophalen van een sleutel [uw locatie op basis van Azure-Services-account en de sleutels beheren](how-to-manage-account-keys.md). 

## <a name="using-fuzzy-search"></a>Met behulp van Fuzzy zoeken

De standaard-API voor de zoekservice is Fuzzy zoeken die invoer van een combinatie van-adres of POI-tokens verwerkt. Deze zoekopdracht API is van de canonieke eenregelige zoekopdracht en is handig als u niet wat uw gebruikersinvoer als een zoekquery weet. De Fuzzy zoeken-API is een combinatie van POI search en geocodering. De API kan ook worden gewogen met een contextuele positie (LAT/ion. Koppel), volledig wordt beperkt door een coördinaat en RADIUS-, of deze meer in het algemeen kan worden uitgevoerd zonder een geo bijstelling fixeerpunt.

De meeste zoekopdrachten standaard ingesteld op ' maxFuzzyLevel = 1' om toegang te krijgen van de prestaties en onvoorspelbare resultaten te verminderen. Deze standaardinstelling kan worden genegeerd, indien nodig per aanvraag door door te geven in de query-parameter ' maxFuzzyLevel = 2' of '3'.

### <a name="search-for-an-address-using-fuzzy-search"></a>Zoeken naar een adres Fuzzy zoeken

1. Open de app Postman, klikt u op Nieuw | Nieuw maken en selecteer **GET-aanvraag**. Voer de Aanvraagnaam van een van **Fuzzy zoeken**, selecteert u een verzameling of een map op te slaan en op **opslaan**.

    Raadpleeg de Postman aanvragen documentatie voor meer informatie.

2. Selecteer op het tabblad Builder de **ophalen** HTTP-methode en voer de aanvraag-URL voor uw API-eindpunt.

    ![Fuzzy zoeken ](./media/how-to-search-for-address/fuzzy_search_url.png)

    | Parameter | Voorgestelde waarde |
    |---------------|------------------------------------------------|
    | HTTP-methode | GET |
    | Aanvraag-URL | https://atlas.microsoft.com/search/fuzzy/json? |
    | Autorisatie | Er is geen verificatie |

    De **json** kenmerk in het URL-pad bepaalt de indeling van het antwoord. U gebruikt json in dit artikel voor gebruiksgemak en leesbaarheid. U vindt de notaties beschikbaar antwoord in de **zoeken bij benadering ophalen** definitie van de [locatie op basis van Services functionele API reference] (https://docs.microsoft.com/rest/api/location-based-services/search/getsearchfuzzy).

3. Klik op **Params**, en voer de volgende sleutel en waarde om te gebruiken als de query of pad parameters in de aanvraag-URL:

    ![Fuzzy zoeken ](./media/how-to-search-for-address/fuzzy_search_params.png)

    | Sleutel | Waarde |
    |------------------|-------------------------|
    | API-versie | 1.0 |
    | abonnement-sleutel | \<de sleutel van uw Azure-locatie op basis van Services\> |
    | query | pizza |

4. Klik op **verzenden** en bekijk de antwoordtekst. 

    De niet-eenduidige queryreeks van 'pizza' geretourneerd 10 punten van belang (POI) resultaten met categorieën vallen in 'pizza' en 'restaurant'. Elk resultaat retourneert een Straatadres, breedtegraad / lengtegraadwaarden, poort en toegangspunten voor de locatie weergeven.
    
    De resultaten voor deze query, niet gekoppeld aan een bepaalde referentielocatie verspreid. U kunt de **countrySet** parameter om alleen de landen waarvoor uw toepassing dekking moet, zoals het standaardgedrag is om te zoeken van de hele wereld, mogelijk onnodige resultaten retourneren.

5. Toevoegen van de volgende sleutel / waarde-paar voor de **Params** sectie en klik op **verzenden**:

    | Sleutel | Waarde |
    |------------------|-------------------------|
    | countrySet | VS |
    
    De resultaten zijn nu begrensd door de landcode en retourneert de query in een restaurant pizza in de Verenigde Staten.
    
    Als u resultaten die zijn gericht op een bepaalde locatie, kunt u query uitvoeren op een punt van belang en de geretourneerde breedtegraad en lengtegraadwaarden in de aanroep van de service Fuzzy zoeken gebruiken. In dit geval de Search-service gebruikt om te retourneren van de locatie van de wijzer van de ruimte Seattle en gebruikt de LAT toe. / ion. waarden voor het plaatsen van de zoekopdracht.
    
4. In-parameters, voer de volgende sleutel / waarde-paren en klikt u op **verzenden**:

    ![Fuzzy zoeken ](./media/how-to-search-for-address/fuzzy_search_latlon.png)
    
    | Sleutel | Waarde |
    |-----|------------|
    | LAT | 47.62039 |
    | ion | -122.34928 |

## <a name="search-for-address-properties-and-coordinates"></a>Adres-eigenschappen en coördinaten zoeken 

U kunt een volledige of gedeeltelijke Straatadres doorgeven aan de adres-API van zoekservice en beantwoord met gedetailleerde adres-eigenschappen zoals gemeente of onderverdeling, evenals positionele waarden in de breedtegraad en lengtegraad.

1. In Postman, klikt u op **nieuwe aanvraag** | **GET-aanvraag** en noem deze **zoeken**.
2. Selecteer op het tabblad Builder de **ophalen** HTTP-methode, voer de aanvraag-URL voor uw API-eindpunt en selecteer een (authorization protocol), indien van toepassing.

    ![Adres zoeken ](./media/how-to-search-for-address/address_search_url.png)
    
    | Parameter | Voorgestelde waarde |
    |---------------|------------------------------------------------|
    | HTTP-methode | GET |
    | Aanvraag-URL | https://atlas.microsoft.com/search/address/json? |
    | Autorisatie | Er is geen verificatie |

2. Klik op **Params**, en voer de volgende sleutel en waarde om te gebruiken als de query of pad parameters in de aanvraag-URL:
    
    ![Adres zoeken ](./media/how-to-search-for-address/address_search_params.png)
    
    | Sleutel | Waarde |
    |------------------|-------------------------|
    | API-versie | 1.0 |
    | abonnement-sleutel | \<de sleutel van uw Azure-locatie op basis van Services\> |
    | query | 400 Broad St, Seattle, WA 98109 |
    
3. Klik op **verzenden** en bekijk de antwoordtekst. 
    
    In dit geval wordt een query volledig adres opgegeven en een enkelvoudig resultaat wordt verkregen in de hoofdtekst van antwoord ontvangen. 
    
4. Bewerk in parameters, de queryreeks in de volgende waarde:
    ```
        400 Broad, Seattle
    ```

5. Toevoegen van de volgende sleutel / waarde-paar voor de **Params** sectie en klik op **verzenden**:

    | Sleutel | Waarde |
    |-----|------------|
    | typeahead | waar |

    De **typeahead** vlag wordt de API-adres zoeken aan de query behandelen als een gedeeltelijke invoer en een matrix van voorspellende waarden retourneren.

## <a name="search-for-a-street-address-using-reverse-address-search"></a>Zoeken naar een Straatadres Reverse adres zoekactie
1. In Postman, klikt u op **nieuwe aanvraag** | **GET-aanvraag** en noem deze **Reverse-adres zoeken**.

2. Selecteer op het tabblad Builder de **ophalen** HTTP-methode en voer de aanvraag-URL voor uw API-eindpunt.
    
    ![URL van omgekeerde-adres zoeken ](./media/how-to-search-for-address/reverse_address_search_url.png)
    
    | Parameter | Voorgestelde waarde |
    |---------------|------------------------------------------------|
    | HTTP-methode | GET |
    | Aanvraag-URL | https://atlas.microsoft.com/search/address/reverse/json? |
    | Autorisatie | Er is geen verificatie |
    
2. Klik op **Params**, en voer de volgende sleutel en waarde om te gebruiken als de query of pad parameters in de aanvraag-URL:
    
    ![De zoekparameters adres omkeren ](./media/how-to-search-for-address/reverse_address_search_params.png)
    
    | Sleutel | Waarde |
    |------------------|-------------------------|
    | API-versie | 1.0 |
    | abonnement-sleutel | \<de sleutel van uw Azure-locatie op basis van Services\> |
    | query | 47.59093,-122.33263 |
    
3. Klik op **verzenden** en bekijk de antwoordtekst. 
    
    Het antwoord bevat de POI-vermelding voor Safeco Field poi categorie 'stadium'. 
    
4. Toevoegen van de volgende sleutel / waarde-paar voor de **Params** sectie en klik op **verzenden**:

    | Sleutel | Waarde |
    |-----|------------|
    | nummer | waar |

    Als de [getal](https://docs.microsoft.com/rest/api/location-based-services/search/getsearchaddressreverse#search_getsearchaddressreverse_uri_parameters) queryparameter met de aanvraag is verzonden, moet het antwoord eventueel de kant van de straat (links/rechts) en ook de positie van een offset voor dat nummer.
    
5. Toevoegen van de volgende sleutel / waarde-paar voor de **Params** sectie en klik op **verzenden**:

    | Sleutel | Waarde |
    |-----|------------|
    | spatialKeys | waar |

    Wanneer de [spatialKeys](https://docs.microsoft.com/rest/api/location-based-services/search/getsearchaddressreverse#search_getsearchaddressreverse_uri_parameters) queryparameter is ingesteld, wordt het antwoord bevat eigen geo-ruimtelijke belangrijke informatie voor een opgegeven locatie.

6. Toevoegen van de volgende sleutel / waarde-paar voor de **Params** sectie en klik op **verzenden**:

    | Sleutel | Waarde |
    |-----|------------|
    | returnSpeedLimit | waar |
    
    Wanneer de [returnSpeedLimit](https://docs.microsoft.com/rest/api/location-based-services/search/getsearchaddressreverse#search_getsearchaddressreverse_uri_parameters) queryparameter is ingesteld, het antwoord geretourneerd van de geboekte limiet voor snelheid.

7. Toevoegen van de volgende sleutel / waarde-paar voor de **Params** sectie en klik op **verzenden**:

    | Sleutel | Waarde |
    |-----|------------|
    | returnRoadUse | waar |

    Wanneer de [returnRoadUse](https://docs.microsoft.com/rest/api/location-based-services/search/getsearchaddressreverse#search_getsearchaddressreverse_uri_parameters) queryparameter is ingesteld, wordt het antwoord retourneert de weg gebruik matrix voor reversegeocodes op niveau van de straat.

8. Toevoegen van de volgende sleutel / waarde-paar voor de **Params** sectie en klik op **verzenden**:

    | Sleutel | Waarde |
    |-----|------------|
    | roadUse | waar |

    U kunt de omgekeerde geocode query beperken tot een specifiek type van het gebruik van weg gebruik de [roadUse](https://docs.microsoft.com/rest/api/location-based-services/search/getsearchaddressreverse#search_getsearchaddressreverse_uri_parameters) queryparameter.
    
## <a name="search-for-the-cross-street-using-reverse-address-cross-street-search"></a>Zoeken naar de cross straat Reverse adres Cross straat zoekactie

1. In Postman, klikt u op **nieuwe aanvraag** | **GET-aanvraag** en noem deze **Reverse adres Cross straat zoeken**.

2. Selecteer op het tabblad Builder de **ophalen** HTTP-methode en voer de aanvraag-URL voor uw API-eindpunt.
    
    ![Omgekeerde adres Cross straat zoeken ](./media/how-to-search-for-address/reverse_address_search_url.png)
    
    | Parameter | Voorgestelde waarde |
    |---------------|------------------------------------------------|
    | HTTP-methode | GET |
    | Aanvraag-URL | https://atlas.microsoft.com/search/address/reverse/crossstreet/json? |
    | Autorisatie | Er is geen verificatie |
    
3. Klik op **Params**, en voer de volgende sleutel en waarde om te gebruiken als de query of pad parameters in de aanvraag-URL:
    
    | Sleutel | Waarde |
    |------------------|-------------------------|
    | API-versie | 1.0 |
    | abonnement-sleutel | \<de sleutel van uw Azure-locatie op basis van Services\> |
    | query | 47.59093,-122.33263 |
    
4. Klik op **verzenden** en bekijk de antwoordtekst. 

## <a name="next-steps"></a>Volgende stappen
- Verken de [Azure locatie op basis van Serices Search-service](https://docs.microsoft.com/rest/api/location-based-services/search) API-documentatie 
