---
title: Partitionering en horizontaal schalen in Azure Cosmos DB | Microsoft Docs
description: Meer informatie over hoe partitionering werkt in Azure Cosmos DB, partitie-sleutels en configureren met het partitioneren en het kiezen van de juiste partitiesleutel voor uw toepassing.
services: cosmos-db
author: arramac
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: cac9a8cd-b5a3-4827-8505-d40bb61b2416
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/05/2018
ms.author: arramac
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 0032a00883cedfe754e14293dc13a1009f6dd3a0
ms.sourcegitcommit: 1d423a8954731b0f318240f2fa0262934ff04bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/05/2018
---
# <a name="partition-and-scale-in-azure-cosmos-db"></a>Partitie en schalen in Azure Cosmos-DB

[Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) is een globaal gedistribueerd, multimodel-database-service waarmee u snel, voorspelbare prestaties bereiken. Het schalen naadloos samen met uw toepassing wanneer het groeit. Dit artikel bevat een overzicht van hoe werkt voor alle gegevens partitioneren in Azure Cosmos DB modellen. Ook wordt beschreven hoe u Azure DB die Cosmos-containers wilt effectief schalen uw toepassingen kunt configureren.

Partitionering en partitiesleutels worden besproken in deze Azure vrijdag video met Scott Hanselman en Azure Cosmos DB Principal Engineering Manager, Shireesh Thota:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Azure-DocumentDB-Elastic-Scale-Partitioning/player]
> 

## <a name="partitioning-in-azure-cosmos-db"></a>Partitioneren in Azure Cosmos DB
U kunt in Azure Cosmos DB, opslaan en opvragen van schema minder gegevens met de volgorde van milliseconde reactietijden op elke schaal. Azure Cosmos DB containers biedt voor het opslaan van gegevens aangeroepen *verzamelingen* (voor documenten) *grafieken*, of *tabellen*. 

Containers zijn logische resources en kunnen een of meer fysieke partities of servers omvatten. Het aantal partities wordt bepaald door Azure Cosmos DB op basis van de grootte van de opslagruimte en de ingerichte doorvoer van de container. 

Een fysieke partitie is een vast bedrag van gereserveerde back SSD opslag. Elke fysieke partitie worden gerepliceerd voor hoge beschikbaarheid. Een of meer fysieke partities vormen een container. Fysieke partitie management volledig wordt beheerd door Azure Cosmos DB en u hoeft te complexe code schrijven of beheren van de partities. Azure DB Cosmos-containers zijn onbeperkte in termen van opslag en doorvoer. 

Een logische partitie is een partitie in een andere fysieke partitie worden alle gegevens die zijn gekoppeld aan een sleutelwaarde van één partitie opgeslagen. Een logische partitie is maximaal 10 GB. In het volgende diagram wordt in een enkele container drie logische partities heeft. Elke logische partitie bevat de gegevens voor een partitiesleutel LAX AMS en MEL respectievelijk. Alle logische partities LAX AMS en MEL kan niet worden uitgebreid voorbij de maximale logische partitie limiet van 10 GB. 

![Resource partitioneren](./media/introduction/azure-cosmos-db-partitioning.png) 

