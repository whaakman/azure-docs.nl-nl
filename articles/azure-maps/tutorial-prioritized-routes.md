---
title: Meerdere routes met Azure Maps | Microsoft Docs
description: Routes vinden voor verschillende manieren van reizen met Azure Maps
author: walsehgal
ms.author: v-musehg
ms.date: 11/14/2018
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: 5458c7e74728952df89380a3649c6ed60eb6ea9a
ms.sourcegitcommit: 039263ff6271f318b471c4bf3dbc4b72659658ec
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/06/2019
ms.locfileid: "55749760"
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
    <html>
    <head>
        <title>Multiple routes by mode of travel</title>
        
        <meta charset="utf-8" />
        <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no" />

        <!-- Add references to the Azure Maps Map control JavaScript and CSS files. -->
        <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/css/atlas.min.css?api-version=1" type="text/css" />
        <script src="https://atlas.microsoft.com/sdk/js/atlas.min.js?api-version=1"></script>

        <!-- Add a reference to the Azure Maps Services Module JavaScript file. -->
        <script src="https://atlas.microsoft.com/sdk/js/atlas-service.js?api-version=1"></script>

        <script>
            var map, datasource, client;

            function GetMap() {
                //Add Map Control JavaScript code here.
            }
        </script>
        <style>
            html,
            body {
                width: 100%;
                height: 100%;
                padding: 0;
                margin: 0;
            }

            #myMap {
                position: relative;
                width: 100%;
                height: 100%;
            }
        </style>
    </head>
    <body onload="GetMap()">
        <div id="myMap"></div>
    </body>
    </html>
    ```
    
    U ziet dat de HTML-header de CSS- en JavaScript-bronbestanden bevat, gehost door de Azure Map Control-bibliotheek. Let op de gebeurtenis `onload` in het hoofdtekstgedeelte van de pagina. Deze zorgt ervoor dat de functie `GetMap` wordt aangeroepen nadat het hoofdtekstgedeelte van de pagina is geladen. Deze functie bevat de inline JavaScript-code voor toegang tot de API's van Azure Maps.

3. Voeg de volgende JavaScript-code toe aan de functie `GetMap`. Vervang de tekenreeks **\<Your Azure Maps Key\>** door de primaire sleutel die u hebt gekopieerd in uw Maps-account.

    ```JavaScript
    //Add your Azure Maps subscription key to the map SDK. 
    atlas.setSubscriptionKey('<Your Azure Maps Key>');

    //Initialize a map instance.
    map = new atlas.Map('myMap');
    ```

    De **atlas.Map** biedt het besturingselement voor een visuele en interactieve webkaart en is een onderdeel van de Azure Map Control API.

4. Sla het bestand op en open het in uw browser. U hebt nu een basiskaart die u verder kunt ontwikkelen.

   ![Basiskaart weergeven](./media/tutorial-prioritized-routes/basic-map.png)

## <a name="visualize-traffic-flow"></a>Verkeerstromen visualiseren

1. Voeg de weergave van de verkeersstroom toe aan de kaart. De `map.events.add` zorgt ervoor dat alle kaartfuncties die zijn toegevoegd aan de kaart, worden geladen nadat de kaart volledig is geladen.

    ```JavaScript
    map.events.add("load", function() {
        // Add Traffic Flow to the Map
        map.setTraffic({
            flow: "relative"
        });
    });
    ```
    
     Er wordt aan de kaart een laadgebeurtenis toegevoegd die wordt geactiveerd wanneer alle kaartresources volledig zijn geladen. In de gebeurtenis-handler voor het laden van de kaart is de instelling voor de verkeersstroom op de kaart ingesteld op `relative`, dit is de snelheid van de weg ten opzichte van een situatie met een vrije verkeersstroom. U kunt deze ook instellen op `absolute` snelheid van de weg of `relative-delay` de relatieve snelheid weergeven, waar deze verschilt van een vrije stroom.

2. Sla het bestand **MapTruckRoute.html** op en vernieuw de pagina in uw browser. U ziet de straten van Los Angeles en de huidige verkeersgegevens.

   ![Kaart met verkeer weergeven](./media/tutorial-prioritized-routes/traffic-map.png)

<a id="queryroutes"></a>

## <a name="define-how-the-route-will-be-rendered"></a>Definiëren hoe de route wordt weergegeven

In deze zelfstudie worden twee routes berekend en weergegeven op de kaart. Eén route die toegankelijk is voor auto's en de andere die toegankelijk is voor vrachtwagens. Samen met de routes wordt een pictogram met een symbool weergegeven dat het begin- en het eindpunt van elke route aanduidt evenals lijnen met een andere kleur voor elk routepad.

1. Voeg de volgende JavaScript-code toe aan de functie GetMap nadat de kaart is geïnitialiseerd.

    ```JavaScript
    //Wait until the map resources have fully loaded.
    map.events.add('load', function () {

        //Create a data source and add it to the map.
        datasource = new atlas.source.DataSource();
        map.sources.add(datasource);

        //Add a layer for rendering the route lines and have it render under the map labels.
        map.layers.add(new atlas.layer.LineLayer(datasource, null, {
            strokeColor: ['get', 'strokeColor'],
            strokeWidth: ['get', 'strokeWidth'],
            lineJoin: 'round',
            lineCap: 'round',
            filter: ['==', '$type', 'LineString']
        }), 'labels');

        //Add a layer for rendering point data.
        map.layers.add(new atlas.layer.SymbolLayer(datasource, null, {
            iconOptions: {
                image: ['get', 'icon'],
                allowOverlap: true
            },
            textOptions: {
                textField: ['get', 'title'],
                offset: [0, 1.2]
            },
            filter: ['==', '$type', 'Point']
        }));
    });
    ```
    
    Er wordt aan de kaart een laadgebeurtenis toegevoegd die wordt geactiveerd wanneer alle kaartresources volledig zijn geladen. In de gebeurtenis-handler voor het laden van de kaart wordt een gegevensbron gemaakt waarin de routelijnen en de begin- en eindpunten worden opgeslagen. Er wordt een lijnlaag gemaakt en aan de gegevensbron gekoppeld waarmee wordt gedefinieerd hoe de routelijn wordt weergegeven. Er wordt gebruikgemaakt van expressies om de lijndikte en kleur op te halen uit de eigenschappen van de routelijnfunctie. Er wordt een filter toegevoegd om ervoor te zorgen dat met deze laag alleen GeoJSON LineString-gegevens worden toegevoegd. Als u de laag toevoegt aan de kaart wordt een tweede parameter met de waarde `'labels'` doorgegeven. Hiermee wordt bepaald dat deze laag onder de kaartlabels wordt weergegeven. Zo kunt u er zeker van zijn dat de routelijnen de straatlabels niet bedekken. Er wordt een symboollaag gemaakt en aan de gegevensbron gekoppeld. Deze laag geeft aan hoe de begin- en eindpunten worden weergegeven. In dit geval zijn er expressies toegevoegd om de afbeelding van het pictogram en de tekstlabelgegevens op te halen uit de eigenschappen van elk puntobject. 
    
2. Voor deze zelfstudie, moet u een fictief bedrijf in Seattle, genaamd Fabrikam, instellen als beginpunt, en een kantoor van Microsoft als eindpunt (bestemming). Voeg de volgende code toe aan de gebeurtenis-handler voor het laden van kaarten.

    ```JavaScript
    //Create the GeoJSON objects which represent the start and end point of the route.
    var startPoint = new atlas.data.Feature(new atlas.data.Point([-122.356099, 47.580045]), {
        title: 'Fabrikam, Inc.',
        icon: 'pin-round-blue'
    });

    var endPoint = new atlas.data.Feature(new atlas.data.Point([-122.201164, 47.616940]), {
        title: 'Microsoft - Lincoln Square',
        icon: 'pin-blue'
    });
    ```
    
    Deze code maakt twee [GeoJSON-objecten](https://en.wikipedia.org/wiki/GeoJSON) die de begin- en eindpunten van de route vertegenwoordigen. Aan elk punt wordt een eigenschap `title` en `icon` toegevoegd.

3. Voeg nu de volgende JavaScript-code toe om de spelden voor de begin- en eindpunten toe te voegen aan de kaart:

    ```JavaScript
    //Add the data to the data source.
    datasource.add([startPoint, endPoint]);

    //Fit the map window to the bounding box defined by the start and end positions.
    map.setCamera({
        bounds: atlas.data.BoundingBox.fromData([startPoint, endPoint]),
        padding: 100
    });
    ```
    
    De begin- en eindpunten worden toegevoegd aan de gegevensbron. Het begrenzingsvak voor de begin- en eindpunten wordt berekend met behulp van de functie `atlas.data.BoundingBox.fromData`. Dit begrenzingsvak wordt gebruikt om de cameraweergave van de kaart met behulp van de functie `map.setCamera` in te stellen op het begin- en eindpunt. Er wordt opvulling toegevoegd om de grootte van de pixels in de symboolpictogrammen te compenseren.

4. Sla het bestand op en vernieuw de browser. De spelden worden weergegeven op de kaart. De kaart is nu gecentreerd op Seattle en u ziet de ronde blauwe speld die het beginpunt aangeeft en de blauwe speld die het eindpunt aangeeft.

   ![Kaart met begin- en eindpunt weergeven](./media/tutorial-prioritized-routes/pins-map.png)

<a id="multipleroutes"></a>

## <a name="render-routes-prioritized-by-mode-of-travel"></a>Routes weergeven gerangschikt naar de manier van reizen

In deze sectie wordt beschreven hoe u de routeservice-API van Azure Maps gebruikt om te zoeken naar meerdere routes van een bepaald beginpunt naar een bestemming op basis van de manier van reizen. De routeservice biedt API's om de *snelste*, *kortste*, *zuinigste* of *leukste* route tussen twee locaties te plannen, waarbij rekening wordt gehouden met de huidige verkeersomstandigheden. Ook kunnen gebruikers in de toekomst routes plannen met behulp van de uitgebreide historische verkeersdatabase van Azure en de duur van de route voor elke dag en tijd voorspellen. Zie [GetRoute Directions](https://docs.microsoft.com/rest/api/maps/route/getroutedirections) (Routebeschrijvingen ophalen) voor meer informatie. Alle volgende codeblokken moeten worden toegevoegd **binnen de map.loadEventListener** om ervoor te zorgen dat de blokken worden geladen nadat de kaart volledig is geladen.

1. Maak een instantie van de serviceclient door de volgende Javascript-code toe te voegen aan de gebeurtenis-handler voor het laden van kaarten.

    ```JavaScript
    //If the service client hasn't been created, create an instance of it.
    if (!client) {
        client = new atlas.service.Client(atlas.getSubscriptionKey());
    }
    ```
    
2. Voeg het volgende blok met code toe om een routequeryreeks te maken.

    ```JavaScript
    //Create the route request with the query being the start and end point in the format 'startLongitude,startLatitude:endLongitude,endLatitude'.
    var routeQuery = startPoint.geometry.coordinates[1] +
        ',' +
        startPoint.geometry.coordinates[0] +
        ':' +
        endPoint.geometry.coordinates[1] +
        ',' +
        endPoint.geometry.coordinates[0];
    ```

3. Voeg de volgende JavaScript-code toe om de route voor een vrachtwagen met een vracht van de klasse USHazmatClass2 aan te vragen en de resultaten weer te geven:

    ```JavaScript
    //Execute the truck route query then add the route to the map.
    client.route.getRouteDirections(routeQuery, {
        travelMode: 'truck',
        vehicleWidth: 2,
        vehicleHeight: 2,
        vehicleLength: 5,
        vehicleLoadType: 'USHazmatClass2'
    }).then(function (response) {
        // Parse the response into GeoJSON
        var geoJsonResponse = new atlas.service.geojson.GeoJsonRouteDirectionsResponse(response);

        //Get the route line and add some style properties to it.
        var routeLine = geoJsonResponse.getGeoJsonRoutes().features[0];
        routeLine.properties.strokeColor = '#2272B9';
        routeLine.properties.strokeWidth = 9;

        //Add the route line to the data source. We want this to render below the car route which will likely be added to the data source faster, so insert it at index 0.
        datasource.add(routeLine, 0);
    });
    ```
    Met dit codefragment wordt een query uitgevoerd op de Azure Maps-routeringservice via de methode [getRouteDirections](https://docs.microsoft.com/javascript/api/azure-maps-rest/services.route?view=azure-iot-typescript-latest), waarna het antwoord met behulp van [getGeoJsonRouteDirectionsResponse](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.geojson.geojsonroutedirectionsresponse?view=azure-iot-typescript-latest) wordt geparseerd naar de GeoJSON-indeling. Vervolgens wordt er een matrix met coördinaten gemaakt voor de route die is geretourneerd en wordt deze toegevoegd aan de gegevensbron. Maar er wordt ook een index van 0 toegevoegd zodat de route altijd eerder wordt weergegeven dan alle andere lijnen in de gegevensbron. Dit gebeurt omdat de berekende vrachtwagenroute vaak langzamer uitvalt dan een autoroute, en als de vrachtwagenroute na de autoroute aan de gegevensbron wordt toegevoegd, wordt de vrachtwagenroute bovenop de autoroute weergegeven. Er worden twee eigenschappen toegevoegd aan de lijn voor de vrachtwagenroute: een mooie blauwe lijnkleur en een lijndikte van 9 pixels. 

4. Voeg de volgende JavaScript-code toe om de route voor een auto aan te vragen en de resultaten weer te geven:

    ```JavaScript
    //Execute the car route query then add the route to the map.
    client.route.getRouteDirections(routeQuery).then(function (response) {
        // Parse the response into GeoJSON
        var geoJsonResponse = new atlas.service.geojson.GeoJsonRouteDirectionsResponse(response);

        //Get the route line and add some style properties to it.
        var routeLine = geoJsonResponse.getGeoJsonRoutes().features[0];
        routeLine.properties.strokeColor = '#B76DAB';
        routeLine.properties.strokeWidth = 5;

        //Add the route line to the data source.
        datasource.add(routeLine);
    });
    ```
    Dit codefragment gebruikt dezelfde query voor het opvragen van de route voor een vrachtwagen als voor een auto. Er wordt een query uitgevoerd op de Azure Maps-routeringservice via de methode [getRouteDirections](https://docs.microsoft.com/javascript/api/azure-maps-rest/services.route?view=azure-iot-typescript-latest), waarna het antwoord met behulp van [getGeoJsonRouteDirectionsResponse](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.geojson.geojsonroutedirectionsresponse?view=azure-iot-typescript-latest) wordt geparseerd naar de GeoJSON-indeling. Ten slotte wordt er een matrix met coördinaten gemaakt voor de route die is geretourneerd, en wordt deze toegevoegd aan de gegevensbron. Er worden twee eigenschappen toegevoegd aan de lijn voor de autoroute: lijnkleur paars en lijndikte van 5 pixels. 

5. Sla het bestand **MapTruckRoute.html** op en vernieuw de browser om het resultaat te bekijken. Bij een succesvolle verbinding met de-API's van Maps ziet de kaart er ongeveer als volgt uit.

    ![Routes rangschikken met Azure Route Service](./media/tutorial-prioritized-routes/prioritized-routes.png)

    De route voor vrachtwagens is blauw en dik. De route voor auto's is paars en dun. De route voor auto's gaat over Lake Washington via de I-90 door tunnels onder woongebieden en is verboden voor het vervoer van gevaarlijke afvalstoffen. De route voor vrachtwagens, met een lading met de Amerikaanse gevarenklasse USHazmatClass2, loopt dan ook via een andere snelweg.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Een nieuwe webpagina maken met de kaartbesturingselement-API
> * Verkeerstromen op de kaart visualiseren
> * Routequery's maken die de manier van reizen declareren
> * Meerdere routes op de kaart weergeven

De voorbeeldcode voor deze zelfstudie vindt u hier:

> [Meerdere routes met Azure Maps](https://github.com/Azure-Samples/AzureMapsCodeSamples/blob/master/AzureMapsCodeSamples/Tutorials/truckRoute.html)

[Het livevoorbeeld ziet u hier](https://azuremapscodesamples.azurewebsites.net/?sample=Multiple%20routes%20by%20mode%20of%20travel)

In de volgende zelfstudie ziet u het proces voor het maken van een eenvoudige winkellocator met behulp van Azure Maps.

> [!div class="nextstepaction"]
> [Een winkellocator maken met behulp van Azure Maps](./tutorial-create-store-locator.md)
