---
title: Overzicht van Azure Location Based Services | Microsoft Docs
description: Route naar een nuttige plaats op basis van Azure Location Based Services
services: location-based-services
keywords: 
author: kgremban
ms.author: kgremban
ms.date: 11/28/2017
ms.topic: tutorial
ms.service: location-based-services
documentationcenter: 
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: b54d97afb4e762a7d252acf7cf53a5deac01d43f
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/08/2018
---
# <a name="route-to-a-point-of-interest-using-azure-location-based-services"></a>Route naar een nuttige plaats op basis van Azure Location Based Services

Deze zelfstudie laat zien hoe u uw Azure Location Based Services-account en de Route Service SDK gebruikt om de route naar uw nuttige plaats te vinden. In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Adrescoördinaten ophalen
> * Query uitvoeren op Route Service voor route naar nuttige plaats

## <a name="prerequisites"></a>Vereisten

Voordat u doorgaat, maakt u [uw Azure Location Based Services-account ](./tutorial-search-location.md#createaccount) en [zorgt u dat u de abonnementssleutel voor uw account verkrijgt](./tutorial-search-location.md#getkey). U merkt ook hoe de Map Control en Search Service API's moet gebruiken, zoals beschreven in de zelfstudie [Zoeken naar nuttige plaats in de buurt met behulp van Azure Location Based Services](./tutorial-search-location.md).


<a id="getcoordinates"></a>

## <a name="get-address-coordinates"></a>Adrescoördinaten ophalen

Gebruik de volgende stappen voor het maken van een statische HTML-pagina, ingesloten met de Map Control API van Location Based Services. 

1. Maak een nieuw bestand op uw lokale computer en noem dit **MapRoute.html**. 
2. Voeg de volgende HTML-onderdelen toe aan het bestand:

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
    Let op dat de resource-locaties voor CSS en JavaScript-bestanden voor de locatie op basis van Azure Location Based Services-bibliotheek in de HTML-header worden ingesloten. U ziet ook het *script*-segment in de hoofdtekst van het HTML-bestand, dat de inline JavaScript-code voor toegang tot de API’s van Azure Location Based Service bevat.

3. Voeg de volgende JavaScript-code toe aan het blok *script* van het HTML-bestand. Gebruik de primaire sleutel van uw Location Based Services-account in het script.

    ```JavaScript
    // Instantiate map to the div with id "map"
    var LBSAccountKey = "<_your account key_>";
    var map = new atlas.Map("map", {
        "subscription-key": LBSAccountKey
    });
    ```
    De **atlas.Map** biedt het besturingselement voor een visuele en interactieve webkaart en is een onderdeel van de Azure Map Control API.

4. Voeg de volgende JavaScript-code toe aan het blok *script*. Hiermee voegt u een laag van *linestrings* toe aan Map Control om de route weer te geven:

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

5. Voeg de volgende JavaScript-code toe voor het maken van de begin- en eindpunten voor de route:

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
    Deze code maakt twee [GeoJSON-objecten](https://en.wikipedia.org/wiki/GeoJSON) die de begin- en eindpunten van de route vertegenwoordigen. Het eindpunt is de combinatie breedtegraad/lengtegraad voor een van de *benzinestations* die werden gezocht in de vorige zelfstudie [Zoeken naar nuttige plaatsen in de buurt met behulp van Azure Location Based Services](./tutorial-search-location.md).

6. Voeg de volgende JavaScript-code toe om de pins voor de begin- en eindpunten aan de kaart toe te voegen:

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
    De API **map.setCameraBounds** past het kaartenvenster aan naar de coördinaten van de begin- en eindpunten. De API **map.addPins** voegt de punten als visuele onderdelen toe aan het kaartbesturingselement.

7. Sla het bestand **MapRoute.html** op uw computer op. 

<a id="getroute"></a>

## <a name="query-route-service-for-directions-to-point-of-interest"></a>Query uitvoeren op Route Service voor route naar nuttige plaats

Deze sectie beschrijft hoe u de Route Service API van Azure Location Based Services gebruikt om te zoeken naar de route van een bepaald beginpunt naar een bestemming. De Route Service biedt API’s om de snelste, kortste of zuinigste route tussen twee locaties te plannen, rekening houdend met real-time verkeerscondities. Ook kunnen gebruikers in de toekomst routes plannen met behulp van de uitgebreide historische verkeersdatabase van Azure en de duur van de route voor elke dag en tijd voorspellen. 

1. Open het bestand **MapRoute.html** dat in de voorgaande sectie is gemaakt en voeg de volgende JavaScript-code toe aan het blok *script* ter illustratie van de Route Service.

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
    Dit codefragment maakt een [XMLHttpRequest](https://xhr.spec.whatwg.org/) en voegt een gebeurtenis-handler toe voor het parseren van de binnenkomende respons. Voor een geslaagde respons vormt het een matrix van coördinaten voor lijnsegmenten van de eerste route die geretourneerd worden. Vervolgens voegt het deze reeks coördinaten aan deze route toe aan de *linestrings*-laag van de kaart.

2. Voeg de volgende code toe aan het blok *script* om de XMLHttpRequest te verzenden naar Route Service van Azure Location Based Services:

    ```JavaScript
    var url = "https://atlas.microsoft.com/route/directions/json?";
    url += "&api-version=1.0";
    url += "&subscription-key=" + LBSAccountKey;
    url += "&query=" + startPoint.coordinates[1] + "," + startPoint.coordinates[0] + ":" +
        destinationPoint.coordinates[1] + "," + destinationPoint.coordinates[0];

    xhttp.open("GET", url, true);
    xhttp.send();
    ```
    De bovenstaande aanvraag bevat de vereiste parameters die de sleutel van uw account en de coördinaten voor de begin- en eindpunten zijn, in de opgegeven volgorde. 

3. Sla het bestand **MapRoute.html** lokaal op, open het vervolgens in een webbrowser naar keuze en bekijk het resultaat. Voor een succesvolle verbinding met de Location Based Services API's bekijkt u een kaart die vergelijkbaar is met de volgende. 

    ![Azure Map Control en Route Service](./media/tutorial-route-location/lbs-map-route.png)


## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Adrescoördinaten ophalen
> * Query uitvoeren op Route Service voor route naar nuttige plaats

Ga door naar de zelfstudie [Routes vinden voor verschillende voertuigen met behulp van Azure Location Based Services](./tutorial-prioritized-routes.md) om te leren hoe u de Azure Location Based Services gebruikt om routes naar nuttige plaatsen te rangschikken, op basis van de manier van reizen. 
