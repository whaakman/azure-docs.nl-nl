---
title: Gegevens van Azure Maps clustering | Microsoft Docs
description: Hoe u gegevens van cluster in de Web-SDK
author: rbrundritt
ms.author: richbrun
ms.date: 03/27/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: cpendleton
ms.custom: codepen
ms.openlocfilehash: d4dc6f0c8fd2dff74a1997c9dca5a31abc70c03a
ms.sourcegitcommit: c63fe69fd624752d04661f56d52ad9d8693e9d56
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/28/2019
ms.locfileid: "58580821"
---
# <a name="clustering-point-data"></a>Gegevens van clusters

Bij het visualiseren van veel gegevenspunten op de kaart, gegevenspunten elkaar overlappen, de toewijzing eruitziet rommelige en wordt het moeilijk is om te zien en gebruiken. Clusters van gegevens kan worden gebruikt om deze gebruikerservaring te verbeteren. Gegevens van clustering is het proces van het combineren van gegevens die zich dicht bij elkaar en op de kaart als één geclusterde gegevenspunt vertegenwoordigen. Als de gebruiker in-/ op de kaart uitzoomen, splitst de clusters in hun afzonderlijke gegevenspunten.

## <a name="enabling-clustering-on-a-data-source"></a>Inschakelen van clusters op een gegevensbron

Clustering kan eenvoudig worden ingeschakeld op de `DataSource` klasse door in te stellen de `cluster` optie op ' True '. Bovendien de straal pixels om te selecteren in de buurt punten om te combineren in een cluster kan worden ingesteld met behulp van de `clusterRadius` en een zoomniveau kan worden opgegeven waarmee om uit te schakelen de clustering logica met behulp van de `clusterMaxZoom` optie. Hier volgt een voorbeeld van het inschakelen van clusters in een gegevensbron.

```javascript
//Create a data source and enable clustering.
var datasource = new atlas.source.DataSource(null, {
    //Tell the data source to cluster point data.
    cluster: true,

    //The radius in pixels to cluster points together.
    clusterRadius: 45,

    //The maximium zoom level in which clustering occurs.
    //If you zoom in more than this, all points are rendered as symbols.
    clusterMaxZoom: 15 
});
```

> [!TIP]
> Als twee gegevenspunten dicht bij elkaar op de grond, is het mogelijk dat het cluster wordt nooit splitsen, ongeacht hoe dicht zoomt van een gebruiker in. Om dit op te lossen, kunt u instellen de `clusterMaxZoom` optie van de gegevensbron die Hiermee geeft u op het zoomniveau uitschakelen van de logica voor clustering en alles gewoon worden weergegeven.

De `DataSource` klasse heeft ook de volgende methoden met betrekking tot het cluster:

| Methode | Retourtype | Description |
|--------|-------------|-------------|
| getClusterChildren(clusterId: number) | Promise&lt;functie&lt;geometrie, eventuele&gt; \| vorm&gt; | Hiermee haalt u de onderliggende objecten van het opgegeven cluster op de volgende zoomniveau. Deze onderliggende items kunnen bestaan uit een combinatie van vormen en subclusters. De subclusters worden functies met eigenschappen die overeenkomen met ClusteredProperties. |
| getClusterExpansionZoom(clusterId: number) | Promise&lt;getal&gt; | Berekent een zoomniveau waarmee het cluster wordt gestart uitbreiden of splitsen. |
| getClusterLeaves (clusterId: aantal, limiet: getal, offset: getal) | Promise&lt;functie&lt;geometrie, eventuele&gt; \| vorm&gt; | Hiermee haalt u alle punten in een cluster. Stelt de `limit` te retourneren van een subset van de punten en gebruik de `offset` naar pagina door de punten. |

## <a name="display-clusters-using-a-bubble-layer"></a>Clusters met behulp van een laag bel weergeven

Een bel-laag is een uitstekende manier om de geclusterde punten weergegeven zoals u kunt eenvoudig schalen van de radius en de kleur die ze op basis van het aantal punten in het cluster met behulp van een expressie wijzigen. Clusters met behulp van de laag van een bel om weer te geven, moet u ook een afzonderlijke laag gebruiken voor het renderen van geclusterde gegevenspunten. Het is vaak handig om te ook in staat om weer te geven van de grootte van het cluster boven op de bellen zijn. Een symbool laag met tekst en er geen pictogram kan worden gebruikt om dit gedrag bewerkstelligen. 

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Bel Basic-laag clustering" src="//codepen.io/azuremaps/embed/qvzRZY/?height=500&theme-id=0&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
Zie de Pen <a href='https://codepen.io/azuremaps/pen/qvzRZY/'>Basic Bel laag clustering</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="display-clusters-using-a-symbol-layer"></a>Clusters met behulp van een laag symbool weergeven

