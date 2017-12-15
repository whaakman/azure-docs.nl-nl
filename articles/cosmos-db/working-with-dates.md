---
title: Werken met datums in Azure Cosmos DB | Microsoft Docs
description: Meer informatie over het werken met datums in Azure Cosmos DB.
services: cosmos-db
author: arramac
manager: jhubbard
editor: mimig
documentationcenter: 
ms.assetid: e587772f-ce9f-498c-a017-a51e7265bb23
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: arramac
ms.openlocfilehash: 1a54884196e5b4ff5b16425e902abeb8d82aa8f1
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2017
---
# <a name="working-with-dates-in-azure-cosmos-db"></a>Werken met datums in Azure Cosmos DB
Azure Cosmos DB biedt schemaflexibiliteit en geavanceerde indexeermogelijkheden via een systeemeigen [JSON](http://www.json.org) gegevensmodel. Alle Azure DB die Cosmos-bronnen, zoals databases, verzamelingen, documenten en opgeslagen procedures worden gemodelleerd en opgeslagen als JSON-documenten. Als een vereiste voor draagbare wordt JSON (en Azure Cosmos DB) ondersteunt een kleine set basistypen: String, getal, Booleaanse waarde, Array, Object en Null. Echter JSON is flexibel en ontwikkelaars en frameworks voor complexe typen met behulp van deze primitieven en samenstellen van deze objecten of-matrices. 

Naast de basistypen veel toepassingen moeten de [DateTime](https://msdn.microsoft.com/library/system.datetime(v=vs.110).aspx) type datums en tijdstempels vertegenwoordigt. Dit artikel wordt beschreven hoe ontwikkelaars kunnen opslaan, ophalen en query datums in Azure Cosmos DB met de .NET SDK.

## <a name="storing-datetimes"></a>Opslaan van datum/tijd
Standaard de [Azure Cosmos DB SDK](sql-api-sdk-dotnet.md) serialiseert datum/tijd-waarden als [ISO 8601](http://www.iso.org/iso/catalogue_detail?csnumber=40874) tekenreeksen. De meeste toepassingen kunnen de standaard tekenreeksweergave gebruiken voor datum/tijd om de volgende redenen:

* Tekenreeksen kunnen worden vergeleken en de relatieve volgorde van de datum/tijd-waarden behouden blijft wanneer ze op tekenreeksen worden getransformeerd. 
* Deze benadering vereist geen aangepaste code of -kenmerken voor JSON-conversie.
* De datums die is opgeslagen in JSON menselijke worden gelezen.
* Deze aanpak kunt profiteren van Azure Cosmos DB index voor snelle queryprestaties.

Bijvoorbeeld, het volgende fragment slaat een `Order` objecteigenschappen met twee DateTime - `ShipDate` en `OrderDate` als een document met de .NET SDK:

    public class Order
    {
        [JsonProperty(PropertyName="id")]
        public string Id { get; set; }
        public DateTime OrderDate { get; set; }
        public DateTime ShipDate { get; set; }
        public double Total { get; set; }
    }

    await client.CreateDocumentAsync("/dbs/orderdb/colls/orders", 
        new Order 
        { 
            Id = "09152014101",
            OrderDate = DateTime.UtcNow.AddDays(-30),
            ShipDate = DateTime.UtcNow.AddDays(-14), 
            Total = 113.39
        });

Dit document wordt opgeslagen in Azure Cosmos DB als volgt:

    {
        "id": "09152014101",
        "OrderDate": "2014-09-15T23:14:25.7251173Z",
        "ShipDate": "2014-09-30T23:14:25.7251173Z",
        "Total": 113.39
    }
    

U kunt ook kunt u de datum/tijd als Unix tijdstempels, dat wil zeggen, opslaan als een getal dat het aantal verstreken seconden sinds 1 januari 1970 vertegenwoordigt. Interne tijdstempel Azure Cosmos-DB (`_ts`) eigenschap deze aanpak volgt. U kunt de [UnixDateTimeConverter](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.unixdatetimeconverter.aspx) klasse voor het serialiseren van datum/tijd as-nummers. 

## <a name="indexing-datetimes-for-range-queries"></a>Indexeren van datum/tijd voor query's bereik
Bereik query's zijn veelvoorkomende met DateTime-waarden. Als u wilt zoeken naar alle bestellingen die zijn gemaakt sinds gisteren of zoeken naar alle bestellingen in de laatste vijf minuten hebt verzonden, moet u bijvoorbeeld bereik query's uitvoeren. Voor het uitvoeren van deze query's efficiënt, moet u uw collectie bereik indexeren op tekenreeksen configureren.

    DocumentCollection collection = new DocumentCollection { Id = "orders" };
    collection.IndexingPolicy = new IndexingPolicy(new RangeIndex(DataType.String) { Precision = -1 });
    await client.CreateDocumentCollectionAsync("/dbs/orderdb", collection);

U kunt meer informatie over het configureren van indexering beleid op [Azure Cosmos DB indexeren beleid](indexing-policies.md).

## <a name="querying-datetimes-in-linq"></a>Datum/tijd in LINQ opvragen
De SQL-SDK voor .NET ondersteunt automatisch gegevens opgeslagen in Azure Cosmos DB via LINQ opvragen. Bijvoorbeeld, toont het volgende fragment een LINQ-query die filters orders die zijn verzonden in de afgelopen drie dagen.

    IQueryable<Order> orders = client.CreateDocumentQuery<Order>("/dbs/orderdb/colls/orders")
        .Where(o => o.ShipDate >= DateTime.UtcNow.AddDays(-3));
          
    // Translated to the following SQL statement and executed on Azure Cosmos DB
    SELECT * FROM root WHERE (root["ShipDate"] >= "2016-12-18T21:55:03.45569Z")

U kunt meer informatie over Azure Cosmos DB SQL-querytaal en de LINQ-provider op [Cosmos DB opvragen](sql-api-sql-query.md).

In dit artikel wordt bekeken hoe opslaan, index-en datum/tijd in Azure Cosmos DB een query.

## <a name="next-steps"></a>Volgende stappen
* Downloaden en uitvoeren van de [codevoorbeelden op GitHub](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples)
* Meer informatie over [SQL-query's](sql-api-sql-query.md)
* Meer informatie over [Azure Cosmos DB indexeren beleid](indexing-policies.md)
