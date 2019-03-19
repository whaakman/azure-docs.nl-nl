---
title: Lokaal ontwikkelen met de Azure Cosmos-Emulator
description: Met behulp van de Azure Cosmos-emulator gebruikt, kunt u ontwikkelen en testen van uw toepassing lokaal voor gratis, zonder dat het maken van een Azure-abonnement.
ms.service: cosmos-db
ms.topic: tutorial
ms.date: 04/20/2018
author: deborahc
ms.author: dech
ms.openlocfilehash: 0adb24458f718511c7134fc3bf36dd0b03173e30
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58011508"
---
# <a name="use-the-azure-cosmos-emulator-for-local-development-and-testing"></a>Gebruik de Azure Cosmos-emulator gebruikt voor lokale ontwikkeling en testen

De Azure Cosmos-Emulator biedt een lokale omgeving waarin de Azure Cosmos DB-service voor ontwikkelingsdoeleinden worden geëmuleerd. Met behulp van de Azure Cosmos-emulator gebruikt, kunt u ontwikkelen en testen van uw toepassing lokaal, zonder dat het maken van een Azure-abonnement of er kosten in rekening. Wanneer u tevreden bent over hoe uw toepassing in de Azure Cosmos-emulator gebruikt werkt, kunt u overschakelen naar een Azure Cosmos-account in de cloud.

