---
title: Over het zoeken naar een adres met behulp van de service Azure Maps Search | Microsoft Docs
description: Meer informatie over het zoeken naar een adres met behulp van de Azure Maps Search-service
author: dsk-2015
ms.author: dkshir
ms.date: 09/11/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: ba5ea7205a5d0e5fdbadc8a1ba955091ae95b569
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/26/2019
ms.locfileid: "56875783"
---
# <a name="find-an-address-using-the-azure-maps-search-service"></a>Zoeken naar een adres met behulp van de search-service van Azure Maps

De kaarten search-service is een set RESTful API's die zijn ontworpen voor ontwikkelaars om te zoeken naar adressen, plaatsen, punten van belang, bedrijfsvermeldingen en andere geografische informatie. De service wijst een breedtegraad/lengtegraad toe aan een specifiek adres, cross straat, geografische functie of nuttige (pijl). Breedtegraad en lengtegraad waarden die zijn geretourneerd door de zoekopdracht kunnen worden gebruikt als parameters in andere kaarten-services zoals route en.

## <a name="prerequisites"></a>Vereisten

Als u wilt dat alle aanroepen naar de Maps-service API's, moet u een Maps-account en een sleutel. Zie voor meer informatie over het maken van een account en het ophalen van een sleutel [over het beheren van uw Azure kaarten-account en sleutels](how-to-manage-account-keys.md).

