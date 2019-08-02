---
title: Een heatmap toevoegen aan Azure Maps | Microsoft Docs
description: Een hitte kaart laag toevoegen aan de Java script-kaart
author: rbrundritt
ms.author: richbrun
ms.date: 07/29/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 85f184603cdcadce6bf750db5765f32a0735453d
ms.sourcegitcommit: 3877b77e7daae26a5b367a5097b19934eb136350
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/30/2019
ms.locfileid: "68639018"
---
# <a name="add-a-heat-map-layer"></a>Een heatmap-laag toevoegen

Heatmap, ook wel bekend als punt-dichtheids kaarten, is een type gegevens visualisatie dat wordt gebruikt voor de densiteit van gegevens met behulp van een reeks kleuren. Ze worden vaak gebruikt om de gegevens ' hot spots ' op een kaart weer te geven en zijn een uitstekende manier om grote punt gegevenssets te renderen.  U kunt bijvoorbeeld tien duizenden punten in de kaart weergave weer geven als symbolen, waarbij het grootste deel van het kaart gebied wordt bedekt, waardoor veel symbolen elkaar overlappen, waardoor het moeilijk is om veel inzicht te krijgen in de gegevens. Als u echter dezelfde gegevensset visualiseren als een heatmap, is het eenvoudig om te zien waar de punt gegevens de dichtste en de relatieve dichtheid van andere gebieden zijn. Er zijn veel scenario's waarin Heatmaps worden gebruikt. Hier volgen enkele voor beelden.

* Temperatuur gegevens worden doorgaans weer gegeven als heatmap, omdat hiermee kan worden geprofiteerd van de Tempe ratuur tussen twee gegevens punten.
* Het renderen van gegevens voor ruis Sens oren als een heatmap heeft niet alleen betrekking op de intensiteit van de ruis waarbij de sensor zich bevindt, maar kan ook inzicht krijgen in de dissipatie over een afstand. Het geluids niveau van een wille keurige site mag niet hoog zijn, maar als het gebied van de ruis dekking van meerdere Sens oren elkaar overlapt, is het mogelijk dat dit overlappende gebied hogere geluids niveaus ondervindt en dus zichtbaar is in de heatmap.
* Het visualiseren van een GPS-tracering die de snelheid bevat als toewijzing met een gewogen hoogte waarbij de intensiteit van elk gegevens punt is gebaseerd op de snelheid, is een uitstekende manier om te zien waar het Voer tuig sneller is.

> [!TIP]
> Met heatmap worden standaard de coördinaten van alle geometrieën in een gegevens bron weer gegeven. Als u de laag wilt beperken zodat deze alleen functies van punt geometrie weergeeft, stelt `filter` u de eigenschap van de `['==', ['geometry-type'], 'Point']` laag `['any', ['==', ['geometry-type'], 'Point'], ['==', ['geometry-type'], 'MultiPoint']]` in of als u ook multi point-functies wilt toevoegen.

## <a name="add-a-heat-map-layer"></a>Een heatmap-laag toevoegen

Als u een gegevens bron van punten wilt weer geven als een heatmap, geeft u uw gegevens bron door aan `HeatMapLayer` een instantie van de klasse en voegt u deze toe aan de kaart zoals hier wordt weer gegeven.

<br/>

<iframe height='500' scrolling='no' title='Eenvoudige kaart laag voor hitte' src='//codepen.io/azuremaps/embed/gQqdQB/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de pen- <a href='https://codepen.io/azuremaps/pen/gQqdQB/'>eenvoudige heatmap laag</a> door Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

