---
title: Meerdere routes met Azure Maps | Microsoft Docs
description: Routes vinden voor verschillende manieren van reizen met Azure Maps
author: kgremban
ms.author: kgremban
ms.date: 05/07/2018
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: ea68c70befde1922fc30d06386a2ae326460c722
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34601408"
---
# <a name="find-routes-for-different-modes-of-travel-using-azure-maps"></a>Routes vinden voor verschillende manieren van reizen met Azure Maps

Deze zelfstudie laat zien hoe u uw Azure Maps-account en de routeservice gebruikt om de route naar uw nuttige plaats te vinden, gerangschikt naar uw manier van reizen. U geeft twee verschillende routes op de kaart weer: een voor auto's en een voor vrachtwagens waarvoor mogelijk routebeperkingen gelden qua hoogte, gewicht of gevaarlijke lading. In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een nieuwe webpagina maken met de kaartbesturingselement-API
> * Verkeerstromen op de kaart visualiseren
> * Routequery's maken die de manier van reizen declareren
> * Meerdere routes op de kaart weergeven

## <a name="prerequisites"></a>Vereisten

Voordat u doorgaat, voert u de stappen in de eerste zelfstudie uit voor het [maken van uw Azure Kaarten-account ](./tutorial-search-location.md#createaccount) en het [ophalen van de abonnementssleutel voor uw account](./tutorial-search-location.md#getkey). 


## <a name="create-a-new-map"></a>Een nieuwe kaart maken 
Gebruik de volgende stappen voor het maken van een statische HTML-pagina, ingesloten met de Map Control-API. 

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
    De resourcelocaties voor CSS en JavaScript-bestanden voor de Azure Maps-bibliotheek worden ingesloten in de HTML-koptekst. Het segment *script* in de hoofdtekst van de HTML-code bevat de inline JavaScript-code voor de kaart.
3. Voeg de volgende JavaScript-code toe aan het blok *script* van het HTML-bestand. Vervang de tekenreeks **\<your account key\>** door de primaire sleutel die u hebt gekopieerd uit uw Maps-account.

    ```JavaScript
    // Instantiate map to the div with id "map"
    var MapsAccountKey = "<your account key>";
    var map = new atlas.Map("map", {
        "subscription-key": MapsAccountKey
    });
    ```
    De **atlas.Map** biedt het besturingselement voor een visuele en interactieve webkaart en is een onderdeel van de Azure Map Control API.

4. Sla het bestand op en open het in uw browser. U hebt nu een basiskaart die u verder kunt ontwikkelen. 

   ![Basiskaart weergeven](./media/tutorial-prioritized-routes/basic-map.png)

## <a name="visualize-traffic-flow"></a>Verkeerstromen visualiseren

1. Als u geen focus voor de kaart instelt, wordt de hele wereld weergegeven. Als u de verkeersgegevens wilt weergeven, moet u een middelpunt en een zoomniveau instellen voor de kaart. Vervang de code die een `new atlas.Map` declareert door de volgende JavaScript-code: 
    
    ```JavaScript
    var map = new atlas.Map("map", {
        "subscription-key": MapsAccountKey,
        center: [-118.2437,34.0522],
        zoom: 12
    });
    ```

    Met deze code wordt het middelpunt van de kaart en een zoomniveau ingesteld zodat u zich standaard op een bepaald gebied kunt richten. 

1. Voeg de weergave van de verkeersstroom toe aan de kaart:

    ```JavaScript
    // Add Traffic Flow to the Map
    map.setTraffic({
        flow: "relative"
    });
    ```
    Deze code stelt de verkeersstroom in op `relative`, dit is de snelheid van de weg ten opzichte van een vrije stroom. U kunt deze ook instellen op `absolute` snelheid van de weg of `relative-delay` de relatieve snelheid weergeven, waar deze verschilt van een vrije stroom. 

2. Sla het bestand **MapTruckRoute.html** op en vernieuw de pagina in uw browser. U ziet de straten van Los Angeles en de huidige verkeersgegevens.

   ![Kaart met verkeer weergeven](./media/tutorial-prioritized-routes/traffic-map.png)

<a id="queryroutes"></a>

## <a name="set-start-and-end-points"></a>Het begin- en eindpunt instellen

Voor deze zelfstudie, moet u een fictief bedrijf in Seattle, genaamd Fabrikam, instellen als beginpunt, en een kantoor van Microsoft als eindpunt (bestemming). 

1. Voeg de volgende JavaScript-code toe om de pins te maken voor de begin- en eindpunten van de route:

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

2. Voeg de volgende JavaScript-code toe om de begin- en eindpunten aan de kaart toe te voegen:

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
    Door **map.setCameraBounds** aan te roepen, wordt het kaartenvenster aangepast aan de coördinaten van de begin- en eindpunten. De API **map.addPins** voegt de punten als visuele onderdelen toe aan het kaartbesturingselement.

3. Sla het bestand op en vernieuw de browser. De spelden worden weergegeven op de kaart. Ook al had u de kaart gedeclareerd met Los Angeles als middelpunt, met **map.setCameraBounds** is de kaartweergave aangepast aan het begin- en eindpunt. 

   ![Kaart met begin- en eindpunt weergeven](./media/tutorial-prioritized-routes/pins-map.png)


<a id="multipleroutes"></a>

## <a name="render-routes-prioritized-by-mode-of-travel"></a>Routes weergeven gerangschikt naar de manier van reizen

In deze sectie wordt beschreven hoe u de routeservice-API van Azure Maps gebruikt om te zoeken naar meerdere routes van een bepaald beginpunt naar een bestemming op basis van de manier van reizen. De routeservice biedt API's om de *snelste*, *kortste*, *zuinigste* of *leukste* route tussen twee locaties te plannen, waarbij rekening wordt gehouden met de huidige verkeersomstandigheden. Ook kunnen gebruikers in de toekomst routes plannen met behulp van de uitgebreide historische verkeersdatabase van Azure en de duur van de route voor elke dag en tijd voorspellen. Zie [Routebeschrijvingen ophalen](https://docs.microsoft.com/rest/api/maps/route/getroutedirections) voor meer informatie.


1. Voeg eerst een nieuwe laag aan de kaart toe om het routepad, de *linestring*, weer te geven. In deze zelfstudie zijn er twee verschillende routes: een **autoroute** en een **vrachtwagenroute**, elk met een eigen stijl. Voeg de volgende JavaScript-code toe aan het blok *script*:

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

2. Voeg de volgende JavaScript-code toe aan het blok *script*, om de route voor een vrachtwagen aan te vragen en de resultaten weer te geven op de kaart:

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
    truckRouteUrl += "&subscription-key=" + MapsAccountKey;
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
    Dit codefragment maakt een [XMLHttpRequest](https://xhr.spec.whatwg.org/) en voegt een gebeurtenis-handler toe voor het parseren van de binnenkomende respons. Voor een geslaagde respons wordt een matrix van coördinaten gemaakt voor de route die is geretourneerd, en toegevoegd aan de laag `truckRouteLayerName` van de kaart. 
    
    Dit codefragment maakt ook de query voor de routeservice van Maps met behulp van uw accountsleutel. De query bevat de coördinaten van het begin- en eindpunt en optionele parameters om aan te geven dat de route bedoeld is voor een zware vrachtwagen.

2. Voeg de volgende JavaScript-code toe om de route voor een auto aan te vragen en de resultaten weer te geven:

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
    carRouteUrl += "&subscription-key=" + MapsAccountKey;
    carRouteUrl += "&query=" + startPoint.coordinates[1] + "," + startPoint.coordinates[0] + ":" +
        destinationPoint.coordinates[1] + "," + destinationPoint.coordinates[0];

    xhttpCar.open("GET", carRouteUrl, true);
    xhttpCar.send();
    ```
    Dit codefragment maakt nog een [XMLHttpRequest](https://xhr.spec.whatwg.org/), en voegt een gebeurtenis-handler toe voor het parseren van de binnenkomende respons. Voor een geslaagde respons wordt een matrix van coördinaten gemaakt voor de route die is geretourneerd, en toegevoegd aan de laag `carRouteLayerName` van de kaart. 
    
    Dit codefragment maakt ook de query voor de routeservice van Maps met behulp van uw accountsleutel. De query bevat de coördinaten van het begin- en eindpunt. Omdat er geen aanvullende parameters zijn opgegeven, wordt de routeservice standaard ingesteld op *auto* als de manier van reizen. 

3. Sla het bestand **MapTruckRoute.html** op en vernieuw de browser om het resultaat te bekijken. Bij een succesvolle verbinding met de-API's van Maps ziet de kaart er ongeveer als volgt uit. 

    ![Routes rangschikken met Azure Route Service](./media/tutorial-prioritized-routes/prioritized-routes.png)

    De route voor vrachtwagens is blauw en dik. De route voor auto's is paars en dun. De route voor auto's gaat over Lake Washington via de I-90 door tunnels onder woongebieden en is verboden voor het vervoer van gevaarlijke afvalstoffen. De route voor vrachtwagens, met een lading met de Amerikaanse gevarenklasse USHazmatClass2, loopt dan ook via een andere snelweg. 

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Een nieuwe webpagina maken met de kaartbesturingselement-API
> * Verkeerstromen op de kaart visualiseren
> * Routequery's maken die de manier van reizen declareren
> * Meerdere routes op de kaart weergeven

Zie [Zoomniveaus en tegelraster](zoom-levels-and-tile-grid.md) en de overige artikelen over begrippen voor meer informatie over de dekking en mogelijkheden van Azure Maps. 

Zie [Het kaartbesturingselement gebruiken](how-to-use-map-control.md) en de overige handleidingen voor meer voorbeelden van code en interactieve codering. 
