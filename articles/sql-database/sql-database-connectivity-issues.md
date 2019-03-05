---
title: Werken met tijdelijke fouten - Azure SQL Database | Microsoft Docs
description: Informatie over het oplossen, opsporen en voorkomen dat een SQL-verbindingsfout of een tijdelijke fout in Azure SQL Database.
keywords: SQL-verbinding, verbindingsreeks, problemen met de netwerkverbinding, tijdelijke fout, -verbindingsfout
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: dalechen
ms.author: ninarn
ms.reviewer: carlrab
manager: craigg
ms.date: 11/14/2018
ms.openlocfilehash: 0377f192ea9bc6502065fa037704ec8461ce1447
ms.sourcegitcommit: 3f4ffc7477cff56a078c9640043836768f212a06
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/04/2019
ms.locfileid: "57311098"
---
# <a name="working-with-sql-database-connection-issues-and-transient-errors"></a>Werken met SQL Database-verbindingsproblemen en tijdelijke fouten

In dit artikel wordt beschreven hoe om te voorkomen, oplossen, opsporen en oplossen van verbindingsfouten en tijdelijke fouten die uw clienttoepassing tegenkomt wanneer u de interactie met Azure SQL Database. Informatie over het configureren van de logica voor opnieuw proberen, de verbindingsreeks en andere verbindingsinstellingen aanpassen.

<a id="i-transient-faults" name="i-transient-faults"></a>

## <a name="transient-errors-transient-faults"></a>Tijdelijke fouten (tijdelijke fouten)

Een tijdelijke fout, ook wel bekend als een tijdelijke fouten, heeft een oorzaak die binnenkort wordt omgezet zelf. Een incidentele oorzaak van tijdelijke fouten is wanneer de Azure-systeem snel hardwareresources betere taakverdeling verschillende workloads verschuift. De meeste van deze gebeurtenissen herconfiguratie voltooid in minder dan 60 seconden. Tijdens deze periode herconfiguratie is mogelijk verbindingsproblemen met SQL-Database. Toepassingen die verbinding met SQL-Database maken moeten worden gebouwd om te verwachten dat deze tijdelijke fouten. Om deze te verwerken, logica voor opnieuw proberen in de code in plaats van ze zichtbaar te maken voor gebruikers als toepassingsfouten te implementeren.

Als uw clientprogramma gebruikmaakt van ADO.NET, uw programma over de tijdelijke fout is gemeld door het throw van **SqlException**. Vergelijk de **getal** van de eigenschap met de lijst van tijdelijke fouten die zijn gevonden aan de bovenkant van het artikel [SQL-foutcodes voor SQL Database-clienttoepassingen](sql-database-develop-error-messages.md).

<a id="connection-versus-command" name="connection-versus-command"></a>

### <a name="connection-vs-command"></a>Verbinding versus opdracht

De SQL-verbinding opnieuw of opnieuw instellen, afhankelijk van het volgende:

- **Een tijdelijke fout optreedt tijdens een verbindingspoging**

Na een vertraging van enkele seconden opnieuw verbinding te maken.

- **Een tijdelijke fout optreedt tijdens een SQL-query-opdracht**

Probeer niet onmiddellijk opnieuw de opdracht. In plaats daarvan na een tijdje opnieuw worden de verbinding te maken. Voer de opdracht vervolgens opnieuw uit.

<a id="j-retry-logic-transient-faults" name="j-retry-logic-transient-faults"></a>

## <a name="retry-logic-for-transient-errors"></a>Pogingslogica voor tijdelijke problemen

Client-programma's die af en toe een tijdelijke fout optreden zijn nog krachtiger wanneer daarin logica voor opnieuw proberen. Wanneer uw programma met SQL Database via externe middleware communiceert, vraagt u de leverancier of de middleware Pogingslogica voor tijdelijke fouten bevat.

<a id="principles-for-retry" name="principles-for-retry"></a>

### <a name="principles-for-retry"></a>Beginselen voor het opnieuw proberen