In dit voor beeld heeft elk verwarmings punt een straal van 10 pixels op alle zoom niveaus. Wanneer u de laag van de heatmap toevoegt aan de kaart, wordt deze onder de label laag ingevoegd om een betere gebruikers ervaring te maken, omdat de labels duidelijk zichtbaar zijn boven de heatmap. De gegevens in dit voor beeld zijn afkomstig van het [USGS aardings risico programma](https://earthquake.usgs.gov/) en duiden op aanzienlijke aard bevingen die in de afgelopen 30 dagen zijn opgetreden.

## <a name="customizing-the-heat-map-layer"></a>De laag van de heatmap aanpassen

In het vorige voor beeld is de heatmap aangepast door de opties voor RADIUS en dekking in te stellen. De laag van de heatmap biedt verschillende opties voor aanpassing.

* `radius`: Hiermee wordt een pixel RADIUS gedefinieerd waarin elk gegevens punt wordt weer gegeven. De RADIUS kan worden ingesteld als een vast getal of als een expressie. Met behulp van een expressie kunt u de RADIUS schalen op basis van het zoom niveau, dat lijkt op een consistent ruimtelijk gebied op de kaart (bijvoorbeeld een RADIUS van 5 mijl).
* `color`: Hiermee wordt aangegeven hoe de heatmap wordt gekleurd. Een kleur overgang wordt vaak gebruikt voor hitte kaarten en kan worden bereikt met een `interpolate` expressie. Het gebruik `step` van een expressie voor het inkleuren van de heatmap onthoudt de densiteit visueel in bereiken die meer lijkt op een contour-of radar stijl kaart. Met deze kleuren paletten worden de kleuren van het minimum naar de maximale dichtheids waarde gedefinieerd. Kleur waarden voor heatmap zijn opgegeven als een expressie voor de `heatmap-density` waarde. De kleur bij index 0 in een interpolatie-expressie of de standaard kleur van een stap expressie definieert de kleur van het gebied waar er geen gegevens zijn en kunnen worden gebruikt voor het definiëren van een achtergrond kleur. Veel gewenst om deze waarde in te stellen op transparant of semi-transparant zwart. Hier volgen enkele voor beelden van kleur expressies.

| Expressie voor interpolatie kleur | Expressie met stap kleur | 
|--------------------------------|--------------------------|
| \[<br/>&nbsp;&nbsp;&nbsp;&nbsp;' interpolatie ',<br/>&nbsp;&nbsp;&nbsp;&nbsp;\[lineair\],<br/>&nbsp;&nbsp;&nbsp;&nbsp;\[' heatmap-densiteit '\],<br/>&nbsp;&nbsp;&nbsp;&nbsp;0, ' transparant ',<br/>&nbsp;&nbsp;&nbsp;&nbsp;0,01, paars,<br/>&nbsp;&nbsp;&nbsp;&nbsp;0,5, ' #fb00fb ',<br/>&nbsp;&nbsp;&nbsp;&nbsp;1, '#00c3ff'<br/>\] | \[<br/>&nbsp;&nbsp;&nbsp;&nbsp;' stap ',<br/>&nbsp;&nbsp;&nbsp;&nbsp;\[' heatmap-densiteit '\],<br/>&nbsp;&nbsp;&nbsp;&nbsp;transparent,<br/>&nbsp;&nbsp;&nbsp;&nbsp;0,01, ' Navy ',<br/>&nbsp;&nbsp;&nbsp;&nbsp;0,25, ' groen ',<br/>&nbsp;&nbsp;&nbsp;&nbsp;0,50, geel,<br/>&nbsp;&nbsp;&nbsp;&nbsp;0,75, ' rood '<br/>\] | 

* `opacity`: Hiermee geeft u op hoe ondoorzichtig of transparant de laag van de heatmap.
* `intensity`: Past een vermenigvuldigings factor toe op het gewicht van elk gegevens punt om de algehele intensiteit van de heatmap te verhogen en helpt de kleine verschillen in het gewicht van gegevens punten eenvoudiger te maken.
* `weight`: Standaard hebben alle gegevens punten een gewicht van 1, waardoor alle gegevens punten gelijk worden gewogen. De optie gewicht fungeert als vermenigvuldiger en kan worden ingesteld als een getal of een expressie. Als een getal wordt ingesteld als het gewicht, zegt 2, zou het gelijk zijn aan het twee maal plaatsen van elk gegevens punt op de kaart, waardoor de dichtheid wordt verdubbeld. Als u de optie gewicht instelt op een getal, wordt de heatmap op een vergelijk bare manier weer gegeven als met de optie intensiteit. Als er echter een expressie wordt gebruikt, kan het gewicht van elk gegevens punt worden gebaseerd op de eigenschappen van elk gegevens punt. Maak aard bevings gegevens als voor beeld. elk gegevens punt vertegenwoordigt een aard beving. Een belang rijke metriek van elk bevings gegevens punt is, is een waarde. Aard bevingen gebeuren altijd, maar de meeste hebben een lage grootte en zijn nog niet even vilt. Door gebruik te maken van de magnitude waarde in een expressie om het gewicht aan elk gegevens punt toe te wijzen, kunnen grotere aarders beter worden weer gegeven in de heatmap.
* Naast de basislaag opties; min/max. zoom, zichtbaar en filter, er is ook `source` een optie als u de gegevens bron en `source-layer` optie wilt bijwerken als uw gegevens bron een vector tegel bron is.

Hier vindt u een hulp programma voor het testen van de verschillende opties voor de heatmap voor de hitte kaart.

<br/>

<iframe height='700' scrolling='no' title='Opties voor de laag van de heatmap' src='//codepen.io/azuremaps/embed/WYPaXr/?height=700&theme-id=0&default-tab=result' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de opties van de pen voor het <a href='https://codepen.io/azuremaps/pen/WYPaXr/'>heatmap</a> van<a href='https://codepen.io/azuremaps'>@azuremaps</a>peninstellingen door Azure Maps () op <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="consistent-zoomable-heat-map"></a>Consistente, zoom bare heatmap

De RADIUS van gegevens punten die worden weer gegeven in de laag van de heatmap hebben standaard een straal van vaste pixels voor alle zoom niveaus. Als de kaart is ingezoomd, worden de gegevens samen geaggregeerd en de heatmap van de kaart laag. Een `zoom` expressie kan worden gebruikt om de RADIUS voor elk zoom niveau zodanig te schalen dat elk gegevens punt overeenkomt met hetzelfde fysieke gebied van de kaart. Hierdoor is de heatmap van de hitte statisch en consistent. Elk zoom niveau van de kaart heeft twee keer zoveel pixels verticaal en horizon taal als het vorige zoom niveau. Door de RADIUS zodanig te schalen dat deze met elk zoom niveau verdubbelt, wordt een heatmap gemaakt die consistent is op alle zoom niveaus. Dit kunt u doen met behulp `zoom` van de expressie met `exponential interpolation` de basis 2, zoals wordt weer gegeven in het voor beeld hieronder. Zoom in op de kaart om te zien hoe de heatmap met het zoom niveau wordt geschaald.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Consistente, zoom bare heatmap" src="//codepen.io/azuremaps/embed/OGyMZr/?height=500&theme-id=light&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
Zie de pen <a href='https://codepen.io/azuremaps/pen/OGyMZr/'>consistente, zoom bare heatmap</a> met Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

> [!TIP]
> Door Clustering op de gegevens bron in te scha kelen, worden punten die zich dicht bij elkaar bevinden, gegroepeerd als een geclusterd punt. De punt telling van elk cluster kan worden gebruikt als de gewichts expressie voor de heatmap en het aantal punten dat moet worden weer gegeven, aanzienlijk verminderen. De punt telling van een cluster wordt opgeslagen in een `point_count` eigenschap van de punt functie, zoals hieronder wordt weer gegeven. 
> ```JavaScript
> var layer = new atlas.layer.HeatMapLayer(datasource, null, {
>    weight: ['get', 'point_count']
> });
> ```
> Als de cluster RADIUS slechts een paar pixels is, is er weinig visueel verschil met de rendering. Een grotere straal groepeert meer punten in elk cluster en verbetert de prestaties van de heatmap, maar het is duidelijker dat de verschillen worden weer gegeven.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de klassen en methoden die in dit artikel worden gebruikt:

> [!div class="nextstepaction"]
> [HeatMapLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.htmlmarker?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [HeatMapLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.heatmaplayeroptions?view=azure-iot-typescript-latest)

Raadpleeg de volgende artikelen voor meer code voorbeelden om toe te voegen aan uw Maps:

> [!div class="nextstepaction"]
> [Een symbool laag toevoegen](./map-add-pin.md)

> [!div class="nextstepaction"]
> [Op gegevens gebaseerde stijl expressies gebruiken](data-driven-style-expressions-web-sdk.md)