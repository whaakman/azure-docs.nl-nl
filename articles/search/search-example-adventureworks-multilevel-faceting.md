---
title: 'Voorbeeld: Facetten met meerdere niveaus - Azure Search'
description: Meer informatie over het bouwen van onderverdeling structuren voor meerdere niveaus taxonomie, het maken van een geneste navigatiestructuur die u kunt opnemen op de toepassingspagina's.
author: cstone
manager: cgronlun
services: search
ms.service: search
ms.topic: conceptual
ms.date: 01/25/2019
ms.author: chstone
ms.openlocfilehash: 7fa17528931be40109d81edac0f15a6a6822ec01
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55194811"
---
# <a name="example-multi-level-facets-in-azure-search"></a>Voorbeeld: Facetten met meerdere niveaus in Azure Search

Azure Search-schema's ondersteunen meerdere niveaus taxonomie categorieën niet expliciet, maar u kunt deze benaderen door inhoud vóór het indexeren en vervolgens enkele speciale handelingen toe te passen om de resultaten te bewerken. 

## <a name="start-with-the-data"></a>Beginnen met de gegevens

Het voorbeeld in dit artikel is gebaseerd op het voorbeeld van een vorige [Model van de database AdventureWorks inventaris](search-example-adventureworks-modeling.md), ter illustratie van meerdere niveaus op meerdere niveaus in Azure Search.

AdventureWorks is een eenvoudige twee niveaus taxonomie met een bovenliggende / onderliggende relatie. Voor de diepte van de vaste lengte taxonomie van deze structuur, kan een eenvoudige query van de SQL-join worden gebruikt voor het groeperen van de taxonomie:

```T-SQL
SELECT 
  parent.Name Parent, category.Name Category, category.ProductCategoryId
FROM 
  SalesLT.ProductCategory category
LEFT JOIN
  SalesLT.ProductCategory parent
  ON category.ParentProductCategoryId=parent.ProductCategoryId
```

  ![Queryresultaten](./media/search-example-adventureworks/prod-query-results.png "queryresultaten")

## <a name="indexing-to-a-collection-field"></a>Indexeren van een verzameling-veld

In de index met deze structuur, maakt u een **Collection(Edm.String)** veld in de Azure Search-schema voor het opslaan van deze gegevens, ervoor te zorgen dat veldkenmerken kan worden doorzocht, Filterbaar, geschikt voor facetten, zijn en worden opgehaald.

Nu bij het indexeren van inhoud die naar een specifieke taxonomie categorie verwijst, dienen de taxonomie als een matrix met de tekst van elk niveau van de taxonomie. Bijvoorbeeld, voor een entiteit met `ProductCategoryId = 5 (Mountain Bikes)`, het veld als verzenden `[ "Bikes", "Bikes|Mountain Bikes"]`

U ziet de opname van de bovenliggende categorie 'Fietsen' in de onderliggende categoriewaarde 'Mountainbikes'. Elke subcategorie moet het volledige pad ten opzichte van het hoofdelement insluiten. Het scheidingsteken pipe-teken is willekeurig, maar het moet consistent zijn en moet niet worden weergegeven in de brontekst. Het scheidingsteken wordt gebruikt in de toepassingscode te reconstrueren de structuur van de taxonomie van facet resultaten.

## <a name="construct-the-query"></a>Bouw de query

Bij de uitgifte van query's, zijn onder andere de volgende facet-specificatie (taxonomie is het geschikt voor facetten taxonomieveld): `facet = taxonomy,count:50,sort:value`

De waarde moet hoog genoeg is om alle mogelijke taxonomie waarden te retourneren. De gegevens van AdventureWorks bevat 41 afzonderlijke taxonomie waarden, dus `count:50` is voldoende.

  ![Basis van meervoudig filteren](./media/search-example-adventureworks/facet-filter.png "basis van meervoudig filteren")

## <a name="build-the-structure-in-client-code"></a>De structuur in clientcode bouwen

In de code van uw client-toepassing reconstrueren, de structuur van de taxonomie door het splitsen van elke facetwaarde op het pipe-teken.

```javascript
var sum = 0
  , categories = {children:{},length:0}
  , results = getSearchResults();
separator = separator || '|';
field = field || 'taxonomy';
results['@search.facets'][field].forEach(function(d) {
  var node = categories;
  var parts = d.value.split(separator);
  sum += d.count;
  parts.forEach(function(c, i) {
    if (!_(node.children).has(c)) {
      node.children[c] = {};
      node.children[c].count = d.count;
      node.children[c].children = {};
      node.children[c].length = 0;
      node.children[c].filter = parts.slice(0,i+1).join(separator);
      node.length++;
    }
    node = node.children[c];
  });
});
categories.count = sum;
```

De **categorieën** object kan nu worden gebruikt om een boomstructuur samenvouwbare taxonomie met nauwkeurige telt het aantal weer te geven:

  ![met meerdere niveaus basis van meervoudig filteren](./media/search-example-adventureworks/multi-level-facet.png "met meerdere niveaus basis van meervoudig filteren")

 
Elke koppeling in de structuur moet het gerelateerde filter toepassen. Bijvoorbeeld:

+ **taxonomie/any** `(x:x eq 'Accessories')` retourneert alle documenten in de vertakking accessoires
+ **taxonomie/any** `(x:x eq 'Accessories|Bike Racks')` retourneert alleen de documenten met een subcategorie van Bike Racks onder de tak accessoires.

Deze techniek wordt passend gemaakt zodat complexere scenario's zoals diepere taxonomie structuren en subcategorieën die in een andere bovenliggende categorieën optreden gedupliceerd (bijvoorbeeld `Bike Components|Forks` en `Camping Equipment|Forks`).

> [!TIP]
> Snelheid van de query wordt beïnvloed door het aantal facetten geretourneerd. Ondersteuning voor zeer grote taxonomie sets, overweegt u het toevoegen van een geschikt voor facetten **Edm.String** veld voor het opslaan van de waarde op het hoogste niveau taxonomie voor elk document. De bovenstaande dezelfde techniek vervolgens van toepassing, maar alleen de verzameling-facet query (gefilterd op het hoofdveld taxonomie) uitvoeren wanneer de gebruiker een knooppunt op het hoogste niveau uitgebreid. Of, als 100% intrekken niet vereist is, gewoon het aantal facet beperken tot een redelijk aantal, en zorg ervoor dat de vermeldingen facet worden gesorteerd op aantal.

## <a name="see-also"></a>Zie ook

[Voorbeeld: Model van de inventaris van de AdventureWorks-database van Azure Search](search-example-adventureworks-modeling.md)