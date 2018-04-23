---
title: 'Azure Cosmos DB: ontwikkelen met de tabel-API in .NET | Microsoft Docs'
description: Meer informatie over ontwikkelen met de tabel-API van Azure Cosmos DB met .NET
services: cosmos-db
documentationcenter: ''
author: SnehaGunda
manager: kfile
editor: ''
ms.assetid: 4b22cb49-8ea2-483d-bc95-1172cd009498
ms.service: cosmos-db
ms.workload: ''
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: tutorial
ms.date: 12/18/2017
ms.author: sngun
ms.custom: mvc
ms.openlocfilehash: 52d91205e7b0daa82fb12c166222b1dce4c699eb
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2018
---
# <a name="azure-cosmos-db-develop-with-the-table-api-in-net"></a>Azure Cosmos DB: ontwikkelen met de tabel-API in .NET

Azure Cosmos DB is de globaal gedistribueerde multimodel-databaseservice van Microsoft. U kunt snel databases maken van documenten, sleutel/waarde-paren en grafieken en hier query’s op uitvoeren. Deze databases genieten allemaal het voordeel van de globale distributie en horizontale schaalmogelijkheden die ten grondslag liggen aan Azure Cosmos DB.

Deze zelfstudie bestaat uit de volgende taken: 

> [!div class="checklist"] 
> * Maak een Azure Cosmos DB-account 
> * Functionaliteit inschakelen in het bestand app.config 
> * Een tabel maken met de [tabel-API](table-introduction.md)
> * Een entiteit toevoegen aan een tabel 
> * Een batch entiteiten invoegen 
> * Eén entiteit ophalen 
> * Een query uitvoeren op entiteiten met behulp van automatische secundaire indexen 
> * Een entiteit vervangen 
> * Een entiteit verwijderen 
> * Een tabel verwijderen
 
## <a name="tables-in-azure-cosmos-db"></a>Tabellen in Azure Cosmos DB 

Azure Cosmos DB bevat de [tabel-API](table-introduction.md) voor toepassingen waarvoor een sleutelwaardearchief nodig is met een schemaloos ontwerp. Zowel de tabel-API van Azure Cosmos DB als [Azure Table Storage](../storage/common/storage-introduction.md) ondersteunen nu dezelfde SDK's en REST-API's. U kunt Azure Cosmos DB gebruiken om tabellen te maken met hoge vereisten voor doorvoersnelheid.

Deze zelfstudie is bedoeld voor ontwikkelaars die bekend zijn met de Azure Table Storage-SDK en de Premium-functies willen gebruiken die beschikbaar zijn met Azure Cosmos DB. De zelfstudie is gebaseerd op [Get Started with Azure Table storage using .NET](table-storage-how-to-use-dotnet.md) (Aan de slag met Azure Table Storage met .NET) en laat zien hoe u kunt profiteren van extra mogelijkheden, zoals secundaire indexen, ingerichte doorvoer en multihoming. In deze zelfstudie wordt beschreven hoe u Azure Portal kunt gebruiken om een Azure Cosmos DB-account te maken en vervolgens een tabel-API-toepassing te bouwen en implementeren. We doorlopen ook .NET-voorbeelden voor het maken en verwijderen van een tabel en het invoegen, bijwerken, verwijderen en opvragen van tabelgegevens. 

Als u momenteel gebruikmaakt van Azure Table Storage, levert de Azure Cosmos DB tabel-API u de volgende voordelen op:

- Directe [wereldwijde distributie](distribute-data-globally.md) met multihoming en [automatische en handmatige failover](regional-failover.md)
- Ondersteuning voor automatisch schema-agnostisch indexeren voor alle eigenschappen ('secundaire indexen') en snelle query's 
- Ondersteuning voor [onafhankelijk schalen van opslag en doorvoer](partition-data.md), in verschillende regio's
- Ondersteuning voor [toegewezen doorvoer per tabel](request-units.md) die kan worden geschaald van honderden tot miljoenen aanvragen per seconde
- Ondersteuning voor [vijf afstelbare consistentieniveaus](consistency-levels.md) voor een wisselwerking tussen beschikbaarheid, latentie en consistentie op basis van uw toepassingsvereisten
- 99,99% beschikbaarheid binnen één regio en de mogelijkheid meer regio's toe te voegen voor hogere beschikbaarheid en [toonaangevende uitgebreide serviceovereenkomsten](https://azure.microsoft.com/support/legal/sla/cosmos-db/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) over algemene beschikbaarheid
- Werken met de bestaande Azure Storage .NET SDK en geen codewijzigingen in uw toepassing

