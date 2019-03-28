---
title: Meerdere routes met Azure Maps | Microsoft Docs
description: Routes vinden voor verschillende manieren van reizen met Azure Maps
author: walsehgal
ms.author: v-musehg
ms.date: 03/07/2019
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: d78f46d2d62ca9db9400e0f436a8c0358734a54e
ms.sourcegitcommit: cf971fe82e9ee70db9209bb196ddf36614d39d10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/27/2019
ms.locfileid: "58540513"
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
        <title>Map Route</title>
        <meta charset="utf-8">
        <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">

        <!-- Add references to the Azure Maps Map control JavaScript and CSS files. -->
        <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/css/atlas.min.css?api-version=2" type="text/css">
        <script src="https://atlas.microsoft.com/sdk/js/atlas.min.js?api-version=2"></script>

        <!-- Add a reference to the Azure Maps Services Module JavaScript file. -->
        <script src="https://atlas.microsoft.com/sdk/js/atlas-service.js?api-version=2"></script>

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
    //Instantiate a map object
    var map = new atlas.Map("myMap", {
        //Add your Azure Maps subscription key to the map SDK. Get an Azure Maps key at https://azure.com/maps
        authOptions: {
            authType: 'subscriptionKey',
            subscriptionKey: '<Your Azure Maps Key>'
        }
    });
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

2. Sla het bestand **MapTruckRoute.html** op en vernieuw de pagina in uw browser. Als u in de kaart werken en zoom in op Los Angeles ziet u de Streets (straten) met de huidige verkeersgegevens.

   ![Kaart met verkeer weergeven](./media/tutorial-prioritized-routes/traffic-map.png)

<a id="queryroutes"></a>

## <a name="define-how-the-route-will-be-rendered"></a>Definiëren hoe de route wordt weergegeven

In deze zelfstudie worden twee routes berekend en weergegeven op de kaart. Eén route die toegankelijk is voor auto's en de andere die toegankelijk is voor vrachtwagens. Samen met de routes wordt een pictogram met een symbool weergegeven dat het begin- en het eindpunt van elke route aanduidt evenals lijnen met een andere kleur voor elk routepad.

1. Na het initialiseren van de kaart, toevoegen de volgende JavaScript-code in een gebeurtenis voor het laden van de kaart.

    ```JavaScript
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
    ```

    Er wordt aan de kaart een laadgebeurtenis toegevoegd die wordt geactiveerd wanneer alle kaartresources volledig zijn geladen. In de gebeurtenis-handler voor het laden van de kaart wordt een gegevensbron gemaakt waarin de routelijnen en de begin- en eindpunten worden opgeslagen. Een laag van de regel is gemaakt en gekoppeld aan de gegevensbron om te definiëren hoe de route-regel wordt weergegeven. Er wordt gebruikgemaakt van expressies om de lijndikte en kleur op te halen uit de eigenschappen van de routelijnfunctie. Er wordt een filter toegevoegd om ervoor te zorgen dat met deze laag alleen GeoJSON LineString-gegevens worden toegevoegd. Als u de laag toevoegt aan de kaart wordt een tweede parameter met de waarde `'labels'` doorgegeven. Hiermee wordt bepaald dat deze laag onder de kaartlabels wordt weergegeven. Zo kunt u er zeker van zijn dat de routelijnen de straatlabels niet bedekken. Er wordt een symboollaag gemaakt en aan de gegevensbron gekoppeld. Deze laag geeft aan hoe de begin- en eindpunten worden weergegeven. In dit geval zijn er expressies toegevoegd om de afbeelding van het pictogram en de tekstlabelgegevens op te halen uit de eigenschappen van elk puntobject.

