---
title: 'Azure Cosmos DB: Ontwikkelen met de tabel API in .NET | Microsoft Docs'
description: Meer informatie over het ontwikkelen met Azure Cosmos DB tabel API met .NET
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
ms.assetid: 4b22cb49-8ea2-483d-bc95-1172cd009498
ms.service: cosmos-db
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: tutorial
ms.date: 12/18/2017
ms.author: arramac
ms.custom: mvc
ms.openlocfilehash: 41d7e42f203170e4fa3b8e3a8c973e23808f941b
ms.sourcegitcommit: c87e036fe898318487ea8df31b13b328985ce0e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/19/2017
---
# <a name="azure-cosmos-db-develop-with-the-table-api-in-net"></a>Azure Cosmos DB: Ontwikkelen met de tabel API in .NET

Azure Cosmos DB is de globaal gedistribueerde multimodel-databaseservice van Microsoft. U kunt snel databases maken van documenten, sleutel/waarde-paren en grafieken en hier query’s op uitvoeren. Deze databases genieten allemaal het voordeel van de globale distributie en horizontale schaalmogelijkheden die ten grondslag liggen aan Azure Cosmos DB.

Deze zelfstudie bevat de volgende taken: 

> [!div class="checklist"] 
> * Maak een Azure Cosmos DB-account 
> * Schakel in het bestand app.config-functionaliteit 
> * Maak een tabel met de [tabel-API](table-introduction.md)
> * Een entiteit toevoegen aan een tabel 
> * Een batch entiteiten invoegen 
> * Eén entiteit ophalen 
> * Query-entiteiten met behulp van automatische secundaire indexen 
> * Een entiteit vervangen 
> * Een entiteit verwijderen 
> * Een tabel verwijderen
 
## <a name="tables-in-azure-cosmos-db"></a>Tabellen in Azure Cosmos DB 

Azure Cosmos DB biedt de [tabel API](table-introduction.md) voor toepassingen die een sleutel-waardearchief met een schema-less-ontwerp moeten. Zowel Azure Cosmos DB tabel API en [Azure Table storage](../storage/common/storage-introduction.md) ondersteunen nu de dezelfde REST-API's en SDK's. U kunt Azure Cosmos DB gebruiken om tabellen te maken met hoge vereisten voor doorvoersnelheid.

Deze zelfstudie is bedoeld voor ontwikkelaars die bekend bent met het Azure Table-opslag-SDK en wilt de beschikbare Premiumfuncties gebruiken met Azure Cosmos DB. Deze is gebaseerd op [aan de slag met Azure Table storage met .NET](table-storage-how-to-use-dotnet.md) en laat zien hoe om te profiteren van extra mogelijkheden, zoals secundaire indexen, ingerichte doorvoer en multihoming. Deze zelfstudie wordt beschreven hoe u met de Azure portal een Azure DB die Cosmos-account maken en bouwen en implementeren van een tabel-API-toepassing. We helpt ook bij .NET-voorbeelden voor het maken en verwijderen van een tabel en invoegen, bijwerken, verwijderen en opvragen van tabelgegevens. 

Als u momenteel Azure Table storage, krijgt u de volgende voordelen met API van Azure Cosmos DB tabel:

- Directe [globale distributie](distribute-data-globally.md) met multihoming en [automatische en handmatige failover](regional-failover.md)
- Ondersteuning voor automatische schema-networkdirect indexeren tegen alle eigenschappen ('secundaire indexen') en snelle query 's 
- Ondersteuning voor [onafhankelijke schalen van opslag en doorvoer](partition-data.md), via verschillende regio's
- Ondersteuning voor [toegewezen doorvoer per tabel](request-units.md) die kunnen worden geschaald van honderden miljoenen aanvragen per seconde
- Ondersteuning voor [vijf instelbare consistentieniveaus](consistency-levels.md) moeten op de handel uitschakelen beschikbaarheid, latentie en consistentie op basis van uw toepassing
- 99,99% beschikbaarheid binnen één regio, en het toevoegen van meer regio's voor hogere beschikbaarheid en [toonaangevende uitgebreide serviceovereenkomsten](https://azure.microsoft.com/support/legal/sla/cosmos-db/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) op algemene beschikbaarheid
- Werken met de bestaande Azure-opslag .NET SDK en geen codewijzigingen in uw toepassing