Wanneer een verzameling voldoet aan de [partitioneren vereisten](#prerequisites), het partitioneren is transparant voor uw toepassing. Azure Cosmos DB ondersteunt snelle leesbewerkingen en schrijfbewerkingen, query's, transactionele logica, consistentieniveaus en verfijnd toegangsbeheer via methoden/API's aan een enkele container-resource. Aanvragen aan de juiste partitie query uitvoeren op de service-ingangen gegevens over fysieke en logische partities verdeeld en routering. 

## <a name="how-does-partitioning-work"></a>Hoe partitioneren werkt

Hoe partitioneren werkt? Elk item moet hebben een partitiesleutel en een rijsleutel die worden geïdentificeerd. De partitiesleutel fungeert als een logische partitie voor uw gegevens en biedt een grens van een natuurlijke Azure Cosmos DB voor gegevens over fysieke partities verdeeld. Houd er rekening mee dat de gegevens voor één logische partitie zich in een enkele fysieke partitie bevinden moet, maar fysieke partitie management wordt beheerd door Azure Cosmos DB. 

Kort samengevat: dit is hoe partitioneren werkt in Azure Cosmos DB:

* Inrichten van een Azure DB die Cosmos-container met **T** aanvragen per tweede doorvoer.
* Achter de schermen, richt Azure Cosmos DB partities nodig om te fungeren **T** aanvragen per seconde. Als **T** hoger is dan de maximale doorvoer per partitie **t**, vervolgens Azure Cosmos DB bepalingen **N = T/t** partities.
* Azure Cosmos DB wordt de sleutel ruimte van de partitie sleutel hashes gelijkmatig meerdere de **N** partities. Dus elke partitie (fysieke partitie) hosts **1/N** partitie sleutelwaarden (logische partities).
* Wanneer een fysieke partitie **p** bereikt de opslaglimiet bereikt, Azure Cosmos DB naadloos splitst **p** in twee nieuwe partities, **p1** en **p2** . Deze distribueert waarden die overeenkomen met ongeveer de helft van de sleutels aan elk van de partities. Deze bewerking gesplitste is onzichtbaar voor uw toepassing. De splitsbewerking wordt niet uitgevoerd als een fysieke partitie de opslaglimiet bereikt en alle gegevens in de fysieke partitie hoort bij dezelfde logische partitiesleutel. Dit komt doordat de gegevens voor een sleutel die één logische partitie zich op dezelfde fysieke partitie bevinden moet en dus de fysieke partitie in p1 en p2 kan niet worden opgesplitst. In dit geval moet de strategie voor een andere partitie worden gebruikt.
* Wanneer u de doorvoer die hoger is dan inrichten  **t*N**, Azure Cosmos DB splitst een of meer van de partities ter ondersteuning van de hogere doorvoer.

De semantiek voor partitiesleutels zijn enigszins verschillen overeenkomen met de semantiek van elke API, zoals wordt weergegeven in de volgende tabel:

| API | Partitiesleutel | Rijsleutel |
| --- | --- | --- |
| Azure Cosmos DB | Aangepaste partitie sleutelpad | Probleem met `id` opgelost | 
| MongoDB | Aangepaste gedeelde sleutel  | Probleem met `_id` opgelost | 
| Graph | Aangepaste partitie sleuteleigenschap | Probleem met `id` opgelost | 
| Tabel | Probleem met `PartitionKey` opgelost | Probleem met `RowKey` opgelost | 

Azure Cosmos DB gebruikt op basis van de hash partitionering. Bij het schrijven van een item wordt Azure Cosmos DB de waarde voor de partitiesleutel-hashes en gebruikt het hash-resultaat om te bepalen welke partitie voor het opslaan van het item in. Azure Cosmos DB slaat alle items met dezelfde partitiesleutel op dezelfde fysieke partitie. De keuze van de partitiesleutel is een belangrijke beslissing die u moet aanbrengen in de ontwerpfase. U moet een eigenschapsnaam die heeft een breed scala aan waarden en zelfs toegangspatronen kiezen. Als een fysieke partitie heeft de opslaglimiet bereikt en dezelfde partitiesleutel op de gegevens in de partitie is, Azure Cosmos DB de fout "de partitiesleutel bereikt maximale grootte van 10 GB retourneert" en de partitie niet is gesplitst, dus het kiezen van een partitiesleutel is een zeer importeren besluit ant.

> [!NOTE]
> Het is een best practice om een partitiesleutel met veel afzonderlijke waarden (500 en 5000 liggen minimaal).
>

Azure DB Cosmos-containers kunnen worden gemaakt als *vaste* of *onbeperkte* in de Azure portal. Containers met vaste grootte hebben een maximale limiet van 10 GB en doorvoer van 10.000 RU/s. Voor het maken van een container als onbeperkt, moet u een minimale doorvoer van 1000 RU/s en moet u een partitiesleutel opgeven.

Er is een goed idee om te controleren hoe uw gegevens wordt gedistribueerd in partities. U kunt dit controleren in de portal, gaat u naar uw Azure DB die Cosmos-account en klik op **metrische gegevens** in **bewaking** sectie en klik vervolgens op op in het rechterdeelvenster **opslag** tab om te zien hoe uw gegevens gepartitioneerd in verschillende fysieke partitie.

![Resource partitioneren](./media/partition-data/partitionkey-example.png)

De afbeelding links geeft het resultaat van een ongeldige partitiesleutel en de juiste afbeelding toont het resultaat van een goede partitiesleutel. In afbeelding links ziet u dat de gegevens worden niet gelijkmatig verdeeld over de partities. U moet streven voor het distribueren van uw gegevens, zodat uw grafiek op de juiste installatiekopie lijkt.

<a name="prerequisites"></a>
## <a name="prerequisites-for-partitioning"></a>Vereisten voor het partitioneren

Voor fysieke partities voor automatisch in te splitsen **p1** en **p2** zoals beschreven in [hoe werkt partitionering](#how-does-partitioning-work), de container moet worden gemaakt met een doorvoersnelheid van 1000 RU/s of meer , en een partitiesleutel moet worden opgegeven. Bij het maken van een container in Azure portal, selecteer de **onbeperkt** opslagoptie de capaciteit om te profiteren van de partitionerings- en automatisch schalen. 

Als u een container in Azure portal of programmatisch gemaakt en de initiële doorvoer 1000 RU/s of meer is, en uw gegevens een partitiesleutel bevatten, u profiteren kunt van het partitioneren van zonder wijzigingen in de container - dit omvat **vast**  grootte containers, zolang de initiële container is gemaakt met ten minste 1000 RU/s in througput en een partitiesleutel aanwezig in de gegevens is.

Als u hebt gemaakt een **vast** grootte container met geen partitie key of gemaakte een **vast** grootte container met doorvoer minder dan 1000 RU/s, de container kan niet automatisch splitsing zoals beschreven in dit artikel. Om gegevens te migreren van de container als volgt aan een onbeperkte container (een met ten minste 1000 RU/s in de doorvoer en een partitiesleutel), die u wilt gebruiken, de [hulpprogramma voor gegevensmigratie](import-data.md) of de [wijziging gegevensfeedbibliotheek](change-feed.md) naar de wijzigingen worden gemigreerd. 

## <a name="partitioning-and-provisioned-throughput"></a>Partitionering en ingerichte doorvoer
Azure Cosmos DB is ontworpen voor voorspelbare prestaties. Wanneer u een container maakt, u doorvoer in termen van reserveren  *[aanvraageenheden](request-units.md) (RU) per seconde*. Elke aanvraag wordt RU kosten met zich mee die evenredig aan de hoeveelheid systeembronnen, zoals CPU, geheugen en i/o verbruikt door de bewerking is toegewezen. Het lezen van een document 1 KB met sessieconsistentie verbruikt 1 RU. Een leesbewerking is 1 RU ongeacht het aantal items die zijn opgeslagen of het aantal gelijktijdige aanvragen die actief zijn op hetzelfde moment. Grotere items moet hoger RUs afhankelijk van de grootte. Als u de grootte van de entiteiten en het aantal leesbewerkingen die u wilt ondersteunen voor uw toepassing weet, kunt u de exacte hoeveelheid doorvoer die nodig is voor uw toepassing moet vindt inrichten. 

> [!NOTE]
> Voor de volledige doorvoer van de container, moet u een partitiesleutel waarmee u aanvragen tussen sommige sleutelwaarden afzonderlijke partitie gelijkmatig wordt verdeeld.
> 
> 

<a name="designing-for-partitioning"></a>
## <a name="work-with-the-azure-cosmos-db-apis"></a>Werken met de Azure Cosmos DB-API 's
U kunt de Azure portal of Azure CLI gebruiken containers maken en op elk gewenst moment worden geschaald. Deze sectie wordt beschreven hoe containers maken en de sleuteldefinitie doorvoer en partitie opgeven in elk van de ondersteunde API's.

### <a name="azure-cosmos-db-api"></a>Azure Cosmos DB-API
Het volgende voorbeeld laat zien hoe een container (verzameling) maken met behulp van de API van Azure Cosmos DB. 

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

U kunt een item (document) lezen met behulp van de `GET` methode in de REST-API of met behulp van `ReadDocumentAsync` in een van de SDK's.

```csharp
// Read document. Needs the partition key and the ID to be specified
DeviceReading document = await client.ReadDocumentAsync<DeviceReading>(
  UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
  new RequestOptions { PartitionKey = new PartitionKey("XMS-0001") });
```

### <a name="mongodb-api"></a>MongoDB-API
U kunt een verzameling shard via uw favoriete hulpprogramma, stuurprogramma of SDK maken met de MongoDB-API. In dit voorbeeld gebruiken we de Mongo-Shell voor het maken van de verzameling.

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

Gebruik de CreateIfNotExists-methode voor het maken van een tabel met de Azure-API voor tabel Cosmos DB. 

```csharp
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

CloudTable table = tableClient.GetTableReference("people");
table.CreateIfNotExists(throughput: 800);
```

Doorvoer is ingesteld als een argument van CreateIfNotExists.

De partitiesleutel is impliciet gemaakt als de `PartitionKey` waarde. 

U kunt één entiteit ophalen met behulp van het volgende fragment:

```csharp
// Create a retrieve operation that takes a customer entity.
TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

// Execute the retrieve operation.
TableResult retrievedResult = table.Execute(retrieveOperation);
```
Zie voor meer informatie [ontwikkelen met de API van de tabel](tutorial-develop-table-dotnet.md).

### <a name="graph-api"></a>Graph API

Met de Graph-API, moet u de Azure portal of Azure CLI om containers maken. U kunt ook omdat Azure Cosmos DB multimodel is, kunt u een van de andere modellen te maken en schalen van uw grafiek container.

U kunt hoekpunt of edge lezen met behulp van de partitiesleutel en -ID in Gremlin. Voor een grafiek met regio ('VS') als de partitiesleutel en "Seattle" Als de rijsleutel, kunt u bijvoorbeeld een hoekpunt vinden met behulp van de volgende syntaxis:

```
g.V(['USA', 'Seattle'])
```

U kunt verwijzen naar een edge met behulp van de partitiesleutel en de rijsleutel.

```
g.E(['USA', 'I5'])
```

Zie voor meer informatie [Gremlin ondersteuning voor Azure Cosmos DB](gremlin-support.md).


<a name="designing-for-partitioning"></a>
## <a name="design-for-partitioning"></a>Ontwerp voor het partitioneren
Als u wilt schalen effectief met Azure Cosmos DB, moet u een goede partitiesleutel kiezen wanneer u de container maken. Er zijn twee belangrijke overwegingen voor het kiezen van een partitiesleutel:

* **Grens voor query's en transacties**. Uw keuze van partitiesleutel moet de noodzaak om het gebruik van transacties de vereiste voor de distributie van de entiteiten over meerdere partitiesleutels om ervoor te zorgen schaalbare oplossing worden verdeeld. U kunt dezelfde partitiesleutel instellen voor alle objecten op één extreme, maar deze optie kan de schaalbaarheid van uw oplossing beperken. Het andere uiterste, kunt u een unieke partitiesleutel voor elk item toewijzen. Deze keuze is zeer schaalbaar, maar deze voorkomt u dat via de transacties tussen meerder documenten via opgeslagen procedures en triggers. Een ideaal partitiesleutel kunt u gebruiken efficiënt query's en heeft voldoende kardinaliteit om te controleren of dat uw oplossing schaalbaar is. 
* **Er is geen opslag en prestaties knelpunten**. Het is belangrijk om te selecteren van een eigenschap die kan worden verdeeld over verschillende afzonderlijke waarden. Aanvragen voor dezelfde partitiesleutel mag niet langer dan de doorvoer van één partitie en zijn beperkt. Het is daarom belangrijk dat u kiest een partitiesleutel die niet in 'hotspots' in uw toepassing resulteren. Omdat de gegevens voor een sleutel die één partitie moet worden opgeslagen in een partitie, moet u vermijden partitiesleutels die grote hoeveelheden gegevens voor dezelfde waarde hebben. 

Bekijk enkele praktijkscenario's en goede partitiesleutels voor elk:
* Als u een gebruiker profiel back-end implementeren, is de gebruikers-ID een goede keuze voor partitiesleutel.
* Als u bij het opslaan van IoT-gegevens, bijvoorbeeld, status van het apparaat, is een apparaat-ID een goede keuze voor partitiesleutel.
* Als u van Azure DB die Cosmos gebruikmaakt voor timeseries gegevens in een logboek, is de hostnaam of het proces-ID een goede keuze voor partitiesleutel.
* Als u een multitenant-architectuur hebt, is de tenant-ID een goede keuze voor partitiesleutel.

Gebruik in sommige gevallen, zoals IoT- en profielen, de partitiesleutel kan niet hetzelfde zijn als uw ID (documentsleutel). In andere gevallen, zoals de timeseries gegevens, moet u wellicht een partitiesleutel die verschilt van de-ID.

### <a name="partitioning-and-loggingtime-series-data"></a>Partitionering en logboekregistratie-tijdreeks gegevens
Een van de algemene gebruiksvoorbeelden van Azure DB die Cosmos is voor logboekregistratie en telemetrie. Het is belangrijk een goede partitiesleutel kiest u wellicht enorme hoeveelheden gegevens lezen/schrijven. De keuze, is afhankelijk van uw tarieven lezen en schrijven en de soorten query's die u verwacht te worden uitgevoerd. Hier volgen enkele tips over het kiezen van een goede partitiesleutel:

* Als uw gebruiksvoorbeeld omvat een klein aantal schrijfbewerkingen die worden verzameld via lang duren en moet u een query door bereiken van tijdstempels en andere filters, gebruikt u een updatepakket van het tijdstempel. Bijvoorbeeld, is een goede benadering datum als een partitiesleutel gebruikt. Met deze methode kunt u over alle gegevens opvragen voor een datum van één partitie. 
* Gebruik een partitiesleutel die niet gebaseerd op tijdstempel als uw werkbelasting zware die veelvoorkomende, is geschreven. Met deze methode voert kunt Azure Cosmos DB schrijfbewerkingen gelijkmatig verdelen over verschillende partities. Een hostnaam, proces-ID, activiteits-ID of een andere eigenschap met hoge kardinaliteit is hier een goede keuze. 
* Een andere manier is een hybride een wanneer u meerdere containers, één voor elke dag/maand, werkt en de partitiesleutel is een gedetailleerd eigenschap als de hostnaam. Deze methode heeft als voordeel dat u kunt verschillende doorvoer op basis van de periode instellen. Bijvoorbeeld, wordt de container voor de huidige maand met hogere doorvoer ingericht, omdat deze leest fungeert en schrijft. Vorige maanden zijn ingericht met lagere doorvoer omdat ze alleen leesbewerkingen fungeren.

### <a name="partitioning-and-multitenancy"></a>Partitionering en multitenancy
Als u een multitenant-toepassing implementeren met behulp van Azure Cosmos DB, er zijn twee populaire patronen: een partitiesleutel per tenant en een container per tenant. Hier volgen de voordelen en nadelen voor elk:

* **Een partitiesleutel per tenant**. In dit model worden tenants geplaatste binnen een enkele container. Maar query's en voegt voor items binnen een enkele tenant kunnen worden uitgevoerd op een enkele partitie. U kunt ook transactionele logica implementeren voor alle items in een tenant. Omdat meerdere tenants een container delen, kunt u opslag en doorvoer kosten besparen door groeperen van bronnen voor tenants binnen een enkele container dan extra ruimte voor elke tenant wordt ingericht. Het nadeel is dat er geen isolatie van de prestaties per tenant. Toename van prestaties/doorvoer van toepassing op de volledige container versus gerichte toeneemt voor tenants.
* **Een container per tenant**. In dit model elke tenant heeft zijn eigen container en kunt u de prestaties per tenant reserveren. Dit model is met de Cosmos Azure DB inrichten van nieuwe prijzen, rendabeler voor multitenant-toepassingen met een paar tenants.

U kunt ook een combinatie/gelaagde benadering die kleine tenants collocates en grotere tenants naar hun eigen container wordt gemigreerd.

## <a name="next-steps"></a>Volgende stappen
In dit artikel opgegeven we een overzicht van de concepten en aanbevolen procedures voor het partitioneren van Azure Cosmos DB API. 

* Meer informatie over [ingerichte doorvoer in Azure Cosmos DB](request-units.md).
* Meer informatie over [globale distributie in Azure Cosmos DB](distribute-data-globally.md).



