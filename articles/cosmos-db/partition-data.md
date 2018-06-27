---
title: Partitionering en horizontaal schalen in Azure Cosmos DB | Microsoft Docs
description: Meer informatie over hoe partitionering werkt in Azure Cosmos DB, partitie-sleutels en configureren met het partitioneren en het kiezen van de juiste partitiesleutel voor uw toepassing.
services: cosmos-db
author: rimman
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 05/07/2018
ms.author: rimman
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 046e45978d401e05d0ab8154aff994052f5d7717
ms.sourcegitcommit: 0408c7d1b6dd7ffd376a2241936167cc95cfe10f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/26/2018
ms.locfileid: "36960369"
---
# <a name="partition-and-scale-in-azure-cosmos-db"></a>Partitie en schalen in Azure Cosmos-DB

[Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) is een globaal gedistribueerd, meerdere model-database-service waarmee u snel, voorspelbare prestaties bereiken. Het schalen naadloos samen met uw toepassing wanneer het groeit. Dit artikel bevat een overzicht van hoe werkt voor alle gegevens partitioneren in Azure Cosmos DB modellen. Ook wordt beschreven hoe u Azure DB die Cosmos-containers wilt effectief schalen uw toepassingen kunt configureren.

Partitionering en partitiesleutels worden in deze video besproken:

