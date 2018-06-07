---
title: Het gebruik van het Kaartbesturingselement van Azure Maps | Microsoft Docs
description: Informatie over het gebruik van de Azure Maps Kaartbesturingselement clientzijde Javascript-bibliotheek.
author: kgremban
ms.author: kgremban
ms.date: 05/07/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: 4ba4b9a9f2357d283ddc03a4723cb08b48d40a9b
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34599208"
---
# <a name="how-to-use-the-azure-maps-map-control"></a>Het gebruik van het Kaartbesturingselement van Azure Maps
Het Kaartbesturingselement clientzijde Javascript-bibliotheek kunt u het weergeven van maps en ingesloten toegewezen Azure-functionaliteit in uw web- of mobiele toepassing. 

## <a name="create-a-new-map-in-a-web-page"></a>Een nieuwe map maken in een webpagina

U kunt een kaart insluiten in een webpagina met behulp van het Kaartbesturingselement clientzijde Javascript-bibliotheek.

1. Maak een nieuw bestand en noem deze MapSearch.html.

2. Toevoegen van de Azure-kaarten opmaakmodel en het script bron verwijzingen naar de `<head>` element van het bestand:

    ```html
    <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/css/atlas.min.css?api-version=1.0" type="text/css" />
    <script src="https://atlas.microsoft.com/sdk/js/atlas.min.js?api-version=1.0"></script>
    ```
    
3. Om een nieuwe kaart in uw browser renderen, Voeg een **#map** verwijzing in de `<style>` element.

    ```html
    #map {
                width: 100%;
                height: 100%;
            }
    ``` 
    
4. Als u wilt initialiseren van het kaartbesturingselement, het definiëren van een nieuwe sectie in de hoofdtekst html en maakt u een script. Gebruik de sleutel van uw eigen Azure Maps-account in het script. Als u wilt maken van een account of het vinden van uw sleutel, Zie [uw Azure-Maps-account en de sleutels beheren](how-to-manage-account-keys.md)

    ```html
    <div id="map">
        <script>
            var MapsAccountKey = "<_your account key_>";
            var map = new atlas.Map("map", {
                "subscription-key": MapsAccountKey,
                center: [47.59093,-122.33263],
                zoom: 12
            });
        </script>
    </div>
    ```
    
5. Open het bestand in uw webbrowser en bekijk de gerenderde kaart.

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd u een eenvoudige kaart maken met de sleutel van uw Azure-kaarten. Zie de volgende artikelen voor meer voorbeelden van programmacode toevoegen aan uw maps: 

* [Een map maken](map-create.md)
* [Toevoegen van een pincode](map-add-pin.md)