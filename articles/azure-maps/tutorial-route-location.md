---
title: Route zoeken met Azure Maps | Microsoft Docs
description: Zoeken naar een nuttige plaats met Azure Maps
author: walsehgal
ms.author: v-musehg
ms.date: 11/14/2018
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: a3807dc792c2e56c3e7c1b74f7d3e8f73ac0f4b0
ms.sourcegitcommit: 275eb46107b16bfb9cf34c36cd1cfb000331fbff
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/15/2018
ms.locfileid: "51705086"
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
    <html>
    <head>
        <title>Map Route</title>
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

            #map {
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

    De `atlas.Map` biedt het besturingselement voor een visuele en interactieve webkaart en is een onderdeel van de Azure Map Control API.

4. Sla het bestand op en open het in uw browser. U hebt nu een basiskaart die u verder kunt ontwikkelen.

   ![Basiskaart weergeven](./media/tutorial-route-location/basic-map.png)

## <a name="define-how-the-route-will-be-rendered"></a>Definiëren hoe de route wordt weergegeven

In deze zelfstudie wordt een eenvoudige route weergegeven met behulp van een symboolpictogram voor het begin en het einde van de route, en een lijn voor het routepad.

1. Voeg de volgende JavaScript-code toe aan de functie GetMap nadat de kaart is geïnitialiseerd.

    ```JavaScript
    //Wait until the map resources have fully loaded.
    map.events.add('load', function () {

        //Create a data source and add it to the map.
        datasource = new atlas.source.DataSource();
        map.sources.add(datasource);

        //Add a layer for rendering the route lines and have it render under the map labels.
        map.layers.add(new atlas.layer.LineLayer(datasource, null, {
            strokeColor: '#2272B9',
            strokeWidth: 5,
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
    
    Er wordt aan de kaart een laadgebeurtenis toegevoegd die wordt geactiveerd wanneer alle kaartresources volledig zijn geladen. In de gebeurtenis-handler voor het laden van de kaart wordt een gegevensbron gemaakt waarin de routelijn, en de begin- en eindpunten worden opgeslagen. Er wordt een lijnlaag gemaakt en aan de gegevensbron gekoppeld waarmee wordt gedefinieerd hoe de routelijn wordt weergegeven. De routelijn wordt weergegeven met een blauwe schaduw met een breedte van 5 pixels, en afgeronde verbindingen en eindstukken. Er wordt een filter toegevoegd om ervoor te zorgen dat met deze laag alleen GeoJSON LineString-gegevens worden weergegeven. Als u de laag toevoegt aan de kaart, wordt een tweede parameter met de waarde `'labels'` doorgegeven. Hiermee wordt bepaald dat deze laag onder de kaartlabels wordt weergegeven. Zo kunt u er zeker van zijn dat de routelijnen de straatlabels niet bedekken. Er wordt een symboollaag gemaakt en aan de gegevensbron gekoppeld. Deze laag geeft aan hoe de begin- en eindpunten worden weergegeven. In dit geval zijn er expressies toegevoegd om de afbeelding van het pictogram en de tekstlabelgegevens op te halen uit de eigenschappen van elk puntobject. 
    
2. Voor deze zelfstudie stelt u Microsoft in als beginpunt en een benzinestation in Seattle als eindpunt. Voeg de volgende code toe aan de gebeurtenis-handler voor het laden van kaarten.

    ```JavaScript
    //Create the GeoJSON objects which represent the start and end point of the route.
    var startPoint = new atlas.data.Feature(new atlas.data.Point([-122.130137, 47.644702]), {
        title: 'Microsoft',
        icon: 'pin-round-blue'
    });

    var endPoint = new atlas.data.Feature(new atlas.data.Point([-122.3352, 47.61397]), {
        title: 'Contoso Oil & Gas',
        icon: 'pin-blue'
    });    
    ```

    Deze code maakt twee [GeoJSON-puntobjecten](https://en.wikipedia.org/wiki/GeoJSON) die de begin- en eindpunten van de route vertegenwoordigen. Aan elk punt wordt een eigenschap `title` en `icon` toegevoegd.
    
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
    
    De begin- en eindpunten worden toegevoegd aan de gegevensbron. Het begrenzingsvak voor de begin- en eindpunten wordt berekend met behulp van de functie `atlas.data.BoundingBox.fromData`. Dit begrenzingsvak wordt gebruikt om de cameraweergave van de kaart met behulp van de functie **map.setCamera** in te stellen op het begin- en eindpunt. Er wordt opvulling toegevoegd om de grootte van de pixels in de symboolpictogrammen te compenseren.

3. Sla het bestand **MapRoute.html** op en vernieuw de browser. De kaart is nu gecentreerd op Seattle en u ziet de ronde blauwe speld die het beginpunt aangeeft en de blauwe speld die het eindpunt aangeeft.

   ![Kaart met gemarkeerd begin- en eindpunt weergeven](./media/tutorial-route-location/map-pins.png)

<a id="getroute"></a>

## <a name="get-directions"></a>Routebeschrijving ophalen

Deze sectie beschrijft hoe u de routeservice-API van Maps gebruikt om te zoeken naar de route van een bepaald beginpunt naar een bestemming. De routeservice biedt API's om de *snelste*, *kortste*, *zuinigste* of *leukste* route tussen twee locaties te plannen. Ook kunnen gebruikers in de toekomst routes plannen met behulp van de uitgebreide historische verkeersdatabase van Azure en de duur van de route voor elke dag en tijd voorspellen. Zie [Routebeschrijvingen ophalen](https://docs.microsoft.com/rest/api/maps/route/getroutedirections) voor meer informatie. Alle van de volgende functies moeten worden toegevoegd **binnen de eventListener voor het laden van de kaart** om ervoor te zorgen dat deze worden geladen nadat de kaart volledig is geladen.

1. Maak een instantie van de serviceclient door de volgende Javascript-code toe te voegen aan de gebeurtenis-handler voor het laden van kaarten.

    ```JavaScript
    //If the service client hasn't already been created, create an instance.
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

