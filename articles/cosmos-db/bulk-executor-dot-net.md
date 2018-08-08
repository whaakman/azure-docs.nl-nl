---
title: Met behulp van .NET-bibliotheek voor bulksgewijs executor bulksgewijs bewerkingen uitvoeren in Azure Cosmos DB | Microsoft Docs
description: Azure Cosmos DB bulksgewijs executor .NET-clientbibliotheek gebruiken voor het bulksgewijs importeren en bijwerken van documenten naar Azure Cosmos DB-containers.
keywords: .NET bulksgewijs executor
services: cosmos-db
author: tknandu
manager: kfile
ms.service: cosmos-db
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 05/07/2018
ms.author: ramkris
ms.openlocfilehash: 842ec8f641fca80e469864dd4d81e14c863fd464
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/07/2018
ms.locfileid: "39600054"
---
# <a name="use-bulk-executor-net-library-to-perform-bulk-operations-in-azure-cosmos-db"></a>Bulksgewijs executor .NET-clientbibliotheek gebruiken voor het bulksgewijs bewerkingen uitvoeren in Azure Cosmos DB

In deze zelfstudie bevat instructies over het gebruik van de Azure Cosmos DB bulksgewijs executor-bibliotheek voor .NET om te importeren en bijwerken van documenten naar Azure Cosmos DB-container. Zie voor meer informatie over het bulksgewijs executor-bibliotheek en hoe Hiermee kunt u gebruikmaken van de enorme doorvoer en opslag, [bulksgewijs executor-bibliotheek overzicht](bulk-executor-overview.md) artikel. In deze zelfstudie begeleidt u door een voorbeeld van .NET-toepassing die bulksgewijs invoer willekeurig gegenereerde documenten in een Azure Cosmos DB-container. Nadat u hebt geïmporteerd ziet u hoe u kunt bulksgewijs de geïmporteerde gegevens bijwerken met patches als bewerkingen moeten worden uitgevoerd op specifieke documentvelden op te geven.

## <a name="prerequisites"></a>Vereisten

* Als u Visual Studio 2017 geïnstalleerd nog niet hebt, kunt u downloaden en gebruiken de [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/). Zorg ervoor dat u tijdens de installatie van Visual Studio Azure-ontwikkeling inschakelt.

* Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) aan voordat u begint. 

