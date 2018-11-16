---
title: Zoekresultaten met Azure-kaarten weergeven | Microsoft Docs
description: Hoe u een zoekopdracht uitvoert met Azure Maps aanvragen en vervolgens de resultaten weergeven op een Javascript-kaart
author: jingjing-z
ms.author: jinzh
ms.date: 09/07/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: cf27864d691fe2fe13c9483348fb2abed121874d
ms.sourcegitcommit: a4e4e0236197544569a0a7e34c1c20d071774dd6
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/15/2018
ms.locfileid: "51713500"
---
# <a name="show-search-results-on-the-map"></a>Zoekresultaten weergeven op de kaart

In dit artikel wordt beschreven hoe u zoeken naar de locatie van belang en de lijst met zoekresultaten weergeven op de kaart.

Er zijn twee manieren om te zoeken naar een locatie van belang zijn. Een manier is een servicemodule gebruiken om te maken van een zoekaanvraag. De andere manier is om te zoeken vragen via een [XMLHttpRequest](https://xhr.spec.whatwg.org/) naar [Azure Maps Fuzzy zoeken-API](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy). Beide methoden worden hieronder besproken.

## <a name="make-a-search-request-via-service-module"></a>Een zoekopdracht indienen via de servicemodule

<iframe height='500' scrolling='no' title='Zoekresultaten weergeven op een kaart (Service-Module)' src='//codepen.io/azuremaps/embed/zLdYEB/?height=265&theme-id=0&default-tab=js,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/zLdYEB/'>zoekresultaten weergeven op een kaart (Service-Module)</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

Het eerste vereiste blok van code vormt een kaartobject en de clientservice wordt geïnitialiseerd. U kunt zien [maken van een kaart](./map-create.md) voor instructies.

Het tweede blok van code gebruikt de [Azure Maps Fuzzy zoeken-API](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy) om te zoeken naar nuttige plaats. Fuzzy zoeken-API kan elke combinatie van fuzzy invoer verwerken. Het antwoord van de service fuzzy zoeken wordt vervolgens geparseerd in de GeoJSON-notatie gebruiken de [getGeoJsonSearchResponse](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.geojson.geojsonsearchresponse?view=azure-iot-typescript-latest#geojsonsearchresponse) methode. De pincodes worden vervolgens toegevoegd aan de kaart om de punten van belang zijn op de kaart weer te geven.

Het laatste blok van code Hiermee past u de grenzen van de camera voor de kaart met behulp van de kaart [setCameraBounds](/javascript/api/azure-maps-control/atlas.map#setcamerabounds-cameraboundsoptions-) eigenschap.

## <a name="make-a-search-request-via-xmlhttprequest"></a>Een aanvraag zoeken via XMLHttpRequest

<iframe height='500' scrolling='no' title='Zoekresultaten op een kaart weergeven' src='//codepen.io/azuremaps/embed/KQbaeM/?height=265&theme-id=0&default-tab=js,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/KQbaeM/'>zoekresultaten weergeven op een kaart</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

Het eerste vereiste blok van code wordt een kaartobject. U kunt zien [maken van een kaart](./map-create.md) voor instructies.

Het tweede codeblok wordt search resultaten laag toegevoegd aan de kaart. De laag van de resultaten zoeken, wordt de lijst met zoekresultaten zoals kledingwinkelketen op de kaart weergegeven. Pincodes worden toegevoegd met behulp van [addPins](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addpins).

Het derde blok van code verzendt een [XMLHttpRequest](https://xhr.spec.whatwg.org/) naar [Azure Maps Fuzzy zoeken-API](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy) om te zoeken naar het punt van belang zijn. Fuzzy zoeken-API kan elke combinatie van fuzzy invoer verwerken.

Het laatste blok van code parseert het antwoord en past u de Hiermee past u de grenzen van de camera voor de kaart met behulp van de kaart [setCameraBounds](/javascript/api/azure-maps-control/atlas.map#setcamerabounds-cameraboundsoptions-) om de pins resultaat weer te geven.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over **Fuzzy zoekopdrachten**:

> [!div class="nextstepaction"]
> [Azure kaarten-Fuzzy zoeken-API](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy)

Meer informatie over de klassen en methoden die in dit artikel worden gebruikt:

> [!div class="nextstepaction"]
> [Kaart](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)

Zie de volgende artikelen voor volledige codevoorbeelden:

> [!div class="nextstepaction"]
> [Gegevens ophalen uit een coördinaat](./map-get-information-from-coordinate.md)
<!-- Comment added to suppress false positive warning -->
> [!div class="nextstepaction"]
> [Routebeschrijving van A naar B](./map-route.md)
