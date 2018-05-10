---
title: Met BulkExecutor .NET-bibliotheek voor het uitvoeren van bulkbewerkingen in Azure Cosmos DB | Microsoft Docs
description: Gebruik Azure Cosmos DB BulkExecutor .NET-bibliotheek bulkimport en documenten bijwerken naar Azure Cosmos DB verzamelingen.
keywords: .NET bulksgewijs executor
services: cosmos-db
author: tknandu
manager: kfile
ms.service: cosmos-db
ms.workload: data-services
ms.topic: article
ms.date: 05/07/2018
ms.author: ramkris
ms.openlocfilehash: 608551090ce10e08ba517def644c72186a6f25e1
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/08/2018
---
# <a name="using-bulkexecutor-net-library-to-perform-bulk-operations-in-azure-cosmos-db"></a>Met BulkExecutor .NET-bibliotheek voor het uitvoeren van bulkbewerkingen in Azure Cosmos-DB

Deze zelfstudie bevat instructies over het gebruik van de Azure Cosmos-DB BulkExecutor .NET-bibliotheek te importeren en bijwerken van documenten voor Azure DB die Cosmos-verzamelingen. Zie voor meer informatie over BulkExecutor-bibliotheek en hoe kunt u gebruikmaken van grote doorvoer en opslag, [BulkExecutor bibliotheek overzicht](bulk-executor-overview.md) artikel. Deze zelfstudie helpt u bij een voorbeeldtoepassing .NET welke bulksgewijs willekeurig gegenereerde documenten in een verzameling Azure Cosmos DB importeert. Nadat u hebt geïmporteerd ziet u hoe u kunt bulksgewijs de geïmporteerde gegevens door te geven van patches als bewerkingen om uit te voeren op specifieke documentvelden bijwerken.

## <a name="prerequisites"></a>Vereisten

* Als u Visual Studio 2017 geïnstalleerd nog geen hebt, kunt u downloaden en gebruiken de [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/). Zorg ervoor dat het ontwikkelen van Azure tijdens de installatie van Visual Studio in te schakelen.

* Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) aan voordat u begint. 

