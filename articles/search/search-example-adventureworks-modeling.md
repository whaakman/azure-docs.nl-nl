---
title: 'Voorbeeld: Model van de voorraad AdventureWorks-database - Azure Search'
description: Informatie over het model van relationele gegevens, waar ze worden getransformeerd in een platte gegevensset voor indexering en volledige-tekstzoekopdrachten in Azure Search.
author: cstone
manager: cgronlun
services: search
ms.service: search
ms.topic: conceptual
ms.date: 01/25/2019
ms.author: chstone
ms.openlocfilehash: 6d5d01dfbbcfda56818f5c38b06117a87e021445
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55174564"
---
# <a name="example-model-the-adventureworks-inventory-database-for-azure-search"></a>Voorbeeld: Model van de inventaris van de AdventureWorks-database van Azure Search

Modellering van gestructureerde database-inhoud in een efficiënte search-index is zelden een eenvoudig proces. Plannings- en wijzigingsbeheer gereserveerd, is er de uitdaging van de bronrijen van hun status lid is van een tabel in op entiteiten zoeken-vriendelijk denormalizing. In dit artikel wordt de AdventureWorks voorbeeldgegevens beschikbaar online, om te markeren algemene ervaringen in de overgang van de database te zoeken. 

## <a name="about-adventureworks"></a>About AdventureWorks

Als u een SQL Server-exemplaar hebt, is het mogelijk dat u bekend bent met de AdventureWorks voorbeelddatabase. Tussen de tabellen die zijn opgenomen in deze database zijn vijf tabellen die beschikbaar maken van informatie over het product.

+ **Productmodelbestand**: naam
+ **Product**: naam, kleur, kosten, grootte, gewicht, afbeelding, categorie (elke rij aan wordt toegevoegd aan een specifieke productmodelbestand)
+ **ProductDescription**: beschrijving
+ **ProductModelProductDescription**: landinstelling (elke rij lid wordt van een ProductModel aan een specifieke ProductDescription voor een specifieke taal)
+ **ProductCategory**: naam, de bovenliggende categorie

Al deze gegevens te combineren in een platte rijenset die kan worden opgenomen in een search-index is kan de taak. 

## <a name="considering-our-options"></a>Onze opties overwegen

De naïve-benadering zou zijn om te indexeren van alle rijen uit de tabel Product (indien van toepassing toegevoegd) omdat de tabel Product beschikt over de meest specifieke informatie. Deze aanpak zou echter de zoekindex waargenomen dubbele waarden in een resultatenset beschikbaar. Bijvoorbeeld, is het weg-650-model beschikbaar in twee kleuren en zes formaten. Een query uitvoeren voor "Wegfietsen" zou vervolgens gedomineerd door twaalf exemplaren van hetzelfde model, gedifferentieerde alleen door de grootte en kleur. De zes overige weg-specifieke modellen zou alle worden het systeem is toegewezen aan de beveiligingswereld nether van search: pagina twee.

  ![Lijst met producten](./media/search-example-adventureworks/products-list.png "lijst met producten")
 
U ziet dat het model weg-650 twaalf opties. Een-op-veel-entiteit rijen zijn beste weergegeven als velden met meerdere waarden of pre-aggregated / waarde-velden in de search-index.

Het oplossen van dit probleem is niet zo eenvoudig als het verplaatsen van de doelindex aan de tabel productmodelbestand. In dat geval zou de belangrijke gegevens in de tabel Product die nog moet worden weergegeven in zoekresultaten negeren.

## <a name="use-a-collection-data-type"></a>Het gegevenstype van een verzameling gebruiken

De 'juiste aanpak"is het zoekschema functie heeft geen een directe parallelle in het databasemodel gebruiken: **Collection(Edm.String)**. Het gegevenstype van een verzameling wordt gebruikt wanneer u een lijst van afzonderlijke tekenreeksen, in plaats van een zeer lange tekenreeks (single). Als u tags of trefwoorden hebt, gebruikt u het gegevenstype van een verzameling op voor dit veld.

Met het definiëren van meerdere waarden indexvelden van **Collection(Edm.String)** voor 'kleur', 'grootte' en 'afbeelding', de bijkomende informatie wordt behouden voor onderverdeling en filteren zonder de index met dubbele vermeldingen vervuilende. Statistische functies op dezelfde manier van toepassing op de numerieke velden van het Product indexeren **minListPrice** in plaats van elk één product **listPrice**.

Uitgaande van een index met deze structuren, weergegeven een zoekopdracht voor "mountainbikes" discrete fiets modellen, met behoud van belangrijke metagegevens, zoals kleur, grootte en laagste koers. De volgende schermafbeelding geeft een illustratie.

  ![Een voorbeeld van Mountain fiets zoekopdracht](./media/search-example-adventureworks/mountain-bikes-visual.png "Mountain fiets een voorbeeld van zoekopdracht")

