---
title: Zoeken met Azure Maps | Microsoft Docs
description: Zoeken naar nuttige plaatsen in de buurt met behulp van Azure Maps
author: walsehgal
ms.author: v-musehg
ms.date: 03/07/2019
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: e787366694b55d3dcd61eec2c5a5b42dd187ec55
ms.sourcegitcommit: 920ad23613a9504212aac2bfbd24a7c3de15d549
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/15/2019
ms.locfileid: "68226840"
---
# <a name="search-nearby-points-of-interest-using-azure-maps"></a>Zoeken naar nuttige plaatsen in de buurt met behulp van Azure Maps

Deze zelfstudie laat zien hoe u een account van Azure Maps instelt en vervolgens de API's van Maps gebruikt om te zoeken naar een nuttige plaats. In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een Azure Maps-account maken
> * De primaire sleutel voor uw Maps-account ophalen
> * Een nieuwe webpagina maken met de kaartbesturingselement-API
> * De zoekservice van Maps gebruiken om een nuttige plaats in de buurt te vinden

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/) aan voordat u begint.

## <a name="sign-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal

Meld u aan bij [Azure Portal](https://portal.azure.com).

<a id="createaccount"></a>

## <a name="create-an-account-with-azure-maps"></a>Een account van Azure Maps maken

Voer de volgende stappen uit om een nieuw Maps-account te maken:

1. Klik in de linkerbovenhoek van [Azure Portal](https://portal.azure.com) op **Een resource maken**.
2. Typ **Maps** in het vak *Marketplace doorzoeken*.
3. Selecteer **Toewijzingen** in de *Resultaten*. Klik op de knop **Maken** die onder de kaart wordt weergegeven.
4. Voer de volgende waarden in op de pagina **Azure Kaarten-account maken**:
    * Het *Abonnement* dat u wilt gebruiken voor dit account.
    * De naam van de *Resourcegroep* voor dit account. U kunt kiezen om een *Nieuwe* of *Bestaande* resourcegroep te gebruiken.
    * De *Naam* van uw nieuwe account.
    * De *prijscategorie* voor dit account.
    * Lees de *licentie* en de *privacyverklaring*, en schakel het selectievakje in om de voorwaarden te accepteren.
    * Klik op de knop **Maken**.

![Maps-account maken in de portal](./media/tutorial-search-location/create-account.png)

<a id="getkey"></a>

## <a name="get-the-primary-key-for-your-account"></a>De primaire sleutel voor uw account ophalen

Als het Azure Kaarten-account is gemaakt, haalt u de sleutel op waarmee u query's kunt uitvoeren op de API's van kaarten.

1. Open uw Maps-account in de portal.
2. Selecteer in de sectie instellingen **verificatie**.
3. Kopieer de **Primaire Sleutel** naar het Klembord. Sla de sleutel lokaal op voor gebruik verderop in deze zelfstudie.

![Primaire sleutel in de portal ophalen](./media/tutorial-search-location/get-key.png)

<a id="createmap"></a>

## <a name="create-a-new-map"></a>Een nieuwe kaart maken

De Map Control-API is een handige clientbibliotheek waarmee u Maps eenvoudig kunt integreren in uw webtoepassing. Het verbergt de complexiteit van het aanroepen van de REST-service en verhoogt de productiviteit met stijlbare en aanpasbare onderdelen. Gebruik de volgende stappen voor het maken van een statische HTML-pagina, ingesloten met de Map Control-API.

1. Maak een nieuw bestand op uw lokale computer en noem dit **MapSearch.html**.
2. Voeg de volgende HTML-onderdelen toe aan het bestand:

   ```HTML
    <!DOCTYPE html>
    <html>
    <head>
        <title>Map Search</title>
        <meta charset="utf-8">
        <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">

        <!-- Add references to the Azure Maps Map control JavaScript and CSS files. -->
        <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.css" type="text/css">
        <script src="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.js"></script>

        <!-- Add a reference to the Azure Maps Services Module JavaScript file. -->
        <script src="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas-service.min.js"></script>

        <script>
        function GetMap(){
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

   U ziet dat de HTML-header de CSS- en JavaScript-bronbestanden bevat, gehost door de Azure Map Control-bibliotheek. Let op de gebeurtenis `onload` in het hoofdtekstgedeelte van de pagina. Deze zorgt ervoor dat de functie `GetMap` wordt aangeroepen nadat het hoofdtekstgedeelte van de pagina is geladen. De `GetMap` functie bevat de inline JavaScript-code voor toegang tot de Azure kaarten-API's.

3. Voeg de volgende JavaScript-code toe aan de functie `GetMap` van het HTML-bestand. Vervang de tekenreeks `<Your Azure Maps Key>` met de primaire sleutel die u hebt gekopieerd uit uw kaarten-account.

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

   Dit segment initialiseert de Map Control-API voor de sleutel van uw Azure Maps-account. `atlas` is de naamruimte die de API en gerelateerde visuele onderdelen bevat. `atlas.Map` biedt het besturingselement voor een visuele en interactieve webkaart.

4. Sla de wijzigingen op in het bestand en open de HTML-pagina in een browser. Dit is de meest eenvoudige toewijzing die u maken door het aanroepen van kunt `atlas.Map` met behulp van de accountsleutel van uw.

   ![De kaart weergeven](./media/tutorial-search-location/basic-map.png)

5. Voeg de volgende JavaScript-code toe aan de functie `GetMap` nadat de kaart is geïnitialiseerd.

    ```JavaScript
    //Wait until the map resources are ready.
    map.events.add('ready', function() {

        //Create a data source and add it to the map.
        datasource = new atlas.source.DataSource();
        map.sources.add(datasource);

        //Add a layer for rendering point data.
        var resultLayer = new atlas.layer.SymbolLayer(datasource, null, {
            iconOptions: {
                image: 'pin-round-darkblue',
                anchor: 'center',
                allowOverlap: true
            },
            textOptions: {
                anchor: "top"
            }
        });

        map.layers.add(resultLayer);
    });
    ```

   In deze codesegment een `ready` gebeurtenis wordt toegevoegd aan de kaart die wordt geactiveerd wanneer het toewijzen van resources zijn geladen en de kaart is gereed om te worden geopend. Op de kaart `ready` gebeurtenis-handler een gegevensbron wordt gemaakt voor het opslaan van gegevens. Er wordt een symboollaag gemaakt en aan de gegevensbron gekoppeld. Deze laag geeft aan hoe de resultaatgegevens in de gegevensbron moeten worden weergegeven, in dit geval met een donkerblauw speldpictogram dat is gecentreerd over de resultatencoördinaat en dat andere pictogrammen de mogelijkheid biedt te overlappen. De resultaat-laag wordt toegevoegd aan de kaartlagen.

<a id="usesearch"></a>

## <a name="add-search-capabilities"></a>Zoekmogelijkheden toevoegen

In deze sectie ziet u hoe u de toewijzingen [zoeken-API](https://docs.microsoft.com/rest/api/maps/search) bezienswaardigheden vinden op de kaart. Het is een RESTful-API die is bedoeld voor ontwikkelaars om te zoeken naar adressen, nuttige plaatsen en andere geografische informatie. De Search-service wijst een breedtegraad en lengtegraad toe aan een opgegeven adres. Met de **servicemodule** die hieronder wordt beschreven, kunt u zoeken naar een locatie met de Kaarten zoeken-API.

### <a name="service-module"></a>Servicemodule

1. Op de kaart `ready` gebeurtenis-handler het samenstellen van de search service-URL door de volgende Javascript-code toe te voegen.

    ```JavaScript
   // Use SubscriptionKeyCredential with a subscription key
   var subscriptionKeyCredential = new atlas.service.SubscriptionKeyCredential(atlas.getSubscriptionKey());

   // Use subscriptionKeyCredential to create a pipeline
   var pipeline = atlas.service.MapsURL.newPipeline(subscriptionKeyCredential);

   // Construct the SearchURL object
   var searchURL = new atlas.service.SearchURL(pipeline); 
   ```

   De `SubscriptionKeyCredential` maakt een `SubscriptionKeyCredentialPolicy` om HTTP-aanvragen naar Azure kaarten met de abonnementssleutel te verifiëren. De `atlas.service.MapsURL.newPipeline()` wordt in de `SubscriptionKeyCredential` beleid en maakt een [pijplijn](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.pipeline?view=azure-maps-typescript-latest) exemplaar. De `searchURL` vertegenwoordigt een URL naar Azure Maps [zoeken](https://docs.microsoft.com/rest/api/maps/search) bewerkingen.

2. Voeg vervolgens het volgende scriptblok toe om de zoekquery te maken. Hierin wordt de service Fuzzy zoeken gebruikt, een basiszoek-API van de Search Service. Via de service Fuzzy zoeken wordt de meeste fuzzy invoer verwerkt, zoals adressen, plaatsen en nuttige plaatsen. Deze code zoekt u in de buurt benzinestations binnen de opgegeven radius van de opgegeven lengtegraad en breedtegraad. Een verzameling van de functie GeoJSON uit het antwoord wordt vervolgens opgehaald met behulp van de `geojson.getFeatures()` methode en toegevoegd aan de gegevensbron, worden automatisch de gegevens die wordt weergegeven op de kaart via het symbool-laag. In het laatste deel van het script wordt de cameraweergave van de kaarten weergegeven met behulp van het selectiekader van de resultaten met behulp van de eigenschap [setCamera](/javascript/api/azure-maps-control/atlas.map#setcamera-cameraoptions---cameraboundsoptions---animationoptions-) van Maps.

    ```JavaScript
    var query =  'gasoline-station';
    var radius = 9000;
    var lat = 47.64452336193245;
    var lon = -122.13687658309935;

    searchURL.searchPOI(atlas.service.Aborter.timeout(10000), query, {
        limit: 10,
        lat: lat,
        lon: lon,
        radius: radius
    }).then((results) => {

        // Extract GeoJSON feature collection from the response and add it to the datasource
        var data = results.geojson.getFeatures();
        datasource.add(data);

        // set camera to bounds to show the results
        map.setCamera({
            bounds: data.bbox,
            zoom: 10
        });
    });
    ```

3. Sla het bestand **MapSearch.html** op en vernieuw de browser. U ziet nu dat de kaart wordt gecentreerd op Seattle met round-blauw pincodes markering van de locaties van benzinestations in het gebied.

   ![De kaart met zoekresultaten weergeven](./media/tutorial-search-location/pins-map.png)

4. U kunt de onbewerkte gegevens die op de kaart worden weergegeven, zien door de volgende HTTPRequest in uw browser te typen. Vervang \<Your Azure Maps Key\> door de primaire sleutel.

   ```http
   https://atlas.microsoft.com/search/poi/json?api-version=2&query=gasoline%20station&subscription-key=<subscription-key>&lat=47.6292&lon=-122.2337&radius=100000
   ```

Op dit moment kunnen op de pagina MapSearch de locaties worden weergegeven van nuttige plaatsen die worden geretourneerd door een fuzzy zoekquery. Laten we enkele interactieve mogelijkheden en meer informatie over de locaties toevoegen.

## <a name="add-interactive-data"></a>Interactieve gegevens toevoegen

De kaart die we tot nu toe hebben gemaakt, is uitsluitend gebaseerd op de gegevens voor lengtegraad/breedtegraad voor de zoekresultaten. Als u kijkt naar de onbewerkte JSON die door de service Maps Search wordt geretourneerd, ziet u echter dat deze aanvullende informatie over elk tankstation bevat, inclusief de naam en het adres. U kunt die gegevens opnemen in de kaart met behulp van interactieve pop-upvakken.

1. Voeg de volgende regels code op de kaart `ready` gebeurtenis-handler na de code om op te vragen van de service fuzzy zoeken. Hiermee wordt een exemplaar van een pop-upvenster gemaakt en een mouse-overgebeurtenis toegevoegd aan de symboollaag.

    ```JavaScript
   //Create a popup but leave it closed so we can update it and display it later.
    popup = new atlas.Popup();

    //Add a mouse over event to the result layer and display a popup when this event fires.
    map.events.add('mouseover', resultLayer, showPopup);
    ```

    De API `*atlas.Popup` voorziet in een venster verankerd op de juiste positie op de kaart. 

2. Voeg de volgende code binnen de `GetMap` functie om weer te geven de moused boven resultaatgegevens in het pop-upvenster.

    ```JavaScript
    function showPopup(e) {
        //Get the properties and coordinates of the first shape that the event occured on.

        var p = e.shapes[0].getProperties();
        var position = e.shapes[0].getCoordinates();

        //Create HTML from properties of the selected result.
        var html = ['<div style="padding:5px"><div><b>', p.poi.name,
            '</b></div><div>', p.address.freeformAddress,
            '</div><div>', position[1], ', ', position[0], '</div></div>'];

        //Update the content and position of the popup.
        popup.setPopupOptions({
            content: html.join(''),
            position: position
        });

        //Open the popup.
        popup.open(map);
    }
    ```

3. Sla het bestand op en vernieuw de browser. De kaart in de browser toont nu informatie in pop-ups wanneer u de muisaanwijzer over een van de spelden beweegt.

    ![Azure Map Control en Search Service](./media/tutorial-search-location/popup-map.png)

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Een account van Azure Maps maken
> * De primaire sleutel voor uw account ophalen
> * Nieuwe webpagina met Map Control API maken
> * Search Service gebruiken om nuttige plaatsen in de buurt te vinden

> [!div class="nextstepaction"]
> [De volledige broncode weergeven](https://github.com/Azure-Samples/AzureMapsCodeSamples/blob/master/AzureMapsCodeSamples/Tutorials/search.html)

> [!div class="nextstepaction"]
> [Live voorbeeld weergeven](https://azuremapscodesamples.azurewebsites.net/?sample=Search%20for%20points%20of%20interest)

De volgende zelfstudie laat zien hoe u een route tussen twee locaties weergeeft.

> [!div class="nextstepaction"]
> [Route naar een bestemming](./tutorial-route-location.md)