Deze zelfstudie gaat over de Azure Cosmos DB tabel-API waarbij gebruik wordt gemaakt van de .NET SDK. U kunt de [Azure Cosmos DB tabel-API .NET SDK](https://aka.ms/tableapinuget) downloaden vanuit NuGet.

Zie voor meer informatie over complexe Azure Table Storage-taken:

* [Inleiding tot Azure Cosmos DB Table-API](table-introduction.md)
* De naslagdocumentatie over de tabelservice voor meer informatie over beschikbare API's: [Azure Cosmos DB tabel-API .NET SDK](https://docs.microsoft.com/dotnet/api/overview/azure/cosmosdb/client?view=azure-dotnet)

### <a name="about-this-tutorial"></a>Over deze zelfstudie
Deze zelfstudie is bedoeld voor ontwikkelaars die bekend zijn met de Azure Table Storage-SDK en de Premium-functies willen gebruiken die beschikbaar zijn met Azure Cosmos DB. De zelfstudie is gebaseerd op [Get Started with Azure Table storage using .NET](table-storage-how-to-use-dotnet.md) (Aan de slag met Azure Table Storage met .NET) en laat zien hoe u kunt profiteren van extra mogelijkheden, zoals secundaire indexen, ingerichte doorvoer en multihoming. Er wordt beschreven hoe u Azure Portal kunt gebruiken om een Azure Cosmos DB-account te maken en vervolgens een tabeltoepassing te bouwen en implementeren. We doorlopen ook .NET-voorbeelden voor het maken en verwijderen van een tabel en het invoegen, bijwerken, verwijderen en opvragen van tabelgegevens. 

Als u Visual Studio 2017 nog niet hebt geïnstalleerd, kunt u het downloaden en de **gratis** [Community Edition van Visual Studio 2017](https://www.visualstudio.com/downloads/) gebruiken. Zorg ervoor dat u **Azure-ontwikkeling** inschakelt tijdens de installatie van Visual Studio.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a>Een databaseaccount maken

Begin met het maken van een Azure Cosmos DB-account in Azure Portal.  
 
> [!IMPORTANT]  
> U moet een nieuw Table-API-account maken om te kunnen werken met de algemeen beschikbare SDK's voor de Table-API. Table-API-accounts die zijn gemaakt tijdens de preview worden niet ondersteund door de algemeen beschikbare SDK's. 
>

[!INCLUDE [cosmosdb-create-dbaccount-table](../../includes/cosmos-db-create-dbaccount-table.md)] 

## <a name="clone-the-sample-application"></a>De voorbeeldtoepassing klonen

We gaan nu een Table-app klonen vanaf GitHub, de verbindingsreeks instellen en de app uitvoeren. U zult zien hoe gemakkelijk het is om op een programmatische manier met gegevens te werken. 

1. Open een git-terminalvenster, bijvoorbeeld git bash, en gebruik de opdracht `cd` om naar een map te gaan voor het installeren van de voorbeeld-app. 

    ```bash
    cd "C:\git-samples"
    ```

2. Voer de volgende opdracht uit om de voorbeeldopslagplaats te klonen. Deze opdracht maakt een kopie van de voorbeeld-app op uw computer. 

    ```bash
    git clone https://github.com/Azure-Samples/storage-table-dotnet-getting-started.git
    ```

3. Open vervolgens het oplossingenbestand in Visual Studio. 

## <a name="update-your-connection-string"></a>Uw verbindingsreeks bijwerken

Ga nu terug naar Azure Portal om de verbindingsreeksinformatie op te halen en kopieer deze in de app. Hierdoor kan de app communiceren met de gehoste database. 

1. Klik in [Azure Portal](http://portal.azure.com/) op **Verbindingsreeks**. 

    Gebruik de kopieerknoppen aan de rechterkant van het scherm om de PRIMARY CONNECTION STRING te kopiëren.

    ![De CONNECTION STRING in het deelvenster Verbindingsreeks weergeven en kopiëren](./media/create-table-dotnet/connection-string.png)

2. In Visual Studio opent u het bestand app.config. 

3. Verwijder de opmerking bij de StorageConnectionString op regel 8 en plaats een opmerking bij de StorageConnectionString op regel 7. Dit omdat deze zelfstudie geen gebruik maakt van de Storage Emulator. Regel 7 en 8 moeten er nu als volgt uitzien:

    ```
    <!--key="StorageConnectionString" value="UseDevelopmentStorage=true;" />-->
    <add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=[AccountName];AccountKey=[AccountKey]" />
    ```

4. Plak de PRIMARY CONNECTION STRING vanuit de portal in de waarde StorageConnectionString op regel 8. Plak de tekenreeks tussen de aanhalingstekens.
   
    > [!IMPORTANT]
    > Als uw eindpunt documents.azure.com gebruikt, hebt u een preview-account en moet u een [nieuw Table-API-account](#create-a-database-account) maken om te kunnen werken met de algemeen beschikbare SDK voor Table- API. 
    >

    Regel 8 moet nu ongeveer als volgt uitzien:

    ```
    <add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=txZACN9f...==;TableEndpoint=https://<account name>.table.cosmosdb.azure.com;" />
    ```

5. Sla het bestand app.config op.

U hebt uw app nu bijgewerkt met alle informatie die nodig is voor de communicatie met Azure Cosmos DB. 

## <a name="azure-cosmos-db-capabilities"></a>Mogelijkheden van Azure Cosmos DB
Azure Cosmos DB ondersteunt een aantal mogelijkheden die niet beschikbaar in de Azure Table Storage-API. 

Bepaalde functionaliteit wordt benaderd via nieuwe overloads naar CreateCloudTableClient waarmee het mogelijk is een verbindingsbeleid en consistentieniveau op te geven.

| Verbindingsinstellingen voor tabellen | Beschrijving |
| --- | --- |
| Verbindingsmodus  | Azure Cosmos DB ondersteunt twee modi voor connectiviteit. In de modus `Gateway` worden aanvragen altijd gedaan naar de Azure Cosmos DB-gateway, die deze doorstuurt naar de overeenkomende gegevenspartities. In de connectiviteitsmodus `Direct` haalt de client de toewijzing van tabellen aan partities op en worden aanvragen rechtstreeks gedaan aan gegevenspartities. Wij raden `Direct` aan, de standaardwaarde.  |
| Verbindingsprotocol | Azure Cosmos DB ondersteunt twee modi voor connectiviteit: `Https` en `Tcp`. `Tcp` is de standaardwaarde en wordt aanbevolen omdat deze wat minder ingrijpend is. |
| Voorkeurslocaties | Door komma's gescheiden lijst met aanbevolen (multihoming) locaties voor leesbewerkingen. Elk Azure Cosmos DB-account kan worden gekoppeld aan 1 tot meer dan 30 regio's. Elke clientinstantie kan een subset van deze regio's opgeven in de gewenste volgorde voor leesbewerkingen met een lage latentie. De naam van de regio's moet overeenkomen met hun [weergavenaam](https://msdn.microsoft.com/library/azure/gg441293.aspx), bijvoorbeeld `West US`. Zie ook [Multihoming-API's](tutorial-global-distribution-table.md). |
| Consistentieniveau | U kunt afwegingen maken tussen latentie, consistentie en beschikbaarheid door een keuze te maken uit vijf goed gedefinieerde consistentieniveaus: `Strong`, `Session`, `Bounded-Staleness`, `ConsistentPrefix` en `Eventual`. De standaardwaarde is `Session`. De keuze van consistentieniveau zorgt voor een aanzienlijk prestatieverschil in meerdere land-/regio-instellingen. Zie [Consistentieniveaus](consistency-levels.md) voor meer informatie. |

Andere functionaliteit kan worden ingeschakeld via de volgende `appSettings`-configuratiewaarden.

| Sleutel | Beschrijving |
| --- | --- |
| TableQueryMaxItemCount | Het maximum aantal items configureren dat wordt geretourneerd per tabelquery. De standaardwaarde `-1`, waarmee Azure Cosmos DB de waarde dynamisch bepaalt tijdens runtime. |
| TableQueryEnableScan | Als de query de index voor een filter niet kan gebruiken, voert u deze vervolgens toch uit via een scan. De standaardwaarde is `false`.|
| TableQueryMaxDegreeOfParallelism | De mate van parallelle uitvoering voor de uitvoering van een partitie-overkoepelende query. `0` is serieel zonder vooraf ophalen, `1` is serieel met vooraf ophalen en hogere waarden verhogen de snelheid van de parallelle uitvoering. De standaardwaarde `-1`, waarmee Azure Cosmos DB de waarde dynamisch bepaalt tijdens runtime. |

Open het bestand `app.config` vanuit Solution Explorer in Visual Studio als u de standaardwaarde wilt wijzigen. Voeg de inhoud van het element `<appSettings>` hieronder toe. Vervang `account-name` door de naam van uw opslagaccount en `account-key` door de toegangssleutel van uw account. 

```xml
<configuration>
    ...
    <appSettings>
      <!-- Client options -->
      <add key="CosmosDBStorageConnectionString" 
        value="DefaultEndpointsProtocol=https;AccountName=MYSTORAGEACCOUNT;AccountKey=AUTHKEY;TableEndpoint=https://account-name.table.cosmosdb.azure.com" />
      <add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key; TableEndpoint=https://account-name.documents.azure.com" />

      <!-- Table query options -->
      <add key="TableQueryMaxItemCount" value="-1"/>
      <add key="TableQueryEnableScan" value="false"/>
      <add key="TableQueryMaxDegreeOfParallelism" value="-1"/>
      <add key="TableQueryContinuationTokenLimitInKb" value="16"/>
            
    </appSettings>
</configuration>
```

Laten we eens kijken wat er precies gebeurt in de app. Open het bestand `Program.cs` en u zult zien dat deze coderegels de tabelresources maken. 

## <a name="create-the-table-client"></a>De tabelclient maken
U initialiseert een `CloudTableClient` om verbinding te maken met de tabelaccount.

```csharp
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
```
Deze client wordt geïnitialiseerd met behulp van de configuratiewaarden `TableConnectionMode`, `TableConnectionProtocol`, `TableConsistencyLevel` en `TablePreferredLocations` als deze zijn opgegeven in de app-instellingen.

## <a name="create-a-table"></a>Een tabel maken
Vervolgens maakt u een tabel met `CloudTable`. Tabellen in Azure Cosmos DB kunnen onafhankelijk worden geschaald als het gaat om opslag en doorvoer en partitionering wordt automatisch afgehandeld door de service. Azure Cosmos DB ondersteunt zowel tabellen met een vaste grootte als onbeperkte tabellen. Zie [Partitionering in Azure Cosmos DB](partition-data.md) voor meer informatie. 

```csharp
CloudTable table = tableClient.GetTableReference("people");
400
table.CreateIfNotExists(throughput: 800);
```

Er is een belangrijk verschil in hoe tabellen worden gemaakt. Azure Cosmos DB reserveert doorvoer, in tegenstelling tot het model op basis van verbruik voor transacties van Azure Storage. De doorvoer is toegewezen/gereserveerd, zodat u nooit beperkt wordt in het ophalen als uw aanvraagsnelheid op of onder de ingerichte doorvoer is.

U kunt de standaarddoorvoer configureren door deze als een parameter op te nemen in CreateIfNotExists.

Het lezen van een entiteit van 1 KB wordt genormaliseerd als 1 RU en andere bewerkingen zijn genormaliseerd op een vaste RU-waarde op basis van hun CPU-, geheugen- en IOPS-gebruik. Lees meer over [Aanvraageenheden in Azure Cosmos DB](request-units.md) en specifiek voor [sleutelwaardearchieven](key-value-store-cost.md).

We gaan nu eenvoudige lees- en schrijfbewerkingen (CRUD) met behulp van de SDK van Azure Table Storage doorlopen. In deze zelfstudie worden voorspelbare lage latenties van een milliseconde met één cijfer geïllustreerd en snelle query's die worden geleverd door Azure Cosmos DB.

## <a name="add-an-entity-to-a-table"></a>Een entiteit toevoegen aan een tabel
Entiteiten in Azure Table Storage vormen een uitbreiding van de `TableEntity`-klasse en moeten de eigenschappen `PartitionKey` en `RowKey` hebben. Hier volgt een voorbeelddefinitie voor een klantentiteit.

```csharp
public class CustomerEntity : TableEntity
{
    public CustomerEntity(string lastName, string firstName)
    {
        this.PartitionKey = lastName;
        this.RowKey = firstName;
    }

    public CustomerEntity() { }

    public string Email { get; set; }

    public string PhoneNumber { get; set; }
}
```

Het volgende fragment toont hoe u een entiteit invoegt met de Azure Storage SDK. Azure Cosmos DB is ontworpen voor een gegarandeerde lage latentie op elke schaal overal ter wereld.

Schrijfbewerkingen worden voltooid in < 15 ms op p99 en ~ 6 ms op p50 voor toepassingen die worden uitgevoerd in dezelfde regio als de Azure Cosmos DB-account. En deze duur is verantwoordelijk voor het feit dat schrijfbewerkingen pas terug naar de client worden bevestigd nadat ze synchroon zijn gerepliceerd, blijvend zijn doorgevoerd en alle inhoud is geïndexeerd.


```csharp
// Create a new customer entity.
CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
customer1.Email = "Walter@contoso.com";
customer1.PhoneNumber = "425-555-0101";

// Create the TableOperation object that inserts the customer entity.
TableOperation insertOperation = TableOperation.Insert(customer1);

// Execute the insert operation.
table.Execute(insertOperation);
```

## <a name="insert-a-batch-of-entities"></a>Een batch entiteiten invoegen
Azure Table Storage ondersteunt een batchbewerkings-API, waarmee u updates, verwijderingen en invoegingen in dezelfde batchbewerking kunt combineren.

```csharp
// Create the batch operation.
TableBatchOperation batchOperation = new TableBatchOperation();

// Create a customer entity and add it to the table.
CustomerEntity customer1 = new CustomerEntity("Smith", "Jeff");
customer1.Email = "Jeff@contoso.com";
customer1.PhoneNumber = "425-555-0104";

// Create another customer entity and add it to the table.
CustomerEntity customer2 = new CustomerEntity("Smith", "Ben");
customer2.Email = "Ben@contoso.com";
customer2.PhoneNumber = "425-555-0102";

// Add both customer entities to the batch insert operation.
batchOperation.Insert(customer1);
batchOperation.Insert(customer2);

// Execute the batch operation.
table.ExecuteBatch(batchOperation);
```
## <a name="retrieve-a-single-entity"></a>Eén entiteit ophalen
Ophaalbewerkingen (GET's) in Azure Cosmos DB worden voltooid in < 10 ms op p99 en ~ 1 ms op p50 in dezelfde Azure-regio. U kunt zoveel regio's toevoegen aan uw account voor leesbewerkingen met lage latentie als u wilt en toepassingen implementeren om te lezen uit hun eigen lokale regio ('multihomed') door `TablePreferredLocations` in te stellen. 

U kunt met behulp van het volgende fragment één entiteit ophalen:

```csharp
// Create a retrieve operation that takes a customer entity.
TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

// Execute the retrieve operation.
TableResult retrievedResult = table.Execute(retrieveOperation);
```
> [!TIP]
> Meer informatie over multihoming-API's vindt u in [Ontwikkelen met meerdere regio's](tutorial-global-distribution-table.md)
>

## <a name="query-entities-using-automatic-secondary-indexes"></a>Een query uitvoeren op entiteiten met behulp van automatische secundaire indexen
Tabellen kunnen worden opgevraagd met de `TableQuery`-klasse. Azure Cosmos DB heeft een voor schrijfbewerkingen geoptimaliseerde database-engine die automatisch alle kolommen in de tabel indexeert. Indexering in Azure Cosmos DB is agnostisch ten opzichte van het schema. Daarom wordt uw schema, zelfs als het afwijkt tussen rijen, of als het schema zich gedurende een bepaalde periode verder ontwikkelt, automatisch geïndexeerd. Omdat Azure Cosmos DB automatisch secundaire indexen ondersteunt, kunnen query's op een eigenschap de index gebruiken en efficiënt worden uitgevoerd.

```csharp
CloudTable table = tableClient.GetTableReference("people");

// Filter against a property that's not partition key or row key
TableQuery<CustomerEntity> emailQuery = new TableQuery<CustomerEntity>().Where(
    TableQuery.GenerateFilterCondition("Email", QueryComparisons.Equal, "Ben@contoso.com"));

foreach (CustomerEntity entity in table.ExecuteQuery(emailQuery))
{
    Console.WriteLine("{0}, {1}\t{2}\t{3}", entity.PartitionKey, entity.RowKey,
        entity.Email, entity.PhoneNumber);
}
```

Azure Cosmos DB ondersteunt dezelfde queryfunctionaliteit als Azure Table Storage voor de tabel-API. Azure Cosmos DB biedt ook ondersteuning voor sorteren, statistische functies, georuimtelijke query, hiërarchie en een groot aantal ingebouwde functies. De aanvullende functionaliteit wordt vermeld in de tabel-API in een toekomstige service-update. Zie [Azure Cosmos DB-query](sql-api-sql-query.md) voor een overzicht van deze mogelijkheden. 

## <a name="replace-an-entity"></a>Een entiteit vervangen
Als u een entiteit wilt bijwerken, haalt u deze op uit de Tabelservice, wijzigt u het entiteitsobject en slaat u de wijzigingen weer op in de Tabelservice. De volgende code wijzigt het telefoonnummer van een bestaande klant. 

```csharp
TableOperation updateOperation = TableOperation.Replace(updateEntity);
table.Execute(updateOperation);
```
Op dezelfde manier kunt u `InsertOrMerge`- of `Merge`-bewerkingen uitvoeren.  

## <a name="delete-an-entity"></a>Een entiteit verwijderen
U kunt een entiteit gemakkelijk verwijderen nadat u deze hebt opgehaald. Hiervoor gebruikt u hetzelfde patroon dat is getoond voor het bijwerken van een entiteit. Met de volgende code wordt een klantentiteit opgehaald en verwijderd.

```csharp
TableOperation deleteOperation = TableOperation.Delete(deleteEntity);
table.Execute(deleteOperation);
```

## <a name="delete-a-table"></a>Een tabel verwijderen
Ten slotte wordt met het volgende codevoorbeeld een tabel uit een opslagaccount verwijderd. U kunt een tabel verwijderen en onmiddellijk opnieuw maken met Azure Cosmos DB.

```csharp
CloudTable table = tableClient.GetTableReference("people");
table.DeleteIfExists();
```

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie is besproken hoe u aan de slag kunt met Azure Cosmos DB met de tabel-API en hebt u het volgende gedaan: 

> [!div class="checklist"] 
> * Een Azure Cosmos DB-account gemaakt 
> * Functionaliteit ingeschakeld in het bestand app.config 
> * Een tabel gemaakt 
> * Een entiteit toegevoegd aan een tabel 
> * Een batch entiteiten ingevoegd 
> * Eén entiteit opgehaald 
> * Een query uitgevoerd op entiteiten met behulp van automatische secundaire indexen 
> * Een entiteit vervangen 
> * Een entiteit verwijderd 
> * Een tabel verwijderd  

U kunt nu doorgaan met de volgende zelfstudie en meer leren over het opvragen van tabelgegevens. 

> [!div class="nextstepaction"]
> [Query uitvoeren met de tabel-API](tutorial-query-table.md)