## <a name="use-script-for-data-manipulation"></a>Script gebruiken voor gegevensmanipulatie

Dit type modellen niet kan helaas eenvoudig bereikt via SQL-instructies die alleen. In plaats daarvan een eenvoudige NodeJS-script gebruiken om de gegevens worden geladen en wijs deze toe in JSON-entiteiten zoeken-vriendelijk.

De toewijzing van het laatste database zoeken ziet er als volgt:

+ model (Edm.String: doorzoekbaar Filterbaar, ophaalbaar) van "ProductModel.Name"
+ description_en (Edm.String: doorzoekbare) van 'ProductDescription' voor het model waarbij de cultuur = 'en'
+ kleur (Collection(Edm.String): doorzoekbare, Filterbaar, geschikt voor facetten, ophaalbaar): de unieke waarden uit 'Product.Color' voor het model
+ grootte (Collection(Edm.String): doorzoekbare, Filterbaar, geschikt voor facetten, ophaalbaar): de unieke waarden uit 'Product.Size' voor het model
+ afbeelding (Collection(Edm.String): ophaalbaar): de unieke waarden uit 'Product.ThumbnailPhoto' voor het model
+ minStandardCost (Edm.Double: Filterbaar, geschikt voor facetten, sorteerbaar, ophaalbaar): cumulatieve minimum van alle "Product.StandardCost' voor het model
+ minListPrice (Edm.Double: Filterbaar, geschikt voor facetten, sorteerbaar, ophaalbaar): cumulatieve minimum van alle "Product.ListPrice' voor het model
+ minWeight (Edm.Double: Filterbaar, geschikt voor facetten, sorteerbaar, ophaalbaar): cumulatieve minimum van alle "Product.Weight' voor het model
+ producten (Collection(Edm.String): doorzoekbaar Filterbaar, ophaalbaar): de unieke waarden uit 'Product.Name' voor het model

Na toevoeging aan de tabel productmodelbestand met gebruik van Product- en ProductDescription, [lodash](https://lodash.com/) (of Linq in C#) om te zetten snel de resultatenset:

```javascript
var records = queryYourDatabase();
var models = _(records)
  .groupBy('ModelName')
  .values()
  .map(function(d) {
    return {
      model: _.first(d).ModelName,
      description: _.first(d).Description,
      colors: _(d).pluck('Color').uniq().compact().value(),
      products: _(d).pluck('ProductName').uniq().compact().value(),
      sizes: _(d).pluck('Size').uniq().compact().value(),
      images: _(d).pluck('ThumbnailPhotoFilename').uniq().compact().value(),
      minStandardCost: _(d).pluck('StandardCost').min(),
      maxStandardCost: _(d).pluck('StandardCost').max(),
      minListPrice: _(d).pluck('ListPrice').min(),
      maxListPrice: _(d).pluck('ListPrice').max(),
      minWeight: _(d).pluck('Weight').min(),
      maxWeight: _(d).pluck('Weight').max(),
    };
  })
  .value();
```

De resulterende JSON ziet er als volgt:

```json
[
  {
    "model": "HL Road Frame",
    "colors": [
      "Black",
      "Red"
    ],
    "products": [
      "HL Road Frame - Black, 58",
      "HL Road Frame - Red, 58",
      "HL Road Frame - Red, 62",
      "HL Road Frame - Red, 44",
      "HL Road Frame - Red, 48",
      "HL Road Frame - Red, 52",
      "HL Road Frame - Red, 56",
      "HL Road Frame - Black, 62",
      "HL Road Frame - Black, 44",
      "HL Road Frame - Black, 48",
      "HL Road Frame - Black, 52"
    ],
    "sizes": [
      "58",
      "62",
      "44",
      "48",
      "52",
      "56"
    ],
    "images": [
      "no_image_available_small.gif"
    ],
    "minStandardCost": 868.6342,
    "maxStandardCost": 1059.31,
    "minListPrice": 1431.5,
    "maxListPrice": 1431.5,
    "minWeight": 961.61,
    "maxWeight": 1043.26
  }
]
```

Ten slotte, als volgt de SQL-query naar de oorspronkelijke recordset niet retourneren. Ik gebruikt de [mssql](https://www.npmjs.com/package/mssql) npm-module laden van de gegevens in mijn NodeJS-app.

```T-SQL
SELECT
  m.Name as ModelName,
  d.Description,
  p.Name as ProductName,
  p.*
FROM 
  SalesLT.ProductModel m
INNER JOIN 
  SalesLT.ProductModelProductDescription md
  ON m.ProductModelId = md.ProductModelId
INNER JOIN 
  SalesLT.ProductDescription d
  ON md.ProductDescriptionId = d.ProductDescriptionId
LEFT JOIN 
  SalesLT.product p
  ON m.ProductModelId = p.ProductModelId
WHERE
  md.Culture='en'
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Voorbeeld: Multi-level facet taxonomie in Azure Search](search-example-adventureworks-multilevel-faceting.md)


