---
title: Inrichten doorvoer voor Azure Cosmos DB | Microsoft Docs
description: Informatie over het instellen van de ingerichte doorvoer voor uw Azure Cosmos DB containsers, verzamelingen, grafieken en tabellen.
services: cosmos-db
author: mimig1
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: f98def7f-f012-4592-be03-f6fa185e1b1e
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/02/2018
ms.author: mimig
ms.openlocfilehash: 8797910651c54baa3529b015d4195cf2a5c06ece
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="set-throughput-for-azure-cosmos-db-containers"></a>Doorvoer instellen voor Azure DB die Cosmos-containers

U kunt de doorvoer instellen voor uw Azure DB die Cosmos-containers in de Azure-portal of met behulp van de client-SDK's. 

De volgende tabel bevat de doorvoer die beschikbaar zijn voor containers:

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p></p></td>
            <td valign="top"><p><strong>Container één partitie</strong></p></td>
            <td valign="top"><p><strong>Gepartitioneerde Container</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>Minimaal doorvoer</p></td>
            <td valign="top"><p>400 aanvraageenheden per seconde</p></td>
            <td valign="top"><p>1000 aanvraageenheden per seconde</p></td>
        </tr>
        <tr>
            <td valign="top"><p>Maximale doorvoer</p></td>
            <td valign="top"><p>10.000 aanvraageenheden per seconde</p></td>
            <td valign="top"><p>Onbeperkt</p></td>
        </tr>
    </tbody>
</table>

## <a name="to-set-the-throughput-by-using-the-azure-portal"></a>De doorvoer instellen via de Azure-portal

1. In een nieuw venster openen de [Azure-portal](https://portal.azure.com).
2. Klik op de linkerbalk op **Azure Cosmos DB**, of klik op **meer Services** aan de onderkant en schuif vervolgens naar **Databases**, en klik vervolgens op **Azure Cosmos DB**.
3. Selecteer uw account Cosmos DB.
4. Klik in het nieuwe venster **Data Explorer** in het navigatiemenu.
5. Vouw uw database en de container in het nieuwe venster en klik vervolgens op **schaal & instellingen**.
6. Typ in het nieuwe venster de nieuwe waarde van de doorvoer in de **doorvoer** vak en klik vervolgens op **opslaan**.

<a id="set-throughput-sdk"></a>

## <a name="to-set-the-throughput-by-using-the-sql-api-for-net"></a>De doorvoer instellen met behulp van de SQL-API voor .NET

```csharp
// Fetch the offer of the collection whose throughput needs to be updated
Offer offer = client.CreateOfferQuery()
    .Where(r => r.ResourceLink == collection.SelfLink)    
    .AsEnumerable()
    .SingleOrDefault();

// Set the throughput to the new value, for example 12,000 request units per second
offer = new OfferV2(offer, 12000);

// Now persist these changes to the collection by replacing the original offer resource
await client.ReplaceOfferAsync(offer);
```

<a id="set-throughput-java"></a>

## <a name="to-set-the-throughput-by-using-the-sql-api-for-java"></a>De doorvoer instellen met behulp van de SQL-API voor Java

In dit fragment is overgenomen uit het bestand OfferCrudSamples.java in de [azure-documentdb-java](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/OfferCrudSamples.java) opslagplaats. 

```Java
// find offer associated with this collection
Iterator < Offer > it = client.queryOffers(
    String.format("SELECT * FROM r where r.offerResourceId = '%s'", collectionResourceId), null).getQueryIterator();
assertThat(it.hasNext(), equalTo(true));

Offer offer = it.next();
assertThat(offer.getString("offerResourceId"), equalTo(collectionResourceId));
assertThat(offer.getContent().getInt("offerThroughput"), equalTo(throughput));

// update the offer
int newThroughput = 10300;
offer.getContent().put("offerThroughput", newThroughput);
client.replaceOffer(offer);
```

## <a name="throughput-faq"></a>Veelgestelde vragen over doorvoer

**Kan ik mijn doorvoer ingesteld op minder dan 400 RU/s**

400 RU/s is de minimale doorvoer voor Cosmos DB één partitie containers (het minimum voor gepartitioneerde containers 1000 RU/s is). Aanvraag eenheden worden ingesteld met intervallen van 100 RU/s, maar doorvoer kan niet worden ingesteld op 100 RU/s of een willekeurige waarde kleiner is dan 400 RU/s. Als u naar een voordelige methode zoekt te ontwikkelen en testen van de Cosmos-DB, kunt u de gratis [Azure Cosmos DB Emulator](local-emulator.md), die u lokaal kunt implementeren zonder kosten. 

**Hoe stel ik througput met de MongoDB-API**

Er is geen extensie MongoDB-API om in te stellen doorvoer. De aanbeveling is het gebruik van de SQL-API, zoals wordt weergegeven in [de doorvoer instellen met behulp van de SQL-API voor .NET](#set-throughput-sdk).

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het inrichten en gaat wereld-schaal met Cosmos DB, [partitionering en schalen met Cosmos DB](partition-data.md).
