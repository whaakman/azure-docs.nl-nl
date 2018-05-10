---
title: Inrichten doorvoer voor Azure Cosmos DB | Microsoft Docs
description: Informatie over het instellen van de ingerichte doorvoer voor uw Azure Cosmos DB containsers, verzamelingen, grafieken en tabellen.
services: cosmos-db
author: SnehaGunda
manager: kfile
documentationcenter: ''
ms.assetid: f98def7f-f012-4592-be03-f6fa185e1b1e
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/07/2018
ms.author: sngun
ms.openlocfilehash: bede91ed3ffc456740a0eb63ed7a15278e99ebe2
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
# <a name="set-and-get-throughput-for-azure-cosmos-db-containers"></a>Instellen en ophalen van doorvoer voor Azure DB die Cosmos-containers

U kunt de doorvoer instellen voor uw Azure DB die Cosmos-containers of een set van containers in de Azure-portal of met behulp van de client-SDK's. 

De volgende tabel bevat de doorvoer die beschikbaar zijn voor containers:

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p></p></td>
            <td valign="top"><p><strong>Container één partitie</strong></p></td>
            <td valign="top"><p><strong>Gepartitioneerde Container</strong></p></td>
            <td valign="top"><p><strong>Aantal Containers</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>Minimaal doorvoer</p></td>
            <td valign="top"><p>400 aanvraageenheden per seconde</p></td>
            <td valign="top"><p>1000 aanvraageenheden per seconde</p></td>
            <td valign="top"><p>50.000 aanvraageenheden per seconde</p></td>
        </tr>
        <tr>
            <td valign="top"><p>Maximale doorvoer</p></td>
            <td valign="top"><p>10.000 aanvraageenheden per seconde</p></td>
            <td valign="top"><p>Onbeperkt</p></td>
            <td valign="top"><p>Onbeperkt</p></td>
        </tr>
    </tbody>
</table>

## <a name="to-set-the-throughput-by-using-the-azure-portal"></a>De doorvoer instellen via de Azure-portal