* [Probeer Azure Cosmos DB gratis uit](https://azure.microsoft.com/try/cosmosdb/) zonder Azure-abonnement, zonder kosten en zonder verplichtingen. Of u kunt de [Azure Cosmos DB-Emulator](https://docs.microsoft.com/azure/cosmos-db/local-emulator) met de `https://localhost:8081` URI. De primaire sleutel wordt gegeven in [Aanvragen verifiëren](local-emulator.md#authenticating-requests).

* Een Azure Cosmos DB SQL API-account maken met behulp van de stappen in [-databaseaccount maken](create-sql-api-dotnet.md#create-a-database-account) gedeelte van het .NET-quickstart-artikel. 

## <a name="clone-the-sample-application"></a>De voorbeeldtoepassing klonen

Nu gaan we werken met code door sommige .NET-voorbeeldtoepassingen downloaden vanuit GitHub. Deze toepassingen bulksgewijs bewerkingen uitvoeren op Azure Cosmos DB-gegevens. Als u wilt klonen van de toepassingen, open een opdrachtprompt, Ga naar de map waar u de deze kopiëren en voer de volgende opdracht uit:

```
git clone https://github.com/Azure/azure-cosmosdb-bulkexecutor-dotnet-getting-started.git
```

De gekloonde opslagplaats bevat twee voorbeelden "BulkImportSample" en "BulkUpdateSample." U kunt openen van de voorbeeldtoepassingen, werk de verbindingsreeksen in App.config-bestand met uw Azure Cosmos DB-account met verbindingsreeksen, de oplossing bouwen en uitvoeren. 

De toepassing 'BulkImportSample' genereert willekeurige documenten en bulksgewijs importeert in Azure Cosmos DB. Het merendeel van de toepassing "BulkUpdateSample" werkt de geïmporteerde documenten door patches op te geven als bewerkingen moeten worden uitgevoerd op specifieke documentvelden. In de volgende secties controleert u de code in elk van deze voorbeeld-apps.

## <a name="bulk-import-data-to-azure-cosmos-db"></a>Bulksgewijs importeren van gegevens met Azure Cosmos DB

1. Navigeer naar de map 'BulkImportSample' en open het bestand 'BulkImportSample.sln'.  

2. De Azure Cosmos DB-verbindingsreeksen worden opgehaald uit het bestand App.config zoals wordt weergegeven in de volgende code:  

   ```csharp
   private static readonly string EndpointUrl = ConfigurationManager.AppSettings["EndPointUrl"];
   private static readonly string AuthorizationKey = ConfigurationManager.AppSettings["AuthorizationKey"];
   private static readonly string DatabaseName = ConfigurationManager.AppSettings["DatabaseName"];
   private static readonly string CollectionName = ConfigurationManager.AppSettings["CollectionName"];
   private static readonly int CollectionThroughput = int.Parse(ConfigurationManager.AppSettings["CollectionThroughput"]);
   ```

   Het importprogramma bulksgewijs maakt een nieuwe database en een verzameling met de databasenaam, de naam van verzameling en de doorvoer die zijn opgegeven in het bestand App.config. 

3. Naast is het DocumentClient-object geïnitialiseerd met de modus voor Direct via TCP-verbinding:  

   ```csharp
   ConnectionPolicy connectionPolicy = new ConnectionPolicy
   {
      ConnectionMode = ConnectionMode.Direct,
      ConnectionProtocol = Protocol.Tcp
   };
   DocumentClient client = new DocumentClient(new Uri(endpointUrl),authorizationKey,
   connectionPolicy)
   ```

4. Het object BulkExecutor wordt geïnitialiseerd met de waarden voor een hoge nieuwe pogingen voor de wachttijd en aanvragen beperkt. En vervolgens worden ze ingesteld op 0 overbezetting doorgeven aan BulkExecutor voor de levensduur.  

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

5. De toepassing roept de API BulkImportAsync. De .NET-bibliotheek biedt twee overloads van de bulksgewijs importeren API - naam die een lijst van seriële JSON-documenten accepteert en het andere accepteert een lijst met gedeserialiseerde POCO-documenten. Raadpleeg voor meer informatie over de definities van elk van deze overbelaste methoden, [API-documentatie](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmosdb.bulkexecutor.bulkexecutor.bulkimportasync?view=azure-dotnet).

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
   |enableUpsert    |   Een vlag om in te schakelen upsert van de documenten. Als een document met de opgegeven id bestaat al, wordt deze bijgewerkt. Standaard is het ingesteld op false.      |
   |disableAutomaticIdGeneration    |    Een vlag om uit te schakelen automatisch genereren van ID. Standaard is ingesteld op true.     |
   |maxConcurrencyPerPartitionKeyRange    | De maximale graad van gelijktijdigheid per partitiesleutelbereik instellen op null zorgt ervoor dat de bibliotheek te gebruiken een standaardwaarde van 20. |
   |maxInMemorySortingBatchSize     |  Het maximum aantal documenten die zijn opgehaald uit de document-enumerator die is doorgegeven aan de API aanroepen in elke fase.  Voor in het geheugen vooraf verwerken sorteren fase vóór bulkbewerkingen voor importeren, instellen op null zorgt ervoor dat bibliotheek standaardwaarde van min (documents.count, 1000000) te gebruiken.       |
   |cancellationToken    |    Het token Annuleren om af te sluiten zonder problemen bulkimport.     |

   **Bulksgewijs importeren antwoord objectdefinitie** het resultaat van de bulkimport API-aanroep bevat de volgende kenmerken:

   |**Parameter**  |**Beschrijving**  |
   |---------|---------|
   |NumberOfDocumentsImported (lang)   |  Het totale aantal documenten die zijn geïmporteerd uit de documenten die zijn opgegeven voor het bulksgewijs importeren API-aanroep.       |
   |TotalRequestUnitsConsumed (double)   |   Het totaal aantal aanvragen van aanvraageenheden (RU) die worden gebruikt door de bulksgewijs importeren API-aanroep.      |
   |TotalTimeTaken (TimeSpan)    |   De totale tijd die door de bulkimport API-aanroep uitgevoerd.      |
   |BadInputDocuments (lijst<object>)   |     De lijst met slechte indeling documenten die zijn niet geïmporteerd in de bulksgewijs importeren API-aanroep. Gebruiker moet de geretourneerde documenten los en probeer het opnieuw importeren. -Ongeldige indeling of meer documenten waarvan u de id-waarde geen tekenreeks is (null of een andere gegevenstype is als ongeldig wordt beschouwd).    |

## <a name="bulk-update-data-in-azure-cosmos-db"></a>Gegevens voor bulksgewijs bijwerken in Azure Cosmos DB

U kunt bestaande documenten met behulp van de API BulkUpdateAsync bijwerken. In dit voorbeeld wordt u het veld naam ingesteld op een nieuwe waarde en de beschrijving van veld verwijderen uit de bestaande documenten. Voor een volledig overzicht van ondersteunde veld updatebewerkingen, raadpleeg dan [API-documentatie](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmosdb.bulkexecutor.bulkupdate?view=azure-dotnet). 

1. Navigeer naar de map 'BulkUpdateSample' en open het bestand 'BulkUpdateSample.sln'.  

2. De update-items, samen met het bijbehorende veld updatebewerkingen definiëren. In dit voorbeeld gebruikt u SetUpdateOperation het veld naam en UnsetUpdateOperation het omschrijvingsveld verwijderen uit alle documenten bij te werken. U kunt ook andere bewerkingen zoals het verhogen van een veld van het document door een specifieke waarde uitvoeren, push-specifieke waarden in het matrixveld van een of een specifieke waarde verwijderen uit een matrixveld. Raadpleeg voor meer informatie over verschillende methoden die door de bulk-update API, de [API-documentatie](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmosdb.bulkexecutor.bulkupdate?view=azure-dotnet).

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
   |maxConcurrencyPerPartitionKeyRange    |   De maximale graad van gelijktijdigheid per partitiesleutelbereik instellen op null zorgt ervoor dat bibliotheek te gebruiken van de standaardwaarde van 20.   |
   |maxInMemorySortingBatchSize    |    Het maximum aantal update-items opgehaald uit de update-items enumerator doorgegeven aan de API-aanroep in elke fase voor in het geheugen vooraf verwerken sorteren fase vóór het bijwerken van bulksgewijs, instellen op null, zullen bibliotheek te gebruiken van de standaardwaarde van min (updateItems.count, 1000000).     |
   | cancellationToken|De annulering token om bulksgewijs bijwerken zonder problemen af te sluiten. |

   **Bulk-update antwoord objectdefinitie** het resultaat van de API-aanroep van de bulk-update bevat de volgende kenmerken:

   |**Parameter**  |**Beschrijving** |
   |---------|---------|
   |NumberOfDocumentsUpdated (lang)    |   Het totale aantal documenten die zijn bijgewerkt uit die is opgegeven voor de Bulkupdate-API-aanroep.      |
   |TotalRequestUnitsConsumed (double)   |    Het totale aantal aanvraageenheden (RU) die worden gebruikt door de bulk-update-API-aanroep.    |
   |TotalTimeTaken (TimeSpan)   | De totale tijd die door de bulksgewijs bijwerken API-aanroep uitgevoerd. |
    
## <a name="performance-tips"></a>Tips voor prestaties 

Houd rekening met de volgende punten voor betere prestaties bij het gebruik van grote hoeveelheden executor-bibliotheek:

* Voor optimale prestaties moet u de toepassing uitvoeren vanuit een Azure-machine die zich in dezelfde regio als de schrijfregio van uw Cosmos DB-account.  

* Het verdient aanbeveling om een één BulkExecutor-object voor de gehele toepassing binnen een enkele virtuele machine die overeenkomt met een specifieke Cosmos DB-container te maken.  

* Omdat de uitvoering van een enkele bulksgewijs bewerking API een grote hoeveelheid CPU- en IO van de client-computer verbruikt. Dit gebeurt door bij het maken van meerdere taken intern, te voorkomen dat bij het maken van meerdere gelijktijdige taken binnen uw het toepassingsproces dat elke uitvoering bulksgewijze bewerking API-aanroepen. Als een enkel bulksgewijs bewerking API-aanroep die wordt uitgevoerd op een enkele virtuele machine niet kan gebruiken voor uw hele containerdoorvoer (als uw container doorvoer > 1 miljoen RU/s), is het beter om te maken van afzonderlijke virtuele machines voor het gelijktijdig uitvoeren van bulksgewijs bewerking API-aanroepen.  

* Zorg ervoor dat InitializeAsync() wordt aangeroepen na een BulkExecutor instantiëren om op te halen van de partitiekaart doel Cosmos DB-container.  

* Zorg er in App.Config van uw toepassing, **gcServer** is ingeschakeld voor betere prestaties
  ```xml  
  <runtime>
    <gcServer enabled="true" />
  </runtime>
  ```
* De bibliotheek verzendt traceringen die kunnen worden verzameld in een logboekbestand of in de console. Om in te schakelen op beide, voegt u het volgende toe aan App.Config van uw toepassing.

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
* Zie voor meer informatie over de details van de Nuget-pakket en release-opmerkingen van bulksgewijs executor .net-bibliotheek,[bulksgewijs details over de SDK van de executor](sql-api-sdk-bulk-executor-dot-net.md). 