* [Probeer Azure Cosmos DB gratis uit](https://azure.microsoft.com/try/cosmosdb/) zonder Azure-abonnement, zonder kosten en zonder verplichtingen. Of u kunt de [Azure Cosmos DB Emulator](https://docs.microsoft.com/azure/cosmos-db/local-emulator) met de `https://localhost:8081` URI. De primaire sleutel is opgegeven [aanvragen verifiëren](local-emulator.md#authenticating-requests).

* Een SQL-API van Azure Cosmos DB-account maken met behulp van de stappen in [-databaseaccount maken](create-sql-api-dotnet.md#create-a-database-account) sectie van het .NET-Quick Start-artikel. 

## <a name="clone-the-sample-application"></a>De voorbeeldtoepassing klonen

Nu gaan we overschakelen naar het werken met code sommige .NET-voorbeeldtoepassingen vanuit GitHub downloaden. Deze toepassingen Bulksgewijze bewerkingen uitvoeren op Azure Cosmos DB gegevens. Klonen van de toepassingen, open een opdrachtprompt, Ga naar de map waar u de deze kopiëren en voer de volgende opdracht:

```
git clone https://github.com/Azure/azure-cosmosdb-bulkexecutor-dotnet-getting-started.git
```

De gekloonde opslagplaats bevat twee voorbeelden 'BulkImportSample' en 'BulkUpdateSample'. U kunt openen op een van de voorbeeldtoepassingen, werk de verbindingsreeksen in het bestand App.config met verbindingsreeksen voor uw Azure DB die Cosmos-account, de oplossing bouwen en uitvoeren. 

De toepassing 'BulkImportSample' willekeurige documenten genereert en bulksgewijs importeert in Azure Cosmos DB. Het merendeel van de toepassing 'BulkUpdateSample' updates de geïmporteerde documenten door te geven van patches als bewerkingen om uit te voeren op specifieke documentvelden. In de volgende secties controleert u de code in elk van deze voorbeeld-apps.

## <a name="bulk-import-data-to-azure-cosmos-db"></a>Importeer gegevens bulksgewijs naar Azure Cosmos-DB

1. Navigeer naar de map 'BulkImportSample' en open het bestand 'BulkImportSample.sln'.  

2. De Azure Cosmos-DB-verbindingsreeksen worden opgehaald uit het bestand App.config, zoals wordt weergegeven in de volgende code:  

   ```csharp
   private static readonly string EndpointUrl = ConfigurationManager.AppSettings["EndPointUrl"];
   private static readonly string AuthorizationKey = ConfigurationManager.AppSettings["AuthorizationKey"];
   private static readonly string DatabaseName = ConfigurationManager.AppSettings["DatabaseName"];
   private static readonly string CollectionName = ConfigurationManager.AppSettings["CollectionName"];
   private static readonly int CollectionThroughput = int.Parse(ConfigurationManager.AppSettings["CollectionThroughput"]);
   ```

   De importfunctie bulksgewijs maakt een nieuwe database en een verzameling met de databasenaam, de verzamelingsnaam en de doorvoer waarden die zijn opgegeven in het bestand App.config. 

3. Naast is de DocumentClient-object geïnitialiseerd met de modus voor Direct TCP-verbinding:  

   ```csharp
   ConnectionPolicy connectionPolicy = new ConnectionPolicy
   {
      ConnectionMode = ConnectionMode.Direct,
      ConnectionProtocol = Protocol.Tcp
   };
   DocumentClient client = new DocumentClient(new Uri(endpointUrl),authorizationKey,
   connectionPolicy)
   ```

4. Het object BulkExecutor is geïnitialiseerd met een hoge opnieuw waarden voor de wachttijd en de snelheid van aanvragen. En vervolgens worden ze ingesteld op 0 overbezetting doorgeven aan BulkExecutor voor de levensduur.  

   ```csharp
   // Set retry options high during initialization (default values).
   client.ConnectionPolicy.RetryOptions.MaxRetryWaitTimeInSeconds = 30;
   client.ConnectionPolicy.RetryOptions.MaxRetryAttemptsOnThrottledRequests = 9;

   IBulkExecutor bulkExecutor = new BulkExecutor(client, dataCollection);
   await bulkExecutor.InitializeAsync();

   // Set retries to 0 to pass complete control to bulk executor.
   client.ConnectionPolicy.RetryOptions.MaxRetryWaitTimeInSeconds = 0;
   client.ConnectionPolicy.RetryOptions.MaxRetryAttemptsOnThrottledRequests = 0;
   ```

5. De toepassing roept de API BulkImportAsync. De .NET-bibliotheek biedt twee overloads van de bulksgewijs importeren API - die een lijst met geserialiseerde JSON-documenten accepteert en de andere accepteert een lijst van gedeserialiseerde POCO-documenten. Raadpleeg voor meer informatie over de definities van elk van deze overbelaste methoden, [API-documentatie](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmosdb.bulkexecutor.bulkexecutor.bulkimportasync?view=azure-dotnet).

   ```csharp
   BulkImportResponse bulkImportResponse = await bulkExecutor.BulkImportAsync(
     documents: documentsToImportInBatch,
     enableUpsert: true,
     disableAutomaticIdGeneration: true,
     maxConcurrencyPerPartitionKeyRange: null,
     maxInMemorySortingBatchSize: null,
     cancellationToken: token);
   ```
   **BulkImportAsync methode accepteert de volgende parameters:**
   
   |**Parameter**  |**Beschrijving** |
   |---------|---------|
   |enableUpsert    |   Een markering om in te schakelen upsert van de documenten. Als een document met opgegeven id bestaat al, wordt bijgewerkt. Standaard is deze waarde ingesteld op false.      |
   |disableAutomaticIdGeneration    |    Een markering automatisch genereren van id uitschakelen Standaard is ingesteld op true.     |
   |maxConcurrencyPerPartitionKeyRange    | De maximale mate van inbreuk op gelijktijdige per partitiesleutelbereik instellen op null, wordt een standaardwaarde van 20 gewenste tapewisselaar. |
   |maxInMemorySortingBatchSize     |  Het maximum aantal documenten opgehaald uit de enumerator document dat wordt doorgegeven aan de API-aanroep in elke fase.  In het geheugen vooraf verwerken sorteren fase vóór bulkbewerkingen voor importeren veroorzaakt instelling op null tapewisselaar standaardwaarde van min (documents.count 1000000).       |
   |cancellationToken    |    Het token Annuleren om af te sluiten zonder problemen bulkimport.     |

   **Bulksgewijs importeren antwoord objectdefinitie** het resultaat van de API-aanroep voor bulkimport bevat de volgende kenmerken:

   |**Parameter**  |**Beschrijving**  |
   |---------|---------|
   |NumberOfDocumentsImported (lang)   |  Het totale aantal documenten die zijn geïmporteerd uit de opgegeven voor het bulksgewijs documenten importeren API-aanroep.       |
   |TotalRequestUnitsConsumed (double)   |   De totale aanvraageenheden (RU) gebruikt door de bulksgewijs importeren API-aanroep.      |
   |TotalTimeTaken (TimeSpan)    |   De totale tijd die door de bulkimport API-aanroep uitgevoerd.      |
   |BadInputDocuments (lijst<object>)   |     De lijst met onjuiste indeling documenten die zijn niet geïmporteerd in de meeste importeren API-aanroep. Gebruiker moet los van de documenten die worden geretourneerd en probeer het opnieuw importeren. Onjuiste indeling of meer documenten waarvan id-waarde geen tekenreeks is (null of een andere datatype wordt beschouwd als ongeldig).    |

## <a name="bulk-update-data-in-azure-cosmos-db"></a>Gegevens voor bulksgewijs bijwerken in Azure Cosmos-DB

U kunt bestaande documenten met behulp van de API BulkUpdateAsync bijwerken. In dit voorbeeld wordt u het veld naam ingesteld op een nieuwe waarde en beschrijvingsveld verwijderen uit de bestaande documenten. Voor de volledige set van ondersteunde veld updatebewerkingen, raadpleeg dan [API-documentatie](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmosdb.bulkexecutor.bulkupdate?view=azure-dotnet). 

1. Navigeer naar de map 'BulkUpdateSample' en open het bestand 'BulkUpdateSample.sln'.  

2. Definieer de update-items, samen met de bijbehorende veld update-bewerkingen. In dit voorbeeld gebruikt u SetUpdateOperation het naamveld en UnsetUpdateOperation het beschrijvingsveld verwijderen uit alle documenten bij te werken. U kunt ook andere bewerkingen zoals verhoging een documentveld uitvoeren door een specifieke waarde, push-specifieke waarden in het matrixveld van een of een specifieke waarde verwijderen uit een array-veld. Raadpleeg voor meer informatie over verschillende methoden die worden geleverd door de bulk-update API, de [API-documentatie](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmosdb.bulkexecutor.bulkupdate?view=azure-dotnet).

   ```csharp
   SetUpdateOperation<string> nameUpdate = new SetUpdateOperation<string>("Name", "UpdatedDoc");
   UnsetUpdateOperation descriptionUpdate = new UnsetUpdateOperation("description");

   List<UpdateOperation> updateOperations = new List<UpdateOperation>();
   updateOperations.Add(nameUpdate);
   updateOperations.Add(descriptionUpdate);

   List<UpdateItem> updateItems = new List<UpdateItem>();
   for (int i = 0; i < 10; i++)
   {
    updateItems.Add(new UpdateItem(i.ToString(), i.ToString(), updateOperations));
   }
   ```

3. De toepassing roept de API BulkUpdateAsync. Raadpleeg voor meer informatie over de definitie van de methode BulkUpdateAsync, [API-documentatie](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmosdb.bulkexecutor.ibulkexecutor.bulkupdateasync?view=azure-dotnet).  

   ```csharp
   BulkUpdateResponse bulkUpdateResponse = await bulkExecutor.BulkUpdateAsync(
     updateItems: updateItems,
     maxConcurrencyPerPartitionKeyRange: null,
     maxInMemorySortingBatchSize: null,
     cancellationToken: token);
   ```  
   **BulkUpdateAsync methode accepteert de volgende parameters:**

   |**Parameter**  |**Beschrijving** |
   |---------|---------|
   |maxConcurrencyPerPartitionKeyRange    |   De maximale mate van inbreuk op gelijktijdige per partitiesleutelbereik instelling op null veroorzaakt tapewisselaar standaardwaarde van 20.   |
   |maxInMemorySortingBatchSize    |    Het maximum aantal items van de update wordt opgehaald uit de enumerator voor update-items niet doorgegeven aan de API-aanroep in elk stadium in het geheugen vooraf verwerken sorteren fase vóór bulksgewijs bijwerken, veroorzaken instellen op null tapewisselaar standaardwaarde van min (updateItems.count, 1000000).     |
   | cancellationToken|Het token Annuleren om af te sluiten zonder problemen bulk-update. |

   **Bulk-update antwoord objectdefinitie** het resultaat van de API-aanroep van de bulk-update bevat de volgende kenmerken:

   |**Parameter**  |**Beschrijving** |
   |---------|---------|
   |NumberOfDocumentsUpdated (lang)    |   Het totale aantal documenten die zijn bijgewerkt buiten degene die is opgegeven voor de bulk-update API-aanroep.      |
   |TotalRequestUnitsConsumed (double)   |    Het totale aantal aanvraageenheden (RU) gebruikt door de bulk-update-API-aanroep.    |
   |TotalTimeTaken (TimeSpan)   | De totale tijd die het bulksgewijs bijwerken API-aanroep uitgevoerd. |
    
## <a name="performance-tips"></a>Tips voor prestaties 

Houd rekening met de volgende punten voor betere prestaties bij gebruik van BulkExecutor bibliotheek:

* Voer uw toepassing van een Azure virtuele machine die zich in dezelfde regio bevinden als uw regio Cosmos DB account schrijven voor de beste prestaties.  

* Wordt u aangeraden het instantiëren van een enkel object BulkExecutor voor de gehele toepassing binnen één virtuele machine overeenkomt met een specifieke verzameling van de Cosmos-DB.  

* Omdat de uitvoering van een één bulksgewijze bewerking API een grote hoeveelheid CPU en het netwerk-IO van de client-computer verbruikt. Dit gebeurt door het starten van meerdere taken intern, Vermijd meerdere gelijktijdige taken in uw toepassingsproces voor die elke uitvoering bulksgewijze bewerking API-aanroepen te starten. Als een enkel bulksgewijs bewerking API-aanroep uitgevoerd op een enkele virtuele machine niet kan gebruiken voor uw hele verzameling doorvoer (als uw verzameling doorvoer > 1 miljoen RU/s), de voorkeur boven het maken van afzonderlijke virtuele machines voor het gelijktijdig uitvoeren bulksgewijs bewerking API-aanroepen.  

* Zorg ervoor dat InitializeAsync() wordt aangeroepen nadat het instantiëren van een object op BulkExecutor om het ophalen van de doelmap Cosmos DB verzameling partitie.  

* Zorg er in uw toepassing App.Config **gcServer** is ingeschakeld voor betere prestaties
  ```xml  
  <runtime>
    <gcServer enabled="true" />
  </runtime>
  ```
* De bibliotheek verzendt traceringen dat kunnen worden verzameld in een logboekbestand of op de console. Voeg de volgende zodat beide App.Config van uw toepassing.

  ```xml
  <system.diagnostics>
    <trace autoflush="false" indentsize="4">
      <listeners>
        <add name="logListener" type="System.Diagnostics.TextWriterTraceListener" initializeData="application.log" />
        <add name="consoleListener" type="System.Diagnostics.ConsoleTraceListener" />
      </listeners>
    </trace>
  </system.diagnostics>
```

## <a name="next-steps"></a>Volgende stappen
* Zie voor meer informatie over de details van de Nuget-pakket en release-opmerkingen van BulkExecutor .net-bibliotheek,[BulkExecutor SDK details](sql-api-sdk-bulk-executor-dot-net.md). 
