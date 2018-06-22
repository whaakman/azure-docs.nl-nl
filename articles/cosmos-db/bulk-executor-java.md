---
title: Met bulksgewijs executor Java-bibliotheek voor het uitvoeren van bulkbewerkingen in Azure Cosmos DB | Microsoft Docs
description: Gebruik Azure Cosmos DB bulksgewijs executor Java-bibliotheek bulkimport en documenten bijwerken naar Azure Cosmos DB verzamelingen.
keywords: Java bulksgewijs executor
services: cosmos-db
author: tknandu
manager: kfile
ms.service: cosmos-db
ms.devlang: java
ms.topic: conceptual
ms.date: 05/07/2018
ms.author: ramkris
ms.openlocfilehash: f241a98cdcc847ddb579b86b51034d1438ee1395
ms.sourcegitcommit: ea5193f0729e85e2ddb11bb6d4516958510fd14c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/21/2018
ms.locfileid: "36300710"
---
# <a name="use-bulk-executor-java-library-to-perform-bulk-operations-on-azure-cosmos-db-data"></a>Bulksgewijs executor Java-bibliotheek gebruiken voor het uitvoeren van andere bulkbewerkingen op Azure DB die Cosmos-gegevens

Deze zelfstudie bevat instructies over het gebruik van de Azure Cosmos-DB bulksgewijs executor Java-bibliotheek te importeren en bijwerken van Azure DB die Cosmos-documenten. Zie voor meer informatie over bulksgewijs executor-bibliotheek en hoe kunt u gebruikmaken van grote doorvoer en opslag, [bulksgewijs executor bibliotheek overzicht](bulk-executor-overview.md) artikel. In deze zelfstudie maakt u een Java-toepassing bouwt die willekeurige documenten genereert en bulksgewijs geïmporteerd in een verzameling Azure Cosmos DB zijn. Nadat u hebt geïmporteerd, wordt u bulksgewijs bijwerken van sommige eigenschappen van een document. 

## <a name="prerequisites"></a>Vereisten

* Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) aan voordat u begint.  

