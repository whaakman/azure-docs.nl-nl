---
title: Een kaartLaag heatmap toevoegen aan Azure-kaarten | Microsoft Docs
description: Een kaartLaag heatmap toevoegen aan de Javascript-kaart
author: rbrundritt
ms.author: richbrun
ms.date: 12/2/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 957ce60b8519ccb1e3287232f7a5459a56b25bb7
ms.sourcegitcommit: d1c5b4d9a5ccfa2c9a9f4ae5f078ef8c1c04a3b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/08/2019
ms.locfileid: "55960612"
---
# <a name="add-a-heat-map-layer"></a>Een heatmap-laag toevoegen

Warmte kaarten, ook wel bekend als dichtheid maps verwijzen, zijn een vorm van gegevensvisualisatie vertegenwoordigt de dichtheid van gegevens met behulp van een serie kleuren. Ze worden vaak gebruikt om de gegevens van "hotspots" op een kaart weer te geven en zijn een uitstekende manier om het renderen van grote gegevenssets verwijzen.  Bijvoorbeeld, rendering tienduizenden punten in de overzichtsweergave als symbolen, dekt de meeste van het kaartgebied en zou leiden tot veel symbolen wordt gedekt door anderen, waardoor het moeilijk zijn te veel inzicht in de gegevens krijgen. Echter, visualiseren van deze dezelfde gegevensset als een heatmap kunt u eenvoudig om te zien waar de gegevens van de densest en de relatieve dichtheid naar andere gebieden. Er zijn veel scenario's in welke heatmap maps, worden gebruikt. Hier volgen enkele voorbeelden.

* Temperatuurgegevens doorgaans weergegeven als heatmap omdat deze benaderingen voor wat de temperatuur tussen de twee gegevenspunten biedt.
* Rendering van gegevens voor ruis sensoren als een heatmap niet alleen de intensiteit van de ruis staan waar de sensor is, maar ook inzicht kan geven in de dissipatie over een afstand. Het niveau van de ruis op één locatie kan niet worden hoog, maar als de dekkingsgebied ruis van meerdere sensoren overlapt, het is mogelijk dat dit overlappende gebied hogere niveaus van de ruis kan optreden, en daarom zou wel zichtbaar in de heatmap.
* Een GPS visualiseren is tracering met de snelheid als een gewogen hoogte kaart waar de intensiteit van elk gegevenspunt is gebaseerd op de snelheid een uitstekende manier om snel zien waar het voertuig is versnellen.

> [!TIP]
> Heatmap kaartlagen standaard worden de coördinaten van alle geometrie in een gegevensbron weergegeven. Om te beperken van de laag zodat deze alleen renders geometrie-functies, stel de `filter` eigenschap van de laag `['==', '$type', 'Point']`

## <a name="add-a-heat-map-layer"></a>Een heatmap-laag toevoegen

Voor het renderen van een gegevensbron van de punten als een heatmap, geeft u de gegevensbron in een exemplaar van de `HeatMapLayer` klasse en toe te voegen aan de kaart zoals hier wordt weergegeven.

<br/>

<iframe height='500' scrolling='no' title='Eenvoudige Heatmap kaartLaag' src='//codepen.io/azuremaps/embed/gQqdQB/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/gQqdQB/'>eenvoudige Heatmap kaartLaag</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