1. In een nieuw venster openen de [Azure-portal](https://portal.azure.com).
2. Klik op de linkerbalk op **Azure Cosmos DB**, of klik op **alle services** aan de onderkant en schuif vervolgens naar **Databases**, en klik vervolgens op **Azure Cosmos DB**.
3. Selecteer uw account Cosmos DB.
4. Klik in het nieuwe venster **Data Explorer** in het navigatiemenu.
5. Vouw uw database en de container in het nieuwe venster en klik vervolgens op **schaal & instellingen**.
6. Typ in het nieuwe venster de nieuwe waarde van de doorvoer in de **doorvoer** vak en klik vervolgens op **opslaan**.

<a id="set-throughput-sdk"></a>

## <a name="to-set-the-throughput-by-using-the-sql-api-for-net"></a>De doorvoer instellen met behulp van de SQL-API voor .NET

Het volgende codefragment haalt de huidige doorvoer en gewijzigd in 500 RU/s. Zie voor het volledige codevoorbeeld wordt de [CollectionManagement](https://github.com/Azure/azure-documentdb-dotnet/blob/95521ff51ade486bb899d6913880995beaff58ce/samples/code-samples/CollectionManagement/Program.cs#L188-L216) project op GitHub.

```csharp
// Fetch the offer of the collection whose throughput needs to be updated
// To change the throughput for a set of containers, use the database's selflink instead of the collection's selflink
Offer offer = client.CreateOfferQuery()
    .Where(r => r.ResourceLink == collection.SelfLink)    
    .AsEnumerable()
    .SingleOrDefault();

// Set the throughput to the new value, for example 500 request units per second
offer = new OfferV2(offer, 500);

// Now persist these changes to the collection by replacing the original offer resource
await client.ReplaceOfferAsync(offer);
```

<a id="set-throughput-java"></a>

## <a name="to-set-the-throughput-by-using-the-sql-api-for-java"></a>De doorvoer instellen met behulp van de SQL-API voor Java

Het volgende codefragment haalt de huidige doorvoer en gewijzigd in 500 RU/s. Zie voor een compleet codevoorbeeld de [OfferCrudSamples.java](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/OfferCrudSamples.java) -bestand op GitHub. 

```Java
// find offer associated with this collection
// To change the throughput for a set of containers, use the database's resource id instead of the collection's resource id
Iterator < Offer > it = client.queryOffers(
    String.format("SELECT * FROM r where r.offerResourceId = '%s'", collectionResourceId), null).getQueryIterator();
assertThat(it.hasNext(), equalTo(true));

Offer offer = it.next();
assertThat(offer.getString("offerResourceId"), equalTo(collectionResourceId));
assertThat(offer.getContent().getInt("offerThroughput"), equalTo(throughput));

// update the offer
int newThroughput = 500;
offer.getContent().put("offerThroughput", newThroughput);
client.replaceOffer(offer);
```

## <a id="GetLastRequestStatistics"></a>Doorvoer ophalen met behulp van de MongoDB-API GetLastRequestStatistics-opdracht

De MongoDB-API biedt ondersteuning voor een aangepaste opdracht *getLastRequestStatistics*, voor het ophalen van de kosten van de aanvraag voor een bepaalde bewerking.

Bijvoorbeeld, in de Mongo-Shell uitvoeren van de bewerking die u wilt controleren of de kosten van de aanvraag voor.
```
> db.sample.find()
```

Voer vervolgens de opdracht *getLastRequestStatistics*.
```
> db.runCommand({getLastRequestStatistics: 1})
{
    "_t": "GetRequestStatisticsResponse",
    "ok": 1,
    "CommandName": "OP_QUERY",
    "RequestCharge": 2.48,
    "RequestDurationInMilliSeconds" : 4.0048
}
```

Met dat gegeven in gedachte, één methode voor het schatten van de hoeveelheid gereserveerde doorvoer vereist door uw toepassing is voor het vastleggen van de aanvraag eenheid kosten die zijn gekoppeld aan met het normale bewerkingen uitvoeren in een representatieve item gebruikt door de toepassing en vervolgens schatting maken van de aantal bewerkingen die om uit te voeren per seconde wordt verwacht.

> [!NOTE]
> Als er itemtypen die aanzienlijk in termen van de grootte en het aantal geïndexeerde eigenschappen verschillen wordt, en noteer vervolgens de betreffende bewerking aanvraag eenheid kosten die zijn gekoppeld aan elk *type* van typische item.
> 
> 

## <a name="get-throughput-by-using-mongodb-api-portal-metrics"></a>Doorvoer verkrijgen door middel van MongoDB API portal metrische gegevens

De eenvoudigste manier om een goede indicatie van aanvraag ophalen eenheid kosten voor de MongoDB-API-database is het gebruik van de [Azure-portal](https://portal.azure.com) metrische gegevens. Met de *aantal aanvragen* en *aanvraag kosten* grafieken, kunt u een schatting van het aantal aanvraageenheden elke bewerking verbruikt en het aantal aanvraageenheden die ze gebruiken ten opzichte van elkaar ophalen.

![MongoDB API portal metrische gegevens][1]

### <a id="RequestRateTooLargeAPIforMongoDB"></a> Overschrijding van gereserveerde doorvoer grenzen in de MongoDB-API
Toepassingen die groter is dan de ingerichte doorvoer voor een container of een set van containers zijn beperkt in de frequentie waarmee totdat het verbruiksniveau onder de snelheid van de ingerichte doorvoer zakt. Wanneer een snelheid-beperking optreedt, de back-end optie preventief beëindigd wanneer de aanvraag met een `16500` foutcode - `Too Many Requests`. Standaard de MongoDB-API wordt automatisch opnieuw geprobeerd maximaal 10 keer voordat er een `Too Many Requests` foutcode. Als er veel `Too Many Requests` foutcodes, kunt u overwegen een toe te voegen aan een Pogingslogica routines voor foutafhandeling van uw toepassing of [verhogen ingerichte doorvoer voor de container](set-throughput.md).

## <a name="throughput-faq"></a>Veelgestelde vragen over doorvoer

**Kan ik mijn doorvoer ingesteld op minder dan 400 RU/s**

400 RU/s is de minimale doorvoer voor Cosmos DB één partitie containers (het minimum voor gepartitioneerde containers 1000 RU/s is). Aanvraag eenheden worden ingesteld met intervallen van 100 RU/s, maar doorvoer kan niet worden ingesteld op 100 RU/s of een willekeurige waarde kleiner is dan 400 RU/s. Als u naar een voordelige methode zoekt te ontwikkelen en testen van de Cosmos-DB, kunt u de gratis [Azure Cosmos DB Emulator](local-emulator.md), die u lokaal kunt implementeren zonder kosten. 

**Hoe stel ik doorvoer met de MongoDB-API**

Er is geen extensie MongoDB-API om in te stellen doorvoer. De aanbeveling is het gebruik van de SQL-API, zoals wordt weergegeven in [de doorvoer instellen met behulp van de SQL-API voor .NET](#set-throughput-sdk).

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het inrichten en gaat wereld-schaal met Cosmos DB, [partitionering en schalen met Cosmos DB](partition-data.md).

[1]: ./media/set-throughput/api-for-mongodb-metrics.png