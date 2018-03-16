---
title: Meerdere routes met Azure Location Based Services | Microsoft Docs
description: Routes vinden voor verschillende manieren van reizen met Azure Location Based Services
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
ms.openlocfilehash: 986e8667ca421baa78647e77e43fef92d81a7982
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/08/2018
---
# <a name="find-routes-for-different-modes-of-travel-using-azure-location-based-services"></a>Routes vinden voor verschillende manieren van reizen met Azure Location Based Services

Deze zelfstudie laat zien hoe u uw Azure Location Based Services-account en de Route Service-SDK gebruikt om de route naar uw nuttige plaats te vinden, gerangschikt naar uw manier van reizen. In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Uw query aan Route Service configureren
> * Routes weergeven gerangschikt naar de manier van reizen

## <a name="prerequisites"></a>Vereisten

Voordat u doorgaat, zorgt u ervoor dat u [uw Azure Location Based Services-account maakt](./tutorial-search-location.md#createaccount), en [de sleutel van uw account verkrijgt](./tutorial-search-location.md#getkey). U kunt ook leren hoe u de Map Control en Search Service API's gebruikt, zoals beschreven in de zelfstudie [Zoeken naar nuttige plaats in de buurt met behulp van Azure Location Based Services](./tutorial-search-location.md), evenals informatie over het basisgebruik van de Route Service API's, zoals beschreven in de zelfstudie [Route naar een locatie met behulp van Azure Location Based Services](./tutorial-route-location.md).


<a id="queryroutes"></a>

## <a name="configure-your-route-service-query"></a>Uw query aan Route Service configureren

Gebruik de volgende stappen voor het maken van een statische HTML-pagina, ingesloten met de Map Control API van Location Based Services. 

1. Maak een nieuw bestand op uw lokale computer en noem dit **MapTruckRoute.html**. 
2. Voeg de volgende HTML-onderdelen toe aan het bestand:

    ```HTML
    <!DOCTYPE html>
    <html lang="en">

    <head>
        <meta charset="utf-8" />
        <meta name="viewport" content="width=device-width, user-scalable=no" />
        <title>Map Truck Route</title>
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
    Let op dat de resource-locaties voor CSS en JavaScript-bestanden voor de locatie op basis van Azure Location Based Services-bibliotheek in de HTML-header worden ingesloten. U ziet ook het *script*-segment in de hoofdtekst van het HTML-bestand, dat de inline JavaScript-code voor toegang tot de API’s van Azure Map Control bevat.
3. Voeg de volgende JavaScript-code toe aan het blok *script* van het HTML-bestand. Vervang de tijdelijke aanduiding *< insert-key>* met de primaire sleutel van uw Location Based Services-account.

    ```JavaScript
    // Instantiate map to the div with id "map"
    var LBSAccountKey = "<_your account key_>";
    var map = new atlas.Map("map", {
        "subscription-key": LBSAccountKey
    });
    ```
    De **atlas.Map** biedt het besturingselement voor een visuele en interactieve webkaart en is een onderdeel van de Azure Map Control API.

4. Voeg de volgende JavaScript-code toe aan het blok *script*, om de weergave van de verkeersstroom toe te voegen aan de kaart:

    ```JavaScript
    // Add Traffic Flow to the Map
    map.setTraffic({
        flow: "relative"
    });
    ```
    Deze code stelt de verkeersstroom in op `relative`, dit is de snelheid van de weg ten opzichte van een vrije stroom. U kunt deze ook instellen op `absolute` snelheid van de weg of `relative-delay` de relatieve snelheid weergeven, waar deze verschilt van een vrije stroom. 

5. Voeg de volgende JavaScript-code toe om de pins te maken voor de begin- en eindpunten van de route:

    ```JavaScript
    // Create the GeoJSON objects which represent the start and end point of the route
    var startPoint = new atlas.data.Point([-122.356099, 47.580045]);
    var startPin = new atlas.data.Feature(startPoint, {
        title: "Fabrikam, Inc.",
        icon: "pin-round-blue"
    });

    var destinationPoint = new atlas.data.Point([-122.130137, 47.644702]);
    var destinationPin = new atlas.data.Feature(destinationPoint, {
        title: "Microsoft",
        icon: "pin-blue"
    });
    ```
    Deze code maakt twee [GeoJSON-objecten](https://en.wikipedia.org/wiki/GeoJSON) die de begin- en eindpunten van de route vertegenwoordigen. 

6. Voeg de volgende JavaScript-code toe om lagen van *linestrings* toe te voegen voor Map Control, om routes weer te geven op basis van het soort vervoer, bijvoorbeeld _auto_ of _vrachtwagen_.

    ```JavaScript
    // Place route layers on the map
    var carRouteLayerName = "car-route";
    map.addLinestrings([], {
        name: carRouteLayerName,
        color: "#B76DAB",
        width: 5,
        cap: "round",
        join: "round",
        before: "labels"
    });

    var truckRouteLayerName = "truck-route";
    map.addLinestrings([], {
        name: truckRouteLayerName,
        color: "#2272B9",
        width: 9,
        cap: "round",
        join: "round",
        before: carRouteLayerName
    });
    ```

7. Voeg de volgende JavaScript-code toe om de begin- en eindpunten aan de kaart toe te voegen:

    ```JavaScript
    // Fit the map window to the bounding box defined by the start and destination points
    var swLon = Math.min(startPoint.coordinates[0], destinationPoint.coordinates[0]);
    var swLat = Math.min(startPoint.coordinates[1], destinationPoint.coordinates[1]);
    var neLon = Math.max(startPoint.coordinates[0], destinationPoint.coordinates[0]);
    var neLat = Math.max(startPoint.coordinates[1], destinationPoint.coordinates[1]);
    map.setCameraBounds({
        bounds: [swLon, swLat, neLon, neLat],
        padding: 100
    });

    // Add pins to the map for the start and end point of the route
    map.addPins([startPin, destinationPin], {
        name: "route-pins",
        textFont: "SegoeUi-Regular",
        textOffset: [0, -20]
    });
    ``` 
    De API **map.setCameraBounds** past het kaartenvenster aan naar de coördinaten van de begin- en eindpunten. De API **map.addPins** voegt de punten als visuele onderdelen toe aan het kaartbesturingselement.

8. Sla het bestand **MapTruckRoute.html** op uw computer op. 

<a id="multipleroutes"></a>

## <a name="render-routes-prioritized-by-mode-of-travel"></a>Routes weergeven gerangschikt naar de manier van reizen

Deze sectie beschrijft hoe u de Route Service API van Azure Location Based Services gebruikt om te zoeken naar meerdere routes van een bepaald beginpunt naar een bestemming op basis van het soort vervoer. De Route Service biedt API’s om de snelste, kortste of zuinigste route tussen twee locaties te plannen, rekening houdend met real-time verkeerscondities. Ook kunnen gebruikers in de toekomst routes plannen met behulp van de uitgebreide historische verkeersdatabase van Azure en de duur van de route voor elke dag en tijd voorspellen. 

1. Open het bestand **MapTruckRoute.html** dat in de voorgaande sectie is gemaakt en voeg de volgende JavaScript-code toe aan het blok*script* om de route voor een vrachtwagen met de Route Service te verkrijgen.

    ```JavaScript
    // Perform a request to the route service and draw the resulting truck route on the map
    var xhttpTruck = new XMLHttpRequest();
    xhttpTruck.onreadystatechange = function () {
        if (this.readyState == 4 && this.status == 200) {
            var response = JSON.parse(this.responseText);

            var route = response.routes[0];
            var routeCoordinates = [];
            for (var leg of route.legs) {
                var legCoordinates = leg.points.map((point) => [point.longitude, point.latitude]);
                routeCoordinates = routeCoordinates.concat(legCoordinates);
            }

            var routeLinestring = new atlas.data.LineString(routeCoordinates);
            map.addLinestrings([new atlas.data.Feature(routeLinestring)], {
                name: truckRouteLayerName
            });
        }
    };

    var truckRouteUrl = "https://atlas.microsoft.com/route/directions/json?";
    truckRouteUrl += "&api-version=1.0";
    truckRouteUrl += "&subscription-key=" + LBSAccountKey;
    truckRouteUrl += "&query=" + startPoint.coordinates[1] + "," + startPoint.coordinates[0] + ":" +
        destinationPoint.coordinates[1] + "," + destinationPoint.coordinates[0];
    truckRouteUrl += "&travelMode=truck";
    truckRouteUrl += "&vehicleWidth=2";
    truckRouteUrl += "&vehicleHeight=2";
    truckRouteUrl += "&vehicleLength=5";
    truckRouteUrl += "&vehicleLoadType=USHazmatClass2";

    xhttpTruck.open("GET", truckRouteUrl, true);
    xhttpTruck.send();
    ```
    Dit codefragment maakt een [XMLHttpRequest](https://xhr.spec.whatwg.org/) en voegt een gebeurtenis-handler toe voor het parseren van de binnenkomende respons. Voor een geslaagde respons wordt een matrix van coördinaten gemaakt voor de route die is geretourneerd, en voegt u deze toe aan de laag `truckRouteLayerName` van de kaart. 
    
    Dit codefragment verzendt ook de query naar de Route Service, om de route op te halen voor het opgegeven begin- en eindpunt, voor uw accountsleutel. De volgende optionele parameters worden gebruikt om de route voor een zware vrachtwagen aan te geven:
   - De parameter `travelMode=truck` geeft de manier van reizen op als *vrachtwagen*. Andere ondersteunde manieren van reizen zijn *taxi*, *bus*, *bestelwagen*, *motorfiets* en de standaardinstelling *auto*.
   - De parameters `vehicleWidth`, `vehicleHeight` en `vehicleLength` specificeren de afmetingen van het voertuig in meters en worden alleen beschouwd als de manier van reizen *vrachtwagen* is.
   - De `vehicleLoadType` classificeert de lading als gevaarlijk en beperkt op sommige wegen. Dit wordt momenteel ook alleen beschouwd voor de modus *vrachtwagen*.

2. Voeg de volgende JavaScript-code toe voor de route voor een auto met behulp van de Route Service:

    ```JavaScript
    // Perform a request to the route service and draw the resulting car route on the map
    var xhttpCar = new XMLHttpRequest();
    xhttpCar.onreadystatechange = function () {
        if (this.readyState == 4 && this.status == 200) {
            var response = JSON.parse(this.responseText);

            var route = response.routes[0];
            var routeCoordinates = [];
            for (var leg of route.legs) {
                var legCoordinates = leg.points.map((point) => [point.longitude, point.latitude]);
                routeCoordinates = routeCoordinates.concat(legCoordinates);
            }

            var routeLinestring = new atlas.data.LineString(routeCoordinates);
            map.addLinestrings([new atlas.data.Feature(routeLinestring)], {
                name: carRouteLayerName
            });
        }
    };

    var carRouteUrl = "https://atlas.microsoft.com/route/directions/json?";
    carRouteUrl += "&api-version=1.0";
    carRouteUrl += "&subscription-key=" + LBSAccountKey;
    carRouteUrl += "&query=" + startPoint.coordinates[1] + "," + startPoint.coordinates[0] + ":" +
        destinationPoint.coordinates[1] + "," + destinationPoint.coordinates[0];

    xhttpCar.open("GET", carRouteUrl, true);
    xhttpCar.send();
    ```
    Dit codefragment maakt weer een [XMLHttpRequest](https://xhr.spec.whatwg.org/), en voegt een gebeurtenis-handler toe voor het parseren van de binnenkomende respons. Voor een geslaagde respons wordt een matrix van coördinaten gemaakt voor de route die is geretourneerd, en toegevoegd aan de laag `carRouteLayerName` van de kaart. 
    
    Dit codefragment verzendt ook de query met de Route Service, om de route op te halen voor het opgegeven begin- en eindpunt, voor uw accountsleutel. Omdat er geen andere parameters worden gebruikt, wordt de route voor de standaard manier van reizen *auto* geretourneerd. 

3. Sla het bestand **MapTruckRoute.html** lokaal op, open het vervolgens in een webbrowser naar keuze en bekijk het resultaat. Voor een succesvolle verbinding met de Location Based Services API's bekijkt u een kaart die vergelijkbaar is met de volgende. 

    ![Routes rangschikken met Azure Route Service](./media/tutorial-prioritized-routes/lbs-prioritized-routes.png)

    Houd er rekening mee dat de vrachtwagenroute in blauw is weergegeven, terwijl de autoroute paars is.

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Uw query aan Route Service configureren
> * Routes weergeven gerangschikt naar de manier van reizen

Ga door naar de artikelen **Concepten** en **Aan de slag** voor meer diepgaande informatie over de Azure Location Based Services SDK. 
