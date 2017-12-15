---
title: Werken met gegevens in Azure Cosmos DB georuimtelijke | Microsoft Docs
description: Begrijpen hoe maken, index en query ruimtelijke-objecten met Azure Cosmos DB en de SQL-API.
services: cosmos-db
documentationcenter: 
author: arramac
manager: jhubbard
editor: monicar
ms.assetid: 82ce2898-a9f9-4acf-af4d-8ca4ba9c7b8f
ms.service: cosmos-db
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 10/20/2017
ms.author: arramac
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 3e778f4a9b7ec4935d53eb335462f3c414ff99cd
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2017
---
# <a name="working-with-geospatial-and-geojson-location-data-in-azure-cosmos-db"></a>Werken met georuimtelijke en GeoJSON locatiegegevens in Azure Cosmos-DB
In dit artikel bevat een inleiding tot de functionaliteit georuimtelijke in [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/). Na het lezen van dit, kunt u zich de volgende vragen beantwoorden:

* Hoe kan ik ruimtelijke gegevens opslaan in Azure Cosmos DB?
* Hoe kan ik een query over gegevens in Azure Cosmos-database in SQL- en LINQ georuimtelijke?
* Hoe ik in- of uitschakelen ruimtelijke indexeren in Azure Cosmos DB?

Dit artikel laat zien hoe u werkt met ruimtelijke gegevens met de SQL-API. Zie dit [GitHub project](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/Geospatial/Program.cs) voor codevoorbeelden.

## <a name="introduction-to-spatial-data"></a>Inleiding tot ruimtelijke gegevens
Ruimtelijke gegevens beschrijft de positie en de vorm van objecten in de ruimte. In de meeste toepassingen deze komen overeen met objecten op de aarde en georuimtelijke gegevens. Ruimtelijke gegevens kunnen worden gebruikt voor de locatie van een persoon, een plaats van belang of de grens van een stad of een meer. Algemene gebruiksvoorbeelden vaak betrekken nabijheid query's, bijvoorbeeld, 'alle in internetcafés vinden in de buurt van mijn huidige locatie'. 

### <a name="geojson"></a>GeoJSON
Azure Cosmos-DB biedt ondersteuning voor indexering en query's naar georuimtelijke gegevens die is weergegeven met behulp van de [GeoJSON-specificatie](https://tools.ietf.org/html/rfc7946). GeoJSON gegevensstructuren zijn altijd geldig JSON-objecten, zodat ze kunnen worden opgeslagen en opgevraagd met Azure Cosmos DB zonder speciale hulpprogramma's of bibliotheken. De Azure Cosmos DB SDK's bieden helperklassen en methoden die gemakkelijk te werken met ruimtelijke gegevens. 

### <a name="points-linestrings-and-polygons"></a>Punten en multipoint veelhoek
Een **punt** wordt bepaald door een enkele positie in de ruimte. Een punt vertegenwoordigt georuimtelijke gegevens, de exacte locatie, wat erop kan een adres van een winkelketen, een kiosk, een auto of een plaats.  GeoJSON (en Azure Cosmos DB) met behulp van de coördinaat paar of de lengte en de breedte van een punt wordt weergegeven. Hier volgt een voorbeeld van JSON voor een punt.

**Punten in Azure Cosmos DB**

```json
{
    "type":"Point",
    "coordinates":[ 31.9, -4.8 ]
}
```

> [!NOTE]
> Het GeoJSON-specificatie bevat lengtegraad eerste en breedtegraad tweede. Net als in andere toepassingen toewijzing breedtegraad worden hoeken en vertegenwoordigd in termen van graden. Lengtegraadwaarden worden gemeten vanaf de nulmeridiaan tussen-180 graden en 180.0 graden en breedtegraadwaarden worden gemeten vanaf de evenaar liggen tussen-90.0 graden en 90.0 graden zijn. 
> 
> Azure Cosmos DB interpreteert coördinaten, zoals wordt weergegeven per het referentiesysteem WGS 84. Zie hieronder voor meer informatie over coördinaat referentiesystemen.
> 
> 