Deze zelfstudie bevat informatie over Azure Cosmos DB tabel API met de .NET SDK. U kunt downloaden via de [Preview-SDK van Azure Storage](https://aka.ms/tableapinuget) vanuit NuGet.

Zie voor meer informatie over complexe Azure Table storage taken:

* [Inleiding tot Azure Cosmos DB tabel API](table-introduction.md)
* De tabel-naslagdocumentatie voor meer informatie over beschikbare API's [Azure Cosmos DB tabel API .NET SDK](https://docs.microsoft.com/dotnet/api/overview/azure/cosmosdb/client?view=azure-dotnet)

### <a name="about-this-tutorial"></a>Over deze zelfstudie
Voor ontwikkelaars die bekend bent met het Azure Table-opslag-SDK en wilt gebruiken, de premiumfuncties die beschikbaar is in deze zelfstudie met behulp van Azure Cosmos DB. Deze is gebaseerd op [aan de slag met Azure Table storage met .NET](table-storage-how-to-use-dotnet.md) en laat zien hoe om te profiteren van extra mogelijkheden, zoals secundaire indexen, ingerichte doorvoer en multihoming. We uitgelegd hoe de Azure portal gebruiken voor een Azure DB die Cosmos-account maken en bouwen en implementeren van de toepassing van een tabel. We helpt ook bij .NET-voorbeelden voor het maken en verwijderen van een tabel en invoegen, bijwerken, verwijderen en opvragen van tabelgegevens. 

Als u Visual Studio 2017 nog niet hebt geïnstalleerd, kunt u het downloaden en de **gratis** [Community Edition van Visual Studio 2017](https://www.visualstudio.com/downloads/) gebruiken. Zorg ervoor dat u **Azure-ontwikkeling** inschakelt tijdens de installatie van Visual Studio.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a>Een databaseaccount maken

Begint met het maken van een Azure DB die Cosmos-account in de Azure portal.  
 
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

## <a name="azure-cosmos-db-capabilities"></a>Mogelijkheden van Azure DB Cosmos
Azure Cosmos DB ondersteunt een aantal mogelijkheden die niet beschikbaar in de Azure Table storage-API. 

Bepaalde functionaliteit wordt benaderd via nieuwe overloads aan CreateCloudTableClient waarmee een beleid en consistentie verbindingsniveau opgeven.

| Tabel-verbindingsinstellingen | Beschrijving |
| --- | --- |
| Verbindingsmodus  | Azure Cosmos DB ondersteunt twee modi voor connectiviteit. In `Gateway` modus aanvragen worden altijd aangebracht in de Azure DB die Cosmos-gateway stuurt deze door naar de bijbehorende gegevenspartities. In `Direct` connectiviteitsmodus, de client haalt de toewijzing van tabellen partities en aanvragen worden gedaan, rechtstreeks op basis van de gegevenspartities. Het is raadzaam `Direct`, de standaardwaarde.  |
| Protocol van verbinding | Azure Cosmos DB ondersteunt twee verbindingsprotocollen - `Https` en `Tcp`. `Tcp`de standaardwaarde is en omdat het meer lightweight aanbevolen. |
| Voorkeur locaties | Door komma's gescheiden lijst met aanbevolen (multihoming) locaties voor leesbewerkingen. Elke Azure DB die Cosmos-account kan worden gekoppeld met 1-30 + regio's. Elk clientexemplaar kunt u een subset van deze gebieden in de gewenste volgorde voor lage latentie leesbewerkingen opgeven. De regio's moeten de namen van hun [weergavenamen](https://msdn.microsoft.com/library/azure/gg441293.aspx), bijvoorbeeld `West US`. Zie ook [multihoming API's](tutorial-global-distribution-table.md). |
| Consistentieniveau | U kunt uitschakelen handel tussen latentie, consistentie en beschikbaarheid door te kiezen tussen vijf goed gedefinieerde consistentieniveaus: `Strong`, `Session`, `Bounded-Staleness`, `ConsistentPrefix`, en `Eventual`. Standaard is `Session`. De keuze van de consistentieniveau maakt een belangrijke prestatieverbetering verschil in meerdere landen/regio-instellingen. Zie [consistentieniveaus](consistency-levels.md) voor meer informatie. |

Andere functies kan worden ingeschakeld via de volgende `appSettings` configuratiewaarden.

| Sleutel | Beschrijving |
| --- | --- |
| TableQueryMaxItemCount | Het maximum aantal items geretourneerd per tabelquery in een enkel retour configureren. Standaard is `-1`, waarmee Azure Cosmos DB dynamisch bepalen van de waarde tijdens runtime. |
| TableQueryEnableScan | Als de query niet kan de index voor elk filter, klikt u vervolgens het toch uitvoeren via een scan. Standaard is `false`.|
| TableQueryMaxDegreeOfParallelism | De mate van parallelle uitvoering voor uitvoering van een query cross-partitie. `0`seriële met geen vooraf ophalen, is `1` is seriële met waarden vooraf ophalen en hoger de snelheid van parallelle uitvoering verhogen. Standaard is `-1`, waarmee Azure Cosmos DB dynamisch bepalen van de waarde tijdens runtime. |

De standaardwaarde wilt wijzigen, opent u de `app.config` bestand vanuit Solution Explorer in Visual Studio. Voeg de inhoud van het element `<appSettings>` hieronder toe. Vervang `account-name` met de naam van uw opslagaccount en `account-key` door de toegangssleutel voor uw account. 

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

Laten we eens kijken wat er precies gebeurt in de app. Open de `Program.cs` bestands- en u ziet dat deze regels code maken met de resources van de tabel. 

## <a name="create-the-table-client"></a>De tabel-client maken
U initialiseren een `CloudTableClient` verbinding maken met het account van de tabel.

```csharp
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
```
Deze client is geïnitialiseerd met behulp van de `TableConnectionMode`, `TableConnectionProtocol`, `TableConsistencyLevel`, en `TablePreferredLocations` configuratiewaarden als opgegeven in de app-instellingen.

## <a name="create-a-table"></a>Een tabel maken
Maakt u een tabel met `CloudTable`. Tabellen in Azure Cosmos DB onafhankelijk kunnen schalen in termen van opslag en de doorvoer en automatisch partitioneren is verwerkt door de service. Azure Cosmos DB ondersteunt zowel vaste grootte als onbeperkt aantal tabellen. Zie [partitioneren in Azure Cosmos DB](partition-data.md) voor meer informatie. 

```csharp
CloudTable table = tableClient.GetTableReference("people");
400
table.CreateIfNotExists(throughput: 800);
```

Er is een belangrijk verschil in hoe tabellen worden gemaakt. Azure Cosmos DB reserveert doorvoer, in tegenstelling tot Azure storage op basis van verbruik model voor transacties. De doorvoer is toegewezen/gereserveerd, zodat u nooit kunnen beperkt ophalen als uw aanvraagsnelheid op of onder uw ingerichte doorvoer is.

U kunt de standaard-doorvoer configureren door te nemen als een parameter van CreateIfNotExists.

Het lezen van een entiteit 1 KB is genormaliseerd als 1 RU en andere bewerkingen zijn genormaliseerd op een vaste RU-waarde op basis van hun verbruik CPU, geheugen en IOPS. Meer informatie over [Aanvraageenheden in Azure Cosmos DB](request-units.md) en specifiek voor [sleutel-waardearchieven](key-value-store-cost.md).

Vervolgens we doorlopen van het eenvoudige lezen en schrijven (CRUD)-bewerkingen met de SDK van Azure Table storage. Deze zelfstudie wordt gedemonstreerd voorspelbare lage één cijfer milliseconde latenties en snelle query's die worden geleverd door Azure Cosmos DB.

## <a name="add-an-entity-to-a-table"></a>Een entiteit toevoegen aan een tabel
Entiteiten in Azure Table storage uitbreiden van de `TableEntity` klasse en moet `PartitionKey` en `RowKey` eigenschappen. Hier volgt een voorbeeld-definitie voor een klantentiteit.

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

Het volgende fragment toont het invoegen van een entiteit met de Azure SDK-opslag. Azure Cosmos DB is ontworpen voor lage latentie op elke schaal gegarandeerd over de hele wereld.

Schrijfbewerkingen voltooien < 15 ms op p99 en ~ 6 ms op p50 voor toepassingen die worden uitgevoerd in dezelfde regio bevinden als de Azure DB die Cosmos-account. En deze duur is verantwoordelijk voor het feit dat schrijfbewerkingen terug naar de client zijn bevestigd nadat ze zijn synchroon gerepliceerd, blijvend doorgevoerd, en alle inhoud wordt geïndexeerd.


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
Azure Table storage ondersteunt een batchbewerking API, waarmee u het combineren van updates, verwijderingen en invoegingen in dezelfde batchbewerking.

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
(Opgehaald) opgehaald in Azure Cosmos DB voltooid < 10 ms op p99 en ~ 1 ms op p50 in dezelfde Azure-regio. U kunt zoveel regio's toevoegen aan uw account voor leesbewerkingen lage latentie en implementeren van toepassingen voor het lezen van de lokale regio ('multihomed') door in te stellen `TablePreferredLocations`. 

U kunt met behulp van het volgende fragment één entiteit ophalen:

```csharp
// Create a retrieve operation that takes a customer entity.
TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

// Execute the retrieve operation.
TableResult retrievedResult = table.Execute(retrieveOperation);
```
> [!TIP]
> Meer informatie over multihoming API's op [ontwikkelen met meerdere regio's](tutorial-global-distribution-table.md)
>

