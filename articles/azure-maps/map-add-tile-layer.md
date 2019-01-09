---
title: Een tegellaag toevoegen aan Azure-kaarten | Microsoft Docs
description: Een tegel laag toevoegen aan de Javascript-kaart
author: rbrundritt
ms.author: richbrun
ms.date: 12/3/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: a9decacc3b22676d94726e3cf979198b3486d270
ms.sourcegitcommit: 30d23a9d270e10bb87b6bfc13e789b9de300dc6b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2019
ms.locfileid: "54104638"
---
# <a name="add-a-tile-layer-to-a-map"></a>Een tegellaag toevoegen aan een kaart

Dit artikel leest u hoe u een tegellaag op de kaart kunt overlay. Tegellagen kunnen u installatiekopieën op Azure Maps basiskaart tegels plaatsen. Meer informatie over Azure-kaarten naast elkaar systeem vindt u de [zoomniveaus en tegelraster](zoom-levels-and-tile-grid.md) documentatie.

Een tegel van laag voor in de tegels van een server. Deze installatiekopieën kunnen vooraf weergegeven en die zijn opgeslagen, zoals een andere installatiekopie op een server met behulp van een naamgevingsconventie die de tegellaag begrijpt, of een dynamische service die de afbeeldingen op elk gewenst moment worden gegenereerd. Er zijn drie verschillende naamconventies service ondersteund door Azure Maps tegel [TileLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.tilelayer?view=azure-iot-typescript-latest) klasse; 

* X, Y, zoomen notatie - gebaseerd op het zoomniveau, x staat voor de kolom en y is de rijpositie van de tegel in het tegelraster.
* Quadkey notatie - combinatie x, y, Inzoomen op gegevens in een enkele tekenreeks-waarde die is een unieke id voor een tegel.
* Selectiekader - vak-coördinaten voor begrenzingsvak kan worden gebruikt om op te geven van een installatiekopie in de indeling `{west},{south},{east},{north}` die vaak wordt gebruikt door [Web toewijzing diensten (WMS)](https://www.opengeospatial.org/standards/wms).

> [!TIP]
> Een [TileLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.tilelayer?view=azure-iot-typescript-latest) is een uitstekende manier om grote gegevenssets op de kaart te visualiseren. Niet alleen kunt een tegellaag worden gegenereerd op basis van een afbeelding, maar vectorgegevens kunnen ook worden weergegeven als een tegellaag te. Door rendering vectorgegevens als een tegellaag, moet het kaartbesturingselement alleen laden van de tegels die kunnen worden in bestandsgrootte veel kleiner is dan de vectorgegevens staan. Deze techniek wordt gebruikt door veel die nodig hebt om miljoenen rijen met gegevens op de kaart weer te geven.

De URL van de tegel doorgegeven aan een tegellaag moet een http/https-URL aan een resource TileJSON of de sjabloon van een tegel-URL die gebruikmaakt van de volgende parameters: 

* `{x}` -X-positie van de tegel. Moet ook `{y}` en `{z}`.
* `{y}` -Y positie van de tegel. Moet ook `{x}` en `{z}`.
* `{z}` -Zoomniveau van de tegel. Moet ook `{x}` en `{y}`.
* `{quadkey}` -Tegel quadkey-id op basis van de naamconventie voor Bing-kaarten tegel system.
* `{bbox-epsg-3857}` -Een selectiekader vak tekenreeks zijn met de indeling `{west},{south},{east},{north}` in de EPSG 3857 ruimtelijke referentiesysteem.
* `{subdomain}` -Een tijdelijke aanduiding waar de waarden van het subdomein als opgegeven wordt toegevoegd.

## <a name="add-a-tile-layer"></a>Een titellaag toevoegen

 In dit voorbeeld laat zien hoe een tegellaag die verwijst naar een set tegels die gebruikmaken van de x, y, zoomen naast elkaar systeem maken. De bron van deze tegellaag is een radar-overlay weer van de [Iowa uitwerking Mesonet van Iowa State University](https://mesonet.agron.iastate.edu/ogc/).

<br/>

<iframe height='500' scrolling='no' title='Tegel laag met behulp van X, Y en Z' src='//codepen.io/azuremaps/embed/BGEQjG/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/BGEQjG/'>Tegellaag met behulp van X, Y en Z</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

In de bovenstaande code wordt het eerste vereiste blok van code een kaartobject. U kunt zien [maken van een kaart](./map-create.md) voor instructies.

In het tweede blok van code, een [TileLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.tilelayer?view=azure-iot-typescript-latest) wordt gemaakt door een opgemaakte URL wordt doorgegeven aan een tegel-service, de tegelgrootte van de en een dekking zodat de token semi-transparant zijn. Bovendien, als de tegellaag wordt toegevoegd aan de kaart, deze wordt toegevoegd onder de `labels` laag zodat de labels nog steeds duidelijk zichtbaar zijn zijn.

## <a name="customize-a-tile-layer"></a>Een tegellaag aanpassen

De tegel-laag heeft alleen een groot aantal opties voor stijl. Hier is een hulpprogramma voor ze kunt uitproberen.

<br/>

<iframe height='700' scrolling='no' title='Opties voor Tegellaag' src='//codepen.io/azuremaps/embed/xQeRWX/?height=700&theme-id=0&default-tab=result' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/xQeRWX/'>laag tegelopties</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de klassen en methoden die in dit artikel worden gebruikt:

> [!div class="nextstepaction"]
> [TileLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.tilelayer?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [TileLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.tilelayeroptions?view=azure-iot-typescript-latest)

Zie de volgende artikelen voor meer voorbeelden van code toevoegen aan uw kaarten:

> [!div class="nextstepaction"]
> [De afbeeldingslaag van een toevoegen](./map-add-image-layer.md)