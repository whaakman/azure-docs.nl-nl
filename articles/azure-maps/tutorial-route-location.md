---
title: Route zoeken met Azure Maps | Microsoft Docs
description: Zoeken naar een nuttige plaats met Azure Maps
author: walsehgal
ms.author: v-musehg
ms.date: 03/07/2019
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: 5c5465562c1af3dbd3fcaff2031149e510a43cfd
ms.sourcegitcommit: cf971fe82e9ee70db9209bb196ddf36614d39d10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/27/2019
ms.locfileid: "58540734"
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

   ![Basiskaart weergeven](./media/tutorial-route-location/basic-map.png)

## <a name="define-how-the-route-will-be-rendered"></a>Definiëren hoe de route wordt weergegeven

In deze zelfstudie wordt een eenvoudige route weergegeven met behulp van een symboolpictogram voor het begin en het einde van de route, en een lijn voor het routepad.

1. Na het initialiseren van de kaart toevoegen de volgende JavaScript-code.

    ```JavaScript
    //Wait until the map resources have fully loaded.
    map.events.add('load', function() {

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

    Er wordt aan de kaart een laadgebeurtenis toegevoegd die wordt geactiveerd wanneer alle kaartresources volledig zijn geladen. In de gebeurtenis-handler voor het laden van de kaart wordt een gegevensbron gemaakt waarin de routelijn, en de begin- en eindpunten worden opgeslagen. Een laag van de regel is gemaakt en gekoppeld aan de gegevensbron om te definiëren hoe de route-regel wordt weergegeven. De route-regel wordt weergegeven een tint van blauw met een breedte van 5 pixels en afgeronde line joins en limieten. Er wordt een filter toegevoegd om ervoor te zorgen dat met deze laag alleen GeoJSON LineString-gegevens worden toegevoegd. Als u de laag toevoegt aan de kaart wordt een tweede parameter met de waarde `'labels'` doorgegeven. Hiermee wordt bepaald dat deze laag onder de kaartlabels wordt weergegeven. Zo kunt u er zeker van zijn dat de routelijnen de straatlabels niet bedekken. Er wordt een symboollaag gemaakt en aan de gegevensbron gekoppeld. Deze laag geeft aan hoe de begin- en eindpunten worden weergegeven. In dit geval zijn er expressies toegevoegd om de afbeelding van het pictogram en de tekstlabelgegevens op te halen uit de eigenschappen van elk puntobject.

2. Voor deze zelfstudie, moet u het beginpunt instellen als de Microsoft-campus en het eindpunt als een Tankstation in Seattle. Voeg de volgende code toe aan de gebeurtenis-handler voor het laden van kaarten.

    ```JavaScript
    //Create the GeoJSON objects which represent the start and end points of the route.
    var startPoint = new atlas.data.Feature(new atlas.data.Point([-122.130137, 47.644702]), {
        title: "Redmond",
        icon: "pin-blue"
    });

    var endPoint = new atlas.data.Feature(new atlas.data.Point([-122.3352, 47.61397]), {
        title: "Seattle",
        icon: "pin-round-blue"
    });

    //Add the data to the data source.
    datasource.add([startPoint, endPoint]);

    map.setCamera({
        bounds: atlas.data.BoundingBox.fromData([startPoint, endPoint]),
        padding: 80
    });
    ```

    Deze code maakt twee [GeoJSON Point-objecten](https://en.wikipedia.org/wiki/GeoJSON) om weer te geven van de begin- en eindpunten van de route en de punten worden toegevoegd aan de gegevensbron. Aan elk punt wordt een eigenschap `title` en `icon` toegevoegd. Het laatste blok Hiermee stelt u de weergave van de camera met behulp van de gegevens voor breedtegraad en lengtegraad van de begin- en, met behulp van de kaart [setCamera](/javascript/api/azure-maps-control/atlas.map#setcamera-cameraoptions---cameraboundsoptions---animationoptions-) eigenschap.

3. Sla het bestand **MapRoute.html** op en vernieuw de browser. Nu de kaart wordt gecentreerd via Seattle en ziet u de blauwe pincode markeren van het eindpunt en de ronde blauwe pincode markering van de punt voltooien.

   ![Kaart met gemarkeerd begin- en eindpunt weergeven](./media/tutorial-route-location/map-pins.png)

<a id="getroute"></a>

## <a name="get-directions"></a>Routebeschrijving ophalen

Deze sectie wordt beschreven hoe route service API van Azure Maps gebruiken om te bepalen van de route van een bepaald beginpunt naar eindpunt. De routeservice biedt API's om de *snelste*, *kortste*, *zuinigste* of *leukste* route tussen twee locaties te plannen. Ook kunnen gebruikers in de toekomst routes plannen met behulp van de uitgebreide historische verkeersdatabase van Azure en de duur van de route voor elke dag en tijd voorspellen. Zie [Routebeschrijvingen ophalen](https://docs.microsoft.com/rest/api/maps/route/getroutedirections) voor meer informatie. Alle van de volgende functies moeten worden toegevoegd **binnen de eventListener voor het laden van de kaart** om ervoor te zorgen dat deze worden geladen nadat de kaart volledig is geladen.

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

2. Na het instellen van referenties en de URL, de volgende JavaScript-code kan de route van begin tot eindpunt toevoegen De **routeURL** aanvragen voor de Azure-kaarten-route service route routebeschrijvingen berekenen. Een verzameling van de functie GeoJSON uit het antwoord wordt vervolgens opgehaald met behulp van de **geojson.getFeatures()** methode en toegevoegd aan de gegevensbron.

    ```JavaScript
    //Start and end point input to the routeURL
    var coordinates= [[startPoint.geometry.coordinates[0], startPoint.geometry.coordinates[1]], [endPoint.geometry.coordinates[0], endPoint.geometry.coordinates[1]]];

    //Make a search route request
    routeURL.calculateRouteDirections(atlas.service.Aborter.timeout(10000), coordinates).then((directions) => {
        //Get data features from response
        var data = directions.geojson.getFeatures();
        datasource.add(data);
    });
    ```

3. Sla het bestand **MapRoute.html** op en vernieuw de webbrowser. Bij een succesvolle verbinding met de API's van Maps ziet de kaart er ongeveer als volgt uit.

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
