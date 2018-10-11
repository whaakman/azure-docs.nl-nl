---
title: Lokaal ontwikkelen met de Azure Cosmos DB Emulator | Microsoft Docs
description: Met behulp van de Azure Cosmos DB Emulator kunt u uw toepassing lokaal ontwikkelen en testen, kosteloos en zonder dat u een Azure-abonnement hoeft te nemen.
services: cosmos-db
keywords: Azure Cosmos DB Emulator
author: David-Noble-at-work
manager: kfile
editor: ''
ms.service: cosmos-db
ms.devlang: na
ms.topic: tutorial
ms.date: 04/20/2018
ms.author: danoble
ms.openlocfilehash: a5481f9b2b443a0860ce0df5643427f357e1c294
ms.sourcegitcommit: 4edf9354a00bb63082c3b844b979165b64f46286
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/04/2018
ms.locfileid: "48785368"
---
# <a name="use-the-azure-cosmos-db-emulator-for-local-development-and-testing"></a>De Azure Cosmos DB Emulator gebruiken voor lokaal ontwikkelen en testen

<table>
<tr>
  <td><strong>Binaire bestanden</strong></td>
  <td>[MSI downloaden](https://aka.ms/cosmosdb-emulator)</td>
</tr>
<tr>
  <td><strong>Docker</strong></td>
  <td>[Docker Hub](https://hub.docker.com/r/microsoft/azure-cosmosdb-emulator/)</td>
</tr>
<tr>
  <td><strong>Docker-bron</strong></td>
  <td>[GitHub](https://github.com/Azure/azure-cosmos-db-emulator-docker)</td>
</tr>
</table>
  
De Azure Cosmos DB Emulator is een lokale omgeving waarin de Azure Cosmos DB-service wordt geëmuleerd voor ontwikkelingsdoeleinden. Met behulp van de Azure Cosmos DB Emulator kunt u uw toepassing lokaal ontwikkelen en testen, kosteloos en zonder een Azure-abonnement te maken. Als u tevreden bent over hoe uw toepassing in de Azure Cosmos DB Emulator werkt, kunt u overstappen naar een Azure Cosmos DB-account in de cloud. U kunt de Azure Cosmos DB-emulator gebruiken met alle API's - SQL, MongoDB, Cassandra, Gremlin en Table.

Dit artikel behandelt de volgende taken: 

> [!div class="checklist"]
> * De emulator installeren
> * Aanvragen verifiëren
> * De Data Explorer in de emulator gebruiken
> * SSL-certificaten exporteren
> * De emulator aanroepen vanaf de opdrachtregel
> * De emulator uitvoeren op Docker voor Windows
> * Traceringsbestanden verzamelen
> * Problemen oplossen

## <a name="how-the-emulator-works"></a>Hoe de emulator werkt

De Azure Cosmos DB Emulator biedt een hoogwaardige emulatie van de Azure Cosmos DB-service. Hij ondersteunt dezelfde functionaliteit als Azure Cosmos DB, inclusief het maken van en het uitvoeren van query's op JSON-documenten, het inrichten en schalen van verzamelingen en het uitvoeren van opgeslagen procedures en triggers. U kunt toepassingen ontwikkelen en testen met behulp van de Azure Cosmos DB-emulator en deze op globale schaal implementeren in Azure door slechts één wijziging aan te brengen in de configuratie van het verbindingseindpunt voor Azure Cosmos DB.

Emulatie van de Azure Cosmos DB-service is zeer betrouwbaar, maar de implementatie van de emulator is anders dan die van de service. De emulator gebruikt bijvoorbeeld standaardbesturingssysteemonderdelen, zoals het lokale bestandssysteem voor persistentie en de HTTPS-protocolstack voor connectiviteit. Hierdoor zijn bepaalde functies die afhankelijk zijn van de Azure-infrastructuur, zoals globale replicatie, vertragingen van milliseconden voor lezen/schrijven en instelbare consistentieniveaus, niet beschikbaar via de emulator.

## <a name="differences-between-the-emulator-and-the-service"></a>Verschillen tussen de emulator en de service 
Omdat de Azure Cosmos DB Emulator een geëmuleerde omgeving op een lokaal ontwikkelaarswerkstation biedt, zijn er enkele verschillen in functionaliteit tussen de emulator en een Azure Cosmos DB-account in de cloud:

* Op dit moment biedt de Data Explorer in de emulator alleen ondersteuning voor SQL API-verzamelingen en MongoDB-verzamelingen. Tabel-, grafiek- en Cassandra-API's worden nog niet ondersteund.  
* De Azure Cosmos DB Emulator ondersteunt slechts één vast account en een bekende hoofdsleutel.  Het genereren van toegangssleutels is niet mogelijk in de  Azure Cosmos DB Emulator.
* De Azure Cosmos DB Emulator-service is niet schaalbaar en biedt geen ondersteuning voor een groot aantal verzamelingen.
* De Azure Cosmos DB Emulator simuleert niet verschillende [Azure Cosmos DB-consistentieniveaus](consistency-levels.md).
* De Azure Cosmos DB Emulator simuleert niet [replicatie in meerdere regio's](distribute-data-globally.md).
* De Azure Cosmos DB Emulator biedt geen ondersteuning voor overschrijdingen van het servicequotum die beschikbaar zijn in de Azure Cosmos DB-service (bijvoorbeeld de limieten voor documentgrootte, meer gepartitioneerde opslag van verzamelingen).
* Aangezien uw exemplaar van Azure Cosmos DB Emulator mogelijk niet is bijgewerkt met de meest recente wijzigingen via de Azure Cosmos DB-service, gebruikt u de [Azure Cosmos DB-capaciteitsplanner](https://www.documentdb.com/capacityplanner) om de productiedoorvoerbehoefte van uw toepassing nauwkeurig te schatten.

## <a name="system-requirements"></a>Systeemvereisten
De Azure Cosmos DB Emulator heeft de volgende hardware- en software-vereisten:

* Softwarevereisten
  * Windows Server 2012 R2, Windows Server 2016 of Windows 10
*   Minimale hardwarevereisten
  * 2 GB RAM
  * 10 GB beschikbare schijfruimte

## <a name="installation"></a>Installatie
U kunt de Azure Cosmos DB Emulator downloaden en installeren via het [Microsoft Downloadcentrum](https://aka.ms/cosmosdb-emulator) of u kunt de emulator uitvoeren op Docker voor Windows. Zie [Uitvoeren op Docker](#running-on-docker) voor instructies over het gebruik van de emulator op Docker voor Windows. 

> [!NOTE]
> Als u de Azure Cosmos DB Emulator wilt installeren, configureren en uitvoeren, moet u beheerdersbevoegdheden hebben op de computer.

## <a name="running-on-windows"></a>Uitvoeren in Windows

U kunt de Azure Cosmos DB Emulator starten door de Start-knop te selecteren of op de Windows-toets te drukken. Begin met het typen van **Azure Cosmos DB Emulator** en selecteer de emulator in de lijst met toepassingen. 

![Selecteer de Start-knop of druk op de Windows-toets, begin met het typen van ** Azure Cosmos DB Emulator ** en selecteer de emulator in de lijst met toepassingen](./media/local-emulator/database-local-emulator-start.png)

Wanneer de emulator wordt gestart, ziet u een pictogram in het systeemvak op de taakbalk van Windows. ![Pictogram in systeemvak van lokale Azure DB Cosmos Emulator](./media/local-emulator/database-local-emulator-taskbar.png)

De Azure Cosmos DB Emulator wordt standaard uitgevoerd op de lokale computer ('localhost'), en luistert op poort 8081.

De Azure Cosmos DB Emulator wordt standaard geïnstalleerd in `C:\Program Files\Azure Cosmos DB Emulator`. U kunt de emulator ook starten en stoppen vanaf de opdrachtregel. Zie de [informatie over het opdrachtregelprogramma](#command-line) voor meer informatie.

## <a name="start-data-explorer"></a>Data Explorer starten

Als de Azure Cosmos DB-emulator wordt gestart, wordt automatisch de Azure Cosmos DB Data Explorer in uw browser geopend. Het adres wordt weergegeven als [https://localhost:8081/_explorer/index.html](https://localhost:8081/_explorer/index.html). Als u de explorer sluit en later opnieuw wilt openen, kunt u de URL in uw browser openen of deze starten vanuit de Azure Cosmos DB Emulator in het Windows-systeemvak, zoals hieronder weergegeven.

![Startprogramma voor Data Explorer van lokale Azure Cosmos DB Emulator](./media/local-emulator/database-local-emulator-data-explorer-launcher.png)

## <a name="checking-for-updates"></a>Controleren op updates
Data Explorer geeft aan of er een nieuwe update beschikbaar is om te downloaden. 

> [!NOTE]
> Gegevens die zijn gemaakt in de ene versie van de Azure Cosmos DB Emulator, zijn niet gegarandeerd toegankelijk wanneer u een andere versie gebruikt. Als u uw gegevens voor de lange termijn wilt bewaren, verdient het aanbeveling om die gegevens op te slaan in een Azure DB Cosmos-account, in plaats van in de Azure Cosmos DB Emulator. 

## <a name="authenticating-requests"></a>Aanvragen verifiëren
Net als bij Azure Cosmos DB in de cloud, moet elke aanvraag die u op basis van de Azure Cosmos DB Emulator maakt, worden geverifieerd. De Azure Cosmos DB Emulator ondersteunt één vast account en een bekende verificatiesleutel voor hoofdsleutelverificatie. Dit account en deze sleutel zijn de enige referenties die zijn toegestaan voor gebruik met de Azure Cosmos DB Emulator. Dit zijn:

    Account name: localhost:<port>
    Account key: C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==

> [!NOTE]
> De hoofdsleutel ondersteund door de Azure Cosmos DB Emulator is alleen bedoeld voor gebruik met de emulator. U kunt niet uw Azure DB Cosmos-account en -sleutel voor productie gebruiken met de Azure Cosmos DB Emulator. 

> [!NOTE] 
> Als u de emulator hebt gestart met de optie/Key, gebruikt u de gegenereerde sleutel in plaats van 'C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw=='

Bovendien ondersteunt de Azure Cosmos DB Emulator (net als de Azure Cosmos DB-service) alleen beveiligde communicatie via SSL.

## <a name="running-on-a-local-network"></a>Uitvoeren op een lokaal netwerk

U kunt de emulator uitvoeren op een lokaal netwerk. U maakt netwerktoegang mogelijk door de optie /AllowNetworkAccess op te geven bij de [opdrachtregel](#command-line-syntax). U moet hiervoor ook /Key=sleutelreeks of /KeyFile=bestandsnaam opgeven. U kunt /GenKeyFile=bestandsnaam gebruiken voor het van tevoren genereren van een bestand met een willekeurige sleutel.  Dat kunt u doorgeven aan /KeyFile=bestandsnaam of /Key=inhoud van bestand.

Om voor de eerste keer toegang te krijgen tot het netwerk, moet de gebruiker de emulator afsluiten en de gegevensmap van de emulator verwijderen (C:\Users\gebruikersnaam\AppData\Local\CosmosDBEmulator).

## <a name="developing-with-the-emulator"></a>Ontwikkelen met de emulator
Wanneer de Azure Cosmos DB-emulator op uw computer wordt uitgevoerd, kunt u elke ondersteunde [Azure Cosmos DB SDK](sql-api-sdk-dotnet.md) of de [Azure Cosmos DB REST API](/rest/api/cosmos-db/) gebruiken om met de emulator te communiceren. De Azure Cosmos DB Emulator bevat ook een ingebouwde Data Explorer. Hiermee kunt u verzamelingen maken voor de SQL en MongoDB API's en kunt u documenten weergeven en bewerken zonder code te schrijven.   

    // Connect to the Azure Cosmos DB Emulator running locally
    DocumentClient client = new DocumentClient(
        new Uri("https://localhost:8081"), 
        "C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==");

Als u [Azure Cosmos DB-protocolondersteuning voor MongoDB](mongodb-introduction.md) gebruikt, gebruikt u de volgende verbindingsreeks:

    mongodb://localhost:C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==@localhost:10255/admin?ssl=true

U kunt bestaande hulpprogramma's zoals [Azure DocumentDB Studio](https://github.com/mingaliu/DocumentDBStudio) gebruiken om verbinding te maken met de Azure Cosmos DB Emulator. U kunt ook gegevens migreren tussen de Azure Cosmos DB Emulator en de Azure Cosmos DB-service met behulp van de [Azure Cosmos DB Data Migration Tool](https://github.com/azure/azure-documentdb-datamigrationtool).

> [!NOTE] 
> Als u de emulator hebt gestart met de optie/Key, gebruikt u de gegenereerde sleutel in plaats van 'C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw=='

Met de Azure Cosmos DB-emulator kunt u standaard maximaal 25 verzamelingen met één partitie of één gepartitioneerde verzameling maken. Zie [De PartitionCount-waarde instellen](#set-partitioncount) voor meer informatie over het wijzigen van deze waarde.

## <a name="export-the-ssl-certificate"></a>Het SSL-certificaat exporteren

.NET-talen en -runtime maken gebruik van het Windows-certificaatarchief om veilig verbinding te maken met de lokale Azure DB Cosmos Emulator. Andere talen hebben hun eigen methode voor het beheren en gebruiken van certificaten. Java maakt gebruik van een eigen [certificaatarchief](https://docs.oracle.com/cd/E19830-01/819-4712/ablqw/index.html), terwijl Python gebruikmaakt van [socketwrappers](https://docs.python.org/2/library/ssl.html).

Om een certificaat te verkrijgen voor gebruik bij talen en runtimes die niet worden geïntegreerd in het certificaatarchief van Windows, moet u het exporteren met behulp van de Windows Certificate Manager. U kunt dit starten door certlm.msc uit te voeren of de stapsgewijze instructies te volgen in [De Azure Cosmos DB Emulator-certificaten exporteren](./local-emulator-export-ssl-certificates.md). Zodra de certificaatmanager wordt uitgevoerd, opent u de persoonlijke certificaten zoals hieronder weergegeven en exporteert u het certificaat met de beschrijvende naam 'DocumentDBEmulatorCertificate' als een BASE 64-gecodeerd X.509-bestand (.cer).

![SSL-certificaat voor een lokale Azure Cosmos DB Emulator](./media/local-emulator/database-local-emulator-ssl_certificate.png)

Volg de instructies in [Een certificaat toevoegen aan het Java CA-certificaatarchief](https://docs.microsoft.com/azure/java-add-certificate-ca-store) om het X.509-certificaat te importeren in het Java-certificaatarchief. Zodra het certificaat is geïmporteerd in het certificaatarchief, kunnen Java en de MongoDB-toepassingen verbinding maken met de Azure Cosmos DB Emulator.

Bij het verbinden met de emulator van Python en Node.js SDK's, is SSL-verificatie uitgeschakeld.

## <a id="command-line"></a>Opdrachtregelprogramma
Vanaf de installatielocatie kunt u de opdrachtregel gebruiken om de emulator te starten en te stoppen, opties te configureren en andere bewerkingen uitvoeren.

### <a name="command-line-syntax"></a>De syntaxis van opdrachtregel

    CosmosDB.Emulator.exe [/Shutdown] [/DataPath] [/Port] [/MongoPort] [/DirectPorts] [/Key] [/EnableRateLimiting] [/DisableRateLimiting] [/NoUI] [/NoExplorer] [/?]

Typ `CosmosDB.Emulator.exe /?` bij de opdrachtprompt om een lijst met opties te zien.

<table>
<tr>
  <td><strong>Optie</strong></td>
  <td><strong>Beschrijving</strong></td>
  <td><strong>Opdracht</strong></td>
  <td><strong>Argumenten</strong></td>
</tr>
<tr>
  <td>[Geen argumenten]</td>
  <td>Start de Azure Cosmos DB Emulator met standaardinstellingen.</td>
  <td>CosmosDB.Emulator.exe</td>
  <td></td>
</tr>
<tr>
  <td>[Help]</td>
  <td>Toont de lijst met ondersteunde opdrachtregelargumenten.</td>
  <td>CosmosDB.Emulator.exe /?</td>
  <td></td>
</tr>
<tr>
  <td>GetStatus</td>
  <td>Downloadt de status van de Azure Cosmos DB Emulator. De status wordt aangegeven door de afsluitcode: 1 = starten, 2 = wordt uitgevoerd, 3 = gestopt. Een negatieve afsluitcode geeft aan dat er een fout is opgetreden. Er wordt geen andere uitvoer geproduceerd.</td>
  <td>CosmosDB.Emulator.exe /GetStatus</td>
  <td></td>
<tr>
  <td>Afsluiten</td>
  <td>Sluit de Azure Cosmos DB Emulator af.</td>
  <td>CosmosDB.Emulator.exe /Shutdown</td>
  <td></td>
</tr>
<tr>
  <td>DataPath</td>
  <td>Specificeert het pad waarin de gegevensbestanden worden opgeslagen. Standaard is %LocalAppdata%\CosmosDBEmulator.</td>
  <td>CosmosDB.Emulator.exe /DataPath=&lt;gegevenspad&gt;</td>
  <td>&lt;gegevenspad&gt;: een toegankelijk pad</td>
</tr>
<tr>
  <td>Poort</td>
  <td>Specificeert het poortnummer dat moet worden gebruikt voor de emulator.  Standaardpoort is 8081.</td>
  <td>CosmosDB.Emulator.exe /Port=&lt;poort&gt;</td>
  <td>&lt;poort&gt;: enkel poortnummer</td>
</tr>
<tr>
  <td>MongoPort</td>
  <td>Specificeert het poortnummer dat moet worden gebruikt MongoDB compatibiliteit-API. Standaardinstelling is 10255.</td>
  <td>CosmosDB.Emulator.exe /MongoPort=&lt;mongopo0rt&gt;</td>
  <td>&lt;mongopo0rt&gt;: enkel poortnummer</td>
</tr>
<tr>
  <td>DirectPorts</td>
  <td>Specificeert de poorten die worden gebruikt voor rechtstreekse connectiviteit. Standaardwaarden zijn 10251,10252,10253,10254.</td>
  <td>CosmosDB.Emulator.exe /DirectPorts:&lt;directports&gt;</td>
  <td>&lt;directports&gt;: door komma's gescheiden lijst met 4 poorten</td>
</tr>
<tr>
  <td>Sleutel</td>
  <td>De autorisatiesleutel voor de emulator. De sleutel moet de base 64-codering zijn van een 64-byte-vector.</td>
  <td>CosmosDB.Emulator.exe /Key:&lt;sleutel&gt;</td>
  <td>&lt;sleutel&gt;: sleutel moet de base 64-codering zijn van een 64-byte-vector</td>
</tr>
<tr>
  <td>EnableRateLimiting</td>
  <td>Geeft aan dat het beperkingsgedrag van de aanvraagsnelheid is ingeschakeld.</td>
  <td>CosmosDB.Emulator.exe /EnableRateLimiting</td>
  <td></td>
</tr>
<tr>
  <td>DisableRateLimiting</td>
  <td>Geeft aan dat het beperkingsgedrag van de aanvraagsnelheid is uitgeschakeld.</td>
  <td>CosmosDB.Emulator.exe /DisableRateLimiting</td>
  <td></td>
</tr>
<tr>
  <td>NoUI</td>
  <td>De gebruikersinterface van de emulator niet weergeven.</td>
  <td>CosmosDB.Emulator.exe /NoUI</td>
  <td></td>
</tr>
<tr>
  <td>NoExplorer</td>
  <td>Geen Data Explorer weergeven bij het opstarten.</td>
  <td>CosmosDB.Emulator.exe /NoExplorer</td>
  <td></td>
</tr>
<tr>
  <td>PartitionCount</td>
  <td>Specificeert het maximumaantal gepartitioneerde verzamelingen. Zie [Het aantal verzamelingen wijzigen](#set-partitioncount) voor meer informatie.</td>
  <td>CosmosDB.Emulator.exe /PartitionCount=&lt;aantal partities&gt;</td>
  <td>&lt;aantal partities&gt;: het maximumaantal toegestane verzamelingen met één partitie. Standaardaantal is 25. Maximaal toegestaan is 250.</td>
</tr>
<tr>
  <td>DefaultPartitionCount</td>
  <td>Specificeert het aantal partities voor een gepartitioneerde verzameling.</td>
  <td>CosmosDB.Emulator.exe /DefaultPartitionCount=&lt;standaardaantal partities&gt;</td>
  <td>&lt;standaardaantal partities&gt; Standaardaantal is 25.</td>
</tr>
<tr>
  <td>AllowNetworkAccess</td>
  <td>Geeft toegang tot de emulator via een netwerk. U moet ook /Key=&lt;sleutelreeks&gt; of /KeyFile=&lt;bestandsnaam&gt; doorgeven om netwerktoegang in te schakelen.</td>
  <td>CosmosDB.Emulator.exe /AllowNetworkAccess /Key=&lt;sleutelreeks&gt;<br><br>of<br><br>CosmosDB.Emulator.exe /AllowNetworkAccess /KeyFile=&lt;bestandsnaam&gt;</td>
  <td></td>
</tr>
<tr>
  <td>NoFirewall</td>
  <td>Firewallregels niet aanpassen wanneer /AllowNetworkAccess wordt gebruikt.</td>
  <td>CosmosDB.Emulator.exe /NoFirewall</td>
  <td></td>
</tr>
<tr>
  <td>GenKeyFile</td>
  <td>Een nieuwe autorisatiesleutel genereren en opslaan in het opgegeven bestand. De gegenereerde sleutel kan worden gebruikt met de opties/Key of/KeyFile.</td>
  <td>CosmosDB.Emulator.exe  /GenKeyFile=&lt;pad naar sleutelbestand&gt;</td>
  <td></td>
</tr>
<tr>
  <td>Consistentie</td>
  <td>Het standaard consistentieniveau voor het account instellen.</td>
  <td>CosmosDB.Emulator.exe /Consistency=&lt;consistentie&gt;</td>
  <td>&lt;consistentie&gt;: waarde moet een van de volgende [consistentieniveaus](consistency-levels.md) zijn: sessie, sterk, mogelijk of gebonden veroudering.  De standaardwaarde is Sessie.</td>
</tr>
<tr>
  <td>?</td>
  <td>Het helpbericht weergeven.</td>
  <td></td>
  <td></td>
</tr>
</table>

## <a id="set-partitioncount"></a>Het aantal verzamelingen wijzigen

Standaard kunt u maximaal 25 verzamelingen met één partitie of één gepartitioneerde verzameling maken met de Azure Cosmos DB-emulator. Door de waarde voor **PartitionCount** aan te passen, kunt u maximaal 250 verzamelingen met één partitie of tien gepartitioneerde verzamelingen maken, of een combinatie van de twee die niet meer dan 250 enkele partities bevat (waarbij één gepartitioneerde verzameling gelijk is aan 25 verzamelingen met één partitie).

Als u probeert een verzameling maken nadat het huidige aantal partities is overschreden, genereert de emulator een ServiceUnavailable-uitzondering met het volgende bericht.

    Sorry, we are currently experiencing high demand in this region, 
    and cannot fulfill your request at this time. We work continuously 
    to bring more and more capacity online, and encourage you to try again. 
    Please do not hesitate to email askcosmosdb@microsoft.com at any time or
    for any reason. ActivityId: 29da65cc-fba1-45f9-b82c-bf01d78a1f91

Als u het aantal beschikbare verzamelingen voor de Azure Cosmos DB Emulator wilt wijzigen, doet u het volgende:

1. Verwijder alle lokale Azure Cosmos DB Emulator-gegevens door met de rechtermuisknop te klikken op het pictogram van de **Azure Cosmos DB Emulator** pictogram in het systeemvak en vervolgens te klikken op **Gegevens opnieuw instellen...** .
2. Verwijder alle gegevens van de emulator in de map C:\Users\gebruikersnaam\AppData\Local\CosmosDBEmulator.
3. Sluit alle geopende exemplaren door met de rechtermuisknop te klikken op het pictogram van de **Azure Cosmos DB Emulator** in het systeemvak en vervolgens te klikken op **Afsluiten**. Het afsluiten van alle exemplaren kan een paar minuten duren.
4. Installeer de nieuwste versie van de [Azure Cosmos DB Emulator](https://aka.ms/cosmosdb-emulator).
5. Start de emulator met de vlag PartitionCount door een waarde < = 250 in te stellen. Bijvoorbeeld: `C:\Program Files\Azure Cosmos DB Emulator> CosmosDB.Emulator.exe /PartitionCount=100`.

## <a name="controlling-the-emulator"></a>De emulator beheren

De emulator wordt geleverd met een PowerShell-module voor het starten, stoppen, en verwijderen, en het ophalen van de status van de service. U gebruikt deze als volgt:

```powershell
Import-Module "$env:ProgramFiles\Azure Cosmos DB Emulator\PSModules\Microsoft.Azure.CosmosDB.Emulator"
```

of plaats de map `PSModules` op uw `PSModulesPath` en importeer deze als volgt:

```powershell
$env:PSModulesPath += "$env:ProgramFiles\Azure Cosmos DB Emulator\PSModules"
Import-Module Microsoft.Azure.CosmosDB.Emulator
```

Hier volgt een samenvatting van de opdrachten voor het beheren van de emulator vanuit PowerShell:

### `Get-CosmosDbEmulatorStatus`

#### <a name="syntax"></a>Syntaxis

`Get-CosmosDbEmulatorStatus`

#### <a name="remarks"></a>Opmerkingen

Retourneert een van deze ServiceControllerStatus-waarden: ServiceControllerStatus.StartPending, ServiceControllerStatus.Running of ServiceControllerStatus.Stopped.

### `Start-CosmosDbEmulator`

#### <a name="syntax"></a>Syntaxis

`Start-CosmosDbEmulator [-DataPath <string>] [-DefaultPartitionCount <uint16>] [-DirectPort <uint16[]>] [-MongoPort <uint16>] [-NoUI] [-NoWait] [-PartitionCount <uint16>] [-Port <uint16>]  [<CommonParameters>]`

#### <a name="remarks"></a>Opmerkingen

Start de emulator. Standaard wacht de opdracht totdat de emulator klaar is om aanvragen te accepteren. Gebruik de optie -NoWait als u wilt dat de cmdlet wordt geretourneerd zodra deze de emulator heeft gestart.

### `Stop-CosmosDbEmulator`

#### <a name="syntax"></a>Syntaxis

 `Stop-CosmosDbEmulator [-NoWait]`

#### <a name="remarks"></a>Opmerkingen

Stopt de emulator. Standaard wacht deze opdracht totdat de emulator volledig is afgesloten. Gebruik de optie -NoWait als u wilt dat de cmdlet wordt geretourneerd zodra het afsluiten van de emulator wordt gestart.

### `Uninstall-CosmosDbEmulator`

#### <a name="syntax"></a>Syntaxis

`Uninstall-CosmosDbEmulator [-RemoveData]`

#### <a name="remarks"></a>Opmerkingen

Verwijdert de emulator en optioneel ook de inhoud van $env:LOCALAPPDATA\CosmosDbEmulator.
De cmdlet zorgt ervoor dat de emulator is gestopt deze wordt verwijderd.

## <a name="running-on-docker"></a>Uitvoeren op Docker

De Azure Cosmos DB Emulator kan worden uitgevoerd op Docker voor Windows. De emulator werkt niet op Docker voor Oracle Linux.

Nadat u [Docker voor Windows](https://www.docker.com/docker-windows) hebt geïnstalleerd, schakelt u over naar Windows-containers door met de rechtermuisknop te klikken op het Docker-pictogram op de werkbalk en **Overschakelen naar Windows-containers** te selecteren.

Haal vervolgens de installatiekopie van de emulator op van Docker Hub met de volgende opdracht in uw favoriete shell.

```     
docker pull microsoft/azure-cosmosdb-emulator 
```
Voer de volgende opdrachten uit om de installatiekopie te starten.

Vanaf de opdrachtregel:
```cmd 
md %LOCALAPPDATA%\CosmosDBEmulatorCert 2>null
docker run -v %LOCALAPPDATA%\CosmosDBEmulatorCert:C:\CosmosDB.Emulator\CosmosDBEmulatorCert -P -t -i -m 2GB microsoft/azure-cosmosdb-emulator 
```

Vanuit PowerShell:
```powershell
md $env:LOCALAPPDATA\CosmosDBEmulatorCert 2>null
docker run -v $env:LOCALAPPDATA\CosmosDBEmulatorCert:C:\CosmosDB.Emulator\CosmosDBEmulatorCert -P -t -i -m 2GB microsoft/azure-cosmosdb-emulator 
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
cd %LOCALAPPDATA%\CosmosDBEmulatorCert
powershell .\importcert.ps1
```

Vanuit PowerShell:
```powershell
cd $env:LOCALAPPDATA\CosmosDBEmulatorCert
.\importcert.ps1
```

Wanneer u de interactieve shell sluit nadat de emulator is gestart, wordt de container van de emulator afgesloten.

U opent de Data Explorer door naar de volgende URL in uw browser te gaan. Het eindpunt van de emulator wordt vermeld in het reactiebericht dat hierboven wordt getoond.

    https://<emulator endpoint provided in response>/_explorer/index.html


## <a name="troubleshooting"></a>Problemen oplossen

Gebruik de volgende tips voor het oplossen van problemen die u kunnen optreden met de Azure Cosmos DB-emulator:

- Als u een nieuwe versie van de emulator hebt geïnstalleerd en er fouten optreden, zorg dan dat u uw gegevens opnieuw instelt. U kunt uw gegevens opnieuw instellen door met de rechtermuisknop te klikken op het pictogram van de Azure Cosmos DB Emulator in het systeemvak en vervolgens te klikken op Gegevens opnieuw instellen…. Als de fouten hiermee niet worden opgelost, kunt u de app verwijderen en opnieuw installeren. Zie [De lokale emulator verwijderen](#uninstall) voor instructies.

- Als de Azure Cosmos DB-emulator vastloopt, verzamelt u dumpbestanden vanuit de map c:\Users\gebruikersnaam\AppData\Local\CrashDumps, comprimeert u de bestanden en verstuurt u het gecomprimeerde bestand als bijlage bij een e-mail naar [askcosmosdb@microsoft.com](mailto:askcosmosdb@microsoft.com).

- Als CosmosDB.StartupEntryPoint.exe vastloopt, voert u de volgende opdracht uit vanaf een opdrachtprompt voor een beheerder: `lodctr /R` 

- Als er sprake is van een verbindingsprobleem, [verzamelt u de traceringsbestanden](#trace-files), comprimeert u de bestanden en verstuurt u het gecomprimeerde bestand als bijlage bij een e-mail naar [askcosmosdb@microsoft.com](mailto:askcosmosdb@microsoft.com).

- Als u het bericht **Service niet beschikbaar** krijgt, is het mogelijk dat de emulator de netwerkstack niet kan initialiseren. Controleer of de veilige Pulse-client of Juniper-netwerkclient is geïnstalleerd. De netwerkfilterstuurprogramma's van deze clients kunnen mogelijk de oorzaak zijn van het probleem. Doorgaans kan het probleem worden opgelost door stuurprogramma's voor netwerkfilters van derden te verwijderen.

- Als de emulator wordt uitgevoerd en uw computer naar de slaapstand gaat of er besturingssysteemupdates worden uitgevoerd, wordt mogelijk het bericht **Service is momenteel niet beschikbaar** weergegeven. Stel de emulator opnieuw in door met de rechtermuisknop op het pictogram te klikken dat wordt weergegeven in het Windows-systeemvak. Selecteer dan **Gegevens opnieuw instellen**.

### <a id="trace-files"></a>Traceringsbestanden verzamelen

Voor het verzamelen van foutopsporingsgegevens, voert u de volgende opdrachten uit vanaf een opdrachtprompt voor een beheerder:

1. `cd /d "%ProgramFiles%\Azure Cosmos DB Emulator"`
2. `CosmosDB.Emulator.exe /shutdown`. Kijk in het systeemvak of het programma is afgesloten. Dit kan een minuut duren. U kunt ook klikken op **Afsluiten** in de gebruikersinterface van de Azure Cosmos DB-emulator.
3. `CosmosDB.Emulator.exe /starttraces`
4. `CosmosDB.Emulator.exe`
5. Reproduceer het probleem. Als Data Explorer niet werkt, hoeft u alleen te wachten tot de browser een paar seconden wordt geopend om de fout te achterhalen.
5. `CosmosDB.Emulator.exe /stoptraces`
6. Ga naar `%ProgramFiles%\Azure Cosmos DB Emulator` en zoek het bestand docdbemulator_000001.etl op.
7. Stuur het .etl-bestand samen met de reproductiestappen naar [askcosmosdb@microsoft.com](mailto:askcosmosdb@microsoft.com) om de fout op te sporen.

### <a id="uninstall"></a>De lokale emulator verwijderen

1. Sluit alle geopende exemplaren van de lokale emulator door met de rechtermuisknop te klikken op het pictogram van de Azure Cosmos DB-emulator in het systeemvak en vervolgens te klikken op Afsluiten. Het afsluiten van alle exemplaren kan een paar minuten duren.
2. Typ in het zoekvak Windows **Apps en onderdelen** en klik op **Apps en onderdelen (systeeminstellingen)**.
3. Ga in de lijst met apps naar **Azure Cosmos DB Emulator**, selecteer de app, klik op **Verwijderen**, bevestig dit en klik nogmaals op **Verwijderen**.
4. Wanneer de app is verwijderd, gaat u naar `C:\Users\<user>\AppData\Local\CosmosDBEmulator` en verwijdert u de map. 

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u het volgende gedaan:

> [!div class="checklist"]
> * De lokale emulator geïnstalleerd
> * De emulator uitgevoerd op Docker voor Windows
> * Aanvragen geverifieerd
> * De Data Explorer in de emulator gebruikt
> * SSL-certificaten geëxporteerd
> * De emulator aangeroepen vanaf de opdrachtregel
> * Traceringsbestanden verzameld

In deze zelfstudie hebt u geleerd hoe u de lokale emulator kunt gebruiken voor gratis lokale ontwikkelingsdoeleinden. U kunt nu verder gaan met de volgende zelfstudie om te leren hoe u SSL-certificaten voor de emulator exporteert. 

> [!div class="nextstepaction"]
> [De Azure Cosmos DB Emulator-certificaten exporteren](local-emulator-export-ssl-certificates.md)
