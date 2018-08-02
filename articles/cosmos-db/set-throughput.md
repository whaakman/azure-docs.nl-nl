---
title: Doorvoer inrichten voor Azure Cosmos DB | Microsoft Docs
description: Informatie over het instellen van de ingerichte doorvoer voor uw Azure Cosmos DB containsers, verzamelingen, grafieken en tabellen.
services: cosmos-db
author: SnehaGunda
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 07/03/2018
ms.author: sngun
ms.openlocfilehash: 5f022f366c0247fade4cc39925e116a09b3d08de
ms.sourcegitcommit: d4c076beea3a8d9e09c9d2f4a63428dc72dd9806
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2018
ms.locfileid: "39399087"
---
# <a name="set-and-get-throughput-for-azure-cosmos-db-containers-and-database"></a>Instellen en opvragen van doorvoer voor Azure Cosmos DB-containers en -database

U kunt doorvoer instellen voor een Azure Cosmos DB-container of een set van containers met behulp van Azure portal of met behulp van de client-SDK's. 

**Doorvoer voor een afzonderlijke container inrichten:** wanneer u de doorvoer voor een set met containers inrichten, alle containers die de ingerichte doorvoer delen. Inrichting doorvoer voor afzonderlijke containers zorgt ervoor dat de reservering van de doorvoer voor die specifieke container. Bij het toewijzen van RU/sec. op het niveau van de afzonderlijke container, de containers kunnen worden gemaakt als *vaste* of *onbeperkte*. Containers met vaste grootte hebben een maximale limiet van 10 GB en doorvoer van 10.000 RU/s. Voor het maken van een onbeperkte container, moet u een minimale doorvoer van 1000 RU/s en een [partitiesleutel](partition-data.md). Omdat uw gegevens worden verdeeld over meerdere partities moeten mogelijk, is het nodig om het kiezen van een partitiesleutel waarvoor een hoge kardinaliteit (100 naar miljoenen afzonderlijke waarden). Door een partitiesleutel met veel verschillende waarden selecteert, zorgt u ervoor dat uw container/tabel/graph en aanvragen kunnen worden geschaald op uniforme wijze door Azure Cosmos DB. 

**Doorvoer voor een set met containers of een database inrichten:** Provisioning doorvoer voor een database kunt u voor het delen van de doorvoer van alle containers die deel uitmaken van die database. U kunt een set van containers die de doorvoer, evenals de containers die zijn toegewezen doorvoer deelt hebben binnen een Azure Cosmos DB-database. Bij het toewijzen van RU/sec. over een verzameling van containers, de containers die behoren tot deze verzameling worden behandeld als *onbeperkte* containers en moeten een partitiesleutel opgeven.

Op basis van de ingerichte doorvoer, toewijzen Azure Cosmos DB fysieke partities voor het hosten van uw containers en splitsingen/rebalances gegevens over meerdere partities wanneer deze groeit. Container en de inrichting van de database-level doorvoer zijn aparte aanbiedingen en schakelen tussen een van deze vereisen migreren van gegevens van bron naar bestemming. Dit betekent dat u wilt maken van een nieuwe database of een nieuwe verzameling en vervolgens migreren van gegevens met behulp van [bulksgewijs executor bibliotheek](bulk-executor-overview.md) of [Azure Data Factory](../data-factory/connector-azure-cosmos-db.md). De volgende afbeelding ziet u inrichting doorvoer op verschillende niveaus:

![Inrichting van aanvraageenheden voor afzonderlijke containers en instellen van containers](./media/request-units/provisioning_set_containers.png)

In de volgende gedeelten leert u de stappen die zijn vereist voor het configureren van doorvoer op verschillende niveaus voor een Azure Cosmos DB-account. 

## <a name="provision-throughput-by-using-azure-portal"></a>Doorvoer inrichten met behulp van Azure portal

### <a name="provision-throughput-for-a-container-collectiongraphtable"></a>Inrichten doorvoer voor een container (verzameling/grafiek/tabel)

