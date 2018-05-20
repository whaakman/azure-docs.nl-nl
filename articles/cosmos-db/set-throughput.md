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
ms.date: 05/09/2018
ms.author: sngun
ms.openlocfilehash: 925167c6b4a7f173726ec094c2847a16ca3d0ef4
ms.sourcegitcommit: d78bcecd983ca2a7473fff23371c8cfed0d89627
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/14/2018
---
# <a name="set-and-get-throughput-for-azure-cosmos-db-containers-and-database"></a>Stel en doorvoer ophalen voor Azure DB die Cosmos-containers en -database

Met behulp van Azure-portal of met behulp van de client-SDK's, kunt u de doorvoer instellen voor een Azure DB die Cosmos-container of een set van containers. Wanneer u de doorvoer voor een set van containers inricht, delen alle containers die de ingerichte doorvoer. Inrichting doorvoer voor afzonderlijke containers wordt gegarandeerd dat de reservering van doorvoer voor die specifieke container. Aan de andere kant kunt u de doorvoer van de containers die deel uitmaken van die database delen inrichting doorvoer voor een database. U kunt een set van containers die delen van de doorvoer als containers voor doorvoer hebben speciale hebben in een Azure DB die Cosmos-database. 

Op basis van de ingerichte doorvoer, toewijzen Azure Cosmos DB fysieke partities voor het hosten van de container (s) en splitsingen/rebalances gegevens meerdere partities wanneer deze groeit.

Bij het toewijzen van RU per seconde op het niveau van afzonderlijke container de containers kunnen worden gemaakt als *vaste* of *onbeperkte*. Containers met vaste grootte hebben een maximale limiet van 10 GB en doorvoer van 10.000 RU/s. Een onbeperkte om container te maken, moet u een minimale doorvoer van 1000 RU/s en een [partitiesleutel](partition-data.md). Aangezien uw gegevens hebben mogelijk om te worden verdeeld over meerdere partities, is het nodig om het kiezen van een partitiesleutel met een hoge kardinaliteit (100 tot miljoenen afzonderlijke waarden). Als u een partitiesleutel met veel afzonderlijke waarden selecteert, zorgt u ervoor dat uw tabel-container/grafiek en aanvragen kunnen worden geschaald op uniforme wijze door Azure Cosmos DB. 

Bij het toewijzen van RU per seconde over een set van containers, de containers die horen bij deze set worden behandeld als *onbeperkte* containers en een partitiesleutel moet opgeven.

![Inrichting aanvraageenheden voor afzonderlijke containers en set containers](./media/request-units/provisioning_set_containers.png)

Dit artikel begeleidt u bij de stappen die nodig zijn voor het configureren van doorvoer op verschillende niveaus voor een Azure DB die Cosmos-account. 

## <a name="provision-throughput-by-using-azure-portal"></a>Doorvoer inrichten met behulp van Azure portal

### <a name="provision-throughput-for-a-container-collectiongraphtable"></a>Inrichten doorvoer voor een container (graph-verzameling/tabel)

