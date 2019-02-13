---
title: Het genereren van aangepaste gegevens op rasterafbeeldingen van Azure Maps | Microsoft Docs
description: Aangepaste gegevens op rasterafbeeldingen van Azure Maps worden weergegeven.
author: walsehgal
ms.author: v-musehg
ms.date: 02/12/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: 9b3c0f7b1ff56cb269f6852be8fd2affeca8b8f1
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/12/2019
ms.locfileid: "56143766"
---
# <a name="render-custom-data-on-raster-map"></a>Maken van aangepaste gegevens op rasterafbeeldingen

In dit artikel leest u hoe u [statische afbeelding service](https://docs.microsoft.com/rest/api/maps/render/getmapimage) met functionaliteit voor samenstelling van afbeeldingen om toe te staan overlays boven op een rasterafbeeldingen. Samenstelling van de afbeelding biedt de mogelijkheid om op te halen van een tegel raster met aanvullende gegevens, zoals aangepaste punaises, labels en geometrie-overlays. Voor het renderen van aangepaste punaises, labels en geometry, gebruiken we de postman-toepassing. Open de Postman-app, klikt u op Nieuw | Nieuw maken, selecteren en een verzameling of een map die u wilt opslaan op naam en klik op opslaan.

We gebruiken Azure Maps [Data Service-API's](https://docs.microsoft.com/rest/api/maps/data) opslaan en weergeven van overlays. 


## <a name="prerequisites"></a>Vereisten

### <a name="create-an-azure-maps-account"></a>Een Azure Maps-account maken 

Als u wilt volgen de stappen in deze handleiding, moet u eerst om te zien [account en sleutels beheren](how-to-manage-account-keys.md) maken en beheren van uw account-abonnement met S1 prijscategorie.

## <a name="render-pushpins-with-labels-and-custom-image"></a>Punaises met labels en de aangepaste afbeelding weergeven

> [!Note]
> In dit voorbeeld is een Azure kaarten-account met de prijscategorie S0 of S1 vereist. 

De Azure-kaarten-account S0-SKU biedt alleen ondersteuning voor één exemplaar van de `pins` parameter zodat gebruikers om maximaal 5 punaises opgegeven in de url-aanvraag met een aangepaste installatiekopie weer te geven.

Voor het renderen van push stappen pincodes met labels en aangepaste installatiekopie van de volgende:

1. Open de Postman-app en klik op Nieuw | Maak een nieuwe, en selecteer aanvraag. Voer een Aanvraagnaam voor render punaises, selecteert u een verzameling of een map die u wilt opslaan op en klik op opslaan.
    
    ![Uploaden van geofences met Postman](./media/tutorial-geofence/postman-new.png)

2. Selecteer GET HTTP-methode op het tabblad builder en voer de volgende URL als u wilt maken van een GET-aanvraag.

    ```HTTP
    https://atlas.microsoft.com/map/static/png?subscription-key={subscription-key}&api-version=1.0&layer=basic&style=main&zoom=12&center=-73.98,%2040.77&pins=custom%7Cla15+50%7Cls12%7Clc003b61%7C%7C%27CentralPark%27-73.9657974+40.781971%7C%7Chttp%3A%2F%2Fazuremapscodesamples.azurewebsites.net%2FCommon%2Fimages%2Fpushpins%2Fylw-pushpin.png
    ```
    Hieronder volgt de installatiekopie van het antwoord krijgt.

    ![aangepaste punaises met labels weergeven](./media/how-to-render-custom-data/render-pins.png)


## <a name="get-data-from-azure-maps-data-storage"></a>Gegevens ophalen uit Azure Maps-gegevensopslag

> [!Note]
> In dit voorbeeld vereist Azure kaarten-account met de prijscategorie S1.

De locatie-informatie van het pad en de pincodes kan ook worden verkregen via [gegevens uploaden API](https://docs.microsoft.com/rest/api/maps/mapdata/upload). Volg de stappen hieronder om de gegevens van het pad en pincodes te uploaden.

1. Open een nieuw tabblad in dezelfde verzameling die u hierboven hebt gemaakt in de Postman-app. Selecteer HTTP POST-methode op het tabblad builder en voer de volgende URL als u wilt maken van een POST-aanvraag:

    ```HTTP
    https://atlas.microsoft.com/mapData/upload?subscription-key={subscription-key}&api-version=1.0&dataFormat=geojson
    ```

2. Klik op parameters en voer de volgende sleutel/waarde-paren moet worden gebruikt voor de POST-aanvraag-URL. Abonnementssleutel waarde vervangen door uw abonnementssleutel Azure Maps.
    
    ![Sleutel / waarde-parameters Postman](./media/how-to-render-custom-data/postman-key-vals.png)

3. Klik op **hoofdtekst** vervolgens onbewerkte invoer indeling selecteren en JSON als de invoerindeling kiezen uit de vervolgkeuzelijst. Geef de volgende JSON als gegevens worden geüpload:
    
    ```JSON
    {
      "type": "FeatureCollection",
      "features": [
        {
          "type": "Feature",
          "properties": {},
          "geometry": {
            "type": "Polygon",
            "coordinates": [
              [
                [
                  -73.98235,
                  40.76799
                ],
                [
                  -73.95785,
                  40.80044
                ],
                [
                  -73.94928,
                  40.7968
                ],
                [
                  -73.97317,
                  40.76437
                ],
                [
                  -73.98235,
                  40.76799
                ]
              ]
            ]
          }
        },
        {
          "type": "Feature",
          "properties": {},
          "geometry": {
            "type": "LineString",
            "coordinates": [
              [
                -73.97624731063843,
                40.76560773817073
              ],
              [
                -73.97914409637451,
                40.766826609362575
              ],
              [
                -73.98513078689575,
                40.7585866048861
              ]
            ]
          }
        }
      ]
    }
    ```

4. Klik op verzenden en controleren van de response-header. De location-header bevat de URI voor het openen of downloaden van de gegevens voor toekomstig gebruik. Het bevat ook een unieke `udId` voor de geüploade gegevens.   

  ```HTTP
  https://atlas.microsoft.com/mapData/{udId}/status?api-version=1.0&subscription-key={Subscription-key}
  ```



5. Gebruik de waarde van `udid` ontvangen van de gegevens uploaden API om functies op de kaart weer te geven. Om dit te doen, opent u een nieuw tabblad in dezelfde verzameling die u hierboven hebt gemaakt. Selecteer GET HTTP-methode op het tabblad builder en voer de volgende URL als u wilt maken van een GET-aanvraag:

    ```HTTP
    https://atlas.microsoft.com/map/static/png?subscription-key={subscription-key}&api-version=1.0&layer=basic&style=main&zoom=12&center=-73.96682739257812%2C40.78119135317995&pins=default|la-35+50|ls12|lc003C62|co9B2F15||'Times Square'-73.98516297340393 40.758781646381024|'Central Park'-73.96682739257812 40.78119135317995&path=lc0000FF|fc0000FF|lw3|la0.80|fa0.30||udid-{udId}
    ```

6. De installatiekopie van het antwoord ziet er als volgt uit:

    ![geüploade gegevens weergeven](./media/how-to-render-custom-data/uploaded-path.png)

## <a name="render-polygon-with-color-and-opacity"></a>Veelhoek met kleur en dekking weergeven

> [!Note]
> In dit voorbeeld vereist Azure kaarten-account met de prijscategorie S1.

U kunt het uiterlijk van een polygoon wijzigen met behulp van de stijl van parameters met de [padparameter](https://docs.microsoft.com/rest-staging/api/maps/render/getmapimage#uri-parameters).

1. Open een nieuw tabblad in dezelfde verzameling die u hierboven hebt gemaakt in de Postman-app. Selecteer GET HTTP-methode op het tabblad builder en voer de volgende URL als u wilt maken van een GET-aanvraag om een veelhoek met kleur en dekking weer te geven:
    
    ```HTTP
    https://atlas.microsoft.com/map/static/png?api-version=1.0&style=main&layer=basic&sku=S1&zoom=14&height=500&Width=500&center=-74.040701, 40.698666&path=lc0000FF|fc0000FF|lw3|la0.80|fa0.50||-74.03995513916016 40.70090237454063|-74.04082417488098 40.70028420372218|-74.04113531112671 40.70049568385827|-74.04298067092896 40.69899904076542|-74.04271245002747 40.69879568992435|-74.04367804527283 40.6980961582905|-74.04364585876465 40.698055487620714|-74.04368877410889 40.698022951066996|-74.04168248176573 40.696444909137|-74.03901100158691 40.69837271818651|-74.03824925422668 40.69837271818651|-74.03809905052185 40.69903971085914|-74.03771281242369 40.699340668780984|-74.03940796852112 40.70058515602143|-74.03948307037354 40.70052821920425|-74.03995513916016 40.70090237454063
    &subscription-key={subscription--key}
    ```

De installatiekopie van het antwoord ziet er als volgt uit:

![ondoorzichtige veelhoek weergeven](./media/how-to-render-custom-data/opaque-polygon.png)


## <a name="render-polygon-with-circle-and-push-pins-with-custom-labels"></a>Veelhoek met cirkel- en push pincodes met aangepaste labels weergeven

> [!Note]
> In dit voorbeeld vereist Azure kaarten-account met de prijscategorie S1.

U kunt punaises en de bijbehorende labels groter of kleiner maken met behulp van de modifier 'sc' scale stijl. Dit is een waarde die groter zijn dan nul. De waarde 1 is de standaardschaal. Waarden die groter zijn dan 1 wordt de pincodes groter maken, en waarden die kleiner zijn dan 1 zorgt ervoor dat deze kleiner. Zie voor meer informatie op de stijl van parameters, [statische afbeelding service padparameters](https://docs.microsoft.com/rest/api/maps/render/getmapimage#uri-parameters).

De volgende stappen om een veelhoek met cirkel- en push pincodes met aangepaste labels weer te geven:

1. Open een nieuw tabblad in dezelfde verzameling die u hierboven hebt gemaakt in de Postman-app. Selecteer GET HTTP-methode op het tabblad builder en voer de volgende URL als u wilt maken van een GET-aanvraag:

    ```HTTP
    https://atlas.microsoft.com/map/static/png?api-version=1.0&style=main&layer=basic&zoom=14&height=700&Width=700&center=-122.13230609893799,47.64599069048016&path=lcFF0000|lw2|la0.60|ra1000||-122.13230609893799 47.64599069048016&pins=default|la15+50|al0.66|lc003C62|co002D62||'Microsoft Corporate Headquarters'-122.14131832122801  47.64690503939462|'Microsoft Visitor Center'-122.136828 47.642224|'Microsoft Conference Center'-122.12552547454833 47.642940335653996|'Microsoft The Commons'-122.13687658309935  47.64452336193245&subscription-key={subscription-key}
    ```

De installatiekopie van het antwoord ziet er als volgt uit:

![cirkel met aangepaste pincodes weergeven](./media/how-to-render-custom-data/circle-custom-pins.png)

## <a name="next-steps"></a>Volgende stappen

* Verken de [Azure Maps ophalen Map installatiekopie API](https://docs.microsoft.com/rest/api/maps/search) documentatie.
* Zie voor meer informatie over de functies van Azure Maps Data Service, [documentatie voor service](https://docs.microsoft.com/rest/api/maps/data).