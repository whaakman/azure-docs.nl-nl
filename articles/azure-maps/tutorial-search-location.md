---
title: Zoeken met Azure Maps | Microsoft Docs
description: Zoeken naar nuttige plaatsen in de buurt met behulp van Azure Maps
author: walsehgal
ms.author: v-musehg
ms.date: 12/14/2018
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: 1396260ef703ce22f8e0309bd2c8df691d0af86e
ms.sourcegitcommit: 818d3e89821d101406c3fe68e0e6efa8907072e7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/09/2019
ms.locfileid: "54120458"
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
    * De *Naam* van uw nieuwe account.
    * Het *Abonnement* dat u wilt gebruiken voor dit account.
    * De naam van de *Resourcegroep* voor dit account. U kunt kiezen om een *Nieuwe* of *Bestaande* resourcegroep te gebruiken.
    * Selecteer een *Loacatie voor de resourcegroep*.
    * Lees de *licentie* en de *privacyverklaring*, en schakel het selectievakje in om de voorwaarden te accepteren.
    * Klik op de knop **Maken**.

    ![Maps-account maken in de portal](./media/tutorial-search-location/create-account.png)

<a id="getkey"></a>

## <a name="get-the-primary-key-for-your-account"></a>De primaire sleutel voor uw account ophalen

Als het Azure Kaarten-account is gemaakt, haalt u de sleutel op waarmee u query's kunt uitvoeren op de API's van kaarten.

1. Open uw Maps-account in de portal.
2. Selecteer **Sleutels** in de sectie Instellingen.
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
      <meta charset="utf-8" />
      <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no" />

      <!-- Add references to the Azure Maps Map control JavaScript and CSS files. -->
      <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/css/atlas.min.css?api-version=1" type="text/css" />
      <script src="https://atlas.microsoft.com/sdk/js/atlas.min.js?api-version=1"></script>

      <!-- Add a reference to the Azure Maps Services Module JavaScript file. -->
      <script src="https://atlas.microsoft.com/sdk/js/atlas-service.js?api-version=1"></script>

      <script>      
         var map, datasource, client, popup;

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

   U ziet dat de HTML-header de CSS- en JavaScript-bronbestanden bevat, gehost door de Azure Map Control-bibliotheek. Let op de gebeurtenis `onload` in het hoofdtekstgedeelte van de pagina. Deze zorgt ervoor dat de functie `GetMap` wordt aangeroepen nadat het hoofdtekstgedeelte van de pagina is geladen. Deze functie bevat de inline JavaScript-code voor toegang tot de API's van Azure Maps.

3. Voeg de volgende JavaScript-code toe aan de functie `GetMap` van het HTML-bestand. Vervang de tekenreeks **\<Your Azure Maps Key\>** door de primaire sleutel die u hebt gekopieerd in uw Maps-account.

   ```JavaScript
   //Add your Azure Maps subscription key to the map SDK. Get an Azure Maps key at https://azure.com/maps
   atlas.setSubscriptionKey('<Your Azure Maps Key>');

   //Initialize a map instance.
   map = new atlas.Map('myMap');
   ```

   Dit segment initialiseert de Map Control-API voor de sleutel van uw Azure Maps-account. **Atlas** is de naamruimte die de API en gerelateerde visuele onderdelen bevat. **atlas.Map** biedt het besturingselement voor een visuele en interactieve webkaart. 

4. Sla de wijzigingen op in het bestand en open de HTML-pagina in een browser. Dit is de eenvoudigste kaart die u kunt maken door **atlas.map** aan te roepen en de sleutel van uw account op te geven.

   ![De kaart weergeven](./media/tutorial-search-location/basic-map.png)

5. Voeg de volgende JavaScript-code toe aan de functie `GetMap` nadat de kaart is geïnitialiseerd. 

   ```JavaScript
   //Wait until the map resources have fully loaded.
   map.events.add('load', function () {

      //Create a data source and add it to the map.
      datasource = new atlas.source.DataSource();
      map.sources.add(datasource);

      //Add a layer for rendering point data.
      var resultLayer = new atlas.layer.SymbolLayer(datasource, null, {
         iconOptions: {
            iconImage: 'pin-round-darkblue',
            anchor: 'center',
            allowOverlap: true
         }
      });
      map.layers.add(resultLayer);

      //Create a popup but leave it closed so we can update it and display it later.
      popup = new atlas.Popup();

      //Add a mouse over event to the result layer and display a popup when this event fires.
      map.events.add('mouseover', resultLayer, showPopup);
   });
   ```

   Er wordt aan de kaart een laadgebeurtenis toegevoegd die wordt geactiveerd wanneer alle kaartresources volledig zijn geladen. In de gebeurtenis-handler voor het laden van kaarten wordt een gegevensbron gemaakt om resultaatgegevens in op te slaan. Er wordt een symboollaag gemaakt en aan de gegevensbron gekoppeld. Deze laag geeft aan hoe de resultaatgegevens in de gegevensbron moeten worden weergegeven, in dit geval met een donkerblauw speldpictogram dat is gecentreerd over de resultatencoördinaat en dat andere pictogrammen de mogelijkheid biedt te overlappen. 

<a id="usesearch"></a>

## <a name="add-search-capabilities"></a>Zoekmogelijkheden toevoegen

In dit gedeelte ziet u hoe u met de Maps Search-API een nuttige plaats vindt op de kaart. Het is een RESTful-API die is bedoeld voor ontwikkelaars om te zoeken naar adressen, nuttige plaatsen en andere geografische informatie. De Search-service wijst een breedtegraad en lengtegraad toe aan een opgegeven adres. Met de **servicemodule** die hieronder wordt beschreven, kunt u zoeken naar een locatie met de Kaarten zoeken-API.