- Als de fout tijdelijk is, kunt u proberen om een verbinding openen.
- Probeer niet rechtstreeks opnieuw een SQL `SELECT` -instructie is mislukt met een tijdelijke fout. In plaats daarvan een nieuwe verbinding tot stand brengen, en probeer het vervolgens opnieuw de `SELECT`.
- Wanneer een SQL `UPDATE` instructie is mislukt met een tijdelijke fout een nieuwe verbinding tot stand brengen voordat u de UPDATE opnieuw. De logica voor opnieuw proberen moet ervoor zorgen dat de hele databasetransactie is voltooid of dat de hele transactie wordt teruggedraaid.

### <a name="other-considerations-for-retry"></a>Andere overwegingen voor opnieuw proberen

- Een batch-programma dat automatisch wordt gestart nadat de werkuren en is voltooid voordat de ochtend kunt zich veroorloven zeer patiënten met lange tijdsintervallen tussen de pogingen.
- Het programma voor een gebruikersinterface moet rekening gehouden met de menselijke neiging op te geven na een wachttijd te lang. De oplossing moet geen nieuwe poging gedaan om de paar seconden, omdat dit beleid kan het systeem met aanvragen overspoelen.

### <a name="interval-increase-between-retries"></a>De toename van het interval tussen nieuwe pogingen

Het is raadzaam dat u 5 seconden voordat de eerste poging wacht. Opnieuw proberen na een vertraging van minder dan 5 seconden risico's overbelasting van de cloudservice. Voor elke opeenvolgende pogingen, de vertraging moet groeien exponentieel toeneemt, tot maximaal 60 seconden.

