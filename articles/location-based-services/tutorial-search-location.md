---
title: Zoeken met de Azure-locatie op basis van Services | Microsoft Docs
description: Zoeken in de buurt interessante met Azure locatie op basis van Services
services: location-based-services
keywords: 
author: dsk-2015
ms.author: dkshir
ms.date: 11/28/2017
ms.topic: tutorial
ms.service: location-based-services
documentationcenter: 
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 15afdead60d4c1ee3c7e3c079d43e0651b262ec8
ms.sourcegitcommit: 310748b6d66dc0445e682c8c904ae4c71352fef2
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2017
---
# <a name="search-nearby-point-of-interest-using-azure-location-based-services"></a>Zoeken in de buurt interessante met Azure locatie op basis van Services

Deze zelfstudie laat zien hoe een account met Azure op basis van Locatieservices instellen en vervolgens de opgegeven API's gebruiken om te zoeken naar een punt van belang. In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Maak een account met Azure locatie op basis van Services
> * De abonnementssleutel voor het voor uw account ophalen
> * Nieuwe webpagina met kaart beheer-API maken
> * Search-Service gebruiken om te zoeken in de buurt interessante

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/) aan voordat u begint.

# <a name="log-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal
Meld u aan bij [Azure Portal](https://portal.azure.com).

<a id="createaccount"></a>

## <a name="create-an-account-with-azure-location-based-services"></a>Maak een account met Azure locatie op basis van Services

Volg deze stappen om een nieuwe locatie op basis van Services-account maken.

1. In de linkerbovenhoek van de [Azure-portal](https://portal.azure.com), klikt u op **maken van een resource**.
2. In de *zoeken van de Marketplace* in het vak **services op basis van locatie**.
3. Van de *resultaten*, klikt u op de **op basis van Locatieservices (preview)**. Klik op **maken** knop die wordt weergegeven onder de kaart. 
4. Op de **locatie op basis van Services-Account maken** pagina, voer de volgende waarden:
    - De *naam* van uw nieuwe account. 
    - De *abonnement* die u wilt gebruiken voor dit account.
    - De *resourcegroep* naam in voor dit account. U kunt *nieuw* of *gebruik bestaande* resourcegroep.
    - Selecteer de *locatie voor resourcegroep*.
    - Lees de *Preview-voorwaarden* en schakel het selectievakje in om de voorwaarden accepteren. 
    - Ten slotte op de **maken** knop.
   
    ![Op basis van locatie-Services-account maken in portal](./media/tutorial-search-location/create-lbs-account.png)


<a id="getkey"></a>

## <a name="get-the-subscription-key-for-your-account"></a>De abonnementssleutel voor het voor uw account ophalen

Als uw locatie op basis van Services-account is gemaakt, volg u de stappen om een koppeling naar de map search API's:

1. Open uw account op basis van locatie-Services in de portal.
2. Navigeer naar uw account **instellingen**, en selecteer vervolgens **sleutels**.
3. Kopieer de **primaire sleutel** naar het Klembord. Sla het lokaal moet worden gebruikt in de stappen voor u doorgaat. 

    ![Primaire sleutel in de portal ophalen](./media/tutorial-search-location/lbs-get-key.png)


<a id="createmap"></a>

## <a name="create-new-web-page-using-azure-map-control-api"></a>Nieuwe webpagina met Azure kaart beheer-API maken
Azure kaart beheer-API is een handige clientbibliotheek waarmee u eenvoudig Azure locatie op basis van Services in uw webtoepassing te integreren. Het verbergen van de complexiteit van het aanroepen van de bare REST-service en verhoogt de productiviteit met styleable en aanpasbare onderdelen. De volgende stappen ziet u het maken van een statische HTML-pagina in de locatie op basis van Services toewijzen besturingselement API is ingesloten. 

1. Maak een nieuw bestand op uw lokale machine en noem deze **MapSearch.html**. 
2. De volgende HTML-onderdelen toevoegen aan het bestand:

    ```HTML
    <!DOCTYPE html>
    <html lang="en">

    <head>
        <meta charset="utf-8" />
        <meta name="viewport" content="width=device-width, user-scalable=no" />
        <title>Map Search</title>

        <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/css/atlas.min.css?api-version=1.0" type="text/css" />
        <script src="https://atlas.microsoft.com/sdk/js/atlas.min.js?api-version=1.0"></script>

        <style>
            html,
            body {
                width: 100%;
                height: 100%;
                padding: 0;
                margin: 0;
            }

            #map {
                width: 100%;
                height: 100%;
            }
        </style>
    </head>
    <body>
        <div id="map"></div>
        <script>
        // Embed Map Control JavaScript code here
        </script>

    </body>

    </html>
    ``` 
    U ziet dat de HTML-header de bronbestanden CSS- en JavaScript gehost door de Azure-Kaartbesturingselement-bibliotheek bevat. Opmerking de *script* segment toegevoegd aan de *hoofdtekst* van het HTML-bestand. Dit segment bevat de inline JavaScript-code voor toegang tot Azure locatie op basis van de Service-API's.
 
3.  Voeg de volgende JavaScript-code naar de *script* blok van het HTML-bestand. Vervang de tijdelijke aanduiding *< insert-sleutel >* met primaire sleutel van uw Services op basis van locatie-account. 

    ```HTML/JavaScript
            // Instantiate map to the div with id "map"
            var subscriptionKey = "<insert-key>";
            var map = new atlas.Map("map", {
                "subscription-key": subscriptionKey
            });

    ```
    Dit segment initieert de kaart beheer-API voor uw abonnement. **Atlas** is de naamruimte waarin de besturingselement-API van Azure-Map en gerelateerde visuele onderdelen. **atlas. Kaart** biedt van het besturingselement voor een visuele en interactieve webpagina-kaart. U kunt zien hoe de kaart eruit door het openen van de HTML-pagina in de browser. 

4. Voeg de volgende JavaScript-code naar de *script* blok, een laag van search pincodes toevoegen aan het Kaartbesturingselement:

    ```HTML/JavaScript
            // Initialize the pin layer for search results to the map
            var searchLayerName = "search-results";
            map.addPins([], {
                name: searchLayerName,
                cluster: false,
                icon: "pin-round-darkblue"
            });
    ```

5. Sla het bestand op uw computer. 


<a id="usesearch"></a>

## <a name="use-search-service-to-find-nearby-point-of-interest"></a>Search-Service gebruiken om te zoeken in de buurt interessante

Deze sectie wordt beschreven hoe u met de Azure Locatieservices op basis van de API van zoekservice een punt van belang vinden op de kaart. Het is een RESTful-API die zijn bestemd voor ontwikkelaars om te zoeken naar adressen, wetenswaardigheden en andere geografische informatie. De Search-Service toewijst een breedtegraad en lengtegraad informatie aan een opgegeven adres. 

1. Open de **MapSearch.html** bestand in de voorgaande sectie hebt gemaakt en voeg de volgende JavaScript-code naar de *script* blok ter illustratie van de Search-Service. 
    ```HTML/JavaScript
            // Perform a request to the search service and create a pin on the map for each result
            var xhttp = new XMLHttpRequest();
            xhttp.onreadystatechange = function () {
                var searchPins = [];

                if (this.readyState === 4 && this.status === 200) {
                    var response = JSON.parse(this.responseText);

                    var poiResults = response.results.filter((result) => { return result.type === "POI" }) || [];

                    searchPins = poiResults.map((poiResult) => {
                        var poiPosition = [poiResult.position.lon, poiResult.position.lat];
                        return new atlas.data.Feature(new atlas.data.Point(poiPosition), {
                            name: poiResult.poi.name,
                            address: poiResult.address.freeformAddress,
                            position: poiResult.position.lat + ", " + poiResult.position.lon
                        });
                    });

                    map.addPins(searchPins, {
                        name: searchLayerName
                    });

                    var lons = searchPins.map((pin) => { return pin.geometry.coordinates[0] });
                    var lats = searchPins.map((pin) => { return pin.geometry.coordinates[1] });

                    var swLon = Math.min.apply(null, lons);
                    var swLat = Math.min.apply(null, lats);
                    var neLon = Math.max.apply(null, lons);
                    var neLat = Math.max.apply(null, lats);

                    map.setCameraBounds({
                        bounds: [swLon, swLat, neLon, neLat],
                        padding: 50
                    });
                }
            };
    ```
    Dit codefragment maakt een [XMLHttpRequest](https://xhr.spec.whatwg.org/), en voegt u een gebeurtenis-handler voor het parseren van het binnenkomende antwoord. Voor een geslaagde reactie deze verzamelt de adressen, namen, breedtegraad en logitude informatie voor elke locatie die is geretourneerd, in de `searchPins` variabele. Ten slotte wordt deze verzameling van locatie verwijst naar de `map` besturingselement als pincodes. 

2. Voeg de volgende code naar de *script* blok, de XMLHttpRequest verzenden naar de locatie op basis van de zoekopdracht Service van Azure Services:

    ```HTML/JavaScript
            var url = "https://atlas.microsoft.com/search/fuzzy/json?";
            url += "&api-version=1.0";
            url += "&query=gasoline%20station";
            url += "&subscription-key=" + subscriptionKey;
            url += "&lat=47.6292";
            url += "&lon=-122.2337";
            url += "&radius=100000"

            xhttp.open("GET", url, true);
            xhttp.send();
    ``` 
    In dit fragment gebruikmaakt van de basic search API van de zoekservice aangeroepen de **Fuzzy zoeken**. Verwerkt de meest fuzzy invoer behandeling van elke combinatie van-adres of *POI* tokens. Hij zoekt naar de in de buurt **benzine station**, voor het opgegeven adres in breedtegraad en lengtegraad en binnen de opgegeven radius. Abonnementssleutel voor uw account eerder opgegeven in het voorbeeldbestand wordt gebruikt voor het gesprek tot de Services op basis van locatie. Deze retourneert het resultaat als breedtegraad lengtegraad/waardeparen voor de locaties die zijn gevonden. U mag de pincodes zoeken zien door het openen van de HTML-pagina in de browser. 

3. Voeg de volgende regels voor de *script* blok pop-ups voor de punten van belang zijn geretourneerd door de Search-Service maken:

    ```HTML/JavaScript
            // Add a popup to the map which will display some basic information about a search result on hover over a pin
            var popup = new atlas.Popup();
            map.addEventListener("mouseover", searchLayerName, (e) => {
                var popupContentElement = document.createElement("div");
                popupContentElement.style.padding = "5px";

                var popupNameElement = document.createElement("div");
                popupNameElement.innerText = e.features[0].properties.name;
                popupContentElement.appendChild(popupNameElement);

                var popupAddressElement = document.createElement("div");
                popupAddressElement.innerText = e.features[0].properties.address;
                popupContentElement.appendChild(popupAddressElement);

                var popupPositionElement = document.createElement("div");
                popupPositionElement.innerText = e.features[0].properties.position;
                popupContentElement.appendChild(popupPositionElement);

                popup.setPopupOptions({
                    position: e.features[0].geometry.coordinates,
                    content: popupContentElement
                });

                popup.open(map);
            });
    ```
    De API **atlas. Pop-upvenster** voorziet in een venster verankerd op de juiste positie op de kaart. Dit codefragment Hiermee stelt u de inhoud en de positie voor het pop-upvenster, evenals een gebeurtenislistener aan voegt de `map` besturingselement, wacht de _muis_ herstellen via de pop-up. 

4. Sla het bestand en open vervolgens de **MapSearch.html** bestand in een webbrowser naar keuze en bekijk het resultaat. Op dit moment ziet de kaart in de browser informatie pop-ups wanneer u de muisaanwijzer op een van de zoekopdracht pincodes dat wordt weergegeven, vergelijkbaar met het volgende. 

    ![Azure-Map Control en Service voor zoeken in](./media/tutorial-search-location/lbs-map-search.png)


## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Maak een account met Azure locatie op basis van Services
> * De abonnementssleutel voor het voor uw account ophalen
> * Nieuwe webpagina met kaart beheer-API maken
> * Search-Service gebruiken om te zoeken in de buurt interessante

Ga door naar de zelfstudie [Route naar een locatie op basis van Azure-Services met behulp van](./tutorial-route-location.md) voor meer informatie over het gebruik van de Azure-Services voor het op basis van locatie voor het routeren naar interessante. 