Dit kan worden ingesloten in een document Azure Cosmos DB zoals in dit voorbeeld van een gebruikersprofiel met locatiegegevens:

**Gebruik het profiel met locatie opgeslagen in Azure Cosmos-DB**

```json
{
    "id":"cosmosdb-profile",
    "screen_name":"@CosmosDB",
    "city":"Redmond",
    "topics":[ "global", "distributed" ],
    "location":{
        "type":"Point",
        "coordinates":[ 31.9, -4.8 ]
    }
}
```

Naast punten ondersteunt GeoJSON ook multipoint en veelhoek. **Multipoint** vertegenwoordigen een reeks van twee of meer punten in de lijnsegmenten waarmee ze verbinding maken en de ruimte. In georuimtelijke gegevens wordt multipoint vaak gebruikt om te wegen of rivieren vertegenwoordigen. Een **veelhoek** is een grens verbonden punten die een gesloten LineString vormt. Veelhoek wordt vaak gebruikt om natuurlijke formaties zoals meren of politieke rechtsgebieden zoals steden en statussen te geven. Hier volgt een voorbeeld van een Polygoon in Azure Cosmos DB. 

**Veelhoek in GeoJSON**

```json
{
    "type":"Polygon",
    "coordinates":[ [
        [ 31.8, -5 ],
        [ 31.8, -4.7 ],
        [ 32, -4.7 ],
        [ 32, -5 ],
        [ 31.8, -5 ]
    ] ]
}
```

> [!NOTE]
> De specificatie GeoJSON vereist dat voor geldige veelhoek het laatste coördinaat paar opgegeven moet hetzelfde zijn als de eerste pagina, voor het maken van een gesloten vorm.
> 
> Punten in een Polygoon moeten worden opgegeven in rechtsom volgorde. Een Polygoon die is opgegeven in rechtsom volgorde vertegenwoordigt de inverse van de regio binnen deze.
> 
> 

Naast Point, LineString en Veelhoek, GeoJSON ook Hiermee geeft u de weergave voor het groeperen van meerdere locaties voor georuimtelijke, evenals willekeurige eigenschappen koppelen aan geolocatie als een **functie**. Aangezien deze objecten geldige JSON zijn, kunnen ze alle worden opgeslagen en verwerkt in Azure Cosmos DB. Maar Azure Cosmos DB biedt alleen ondersteuning voor automatisch indexeren van punten.

### <a name="coordinate-reference-systems"></a>Coördinaat referentiesystemen
Omdat de vorm van de aarde onregelmatige is, worden in veel coördinaat referentiesystemen (CRS), elk met hun eigen frames van verwijzing en maateenheden coördinaten van georuimtelijke gegevens weergegeven. Bijvoorbeeld, de 'National raster van-Brittannië' is een referentiesysteem zeer nauwkeurig is voor het Verenigd Koninkrijk, maar niet buiten. 

