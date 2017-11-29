---
title: Meerdere routes met Azure op basis van Locatieservices | Microsoft Docs
description: Routes voor verschillende modi van reizen met locatie op basis van Azure-Services vinden
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
ms.openlocfilehash: 3631bab8e5cb505689e92d2862c6863bcd56404d
ms.sourcegitcommit: 310748b6d66dc0445e682c8c904ae4c71352fef2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2017
---
# <a name="find-routes-for-different-modes-of-travel-using-azure-location-based-services"></a>Routes voor verschillende modi van reizen met locatie op basis van Azure-Services vinden

Deze zelfstudie laat zien hoe uw locatie op basis van Azure-Services-account en de Route-SDK, gebruiken om te vinden van de route naar interessante, door de modus van reizen geplaatst. In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Uw query Route-Service configureren
> * Routes geplaatst door de modus van reizen weergeven

## <a name="prerequisites"></a>Vereisten

Voordat u doorgaat, zorg ervoor dat [uw locatie op basis van Azure-Services-account maken](./tutorial-search-location.md#createaccount), en [ophalen van de sleutel van het abonnement voor uw account](./tutorial-search-location.md#getkey). U merkt ook het gebruik van het Kaartbesturingselement en zoeken-API's voor Service, zoals beschreven in de zelfstudie [zoeken in de buurt interessante met behulp van Azure op basis van Locatieservices](./tutorial-search-location.md), evenals informatie over het basisgebruik van de Route Service API's als beschreven in de zelfstudie [Route naar een locatie op basis van Azure-Services met behulp van](./tutorial-route-location.md).


<a id="queryroutes"></a>

## <a name="configure-your-route-service-query"></a>Uw query Route-Service configureren

Gebruik de volgende stappen voor het maken van een statische HTML-pagina in de locatie op basis van Services toewijzen besturingselement API is ingesloten. 

1. Maak een nieuw bestand op uw lokale machine en noem deze **MapTruckRoute.html**. 
2. De volgende HTML-onderdelen toevoegen aan het bestand:

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
    Houd er rekening mee dat de resource-locaties voor CSS en JavaScript-bestanden voor de locatie op basis van Azure-Services-bibliotheek wordt ingesloten door de HTML-header. U ziet ook de *script* segment toevoegen aan de hoofdtekst van het HTML-code, de inline JavaScript-code voor toegang tot de Azure-API voor het beheer van kaart bevat.
3. Voeg de volgende JavaScript-code naar de *script* blok van het HTML-bestand. Vervang de tijdelijke aanduiding *< insert-sleutel >* met primaire sleutel van uw Services op basis van locatie-account.

    ```HTML
            // Instantiate map to the div with id "map"
            var subscriptionKey = "<insert-key>";
            var map = new atlas.Map("map", {
                "subscription-key": subscriptionKey
            });

    ```
    De **atlas. Kaart** biedt het besturingselement voor een kaart visual en interactieve webpagina en is een onderdeel van de Azure-API voor het beheer van kaart.

4. Voeg de volgende JavaScript-code naar de *script* blok, de weergave van de stroom verkeer toevoegen aan de kaart:

    ```HTML
            // Add Traffic Flow to the Map
            map.setTraffic({
                flow: "relative"
            });
    ```
    Deze code wordt het netwerkverkeer ingesteld op `relative`, dit is de snelheid van de weg ten opzichte van de gratis stroom. U kunt deze ook instellen op `absolute` snelheid van de weg of `relative-delay` die de relatieve processorsnelheid waar deze verschilt van gratis overdracht weergegeven. 

5. Voeg de volgende JavaScript-code voor het maken van de pincodes voor het begin- en eindpunten van de route:

    ```HTML
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
    Deze code maakt u twee [GeoJSON objecten](https://en.wikipedia.org/wiki/GeoJSON) vertegenwoordigt de begin- en eindpunten van de route. 

6. Voeg de volgende JavaScript-code om toe te voegen lagen van *multipoint* voor het Kaartbesturingselement om weer te geven van de routes op basis van de modus van transport, bijvoorbeeld _auto_ en _vrachtwagen_.

    ```HTML
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

7. Voeg de volgende JavaScript-code voor de begin- en toevoegen aan de kaart:

    ```HTML
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
    De API **map.setCameraBounds** past u het venster volgens de coördinaten van de begin- en eindpunten hebben. De API **map.addPins** voegt de punten toe aan het kaartbesturingselement als visuele onderdelen.

8. Sla de **MapTruckRoute.html** bestand op uw computer. 

<a id="multipleroutes"></a>

## <a name="render-routes-prioritized-by-mode-of-travel"></a>Routes geplaatst door de modus van reizen weergeven

Deze sectie wordt beschreven hoe de Azure Locatieservices op basis van de Route Service API gebruiken om te zoeken naar meerdere routes van een bepaalde start verwijzen naar een bestemming op basis van de modus van transport. De Route-Service biedt de snelste plannen kortste-API's of systeem route tussen twee locaties, met inbegrip van de voorwaarden Realtime netwerkverkeer. Ook kunnen gebruikers in de toekomst routes met behulp van Azure uitgebreide historische verkeer database en het voorspellen van de duur van de route voor elke dag en tijd plannen. 

1. Open de **MapTruckRoute.html** bestand in de voorgaande sectie hebt gemaakt en voeg de volgende JavaScript-code naar de *script* blok ophalen van de route voor een vrachtwagen met behulp van de Route-Service.

    ```HTML
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
            truckRouteUrl += "&subscription-key=" + subscriptionKey;
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
    Dit codefragment maakt een [XMLHttpRequest](https://xhr.spec.whatwg.org/), en voegt u een gebeurtenis-handler voor het parseren van het binnenkomende antwoord. Voor een geslaagde reactie wordt gemaakt een matrix van coördinaten voor de route die is geretourneerd, en voegt u het toe van de documentstructuur `truckRouteLayerName` laag. 
    
    Dit codefragment verzendt ook de query met de Route-Service om op te halen van de route voor het opgegeven begin- en eindpunt voor abonnementssleutel voor uw account. De volgende optionele parameters worden gebruikt om aan te geven van de route voor een zware vrachtwagen:-de parameter `travelMode=truck` geeft de modus van reizen als *vrachtwagen*. Andere modi van reizen ondersteund *taxi*, *bus*, *van*, *motor*, en de standaardinstelling *auto* . 
        -De parameters `vehicleWidth`, `vehicleHeight`, en `vehicleLength` Geef de afmetingen van de drager in meters en worden beschouwd als alleen als de modus van reizen is *vrachtwagen*. 
        -De `vehicleLoadType` de lading als gevaarlijke en beperkte op sommige wegen wordt geclassificeerd. Dit wordt momenteel ook beschouwd als alleen voor de *vrachtwagen* modus. 

2. Voeg de volgende JavaScript-code voor de route voor een auto met behulp van de Route-Service:

    ```HTML
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
            carRouteUrl += "&subscription-key=" + subscriptionKey;
            carRouteUrl += "&query=" + startPoint.coordinates[1] + "," + startPoint.coordinates[0] + ":" +
                destinationPoint.coordinates[1] + "," + destinationPoint.coordinates[0];

            xhttpCar.open("GET", carRouteUrl, true);
            xhttpCar.send();
    ```
    Dit codefragment maakt u een andere [XMLHttpRequest](https://xhr.spec.whatwg.org/), en voegt u een gebeurtenis-handler voor het parseren van het binnenkomende antwoord. Voor een geslaagde reactie wordt gemaakt een matrix van coördinaten voor de route die is geretourneerd, en voegt u het toe van de documentstructuur `carRouteLayerName` laag. 
    
    Dit codefragment verzendt ook de query met de Route-Service om op te halen van de route voor het opgegeven begin- en eindpunt voor abonnementssleutel voor uw account. Omdat er geen andere parameters worden gebruikt, de route voor de standaardmodus reizen *auto* wordt geretourneerd. 

3. Sla de **MapTruckRoute.html** lokaal bestand vervolgens opent u het in een webbrowser naar keuze en bekijk het resultaat. Voor een succesvolle verbinding met de locatie op basis van Services-API's ziet u een vergelijkbaar met de volgende kaart. 

    ![Prioriteit routes met Azure Route-Service](./media/tutorial-prioritized-routes/lbs-prioritized-routes.png)

    Houd er rekening mee dat in blauw, de route vrachtwagen wordt terwijl de route auto paarse.

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Uw query Route-Service configureren
> * Routes geplaatst door de modus van reizen weergeven

Ga door naar de **concepten** en **How To** artikelen voor meer informatie over de Azure locatie op basis van Services SDK in de diepte. 