### <a name="service-module"></a>Servicemodule

1. Maak een instantie van de clientservice door de volgende Javascript-code toe te voegen aan de gebeurtenis-handler voor het laden van kaarten.

    ```JavaScript
    //Create an instance of the services client.
     client = new atlas.service.Client(atlas.getSubscriptionKey());
    ```

2. Voeg vervolgens het volgende scriptblok toe om de zoekquery te maken. Hierin wordt de service Fuzzy zoeken gebruikt, een basiszoek-API van de Search Service. Via de service Fuzzy zoeken wordt de meeste fuzzy invoer verwerkt, zoals adressen, plaatsen en nuttige plaatsen. Met deze code wordt gezocht naar benzinestations binnen de opgegeven radius. Het antwoord wordt vervolgens geparseerd in de GeoJSON-indeling en toegevoegd aan de gegevensbron. Dit leidt er automatisch toe dat de gegevens worden weergegeven op de kaart via de symboollaag. In het laatste deel van het script wordt de cameraweergave van de kaarten weergegeven met behulp van het selectiekader van de resultaten met behulp van de eigenschap [setCamera](/javascript/api/azure-maps-control/atlas.map#setcamera-cameraoptions---cameraboundsoptions---animationoptions-) van Maps. Er wordt een opvulling toegevoegd om te compenseren voor de pixelafmetingen van de symboolpictogrammen omdat het selectiekader wordt berekend op basis van de coördinaten. 
 
   ```JavaScript
   //Execute a POI search query then add the results to the map.
    client.search.getSearchPOI('gasoline station', {
        lat: 47.6292,
        lon: -122.2337,
        radius: 100000
    }).then(response => {
        //Parse the response into GeoJSON so that the map can understand.
        var geojsonResponse = new atlas.service.geojson.GeoJsonSearchResponse(response);
        var results = geojsonResponse.getGeoJsonResults();

        //Add the results to the data source so they can be rendered. 
        datasource.add(results);

        // Set the camera bounds
        map.setCamera({
            bounds: results.bbox,
            padding: 50
        });
    });
   ```
 
3. Sla het bestand **MapSearch.html** op en vernieuw de browser. U ziet nu dat de kaart is gecentreerd rond Seattle en dat blauwe spelden de locaties van tankstations in het gebied aangeven.

   ![De kaart met zoekresultaten weergeven](./media/tutorial-search-location/pins-map.png)

4. U kunt de onbewerkte gegevens die op de kaart worden weergegeven, zien door de volgende HTTPRequest in uw browser te typen. Vervang \<Your Azure Maps Key\> door de primaire sleutel.

   ```http
   https://atlas.microsoft.com/search/fuzzy/json?api-version=1.0&query=gasoline%20station&subscription-key=<Your Azure Maps Key>&lat=47.6292&lon=-122.2337&radius=100000
   ```

Op dit moment kunnen op de pagina MapSearch de locaties worden weergegeven van nuttige plaatsen die worden geretourneerd door een fuzzy zoekquery. Laten we enkele interactieve mogelijkheden en meer informatie over de locaties toevoegen.

## <a name="add-interactive-data"></a>Interactieve gegevens toevoegen

De kaart die we tot nu toe hebben gemaakt, is uitsluitend gebaseerd op de gegevens voor lengtegraad/breedtegraad voor de zoekresultaten. Als u kijkt naar de onbewerkte JSON die door de service Maps Search wordt geretourneerd, ziet u echter dat deze aanvullende informatie over elk tankstation bevat, inclusief de naam en het adres. U kunt die gegevens opnemen in de kaart met behulp van interactieve pop-upvakken.

1. Voeg de volgende regels code toe aan de gebeurtenis-handler voor het laden van kaarten na de code om een query uit te voeren op de service voor fuzzy zoeken. Hiermee wordt een exemplaar van een pop-upvenster gemaakt en een mouse-overgebeurtenis toegevoegd aan de symboollaag.

    ```JavaScript
   //Create a popup but leave it closed so we can update it and display it later.
    popup = new atlas.Popup();

    //Add a mouse over event to the result layer and display a popup when this event fires.
    map.events.add('mouseover', resultLayer, showPopup);
    ```
    
    De API **atlas.Popup** voorziet in een informatievenster dat is verankerd op de juiste positie op de kaart. 
      
2. Voeg in de tag *script* na de functie `GetMap` de volgende toe code om de resultaatgegevens van de mouse-overgebeurtenis in het pop-upvenster weer te geven. 

   ```JavaScript
   function showPopup(e) {
        //Get the properties and coordinates of the first shape that the event occurred on.
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

2. Sla het bestand op en vernieuw de browser. De kaart in de browser toont nu informatie in pop-ups wanneer u de muisaanwijzer over een van de spelden beweegt.

    ![Azure Map Control en Search Service](./media/tutorial-search-location/popup-map.png)

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Een account van Azure Maps maken
> * De primaire sleutel voor uw account ophalen
> * Nieuwe webpagina met Map Control API maken
> * Search Service gebruiken om nuttige plaatsen in de buurt te vinden

De voorbeeldcode voor deze zelfstudie vindt u hier:

> [Locatie zoeken met Azure Maps](https://github.com/Azure-Samples/AzureMapsCodeSamples/blob/master/AzureMapsCodeSamples/Tutorials/search.html)

[Het livevoorbeeld ziet u hier](https://azuremapscodesamples.azurewebsites.net/?sample=Search%20for%20points%20of%20interest)

De volgende zelfstudie laat zien hoe u een route tussen twee locaties weergeeft.

> [!div class="nextstepaction"]
> [Route naar een bestemming](./tutorial-route-location.md)