De meest populaire CRS in gebruik is vandaag de dag de wereld geodetisch systeem [WGS 84](http://earth-info.nga.mil/GandG/wgs84/). Gebruik WGS 84 GPS-apparaten en veel toewijzing services, met inbegrip van Google-kaarten en Bing kaarten-API's. Azure Cosmos DB ondersteunt indexeren en query's naar georuimtelijke gegevens met behulp van de WGS 84 CRS alleen. 

## <a name="creating-documents-with-spatial-data"></a>Maken van documenten met ruimtelijke gegevens
Bij het maken van documenten die GeoJSON-waarden bevatten, worden ze automatisch geïndexeerd met een ruimtelijke index overeenkomstig het indexeringsbeleid van de verzameling. Als u met een Azure-SDK Cosmos-database in een dynamisch getypeerde taal zoals Python of Node.js werkt, moet u geldige GeoJSON maken.

**Document maken met georuimtelijke gegevens in Node.js**

```json
var userProfileDocument = {
    "name":"cosmosdb",
    "location":{
        "type":"Point",
        "coordinates":[ -122.12, 47.66 ]
    }
};

client.createDocument(`dbs/${databaseName}/colls/${collectionName}`, userProfileDocument, (err, created) => {
    // additional code within the callback
});
```

Als u met de SQL-API's werkt, kunt u de `Point` en `Polygon` klassen binnen de `Microsoft.Azure.Documents.Spatial` naamruimte voor het insluiten van locatie-informatie in uw toepassingsobjecten. Deze klassen vereenvoudigen de serialisatie en deserialisatie van ruimtelijke gegevens in GeoJSON.

**Document met georuimtelijke gegevens in .NET maken**

```json
using Microsoft.Azure.Documents.Spatial;

public class UserProfile
{
    [JsonProperty("name")]
    public string Name { get; set; }

    [JsonProperty("location")]
    public Point Location { get; set; }

    // More properties
}

await client.CreateDocumentAsync(
    UriFactory.CreateDocumentCollectionUri("db", "profiles"), 
    new UserProfile 
    { 
        Name = "cosmosdb", 
        Location = new Point (-122.12, 47.66) 
    });
```

Als u niet beschikt over de breedtegraad en lengtegraad informatie, maar u beschikt over de fysieke adressen of de locatienaam zoals plaats of land, kunt u de werkelijke coördinaten opzoeken met behulp van een geocodering service, zoals Bing Maps REST-Services. Meer informatie over geocodering van Bing Maps [hier](https://msdn.microsoft.com/library/ff701713.aspx).

## <a name="querying-spatial-types"></a>Spatiale typen opvragen
Nu dat we kijken hoe georuimtelijke gegevens invoegen, eens kijken hoe deze gegevens met behulp van Azure Cosmos DB met SQL en LINQ query hebt genomen.

### <a name="spatial-sql-built-in-functions"></a>Ruimtelijke ingebouwde SQL-functies
Azure Cosmos DB ondersteunt de volgende ingebouwde functies voor Open georuimtelijke Consortium (OGC) georuimtelijke query's. Zie voor meer informatie over de volledige reeks ingebouwde functies in de SQL-taal, [Query Azure Cosmos DB](sql-api-sql-query.md).

<table>
<tr>
  <td><strong>Gebruik</strong></td>
  <td><strong>Beschrijving</strong></td>
</tr>
<tr>
  <td>ST_DISTANCE (spatial_expr, spatial_expr)</td>
  <td>Retourneert de afstand tussen de twee GeoJSON punt, Polygon of LineString expressies.</td>
</tr>
<tr>
  <td>ST_WITHIN (spatial_expr, spatial_expr)</td>
  <td>Retourneert een Booleaanse expressie die aangeeft of het eerste GeoJSON-object (punt, Polygon of LineString) binnen het tweede GeoJSON-object (punt, Polygon of LineString).</td>
</tr>
<tr>
  <td>ST_INTERSECTS (spatial_expr, spatial_expr)</td>
  <td>Retourneert een Booleaanse expressie waarmee wordt aangegeven of de twee opgegeven GeoJSON objecten (punt, Polygon of LineString) intersect.</td>
</tr>
<tr>
  <td>ST_ISVALID</td>
  <td>Retourneert een Booleaanse waarde die aangeeft of de opgegeven expressie voor GeoJSON punt, Polygon of LineString ongeldig is.</td>
</tr>
<tr>
  <td>ST_ISVALIDDETAILED</td>
  <td>Retourneert een JSON-waarde met een Booleaanse waarde als de opgegeven expressie voor GeoJSON punt, Polygon of LineString is geldig, en als ongeldig, ook de reden als een string-waarde.</td>
</tr>
</table>

Ruimtelijke functies kunnen worden gebruikt om uit te voeren nabijheid query's op ruimtelijke gegevens. Hier is bijvoorbeeld een query die alle familie documenten die binnen 30 km van de opgegeven locatie met de ingebouwde functie ST_DISTANCE retourneert. 

**Query**

    SELECT f.id 
    FROM Families f 
    WHERE ST_DISTANCE(f.location, {'type': 'Point', 'coordinates':[31.9, -4.8]}) < 30000

**Resultaten**

    [{
      "id": "WakefieldFamily"
    }]

Als u ruimtelijke indexeren in uw indexeringsbeleid opneemt, kunnen klikt u vervolgens 'afstand query's ' worden geleverd efficiënt via de index. Zie de sectie hieronder voor meer informatie over het ruimtelijke indexeren. Als u een ruimtelijke index voor de opgegeven paden niet hebt, kunt u nog steeds ruimtelijke query's uitvoeren door te geven `x-ms-documentdb-query-enable-scan` aanvraag-header met de waarde ingesteld op 'true'. In .NET, kunt dit doen door de optionele **FeedOptions** argument voor query's met [EnableScanInQuery](https://msdn.microsoft.com/library/microsoft.azure.documents.client.feedoptions.enablescaninquery.aspx#P:Microsoft.Azure.Documents.Client.FeedOptions.EnableScanInQuery) is ingesteld op true. 

ST_WITHIN kan worden gebruikt om te controleren of een punt binnen een Polygoon liggen. Veelhoek worden vaak gebruikt om grenzen zoals zip-codes, status grenzen of natuurlijke formaties vertegenwoordigen. Opnieuw als u ruimtelijke indexeren in uw indexeringsbeleid opneemt, klikt u vervolgens 'in' query's kunnen worden geleverd efficiënt via de index. 

Argumenten van de veelhoek in ST_WITHIN mag slechts één keer, dat wil zeggen, de veelhoek mag geen gaten bevat. 

**Query**

    SELECT * 
    FROM Families f 
    WHERE ST_WITHIN(f.location, {
        'type':'Polygon', 
        'coordinates': [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]
    })

**Resultaten**

    [{
      "id": "WakefieldFamily",
    }]

> [!NOTE]
> Vergelijkbaar met hoe niet-overeenkomende typen werk in Azure Cosmos DB-query, als de locatiewaarde die is opgegeven in het ofwel argument onjuist gevormd of ongeldig is, wordt dit resulteert in **niet-gedefinieerde** en het geëvalueerde document van de queryresultaten worden overgeslagen. Als de query geen resultaten oplevert, voert u ST_ISVALIDDETAILED voor foutopsporing waarom het ruimtelijke type ongeldig is.     
> 
> 

Azure Cosmos DB biedt ook ondersteuning voor inverse query's uitvoeren, dat wil zeggen, kunt u Multilinestring of regels in Azure DB die Cosmos-index en vervolgens een query voor de gebieden die een opgegeven punt bevatten. Dit patroon wordt vaak gebruikt in logistiek om bijvoorbeeld te identificeren wanneer een vrachtwagen binnengaat of een aangewezen gebied verlaat. 

**Query**

    SELECT * 
    FROM Areas a 
    WHERE ST_WITHIN({'type': 'Point', 'coordinates':[31.9, -4.8]}, a.location)


**Resultaten**

    [{
      "id": "MyDesignatedLocation",
      "location": {
        "type":"Polygon", 
        "coordinates": [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]
      }
    }]

ST_ISVALID en ST_ISVALIDDETAILED kunnen worden gebruikt om te controleren of een ruimtelijke object geldig is. Bijvoorbeeld controleert de volgende query de geldigheid van een punt met een out-of breedtegraad bereikwaarde (-132.8). ST_ISVALID alleen een Booleaanse waarde retourneert en ST_ISVALIDDETAILED retourneert de Booleaanse waarde en een tekenreeks met de reden waarom het wordt als ongeldig beschouwd.

** Query **

    SELECT ST_ISVALID({ "type": "Point", "coordinates": [31.9, -132.8] })

**Resultaten**

    [{
      "$1": false
    }]

Deze functies kunnen ook worden gebruikt voor het valideren van de veelhoek. Bijvoorbeeld, hier gebruiken we ST_ISVALIDDETAILED valideren van een Polygoon die niet wordt afgesloten. 

**Query**

    SELECT ST_ISVALIDDETAILED({ "type": "Polygon", "coordinates": [[ 
        [ 31.8, -5 ], [ 31.8, -4.7 ], [ 32, -4.7 ], [ 32, -5 ] 
        ]]})

**Resultaten**

    [{
       "$1": { 
            "valid": false, 
            "reason": "The Polygon input is not valid because the start and end points of the ring number 1 are not the same. Each ring of a Polygon must have the same start and end points." 
          }
    }]

### <a name="linq-querying-in-the-net-sdk"></a>LINQ-query in de .NET SDK
De SQL .NET SDK ook providers stub-methoden `Distance()` en `Within()` voor gebruik in een LINQ-expressies. De SQL-LINQ-provider vertaalt deze methodeaanroepen naar de equivalente SQL ingebouwde functieaanroepen (ST_DISTANCE en ST_WITHIN respectievelijk). 

Hier volgt een voorbeeld van een LINQ-query waarmee wordt gezocht naar alle documenten in de Azure DB die Cosmos-verzameling waarvan de waarde 'locatie' wordt binnen een straal van 30 km van de opgegeven verwijzen met behulp van LINQ.

**LINQ-query voor afstand**

    foreach (UserProfile user in client.CreateDocumentQuery<UserProfile>(UriFactory.CreateDocumentCollectionUri("db", "profiles"))
        .Where(u => u.ProfileType == "Public" && a.Location.Distance(new Point(32.33, -4.66)) < 30000))
    {
        Console.WriteLine("\t" + user);
    }

Hier wordt ook een query voor het zoeken naar alle documenten waarvan 'locatie' binnen de opgegeven vak/veelhoek is. 

**LINQ-query voor binnen**

    Polygon rectangularArea = new Polygon(
        new[]
        {
            new LinearRing(new [] {
                new Position(31.8, -5),
                new Position(32, -5),
                new Position(32, -4.7),
                new Position(31.8, -4.7),
                new Position(31.8, -5)
            })
        });

    foreach (UserProfile user in client.CreateDocumentQuery<UserProfile>(UriFactory.CreateDocumentCollectionUri("db", "profiles"))
        .Where(a => a.Location.Within(rectangularArea)))
    {
        Console.WriteLine("\t" + user);
    }


Nu dat we kijken hoe documenten met behulp van de LINQ- en SQL query hebt gemaakt, gaat u nu een overzicht van het configureren van Azure DB die Cosmos voor ruimtelijke indexeren.

## <a name="indexing"></a>Indexeren
Zoals wordt beschreven in de [Schema Networkdirect indexering met Azure Cosmos DB](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf) artikel, dat we Azure Cosmos DB database-engine worden ontworpen echt schema networkdirect en eerste-klas ondersteuning bieden voor JSON. De schrijven die zijn geoptimaliseerd voor database-engine van Azure DB die Cosmos begrijpt systeemeigen ruimtelijke gegevens (punten, Multilinestring en regels) weergegeven in de standaard GeoJSON.

Kortom, de geometrie wordt geprojecteerd van geodetisch coördinaten op een 2D vlak vervolgens geleidelijk onderverdeeld in de cellen met behulp van een **quadtree**. Deze cellen zijn toegewezen aan 1D op basis van de locatie van de cel binnen een **Hilbert ruimte vullen curve**, die de plaats van punten behouden blijft. Bovendien wanneer locatiegegevens wordt geïndexeerd, wordt er een proces genaamd **mozaïekpatroon**, dat wil zeggen, de cellen die elkaar een locatie overlappen zijn geïdentificeerd en opgeslagen als sleutels in de Azure DB die Cosmos-index. Op het moment dat de query, argumenten zoals points en Veelhoek zijn ook representatie mozaïekpatroon om op te halen van de betreffende cel ID bereiken, en vervolgens gebruikt voor het ophalen van gegevens vanaf de index.

Als u een indexeringsbeleid met ruimtelijke index voor / * (alle paden) en vervolgens alle punten gevonden binnen de verzameling worden geïndexeerd voor efficiënte ruimtelijke query's (ST_WITHIN en ST_DISTANCE). Ruimtelijke indexen niet hebben een precisiewaarde, en gebruik altijd een standaardwaarde precisie.

> [!NOTE]
> Azure Cosmos DB biedt ondersteuning voor automatisch indexeren van punten, Multilinestring en multipoint
> 
> 

Het volgende JSON-fragment toont een indexeringsbeleid met ruimtelijke indexeren is ingeschakeld, dat wil, index-nergens GeoJSON gevonden binnen documenten ruimtelijke query's. Als u het indexeringsbeleid met de Azure portal wijzigt, kunt u de volgende JSON voor indexeringsbeleid om in te schakelen ruimtelijke indexeren op uw verzameling opgeven.

**Verzameling indexeren JSON met Spatial ingeschakeld voor punten en Veelhoek beleid**

    {
       "automatic":true,
       "indexingMode":"Consistent",
       "includedPaths":[
          {
             "path":"/*",
             "indexes":[
                {
                   "kind":"Range",
                   "dataType":"String",
                   "precision":-1
                },
                {
                   "kind":"Range",
                   "dataType":"Number",
                   "precision":-1
                },
                {
                   "kind":"Spatial",
                   "dataType":"Point"
                },
                {
                   "kind":"Spatial",
                   "dataType":"Polygon"
                }                
             ]
          }
       ],
       "excludedPaths":[
       ]
    }

Hier volgt een codefragment in .NET die laat hoe u een verzameling maken zien met ruimtelijke indexeren is ingeschakeld voor alle paden die punten bevat. 

**Maak een verzameling met ruimtelijke indexeren**

    DocumentCollection spatialData = new DocumentCollection()
    spatialData.IndexingPolicy = new IndexingPolicy(new SpatialIndex(DataType.Point)); //override to turn spatial on by default
    collection = await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), spatialData);