1. Meld u aan bij [Azure Portal](https://portal.azure.com).  
2. Selecteer in de Linkernavigatie **alle resources** en zoek uw account voor Azure Cosmos DB.  
3. U kunt de doorvoer configureren tijdens het maken van een container (verzameling, grafiek, tabel) of de doorvoer van de update voor een bestaande container.  
4. Als doorvoer toewijzen tijdens het maken van een container, opent u de **Data Explorer** blade en selecteer **nieuwe verzameling** (nieuwe grafiek, nieuwe tabel voor andere API's)  
5. Het formulier invullen **verzameling toevoegen** blade. Velden in deze blade worden in de volgende tabel beschreven:  

   |**Instelling**  |**Beschrijving**  |
   |---------|---------|
   |Database-id  |  Geef een unieke naam voor uw database. De database is een logische container voor een of meer verzamelingen. Databasenamen moeten tussen de 1 en 255 tekens zijn en mogen geen /, \\, # of ? bevatten en mogen niet eindigen met een spatie. |
   |Verzamelings-id  | Geef een unieke naam voor uw verzameling. Voor id’s van verzamelingen gelden dezelfde tekenvereisten als voor databasenamen. |
   |Opslagcapaciteit   | Deze waarde vertegenwoordigt de capaciteit van de database. Bij het inrichten van doorvoer voor een verzameling afzonderlijke opslagcapaciteit kan worden **vast (10 GB)** of **onbeperkt**. Onbeperkte opslagcapaciteit moet worden ingesteld van een partitiesleutel voor uw gegevens.  |
   |Doorvoer   | Elke verzameling en de database zijn doorvoer in aanvraageenheden per seconde.  Voor vaste opslagcapaciteit, minimale doorvoer is 400 aanvraageenheden per seconde (RU/s), voor onbeperkte opslag capaciteit, minimale doorvoer is ingesteld op 1000 RU/s.|

6. Nadat u waarden voor deze velden opgeven, selecteert u **OK** de instellingen op te slaan.  

   ![Set-doorvoer voor een verzameling](./media/set-throughput/set-throughput-for-container.png)

7. Vouw de database en de container voor het bijwerken van de doorvoer voor een bestaande container, en klik vervolgens op **instellingen**. Typ in het nieuwe venster de nieuwe waarde voor de doorvoer en selecteer vervolgens **opslaan**.  

   ![Doorvoer voor een verzameling bijwerken](./media/set-throughput/update-throughput-for-container.png)

### <a name="provision-throughput-for-a-set-of-containers-or-at-the-database-level"></a>Doorvoer inrichten voor een set van containers of op het databaseniveau

1. Meld u aan bij [Azure Portal](https://portal.azure.com).  
2. Selecteer in de Linkernavigatie **alle resources** en zoek uw account voor Azure Cosmos DB.  
3. U kunt de doorvoer configureren tijdens het maken van de doorvoer van een database of de update voor een bestaande database.  
4. Als doorvoer tijdens het maken van een database toewijzen, opent u de **Data Explorer** blade en selecteer **nieuwe Database**  
5. Vul de **Database-id** waarde, Controleer **inrichten doorvoer** optie en doorvoercapaciteit configureren. Een database kan worden ingericht met minimale doorvoercapaciteit 50.000 RU/s.  

   ![Doorvoer met nieuwe databaseoptie instellen](./media/set-throughput/set-throughput-with-new-database-option.png)

6. Vouw de database en de container voor het bijwerken van de doorvoer voor een bestaande database en klik vervolgens op **Scale**. Typ in het nieuwe venster de nieuwe waarde voor de doorvoer en selecteer vervolgens **opslaan**.  

   ![Doorvoer voor een database bijwerken](./media/set-throughput/update-throughput-for-database.png)

### <a name="provision-throughput-for-a-set-of-containers-as-well-as-for-an-individual-container-in-a-database"></a>Doorvoer inrichten voor een set van containers ook als voor een afzonderlijke-container in een database

1. Meld u aan bij [Azure Portal](https://portal.azure.com).  
2. Selecteer in de Linkernavigatie **alle resources** en zoek uw account voor Azure Cosmos DB.  
3. Een database maken en toewijzen van doorvoer toe. Open de **Data Explorer** blade en selecteer **nieuwe Database**  
4. Vul de **Database-id** waarde, Controleer **inrichten doorvoer** optie en doorvoercapaciteit configureren. Een database kan worden ingericht met minimale doorvoercapaciteit 50.000 RU/s.  

   ![Doorvoer met nieuwe databaseoptie instellen](./media/set-throughput/set-throughput-with-new-database-option.png)

5. Maak een verzameling in de database die u hebt gemaakt in bovenstaande stap. Voor het maken van een verzameling met de rechtermuisknop op de database en selecteer **nieuwe verzameling**.  

6. In de **verzameling toevoegen** blade, voer een naam voor de verzameling en partitie-sleutel. Desgewenst kunt u de doorvoer voor die specifieke container inrichten als u ervoor kiest geen waarde toe te wijzen een doorvoer, de doorvoer die is toegewezen aan de database wordt gedeeld met de verzameling.  

   ![Doorvoer desgewenst instellen voor de container](./media/set-throughput/optionally-set-throughput-for-the-container.png)

## <a name="considerations-when-provisioning-throughput"></a>Overwegingen bij het inrichten van doorvoer

Hieronder vindt u enkele overwegingen die u helpen beslissen over de strategie voor een doorvoer-reservering.

Houd rekening met het inrichten van doorvoer op databaseniveau (dat wil zeggen voor set van containers) in de volgende gevallen:

* Als u meer dan tien of meer aantal containers die doorvoer in enkele of alle mappen delen kan.  

* Wanneer u migreert vanaf een één-tenant-database die is ontworpen om te worden uitgevoerd op IaaS gehoste virtuele machines of on-premises (voor bijvoorbeeld NoSQL of relationele databases) in Azure Cosmos DB en veel containers hebben.  

* Als u rekening houden met niet-geplande pieken in werkbelastingen met behulp van de gegroepeerde doorvoer op databaseniveau.  

* In plaats van de doorvoer van de instelling op een afzonderlijke-container bent u geïnteresseerd in de geaggregeerde doorvoer van een set containers in de database ophalen.

Overweeg het inrichten van doorvoer op een afzonderlijke-container in de volgende gevallen:

* Als u een kleiner aantal Azure Cosmos DB containers hebt.  

* Als u de gegarandeerde doorvoer ophalen op een bepaalde container SLA voor back-up wilt.

## <a name="throughput-ranges"></a>Doorvoer bereiken

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

<a id="set-throughput-sdk"></a>

## <a name="set-throughput-by-using-sql-api-for-net"></a>Doorvoer ingesteld met behulp van SQL-API voor .NET

Hier volgt een codefragment voor het maken van een container met 3000 aanvraageenheden per seconde voor een afzonderlijke-container met de SQL-API .NET SDK:

```csharp
DocumentCollection myCollection = new DocumentCollection();
myCollection.Id = "coll";
myCollection.PartitionKey.Paths.Add("/deviceId");

await client.CreateDocumentCollectionAsync(
    UriFactory.CreateDatabaseUri("db"),
    myCollection,
    new RequestOptions { OfferThroughput = 3000 });
```

Hier volgt een codefragment voor inrichting 100.000 aanvraageenheden per seconde over een set van containers met behulp van de SQL-API .NET SDK:

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

Azure Cosmos-database is van invloed op een model reservering voor de doorvoer. Dat wil zeggen, u wordt gefactureerd voor de hoeveelheid doorvoer *gereserveerde*, ongeacht hoeveel waarop doorvoer is actief *gebruikt*. Als uw toepassing de belasting, gegevens en gebruiksgegevens patronen wijzigen kunt u eenvoudig de schaal omhoog en omlaag op het aantal gereserveerde RUs via SDK's of met behulp van de [Azure Portal](https://portal.azure.com).

Elke container of set van containers, is toegewezen aan een `Offer` resource in Azure Cosmos DB met metagegevens over de ingerichte doorvoer. U kunt de toegewezen doorvoer wijzigen door op de overeenkomende resource in de aanbieding voor een container te zoeken en vervolgens bijgewerkt met de nieuwe waarde voor de doorvoer. Hier volgt een codefragment voor het wijzigen van de doorvoer van een container in 5000 aanvraageenheden per tweede met de .NET SDK:

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

Er zijn geen gevolgen voor de beschikbaarheid van de container of set van containers, wanneer u de doorvoer wijzigt. De nieuwe gereserveerde doorvoer is doorgaans effectieve binnen enkele seconden op de toepassing van de nieuwe doorvoer.

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

Bijvoorbeeld, in de Mongo-shell de bewerking niet uitvoeren die u wilt controleren of de kosten van de aanvraag voor.
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

Een methode voor het schatten van de hoeveelheid gereserveerde doorvoer vereist door uw toepassing is voor het vastleggen van de aanvraag eenheid kosten die zijn gekoppeld aan met het normale bewerkingen uitvoeren in een representatieve item gebruikt door de toepassing en schatting maken van het aantal bewerkingen die u verwacht te uitvoeren per seconde.

> [!NOTE]
> Als er itemtypen die aanzienlijk in termen van de grootte en het aantal geïndexeerde eigenschappen verschillen wordt, en noteer vervolgens de betreffende bewerking aanvraag eenheid kosten die zijn gekoppeld aan elk *type* van typische item.
> 
> 

## <a name="get-throughput-by-using-mongodb-api-portal-metrics"></a>Doorvoer verkrijgen door middel van MongoDB API portal metrische gegevens

De eenvoudigste manier om een goede indicatie van aanvraag ophalen eenheid kosten voor de MongoDB-API-database is het gebruik van de [Azure-portal](https://portal.azure.com) metrische gegevens. Met de *aantal aanvragen* en *aanvraag kosten* grafieken, kunt u een schatting van het aantal aanvraageenheden elke bewerking verbruikt en het aantal aanvraageenheden die ze gebruiken ten opzichte van elkaar ophalen.

![MongoDB API portal metrische gegevens][1]

### <a id="RequestRateTooLargeAPIforMongoDB"></a> Overschrijding van gereserveerde doorvoer grenzen in de MongoDB-API
Toepassingen die groter is dan de ingerichte doorvoer voor een container of een set van containers zijn beperkt in de frequentie waarmee totdat het verbruiksniveau onder de snelheid van de ingerichte doorvoer zakt. Wanneer een snelheid-beperking optreedt, de back-end beëindigd wanneer de aanvraag met een `16500` foutcode - `Too Many Requests`. Standaard de MongoDB-API wordt automatisch opnieuw geprobeerd maximaal 10 keer voordat er een `Too Many Requests` foutcode. Als er veel `Too Many Requests` foutcodes, kunt u overwegen een toe te voegen aan een Pogingslogica routines voor foutafhandeling van uw toepassing of [verhogen ingerichte doorvoer voor de container](set-throughput.md).

## <a name="throughput-faq"></a>Veelgestelde vragen over doorvoer

**Kan ik mijn doorvoer ingesteld op minder dan 400 RU/s**

400 RU/s is de minimale doorvoer voor Cosmos DB één partitie containers (het minimum voor gepartitioneerde containers 1000 RU/s is). Aanvraag eenheden worden ingesteld met intervallen van 100 RU/s, maar doorvoer kan niet worden ingesteld op 100 RU/s of een willekeurige waarde kleiner is dan 400 RU/s. Als u naar een voordelige methode zoekt te ontwikkelen en testen van de Cosmos-DB, kunt u de gratis [Azure Cosmos DB Emulator](local-emulator.md), die u lokaal kunt implementeren zonder kosten. 

**Hoe stel ik doorvoer met de MongoDB-API**

Er is geen extensie MongoDB-API om in te stellen doorvoer. De aanbeveling is het gebruik van de SQL-API, zoals wordt weergegeven in [de doorvoer instellen met behulp van de SQL-API voor .NET](#set-throughput-sdk).

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over het schatten van de doorvoer en aanvraag eenheden, [aanvragen eenheden & schatten van doorvoer in Azure Cosmos-DB](request-units.md)

* Zie voor meer informatie over het inrichten en gaat wereld-schaal met Cosmos DB, [partitionering en schalen met Cosmos DB](partition-data.md).

[1]: ./media/set-throughput/api-for-mongodb-metrics.png