2. Voor deze zelfstudie, het beginpunt niet instellen omdat een fictief bedrijf in Seattle Fabrikam, en het eindpunt als een gebouw in het hoofdkantoor van Microsoft aangeroepen. Voeg de volgende code toe aan de gebeurtenis-handler voor het laden van kaarten.

    ```JavaScript
    //Create the GeoJSON objects which represent the start and end point of the route.
    var startPoint = new atlas.data.Feature(new atlas.data.Point([-122.356099, 47.580045]), {
        title: 'Fabrikam, Inc.',
        icon: 'pin-blue'
    });

    var endPoint = new atlas.data.Feature(new atlas.data.Point([-122.201164, 47.616940]), {
        title: 'Microsoft - Lincoln Square',
        icon: 'pin-round-blue'
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

    De begin- en eindpunten worden toegevoegd aan de gegevensbron. Het begrenzingsvak voor de begin- en eindpunten wordt berekend met behulp van de functie `atlas.data.BoundingBox.fromData`. In dit vak wordt gebruikt voor het instellen van de overzichtsweergave van de camera's over de hele route via de `map.setCamera` functie. Er wordt opvulling toegevoegd om de grootte van de pixels in de symboolpictogrammen te compenseren.

4. Sla het bestand op en vernieuw de browser. De spelden worden weergegeven op de kaart. De kaart is nu gecentreerd op Seattle en u ziet de ronde blauwe speld die het beginpunt aangeeft en de blauwe speld die het eindpunt aangeeft.

   ![Kaart met begin- en eindpunt weergeven](./media/tutorial-prioritized-routes/pins-map.png)

<a id="multipleroutes"></a>

## <a name="render-routes-prioritized-by-mode-of-travel"></a>Routes weergeven gerangschikt naar de manier van reizen

Deze sectie wordt beschreven hoe u met de route service API van kaarten zoeken naar meerdere routes van een bepaald beginpunt naar het eindpunt op basis van de modus van transport. De routeservice biedt API's om de *snelste*, *kortste*, *zuinigste* of *leukste* route tussen twee locaties te plannen, waarbij rekening wordt gehouden met de huidige verkeersomstandigheden. Ook kunnen gebruikers in de toekomst routes plannen met behulp van de uitgebreide historische verkeersdatabase van Azure en de duur van de route voor elke dag en tijd voorspellen. Zie [GetRoute Directions](https://docs.microsoft.com/rest/api/maps/route/getroutedirections) (Routebeschrijvingen ophalen) voor meer informatie. Alle volgende codeblokken moeten worden toegevoegd **binnen de map.loadEventListener** om ervoor te zorgen dat de blokken worden geladen nadat de kaart volledig is geladen.

1. Voeg de volgende Javascript-code in de functie GetMap.

    ```JavaScript
    // Use SubscriptionKeyCredential with a subscription key
    var subscriptionKeyCredential = new atlas.service.SubscriptionKeyCredential(atlas.getSubscriptionKey());

    // Use subscriptionKeyCredential to create a pipeline
    var pipeline = atlas.service.MapsURL.newPipeline(subscriptionKeyCredential);

    // Construct the RouteURL object
    var routeURL = new atlas.service.RouteURL(pipeline);
    ```

   De **SubscriptionKeyCredential** maakt een **SubscriptionKeyCredentialPolicy** om HTTP-aanvragen naar Azure kaarten met de abonnementssleutel te verifiëren. De **atlas.service.MapsURL.newPipeline()** wordt in de **SubscriptionKeyCredential** beleid en maakt een [pijplijn](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.pipeline?view=azure-iot-typescript-latest) exemplaar. De **routeURL** vertegenwoordigt een URL naar Azure Maps [Route](https://docs.microsoft.com/rest/api/maps/route) bewerkingen.

2. Na het instellen van referenties en de URL toevoegen de volgende JavaScript code voor het maken van een route vanaf het begin naar het eindpunt voor een vrachtwagen uitvoering USHazmatClass2 lading ingedeeld en de resultaten weer te geven.

    ```JavaScript
    //Start and end point input to the routeURL
    var coordinates= [[startPoint.geometry.coordinates[0], startPoint.geometry.coordinates[1]], [endPoint.geometry.coordinates[0], endPoint.geometry.coordinates[1]]];

    //Make a search route request for a truck vehicle type
    routeURL.calculateRouteDirections(atlas.service.Aborter.timeout(10000), coordinates,{
        travelMode: 'truck',
        vehicleWidth: 2,
        vehicleHeight: 2,
        vehicleLength: 5,
        vehicleLoadType: 'USHazmatClass2'
    }).then((directions) => {
        //Get data features from response
        var data = directions.geojson.getFeatures();

        //Get the route line and add some style properties to it.  
        var routeLine = data.features[0];
        routeLine.properties.strokeColor = '#2272B9';
        routeLine.properties.strokeWidth = 9;

        //Add the route line to the data source. We want this to render below the car route which will likely be added to the data source faster, so insert it at index 0.
        datasource.add(routeLine, 0);
    });
    ```

    Dit codefragment bovenstaande query's voor de routering Azure Maps-service via de [getRouteDirections](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.models.routedirectionsrequestbody?view=azure-iot-typescript-latest) methode. De route-regel wordt vervolgens opgehaald uit de verzameling van de functie GeoJSON uit het antwoord dat wordt opgehaald met behulp van de **geojson.getFeatures()** methode. De route-regel wordt vervolgens toegevoegd aan de gegevensbron. Een index 0 om ervoor te zorgen dat deze wordt weergegeven vóór alle andere regels in de gegevensbron worden ook toegevoegd. Dit gebeurt omdat de berekende vrachtwagenroute vaak langzamer uitvalt dan een autoroute, en als de vrachtwagenroute na de autoroute aan de gegevensbron wordt toegevoegd, wordt de vrachtwagenroute bovenop de autoroute weergegeven. Er worden twee eigenschappen toegevoegd aan de lijn voor de vrachtwagenroute: een mooie blauwe lijnkleur en een lijndikte van 9 pixels.

3. Voeg de volgende JavaScript-code voor het maken van een route voor een auto en de resultaten weer te geven.

    ```JavaScript
    routeURL.calculateRouteDirections(atlas.service.Aborter.timeout(10000), coordinates).then((directions) => {

        //Get data features from response
        var data = directions.geojson.getFeatures();

        //Get the route line and add some style properties to it.  
        var routeLine = data.features[0];
        routeLine.properties.strokeColor = '#B76DAB';
        routeLine.properties.strokeWidth = 5;

        //Add the route line to the data source. We want this to render below the car route which will likely be added to the data source faster, so insert it at index 0.  
        datasource.add(routeLine);
    });
    ```

    Dit codefragment bovenstaande query's voor de routering Azure Maps-service via de [getRouteDirections](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.models.routedirectionsrequestbody?view=azure-iot-typescript-latest) methode. De route-regel wordt vervolgens opgehaald uit de verzameling van de functie GeoJSON uit het antwoord dat wordt opgehaald met behulp van de **geojson.getFeatures()** methode. De route-regel wordt vervolgens toegevoegd aan de gegevensbron. Er worden twee eigenschappen toegevoegd aan de lijn voor de autoroute: lijnkleur paars en lijndikte van 5 pixels.  

4. Sla het bestand **MapTruckRoute.html** op en vernieuw de browser om het resultaat te bekijken. Bij een succesvolle verbinding met de-API's van Maps ziet de kaart er ongeveer als volgt uit.

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