In dit voorbeeld heeft elk punt heatmap een straal van 10 pixels helemaal zoomniveaus. Als u de kaartLaag heatmap toevoegt aan de kaart, in dit voorbeeld wordt ingevoegd onder de label-laag. Hiermee maakt u een betere gebruikerservaring als de labels duidelijk zichtbare hierboven de heatmap zijn. De gegevens in dit voorbeeld is afkomstig uit de [USGS aardbeving gevaren programma](https://earthquake.usgs.gov/) en bestaat uit punten met aanzienlijke aardbevingen die hebben plaatsgevonden in de afgelopen 30 dagen.

## <a name="customizing-the-heat-map-layer"></a>Aanpassen van de heatmap kaartLaag

De heatmap in het vorige voorbeeld aangepast door het instellen van de RADIUS- en dekking opties. De heatmap kaartLaag biedt verschillende opties voor aanpassing;

* `radius`: Hiermee definieert u een pixelradius waarin om elk gegevenspunt weer te geven. De radius kan worden ingesteld als een vast aantal of als een expressie. Met behulp van een expressie, is het mogelijk voor het schalen van de radius op basis van de zoomfactor die wordt weergegeven voor een consistente ruimtelijke gebied op de kaart (bijvoorbeeld 5-mile radius).
* `color`: Hiermee geeft u op hoe de heatmap wordt gekleurde. Een kleurovergang kleurenpalet wordt vaak gebruikt voor heatmap kaarten, maar getrapte kleur palet ook handig zijn als u wilt maken van de heatmap meer eruit contour gegevens. Deze kleur palet definiëren de kleuren van het minimum aan de waarde voor maximale dichtheid. Kleurwaarden voor heatmap kaarten zijn opgegeven als een expressie op de `heatmap-density` waarde. De kleur bij index 0 in de expressie voor een kleurovergang of de standaardkleur van een kleur stap definieert u de kleur van het gebied waar er geen gegevens zijn en kan worden gebruikt voor het definiëren van een achtergrondkleur. Veel liever deze waarde transparant of een semi-transparant zwart in te stellen. Hier volgen enkele voorbeelden van expressies zijn kleur.

| Gradient Color Expression | Expressie interval | 
|---------------------------|--------------------------|
| \[<br/>&nbsp;&nbsp;&nbsp;&nbsp;'interpolate',<br/>&nbsp;&nbsp;&nbsp;&nbsp;\['lineaire'\],<br/>&nbsp;&nbsp;&nbsp;&nbsp;\['heatmap-dichtheid'\],<br/>&nbsp;&nbsp;&nbsp;&nbsp;0, 'transparent',<br/>&nbsp;&nbsp;&nbsp;&nbsp;0,01, 'paarse',<br/>&nbsp;&nbsp;&nbsp;&nbsp;0,5, '#fb00fb',<br/>&nbsp;&nbsp;&nbsp;&nbsp;1, '#00c3ff'<br/>\] | \[<br/>&nbsp;&nbsp;&nbsp;&nbsp;'stap'<br/>&nbsp;&nbsp;&nbsp;&nbsp;\['heatmap-dichtheid'\],<br/>&nbsp;&nbsp;&nbsp;&nbsp;'transparent',<br/>&nbsp;&nbsp;&nbsp;&nbsp;0,01, 'marineschepen',<br/>&nbsp;&nbsp;&nbsp;&nbsp;0,25, 'marineschepen',<br/>&nbsp;&nbsp;&nbsp;&nbsp;0.50, 'green',<br/>&nbsp;&nbsp;&nbsp;&nbsp;0,75, 'geel',<br/>&nbsp;&nbsp;&nbsp;&nbsp;1,00, "rood"<br/>\] |   

* `opacity`: Hiermee geeft u op hoe ondoorzichtig of doorzichtig de heatmap kaartLaag is.
* `intensity`: Geldt een vermenigvuldiger voor het gewicht van elk gegevenspunt te verhogen van de algehele intensiteit van de heatmap. Dit helpt om te maken van de kleine verschillen in het gewicht van gegevenspunten steeds gemakkelijker te visualiseren.
* `weight`: Standaard alle gegevenspunten hebben een gewicht van 1, dus alle gegevenspunten worden gewogen. De optie gewicht fungeert als een vermenigvuldiger en kan worden ingesteld als een getal of een expressie. Als een getal is ingesteld als het gewicht, bijvoorbeeld 2 is, het is het equivalent van het plaatsen van elk gegevenspunt op de kaart twee keer, dus verdubbeling van de dichtheid. De optie gewicht instelt op een getal de heatmap wordt weergegeven op een soortgelijke manier voor het gebruik van de optie intensiteit genereren. Echter, als een expressie wordt gebruikt, kan het gewicht van elk gegevenspunt kan worden gebaseerd op bepaalde metrische gegevens in de eigenschappen van elk gegevenspunt. Gegevens van aardbeving nemen een voorbeeld: elk gegevenspunt vertegenwoordigt een aardbeving. Een belangrijke metrische elk gegevenspunt aardbeving heeft, is de waarde van een omvang. Aardbevingen gebeurt altijd, maar de meeste hebben een lage magnitude en nog niet zijn ervaren. Met behulp van de waarde van de magnitude het gewicht toewijzen aan elk gegevenspunt in een expressie, kunnen de belangrijker aardbevingen kunnen beter worden weergegeven in de heatmap.
* Naast de opties van de basislaag. min./Max. inzoomen, zichtbaar en filteren, er is ook een `source` optie als u wilt bijwerken van de gegevensbron en `source-layer` optie als uw gegevensbron een bron van de tegel vector is.

Hier is een hulpprogramma voor het testen van de opties voor verschillende heatmap koppelen laag.

<br/>

<iframe height='700' scrolling='no' title='Opties voor Tegellaag warmte-kaart' src='//codepen.io/azuremaps/embed/WYPaXr/?height=700&theme-id=0&default-tab=result' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de Pen <a href='https://codepen.io/azuremaps/pen/WYPaXr/'>kaart Laagopties warmte</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

> [!TIP]
> Door het inschakelen van clusters op de gegevensbron, die zich dicht bij elkaar gegroepeerd als een geclusterde punt. Het aantal punt van elk cluster kan worden gebruikt als de expressie gewicht voor de heatmap en aanzienlijk verminderen het aantal punten die moeten worden weergegeven. Het aantal punt van een cluster worden opgeslagen in een `point_count property` van de punt-functie, zoals hieronder weergegeven. 
> ```JavaScript
> var layer = new atlas.layer.HeatMapLayer(datasource, null, {
>    weight: ['get', 'point_count']
> });
> ```
> Als de clustering radius is slechts een paar pixels er zal worden visual weinig verschil de rendering. Een grotere radius wordt meer punten in elke cluster groeperen en verbeter de prestaties van de heatmap, maar de een opvallender de verschillen zijn.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de klassen en methoden die in dit artikel worden gebruikt:

> [!div class="nextstepaction"]
> [HeatMapLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.htmlmarker?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [HeatMapLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.heatmaplayeroptions?view=azure-iot-typescript-latest)

Zie de volgende artikelen voor meer codevoorbeelden van toevoegen aan uw kaarten:

> [!div class="nextstepaction"]
> [Een symbool laag toevoegen](./map-add-pin.md)