Zie voor een bespreking van de blokkering voor clients die gebruikmaken van ADO.NET [SQL Server-verbinding (ADO.NET) groeperen](https://msdn.microsoft.com/library/8xx3tyca.aspx).

Ook is het verstandig om in te stellen van een maximum aantal nieuwe pogingen voordat het programma zelf wordt beëindigd.

### <a name="code-samples-with-retry-logic"></a>Voorbeelden van code met logica voor opnieuw proberen

Voorbeelden van code met logica voor nieuwe pogingen zijn beschikbaar op:

- [Flexibel verbinding maken met SQL via ADO.NET][step-4-connect-resiliently-to-sql-with-ado-net-a78n]
- [Flexibel verbinding maken met SQL via PHP][step-4-connect-resiliently-to-sql-with-php-p42h]

<a id="k-test-retry-logic" name="k-test-retry-logic"></a>

### <a name="test-your-retry-logic"></a>Test uw logica voor opnieuw proberen

Als u wilt testen uw logica voor opnieuw proberen, moet u simuleren of treedt er een fout die kan worden gecorrigeerd terwijl uw programma wordt nog steeds uitgevoerd.

#### <a name="test-by-disconnecting-from-the-network"></a>Testen door het netwerk verbreekt

Eén manier kunt u uw logica voor opnieuw proberen testen is uw client-computer ontkoppelt van het netwerk terwijl het programma wordt uitgevoerd. De fout is:

- **SqlException.Number** 11001 =
- Bericht: "Er is geen host is onbekend"

Als onderdeel van de eerste nieuwe poging, uw programma het verkeerd gespeld woord corrigeren en vervolgens probeert om verbinding te maken.

Als u deze test praktische, moet u uw computer via het netwerk loskoppelen voordat u begint met uw programma. Uw programma herkent vervolgens een Runtimeparameter die ervoor zorgt het programma dat:

- 11001 tijdelijk toevoegen aan de lijst met fouten te beschouwen als tijdelijke.
- De eerste verbinding gewoon proberen.
- Nadat de fout is aangetroffen, verwijdert u 11001 uit de lijst.
- Een bericht waarin de gebruiker om te koppelen van de computer aan het netwerk weergegeven.
- Onderbreken verder kan worden uitgevoerd met behulp van de **Console.ReadLine** methode of een dialoogvenster met de knop OK. De gebruiker op de Enter-toets drukt nadat de computer is aangesloten op het netwerk.
- Probeer nogmaals om verbinding te maken geslaagd wordt verwacht.

#### <a name="test-by-misspelling-the-database-name-when-connecting"></a>Testen door de naam van de database bevat een typefout wanneer verbinding wordt gemaakt

Het programma kan de naam van de gebruiker voordat de eerste verbindingspoging opzettelijk gespeld. De fout is:

- **SqlException.Number** 18456 =
- Bericht: "Aanmelden mislukt voor gebruiker 'WRONG_MyUserName'."

Als onderdeel van de eerste nieuwe poging, uw programma het verkeerd gespeld woord corrigeren en vervolgens probeert om verbinding te maken.

Als u deze test praktische, herkent uw programma een Runtimeparameter die ervoor zorgt het programma dat:

- Tijdelijk 18456 toevoegen aan de lijst met fouten te beschouwen als tijdelijke.
- Opzettelijk 'WRONG_' toevoegen aan de gebruikersnaam.
- Nadat de fout is aangetroffen, verwijdert u 18456 uit de lijst.
- Verwijder 'WRONG_' uit de naam van de gebruiker.
- Probeer nogmaals om verbinding te maken geslaagd wordt verwacht.

<a id="net-sqlconnection-parameters-for-connection-retry" name="net-sqlconnection-parameters-for-connection-retry"></a>

## <a name="net-sqlconnection-parameters-for-connection-retry"></a>.NET SqlConnection-parameters voor de verbinding opnieuw proberen

Als uw clientprogramma verbinding met SQL-Database maakt met behulp van de .NET Framework-klasse **System.Data.SqlClient.SqlConnection**, .NET 4.6.1 gebruiken of hoger (of .NET Core) zodat u de functie van de verbinding opnieuw proberen kunt. Zie voor meer informatie over de functie [deze webpagina](https://docs.microsoft.com/dotnet/api/system.data.sqlclient.sqlconnection).

<!--
2015-11-30, FwLink 393996 points to dn632678.aspx, which links to a downloadable .docx related to SqlClient and SQL Server 2014.
-->

Wanneer u bouwt de [verbindingsreeks](https://msdn.microsoft.com/library/System.Data.SqlClient.SqlConnection.connectionstring.aspx) voor uw **SqlConnection** object, de coördinatie van de waarden onder de volgende parameters:

- **ConnectRetryCount**:&nbsp;&nbsp;standaardwaarde is 1. Bereik is 0 tot en met 255.
- **ConnectRetryInterval**:&nbsp;&nbsp;standaardwaarde is 1 seconde. Bereik ligt tussen 1 en 60.
- **Time-out voor verbindingen**:&nbsp;&nbsp;standaardwaarde is 15 seconden. Bereik is 0 en 2147483647.

De gekozen waarden moeten specifiek, moet u de volgende gelijkheid true: Time-out voor verbindingen ConnectRetryCount = * ConnectionRetryInterval

Bijvoorbeeld, als de telling is gelijk aan 3 en het interval is gelijk aan 10 seconden, geeft is een time-out van alleen 29 seconden geen het systeem genoeg tijd voor de derde en laatste opnieuw proberen om verbinding te maken: 29 < 3 * 10.

<a id="connection-versus-command" name="connection-versus-command"></a>

## <a name="connection-vs-command"></a>Verbinding versus opdracht

De **ConnectRetryCount** en **ConnectRetryInterval** parameters, kunnen uw **SqlConnection** object probeer het opnieuw verbinding maken zonder te vertellen of proberen alles uit uw programma, zoals het besturingselement aan uw programma wordt geretourneerd. De nieuwe pogingen kunnen optreden in de volgende situaties:

- de methodeaanroep mySqlConnection.Open
- de methodeaanroep mySqlConnection.Execute

Er is een geldt de volgende werkwijze. Als er een tijdelijke fout optreedt terwijl uw *query* wordt uitgevoerd, uw **SqlConnection** object niet probeer het opnieuw verbinding maken. Het opnieuw niet zeker de query. Echter, **SqlConnection** zeer snel controleert of de verbinding voordat de query voor de uitvoering wordt verzonden. Als de snelle controle een verbindingsprobleem detecteert **SqlConnection** opnieuw probeert het opnieuw verbinding maken. Als de nieuwe poging is gelukt, wordt voor de uitvoering van uw query verzonden.

### <a name="should-connectretrycount-be-combined-with-application-retry-logic"></a>Moeten ConnectRetryCount worden gecombineerd met de logica voor opnieuw proberen van toepassing

Stel dat uw toepassing heeft krachtige aangepaste Pogingslogica. Dit kan de bewerking opnieuw proberen verbinding maken met vier keer. Als u toevoegt **ConnectRetryInterval** en **ConnectRetryCount** = 3 op uw verbindingsreeks, verhoogt u het aantal nieuwe pogingen tot en met 4 * 3 = 12 nieuwe pogingen. U kunt niet van plan bent die een groot aantal nieuwe pogingen.

<a id="a-connection-connection-string" name="a-connection-connection-string"></a>

## <a name="connections-to-sql-database"></a>Verbindingen met SQL Database

<a id="c-connection-string" name="c-connection-string"></a>

### <a name="connection-connection-string"></a>Verbinding: Verbindingsreeks

De verbindingsreeks dat nodig is om verbinding maken met SQL Database is enigszins afwijken van de tekenreeks die wordt gebruikt voor verbinding met SQL Server. U kunt de verbindingsreeks kopiëren voor uw database uit de [Azure-portal](https://portal.azure.com/).

[!INCLUDE [sql-database-include-connection-string-20-portalshots](../../includes/sql-database-include-connection-string-20-portalshots.md)]

<a id="b-connection-ip-address" name="b-connection-ip-address"></a>

### <a name="connection-ip-address"></a>Verbinding: IP-adres

U moet de SQL-Database-server voor het accepteren van communicatie van het IP-adres van de computer die als host fungeert voor uw clientprogramma configureren. Als u deze configuratie instelt, bewerkt u de firewall-instellingen via de [Azure-portal](https://portal.azure.com/).

Als u vergeet het configureren van het IP-adres, wordt uw programma mislukt met een handige foutbericht waarin wordt aangegeven van de vereiste IP-adres.

[!INCLUDE [sql-database-include-ip-address-22-portal](../../includes/sql-database-include-ip-address-22-v12portal.md)]

Zie voor meer informatie, [firewall-instellingen configureren op de SQL-Database](sql-database-configure-firewall-settings.md).
<a id="c-connection-ports" name="c-connection-ports"></a>

### <a name="connection-ports"></a>Verbinding: Poorten

Normaal gesproken moet u ervoor zorgen dat alleen poort 1433 is geopend voor uitgaande communicatie op de computer die als host fungeert voor uw clientprogramma.

Bijvoorbeeld, wanneer uw clientprogramma wordt gehost op een Windows-computer, kunt u Windows Firewall op de host naar poort 1433 openen.

1. Open het Configuratiescherm.
2. Selecteer **alle onderdelen in het Configuratiescherm** > **Windows Firewall** > **geavanceerde instellingen** > **regels voor uitgaand verkeer**   >  **Acties** > **nieuwe regel**.

Als uw clientprogramma wordt gehost op een Azure-machine (VM), leest u [poorten boven 1433 voor ADO.NET 4.5 en SQL-Database](sql-database-develop-direct-route-ports-adonet-v12.md).

Zie voor achtergrondinformatie over het configureren van poorten en IP-adressen, [Azure SQL Database-firewall](sql-database-firewall-configure.md).

<a id="d-connection-ado-net-4-5" name="d-connection-ado-net-4-5"></a>

### <a name="connection-adonet-462-or-later"></a>Verbinding: ADO.NET 4.6.2 of hoger

Als uw programma maakt gebruik van ADO.NET-klassen, zoals **System.Data.SqlClient.SqlConnection** voor verbinding met SQL-Database, raden wij aan dat u .NET Framework versie 4.6.2 of hoger.

#### <a name="starting-with-adonet-462"></a>Beginnen met ADO.NET 4.6.2

- De open verbindingspoging opnieuw onmiddellijk wordt uitgevoerd voor Azure SQL-databases, waardoor de prestaties van apps met cloud-functionaliteit.

#### <a name="starting-with-adonet-461"></a>Beginnen met ADO.NET 4.6.1

- Voor SQL-Database, de betrouwbaarheid is verbeterd bij het openen van een verbinding met behulp van de **SqlConnection.Open** methode. De **Open** methode bevat nu mechanismen voor het best-effort nieuwe pogingen in reactie op tijdelijke fouten voor bepaalde fouten binnen de time-outperiode.
- Groepsgewijze verbinding wordt ondersteund, waaronder een doeltreffende controle die het verbindingsobject verzekert uw programma werkt.

Wanneer u een verbindingsobject van een verbindingsgroep wordt gebruikt, wordt u aangeraden dat uw programma tijdelijk de verbinding wordt gesloten wanneer het niet onmiddellijk wordt gebruikt. Is het niet duur om opnieuw een verbinding te openen, maar het is om een nieuwe verbinding te maken.

Als u werkt met ADO.NET 4.0 of eerder gebruikt, raden wij aan dat u een upgrade uitvoert naar de meest recente ADO.NET. Vanaf augustus 2018, kunt u [downloaden ADO.NET 4.6.2](https://blogs.msdn.microsoft.com/dotnet/20../../announcing-the-net-framework-4-7-2/).

<a id="e-diagnostics-test-utilities-connect" name="e-diagnostics-test-utilities-connect"></a>

## <a name="diagnostics"></a>Diagnostiek

<a id="d-test-whether-utilities-can-connect" name="d-test-whether-utilities-can-connect"></a>

### <a name="diagnostics-test-whether-utilities-can-connect"></a>Diagnostische gegevens: Test of hulpprogramma's verbinding kunnen maken

Als uw programma geen verbinding maken met SQL Database, wordt een diagnostische optie is om te proberen om te verbinden met een hulpprogramma's. In het ideale geval het hulpprogramma maakt verbinding met behulp van dezelfde bibliotheek die gebruikmaakt van uw programma.

Op een Windows-computer, kunt u proberen deze hulpprogramma's:

- SQL Server Management Studio (ssms.exe), die verbinding maakt met behulp van ADO.NET
- `sqlcmd.exe`, die verbinding maakt met behulp van [ODBC](https://msdn.microsoft.com/library/jj730308.aspx)

Nadat uw programma is verbonden, moet u testen of een korte SQL SELECT-query werkt.

<a id="f-diagnostics-check-open-ports" name="f-diagnostics-check-open-ports"></a>

### <a name="diagnostics-check-the-open-ports"></a>Diagnostische gegevens: Controleer de poorten openen

Als u vermoedt dat verbindingspogingen vanwege problemen met de poort mislukken, kunt u een hulpprogramma uitvoeren op de computer die rapporten weergeven over de poortconfiguraties.

Op Linux, kunnen het nuttig zijn de volgende hulpprogramma's:

- `netstat -nap`
- `nmap -sS -O 127.0.0.1`: Wijzig de Voorbeeldwaarde om te worden van uw IP-adres.

Op Windows, de [PortQry.exe](https://www.microsoft.com/download/details.aspx?id=17148) hulpprogramma kan nuttig zijn. Hier volgt een voorbeeld van de uitvoering van die de poort situatie op een SQL Database-server een query uitgevoerd en die is uitgevoerd op een laptop:

```cmd
[C:\Users\johndoe\]
>> portqry.exe -n johndoesvr9.database.windows.net -p tcp -e 1433

Querying target system called: johndoesvr9.database.windows.net

Attempting to resolve name to IP address...
Name resolved to 23.100.117.95

querying...
TCP port 1433 (ms-sql-s service): LISTENING

[C:\Users\johndoe\]
>>
```

<a id="g-diagnostics-log-your-errors" name="g-diagnostics-log-your-errors"></a>

### <a name="diagnostics-log-your-errors"></a>Diagnostische gegevens: De fouten in logboek registreren

Een onregelmatig terugkerend probleem is soms beste gediagnosticeerd door detectie van een algemeen patroon via dagen of weken.

De client kan een diagnose helpen door logboekregistratie van alle fouten die worden aangetroffen. U kunt mogelijk de logboekvermeldingen correleren met foutgegevens die SQL-Database intern zelf registreert.

Enterprise-bibliotheek 6 (EntLib60) biedt beheerde .NET-klassen om u te helpen bij het aanmelden. Zie voor meer informatie, [5 - net zo gemakkelijk als een logboek vallen: Gebruik het Toepassingsblok voor logboekregistratie](https://msdn.microsoft.com/library/dn440731.aspx).

<a id="h-diagnostics-examine-logs-errors" name="h-diagnostics-examine-logs-errors"></a>

### <a name="diagnostics-examine-system-logs-for-errors"></a>Diagnostische gegevens: Controleer het systeemlogboek in Logboeken voor fouten

Hier volgen enkele Transact-SQL SELECT-instructies die foutenlogboeken en andere informatie opvragen.

| Query van logboek | Description |
|:--- |:--- |
| `SELECT e.*`<br/>`FROM sys.event_log AS e`<br/>`WHERE e.database_name = 'myDbName'`<br/>`AND e.event_category = 'connectivity'`<br/>`AND 2 >= DateDiff`<br/>&nbsp;&nbsp;`(hour, e.end_time, GetUtcDate())`<br/>`ORDER BY e.event_category,`<br/>&nbsp;&nbsp;`e.event_type, e.end_time;` |De [sys.event_log](https://msdn.microsoft.com/library/dn270018.aspx) weergave biedt informatie over afzonderlijke gebeurtenissen, waaronder enkele die leiden tijdelijke fouten of fouten in de netwerkconnectiviteit tot kunnen.<br/><br/>U kunt in het ideale geval correleren de **start_time** of **end_time** waarden met informatie over wanneer uw clientprogramma problemen ondervonden.<br/><br/>U moet verbinding maken met de *master* database deze query uit te voeren. |
| `SELECT c.*`<br/>`FROM sys.database_connection_stats AS c`<br/>`WHERE c.database_name = 'myDbName'`<br/>`AND 24 >= DateDiff`<br/>&nbsp;&nbsp;`(hour, c.end_time, GetUtcDate())`<br/>`ORDER BY c.end_time;` |De [sys.database_connection_stats](https://msdn.microsoft.com/library/dn269986.aspx) weergave biedt samengevoegde tellingen van gebeurtenistypen voor aanvullende diagnostische gegevens.<br/><br/>U moet verbinding maken met de *master* database deze query uit te voeren. |

<a id="d-search-for-problem-events-in-the-sql-database-log" name="d-search-for-problem-events-in-the-sql-database-log"></a>

### <a name="diagnostics-search-for-problem-events-in-the-sql-database-log"></a>Diagnostische gegevens: Zoeken naar gebeurtenissen van het probleem in het logboek voor SQL-Database

U kunt zoeken naar vermeldingen over probleem-gebeurtenissen in het logboek van de SQL-Database. Probeer de volgende Transact-SQL SELECT-instructie in de *master* database:

```sql
SELECT
   object_name
  ,CAST(f.event_data as XML).value
      ('(/event/@timestamp)[1]', 'datetime2')                      AS [timestamp]
  ,CAST(f.event_data as XML).value
      ('(/event/data[@name="error"]/value)[1]', 'int')             AS [error]
  ,CAST(f.event_data as XML).value
      ('(/event/data[@name="state"]/value)[1]', 'int')             AS [state]
  ,CAST(f.event_data as XML).value
      ('(/event/data[@name="is_success"]/value)[1]', 'bit')        AS [is_success]
  ,CAST(f.event_data as XML).value
      ('(/event/data[@name="database_name"]/value)[1]', 'sysname') AS [database_name]
FROM
  sys.fn_xe_telemetry_blob_target_read_file('el', null, null, null) AS f
WHERE
  object_name != 'login_event'  -- Login events are numerous.
  and
  '2015-06-21' < CAST(f.event_data as XML).value
        ('(/event/@timestamp)[1]', 'datetime2')
ORDER BY
  [timestamp] DESC
;
```

#### <a name="a-few-returned-rows-from-sysfnxetelemetryblobtargetreadfile"></a>Een aantal geretourneerde rijen uit sys.fn_xe_telemetry_blob_target_read_file

Het volgende voorbeeld laat zien hoe een geretourneerde rij eruit kan zien. De null-waarden die worden weergegeven, zijn vaak niet null zijn in andere rijen.

```
object_name                   timestamp                    error  state  is_success  database_name

database_xml_deadlock_report  2015-10-16 20:28:01.0090000  NULL   NULL   NULL        AdventureWorks
```

<a id="l-enterprise-library-6" name="l-enterprise-library-6"></a>

## <a name="enterprise-library-6"></a>Enterprise-bibliotheek 6

Enterprise-bibliotheek 6 (EntLib60) is een structuur van .NET-klassen die helpt u bij het implementeren van krachtige clients van cloudservices, waarbij een van de service SQL Database is. Zie het zoeken van onderwerpen die zijn toegewezen aan elk gebied waarin EntLib60 kan helpen [Enterprise Library 6: April 2013](https://msdn.microsoft.com/library/dn169621%28v=pandp.60%29.aspx).

Logica voor opnieuw proberen voor afhandeling van tijdelijke fouten is een gebied waarin EntLib60 kan helpen. Zie voor meer informatie, [4 - Perseverance, geheim van alle successen: Gebruik het Toepassingsblok voor afhandeling van tijdelijke fouten](https://msdn.microsoft.com/library/dn440719%28v=pandp.60%29.aspx).

> [!NOTE]
> De broncode voor EntLib60 is beschikbaar voor openbare downloaden van de [Downloadcentrum](https://go.microsoft.com/fwlink/p/?LinkID=290898). Microsoft heeft geen plannen om te maken voor verdere updates voor de functie of onderhoud EntLib.

<a id="entlib60-classes-for-transient-errors-and-retry" name="entlib60-classes-for-transient-errors-and-retry"></a>

### <a name="entlib60-classes-for-transient-errors-and-retry"></a>EntLib60 klassen voor tijdelijke fouten en probeer het opnieuw

De volgende EntLib60 klassen zijn bijzonder nuttig voor logica voor opnieuw proberen. Alle deze klassen zijn gevonden in of onder de naamruimte **Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling**.

In de naamruimte **Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling**:

- **RetryPolicy** klasse
  - **ExecuteAction** methode
- **ExponentialBackoff** klasse
- **SqlDatabaseTransientErrorDetectionStrategy** klasse
- **ReliableSqlConnection** klasse
  - **ExecuteCommand** methode

In de naamruimte **Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling.TestSupport**:

- **AlwaysTransientErrorDetectionStrategy** klasse
- **NeverTransientErrorDetectionStrategy** klasse

Hier zijn enkele koppelingen naar informatie over EntLib60:

- Gratis eBook downloaden: [Ontwikkelaarshandleiding voor Microsoft Enterprise Library, 2e editie](https://www.microsoft.com/download/details.aspx?id=41145).
- Aanbevolen procedures: [Algemene richtlijnen voor opnieuw proberen](../best-practices-retry-general.md) is een uitstekende uitgebreide beschrijving van de logica voor opnieuw proberen.
- Downloaden van NuGet: [Enterprise Library - tijdelijke fouten afhandelen Application Block 6.0](http://www.nuget.org/packages/EnterpriseLibrary.TransientFaultHandling/).

<a id="entlib60-the-logging-block" name="entlib60-the-logging-block"></a>

### <a name="entlib60-the-logging-block"></a>EntLib60: De registratie blokkeren

- Het blok logboekregistratie is een zeer flexibele en configureerbare oplossing die u kunt gebruiken voor:
  - Maken en opslaan van berichten in het logboek in een groot aantal locaties.
  - Categoriseren en filteren van berichten.
  - Contextuele informatie verzameld die handig voor foutopsporing en tracering, evenals voor controle en algemene logboekregistratie vereisten.
- Het blok logboekregistratie isoleert de logboekregistratie van de doel-logboek, zodat de toepassingscode consistent, ongeacht de locatie en het type van de opslag van de doel-logboekregistratie is.

Zie voor meer informatie, [5 - net zo gemakkelijk als een logboek vallen: Gebruik het Toepassingsblok voor logboekregistratie](https://msdn.microsoft.com/library/dn440731%28v=pandp.60%29.aspx).

<a id="entlib60-istransient-method-source-code" name="entlib60-istransient-method-source-code"></a>

### <a name="entlib60-istransient-method-source-code"></a>EntLib60 IsTransient methode broncode

Vervolgens uit de **SqlDatabaseTransientErrorDetectionStrategy** klasse, is de C#-broncode voor de **IsTransient** methode. De broncode wordt uitleg gegeven over welke fouten zijn beschouwd als tijdelijk en daling van nieuwe pogingen vanaf April 2013.

```csharp
public bool IsTransient(Exception ex)
{
  if (ex != null)
  {
    SqlException sqlException;
    if ((sqlException = ex as SqlException) != null)
    {
      // Enumerate through all errors found in the exception.
      foreach (SqlError err in sqlException.Errors)
      {
        switch (err.Number)
        {
            // SQL Error Code: 40501
            // The service is currently busy. Retry the request after 10 seconds.
            // Code: (reason code to be decoded).
          case ThrottlingCondition.ThrottlingErrorNumber:
            // Decode the reason code from the error message to
            // determine the grounds for throttling.
            var condition = ThrottlingCondition.FromError(err);

            // Attach the decoded values as additional attributes to
            // the original SQL exception.
            sqlException.Data[condition.ThrottlingMode.GetType().Name] =
              condition.ThrottlingMode.ToString();
            sqlException.Data[condition.GetType().Name] = condition;

            return true;

          case 10928:
          case 10929:
          case 10053:
          case 10054:
          case 10060:
          case 40197:
          case 40540:
          case 40613:
          case 40143:
          case 233:
          case 64:
            // DBNETLIB Error Code: 20
            // The instance of SQL Server you attempted to connect to
            // does not support encryption.
          case (int)ProcessNetLibErrorCode.EncryptionNotSupported:
            return true;
        }
      }
    }
    else if (ex is TimeoutException)
    {
      return true;
    }
    else
    {
      EntityException entityException;
      if ((entityException = ex as EntityException) != null)
      {
        return this.IsTransient(entityException.InnerException);
      }
    }
  }

  return false;
}
```

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over het oplossen van andere veelvoorkomende problemen met een SQL-Database-verbindingen [verbindingsfouten oplossen Azure SQL Database](sql-database-troubleshoot-common-connection-issues.md).
- [Verbindingsbibliotheken voor SQL-Database en SQL Server](sql-database-libraries.md)
- [SQL Server groepsgewijze verbindingen (ADO.NET)](https://docs.microsoft.com/dotnet/framework/data/adonet/sql-server-connection-pooling)
- [*Opnieuw proberen* is een Apache-2.0 met een licentie voor algemeen gebruik-bibliotheek, geschreven in Python, opnieuw proberen](https://pypi.python.org/pypi/retrying) voor het vereenvoudigen van de taak van het gedrag voor opnieuw proberen aan bijna van alles toe te voegen.

<!-- Link references. -->

[step-4-connect-resiliently-to-sql-with-ado-net-a78n]: https://docs.microsoft.com/sql/connect/ado-net/step-4-connect-resiliently-to-sql-with-ado-net

[step-4-connect-resiliently-to-sql-with-php-p42h]: https://docs.microsoft.com/sql/connect/php/step-4-connect-resiliently-to-sql-with-php