> [!VIDEO https://www.youtube.com/embed/SS6WrQ-HJ30]
> 

## <a name="partitioning-in-azure-cosmos-db"></a>Partitioneren in Azure Cosmos DB
U kunt in Azure Cosmos DB, opslaan en schema-minder gegevens opvragen met een cijfer milliseconden wachttijd op elke schaal. Azure Cosmos DB containers biedt voor het opslaan van gegevens aangeroepen *verzamelingen* (voor documenten) *grafieken*, of *tabellen*. 

Containers zijn logische resources en kunnen een of meer fysieke partities of servers omvatten. Het aantal partities wordt bepaald door Azure Cosmos DB op basis van de opslaggrootte en ingerichte voor een container of een set van containers doorvoer. 

Een *fysieke* partitie is een vast bedrag van gereserveerde SSD back-opslag in combinatie met de variabele hoeveelheid rekenresources (CPU en geheugen). Elke fysieke partitie worden gerepliceerd voor hoge beschikbaarheid. Elke reeks containers kan een of meer fysieke partities delen. Fysieke partitie management volledig wordt beheerd door Azure Cosmos DB en u hoeft te complexe code schrijven of beheren van de partities. Azure DB Cosmos-containers zijn onbeperkte in termen van opslag en doorvoer. 

Een *logische* is een partitie binnen een fysieke partitie waarin de gegevens die zijn gekoppeld aan een sleutelwaarde van één partitie. Meerdere logische partities kunnen terechtkomen op dezelfde fysieke partitie. In het volgende diagram wordt in een enkele container drie logische partities heeft. Elke logische partitie bevat de gegevens voor een partitiesleutel LAX AMS en MEL respectievelijk. Alle logische partities LAX AMS en MEL kan niet worden uitgebreid voorbij de maximale logische partitie limiet van 10 GB. 

![Resource partitioneren](./media/introduction/azure-cosmos-db-partitioning.png) 

Wanneer een container voldoet aan de [partitioneren vereisten](#prerequisites), partitioneren is volledig transparant voor uw toepassing. Azure Cosmos DB ondersteunt snelle leesbewerkingen en schrijfbewerkingen, query's, transactionele logica, consistentieniveaus en verfijnd toegangsbeheer via methoden/API's aan een enkele container-resource. De service worden gegevens over fysieke en logische partities verdeeld en doorsturen van queryaanvragen voor de juiste partitie verwerkt. 

## <a name="how-does-partitioning-work"></a>Hoe partitioneren werkt

Hoe partitioneren werkt? Elk item moet hebben een *partitiesleutel* en een *rijsleutel*, dat deze uniek identificeren. De partitiesleutel fungeert als een logische partitie voor uw gegevens en biedt een grens van een natuurlijke Azure Cosmos DB voor gegevens over fysieke partities verdeeld. De gegevens voor één logische partitie moet zich bevinden in een enkele fysieke partitie, maar fysieke partitie management wordt beheerd door Azure Cosmos DB. 

Kort samengevat: dit is hoe partitioneren werkt in Azure Cosmos DB:

* Inrichten van een set van Azure DB die Cosmos-containers met **T** doorvoer RU/s (aantal aanvragen per seconde).
* Achter de schermen, richt Azure Cosmos DB fysieke partities nodig om te fungeren **T** aanvragen per seconde. Als **T** hoger is dan de maximale doorvoer per fysieke partitie **t**, vervolgens Azure Cosmos DB bepalingen **N = T/t** fysieke partities. De waarde van maximale doorvoer per partition(t) wordt geconfigureerd via de Azure DB die Cosmos, wordt deze waarde toegewezen op basis van de totale ingerichte doorvoer en de hardwareconfiguratie gebruikt. 
* Azure Cosmos DB wordt de sleutel ruimte van de partitie sleutel hashes gelijkmatig meerdere de **N** fysieke partities. Dus als het aantal logische partities elke fysieke partitie als host fungeert **1/N** * aantal sleutelwaarden partitie.
* Wanneer een fysieke partitie **p** bereikt de opslaglimiet bereikt, Azure Cosmos DB naadloos splitst **p** in twee nieuwe fysieke partities **p1** en **p2**. Deze distribueert waarden die overeenkomen met ongeveer de helft van de sleutels aan elk van de nieuwe fysieke partities. Deze bewerking gesplitste is volledig onzichtbaar voor uw toepassing. De splitsbewerking wordt niet uitgevoerd als een fysieke partitie de opslaglimiet bereikt en alle gegevens in de fysieke partitie hoort bij dezelfde logische partitiesleutel. Dit komt doordat de gegevens voor een sleutel die één logische partitie zich op dezelfde fysieke partitie bevinden moet. In dit geval moet de strategie voor een andere partitie worden gebruikt.
* Wanneer u de doorvoer die hoger is dan inrichten **t * N**, Azure Cosmos DB splitst een of meer van uw fysieke partities ter ondersteuning van de hogere doorvoer.

De semantiek voor partitiesleutels zijn enigszins verschillen overeenkomen met de semantiek van elke API, zoals wordt weergegeven in de volgende tabel:

| API | Partitiesleutel | Rijsleutel |
| --- | --- | --- |
| SQL | Aangepaste partitie sleutelpad | Probleem met `id` opgelost | 
| MongoDB | Aangepaste shard-sleutel  | Probleem met `_id` opgelost | 
| Gremlin | Aangepaste partitie sleuteleigenschap | Probleem met `id` opgelost | 
| Tabel | Probleem met `PartitionKey` opgelost | Probleem met `RowKey` opgelost | 

Azure Cosmos DB gebruikt op basis van de hash partitionering. Bij het schrijven van een item wordt Azure Cosmos DB de waarde voor de partitiesleutel-hashes en gebruikt het hash-resultaat om te bepalen welke partitie voor het opslaan van het item in. Azure Cosmos DB slaat alle items met dezelfde partitiesleutel op dezelfde fysieke partitie. 

## <a name="best-practices-when-choosing-a-partition-key"></a>Aanbevolen procedures bij het kiezen van een partitiesleutel

De keuze van de partitiesleutel is een belangrijke beslissing die u moet aanbrengen in de ontwerpfase. Kies een eigenschapsnaam die een breed scala aan waarden en zelfs toegangspatronen is. Het is een best practice om een partitiesleutel met een groot aantal afzonderlijke waarden (bijvoorbeeld honderden of duizenden). Hiermee kunt u uw werkbelasting gelijkmatig verdelen over deze waarden. Een ideaal partitiesleutel is een die vaak verschijnt als een filter in uw query's en heeft voldoende kardinaliteit om te controleren of dat uw oplossing schaalbaar is.

Als een fysieke partitie heeft de opslaglimiet bereikt en de gegevens in de partitie dezelfde partitiesleutel heeft, Azure Cosmos DB retourneert de *"partitiesleutel bereikt maximale grootte van 10 GB"* bericht en de partitie niet gesplitst. Het kiezen van een goede partitiesleutel is een zeer belangrijke beslissing. Fysieke partities zijn van een interne concept van Azure DB die Cosmos en tijdelijke zijn. Het aantal fysieke partities op basis van uw werkbelasting wordt automatisch schalen door Azure Cosmos-DB. Zodat u het ontwerp van de database op basis van het aantal fysieke partities mag niet corelate moet in plaats daarvan u ervoor kiezen de juiste partitiesleutel (logische partities). 

Kies een partitiesleutel zodat:

* De opslag-distributie is zelfs in alle sleutels.
* De distributie van het volume van aanvragen op een bepaald tijdstip is zelfs voor alle sleutels.
* Query's die worden aangeroepen met hoge gelijktijdigheid kunnen efficiënt worden gerouteerd door te nemen van de partitiesleutel in het filterpredicaat.  
* Het kiezen van een partitiesleutel met een hogere kardinaliteit is doorgaans voorkeur – becaue deze doorgaans resulteert in betere distributie en schaalbaarheid. Een samengestelde sleutel kan bijvoorbeeld worden gevormd door waarden van meerdere eigenschappen voor het verhogen van de kardinaliteit samen te voegen. 

Als u ervoor kiest een partitiesleutel met bovenstaande overwegingen, u hoeft te hoeven maken over het aantal partities of hoeveel doorvoer per fysieke partitie is toegewezen tijdens Azure Cosmos DB het aantal fysieke partities schalen kunnen ook worden geschaald de afzonderlijke partities waar nodig.

Azure DB Cosmos-containers kunnen worden gemaakt als *vaste* of *onbeperkte* in de Azure portal. Containers met vaste grootte hebben een maximale limiet van 10 GB en doorvoer van 10.000 RU/s. U moet een partitiesleutel en een minimale doorvoer van 1000 RU/s opgeven voor het maken van een container als onbeperkt. Azure DB Cosmos-containers kunnen ook worden geconfigureerd als u wilt delen doorvoercapaciteit tussen een set van containers, waarbij elke container nader moet een partitie van belangrijke en onbeperkt kan groeien.

Er is een goed idee om te controleren hoe uw gegevens is verdeeld over de partities. Ga naar uw Azure DB die Cosmos-account en klikt u op te controleren of de gegevensdistributie in de portal, **metrische gegevens** in **bewaking** sectie en klik vervolgens op **opslag** tabblad om te zien hoe uw gegevens zijn over de verschillende fysieke partities gepartitioneerd.

![Resource partitioneren](./media/partition-data/partitionkey-example.png)

De linker afbeelding hierboven toont het resultaat van een ongeldige partitiesleutel en de juiste afbeelding hierboven toont het resultaat wanneer een goede partitiesleutel is gekozen. In de afbeelding links ziet u dat de gegevens niet gelijkmatig wordt verdeeld tussen de partities. U moet een partitiesleutel die uw gegevens distribueert, zodat deze op de juiste installatiekopie lijkt kiezen streven.

<a name="prerequisites"></a>
## <a name="prerequisites-for-partitioning"></a>Vereisten voor het partitioneren

Voor fysieke partities voor automatisch in te splitsen **p1** en **p2** zoals beschreven in [hoe werkt partitionering](#how-does-partitioning-work), de container moet worden gemaakt met een doorvoersnelheid van 1000 RU/s of meer (of share doorvoer van een set van containers) en een partitiesleutel moet worden opgegeven. Bij het maken van een container (bijvoorbeeld een verzameling, een grafiek of een tabel) in de Azure portal, selecteer de **onbeperkt** opslagoptie de capaciteit om te profiteren van onbeperkt schalen. 

Als u een container in Azure portal of programmatisch gemaakt en de initiële doorvoer 1000 RU/s of meer is en u hebt opgegeven een partitiesleutel, kunt u profiteren van onbeperkt schalen zonder wijzigingen in de container. Dit omvat **vast** containers, zo lang als de eerste container is gemaakt met ten minste 1000 RU/s doorvoer en een partitiesleutel is opgegeven.

Alle containers die zijn geconfigureerd voor het delen van doorvoer als onderdeel van een set van containers worden behandeld als **onbeperkt** containers.

Als u hebt gemaakt een **vast** container met geen partitiesleutel of doorvoer minder dan 1000 RU/s, de container wordt niet automatisch geschaald zoals beschreven in dit artikel. Als u wilt de gegevens migreren vanaf een vaste container aan een onbeperkte container (bijvoorbeeld een met ten minste 1000 RU/s en een partitiesleutel), moet u de [hulpprogramma voor gegevensmigratie](import-data.md) of de [wijziging gegevensfeedbibliotheek](change-feed.md). 

## <a name="partitioning-and-provisioned-throughput"></a>Partitionering en ingerichte doorvoer
Azure Cosmos DB is ontworpen voor voorspelbare prestaties. Wanneer u een container of een set van containers maakt, u de doorvoer in termen van reserveren  *[Aanvraageenheden](request-units.md) (RU) per seconde*. Elke aanvraag maakt een RU kosten die evenredig is met de hoeveelheid systeembronnen, zoals CPU, geheugen en i/o verbruikt door de bewerking. Het lezen van een document 1 KB met sessieconsistentie verbruikt 1 RU. Een leesbewerking is 1 RU ongeacht het aantal items die zijn opgeslagen of het aantal gelijktijdige aanvragen die actief zijn op hetzelfde moment. Grotere items moet hoger RUs afhankelijk van de grootte. Als u de grootte van de entiteiten en het aantal leesbewerkingen die u wilt ondersteunen voor uw toepassing weet, kunt u de exacte hoeveelheid doorvoer die is vereist voor de behoeften van uw toepassing inrichten. 

> [!NOTE]
> Gebruik voor een container of een set van containers ingerichte doorvoer, moet u een partitiesleutel waarmee u aanvragen evenredig over alle afzonderlijke partitie sleutelwaarden verdelen.
> 
> 

<a name="designing-for-partitioning"></a>
## <a name="work-with-the-azure-cosmos-db-apis"></a>Werken met de Azure Cosmos DB-API 's
U kunt de Azure portal of Azure CLI gebruiken containers maken en op elk gewenst moment worden geschaald. Deze sectie wordt beschreven hoe containers maken en de ingerichte doorvoer en partitie sleutel met behulp van elke API opgeven.


### <a name="sql-api"></a>SQL-API
Het volgende voorbeeld laat zien hoe een container (een collectie) met behulp van SQL-API maken. 

```csharp
DocumentClient client = new DocumentClient(new Uri(endpoint), authKey);
await client.CreateDatabaseAsync(new Database { Id = "db" });

DocumentCollection myCollection = new DocumentCollection();
myCollection.Id = "coll";
myCollection.PartitionKey.Paths.Add("/deviceId");

await client.CreateDocumentCollectionAsync(
    UriFactory.CreateDatabaseUri("db"),
    myCollection,
    new RequestOptions { OfferThroughput = 20000 });
```

Vindt u een item (document) met behulp van de `GET` methode in de REST API of met behulp van `ReadDocumentAsync` in een van de SDK's.

```csharp
// Read document. Needs the partition key and the ID to be specified
DeviceReading document = await client.ReadDocumentAsync<DeviceReading>(
  UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
  new RequestOptions { PartitionKey = new PartitionKey("XMS-0001") });
```

Zie voor meer informatie [partitioneren in Azure Cosmos-database met behulp van de SQL-API](sql-api-partition-data.md).

### <a name="mongodb-api"></a>MongoDB-API
U kunt een verzameling shard via uw favoriete hulpprogramma, stuurprogramma of SDK maken met de MongoDB-API. In dit voorbeeld gebruiken we de Mongo-Shell om een verzameling te maken.

In de Mongo-Shell:

```
db.runCommand( { shardCollection: "admin.people", key: { region: "hashed" } } )
```
    
Resultaten:

```JSON
{
    "_t" : "ShardCollectionResponse",
    "ok" : 1,
    "collectionsharded" : "admin.people"
}
```

### <a name="table-api"></a>Tabel-API

Een tabel te maken met behulp van de tabel-API, gebruik de `CreateIfNotExists` methode. 

```csharp
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

CloudTable table = tableClient.GetTableReference("people");
table.CreateIfNotExists(throughput: 800);
```

Ingerichte doorvoer is ingesteld als een argument van de `CreateIfNotExists`. De partitiesleutel is impliciet gemaakt als de `PartitionKey` waarde. 

U kunt één entiteit ophalen met behulp van de volgende code:

```csharp
// Create a retrieve operation that takes a customer entity.
TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

// Execute the retrieve operation.
TableResult retrievedResult = table.Execute(retrieveOperation);
```
Zie voor meer informatie [ontwikkelen met de API van de tabel](tutorial-develop-table-dotnet.md).

### <a name="gremlin-api"></a>Gremlin-API

U kunt met de API Gremlin Azure portal of Azure CLI gebruiken voor het maken van een container waarin een grafiek vertegenwoordigt. U kunt ook omdat Azure Cosmos DB met meerdere modellen, kunt u een van de andere API's maken en schalen van uw grafiek container.

U kunt hoekpunt of edge lezen met behulp van de partitiesleutel en -ID in Gremlin. Voor een grafiek met regio ('VS') als de partitiesleutel en "Seattle" Als de rijsleutel, kunt u bijvoorbeeld een hoekpunt vinden met behulp van de volgende syntaxis:

```
g.V(['USA', 'Seattle'])
```

U kunt verwijzen naar een edge met behulp van de partitiesleutel en de rijsleutel.

```
g.E(['USA', 'I5'])
```

Zie voor meer informatie [met behulp van een gepartitioneerde grafiek in Azure Cosmos DB](graph-partitioning.md).


<a name="designing-for-scale"></a>
## <a name="design-for-scale"></a>Ontwerp voor schaal
Als u wilt schalen effectief met Azure Cosmos DB, moet u een goede partitiesleutel kiezen wanneer u de container maken. Er zijn twee belangrijke overwegingen voor het kiezen van een goede partitiesleutel:

* **Query grens en transacties**. Uw keuze van partitiesleutel moet in evenwicht hoeft te transacties de vereiste voor de distributie van de entiteiten over meerdere partitiesleutels om ervoor te zorgen schaalbare oplossing gebruiken. U kunt dezelfde partitiesleutel instellen voor alle objecten op één extreme, maar deze optie kan de schaalbaarheid van uw oplossing beperken. Het andere uiterste, kunt u een unieke partitiesleutel voor elk item toewijzen. Deze keuze is zeer schaalbaar, maar deze voorkomt u dat via de transacties tussen meerder documenten via opgeslagen procedures en triggers. Een ideaal partitiesleutel kunt u gebruiken efficiënt query's en heeft voldoende kardinaliteit om te controleren of dat uw oplossing schaalbaar is. 
* **Er is geen opslag en prestaties knelpunten**. Het is belangrijk om te selecteren van een eigenschap die kan worden verdeeld over verschillende afzonderlijke waarden. Aanvragen voor dezelfde partitiesleutel mag niet langer dan de ingerichte doorvoer die is toegewezen aan een partitie en frequentie beperkt zijn. Het is daarom belangrijk dat u kiest een partitiesleutel die niet in 'hotspots' in uw toepassing resulteren. Omdat de gegevens voor een sleutel die één partitie moet worden opgeslagen in een partitie, moet u vermijden partitiesleutels die grote hoeveelheden gegevens voor dezelfde waarde hebben. 

Bekijk enkele praktijkscenario's en goede partitiesleutels voor elk:
* Als u bij het implementeren van een gebruiker profiel back-end van de *gebruikers-ID* een goede keuze voor een partitiesleutel is.
* Als u bij het opslaan van IoT-gegevens, bijvoorbeeld, status van het apparaat, een *apparaat-ID* een goede keuze voor een partitiesleutel is.
* Als u Azure Cosmos DB voor logboekregistratie timeseries gegevens, de *hostnaam* of *proces-ID* een goede keuze voor een partitiesleutel is.
* Als u een multitenant-architectuur hebt, het *tenant-ID* een goede keuze voor een partitiesleutel is.

Gebruik in sommige gevallen, zoals IoT- en profielen, de partitiesleutel kan niet dezelfde zijn als uw *ID* (documentsleutel). In andere gevallen, zoals de timeseries gegevens, moet u wellicht een partitiesleutel die verschilt van de *ID*.

### <a name="partitioning-and-loggingtime-series-data"></a>Partitionering en logboekregistratie-tijdreeks gegevens
Een van de algemene gebruiksvoorbeelden in Azure Cosmos DB is logboekregistratie en telemetrie. Het is belangrijk een goede partitiesleutel kiest in dit scenario moet u mogelijk grote hoeveelheden gegevens lezen/schrijven. De keuze voor een partitiesleutel, is afhankelijk van uw tarieven lezen en schrijven en de soorten query's die u verwacht te worden uitgevoerd. Hier volgen enkele tips over het kiezen van een goede partitiesleutel:

* Als uw gebruiksvoorbeeld omvat een klein aantal schrijfbewerkingen die gedurende een lange periode worden verzameld en u moet bereiken van tijdstempels query met andere filters, gebruikt u een updatepakket van het tijdstempel. Bijvoorbeeld, is een goede benadering datum als een partitiesleutel gebruikt. Met deze methode kunt u over alle gegevens opvragen voor een bepaalde datum van één partitie. 
* Als uw werkbelasting schrijven-zware, wat heel normaal is in dit scenario, gebruikt u een partitiesleutel die niet is gebaseerd op de timestamp. Als zodanig Azure Cosmos DB kunt distribueren en schalen van schrijfbewerkingen gelijkmatig over verschillende partities. Hier een *hostnaam*, *proces-ID*, *activiteits-ID*, of een andere eigenschap met hoge kardinaliteit is een goede keuze. 
* Een andere manier is een hybride-benadering, waarbij er meerdere containers, één voor elke dag/maand, en de partitiesleutel is een meer gedetailleerd eigenschap als *hostnaam*. Deze methode heeft als voordeel dat u verschillende doorvoer voor elke container of een set van containers op basis van de periode en de behoeften van de schaal en prestaties instellen kunt. Een container voor de huidige maand kan bijvoorbeeld worden ingericht met een hogere doorvoer, omdat deze leest fungeert en schrijft. Vorige maanden kunnen worden ingericht met een lagere doorvoer, omdat ze alleen leesbewerkingen fungeren.

### <a name="partitioning-and-multitenancy"></a>Partitionering en multitenancy
Als u een multitenant-toepassing met behulp van Azure DB die Cosmos implementeert, er zijn twee populaire ontwerpen te overwegen: *een partitiesleutel per tenant* en *één container per tenant*. Hier volgen de voordelen en nadelen voor elk:

* **Een partitiesleutel per tenant**. In dit model zijn tenants binnen een enkele container geplaatst. Query's en voegt voor een enkele tenant kunnen worden uitgevoerd op één partitie. U kunt ook transactionele logica implementeren voor alle items die horen bij een tenant. Omdat meerdere tenants een container delen, kunt u beter gebruikmaken van opslag en ingerichte doorvoer door groeperen van bronnen voor alle tenants binnen een enkele container in plaats van voor elke tenant wordt ingericht. Het nadeel is dat er geen isolatie van de prestaties per tenant. De doorvoer te waarborgen van de prestaties te verhogen geldt voor de volledige container met de tenants versus gerichte toeneemt voor een afzonderlijke tenant.
* **Een container per tenant**. In dit model elke tenant heeft zijn eigen container en kunt u de doorvoer met gegarandeerde prestaties per tenant reserveren. Dit model is rendabeler voor multitenant-toepassingen met een paar tenants.

U kunt ook een hybride-benadering die kleine tenants samen colocates en grotere tenants naar hun eigen containers worden geïsoleerd.

## <a name="next-steps"></a>Volgende stappen
In dit artikel opgegeven we een overzicht van de concepten en aanbevolen procedures voor het schalen en partitioneren in Azure Cosmos DB. 

* Meer informatie over [ingerichte doorvoer in Azure Cosmos DB](request-units.md).
* Meer informatie over [globale distributie in Azure Cosmos DB](distribute-data-globally.md).