In dit artikel wordt de [Postman-app](https://www.getpostman.com/apps) REST-aanroepen te bouwen. U kunt elke API-ontwikkelomgeving die u wilt gebruiken.

## <a name="using-fuzzy-search"></a>Fuzzy zoekopdrachten gebruiken

De standaard-API voor search-service is [fuzzy zoekopdrachten](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy) en is handig als u niet weet wat uw invoer van gebruikers zijn voor een zoekopdracht. De API combineert POI zoeken en geocodering in een canonieke één regel zoeken. De API kan bijvoorbeeld de invoer van een adres of een token combinatie van POI verwerken. Het kan ook worden gewogen met een contextuele positie (LAT/ion. paar), volledig beperkt door een coördinaat en de RADIUS- of meer in het algemeen worden uitgevoerd zonder een geo bijstelling anker.

De meeste zoekopdrachten standaard `maxFuzzyLevel=1` om prestaties en onvoorspelbare resultaten te verminderen. Deze standaardinstelling kan worden genegeerd, indien nodig per aanvraag door te geven in de queryparameter `maxFuzzyLevel=2` of `3`.

### <a name="search-for-an-address-using-fuzzy-search"></a>Zoeken naar een adres met behulp van Fuzzy zoeken

1. Open de Postman-app, klikt u op Nieuw | Nieuw maken, en selecteer **GET-aanvraag**. Voer de Aanvraagnaam van een van **Fuzzy zoekopdrachten**, selecteert u een verzameling of een map op te slaan en op **opslaan**.

2. Selecteer op het tabblad Builder de **ophalen** HTTP-methode en voer de aanvraag-URL voor uw API-eindpunt.

    ![Fuzzy zoeken](./media/how-to-search-for-address/fuzzy_search_url.png)

    | Parameter | Voorgestelde waarde |
    |---------------|------------------------------------------------|
    | HTTP-methode | GET |
    | Aanvraag-URL | [https://atlas.microsoft.com/search/fuzzy/json?](https://atlas.microsoft.com/search/fuzzy/json?) |
    | Autorisatie | Geen autorisatie |

    De **json** kenmerk in het URL-pad bepaalt de indeling van het antwoord. U gebruikt json in dit artikel voor gebruiksgemak en leesbaarheid. U vindt de beschikbare antwoord indelingen in de **zoeken bij benadering ophalen** definitie van de [Maps functionele API-verwijzing](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy).

3. Klik op **Params**, en voer de volgende sleutel / waarde-paren te gebruiken als parameters query of het pad in de aanvraag-URL:

    ![Fuzzy zoeken](./media/how-to-search-for-address/fuzzy_search_params.png)

    | Sleutel | Value |
    |------------------|-------------------------|
    | API-versie | 1.0 |
    | abonnement-sleutel | \<de sleutel van uw Azure-kaarten\> |
    | query | pizza |

4. Klik op **verzenden** en bekijk de antwoordtekst.

    De niet-eenduidige queryreeks van 'pizza' geretourneerd 10 [punt van belang resultaat](https://docs.microsoft.com/rest/api/maps/search/getsearchpoi#searchpoiresponse) (pijl) resultaten met categorieën vallen in "pizza" en 'restaurant'. Elk resultaat retourneert een Straatadres, breedtegraad / lengtegraadwaarden, poort en toegangspunten voor de locatie weergeven.
  
    De resultaten zijn verschillend voor deze query is niet gekoppeld aan een bepaalde referentielocatie. U kunt de **countrySet** parameter om op te geven alleen de landen waarvoor de toepassing nodig dekking heeft, zoals het standaardgedrag is om te zoeken naar de hele wereld, mogelijk onnodige resultaten retourneren.

5. Voeg de volgende sleutel / waarde-paar aan de **Params** sectie en klikt u op **verzenden**:

    | Sleutel | Value |
    |------------------|-------------------------|
    | countrySet | VS |
  
    De resultaten zijn nu begrensd door de landcode en de query retourneert pizza restaurants in de Verenigde Staten.
  
    Om resultaten te krijgen voor een locatie, kunt u een query en de geretourneerde breedtegraad en lengtegraadwaarden in de aanroep van de service Fuzzy zoekopdrachten gebruiken. In dit geval de Search-service gebruikt om te retourneren van de locatie van de wijzer omheen Seattle-ruimte en de LAT gebruikt / ion. de waarden te zien van de zoekopdracht.
  
6. In-parameters, voer de volgende sleutel / waarde-paren en klikt u op **verzenden**:

    ![Fuzzy zoeken](./media/how-to-search-for-address/fuzzy_search_latlon.png)
  
    | Sleutel | Value |
    |-----|------------|
    | LAT | 47.620525 |
    | ion | -122.349274 |

## <a name="search-for-address-properties-and-coordinates"></a>Eigenschappen voor het adres en coördinaten zoeken

U kunt een volledige of gedeeltelijke adres doorgeven aan het adres search API en een antwoord met eigenschappen, zoals gemeenteraad of onderverdeling, evenals positionele waarden gedetailleerde adres in breedtegraad en lengtegraad.

1. In Postman, klikt u op **nieuwe aanvraag** | **GET-aanvraag** en noem het **zoeken**.
2. Selecteer op het tabblad Builder de **ophalen** HTTP-methode, voer de aanvraag-URL voor uw API-eindpunt en selecteer een autorisatieprotocol, indien van toepassing.

    ![Adres zoeken](./media/how-to-search-for-address/address_search_url.png)
  
    | Parameter | Voorgestelde waarde |
    |---------------|------------------------------------------------|
    | HTTP-methode | GET |
    | Aanvraag-URL | [https://atlas.microsoft.com/search/address/json?](https://atlas.microsoft.com/search/address/json?) |
    | Autorisatie | Geen autorisatie |

3. Klik op **Params**, en voer de volgende sleutel / waarde-paren te gebruiken als parameters query of het pad in de aanvraag-URL:
  
    ![Adres zoeken](./media/how-to-search-for-address/address_search_params.png)
  
    | Sleutel | Value |
    |------------------|-------------------------|
    | API-versie | 1.0 |
    | abonnement-sleutel | \<de sleutel van uw Azure-kaarten\> |
    | query | 400 Broad St, Seattle, WA 98109 |
  
4. Klik op **verzenden** en bekijk de antwoordtekst.
  
    In dit geval u een query volledige adres opgegeven en een enkelvoudig resultaat wordt verkregen in de hoofdtekst van antwoord ontvangen.
  
5. In de parameters, bewerkt u de queryreeks toe aan de volgende waarde:
    ```plaintext
        400 Broad, Seattle
    ```

6. Voeg de volgende sleutel / waarde-paar aan de **Params** sectie en klikt u op **verzenden**:

    | Sleutel | Value |
    |-----|------------|
    | typeahead | true |

    De **typeahead** vlag wordt aan het adres zoeken-API te behandelen de query als een gedeeltelijke invoer een matrix van voorspellende waarden retourneren.

## <a name="search-for-a-street-address-using-reverse-address-search"></a>Zoeken naar een adres met behulp van omgekeerde-adres zoeken

1. In Postman, klikt u op **nieuwe aanvraag** | **GET-aanvraag** en noem het **Reverse-adres zoeken**.

2. Selecteer op het tabblad Builder de **ophalen** HTTP-methode en voer de aanvraag-URL voor uw API-eindpunt.
  
    ![URL van omgekeerde-adres zoeken](./media/how-to-search-for-address/reverse_address_search_url.png)
  
    | Parameter | Voorgestelde waarde |
    |---------------|------------------------------------------------|
    | HTTP-methode | GET |
    | Aanvraag-URL | [https://atlas.microsoft.com/search/address/reverse/json?](https://atlas.microsoft.com/search/address/reverse/json?) |
    | Autorisatie | Geen autorisatie |
  
3. Klik op **Params**, en voer de volgende sleutel / waarde-paren te gebruiken als parameters query of het pad in de aanvraag-URL:
  
    ![Reverse-adres zoekparameters](./media/how-to-search-for-address/reverse_address_search_params.png)
  
    | Sleutel | Value |
    |------------------|-------------------------|
    | API-versie | 1.0 |
    | abonnement-sleutel | \<de sleutel van uw Azure-kaarten\> |
    | query | 47.591180,-122.332700 |
  
4. Klik op **verzenden** en bekijk de antwoordtekst.

    Het antwoord bevat belangrijke adresgegevens van Safeco Field.
  
5. Voeg de volgende sleutel / waarde-paar aan de **Params** sectie en klikt u op **verzenden**:

    | Sleutel | Value |
    |-----|------------|
    | getal | true |

    Als de [getal](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse) queryparameter met de aanvraag is verzonden, moet het antwoord kan de kant van de straat (links/rechts) en ook de positie van een offset voor dat getal bevatten.
  
6. Voeg de volgende sleutel / waarde-paar aan de **Params** sectie en klikt u op **verzenden**:

    | Sleutel | Value |
    |-----|------------|
    | returnSpeedLimit | true |
  
    Wanneer de [returnSpeedLimit](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse) queryparameter is ingesteld, het antwoord geretourneerd van de limiet voor geboekte snelheid.

7. Voeg de volgende sleutel / waarde-paar aan de **Params** sectie en klikt u op **verzenden**:

    | Sleutel | Value |
    |-----|------------|
    | returnRoadUse | true |

    Wanneer de [returnRoadUse](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse) queryparameter is ingesteld, wordt het antwoord retourneert de matrix voor het gebruik van weg voor omgekeerde geocodes op straat niveau.

8. Voeg de volgende sleutel / waarde-paar aan de **Params** sectie en klikt u op **verzenden**:

    | Sleutel | Value |
    |-----|------------|
    | roadUse | true |

    U kunt de geocode omgekeerde query beperken tot een specifiek type weg gebruiken met behulp van de [roadUse](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse) queryparameter.
  
## <a name="search-for-the-cross-street-using-reverse-address-cross-street-search"></a>Zoek de cross straat met omgekeerde adres Cross straat zoeken

1. In Postman, klikt u op **nieuwe aanvraag** | **GET-aanvraag** en noem het **omgekeerde adres Cross-adres zoeken**.

2. Selecteer op het tabblad Builder de **ophalen** HTTP-methode en voer de aanvraag-URL voor uw API-eindpunt.
  
    ![Reverse-adres Cross-adres zoeken](./media/how-to-search-for-address/reverse_address_search_url.png)
  
    | Parameter | Voorgestelde waarde |
    |---------------|------------------------------------------------|
    | HTTP-methode | GET |
    | Aanvraag-URL | [https://atlas.microsoft.com/search/address/reverse/crossstreet/json?](https://atlas.microsoft.com/search/address/reverse/crossstreet/json?) |
    | Autorisatie | Geen autorisatie |
  
3. Klik op **Params**, en voer de volgende sleutel / waarde-paren te gebruiken als parameters query of het pad in de aanvraag-URL:
  
    | Sleutel | Value |
    |------------------|-------------------------|
    | API-versie | 1.0 |
    | abonnement-sleutel | \<de sleutel van uw Azure-kaarten\> |
    | query | 47.591180,-122.332700 |
  
4. Klik op **verzenden** en bekijk de antwoordtekst.

## <a name="next-steps"></a>Volgende stappen

- Verken de [search-service van Azure Maps](https://docs.microsoft.com/rest/api/maps/search) API-documentatie.