En hier ziet u hoe u een bestaande verzameling om te profiteren van de ruimtelijke indexeren via alle punten die zijn opgeslagen in de documenten kunt wijzigen.

**Een bestaande verzameling met ruimtelijke indexeren wijzigen**

    Console.WriteLine("Updating collection with spatial indexing enabled in indexing policy...");
    collection.IndexingPolicy = new IndexingPolicy(new SpatialIndex(DataType.Point));
    await client.ReplaceDocumentCollectionAsync(collection);

    Console.WriteLine("Waiting for indexing to complete...");
    long indexTransformationProgress = 0;
    while (indexTransformationProgress < 100)
    {
        ResourceResponse<DocumentCollection> response = await client.ReadDocumentCollectionAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"));
        indexTransformationProgress = response.IndexTransformationProgress;

        await Task.Delay(TimeSpan.FromSeconds(1));
    }

> [!NOTE]
> Als de locatie GeoJSON-waarde in het document onjuist gevormd of ongeldig is, wordt klikt u vervolgens het niet geïndexeerd ruimtelijke query's. U kunt de locatie van waarden met ST_ISVALID en ST_ISVALIDDETAILED valideren.
> 
> Als de definitie van de verzameling een partitiesleutel bevat, is indexeren voortgang van de transformatie niet gerapporteerd. 
> 
> 

## <a name="next-steps"></a>Volgende stappen
Nolearned die u hierover aan de slag met ondersteuning voor georuimtelijke in Azure Cosmos DB vernomen dat hebt, kunt u:

* Gaan coderen met de [georuimtelijke .NET-codevoorbeelden op GitHub](https://github.com/Azure/azure-documentdb-dotnet/blob/fcf23d134fc5019397dcf7ab97d8d6456cd94820/samples/code-samples/Geospatial/Program.cs)
* U handen op waarop het georuimtelijke uitvoeren van query's op de [Azure Cosmos DB Query Playground](http://www.documentdb.com/sql/demo#geospatial)
* Meer informatie over [Azure Cosmos DB-Query](sql-api-sql-query.md)
* Meer informatie over [Azure Cosmos DB indexeren beleid](indexing-policies.md)

