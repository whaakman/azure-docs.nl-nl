---
title: Het gebruik van Azure Maps Map Control | Microsoft Docs
description: Informatie over het gebruik van de Map Control van Azure Maps-client-side Javascript-bibliotheek.
author: dsk-2015
ms.author: dkshir
ms.date: 09/05/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: 5b8703c218790549a0cf5a319345132a0eca66ce
ms.sourcegitcommit: d211f1d24c669b459a3910761b5cacb4b4f46ac9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/06/2018
ms.locfileid: "44025186"
---
# <a name="how-to-use-the-azure-maps-map-control"></a>Het gebruik van het Kaartbesturingselement van Azure Maps
De Map Control-client-side Javascript-bibliotheek kunt u voor rendering van kaarten en ingesloten Azure kaarten-functionaliteit in uw web- of mobiele toepassing. 

## <a name="create-a-new-map-in-a-web-page"></a>Een nieuwe map maken in een webpagina

U kunt een kaart insluiten in webpagina's met behulp van de Map Control-client-side Javascript-bibliotheek.

1. Maak een nieuw bestand en geef deze de naam MapSearch.html.

2. Toevoegen van de Azure-kaarten stylesheet en script bron verwijzingen naar de `<head>` element van het bestand:

    ```html
    <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/css/atlas.min.css?api-version=1" type="text/css" />
    <script src="https://atlas.microsoft.com/sdk/js/atlas.min.js?api-version=1"></script>
    ```
    
3. Toevoegen als u wilt weergeven van een nieuwe kaart in uw browser, een **#map** verwijzing in de `<style>` element.

    ```html
    #map {
                width: 100%;
                height: 100%;
            }
    ``` 
    
4. Als u wilt het kaartbesturingselement initialiseren, definieert u een nieuwe sectie in de HTML-tekst en een script maken. Gebruik de accountsleutel van uw eigen Azure-kaarten in het script. Als u nodig hebt voor het maken van een account of het vinden van uw sleutel, Zie [hoe u uw Azure kaarten-account en sleutels beheren](how-to-manage-account-keys.md)

    ```html
    <div id="map">
        <script>
            var MapsAccountKey = "<_your account key_>";
            var map = new atlas.Map("map", {
                "subscription-key": MapsAccountKey,
                center: [-122.33263,47.59093],
                zoom: 12
            });
        </script>
    </div>
    ```
    
5. Open het bestand in uw webbrowser en de gerenderde kaart weergeven.

## <a name="next-steps"></a>Volgende stappen

In dit artikel laat zien hoe u een eenvoudige kaart maken met uw Azure-kaarten-sleutel. Zie de volgende artikelen voor meer codevoorbeelden van toevoegen aan uw kaarten: 

* [Een kaart maken](map-create.md)
* [Kies een stijl kaart](choose-map-style.md)