## <a name="query-entities-using-automatic-secondary-indexes"></a>Query-entiteiten met behulp van automatische secundaire indexen
Tabellen kunnen worden opgevraagd met de `TableQuery` klasse. Azure Cosmos-database heeft een schrijven geoptimaliseerd database-engine die automatisch alle kolommen in de tabel indexeert. Indexeren in Azure DB die Cosmos is agnostisch ten opzichte van schema. Dus wordt zelfs als uw schema afwijkt van rijen, of als het schema gedurende een bepaalde periode zich verder ontwikkelen, deze automatisch geïndexeerd. Omdat Azure Cosmos DB automatische secundaire indexen ondersteunt, wordt een query uitgevoerd naar een eigenschap de index kunnen gebruiken en efficiënt worden geleverd.

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

Azure Cosmos DB ondersteunt dezelfde queryfunctionaliteit als Azure Table storage voor de tabel-API. Azure Cosmos DB biedt ook ondersteuning voor sorteren, statistische functies, georuimtelijke query hiërarchie en een groot aantal ingebouwde functies. De aanvullende functionaliteit worden vermeld in de tabel-API in een toekomstige service-update. Zie [Azure Cosmos DB-query](sql-api-sql-query.md) voor een overzicht van deze mogelijkheden. 

## <a name="replace-an-entity"></a>Een entiteit vervangen
Als u een entiteit wilt bijwerken, haalt u deze op uit de Tabelservice, wijzigt u het entiteitsobject en slaat u de wijzigingen weer op in de Tabelservice. De volgende code wijzigt het telefoonnummer van een bestaande klant. 