U kunt ontwikkelen met behulp van Azure Cosmos-Emulator met [SQL](local-emulator.md#sql-api), [Cassandra](local-emulator.md#cassandra-api), [MongoDB](local-emulator.md#azure-cosmos-dbs-api-for-mongodb), [Gremlin](local-emulator.md#gremlin-api), en [tabel](local-emulator.md#table-api) API-accounts. Maar op dit moment de Data Explorer-weergave in de emulator biedt volledige ondersteuning clients voor SQL-API alleen. 

## <a name="how-the-emulator-works"></a>Hoe de emulator werkt

De Azure Cosmos Emulator biedt een uiterst betrouwbare en emulatie van de Azure Cosmos DB-service. Het ondersteunt dezelfde functionaliteit als Azure Cosmos DB, inclusief ondersteuning voor het maken en opvragen van gegevens, inrichting en containers schalen en het uitvoeren van opgeslagen procedures en triggers. U kunt ontwikkelen en testen van toepassingen met behulp van de Azure Cosmos-Emulator en implementeer deze in Azure op wereldwijde schaal door het maken van een configuratie voor één wijzigen in het verbindingseindpunt voor Azure Cosmos DB.

Emulatie van de Azure Cosmos DB-service is zeer betrouwbaar, maar de implementatie van de emulator is anders dan die van de service. De emulator gebruikt bijvoorbeeld standaardbesturingssysteemonderdelen, zoals het lokale bestandssysteem voor persistentie en de HTTPS-protocolstack voor connectiviteit. De functionaliteit die is gebaseerd op Azure-infrastructuur, zoals globale replicatie, de latentie van slechts enkele milliseconden voor lees-/ schrijfbewerkingen en instelbare consistentieniveaus zijn niet van toepassing.

U kunt gegevens tussen de Azure Cosmos-Emulator en de service Azure Cosmos DB migreren met behulp van de [hulpprogramma voor migratie van Azure Cosmos DB Data](https://github.com/azure/azure-documentdb-datamigrationtool).

U kunt Azure Cosmos-Emulator moet uitvoeren op de Windows-Docker-container, Zie de [Docker Hub](https://hub.docker.com/r/microsoft/azure-cosmosdb-emulator/) voor de docker pull-opdracht en [GitHub](https://github.com/Azure/azure-cosmos-db-emulator-docker) voor de broncode van de emulator.

## <a name="differences-between-the-emulator-and-the-service"></a>Verschillen tussen de emulator en de service

Omdat de Azure Cosmos Emulator een geëmuleerde omgeving die wordt uitgevoerd op de lokale ontwikkelwerkstation biedt, zijn er enkele verschillen in functionaliteit tussen de emulator en een Azure Cosmos-account in de cloud:

* Data Explorer in de emulator ondersteunt momenteel clients voor SQL-API. De weergave van de Data Explorer en de bewerkingen voor Azure Cosmos DB-API's, zoals MongoDB-, tabel-, grafiek- en Cassandra-API's zijn niet volledig ondersteund.
* De Azure Cosmos Emulator ondersteunt slechts één vast account en een bekende hoofdsleutel. Sleutel opnieuw genereren is niet mogelijk in de Azure Cosmos-Emulator, maar de standaardsleutel kan worden gewijzigd met behulp van de opdrachtregeloptie te gebruiken.
* De Azure Cosmos-Emulator is niet een schaalbare service en wordt geen ondersteuning voor een groot aantal containers.
* De Azure Cosmos-Opslagemulator biedt geen andere [Azure Cosmos DB-consistentieniveaus](consistency-levels.md).
* De Azure Cosmos-Opslagemulator biedt geen [replicatie voor meerdere regio's](distribute-data-globally.md).
* Als uw exemplaar van de Azure Cosmos Emulator mogelijk niet altijd up-to-date zijn met de meest recente wijzigingen in de Azure Cosmos DB-service, raadpleegt u de [Azure Cosmos DB-Capaciteitsplanner](https://www.documentdb.com/capacityplanner) nauwkeurig de productie te schatten (ru's) de doorvoerbehoeften van uw toepassing.
* Wanneer u de Azure Cosmos-Emulator gebruikt standaard, kunt u maximaal 25 vaste grootte containers (alleen ondersteund met behulp van Azure Cosmos DB SDK's) of 5 onbeperkte containers met behulp van de Azure Cosmos-Emulator. Zie [De PartitionCount-waarde instellen](#set-partitioncount) voor meer informatie over het wijzigen van deze waarde.

## <a name="system-requirements"></a>Systeemvereisten

De Azure Cosmos Emulator heeft de volgende hardware- en softwarevereisten:

* Softwarevereisten
  * Windows Server 2012 R2, Windows Server 2016 of Windows 10
  * 64-bits besturingssysteem
* Minimale hardwarevereisten
  * 2 GB RAM
  * 10 GB beschikbare schijfruimte

## <a name="installation"></a>Installatie

U kunt downloaden en installeren van de Azure Cosmos-Emulator van de [Microsoft Download Center](https://aka.ms/cosmosdb-emulator) of u de emulator in Docker voor Windows kunt uitvoeren. Zie [Uitvoeren op Docker](#running-on-docker) voor instructies over het gebruik van de emulator op Docker voor Windows.

> [!NOTE]
> Als u wilt installeren, configureren en uitvoeren van de Azure Cosmos-emulator gebruikt, moet u beheerdersrechten hebben op de computer. De emulator wordt een certificaat maken/Voeg toe en stel ook de firewall-regels om uit te voeren van de services; Daarom is het nodig zijn voor de emulator kunnen deze bewerkingen uitvoert.

## <a name="running-on-windows"></a>Uitvoeren in Windows

Selecteer de knop Start voor het starten van de Azure Cosmos-emulator gebruikt, of druk op de Windows-toets. Begin met typen **Azure Cosmos-Emulator**, en selecteert u de emulator in de lijst met toepassingen.

![Selecteer de knop Start of druk op de Windows-toets, begint te typen ** Azure Cosmos-Emulator ** en selecteer de emulator van de lijst met toepassingen](./media/local-emulator/database-local-emulator-start.png)

Wanneer de emulator wordt gestart, ziet u een pictogram in het systeemvak op de taakbalk van Windows. ![Azure Cosmos DB lokale emulator taak balk-melding](./media/local-emulator/database-local-emulator-taskbar.png)

De Azure Cosmos-Emulator standaard wordt uitgevoerd op de lokale computer ("localhost") op poort 8081 luistert.

De Azure Cosmos-Emulator is geïnstalleerd op `C:\Program Files\Azure Cosmos DB Emulator` standaard. U kunt de emulator ook starten en stoppen vanaf de opdrachtregel. Zie de [informatie over het opdrachtregelprogramma](#command-line) voor meer informatie.

## <a name="start-data-explorer"></a>Data Explorer starten

Als de Azure Cosmos-Emulator wordt gestart, worden automatisch de Azure Cosmos-Data Explorer in uw browser geopend. Het adres wordt weergegeven als `https://localhost:8081/_explorer/index.html`. Als u de Verkenner sluiten en wilt het later opnieuw opent, kunt u de URL in uw browser te openen of starten vanaf de Azure Cosmos-Emulator in het pictogram in systeemvak voor Windows, zoals hieronder wordt weergegeven.

![Azure Cosmos lokale emulator data explorer starten](./media/local-emulator/database-local-emulator-data-explorer-launcher.png)

## <a name="checking-for-updates"></a>Controleren op updates

Data Explorer geeft aan of er een nieuwe update beschikbaar is om te downloaden.

> [!NOTE]
> Gegevens die zijn gemaakt in een versie van de Azure Cosmos-Emulator (Zie %LOCALAPPDATA%\CosmosDBEmulator of gegevens pad optionele instellingen) is niet noodzakelijkerwijs toegankelijk wanneer u een andere versie. Als u nodig hebt om vast te leggen van de gegevens voor de lange termijn, verdient het aanbeveling die gegevens op te slaan in een Azure Cosmos-account, in plaats van in de Azure Cosmos-Emulator.

## <a name="authenticating-requests"></a>Aanvragen verifiëren

Als met Azure Cosmos DB in de cloud, moet elke aanvraag die u op basis van de Azure Cosmos-Emulator maakt worden geverifieerd. De Emulator van Azure Cosmos ondersteunt één vast account en een bekende verificatiesleutel voor de hoofdsleutel van de verificatie. Dit account en de sleutel zijn de enige referenties zijn toegestaan voor gebruik met de Azure Cosmos-Emulator. Dit zijn:

```bash
Account name: localhost:<port>
Account key: C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==
```

> [!NOTE]
> De hoofdsleutel wordt ondersteund door de Azure Cosmos-Emulator is bedoeld voor gebruik met de emulator. U niet uw Azure Cosmos DB-account voor productie en -sleutel met de Azure Cosmos-Emulator gebruiken.

> [!NOTE]
> Als u de emulator hebt gestart met de optie/Key, gebruikt u de gegenereerde sleutel in plaats van `C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==`. Zie voor meer informatie over de optie/Key [opdrachtregelprogramma.](#command-line-syntax)

Als met de Azure Cosmos DB, Azure Cosmos-Emulator biedt ondersteuning voor alleen beveiligde communicatie via SSL.

## <a name="running-on-a-local-network"></a>Uitvoeren op een lokaal netwerk

U kunt de emulator uitvoeren op een lokaal netwerk. Om in te schakelen toegang tot het netwerk, geef de `/AllowNetworkAccess` optie om de [opdrachtregel](#command-line-syntax), die ook vereist dat u opgeeft `/Key=key_string` of `/KeyFile=file_name`. U kunt `/GenKeyFile=file_name` voor het genereren van een bestand met een willekeurige sleutel kosten vooraf. Vervolgens kunt u die u wilt doorgeven `/KeyFile=file_name` of `/Key=contents_of_file`.

Als u wilt toegang tot het netwerk inschakelen voor het eerst moet de gebruiker de emulator uitschakelen en verwijderen van map van de gegevens van de emulator (% LOCALAPPDATA%\CosmosDBEmulator).

## <a name="developing-with-the-emulator"></a>Ontwikkelen met de emulator

### <a name="sql-api"></a>SQL-API

Zodra u de Azure Cosmos-Emulator uitgevoerd op uw bureaublad hebt, kunt u een ondersteund [Azure Cosmos DB SDK](sql-api-sdk-dotnet.md) of de [REST-API van Azure Cosmos DB](/rest/api/cosmos-db/) om te communiceren met de emulator. De Azure Cosmos Emulator bevat ook een ingebouwde Data Explorer waarmee u containers maken voor SQL-API of Cosmos DB voor Mongo DB API en een weergeven en bewerken van items zonder een code te schrijven.

```csharp
// Connect to the Azure Cosmos Emulator running locally
DocumentClient client = new DocumentClient(
   new Uri("https://localhost:8081"), "C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==");

```

### <a name="azure-cosmos-dbs-api-for-mongodb"></a>Azure Cosmos DB-API voor MongoDB

Als u [Azure Cosmos DB voor MongoDB](mongodb-introduction.md), gebruikt u de volgende verbindingsreeks:

```bash
mongodb://localhost:C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==@localhost:10255/admin?ssl=true
```

### <a name="table-api"></a>Tabel-API

Zodra u de Azure Cosmos-Emulator uitgevoerd op uw bureaublad hebt, kunt u de [Azure Cosmos DB tabel-API SDK](table-storage-how-to-use-dotnet.md) om te communiceren met de emulator. Emulator start vanaf een opdrachtprompt als beheerder met ' / EnableTableEndpoint '. Voer vervolgens de volgende code om verbinding maken met de tabel-API-account:

```csharp
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Table;
using CloudTable = Microsoft.WindowsAzure.Storage.Table.CloudTable;
using CloudTableClient = Microsoft.WindowsAzure.Storage.Table.CloudTableClient;

string connectionString = "DefaultEndpointsProtocol=http;AccountName=localhost;AccountKey=C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==;TableEndpoint=http://localhost:8902/;";

CloudStorageAccount account = CloudStorageAccount.Parse(connectionString);
CloudTableClient tableClient = account.CreateCloudTableClient();
CloudTable table = tableClient.GetTableReference("testtable");
table.CreateIfNotExists();
table.Execute(TableOperation.Insert(new DynamicTableEntity("partitionKey", "rowKey")));
```

### <a name="cassandra-api"></a>Cassandra-API

Emulator starten vanaf een opdrachtprompt als administrator met ' / EnableCassandraEndpoint '. U kunt ook kunt u ook de omgevingsvariabele instellen `AZURE_COSMOS_EMULATOR_CASSANDRA_ENDPOINT=true`.

* [Installeer Python 2.7](https://www.python.org/downloads/release/python-2716/)

* [Install Cassandra CLI/CQLSH](http://cassandra.apache.org/download/)

* Voer de volgende opdrachten in een normale opdrachtprompt-venster:

  ```bash
  set Path=c:\Python27;%Path%
  cd /d C:\sdk\apache-cassandra-3.11.3\bin
  set SSL_VERSION=TLSv1_2
  set SSL_VALIDATE=false
  cqlsh localhost 10350 -u localhost -p C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw== --ssl
  ```

* Voer de volgende opdrachten voor het verbinding maken met de Cassandra-eindpunt in de shell CQLSH:

  ```bash
  CREATE KEYSPACE MyKeySpace WITH replication = {'class':'MyClass', 'replication_factor': 1};
  DESCRIBE keyspaces;
  USE mykeyspace;
  CREATE table table1(my_id int PRIMARY KEY, my_name text, my_desc text);
  INSERT into table1 (my_id, my_name, my_desc) values( 1, 'name1', 'description 1');
  SELECT * from table1;
  EXIT
  ```

### <a name="gremlin-api"></a>Gremlin-API

Emulator starten vanaf een opdrachtprompt als administrator met ' / EnableGremlinEndpoint '. U kunt ook kunt u ook de omgevingsvariabele instellen `AZURE_COSMOS_EMULATOR_GREMLIN_ENDPOINT=true`

* [Apache-tinkerpop-gremlin-console-3.3.4 installeren](http://tinkerpop.apache.org/downloads.html)

* In de emulator van Data Explorer maakt u een database 'db1' en een verzameling "coll1"; Kies voor de partitiesleutel ' / naam '

* Voer de volgende opdrachten in een normale opdrachtprompt-venster:

  ```bash
  cd /d C:\sdk\apache-tinkerpop-gremlin-console-3.3.4-bin\apache-tinkerpop-gremlin-console-3.3.4
  
  copy /y conf\remote.yaml conf\remote-localcompute.yaml
  notepad.exe conf\remote-localcompute.yaml
    hosts: [localhost]
    port: 8901
    username: /dbs/db1/colls/coll1
    password: C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==
    connectionPool: {
    enableSsl: false}
    serializer: { className: org.apache.tinkerpop.gremlin.driver.ser.GraphSONMessageSerializerV1d0,
    config: { serializeResultToString: true  }}

  bin\gremlin.bat
  ```

* Voer de volgende opdrachten voor het verbinding maken met de Gremlin-eindpunt in de Gremlin-shell:

  ```bash
  :remote connect tinkerpop.server conf/remote-localcompute.yaml
  :remote console
  :> g.V()
  :> g.addV('person1').property(id, '1').property('name', 'somename1')
  :> g.addV('person2').property(id, '2').property('name', 'somename2')
  :> g.V()
  ```

## <a name="export-the-ssl-certificate"></a>Het SSL-certificaat exporteren

.NET-talen en -runtime maken gebruik van het Windows-certificaatarchief om veilig verbinding te maken met de lokale Azure DB Cosmos Emulator. Andere talen hebben hun eigen methode voor het beheren en gebruiken van certificaten. Java maakt gebruik van een eigen [certificaatarchief](https://docs.oracle.com/cd/E19830-01/819-4712/ablqw/index.html), terwijl Python gebruikmaakt van [socketwrappers](https://docs.python.org/2/library/ssl.html).

Om een certificaat te verkrijgen voor gebruik bij talen en runtimes die niet worden geïntegreerd in het certificaatarchief van Windows, moet u het exporteren met behulp van de Windows Certificate Manager. U kunt starten door het uitvoeren van certlm.msc of volg de stapsgewijze instructies in [de Azure Cosmos-Emulatorcertificaten exporteren](./local-emulator-export-ssl-certificates.md). Zodra de certificaatmanager wordt uitgevoerd, opent u de persoonlijke certificaten zoals hieronder weergegeven en exporteert u het certificaat met de beschrijvende naam 'DocumentDBEmulatorCertificate' als een BASE 64-gecodeerd X.509-bestand (.cer).

![SSL-certificaat voor een lokale Azure Cosmos DB Emulator](./media/local-emulator/database-local-emulator-ssl_certificate.png)

Volg de instructies in [Een certificaat toevoegen aan het Java CA-certificaatarchief](https://docs.microsoft.com/azure/java-add-certificate-ca-store) om het X.509-certificaat te importeren in het Java-certificaatarchief. Zodra het certificaat is geïmporteerd in het certificaatarchief, worden clients voor SQL en Azure Cosmos DB-API voor MongoDB geen verbinding maken met de Azure Cosmos-Emulator.

Bij het verbinden met de emulator van Python en Node.js SDK's, is SSL-verificatie uitgeschakeld.

## <a id="command-line"></a>Opdrachtregelprogramma
Vanaf de installatielocatie, kunt u de opdrachtregel om te starten en stoppen van de emulator, opties configureren en andere bewerkingen uitvoeren.

### <a name="command-line-syntax"></a>De syntaxis van opdrachtregel

    CosmosDB.Emulator.exe [/Shutdown] [/DataPath] [/Port] [/MongoPort] [/DirectPorts] [/Key] [/EnableRateLimiting] [/DisableRateLimiting] [/NoUI] [/NoExplorer] [/?]

Typ `CosmosDB.Emulator.exe /?` bij de opdrachtprompt om een lijst met opties te zien.

|**Optie** | **Beschrijving** | **Opdracht**| **Argumenten**|
|---|---|---|---|
|[Geen argumenten] | De Azure Cosmos-Emulator met standaardinstellingen wordt gestart. |CosmosDB.Emulator.exe| |
|[Help] |Toont de lijst met ondersteunde opdrachtregelargumenten.|CosmosDB.Emulator.exe /? | |
| GetStatus |Hiermee haalt u de status van de Azure Cosmos-Emulator. De status wordt aangegeven door de afsluitcode: 1 = starten, 2 = wordt uitgevoerd, 3 = gestopt. Een negatieve afsluitcode geeft aan dat er een fout is opgetreden. Er wordt geen andere uitvoer geproduceerd. | CosmosDB.Emulator.exe /GetStatus| |
| Afsluiten| De Azure Cosmos-Emulator wordt afgesloten.| CosmosDB.Emulator.exe /Shutdown | |
|DataPath | Specificeert het pad waarin de gegevensbestanden worden opgeslagen. Standaardwaarde is % LocalAppdata%\CosmosDBEmulator. | CosmosDB.Emulator.exe /DataPath=\<gegevenspad\> | \<gegevenspad\>: Een toegankelijk pad |
|Poort | Specificeert het poortnummer dat moet worden gebruikt voor de emulator. Standaardwaarde is 8081. |CosmosDB.Emulator.exe /Port=\<poort\> | \<poort\>: Enkel poortnummer |
| MongoPort | Specificeert het poortnummer dat moet worden gebruikt MongoDB compatibiliteit-API. Standaardwaarde is 10255. |CosmosDB.Emulator.exe /MongoPort= \<mongoport\>|\<mongopoort\>: Enkel poortnummer|
| CassandraPort | Hiermee geeft u het poortnummer dat moet worden gebruikt voor het Cassandra-eindpunt. Standaardwaarde is 10350. | CosmosDB.Emulator.exe /CassandraPort = \<cassandraport\> | \<cassandraport\>: Enkel poortnummer |
| ComputePort | Het poortnummer dat moet worden gebruikt voor de Compute Interop-gatewayservice opgegeven. Poort test HTTP-eindpunt van de Gateway wordt berekend als ComputePort + 79. Daarom kan moet ComputePort en ComputePort + 79 open zijn en beschikbaar. De standaardwaarden zijn 8900, 8979. | CosmosDB.Emulator.exe /ComputePort = \<computeport\> | \<computeport\>: Enkel poortnummer |
| EnableCassandraEndpoint | Kan de Cassandra-API | CosmosDB.Emulator.exe /EnableCassandraEndpoint | |
| EnableGremlinEndpoint | Kan de Gremlin-API | CosmosDB.Emulator.exe /EnableGremlinEndpoint | |
| GremlinPort | Het poortnummer moet worden gebruikt voor de Gremlin-eindpunt. Standaardwaarde is 8901. | CosmosDB.Emulator.exe /GremlinPort =\<poort\> | \<poort\>: Enkel poortnummer |
|TablePort | Het poortnummer moet worden gebruikt voor het eindpunt van de Azure-tabel. Standaardwaarde is 8902. | CosmosDB.Emulator.exe /TablePort=\<port\> | \<poort\>: Enkel poortnummer|
| KeyFile | Autorisatiesleutel lezen uit het opgegeven bestand. De optie /GenKeyFile gebruiken voor het genereren van een sleutelbestand | CosmosDB.Emulator.exe/KEYFILE =\<bestandsnaam\> | \<bestandsnaam\>: Pad naar het bestand |
| ResetDataPath | Recursief Hiermee verwijdert u alle bestanden in het opgegeven pad. Als u geen pad opgeeft, wordt de standaardwaarde %LOCALAPPDATA%\CosmosDbEmulator | CosmosDB.Emulator.exe /ResetDataPath [=<path>] | \<Pad\>: Pad naar bestand  |
| StartTraces  |  Begin met het verzamelen van Logboeken voor foutopsporing tracering. | CosmosDB.Emulator.exe /StartTraces | |
| StopTraces     | Stoppen met het verzamelen van Logboeken voor foutopsporing tracering. | CosmosDB.Emulator.exe /StopTraces  | |
|EnableTableEndpoint | Hiermee kunt Azure Table-API | CosmosDB.Emulator.exe /EnableTableEndpoint | |
|FailOnSslCertificateNameMismatch | Standaard de Emulator wordt opnieuw gegenereerd de zelf-ondertekend SSL-certificaat, als van het certificaat-SAN bevat geen van de host van de Emulator domeinnaam, lokale IPv4-adres 'localhost' en '127.0.0.1'. Met deze optie wordt mislukt de emulator bij het opstarten in plaats daarvan. Vervolgens moet u de optie /GenCert gebruiken om te maken en een nieuw zelfondertekend SSL-certificaat te installeren. | CosmosDB.Emulator.exe /FailOnSslCertificateNameMismatch  | |
| GenCert | Genereren en installeren van een nieuw zelfondertekend SSL-certificaat. (optioneel) met inbegrip van een door komma's gescheiden lijst van extra DNS-namen voor toegang tot de Emulator via het netwerk. | CosmosDB.Emulator.exe /GenCert [ \<met door komma's gescheiden lijst van extra dns-namen\>] | |
| DirectPorts |Specificeert de poorten die worden gebruikt voor rechtstreekse connectiviteit. Standaardwaarden zijn 10251,10252,10253,10254. | CosmosDB.Emulator.exe /DirectPorts:\<directports\> | \<directports\>: Door komma's gescheiden lijst met 4 poorten |
| Sleutel |De autorisatiesleutel voor de emulator. De sleutel moet de base 64-codering zijn van een 64-byte-vector. | CosmosDB.Emulator.exe /Key:\<sleutel\> | \<sleutel\>: De sleutel moet de base 64-codering zijn van een 64-byte-vector|
| EnableRateLimiting | Geeft aan dat het beperkingsgedrag van de aanvraagsnelheid is ingeschakeld. |CosmosDB.Emulator.exe /EnableRateLimiting | |
| DisableRateLimiting |Geeft aan dat het beperkingsgedrag van de aanvraagsnelheid is uitgeschakeld. |CosmosDB.Emulator.exe /DisableRateLimiting | |
| NoUI | De gebruikersinterface van de emulator niet weergeven. | CosmosDB.Emulator.exe /NoUI | |
| NoExplorer | Geen Data Explorer weergeven bij het opstarten. |CosmosDB.Emulator.exe /NoExplorer | | 
| PartitionCount | Hiermee geeft u het maximum aantal gepartitioneerde containers. Zie [wijzigen van het aantal containers](#set-partitioncount) voor meer informatie. | CosmosDB.Emulator.exe /PartitionCount=\<aantal partities\> | \<aantal partities\>: Maximum aantal toegestane één partitie containers. Standaardwaarde is 25. Maximaal toegestaan is 250.|
| DefaultPartitionCount| Hiermee geeft u het aantal partities voor een gepartitioneerde container. | CosmosDB.Emulator.exe /DefaultPartitionCount=\<standaardaantal partities\> | \<defaultpartitioncount\> standaardwaarde is 25.|
| AllowNetworkAccess | Geeft toegang tot de emulator via een netwerk. U moet ook /Key=\<sleutelreeks\> of /KeyFile=\<bestandsnaam\> doorgeven om netwerktoegang in te schakelen. | CosmosDB.Emulator.exe /AllowNetworkAccess /Key=\<key_string\> of  CosmosDB.Emulator.exe /AllowNetworkAccess /KeyFile=\<file_name\>| |
| NoFirewall | Firewall-regels niet worden aangepast wanneer /AllowNetworkAccess optie wordt gebruikt. |CosmosDB.Emulator.exe /NoFirewall | |
| GenKeyFile | Een nieuwe autorisatiesleutel genereren en opslaan in het opgegeven bestand. De gegenereerde sleutel kan worden gebruikt met de opties/Key of/KeyFile. | CosmosDB.Emulator.exe /GenKeyFile=\<pad naar sleutelbestand\> | |
| Consistentie | Het standaard consistentieniveau voor het account instellen. | CosmosDB.Emulator.exe /Consistency=\<consistentie\> | \<consistentie\>: De waarde moet een van de volgende [consistentieniveaus](consistency-levels.md) zijn: Sessie, Sterk, Mogelijk of Gebonden veroudering. De standaardwaarde is Sessie. |
| ? | Het helpbericht weergeven.| | |

## <a id="set-partitioncount"></a>Het aantal containers wijzigen

Standaard kunt u maximaal 25 vaste grootte containers (alleen ondersteund met behulp van Azure Cosmos DB SDK's) of 5 onbeperkte containers met behulp van de Azure Cosmos-Emulator. Door het wijzigen van de **PartitionCount** waarde, kunt u maximaal 250 containers met vaste grootte of 50 onbeperkte containers of een combinatie van de twee die niet meer dan 250 containers met vaste grootte (waarbij een onbeperkte container = 5 vaste grootte containers). Het is echter niet aanbevolen om in te stellen de emulator om uit te voeren met meer dan 200 containers met vaste grootte. Vanwege de overhead die wordt toegevoegd aan de schijf-i/o-bewerkingen, die leiden tot onvoorspelbaar time-outs bij het gebruik van het eindpunt van de API's.


Als u probeert te maken van een container na het huidige aantal partities is overschreden, genereert de emulator een uitzondering ServiceUnavailable met het volgende bericht.

"Er we momenteel erg druk in deze regio, en kan uw aanvraag op dit moment niet voltooien. We werken continu om meer en meer capaciteit online brengen en raden en probeer het opnieuw.
Aarzel niet naar e-mailadres askcosmosdb@microsoft.com op elk gewenst moment of voor een bepaalde reden. ActivityId: 12345678-1234-1234-1234-123456789abc"

Als u wilt wijzigen van het aantal containers is beschikbaar in de Azure Cosmos-emulator gebruikt, voer de volgende stappen uit:

1. Alle lokale Emulator van Azure Cosmos-gegevens verwijderen met de rechtermuisknop op de **Azure Cosmos DB-Emulator** pictogram op de taakbalk en vervolgens te klikken op **gegevens opnieuw instellen...** .
2. Alle gegevens van de emulator in deze map verwijderen `%LOCALAPPDATA%\CosmosDBEmulator`.
3. Sluit alle geopende exemplaren door met de rechtermuisknop te klikken op het pictogram van de **Azure Cosmos DB Emulator** in het systeemvak en vervolgens te klikken op **Afsluiten**. Het afsluiten van alle exemplaren kan een paar minuten duren.
4. Installeer de nieuwste versie van de [Azure Cosmos-Emulator](https://aka.ms/cosmosdb-emulator).
5. Start de emulator met de vlag PartitionCount door een waarde < = 250 in te stellen. Bijvoorbeeld: `C:\Program Files\Azure Cosmos DB Emulator> CosmosDB.Emulator.exe /PartitionCount=100`.

## <a name="controlling-the-emulator"></a>De emulator beheren

De emulator wordt geleverd met een PowerShell-module te starten, stoppen, verwijderen en de status van de service ophalen. Voer de volgende cmdlet voor het gebruik van de PowerShell-module:

```powershell
Import-Module "$env:ProgramFiles\Azure Cosmos DB Emulator\PSModules\Microsoft.Azure.CosmosDB.Emulator"
```

of plaats de `PSModules` map op uw `PSModulesPath` en deze te importeren zoals wordt weergegeven in de volgende opdracht uit:

```powershell
$env:PSModulesPath += "$env:ProgramFiles\Azure Cosmos DB Emulator\PSModules"
Import-Module Microsoft.Azure.CosmosDB.Emulator
```

Hier volgt een samenvatting van de opdrachten voor het beheren van de emulator vanuit PowerShell:

### `Get-CosmosDbEmulatorStatus`

**Syntaxis**

`Get-CosmosDbEmulatorStatus`

**Opmerkingen**

Retourneert een van deze ServiceControllerStatus-waarden: ServiceControllerStatus.StartPending, ServiceControllerStatus.Running of ServiceControllerStatus.Stopped.

### `Start-CosmosDbEmulator`

**Syntaxis**

`Start-CosmosDbEmulator [-DataPath <string>] [-DefaultPartitionCount <uint16>] [-DirectPort <uint16[]>] [-MongoPort <uint16>] [-NoUI] [-NoWait] [-PartitionCount <uint16>] [-Port <uint16>] [<CommonParameters>]`

**Opmerkingen**

Start de emulator. Standaard wacht de opdracht totdat de emulator klaar is om aanvragen te accepteren. Gebruik de optie -NoWait als u wilt dat de cmdlet wordt geretourneerd zodra deze de emulator heeft gestart.

### `Stop-CosmosDbEmulator`

**Syntaxis**

 `Stop-CosmosDbEmulator [-NoWait]`

**Opmerkingen**

Stopt de emulator. Standaard wacht deze opdracht totdat de emulator volledig is afgesloten. Gebruik de optie -NoWait als u wilt dat de cmdlet wordt geretourneerd zodra het afsluiten van de emulator wordt gestart.

### `Uninstall-CosmosDbEmulator`

**Syntaxis**

`Uninstall-CosmosDbEmulator [-RemoveData]`

**Opmerkingen**

Verwijdert de emulator en optioneel ook de inhoud van $env:LOCALAPPDATA\CosmosDbEmulator.
De cmdlet zorgt ervoor dat de emulator is gestopt deze wordt verwijderd.

## <a name="running-on-docker"></a>Uitvoeren op Docker

De Azure Cosmos-Emulator kan worden uitgevoerd op Docker voor Windows. De emulator werkt niet op Docker voor Oracle Linux.

Nadat u [Docker voor Windows](https://www.docker.com/docker-windows) hebt geïnstalleerd, schakelt u over naar Windows-containers door met de rechtermuisknop te klikken op het Docker-pictogram op de werkbalk en **Overschakelen naar Windows-containers** te selecteren.

Haal vervolgens de installatiekopie van de emulator op van Docker Hub met de volgende opdracht in uw favoriete shell.

```bash
docker pull microsoft/azure-cosmosdb-emulator
```
Voer de volgende opdrachten uit om de installatiekopie te starten.

Vanaf de opdrachtregel:
```cmd

md %LOCALAPPDATA%\CosmosDBEmulator\bind-mount

docker run --name azure-cosmosdb-emulator --memory 2GB --mount "type=bind,source=%LOCALAPPDATA%\CosmosDBEmulator\bind-mount,destination=C:\CosmosDB.Emulator\bind-mount" --interactive --tty -p 8081:8081 -p 8900:8900 -p 8901:8901 -p 8902:8902 -p 10250:10250 -p 10251:10251 -p 10252:10252 -p 10253:10253 -p 10254:10254 -p 10255:10255 -p 10256:10256 -p 10350:10350 microsoft/azure-cosmosdb-emulator
```

Vanuit PowerShell:
```powershell

md $env:LOCALAPPDATA\CosmosDBEmulator\bind-mount 2>null

docker run --name azure-cosmosdb-emulator --memory 2GB --mount "type=bind,source=$env:LOCALAPPDATA\CosmosDBEmulator\bind-mount,destination=C:\CosmosDB.Emulator\bind-mount" --interactive --tty -p 8081:8081 -p 8900:8900 -p 8901:8901 -p 8902:8902 -p 10250:10250 -p 10251:10251 -p 10252:10252 -p 10253:10253 -p 10254:10254 -p 10255:10255 -p 10256:10256 -p 10350:10350 microsoft/azure-cosmosdb-emulator

```

De reactie ziet er ongeveer als volgt uit:

```
Starting emulator
Emulator Endpoint: https://172.20.229.193:8081/
Master Key: C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==
Exporting SSL Certificate
You can import the SSL certificate from an administrator command prompt on the host by running:
cd /d %LOCALAPPDATA%\CosmosDBEmulatorCert
powershell .\importcert.ps1
--------------------------------------------------------------------------------------------------
Starting interactive shell
```

Gebruik nu het eindpunt en de hoofdsleutel uit de reactie in uw client en importeer het SSL-certificaat in de host. Doe het volgende vanaf een opdrachtprompt voor een beheerder om het SSL-certificaat te importeren:

Vanaf de opdrachtregel:

```cmd
cd  %LOCALAPPDATA%\CosmosDBEmulator\bind-mount
powershell .\importcert.ps1
```

Vanuit PowerShell:
```powershell
cd $env:LOCALAPPDATA\CosmosDBEmulator\bind-mount
.\importcert.ps1
```

Wanneer u de interactieve shell sluit nadat de emulator is gestart, wordt de container van de emulator afgesloten.

U opent de Data Explorer door naar de volgende URL in uw browser te gaan. Het eindpunt van de emulator wordt vermeld in het reactiebericht dat hierboven wordt getoond.

    https://<emulator endpoint provided in response>/_explorer/index.html


## <a name="troubleshooting"></a>Problemen oplossen

Gebruik de volgende tips voor het oplossen van problemen die met de Azure Cosmos-Emulator optreden:

- Als u een nieuwe versie van de emulator hebt geïnstalleerd en er fouten optreden, zorg dan dat u uw gegevens opnieuw instelt. U kunt uw gegevens herstellen door met de rechtermuisknop op het pictogram van Azure Cosmos-Emulator op het systeemvak klikken en vervolgens op opnieuw instellen van gegevens... Als de fouten die niet wordt opgelost, kunt u verwijderen de emulator en oudere versies van de emulator als gevonden, "C:\Program files\Azure Cosmos DB-Emulator" map verwijderen en opnieuw installeren van de emulator. Zie [De lokale emulator verwijderen](#uninstall) voor instructies.

- Als de Azure Cosmos Emulator vastloopt, verzamelen van dumpbestanden uit map '% LOCALAPPDATA%\CrashDumps', comprimeren en koppelt u ze aan een e-mail naar [ askcosmosdb@microsoft.com ](mailto:askcosmosdb@microsoft.com).

- Als u crashes ondervindt in `Microsoft.Azure.Cosmos.ComputeServiceStartupEntryPoint.exe`, dit kan een symptoom zijn waarin de prestatiemeteritems zijn beschadigd is. Meestal met de volgende opdracht vanaf een opdrachtprompt met beheerdersrechten, wordt het probleem opgelost:

  ```cmd
  lodctr /R
   ```

- Als er sprake is van een verbindingsprobleem, [verzamelt u de traceringsbestanden](#trace-files), comprimeert u de bestanden en verstuurt u het gecomprimeerde bestand als bijlage bij een e-mail naar [askcosmosdb@microsoft.com](mailto:askcosmosdb@microsoft.com).

- Als u het bericht **Service niet beschikbaar** krijgt, is het mogelijk dat de emulator de netwerkstack niet kan initialiseren. Controleer of de veilige Pulse-client of Juniper-netwerkclient is geïnstalleerd. De netwerkfilterstuurprogramma's van deze clients kunnen mogelijk de oorzaak zijn van het probleem. Doorgaans kan het probleem worden opgelost door stuurprogramma's voor netwerkfilters van derden te verwijderen. U kunt ook start de emulator met /DisableRIO, die de netwerkcommunicatie emulator wordt overgeschakeld naar normale Winsock. 

- Als de emulator wordt uitgevoerd en uw computer naar de slaapstand gaat of er besturingssysteemupdates worden uitgevoerd, wordt mogelijk het bericht **Service is momenteel niet beschikbaar** weergegeven. Opnieuw instellen van de emulator gegevens, met de rechtermuisknop op het pictogram dat wordt weergegeven op de windows-systeemvak en selecteer **gegevens opnieuw**.

### <a id="trace-files"></a>Traceringsbestanden verzamelen

Voor het verzamelen van foutopsporingsgegevens, voert u de volgende opdrachten uit vanaf een opdrachtprompt voor een beheerder:

1. `cd /d "%ProgramFiles%\Azure Cosmos DB Emulator"`
2. `CosmosDB.Emulator.exe /shutdown`. Kijk in het systeemvak of het programma is afgesloten. Dit kan een minuut duren. U kunt ook klikken op **afsluiten** in de gebruikersinterface van Azure Cosmos-Emulator.
3. `CosmosDB.Emulator.exe /starttraces`
4. `CosmosDB.Emulator.exe`
5. Reproduceer het probleem. Als Data Explorer niet werkt, hoeft u alleen te wachten tot de browser een paar seconden wordt geopend om de fout te achterhalen.
5. `CosmosDB.Emulator.exe /stoptraces`
6. Ga naar `%ProgramFiles%\Azure Cosmos DB Emulator` en zoek het bestand docdbemulator_000001.etl op.
7. Stuur het .etl-bestand samen met de reproductiestappen naar [askcosmosdb@microsoft.com](mailto:askcosmosdb@microsoft.com) om de fout op te sporen.

### <a id="uninstall"></a>De lokale emulator verwijderen

1. Sluit alle geopende exemplaren van de lokale emulator door met de rechtermuisknop op het pictogram van Azure Cosmos-Emulator op het systeemvak klikken en vervolgens op sluiten. Het afsluiten van alle exemplaren kan een paar minuten duren.
2. Typ in het zoekvak Windows **Apps en onderdelen** en klik op **Apps en onderdelen (systeeminstellingen)**.
3. Ga in de lijst met apps naar **Azure Cosmos DB Emulator**, selecteer de app, klik op **Verwijderen**, bevestig dit en klik nogmaals op **Verwijderen**.
4. Wanneer de app is verwijderd, gaat u naar `%LOCALAPPDATA%\CosmosDBEmulator` en verwijdert u de map.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe u de lokale emulator kunt gebruiken voor gratis lokale ontwikkelingsdoeleinden. U kunt nu verder gaan met de volgende zelfstudie om te leren hoe u SSL-certificaten voor de emulator exporteert.

> [!div class="nextstepaction"]
> [De Azure Cosmos-emulatorcertificaten exporteren](local-emulator-export-ssl-certificates.md)
