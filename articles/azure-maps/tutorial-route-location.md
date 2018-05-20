---
title: Route zoeken met Azure Maps | Microsoft Docs
description: Zoeken naar een nuttige plaats met Azure Maps
services: azure-maps
keywords: ''
author: kgremban
ms.author: kgremban
ms.date: 05/07/2018
ms.topic: tutorial
ms.service: azure-maps
documentationcenter: ''
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 73ca61140f05a65ca75cd703ed226773b9a43dfa
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
# <a name="route-to-a-point-of-interest-using-azure-maps"></a>Zoeken naar een nuttige plaats met Azure Maps

Deze zelfstudie laat zien hoe u uw Azure Maps-account en de Route Service SDK gebruikt om de route naar uw nuttige plaats te vinden. In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een nieuwe webpagina maken met de kaartbesturingselement-API
> * Adrescoördinaten instellen
> * Query uitvoeren op Route Service voor route naar nuttige plaats

## <a name="prerequisites"></a>Vereisten

Voordat u doorgaat, voert u de stappen in de vorige zelfstudie uit voor het [maken van uw Azure Kaarten-account ](./tutorial-search-location.md#createaccount) en het [ophalen van de abonnementssleutel voor uw account](./tutorial-search-location.md#getkey). 

<a id="getcoordinates"></a>

## <a name="create-a-new-map"></a>Een nieuwe kaart maken 
Gebruik de volgende stappen voor het maken van een statische HTML-pagina, ingesloten met de Map Control-API. 

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
    De resourcelocaties voor CSS en JavaScript-bestanden voor de Azure Maps-bibliotheek worden ingesloten in de HTML-koptekst. Het *script*-segment in de hoofdtekst van het HTML-bestand bevat de inline JavaScript-code voor toegang tot de API's van Azure Maps.

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

   ![Basiskaart weergeven](./media/tutorial-route-location/basic-map.png)

## <a name="set-start-and-end-points"></a>Het begin- en eindpunt instellen

Voor deze zelfstudie stelt u Microsoft in als het beginpunt en een benzinestation in Seattle als eindpunt (bestemming). 

1. Voeg in hetzelfde *script*-blok van het bestand **MapRoute.html** de volgende JavaScript-code toe voor het maken van het begin- en eindpunt voor de route:

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
    Deze code maakt twee [GeoJSON-objecten](https://en.wikipedia.org/wiki/GeoJSON) die de begin- en eindpunten van de route vertegenwoordigen. 

2. Voeg de volgende JavaScript-code toe om de pins voor de begin- en eindpunten aan de kaart toe te voegen:

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
    Met **map.setCameraBounds** wordt het kaartvenster aangepast aan de coördinaten van het begin- en eindpunt. De API **map.addPins** voegt de punten als visuele onderdelen toe aan het kaartbesturingselement.

7. Sla het bestand **MapRoute.html** op en vernieuw de browser. De kaart is nu gecentreerd op Seattle en u ziet de ronde blauwe speld die het beginpunt aangeeft en de blauwe speld die het eindpunt aangeeft.

   ![Kaart met gemarkeerd begin- en eindpunt weergeven](./media/tutorial-route-location/map-pins.png)

<a id="getroute"></a>

## <a name="get-directions"></a>Routebeschrijving ophalen

Deze sectie beschrijft hoe u de routeservice-API van Maps gebruikt om te zoeken naar de route van een bepaald beginpunt naar een bestemming. De routeservice biedt API's om de *snelste*, *kortste*, *zuinigste* of *leukste* route tussen twee locaties te plannen. Ook kunnen gebruikers in de toekomst routes plannen met behulp van de uitgebreide historische verkeersdatabase van Azure en de duur van de route voor elke dag en tijd voorspellen. Zie [Routebeschrijvingen ophalen](https://docs.microsoft.com/rest/api/maps/route/getroutedirections) voor meer informatie.


1. Voeg eerst een nieuwe laag aan de kaart toe om het routepad, de *linestring*, weer te geven. Voeg de volgende JavaScript-code toe aan het blok *script*:

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

2. Maak een [XMLHttpRequest](https://xhr.spec.whatwg.org/) en voeg een gebeurtenis-handler toe voor het parseren van het JSON-antwoord dat wordt verzonden door de routeservice van Maps. Deze code maakt een matrix van coördinaten voor de lijnsegmenten van de route en voegt vervolgens de set coördinaten toe aan de linestrings-laag van de kaart. 

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

3. Voeg de volgende code toe om de query samen te stellen en de XMLHttpRequest te verzenden naar de routeservice van Maps:

    ```JavaScript
    var url = "https://atlas.microsoft.com/route/directions/json?";
    url += "api-version=1.0";
    url += "&subscription-key=" + MapsAccountKey;
    url += "&query=" + startPoint.coordinates[1] + "," + startPoint.coordinates[0] + ":" +
        destinationPoint.coordinates[1] + "," + destinationPoint.coordinates[0];

    xhttp.open("GET", url, true);
    xhttp.send();
    ```

3. Sla het bestand **MapRoute.html** op en vernieuw de webbrowser. Bij een succesvolle verbinding met de API's van Maps ziet de kaart er ongeveer als volgt uit. 

    ![Azure Map Control en Route Service](./media/tutorial-route-location/map-route.png)


## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Een nieuwe webpagina maken met de kaartbesturingselement-API
> * Adrescoördinaten instellen
> * Query uitvoeren op routeservice voor routebeschrijving naar nuttige plaats

De volgende zelfstudie laat zien hoe u een routequery maakt met beperkingen zoals de manier van reizen of het type lading, en meerdere routes op dezelfde kaart weergeeft. 

> [!div class="nextstepaction"]
> [Routes vinden voor verschillende manieren van reizen](./tutorial-prioritized-routes.md)