3. Om de route op te vragen, voegt u het volgende blok met code toe aan het script. Er wordt dan een query uitgevoerd op de Azure Maps-routeringservice via de methode [getRouteDirections](https://docs.microsoft.com/javascript/api/azure-maps-rest/services.route?view=azure-iot-typescript-latest#getroutedirections), waarna het antwoord met behulp van [getGeoJsonRoutes](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.geojson.geojsonroutedirectionsresponse?view=azure-iot-typescript-latest#getgeojsonroutes) wordt geparseerd naar de GeoJSON-indeling. Vervolgens wordt een routelijn toegevoegd in het antwoord van de gegevensbron, waardoor de lijn automatisch wordt weergegeven op de kaart.

    ```JavaScript
    //Execute the car route query then add the route to the map once a response is received.
    client.route.getRouteDirections(routeQuery).then(function (response) {
        // Parse the response into GeoJSON
        var geoJsonResponse = new atlas.service.geojson.GeoJsonRouteDirectionsResponse(response);

        //Add the route line to the data source.
        datasource.add(geoJsonResponse.getGeoJsonRoutes().features[0]);
    });
    ```

5. Sla het bestand **MapRoute.html** op en vernieuw de webbrowser. Bij een succesvolle verbinding met de API's van Maps ziet de kaart er ongeveer als volgt uit.

    ![Azure Map Control en Route Service](./media/tutorial-route-location/map-route.png)

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Een nieuwe webpagina maken met de kaartbesturingselement-API
> * Adrescoördinaten instellen
> * Query uitvoeren op routeservice voor routebeschrijving naar nuttige plaats

De voorbeeldcode voor deze zelfstudie vindt u hier:

> [Route zoeken met Azure Maps](https://github.com/Azure-Samples/AzureMapsCodeSamples/blob/master/AzureMapsCodeSamples/Tutorials/route.html)

[Het livevoorbeeld hier bekijken](https://azuremapscodesamples.azurewebsites.net/?sample=Route%20to%20a%20destination)

De volgende zelfstudie laat zien hoe u een routequery maakt met beperkingen zoals de manier van reizen of het type lading, en meerdere routes op dezelfde kaart weergeeft.

> [!div class="nextstepaction"]
> [Routes vinden voor verschillende manieren van reizen](./tutorial-prioritized-routes.md)