```csharp
TableOperation updateOperation = TableOperation.Replace(updateEntity);
table.Execute(updateOperation);
```
Op deze manier kunt u uitvoeren `InsertOrMerge` of `Merge` bewerkingen.  

## <a name="delete-an-entity"></a>Een entiteit verwijderen
U kunt een entiteit gemakkelijk verwijderen nadat u deze hebt opgehaald. Hiervoor gebruikt u hetzelfde patroon dat is getoond voor het bijwerken van een entiteit. Met de volgende code wordt een klantentiteit opgehaald en verwijderd.

```csharp
TableOperation deleteOperation = TableOperation.Delete(deleteEntity);
table.Execute(deleteOperation);
```

## <a name="delete-a-table"></a>Een tabel verwijderen
Ten slotte wordt met het volgende codevoorbeeld een tabel uit een opslagaccount verwijderd. U kunt verwijderen en opnieuw maken van een tabel met Azure Cosmos DB onmiddellijk.

```csharp
CloudTable table = tableClient.GetTableReference("people");
table.DeleteIfExists();
```

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Volgende stappen

We hoe u aan de slag met Azure Cosmos DB met de API tabel behandeld in deze zelfstudie maakt en u het volgende hebt gedaan: 

> [!div class="checklist"] 
> * Een Azure DB die Cosmos-account gemaakt 
> * Ingeschakelde functionaliteit in het bestand app.config 
> * Een tabel gemaakt 
> * Een entiteit toegevoegd aan een tabel 
> * Een batch entiteiten invoegen 
> * Één entiteit opgehaald 
> * De query entiteiten met behulp van automatische secundaire indexen 
> * Een entiteit vervangen 
> * Een entiteit wordt verwijderd 
> * Een tabel verwijderd  

U kunt nu doorgaan met de volgende zelfstudie en meer informatie over het opvragen van tabelgegevens. 

> [!div class="nextstepaction"]
> [Query met de tabel-API](tutorial-query-table.md)
