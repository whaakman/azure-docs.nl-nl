---
title: Zoekresultaten met Azure-kaarten weergeven | Microsoft Docs
description: Hoe u een zoekopdracht uitvoert met Azure Maps aanvragen en vervolgens de resultaten weergeven op een Javascript-kaart
author: jingjing-z
ms.author: jinzh
ms.date: 08/31/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 7d4eb5f9be4a6bcefe4b544d3f97a9b9391c0d81
ms.sourcegitcommit: 31241b7ef35c37749b4261644adf1f5a029b2b8e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/04/2018
ms.locfileid: "43665774"
---
# <a name="show-search-results-on-the-map"></a>Zoekresultaten weergeven op de kaart

In dit artikel wordt beschreven hoe u zoeken naar de locatie van belang en de lijst met zoekresultaten weergeven op de kaart. 

## <a name="understand-the-code"></a>De code begrijpen

<iframe height='500' scrolling='no' title='Zoekresultaten weergeven op een kaart (Service-Module)' src='//codepen.io/azuremaps/embed/zLdYEB/?height=265&theme-id=0&default-tab=js,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/zLdYEB/'>zoekresultaten weergeven op een kaart (Service-Module)</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

In de bovenstaande code wordt het eerste vereiste blok van code vormt een kaartobject en waarmee een client-service wordt gemaakt. U kunt zien [maken van een kaart](./map-create.md) voor instructies.

Het tweede blok van code maakt gebruik van Fuzzy zoekopdrachten [Azure Maps Fuzzy zoeken-API](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy) om te zoeken naar nuttige plaats. Fuzzy zoeken-API kan elke combinatie van fuzzy invoer verwerken. Het antwoord van de service fuzzy zoeken wordt vervolgens geparseerd in de GeoJSON-notatie gebruiken de [getGeoJsonSearchResponse](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.geojson.geojsonsearchresponse?view=azure-iot-typescript-latest#geojsonsearchresponse) methode. De pincodes worden vervolgens toegevoegd aan de kaart om de punten van belang zijn op de kaart weer te geven.

Het laatste blok van code voegt camera grenzen voor de kaart toe met behulp van de kaart [setCameraBounds](https://docs.microsoft.com/javascript/api/azure-maps-control/models.cameraboundsoptions?view=azure-iot-typescript-latest) eigenschap.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de klassen en methoden die in dit artikel worden gebruikt: 

* [Azure kaarten-Fuzzy zoeken-API](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy)
* [Kaart](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)
    * [addPins](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addpins)
    
Zie de volgende artikelen voor meer codevoorbeelden van toevoegen aan uw kaarten: 
* [Gegevens ophalen uit een coördinaat](./map-get-information-from-coordinate.md)
* [Routebeschrijving van A naar B](./map-route.md)
