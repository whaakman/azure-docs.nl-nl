---
title: Partitionering en horizontaal schalen in Azure Cosmos DB | Microsoft Docs
description: Meer informatie over hoe partitionering werkt in Azure Cosmos DB, over het configureren van de partitionering en partitioneren van sleutels en hoe de juiste partitiesleutel voor uw toepassing kiest.
services: cosmos-db
author: aliuy
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 07/26/2018
ms.author: andrl
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c9c1379cd457dcba37246985058b01e85633f968
ms.sourcegitcommit: 387d7edd387a478db181ca639db8a8e43d0d75f7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/10/2018
ms.locfileid: "42061321"
---
# <a name="partition-and-scale-in-azure-cosmos-db"></a>Partitioneren en schalen in Azure Cosmos DB

[Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) is een wereldwijd gedistribueerde, multi-model databaseservice die is ontworpen om u te helpen u Behaal snelle en voorspelbare prestaties. Het schalen naadloos samen met uw toepassing. Dit artikel bevat een overzicht van hoe werkt voor alle gegevens partitioneren in Azure Cosmos DB-modellen. Ook wordt beschreven hoe u Azure Cosmos DB-containers om effectief schalen uw toepassingen configureert.

Partitioneren en partitiesleutels worden besproken in deze video:

> [!VIDEO https://www.youtube.com/embed/SS6WrQ-HJ30]
> 

## <a name="partitioning-in-azure-cosmos-db"></a>Partitionering in Azure Cosmos DB
Azure Cosmos DB biedt containers voor het opslaan van gegevens die verzamelingen (voor documenten), grafieken of tabellen genoemd. Containers zijn logische resources en een of meer fysieke partities of servers kunnen omvatten. Het aantal partities wordt bepaald door de Azure Cosmos DB op basis van de opslaggrootte en ingerichte doorvoer voor een container of een set met containers. 


### <a name="physical-partition"></a>Fysieke partitie

Een *fysieke* partitie is een vaste hoeveelheid gereserveerde SSD-opslag gecombineerd met een wisselende hoeveelheid rekenbronnen (CPU en geheugen). Elke fysieke partitie wordt gerepliceerd voor hoge beschikbaarheid. Elke set van containers kan één of meer fysieke partities delen. Beheer van fysieke partitie volledig wordt beheerd door Azure Cosmos DB en u hoeft te schrijven van complexe code of beheren van de partities. Azure Cosmos DB-containers zijn onbeperkte in termen van opslag en doorvoer. Fysieke partities zijn een interne concept van Azure Cosmos DB en tijdelijke. Azure Cosmos DB wordt automatisch geschaald voor het aantal fysieke partities op basis van uw werkbelasting. Zodat u het ontwerp van de database op basis van het aantal fysieke partities mag niet corelate moet in plaats daarvan u ervoor dat u kiest de juiste partitiesleutel waarmee wordt bepaald hoe de logische partities. 

### <a name="logical-partition"></a>Logische partitie

Een *logische* partitie is een partitie binnen een fysieke partitie waarin alle gegevens die zijn gekoppeld aan een sleutelwaarde van één partitie. Meerdere logische partities kunnen terechtkomen op dezelfde fysieke partitie. In het volgende diagram wordt in een enkele container drie logische partities heeft. Elke logische partitie slaat de gegevens voor één partitiesleutel en LAX AMS en MEL respectievelijk. Elk van de LAX AMS en MEL logische partities kan niet groter wordt dan de maximumgrootte van een logische partitie-limiet van 10 GB. 

![Partitioneren van resource](./media/introduction/azure-cosmos-db-partitioning.png) 

Wanneer een container voldoet aan de [partitioneren vereisten](#prerequisites), partitionering is volledig transparant voor uw toepassing. Azure Cosmos DB wordt gebruikt voor het distribueren van gegevens over meerdere fysieke en logische partities en routering van query-aanvragen naar de juiste partitie. 

## <a name="how-does-partitioning-work"></a>Hoe partitioneren werkt

Elk document moet een *partitiesleutel* en een *rijsleutel*, die het uniek identificeren. De partitiesleutel fungeert als een logische partitie voor uw gegevens en biedt Azure Cosmos DB een natuurlijke grens voor het distribueren van gegevens over fysieke partities. **De gegevens voor één logische partitie moet zich in een enkele fysieke partitie en het beheer van fysieke partitie wordt beheerd door Azure Cosmos DB**. 

In het kort werkt als volgt partitionering in Azure Cosmos DB:

* Inrichten van een set met Azure Cosmos DB-containers met **T** doorvoer RU/s (aanvragen per seconde).  
* Achter de schermen, Azure Cosmos DB fysieke partities die nodig zijn voor de service aan bepalingen **T** aanvragen per seconde. Als **T** hoger is dan de maximale doorvoer per fysieke partitie **t**, klikt u vervolgens de Azure Cosmos DB-bepalingen **N T t / =** fysieke partities. De waarde van maximale doorvoer per partition(t) is geconfigureerd door Azure Cosmos DB, wordt deze waarde toegewezen op basis van de totale ingerichte doorvoer en de hardwareconfiguratie gebruikt.  
* Azure Cosmos DB ruimte wordt toegewezen de sleutel van de partitie sleutel hashes evenredig wordt verdeeld over meerdere de **N** fysieke partities. Dus als het aantal logische partities elke fysieke partitie als host fungeert **1/N** * aantal partitiesleutelwaarden die zijn.  
* Wanneer een fysieke partitie **p** bereikt de limiet voor opslag, Azure Cosmos DB probleemloos splitst **p** in twee nieuwe fysieke partities, **p1** en **p2**. Deze verdeelt waarden die overeenkomen met ongeveer de helft van de sleutels voor elk van de nieuwe fysieke partities. Dit split-bewerking is volledig onzichtbaar voor uw toepassing. De splitsbewerking wordt niet uitgevoerd als een fysieke partitie heeft de opslaglimiet bereikt en alle gegevens in de fysieke partitie behoort tot dezelfde logische partitiesleutel. Dit komt doordat de gegevens voor één logische partitiesleutel moet zich in dezelfde fysieke partitie bevinden. In dit geval moet de strategie voor een andere partitie worden gebruikt.  
* Wanneer u de doorvoer die hoger is dan inrichten **t * N**, Azure Cosmos DB splitst een of meer van uw fysieke partities voor de ondersteuning van de hogere doorvoer.

De semantiek voor partitiesleutels zijn enigszins zodat deze overeenkomen met de semantiek van elke API, zoals wordt weergegeven in de volgende tabel:

| API | Partitiesleutel | Rijsleutel |
| --- | --- | --- |
| SQL | Aangepaste partitie sleutelpad | Probleem met `id` opgelost | 
| MongoDB | Aangepaste shard-sleutel  | Probleem met `_id` opgelost | 
| Gremlin | Aangepaste partitie sleuteleigenschap | Probleem met `id` opgelost | 
| Tabel | Probleem met `PartitionKey` opgelost | Probleem met `RowKey` opgelost | 

Azure Cosmos DB maakt gebruik van hash-gebaseerde partitioneren. Wanneer u een item schrijft, wordt Azure Cosmos DB de waarde voor de partitiesleutel hashes en gebruikt het hash-resultaat om te bepalen welke partitie voor het opslaan van het item in. 

> [!NOTE]
> Azure Cosmos DB slaat alle items met dezelfde partitiesleutel in dezelfde fysieke partitie. 

## <a name="best-practices-when-choosing-a-partition-key"></a>Aanbevolen procedures bij het kiezen van een partitiesleutel

De keuze van de partitiesleutel is een belangrijke beslissing die u moet maken tijdens de ontwerpfase. Kies de naam van een eigenschap die heeft een breed scala aan waarden en zelfs toegangspatronen. Het is een aanbevolen procedure om een partitiesleutel met een groot aantal verschillende waarden (bijvoorbeeld, honderden of duizenden). Hiermee kunt u uw workload gelijkmatig verdelen over deze waarden. Een ideale partitiesleutel is een die vaak worden weergegeven als een filter in uw query's en heeft voldoende kardinaliteit om te controleren of dat uw oplossing is schaalbaar.

Als een fysieke partitie heeft de opslaglimiet bereikt en de gegevens in de partitie dezelfde partitiesleutel heeft, Azure Cosmos DB retourneert de *"partitiesleutel bereikt de maximale grootte van 10 GB"* bericht en de partitie niet wordt gesplitst. Een goede partitiesleutel kiezen is een zeer belangrijke beslissing. 

Een partitiesleutel kiezen zodat:

* De distributie van de opslag is zelfs voor alle sleutels.  
* De distributie van het volume van aanvragen op een bepaald moment in-time is zelfs voor alle sleutels.  

  Er is een goed idee om te controleren hoe uw gegevens worden gedistribueerd over meerdere partities. Om te controleren of de verdeling van gegevens in de portal, gaat u naar uw Azure Cosmos DB-account en klik op **metrische gegevens** in **bewaking** sectie en klik vervolgens op **opslag** tabblad om te zien hoe uw gegevens zijn gepartitioneerd over verschillende fysieke partities.

  ![Partitioneren van resource](./media/partition-data/partitionkey-example.png)

  De bovenstaande afbeelding links toont het resultaat van een ongeldige partitiesleutel en de juiste bovenstaande afbeelding toont het resultaat wanneer een goede partitiesleutels is gekozen. In de afbeelding links ziet u dat de gegevens niet gelijkmatig wordt verdeeld over de partities. U moet streven naar een partitiesleutel die uw gegevens worden verdeeld, zodat deze op de juiste installatiekopie lijkt kiezen.

* Query's die worden aangeroepen met hoge gelijktijdigheid kunnen door de partitiesleutel te nemen in het filterpredicaat efficiënt worden gerouteerd.  
* Een partitiesleutel kiezen met een hogere kardinaliteit is in het algemeen aanbevolen – becaue deze doorgaans resulteert in betere distributie en schaalbaarheid. Bijvoorbeeld, kan een synthetische sleutel worden gevormd door het samenvoegen van waarden uit meerdere eigenschappen om te verhogen van de kardinaliteit van de.  

Als u ervoor kiest een partitiesleutel met bovenstaande overwegingen, u geen zorgen te hoeven maken over het aantal partities of hoeveel doorvoer is toegewezen per fysieke partitie, zoals Azure Cosmos DB is geschaald van het aantal fysieke partities kunnen ook worden geschaald de afzonderlijke partities indien nodig.

<a name="prerequisites"></a>
## <a name="prerequisites-for-partitioning"></a>Vereisten voor het partitioneren van

Azure Cosmos DB-containers kunnen worden gemaakt als vast of onbeperkt in de Azure-portal. Containers met vaste grootte hebben een maximale limiet van 10 GB en doorvoer van 10.000 RU/s. Voor het maken van een container als onbeperkte, moet u een partitiesleutel en een minimale doorvoer van 1000 RU/s. Azure Cosmos DB-containers kunnen ook worden geconfigureerd voor het delen van doorvoer tussen een set van containers, waarbij elke container nader moet een partitie sleutel en onbeperkt kan groeien. Hier volgen de vereisten om te overwegen voor het partitioneren en schalen:

* Bij het maken van een container (bijvoorbeeld een verzameling, een grafiek of een tabel) in Azure portal, selecteert u de **onbeperkt** opslagoptie van capaciteit om te profiteren van onbeperkt schalen. Voor de fysieke partities automatisch in te splitsen **p1** en **p2** zoals beschreven in [hoe partitionering werkt](#how-does-partitioning-work), de container moet worden gemaakt met een doorvoer van 1000 RU/s of meer (of doorvoer van de share op een set met containers), en de partitiesleutel van een moet worden opgegeven. 

* Als u een container in Azure portal of via een programma gemaakt en de eerste doorvoer 1000 RU/s of meer is, en u hebt opgegeven een partitiesleutel, kunt u profiteren van onbeperkt schalen zonder wijzigingen in de container. Dit omvat **vaste** containers, mits ze de eerste container is gemaakt met ten minste 1000 RU/s aan doorvoer en een partitiesleutel is opgegeven.

* Alle containers die zijn geconfigureerd voor het delen van doorvoer als onderdeel van een set van containers worden behandeld als **onbeperkt** containers.

Als u hebt gemaakt een **vaste** container zonder dat er partitie sleutel of doorvoer minder dan 1000 RU/s, de container wordt niet automatisch kan worden geschaald. Als u wilt de gegevens van een vaste container migreren naar een onbeperkte container, die u wilt gebruiken de [hulpprogramma voor gegevensmigratie](import-data.md) of de [Change Feed bibliotheek](change-feed.md). 

## <a name="partitioning-and-provisioned-throughput"></a>Partitionering en ingerichte doorvoer
Azure Cosmos DB is ontworpen voor voorspelbare prestaties. Wanneer u een container of een set van containers maakt, u de doorvoer in termen van reserveren  *[Aanvraageenheden](request-units.md) (RU) per seconde*. Elke aanvraag maakt een RU kosten in rekening gebracht die is evenredig aan de hoeveelheid systeembronnen, zoals CPU, geheugen en i/o-gebruikt door de bewerking. Lezen van een document van 1 KB met sessieconsistentie verbruikt 1 RU. Een leesbewerking is 1 RU, ongeacht het aantal items die zijn opgeslagen of het aantal gelijktijdige aanvragen die actief zijn op hetzelfde moment. Grotere items moet hoger ru's, afhankelijk van de grootte. Als u de grootte van uw entiteiten en het aantal leesbewerkingen die u wilt ondersteunen voor uw toepassing weet, kunt u de exacte hoeveelheid doorvoer die is vereist voor de behoeften van uw toepassing kunt inrichten. 

> [!NOTE]
> Volledig gebruik van ingerichte doorvoer voor een container of een set van containers, moet u een partitiesleutel waarmee u aanvragen gelijkmatig wordt verdeeld over alle afzonderlijke partitie-sleutelwaarden.
> 
> 

<a name="designing-for-partitioning"></a>
## <a name="create-partition-key"></a>Partitiesleutel maken 
U kunt de Azure portal of Azure CLI containers te maken en ze op elk gewenst moment te schalen. Deze sectie wordt beschreven hoe u containers maken en de ingerichte doorvoer en partitie sleutel met behulp van elke API opgeven.


### <a name="sql-api"></a>SQL-API
Het volgende voorbeeld toont het maken van een container (verzameling) met behulp van SQL-API. 

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

Vindt u een item (document) met behulp van de `GET` methode in de REST-API of met behulp van `ReadDocumentAsync` in een van de SDK's.

```csharp
// Read document. Needs the partition key and the ID to be specified
DeviceReading document = await client.ReadDocumentAsync<DeviceReading>(
  UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
  new RequestOptions { PartitionKey = new PartitionKey("XMS-0001") });
```

Zie voor meer informatie, [partitionering in Azure Cosmos DB met behulp van de SQL-API](sql-api-partition-data.md).

### <a name="mongodb-api"></a>MongoDB-API
U kunt een verzameling met sharding van via uw favoriete hulpprogramma, stuurprogramma of SDK kunt maken met de MongoDB-API. In dit voorbeeld gebruiken we de Mongo-Shell om een verzameling te maken.

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

Gebruik voor het maken van een tabel met behulp van de Table-API de `CreateIfNotExists` methode. 

```csharp
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

CloudTable table = tableClient.GetTableReference("people");
table.CreateIfNotExists(throughput: 800);
```

Ingerichte doorvoer is ingesteld als een argument van `CreateIfNotExists`. De partitiesleutel is impliciet gemaakt als de `PartitionKey` waarde. 

U kunt één entiteit ophalen met behulp van de volgende code:

```csharp
// Create a retrieve operation that takes a customer entity.
TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

// Execute the retrieve operation.
TableResult retrievedResult = table.Execute(retrieveOperation);
```
Zie voor meer informatie, [ontwikkelen met de Table-API](tutorial-develop-table-dotnet.md).

### <a name="gremlin-api"></a>Gremlin-API

Met de Gremlin-API, kunt u de Azure portal of Azure CLI om een container die staat voor een grafiek maken. U kunt ook omdat Azure Cosmos DB multi-modeldatabase, kunt u een van de andere API's te maken en schalen van de graph-container.

U kunt een hoekpunt of edge lezen met behulp van de partitiesleutel en -ID in Gremlin. Voor een grafiek met de regio ("VS") als de partitiesleutel en "Seattle" Als de recordsleutel, kunt u bijvoorbeeld een hoekpunt vinden met behulp van de volgende syntaxis:

```
g.V(['USA', 'Seattle'])
```

U kunt verwijzen naar een rand met behulp van de partitiesleutel en de rijsleutel.

```
g.E(['USA', 'I5'])
```

Zie voor meer informatie, [in Azure Cosmos DB met behulp van een gepartitioneerde grafiek](graph-partitioning.md).

## <a name="form-partition-key-by-concatenating-multiple-fields"></a>De partitiesleutel formulier door het samenvoegen van meerdere velden

U kunt ook een partitiesleutel vormen door samenvoegen en opvulling van meerdere waarden van eigenschappen in een enkele kunstmatige 'partitionKey'-eigenschap van het item. Deze sleutels worden aangeduid als synthetische sleutels.

U hebt bijvoorbeeld een document dat lijkt op:

```json
{
"deviceId": "abc-123",
"date": 2018
}
```

Een optie is om in te stellen partitionKey op/DeviceID of /date. Als wilt vormen een partitiesleutel op apparaat-id en datum. Samenvoegen van deze twee waarden in voor een eigenschap kunstmatige 'partitionKey' en de partitiesleutel instellen op /partitionKey.

```json
{
"deviceId": "abc-123",
"date": 2018,
"partitionKey": "abc-123-2018"
}
```

U kunt in realtime scenario's duizenden documenten hebben zodat u logica op de client voor het samenvoegen van waarden in een synthetische sleutel, de synthetische sleutel invoegen in de documenten en vervolgens worden gebruikt om op te geven van de partitiesleutel te definiëren.

<a name="designing-for-scale"></a>
## <a name="design-for-scale"></a>Ontwerpen voor schalen
Als u wilt effectief schalen met Azure Cosmos DB, moet u een goede partitiesleutel kiezen bij het maken van de container. Er zijn twee belangrijke overwegingen voor het kiezen van een goede partitiesleutels:

* **Query uitvoeren op grens en transacties**. Uw keuze van de partitiesleutel moet de noodzaak om transacties te gebruiken op basis van de behoefte aan uw entiteiten verdelen over meerdere partitiesleutels om te controleren of een schaalbare oplossing in balans brengen. U kunt dezelfde partitiesleutel instellen voor alle items op een extreme, maar deze optie kan de schaalbaarheid van uw oplossing beperken. Op de andere extreme, kunt u een unieke partitiesleutel voor elk item toewijzen. Deze keuze is zeer schaalbaar, maar voorkomt dat u met behulp van de transacties tussen meerder documenten via opgeslagen procedures en triggers. Een ideale partitiesleutel kunt u efficiënt query's en heeft voldoende kardinaliteit om te controleren of dat uw oplossing is schaalbaar. 
* **Er is geen opslag en prestaties knelpunten**. Het is belangrijk om een eigenschap waarmee schrijfbewerkingen wordt verdeeld over verschillende afzonderlijke waarden te kiezen. Aanvragen voor dezelfde partitiesleutel niet langer zijn dan de ingerichte doorvoer is toegewezen aan een partitie en tarief-beperkt is. Het is dus belangrijk om te kiezen van een partitiesleutel die niet in 'hotspots"in uw toepassing resulteren. Omdat alle gegevens voor een enkele partitiesleutel moet binnen een partitie worden opgeslagen, dient u partitiesleutels waarvoor grote hoeveelheden gegevens voor dezelfde waarde te vermijden. 

Bekijk een aantal praktijkscenario's en goede partitiesleutels voor elk:
* Als u bij het implementeren van een profiel gebruiker back-end de *gebruikers-ID* is een goede keuze voor een partitiesleutel.
* Als u bij het opslaan van IoT-gegevens, bijvoorbeeld, status van het apparaat, een *apparaat-ID* is een goede keuze voor een partitiesleutel.
* Als u Azure Cosmos DB gebruikt voor het vastleggen van time series-gegevens, de *hostnaam* of *proces-ID* is een goede keuze voor een partitiesleutel.
* Als u een architectuur met meerdere tenants hebt de *tenant-ID* is een goede keuze voor een partitiesleutel.

Gebruik in sommige gevallen, zoals IoT- en -profielen, de partitiesleutel mogelijk niet hetzelfde zijn als uw *ID* (documentsleutel). In andere gevallen, zoals de time series-gegevens, mogelijk hebt u een partitiesleutel die verschilt van de *ID*.

### <a name="partitioning-and-loggingtime-series-data"></a>Partitionering en logboekregistratie/time series-gegevens
Een van de algemene scenario's in Azure Cosmos DB is logboekregistratie en telemetrie. Het is belangrijk dat een goede partitiesleutel kiezen in dit scenario, omdat u mogelijk grote hoeveelheden gegevens lezen/schrijven. De keuze voor een partitiesleutel, is afhankelijk van de tarieven voor lezen en schrijven en de typen query's die u verwacht om uit te voeren. Hier volgen enkele tips over hoe u een goede partitiesleutel kiezen:

* Als uw situatie omvat een klein aantal schrijfbewerkingen die worden verzameld via een lange periode en moet u query's uitvoeren met bereiken van tijdstempels met andere filters, gebruikt u een samenvoeging van de tijdstempel. Bijvoorbeeld, is een goede benadering datum als een partitiesleutel gebruikt. Met deze methode kunt u opvragen over alle gegevens voor een bepaalde datum van één partitie. 
* Als uw werkbelasting schrijfintensief, dat heel gebruikelijk in dit scenario is, gebruikt u een partitiesleutel die niet is gebaseerd op de tijdstempel. Als zodanig, Azure Cosmos DB kunt distribueren en schalen van schrijfbewerkingen gelijkmatig over verschillende partities. Hier een *hostnaam*, *proces-ID*, *activiteits-ID*, of een andere eigenschap met hoge kardinaliteit is een goede keuze. 
* Een andere benadering is een hybride benadering, waarbij u hebt meerdere containers, één voor elke dag/maand, en de partitiesleutel is een meer gedetailleerd eigenschap als *hostnaam*. Deze benadering heeft als voordeel dat u verschillende doorvoer voor elke container of een set van containers op basis van het tijdvenster en de schaal- en prestatiebehoeften voldoen instellen kunt. Bijvoorbeeld kan een container voor de huidige maand worden ingericht met een hogere doorvoer, omdat deze leest fungeert en schrijft. Vorige maanden kunnen worden ingericht met een lagere doorvoer, omdat ze alleen leesbewerkingen fungeren.

### <a name="partitioning-and-multitenancy"></a>Partitionering en multitenancy
Als u een multitenant-toepassing met behulp van Azure Cosmos DB implementeert, er zijn twee populaire ontwerpen om te overwegen: *sleutel van één partitie per tenant* en *één container per tenant*. Hier volgen de voor- en nadelen van elke:

* **Sleutel van één partitie per tenant**. In dit model worden tenants geplaatst in een enkele container. Query's en voegt voor één tenant kunnen worden uitgevoerd op basis van één partitie. U kunt ook transactionele logica implementeren voor alle items die horen bij een tenant. Omdat meerdere tenants een container delen, kunt u beter gebruikmaken van opslag en ingerichte doorvoer door resourcegroepen voor alle tenants in een enkele container in plaats van voor elke tenant wordt ingericht. Het nadeel is dat u geen isolatie van de prestaties per tenant. De doorvoer te verhogen om te waarborgen van de prestaties wordt toegepast op de hele container, waarbij alle tenants versus beoogde toeneemt voor een afzonderlijke tenant.
* **Een container per tenant**. In dit model, elke tenant heeft een eigen container en u kunt doorvoer met gegarandeerde prestaties per tenant reserveren. Dit model is rendabeler voor multitenant-toepassingen met een aantal tenants.

U kunt ook een hybride benadering die kleine tenants samen colocates en Hiermee isoleert u grotere tenants naar hun eigen containers gebruiken.

## <a name="next-steps"></a>Volgende stappen
In dit artikel wordt beschikbaar er een overzicht van concepten en aanbevolen procedures voor het schalen en partitionering in Azure Cosmos DB. 

* Meer informatie over [ingerichte doorvoer in Azure Cosmos DB](request-units.md).
* Meer informatie over [wereldwijde distributie in Azure Cosmos DB](distribute-data-globally.md).