Bij het visualiseren van de gegevens met behulp van de laag symbool standaard die worden deze automatisch verborgen symbolen die elkaar overlappen elkaar samenwerken om een schoner aan die conferenties, maar dit niet mogelijk de gewenste ervaring als u wilt zien van de dichtheid van gegevens op de kaart verwijst. Instellen van de `allowOverlap` optie van de lagen symbool `iconOptions` eigenschap `true` deze ervaring wordt uitgeschakeld, maar zal leiden tot alle symbolen die worden weergegeven. Met behulp van clustering, kunt u de dichtheid van alle gegevens zien tijdens het maken van een mooie schone gebruikerservaring. In dit voorbeeld wordt de aangepaste symbolen voor clusters en afzonderlijke gegevenspunten worden gebruikt.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Geclusterde symbool laag" src="//codepen.io/azuremaps/embed/Wmqpzz/?height=500&theme-id=0&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
Zie de Pen <a href='https://codepen.io/azuremaps/pen/Wmqpzz/'>geclusterde symbool laag</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="clustering-and-the-heat-maps-layer"></a>Clustering en de heatmap toegewezen laag

Warmte-kaarten zijn een uitstekende manier om de dichtheid van gegevens op de kaart weer te geven. Deze visualisatie kan een groot aantal gegevenspunten in een eigen verwerken, maar er kan nog meer gegevens worden verwerkt als de gegevenspunten zijn geclusterd en de grootte van het cluster wordt gebruikt als het gewicht van de heatmap. Stel de `weight` optie van de heatmap kaartLaag aan `['get', 'point_count']` om dit te bereiken. Als de radius van de cluster klein is, wordt de heatmap ziet eruit bijna identiek aan die een heatmap met behulp van de geclusterde gegevenspunten maar veel betere prestaties. Echter de kleinere radius van de cluster, hoe nauwkeuriger de heatmap worden maar met minder van een prestaties profiteren.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Cluster gewogen Heat Map" src="//codepen.io/azuremaps/embed/VRJrgO/?height=500&theme-id=0&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
Zie de Pen <a href='https://codepen.io/azuremaps/pen/VRJrgO/'>Cluster gewogen Heat Map</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="mouse-events-on-clustered-data-points"></a>Muisgebeurtenissen op geclusterde gegevenspunten

Wanneer muisgebeurtenissen worden uitgevoerd in een laag die geclusterde gegevenspunten bevatten, worden de geclusterde gegevenspunt geretourneerd naar de gebeurtenis als een object GeoJSON point-functie. Deze functie punt wordt de volgende eigenschappen hebben:

| Naam van eigenschap | Type | Description |
|---------------|------|-------------|
| cluster | booleaans | Hiermee wordt aangegeven als functie staat voor een cluster. |
| cluster_id | string | Een unieke ID voor het cluster dat kan worden gebruikt met de gegevensbron `getClusterExpansionZoom`, `getClusterChildren`, en `getClusterLeaves` methoden. |
| point_count | getal | Het aantal punten die het cluster bevat. |
| point_count_abbreviated | string | Een tekenreeks is die de waarde point_count afkorting als het lang is. (bijvoorbeeld 4.000 wordt voor 4K) |

In dit voorbeeld wordt een bel laag die cluster punten wordt weergegeven en voegt een gebeurtenis die wanneer ze worden geactiveerd, berekenen en de kaart Inzoomen op het volgende zoomniveau waarmee het cluster worden verbroken elkaar met behulp van de `getClusterExpansionZoom` -methode van de `DataSource` klasse en de `cluster_id` eigenschap van het aantal malen aangeklikt geclusterde gegevenspunt. 

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Cluster getClusterExpansionZoom" src="//codepen.io/azuremaps/embed/moZWeV/?height=500&theme-id=0&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
Zie de Pen <a href='https://codepen.io/azuremaps/pen/moZWeV/'>Cluster getClusterExpansionZoom</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="display-cluster-area"></a>Cluster weergavegebied 

De gegevens van die staat voor een cluster is verspreid over een gebied. In dit voorbeeld wanneer de muis is muisaanwijzer erover beweegt een cluster, de afzonderlijke gegevens punten bevat (bladeren) wordt gebruikt voor het berekenen van een convexe voorzien en weergegeven op de kaart om weer te geven van het gebied. Alle punten die deel uitmaken van een cluster kunnen worden opgehaald uit de gegevensbron via de `getClusterLeaves` methode. Een convexe voorzien is een veelhoek die de wrap vormt een set punten, zoals een band elastisch en kan worden berekend met behulp van de `atlas.math.getConvexHull` methode.

<br/>

 <iframe height="500" style="width: 100%;" scrolling="no" title="Cluster gebied convexe voorzien" src="//codepen.io/azuremaps/embed/QoXqWJ/?height=500&theme-id=0&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
Zie de Pen <a href='https://codepen.io/azuremaps/pen/QoXqWJ/'>Cluster gebied convexe voorzien</a> Azure kaarten (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de klassen en methoden die in dit artikel worden gebruikt:

> [!div class="nextstepaction"]
> [DataSource-klasse](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [DataSourceOptions object](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.datasourceoptions?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [atlas.Math naamruimte](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.math?view=azure-iot-typescript-latest)

Zie de codevoorbeelden functionaliteit toevoegen aan uw app:

> [!div class="nextstepaction"]
> [Een bellendiagram laag toevoegen](map-add-bubble-layer.md)

> [!div class="nextstepaction"]
> [Een symbool laag toevoegen](map-add-pin.md)

> [!div class="nextstepaction"]
> [Een kaartLaag heatmap toevoegen](map-add-heat-map-layer.md)
