---
title: Het genereren van aangepaste gegevens op een rasterafbeeldingen van Azure Maps | Microsoft Docs
description: Aangepaste gegevens op een rasterafbeeldingen van Azure Maps worden weergegeven.
author: walsehgal
ms.author: v-musehg
ms.date: 02/12/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: 46f08aaa33563f620e7a011620730249e903f7b7
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58086595"
---
# <a name="render-custom-data-on-a-raster-map"></a>Aangepaste gegevens op een kaart raster weergeven

In dit artikel wordt uitgelegd hoe u de [statische afbeelding service](https://docs.microsoft.com/rest/api/maps/render/getmapimage) met functionaliteit voor samenstelling van afbeeldingen om toe te staan overlays boven op een rasterafbeeldingen. Samenstelling van de afbeelding biedt de mogelijkheid om op te halen van de tegel van een raster weer, met extra gegevens, zoals aangepaste punaises, labels en geometrie-overlays.

Voor het renderen van aangepaste punaises, labels en geometrie-overlays, kunt u de Postman-toepassing. U kunt Azure Maps [Data Service-API's](https://docs.microsoft.com/rest/api/maps/data) opslaan en weergeven van overlays.


## <a name="prerequisites"></a>Vereisten

### <a name="create-an-azure-maps-account"></a>Een Azure Maps-account maken

Als u wilt de procedures in dit artikel hebt voltooid, moet u eerst [maken van een Azure kaarten-account](how-to-manage-account-keys.md) in de prijscategorie S1.

## <a name="render-pushpins-with-labels-and-a-custom-image"></a>Punaises met labels en een aangepaste installatiekopie van weergeven

> [!Note]
> De procedure in deze sectie vereist een Azure kaarten-account in de prijscategorie S0 of S1.

De Azure-kaarten rekening S0-laag ondersteunt slechts één exemplaar van de `pins` parameter. Hiermee kunt u maximaal vijf punaises, opgegeven in de URL-aanvraag, met een aangepaste installatiekopie weergegeven.

Voer de volgende stappen uit voor het renderen van punaises met labels en een aangepaste installatiekopie:

1. Maak een verzameling waarin u voor het opslaan van de aanvragen. Selecteer in de Postman-app **nieuw**. In de **nieuw** venster **verzameling**. De naam van de verzameling en selecteer de **maken** knop. 

2. Selecteer voor het maken van de aanvraag, **nieuw** opnieuw. In de **nieuw** venster **aanvragen**. Voer een **Aanvraagnaam** voor de punaises, selecteert u de verzameling die u in de vorige stap hebt gemaakt als de locatie voor de aanvraag opslaan en selecteer vervolgens **opslaan**.
    
    ![Maken van een aanvraag in Postman](./media/tutorial-geofence/postman-new.png)

3. Selecteer de GET HTTP-methode op het tabblad builder en voer de volgende URL voor het maken van een GET-aanvraag.

    ```HTTP
    https://atlas.microsoft.com/map/static/png?subscription-key={subscription-key}&api-version=1.0&layer=basic&style=main&zoom=12&center=-73.98,%2040.77&pins=custom%7Cla15+50%7Cls12%7Clc003b61%7C%7C%27CentralPark%27-73.9657974+40.781971%7C%7Chttp%3A%2F%2Fazuremapscodesamples.azurewebsites.net%2FCommon%2Fimages%2Fpushpins%2Fylw-pushpin.png
    ```
    Dit is de afbeelding:

    ![Een aangepaste markeringspunt met een label](./media/how-to-render-custom-data/render-pins.png)


## <a name="get-data-from-azure-maps-data-storage"></a>Gegevens ophalen uit Azure Maps-gegevensopslag

> [!Note]
> De procedure in deze sectie vereist een Azure kaarten-account in de prijscategorie S1.

U kunt ook de locatie-informatie van het pad en de pincode verkrijgen met behulp van de [gegevens uploaden API](https://docs.microsoft.com/rest/api/maps/data/uploadpreview). Volg de stappen hieronder om de gegevens van het pad en pincodes te uploaden.

1. Open een nieuw tabblad in de verzameling die u in de vorige sectie hebt gemaakt in de Postman-app. Selecteer de HTTP POST-methode op het tabblad builder en voer de volgende URL als u wilt maken van een POST-aanvraag:

    ```HTTP
    https://atlas.microsoft.com/mapData/upload?subscription-key={subscription-key}&api-version=1.0&dataFormat=geojson
    ```

2. Op de **Params** tabblad, voer de volgende sleutel/waarde-paren, die worden gebruikt voor de POST-aanvraag-URL. Vervang de `subscription-key` waarde met de sleutel van uw Azure Maps-abonnement.
    
    ![Sleutel/waarde-parameters in Postman](./media/how-to-render-custom-data/postman-key-vals.png)

3. Op de **hoofdtekst** tabblad, selecteert u de onbewerkte invoerindeling en JSON als de invoerindeling kiezen uit de vervolgkeuzelijst. Deze JSON opgeven als gegevens worden geüpload:
    
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

4. Selecteer **verzenden** en bekijk de antwoord-header. De location-header bevat de URI die wordt gebruikt voor toegang tot of het downloaden van de gegevens voor toekomstig gebruik. Deze bevat ook een unieke `udId` voor de geüploade gegevens.  

   ```HTTP
   https://atlas.microsoft.com/mapData/{udId}/status?api-version=1.0&subscription-key={Subscription-key}
   ```

5. Gebruik de `udId` ontvangen waarde van de gegevens uploaden API om functies op de kaart weer te geven. U doet dit door een nieuw tabblad te openen in de verzameling die u in de voorgaande sectie hebt gemaakt. Selecteer de GET HTTP-methode op het tabblad builder en voer deze URL om te maken van een GET-aanvraag:

    ```HTTP
    https://atlas.microsoft.com/map/static/png?subscription-key={subscription-key}&api-version=1.0&layer=basic&style=main&zoom=12&center=-73.96682739257812%2C40.78119135317995&pins=default|la-35+50|ls12|lc003C62|co9B2F15||'Times Square'-73.98516297340393 40.758781646381024|'Central Park'-73.96682739257812 40.78119135317995&path=lc0000FF|fc0000FF|lw3|la0.80|fa0.30||udid-{udId}
    ```

6. Dit is de afbeelding:

    ![Gegevens ophalen uit Azure Maps-gegevensopslag](./media/how-to-render-custom-data/uploaded-path.png)

## <a name="render-a-polygon-with-color-and-opacity"></a>Een veelhoek met kleur en dekking weergeven

> [!Note]
> De procedure in deze sectie vereist een Azure kaarten-account in de prijscategorie S1.


U kunt het uiterlijk van een polygoon wijzigen met behulp van de stijl van parameters met de [padparameter](https://docs.microsoft.com/rest/api/maps/render/getmapimage#uri-parameters).

1. Open een nieuw tabblad in de verzameling die u eerder hebt gemaakt in de Postman-app. Selecteer de GET HTTP-methode op het tabblad builder en voer de volgende URL voor het configureren van een GET-aanvraag om een veelhoek met kleur en dekking weer te geven:
    
    ```HTTP
    https://atlas.microsoft.com/map/static/png?api-version=1.0&style=main&layer=basic&sku=S1&zoom=14&height=500&Width=500&center=-74.040701, 40.698666&path=lc0000FF|fc0000FF|lw3|la0.80|fa0.50||-74.03995513916016 40.70090237454063|-74.04082417488098 40.70028420372218|-74.04113531112671 40.70049568385827|-74.04298067092896 40.69899904076542|-74.04271245002747 40.69879568992435|-74.04367804527283 40.6980961582905|-74.04364585876465 40.698055487620714|-74.04368877410889 40.698022951066996|-74.04168248176573 40.696444909137|-74.03901100158691 40.69837271818651|-74.03824925422668 40.69837271818651|-74.03809905052185 40.69903971085914|-74.03771281242369 40.699340668780984|-74.03940796852112 40.70058515602143|-74.03948307037354 40.70052821920425|-74.03995513916016 40.70090237454063
    &subscription-key={subscription--key}
    ```

Dit is de afbeelding:

![Een ondoorzichtige veelhoek weergeven](./media/how-to-render-custom-data/opaque-polygon.png)


## <a name="render-a-circle-and-pushpins-with-custom-labels"></a>Een cirkel- en punaises met aangepaste labels weergeven

> [!Note]
> De procedure in deze sectie vereist een Azure kaarten-account in de prijscategorie S1.


U kunt punaises en de bijbehorende labels groter of kleiner maken met behulp van de `sc` schaal stijl modifier. Deze optie wordt een waarde die groter is dan nul. De waarde 1 is de standaardschaal. Waarden die groter zijn dan 1 wordt de pincodes groter maken, en waarden die kleiner zijn dan 1 zorgt ervoor dat deze kleiner. Zie voor meer informatie over de stijl van parameters [statische afbeelding service padparameters](https://docs.microsoft.com/rest/api/maps/render/getmapimage#uri-parameters).


Volg deze stappen voor het renderen van een cirkel- en punaises met aangepaste labels:

1. Open een nieuw tabblad in de verzameling die u eerder hebt gemaakt in de Postman-app. Selecteer de GET HTTP-methode op het tabblad builder en voer deze URL om te maken van een GET-aanvraag:

    ```HTTP
    https://atlas.microsoft.com/map/static/png?api-version=1.0&style=main&layer=basic&zoom=14&height=700&Width=700&center=-122.13230609893799,47.64599069048016&path=lcFF0000|lw2|la0.60|ra1000||-122.13230609893799 47.64599069048016&pins=default|la15+50|al0.66|lc003C62|co002D62||'Microsoft Corporate Headquarters'-122.14131832122801  47.64690503939462|'Microsoft Visitor Center'-122.136828 47.642224|'Microsoft Conference Center'-122.12552547454833 47.642940335653996|'Microsoft The Commons'-122.13687658309935  47.64452336193245&subscription-key={subscription-key}
    ```

Dit is de afbeelding:

![Een cirkel met aangepaste punaises weergeven](./media/how-to-render-custom-data/circle-custom-pins.png)

## <a name="next-steps"></a>Volgende stappen


* Verken de [Azure Maps ophalen Map installatiekopie API](https://docs.microsoft.com/rest/api/maps/render/getmapimage) documentatie.
* Zie voor meer informatie over Azure Maps Data Service, de [documentatie voor service](https://docs.microsoft.com/rest/api/maps/data).