1. Meld u aan bij [Azure Portal](https://portal.azure.com).  
2. Selecteer in de linker navigatie **alle resources** en zoek uw Azure Cosmos DB-account.  
3. U kunt doorvoer configureren tijdens het maken van een container (verzameling, grafiek, tabel) of de doorvoer van de update voor een bestaande container.  
4. Als u wilt toewijzen doorvoer tijdens het maken van een container, opent u de **Data Explorer** blade en selecteer **nieuwe verzameling** (nieuwe grafiek, nieuwe tabel voor andere API's)  
5. Vul het formulier **verzameling toevoegen** blade. Velden in deze blade worden in de volgende tabel beschreven:  

   |**Instelling**  |**Beschrijving**  |
   |---------|---------|
   |Database-id  |  Geef een unieke naam voor het identificeren van uw database. Database is een logische container voor een of meer verzamelingen. Databasenamen moeten tussen de 1 en 255 tekens zijn en mogen geen /, \\, # of ? bevatten en mogen niet eindigen met een spatie. |
   |Verzamelings-id  | Geef een unieke naam voor het identificeren van uw verzameling. Voor id’s van verzamelingen gelden dezelfde tekenvereisten als voor databasenamen. |
   |Opslagcapaciteit   | Deze waarde vertegenwoordigt de opslagcapaciteit van de database. Bij het inrichten van de doorvoer voor een verzameling afzonderlijke opslagcapaciteit kan worden **vast (10 GB)** of **onbeperkt**. Onbeperkte opslagcapaciteit, moet u om in te stellen van een partitiesleutel voor uw gegevens.  |
   |Doorvoer   | Elke verzameling en -database hebben doorvoer in aanvraageenheden per seconde.  Voor vaste opslagcapaciteit, minimaal doorvoer is 400 aanvraageenheden per seconde (RU/s), voor onbeperkte opslag capaciteit, minimum doorvoer is ingesteld op 1000 RU/s.|

6. Nadat u waarden voor deze velden opgeven, selecteert u **OK** de instellingen op te slaan.  

   ![Doorvoer instellen voor een verzameling](./media/set-throughput/set-throughput-for-container.png)

7. Voor het bijwerken van de doorvoer voor een bestaande container, vouw de database en de container en klik vervolgens op **instellingen**. Typ in het nieuwe venster de nieuwe waarde voor de doorvoer en selecteer vervolgens **opslaan**.  

   ![Doorvoer voor een verzameling bijwerken](./media/set-throughput/update-throughput-for-container.png)

### <a name="provision-throughput-for-a-set-of-containers-or-at-the-database-level"></a>Doorvoer voor een set van containers of op het databaseniveau van de inrichten

1. Meld u aan bij [Azure Portal](https://portal.azure.com).  
2. Selecteer in de linker navigatie **alle resources** en zoek uw Azure Cosmos DB-account.  
3. U kunt doorvoer configureren tijdens het maken van de doorvoer van een database of een update voor een bestaande database.  
4. Als u wilt toewijzen doorvoer tijdens het maken van een database, opent u de **Data Explorer** blade en selecteer **nieuwe Database**  
5. Vul de **Database-id** waarde selectievakje **inrichten doorvoer** optie en configureer doorvoercapaciteit. Een database kan worden ingericht met minimale doorvoercapaciteit 50.000 RU/s.  

   ![Doorvoer instellen met de optie voor de nieuwe database](./media/set-throughput/set-throughput-with-new-database-option.png)

6. Voor het bijwerken van de doorvoer voor een bestaande database, vouw de database en de container en klik vervolgens op **schaal**. Typ in het nieuwe venster de nieuwe waarde voor de doorvoer en selecteer vervolgens **opslaan**.  

   ![Doorvoer voor een database bijwerken](./media/set-throughput/update-throughput-for-database.png)

### <a name="provision-throughput-for-a-set-of-containers-as-well-as-for-an-individual-container-in-a-database"></a>Doorvoer voor een set met containers ook als voor een afzonderlijke container in een database inrichten

1. Meld u aan bij [Azure Portal](https://portal.azure.com).  
2. Selecteer in de linker navigatie **alle resources** en zoek uw Azure Cosmos DB-account.  
3. Een database maken en toewijzen van doorvoer toe. Open de **Data Explorer** blade en selecteer **nieuwe Database**  
4. Vul de **Database-id** waarde selectievakje **inrichten doorvoer** optie en configureer doorvoercapaciteit. Een database kan worden ingericht met minimale doorvoercapaciteit 50.000 RU/s.  

   ![Doorvoer instellen met de optie voor de nieuwe database](./media/set-throughput/set-throughput-with-new-database-option.png)

5. Vervolgens maakt u een verzameling in de database die u hebt gemaakt in hierboven genoemde stap. Voor het maken van een verzameling met de rechtermuisknop op de database en selecteer **nieuwe verzameling**.  

6. In de **verzameling toevoegen** blade, voer een naam voor de verzameling en partitiesleutel. (Optioneel) u kunt de doorvoer voor die specifieke container inrichten als u ervoor kiest geen waarde toe te wijzen een doorvoer, de doorvoer die is toegewezen aan de database wordt gedeeld met de verzameling.  

   ![Doorvoer (optioneel) instellen voor de container](./media/set-throughput/optionally-set-throughput-for-the-container.png)

## <a name="considerations-when-provisioning-throughput"></a>Overwegingen bij het inrichten van doorvoer

Hieronder vindt u enkele overwegingen waarmee u kunt beslissen over een strategie voor de reservering doorvoer.

### <a name="considerations-when-provisioning-throughput-at-the-database-level"></a>Overwegingen bij het inrichten van doorvoer op databaseniveau

Houd rekening met het inrichten van doorvoer op databaseniveau (dat wil zeggen voor een set met containers) in de volgende gevallen:

* Als u meer dan tien of meer aantal containers dat doorvoer tussen sommige of alle mappen delen kan hebt.  

* Wanneer u migreert van een database met één tenant die is ontworpen om te worden uitgevoerd op virtuele machines van IaaS gehost of on-premises (voor bijvoorbeeld NoSQL of relationele databases) met Azure Cosmos DB en veel containers hebt.  

* Als u gebruiken niet-geplande pieken in de werkbelastingen wilt met behulp van gegroepeerde doorvoer op het databaseniveau van de.  

* In plaats van de doorvoer van de instelling voor een afzonderlijke container bent u geïnteresseerd in het ophalen van de cumulatieve doorvoer van een set van containers in de database.

### <a name="considerations-when-provisioning-throughput-at-the-container-level"></a>Overwegingen bij het inrichten van doorvoer op het niveau van de container

Houd rekening met provisioning doorvoer op een afzonderlijke container in de volgende gevallen:

* Hebt u een kleiner aantal Azure Cosmos DB-containers.  

* Als u wilt de gegarandeerde doorvoer voor een bepaalde container gesteund door een SLA aan.

## <a name="throughput-ranges"></a>Doorvoer bereiken

De volgende tabel bevat de doorvoer beschikbaar voor containers:

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p></p></td>
            <td valign="top"><p><strong>Container van één partitie</strong></p></td>
            <td valign="top"><p><strong>Gepartitioneerde Container</strong></p></td>
            <td valign="top"><p><strong>Instellen van Containers</strong></p></td>
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

<a id="set-throughput-sdk"></a>

## <a name="set-throughput-by-using-sql-api-for-net"></a>Doorvoer instellen met behulp van SQL-API voor .NET

### <a name="set-throughput-at-the-container-level"></a>Doorvoer instellen op het niveau van de container
Hier volgt een codefragment voor het maken van een container met 3.000 aanvraageenheden per seconde voor een afzonderlijke container met behulp van de SQL-API .NET SDK:

```csharp
DocumentCollection myCollection = new DocumentCollection();
myCollection.Id = "coll";
myCollection.PartitionKey.Paths.Add("/deviceId");

await client.CreateDocumentCollectionAsync(
    UriFactory.CreateDatabaseUri("db"),
    myCollection,
    new RequestOptions { OfferThroughput = 3000 });
```

### <a name="set-throughput-for-a-set-of-containers-at-the-database-level"></a>Doorvoer voor een set van containers op databaseniveau instellen

Hier volgt een codefragment voor inrichting 100.000 aanvraageenheden per seconde in een set van containers met behulp van de SQL-API .NET SDK:

```csharp
// Provision 100,000 RU/sec at the database level. 
// sharedCollection1 and sharedCollection2 will share the 100,000 RU/sec from the parent database
// dedicatedCollection will have its own dedicated 4,000 RU/sec, independant of the 100,000 RU/sec provisioned from the parent database
Database database = client.CreateDatabaseAsync(new Database { Id = "myDb" }, new RequestOptions { OfferThroughput = 100000 }).Result;

DocumentCollection sharedCollection1 = new DocumentCollection();
sharedCollection1.Id = "sharedCollection1";
sharedCollection1.PartitionKey.Paths.Add("/deviceId");

await client.CreateDocumentCollectionAsync(database.SelfLink, sharedCollection1, new RequestOptions())

DocumentCollection sharedCollection2 = new DocumentCollection();
sharedCollection2.Id = "sharedCollection2";
sharedCollection2.PartitionKey.Paths.Add("/deviceId");

await client.CreateDocumentCollectionAsync(database.SelfLink, sharedCollection2, new RequestOptions())

DocumentCollection dedicatedCollection = new DocumentCollection();
dedicatedCollection.Id = "dedicatedCollection";
dedicatedCollection.PartitionKey.Paths.Add("/deviceId");

await client.CreateDocumentCollectionAsync(database.SelfLink, dedicatedCollection, new RequestOptions { OfferThroughput = 4000 )
```

Azure Cosmos DB is van invloed op een model reservering voor doorvoer. Dat wil zeggen, in rekening gebracht voor de hoeveelheid doorvoer *gereserveerde*, ongeacht hoeveel dat de doorvoer is actief *gebruikt*. Als uw toepassing de belasting, gegevens en gebruiksgegevens patronen wijzigen kunt u eenvoudig de schaal omhoog en omlaag het aantal gereserveerde ru's via SDK's of met behulp van de [Azure Portal](https://portal.azure.com).

Elke container of een set van containers, is toegewezen aan een `Offer` resource in Azure Cosmos DB, met metagegevens over de ingerichte doorvoer. U kunt de toegewezen doorvoer wijzigen door het opzoeken van de bijbehorende resource-aanbieding voor een container en klik vervolgens met de nieuwe waarde voor de doorvoer wordt bijgewerkt. Hier volgt een codefragment voor het wijzigen van de doorvoer van een container in 5000 aanvraageenheden per seconde door de .NET SDK. Na het wijzigen van de doorvoer, moet u een bestaande Azure portal windows voor de gewijzigde doorvoer weergegeven vernieuwen. 

```csharp
// Fetch the resource to be updated
// For a updating throughput for a set of containers, replace the collection's self link with the database's self link
Offer offer = client.CreateOfferQuery()
                .Where(r => r.ResourceLink == collection.SelfLink)    
                .AsEnumerable()
                .SingleOrDefault();

// Set the throughput to 5000 request units per second
offer = new OfferV2(offer, 5000);

// Now persist these changes to the database by replacing the original resource
await client.ReplaceOfferAsync(offer);
```

Er zijn geen gevolgen voor de beschikbaarheid van de container of set van containers, wanneer u de doorvoer wijzigen. De nieuwe gereserveerde doorvoer is doorgaans effectieve binnen enkele seconden op aanvraag van de nieuwe doorvoer.

<a id="set-throughput-java"></a>

## <a name="to-set-the-throughput-by-using-the-sql-api-for-java"></a>De doorvoer instellen met behulp van de SQL-API voor Java

Het volgende codefragment wordt de huidige doorvoer opgehaald en gewijzigd in 500 RU/s. Zie voor een volledige codevoorbeeld de [OfferCrudSamples.java](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/OfferCrudSamples.java) -bestand op GitHub. 

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

## <a name="get-throughput-by-using-mongodb-api-portal-metrics"></a>Doorvoer ophalen met behulp van MongoDB-API-portal metrische gegevens

De eenvoudigste manier om een goede schatting van de aanvraag eenheid kosten in rekening gebracht voor uw MongoDB-API-database is het gebruik van de [Azure-portal](https://portal.azure.com) metrische gegevens. Met de *aantal aanvragen* en *aanvraag kosten in rekening gebracht* grafieken, krijgt u een schatting van het aantal aanvraageenheden elke bewerking is verbruikt en het aantal aanvraageenheden dat ze gebruiken ten opzichte van elkaar.

![Portal metrische gegevens voor MongoDB-API][1]

### <a id="RequestRateTooLargeAPIforMongoDB"></a> Overschrijding van grenzen van de gereserveerde doorvoer in de MongoDB-API
Toepassingen die groter zijn dan de ingerichte doorvoer voor een container of een set van containers zijn beperkt in de snelheid totdat het tarief verbruik lager is dan het tarief voor de ingerichte doorvoer. Wanneer er een tarief-beperking optreedt, de back-end verloopt de aanvraag met een `16500` foutcode - `Too Many Requests`. Standaard de MongoDB-API wordt automatisch opnieuw geprobeerd maximaal 10 keer alvorens een `Too Many Requests` foutcode. Als u veel ontvangt `Too Many Requests` foutcodes, kunt u overwegen toe te voegen een logica voor opnieuw proberen routines voor foutafhandeling van uw toepassing of [verhogen van de ingerichte doorvoer voor de container](set-throughput.md).

## <a id="GetLastRequestStatistics"></a>Aanvraag kosten in rekening gebracht met behulp van MongoDB-API GetLastRequestStatistics opdracht ophalen

De MongoDB-API biedt ondersteuning voor een aangepaste opdracht *getLastRequestStatistics*, voor het ophalen van de kosten van de aanvraag voor een bepaalde bewerking.

Voer bijvoorbeeld de bewerking die u wilt controleren of de aanvraag kosten in rekening gebracht voor in de Mongo-shell.
```
> db.sample.find()
```

Vervolgens geeft u de opdracht *getLastRequestStatistics*.
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

Een methode voor het schatten van de hoeveelheid gereserveerde doorvoer die is vereist voor de toepassing is op te nemen de kosten van de aanvraag-eenheden die zijn gekoppeld aan met het normale bewerkingen uitvoeren in een representatieve item dat wordt gebruikt door uw toepassing en maak vervolgens een schatting van het aantal bewerkingen die u verwacht om uit te voeren per seconde.

> [!NOTE]
> Hebt u itemtypen die aanzienlijk wat betreft grootte en het aantal geïndexeerde eigenschappen verschillen, en noteer vervolgens de kosten van toepassing bewerking aanvraag eenheden die zijn gekoppeld aan elk *type* van gemiddeld item.
> 
> 

## <a id="RequestchargeGraphAPI"></a>Aanvraag kosten in rekening gebracht voor de Gremlin-API-accounts ophalen 

Hier volgt een voorbeeld voor het verkrijgen van aanvraag kosten in rekening gebracht voor de Gremlin-API-accounts met behulp van de bibliotheek Gremlin.Net. 

```csharp

var response = await gremlinClient.SubmitAsync<int>(requestMsg, bindings);
                var resultSet = response.AsResultSet();
                var statusAttributes= resultSet.StatusAttributes;
```

Naast de bovenstaande methode, kunt u "x-ms-totaal aantal-verzoek-kosten" header ook gebruiken voor berekeningen van Aanvraageenheden.


## <a name="throughput-faq"></a>Veelgestelde vragen over de doorvoer

**Kan ik mijn doorvoer op minder dan 400 RU/s instellen?**

400 RU/s is de minimale doorvoer beschikbaar is voor Cosmos DB één partitie containers (1000 RU/s is het minimale aantal voor gepartitioneerde containers). De aanvraag eenheden worden ingesteld in intervallen van 100 RU/s, maar de doorvoer kan niet worden ingesteld op 100 RU/s of een willekeurige waarde kleiner zijn dan 400 RU/s. Als u naar een rendabele methode zoekt voor het ontwikkelen en testen van Cosmos DB, kunt u de gratis [Azure Cosmos DB-Emulator](local-emulator.md), die u lokaal kunt implementeren zonder kosten. 

**Hoe stel ik de doorvoer met behulp van de MongoDB-API?**

Er is geen MongoDB-API-extensie om in te stellen doorvoer. De aanbeveling is het gebruik van de SQL-API, zoals wordt weergegeven in [de doorvoer instellen met behulp van de SQL-API voor .NET](#set-throughput-sdk).

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over het schatten van de doorvoer- en aanvraageenheden, [aanvragen rekeneenheden en het schatten van de doorvoer in Azure Cosmos DB](request-units.md)

* Zie voor meer informatie over het inrichten en gaan op planetaire schaal met Cosmos DB, [partitioneren en schalen met Cosmos DB](partition-data.md).

[1]: ./media/set-throughput/api-for-mongodb-metrics.png