* [Probeer Azure Cosmos DB gratis uit](https://azure.microsoft.com/try/cosmosdb/) zonder Azure-abonnement, zonder kosten en zonder verplichtingen. Of u kunt de [Azure Cosmos DB Emulator](https://docs.microsoft.com/azure/cosmos-db/local-emulator) met de `https://localhost:8081` URI. De primaire sleutel wordt gegeven in [Aanvragen verifiëren](local-emulator.md#authenticating-requests).  

* [Java Development Kit (JDK) 1.7+](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)  
  - Voer op Ubuntu `apt-get install default-jdk` uit om de JDK te installeren.  

  - Zorg dat de omgevingsvariabele JAVA_HOME verwijst naar de map waarin de JDK is geïnstalleerd.

* [Download](http://maven.apache.org/download.cgi) en [installeer](http://maven.apache.org/install.html) een binair [Maven](http://maven.apache.org/)-archief  
  
  - Op Ubuntu kunt u `apt-get install maven` uitvoeren om Maven te installeren.

* Een SQL-API van Azure Cosmos DB-account maken met behulp van de stappen in [-databaseaccount maken](create-sql-api-java.md#create-a-database-account) sectie van het artikel Java-Quick Start.

## <a name="clone-the-sample-application"></a>De voorbeeldtoepassing klonen

Nu gaan we overschakelen naar het werken met code door een voorbeeld van een Java-toepassing vanuit GitHub downloaden. Deze toepassing werkt bulkbewerkingen op Azure DB die Cosmos-gegevens. Klonen van de toepassing, open een opdrachtprompt, Ga naar de map waar u wilt kopiëren van de toepassing en voer de volgende opdracht:

```
 git clone https://github.com/Azure/azure-cosmosdb-bulkexecutor-java-getting-started.git 
```

De gekloonde opslagplaats bevat twee voorbeelden 'bulkimport' en 'bulkupdate' ten opzichte van de map '\azure-cosmosdb-bulkexecutor-java-getting-started\samples\bulkexecutor-sample\src\main\java\com\microsoft\azure\cosmosdb\bulkexecutor'. De toepassing 'bulkimport' willekeurige documenten genereert en importeert ze in Azure Cosmos DB. De toepassing 'bulkupdate' een aantal documenten in Azure DB die Cosmos-updates. In de volgende secties wordt we de code in elk van deze voorbeeld-apps te bekijken. 

## <a name="bulk-import-data-to-azure-cosmos-db"></a>Importeer gegevens bulksgewijs naar Azure Cosmos-DB

1. De Azure Cosmos-DB-verbindingsreeksen worden gelezen als argumenten en toegewezen aan variabelen die zijn gedefinieerd in CmdLineConfiguration.java-bestand.  

2. Naast is de DocumentClient-object geïnitialiseerd met behulp van de volgende instructies:  

   ```java
   ConnectionPolicy connectionPolicy = new ConnectionPolicy();
   connectionPolicy.setMaxPoolSize(1000);
   DocumentClient client = new DocumentClient(
      HOST,
      MASTER_KEY, 
      connectionPolicy,
      ConsistencyLevel.Session)
   ```

3. Het object DocumentBulkExecutor is geïnitialiseerd met een hoge opnieuw waarden voor de wachttijd en de snelheid van aanvragen. En vervolgens worden ze ingesteld op 0 overbezetting doorgeven aan DocumentBulkExecutor voor de levensduur.  

   ```java
   // Set client's retry options high for initialization
   client.getConnectionPolicy().getRetryOptions().setMaxRetryWaitTimeInSeconds(30);
   client.getConnectionPolicy().getRetryOptions().setMaxRetryAttemptsOnThrottledRequests(9);

   // Builder pattern
   Builder bulkExecutorBuilder = DocumentBulkExecutor.builder().from(
     client,
     DATABASE_NAME,
     COLLECTION_NAME,
     collection.getPartitionKey(),
     offerThroughput) // throughput you want to allocate for bulk import out of the collection's total throughput

   // Instantiate DocumentBulkExecutor
   DocumentBulkExecutor bulkExecutor = bulkExecutorBuilder.build()

   // Set retries to 0 to pass complete control to bulk executor
   client.getConnectionPolicy().getRetryOptions().setMaxRetryWaitTimeInSeconds(0);
   client.getConnectionPolicy().getRetryOptions().setMaxRetryAttemptsOnThrottledRequests(0);
```

4. Roep de importAll API die willekeurige documenten om te importeren in een verzameling Azure Cosmos DB bulksgewijs genereert. U kunt de opdrachtregel-configuraties in het bestand CmdLineConfiguration.java configureren.

   ```java
   BulkImportResponse bulkImportResponse = bulkExecutor.importAll(documents, false, true, null);
```
   De bulksgewijs import API accepteert een verzameling van documenten JSON-geserialiseerd en heeft de volgende syntaxis, voor meer informatie, Zie de [API-documentatie](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.bulkexecutor):

   ```java
   public BulkImportResponse importAll(
        Collection<String> documents,
        boolean isUpsert,
        boolean disableAutomaticIdGeneration,
        Integer maxConcurrencyPerPartitionRange) throws DocumentClientException;   
   ```

   De methode importAll accepteert de volgende parameters:
 
   |**Parameter**  |**Beschrijving**  |
   |---------|---------|
   |isUpsert    |   Een markering om in te schakelen upsert van de documenten. Als een document met opgegeven ID bestaat al, wordt bijgewerkt.  |
   |disableAutomaticIdGeneration     |   Een markering automatisch genereren van id uitschakelen Standaard is ingesteld op true.   |
   |maxConcurrencyPerPartitionRange    |  De maximale mate van inbreuk op gelijktijdige per partitiesleutelbereik. De standaardwaarde is 20.  |

   **Bulksgewijs importeren antwoord objectdefinitie** het resultaat van de bulkimport API-aanroep heeft de volgende get-methoden:

   |**Parameter**  |**Beschrijving**  |
   |---------|---------|
   |int getNumberOfDocumentsImported()  |   Het totale aantal documenten die zijn geïmporteerd uit de opgegeven voor het bulksgewijs documenten importeren API-aanroep.      |
   |dubbele getTotalRequestUnitsConsumed()   |  De totale aanvraageenheden (RU) gebruikt door de bulksgewijs importeren API-aanroep.       |
   |Duur getTotalTimeTaken()   |    De totale tijd die door de bulkimport API-aanroep uitgevoerd.     |
   |Lijst<Exception> getErrors() |  Haalt de lijst met fouten als bepaalde documenten buiten de opgegeven voor de bulkinschrijving batch importeert API-aanroep kan niet worden ingevoegd ophalen.       |
   |Lijst<Object> getBadInputDocuments()  |    De lijst met onjuiste indeling documenten die zijn niet geïmporteerd in de meeste importeren API-aanroep. Gebruiker moet los van de documenten die worden geretourneerd en probeer het opnieuw importeren. Onjuiste indeling of meer documenten waarvan id-waarde geen tekenreeks is (null of een andere datatype wordt beschouwd als ongeldig).     |

5. Nadat u de toepassing gereed importeren bulksgewijs hebt, maken het opdrachtregelprogramma van bron met de opdracht 'schone pakket mvn'. Met deze opdracht genereert u een jar-bestand in de doelmap:  

   ```java
   mvn clean package
   ```

6. Nadat de doel-afhankelijkheden zijn gegenereerd, kunt u de toepassing van de importfunctie bulksgewijs kunt aanroepen met behulp van de volgende opdracht:  

   ```java
   java -Xmx12G -jar bulkexecutor-sample-1.0-SNAPSHOT-jar-with-dependencies.jar -serviceEndpoint *<Fill in your Azure Cosmos DB’s endpoint URI>*  -masterKey *<Fill in your Azure Cosmos DB’s master key>* -databaseId bulkImportDb -collectionId bulkImportColl -operation import -shouldCreateCollection -collectionThroughput 1000000 -partitionKey /profileid -maxConnectionPoolSize 6000 -numberOfDocumentsForEachCheckpoint 1000000 -numberOfCheckpoints 10
   ```

   De importfunctie bulksgewijs maakt een nieuwe database en een verzameling met de databasenaam, de verzamelingsnaam en de doorvoer waarden die zijn opgegeven in het bestand App.config. 

## <a name="bulk-update-data-in-azure-cosmos-db"></a>Gegevens voor bulksgewijs bijwerken in Azure Cosmos-DB

U kunt bestaande documenten met behulp van de API BulkUpdateAsync bijwerken. In dit voorbeeld wordt u het veld naam ingesteld op een nieuwe waarde en beschrijvingsveld verwijderen uit de bestaande documenten. Voor de volledige lijst met ondersteunde veld updatebewerkingen, Zie [API-documentatie](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.bulkexecutor). 

1. Hiermee definieert u de update-items, samen met de bijbehorende veld update-bewerkingen. In dit voorbeeld gebruikt u SetUpdateOperation het naamveld en UnsetUpdateOperation het beschrijvingsveld verwijderen uit alle documenten bij te werken. U kunt ook andere bewerkingen zoals verhoging een documentveld uitvoeren door een specifieke waarde, push-specifieke waarden in het matrixveld van een of een specifieke waarde verwijderen uit een array-veld. Zie voor meer informatie over verschillende methoden die worden geleverd door de bulk-update API, de [API-documentatie](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.bulkexecutor).  

   ```java
   SetUpdateOperation<String> nameUpdate = new SetUpdateOperation<>("Name","UpdatedDocValue");
   UnsetUpdateOperation descriptionUpdate = new UnsetUpdateOperation("description");

   ArrayList<UpdateOperationBase> updateOperations = new ArrayList<>();
   updateOperations.add(nameUpdate);
   updateOperations.add(descriptionUpdate);

   List<UpdateItem> updateItems = new ArrayList<>(cfg.getNumberOfDocumentsForEachCheckpoint());
   IntStream.range(0, cfg.getNumberOfDocumentsForEachCheckpoint()).mapToObj(j -> {                      
    return new UpdateItem(Long.toString(prefix + j), Long.toString(prefix + j), updateOperations);
    }).collect(Collectors.toCollection(() -> updateItems));
   ```

2. Roep de updateAll API die willekeurige documenten om te worden vervolgens geïmporteerd in een verzameling Azure Cosmos DB bulksgewijs genereert. U kunt de opdrachtregelprogramma configuraties in CmdLineConfiguration.java bestand moet worden doorgegeven.

   ```java
   BulkUpdateResponse bulkUpdateResponse = bulkExecutor.updateAll(updateItems, null)
   ```

   De bulk-update API accepteert een verzameling items worden bijgewerkt. Elk item update geeft de lijst met veld update-bewerkingen worden uitgevoerd op een document aangeduid met een ID en een waarde voor de partitiesleutel. Zie voor meer informatie de [API-documentatie](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb.bulkexecutor):

   ```java
   public BulkUpdateResponse updateAll(
        Collection<UpdateItem> updateItems,
        Integer maxConcurrencyPerPartitionRange) throws DocumentClientException;
   ```

   De methode updateAll accepteert de volgende parameters:

   |**Parameter** |**Beschrijving** |
   |---------|---------|
   |maxConcurrencyPerPartitionRange   |  De maximale mate van inbreuk op gelijktijdige per partitiesleutelbereik. De standaardwaarde is 20.  |
 
   **Bulksgewijs importeren antwoord objectdefinitie** het resultaat van de bulkimport API-aanroep heeft de volgende get-methoden:

   |**Parameter** |**Beschrijving**  |
   |---------|---------|
   |int getNumberOfDocumentsUpdated()  |   Het totale aantal documenten die zijn bijgewerkt buiten de documenten die is opgegeven voor de bulk-update-API-aanroep.      |
   |dubbele getTotalRequestUnitsConsumed() |  Het totale aantal aanvraageenheden (RU) gebruikt door de bulk-update-API-aanroep.       |
   |Duur getTotalTimeTaken()  |   De totale tijd die het bulksgewijs bijwerken API-aanroep uitgevoerd.      |
   |Lijst<Exception> getErrors()   |     Hiermee haalt de lijst van fouten als bepaalde documenten buiten de batch is opgegeven voor de bulk-update API-aanroep kan niet worden ingevoegd ophalen.      |

3. Nadat u het bijwerken van de toepassing gereed bulksgewijs hebt, maken het opdrachtregelprogramma van bron met de opdracht 'schone pakket mvn'. Met deze opdracht genereert u een jar-bestand in de doelmap:  

   ```
   mvn clean package
   ```

4. Nadat de doel-afhankelijkheden zijn gegenereerd, kunt u de toepassing van de update bulksgewijs aanroept met behulp van de volgende opdracht:

   ```
   java -Xmx12G -jar bulkexecutor-sample-1.0-SNAPSHOT-jar-with-dependencies.jar -serviceEndpoint **<Fill in your Azure Cosmos DB’s endpoint URI>* -masterKey **<Fill in your Azure Cosmos DB’s master key>* -databaseId bulkUpdateDb -collectionId bulkUpdateColl -operation update -collectionThroughput 1000000 -partitionKey /profileid -maxConnectionPoolSize 6000 -numberOfDocumentsForEachCheckpoint 1000000 -numberOfCheckpoints 10
   ```

## <a name="performance-tips"></a>Tips voor prestaties 

Houd rekening met de volgende punten voor betere prestaties bij gebruik van bulksgewijs executor bibliotheek:

* Voer uw toepassing van een virtuele machine van Azure in dezelfde regio bevinden als uw regio Cosmos DB account schrijven voor de beste prestaties.  
* Voor hogere doorvoer kan bereiken:  

   * De JVM heap-grootte ingesteld op een aantal groot genoeg zijn om te voorkomen dat een geheugen probleem bij het verwerken van grote aantallen documenten. Voorstel voor de heap-grootte: max (3GB, 3 * (alle documenten die zijn doorgegeven aan bulksgewijs importeren API in één batch) sizeof).  
   * Er is een voorverwerking tijd, vanwege die ontvangt u een hogere doorvoer bij het uitvoeren van andere bulkbewerkingen met een groot aantal documenten. Als u importeren 10.000.000 documenten wilt, is waarop bulkimport 10 keer 10 bulksgewijs documenten elk van de grootte van 1.000.000 beter dan het uitvoeren van bulkimport 100 keer op 100 bulksgewijs documenten elk van de grootte van 100.000 documenten.  

* Het verdient het instantiëren van een enkel object DocumentBulkExecutor voor de gehele toepassing binnen een enkele virtuele machine die overeenkomt met een specifieke Azure DB die Cosmos-verzameling.  

* Omdat de uitvoering van een één bulksgewijze bewerking API een grote hoeveelheid CPU en het netwerk-IO van de client-computer verbruikt. Dit gebeurt door het starten van meerdere taken intern, Vermijd meerdere gelijktijdige taken in uw toepassingsproces voor die elke uitvoering bulksgewijze bewerking API-aanroepen te starten. Als een enkel bulksgewijs bewerking API-aanroep uitgevoerd op een enkele virtuele machine niet kan gebruiken voor uw hele verzameling doorvoer (als uw verzameling doorvoer > 1 miljoen RU/s), is het raadzaam om het maken van afzonderlijke virtuele machines voor het gelijktijdig uitvoeren bulksgewijs bewerking API-aanroepen.

    
## <a name="next-steps"></a>Volgende stappen
* Zie voor meer informatie over de pakketgegevens maven en release-opmerkingen van bulksgewijs executor Java-bibliotheek,[bulksgewijs executor SDK details](sql-api-sdk-bulk-executor-java.md).


