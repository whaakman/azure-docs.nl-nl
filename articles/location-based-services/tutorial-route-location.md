---
title: Zoeken naar route met de Services van Azure-locatie op basis van | Microsoft Docs
description: Routeren naar een locatie op basis van Azure-Services met behulp van
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
ms.openlocfilehash: f2be9ca98330866ac8b6fb12efd56efdc711eedf
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2017
---
# <a name="route-to-a-point-of-interest-using-azure-location-based-services"></a>Routeren naar een locatie op basis van Azure-Services met behulp van

Deze zelfstudie laat zien hoe uw locatie op basis van Azure-Services-account en de Route-SDK, gebruiken om de route naar interessante vinden. In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Adres coördinaten ophalen
> * Query uitvoeren op Route-Service om te verwijzen van belang voor instructies

## <a name="prerequisites"></a>Vereisten

Voordat u doorgaat, zorg ervoor dat [uw locatie op basis van Azure-Services-account maken](./tutorial-search-location.md#createaccount), en [ophalen van de sleutel van het abonnement voor uw account](./tutorial-search-location.md#getkey). U merkt ook het gebruik van het Kaartbesturingselement en zoeken-API's voor Service, zoals beschreven in de zelfstudie [zoeken in de buurt interessante met behulp van Azure op basis van Locatieservices](./tutorial-search-location.md).


<a id="getcoordinates"></a>

## <a name="get-address-coordinates"></a>Adres coördinaten ophalen

Gebruik de volgende stappen voor het maken van een statische HTML-pagina in de locatie op basis van Services toewijzen besturingselement API is ingesloten. 

1. Maak een nieuw bestand op uw lokale machine en noem deze **MapRoute.html**. 
2. De volgende HTML-onderdelen toevoegen aan het bestand:

    ```HTML
    <!DOCTYPE html>
    <html lang="en">

    <head>
        <meta charset="utf-8" />
        <meta name="viewport" content="width=device-width, user-scalable=no" />
        <title>Map Route</title>
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
    Houd er rekening mee hoe de resource-locaties voor CSS en JavaScript-bestanden voor de locatie op basis van Azure-Services-bibliotheek in de HTML-header worden ingesloten. U ziet ook de *script* segment in de hoofdtekst van het HTML-bestand, waarin u de inline JavaScript-code voor toegang tot Azure locatie op basis van de Service-API's.

3. Voeg de volgende JavaScript-code naar de *script* blok van het HTML-bestand. Vervang de tijdelijke aanduiding *< insert-sleutel >* met primaire sleutel van uw Services op basis van locatie-account.

    ```JavaScript
    // Instantiate map to the div with id "map"
    var subscriptionKey = "<insert-key>";
    var map = new atlas.Map("map", {
        "subscription-key": subscriptionKey
    });
    ```
    De **atlas. Kaart** biedt het besturingselement voor een kaart visual en interactieve webpagina en is een onderdeel van de Azure-API voor het beheer van kaart.

4. Voeg de volgende JavaScript-code naar de *script* blok. Hiermee voegt u een laag van *multipoint* voor het Kaartbesturingselement om weer te geven van de route:

    ```JavaScript
    // Initialize the linestring layer for routes on the map
    var routeLinesLayerName = "routes";
    map.addLinestrings([], {
        name: routeLinesLayerName,
        color: "#2272B9",
        width: 5,
        cap: "round",
        join: "round",
        before: "labels"
    });
    ```

5. Voeg de volgende JavaScript-code voor het maken van de begin- en eindpunten voor de route:

    ```JavaScript
    // Create the GeoJSON objects which represent the start and end point of the route
    var startPoint = new atlas.data.Point([-122.130137, 47.644702]);
    var startPin = new atlas.data.Feature(startPoint, {
        title: "Microsoft",
        icon: "pin-round-blue"
    });

    var destinationPoint = new atlas.data.Point([-122.3352, 47.61397]);
    var destinationPin = new atlas.data.Feature(destinationPoint, {
        title: "Contoso Oil & Gas",
        icon: "pin-blue"
    });
    ```
    Deze code maakt u twee [GeoJSON objecten](https://en.wikipedia.org/wiki/GeoJSON) vertegenwoordigt de begin- en eindpunten van de route. Het eindpunt is de combinatie breedtegraad/lengtegraad voor een van de *benzine stations* gezocht in de vorige zelfstudie [zoeken in de buurt interessante met behulp van Azure op basis van Locatieservices](./tutorial-search-location.md).

6. Voeg de volgende JavaScript-code voor de pincodes voor de begin- en toevoegen aan de kaart:

    ```JavaScript
    // Fit the map window to the bounding box defined by the start and destination points
    var swLon = Math.min(startPoint.coordinates[0], destinationPoint.coordinates[0]);
    var swLat = Math.min(startPoint.coordinates[1], destinationPoint.coordinates[1]);
    var neLon = Math.max(startPoint.coordinates[0], destinationPoint.coordinates[0]);
    var neLat = Math.max(startPoint.coordinates[1], destinationPoint.coordinates[1]);
    map.setCameraBounds({
        bounds: [swLon, swLat, neLon, neLat],
        padding: 50
    });

    // Add pins to the map for the start and end point of the route
    map.addPins([startPin, destinationPin], {
        name: "route-pins",
        textFont: "SegoeUi-Regular",
        textOffset: [0, -20]
    });
    ``` 
    De API **map.setCameraBounds** past u het venster volgens de coördinaten van de begin- en eindpunten hebben. De API **map.addPins** voegt de punten toe aan het kaartbesturingselement als visuele onderdelen.

7. Sla de **MapRoute.html** bestand op uw computer. 

<a id="getroute"></a>

## <a name="query-route-service-for-directions-to-point-of-interest"></a>Query uitvoeren op Route-Service om te verwijzen van belang voor instructies

Deze sectie wordt beschreven hoe de Azure Locatieservices op basis van de Route Service API gebruiken om te zoeken naar de route van een bepaalde beginpunt naar een bestemming. De Route-Service biedt de snelste plannen kortste-API's of systeem route tussen twee locaties, met inbegrip van de voorwaarden Realtime netwerkverkeer. Ook kunnen gebruikers in de toekomst routes met behulp van Azure uitgebreide historische verkeer database en het voorspellen van de duur van de route voor elke dag en tijd plannen. 

1. Open de **MapRoute.html** bestand in de voorgaande sectie hebt gemaakt en voeg de volgende JavaScript-code naar de *script* blok ter illustratie van de Route-Service.

    ```JavaScript
    // Perform a request to the route service and draw the resulting route on the map
    var xhttp = new XMLHttpRequest();
    xhttp.onreadystatechange = function () {
        if (this.readyState == 4 && this.status == 200) {
            var response = JSON.parse(xhttp.responseText);

            var route = response.routes[0];
            var routeCoordinates = [];
            for (var leg of route.legs) {
                var legCoordinates = leg.points.map((point) => [point.longitude, point.latitude]);
                routeCoordinates = routeCoordinates.concat(legCoordinates);
            }

            var routeLinestring = new atlas.data.LineString(routeCoordinates);
            map.addLinestrings([new atlas.data.Feature(routeLinestring)], { name: routeLinesLayerName });
        }
    };
    ```
    Dit codefragment maakt een [XMLHttpRequest](https://xhr.spec.whatwg.org/), en voegt u een gebeurtenis-handler voor het parseren van het binnenkomende antwoord. Voor een geslaagde reactie vormt het een matrix van coördinaten voor een regel segmenten van de eerste route geretourneerd. Vervolgens wordt deze reeks coördinaten voor deze route toegevoegd aan de kaart *multipoint* laag.

2. Voeg de volgende code naar de *script* blok, de XMLHttpRequest verzenden naar de locatie op basis van de Route Service van Azure Services:

    ```JavaScript
    var url = "https://atlas.microsoft.com/route/directions/json?";
    url += "&api-version=1.0";
    url += "&subscription-key=" + subscriptionKey;
    url += "&query=" + startPoint.coordinates[1] + "," + startPoint.coordinates[0] + ":" +
        destinationPoint.coordinates[1] + "," + destinationPoint.coordinates[0];

    xhttp.open("GET", url, true);
    xhttp.send();
    ```
    De bovenstaande aanvraag bevat de vereiste parameters die abonnementssleutel voor uw account en de coördinaten voor de begin- en, in de opgegeven volgorde. 

3. Sla de **MapRoute.html** lokaal bestand vervolgens opent u het in een webbrowser naar keuze en bekijk het resultaat. Voor een succesvolle verbinding met de locatie op basis van Services-API's ziet u een vergelijkbaar met de volgende kaart. 

    ![Azure Kaartbesturingselement en Route-Service](./media/tutorial-route-location/lbs-map-route.png)


## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Adres coördinaten ophalen
> * Query uitvoeren op Route-Service om te verwijzen van belang voor instructies

Ga door naar de zelfstudie [routes vinden voor verschillende modi van reizen met behulp van Azure op basis van Locatieservices](./tutorial-prioritized-routes.md) om informatie over het gebruik van de Azure-Services voor het op basis van locatie om de prioriteit van de routes naar interessante, op basis van de modus van transport. 
