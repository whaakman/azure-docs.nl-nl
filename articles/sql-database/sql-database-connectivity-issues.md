---
title: Een SQL-verbindingsfout, een tijdelijke fout oplossen | Microsoft Docs
description: Informatie over het oplossen van, diagnoses stellen en voorkomen dat een SQL-fout of een tijdelijke fout in Azure SQL Database.
keywords: SQL-verbinding, verbindingsreeks, problemen met de netwerkverbinding, tijdelijke fout, is een verbindingsfout
services: sql-database
documentationcenter: 
author: dalechen
manager: cshepard
editor: 
ms.assetid: efb35451-3fed-4264-bf86-72b350f67d50
ms.service: sql-database
ms.custom: develop apps
ms.workload: On Demand
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 11/29/2017
ms.author: daleche
ms.openlocfilehash: 7d393cd08ef5c20ef680e4e1ab3aded191abe932
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="troubleshoot-diagnose-and-prevent-sql-connection-errors-and-transient-errors-for-sql-database"></a>Oplossen, opsporen en voorkomen van SQL-verbindingsfouten en tijdelijke fouten voor SQL-database
In dit artikel wordt beschreven hoe voorkomen, oplossen, analyseren en beperken verbindingsfouten en tijdelijke fouten die uw clienttoepassing tegenkomt wanneer deze met Azure SQL Database samenwerkt. Informatie over het configureren van Pogingslogica, de verbindingsreeks en andere verbindingsinstellingen aanpassen.

<a id="i-transient-faults" name="i-transient-faults"></a>

## <a name="transient-errors-transient-faults"></a>Tijdelijke fouten (tijdelijke fouten)
Een tijdelijke fout, ook wel bekend als een tijdelijke fout heeft een oorzaak die snel zichzelf wordt opgelost. Een incidentele oorzaak van tijdelijke fouten is wanneer het Azure systeem snel hardwarebronnen betere taakverdeling verschillende workloads verschuift. De meeste van deze gebeurtenissen herconfiguratie voltooid in minder dan 60 seconden. Tijdens deze tijdsspanne herconfiguratie wellicht verbindingsproblemen zijn met SQL-Database. Toepassingen die verbinding met SQL-Database maken moeten worden ontworpen om u te verwachten dat deze tijdelijke fouten. Om deze te verwerken, Pogingslogica in de code in plaats van ze naar gebruikers als toepassingsfouten bepaalde te implementeren.

Als uw clientprogramma ADO.NET gebruikt, uw programma over de tijdelijke fout is gemeld door de throw van **SqlException**. Vergelijk de **getal** van de eigenschap met de lijst met tijdelijke fouten die zijn gevonden bij de bovenkant van het artikel [SQL-foutcodes voor SQL-Database-clienttoepassingen](sql-database-develop-error-messages.md).

<a id="connection-versus-command" name="connection-versus-command"></a>

### <a name="connection-vs-command"></a>Verbinding versus opdracht
De SQL-verbinding opnieuw of opnieuw instellen, afhankelijk van het volgende:

* **Een tijdelijke fout optreedt tijdens een verbindingspoging**: na een vertraging van enkele seconden, probeert de verbinding.
* **Een tijdelijke fout optreedt tijdens de opdracht van een SQL-query**: niet direct opnieuw proberen de opdracht. In plaats daarvan na een vertraging na de verbinding te maken. Voer de opdracht vervolgens opnieuw uit.


<a id="j-retry-logic-transient-faults" name="j-retry-logic-transient-faults"></a>

## <a name="retry-logic-for-transient-errors"></a>Pogingslogica voor tijdelijke problemen
Client-programma's die soms treedt er een tijdelijke fout zijn krachtiger wanneer ze Pogingslogica bevatten.

Wanneer uw programma met SQL Database via middleware van derden communiceert, vraagt u de leverancier of de middleware Pogingslogica voor tijdelijke fouten bevat.

<a id="principles-for-retry" name="principles-for-retry"></a>

### <a name="principles-for-retry"></a>Principes voor een nieuwe poging
* Als de fout tijdelijke is, probeert u opnieuw om een verbinding openen.
* Een SQL SELECT-instructie is mislukt met een tijdelijke fout niet direct opnieuw.
  * In plaats daarvan een nieuwe verbinding tot stand brengen en probeer vervolgens de is geselecteerd.
* Wanneer een UPDATE van SQL-instructie is mislukt met een tijdelijke fout een nieuwe verbinding maken voordat u de UPDATE opnieuw.
  * De Pogingslogica moet ervoor zorgen dat de hele databasetransactie is voltooid of de hele transactie wordt teruggedraaid.

### <a name="other-considerations-for-retry"></a>Andere overwegingen voor een nieuwe poging
* Een batchprogramma dat automatisch wordt gestart nadat de werkuren en is voltooid voordat de ochtend kunt ambitieus zijn zeer patiënt met lange tijdsintervallen tussen de nieuwe pogingen.
* Een gebruikersinterfaceprogramma moet rekening houden met de menselijke neiging te geven, na een wachttijd te lang.
  * De oplossing moet geen nieuwe poging gedaan om de paar seconden, omdat dit beleid kan het systeem met aanvragen overspoelen.

### <a name="interval-increase-between-retries"></a>Verhoging van het interval tussen nieuwe pogingen
Het is raadzaam om te wachten op 5 seconden voordat de eerste poging. Opnieuw proberen na een vertraging die korter is dan 5 seconden risico's die zijn afgeleid van de cloudservice. Voor elke opeenvolgende pogingen de vertraging moet groeien exponentieel, maximaal 60 seconden.

Zie voor een beschrijving van de blokkering voor clients die gebruikmaken van ADO.NET [SQL Server-verbinding (ADO.NET) groeperen](http://msdn.microsoft.com/library/8xx3tyca.aspx).

Ook kunt u het maximale aantal nieuwe pogingen ingesteld voordat het programma zelf wordt beëindigd.

### <a name="code-samples-with-retry-logic"></a>Codevoorbeelden met Pogingslogica
Codevoorbeelden met Pogingslogica zijn beschikbaar op:

- [Resiliently verbinding te maken met SQL met ADO.NET][step-4-connect-resiliently-to-sql-with-ado-net-a78n]
- [Resiliently verbinding te maken met SQL met PHP][step-4-connect-resiliently-to-sql-with-php-p42h]

<a id="k-test-retry-logic" name="k-test-retry-logic"></a>

### <a name="test-your-retry-logic"></a>Uw Pogingslogica testen
Als u wilt testen uw Pogingslogica, moet u simuleren of treedt er een fout die kan worden gecorrigeerd terwijl uw programma nog steeds actief is.

#### <a name="test-by-disconnecting-from-the-network"></a>Testen door het netwerk verbreekt
Een manier kunt u uw Pogingslogica testen is op uw computer loskoppelen van het netwerk terwijl het programma wordt uitgevoerd. De fout is:

* **SqlException.Number** 11001 =
* Bericht: "Er is geen host is onbekend'

Als onderdeel van de eerste nieuwe poging, uw programma de onjuist gespeld corrigeren en vervolgens probeert om verbinding te.

Als u deze test praktische, moet u uw computer via het netwerk loskoppelen voordat u uw programma. Het programma herkent vervolgens een Runtimeparameter zorgt ervoor dat het programma:

* 11001 tijdelijk toevoegen aan de lijst met fouten in overweging moet nemen als tijdelijk.
* De eerste verbinding gewoon proberen.
* Nadat de fout is opgetreden, 11001 uit de lijst verwijderen.
* Een bericht waarin wordt gemeld dat de gebruiker de computer op het netwerk aansluiten weergegeven.
   * Onderbreken verder worden uitgevoerd met behulp van de **Console.ReadLine** methode of een dialoogvenster met de knop OK. De gebruiker op de Enter-toets drukt nadat de computer is aangesloten op het netwerk.
* Opnieuw verbinding probeert te, succes verwacht.

#### <a name="test-by-misspelling-the-database-name-when-connecting"></a>Testen door de databasenaam bevat een typefout wanneer verbinding wordt gemaakt
Het programma kan de naam van de gebruiker voordat de eerste verbindingspoging opzettelijk Maak een typefout. De fout is:

* **SqlException.Number** 18456 =
* Bericht: 'aanmelding is mislukt voor gebruiker 'WRONG_MyUserName'.'

Als onderdeel van de eerste nieuwe poging, uw programma de onjuist gespeld corrigeren en vervolgens probeert om verbinding te.

Als u deze test praktische, herkent het programma een Runtimeparameter zorgt ervoor dat het programma:

* Tijdelijk 18456 toevoegen aan de lijst met fouten in overweging moet nemen als tijdelijk.
* Opzettelijk 'WRONG_' toevoegen aan de gebruikersnaam.
* Nadat de fout is opgetreden, 18456 uit de lijst verwijderen.
* 'WRONG_' verwijderen uit de gebruikersnaam.
* Opnieuw verbinding probeert te, succes verwacht.


<a id="net-sqlconnection-parameters-for-connection-retry" name="net-sqlconnection-parameters-for-connection-retry"></a>

## <a name="net-sqlconnection-parameters-for-connection-retry"></a>.NET-SqlConnection parameters voor een nieuwe poging verbinding
Als uw clientprogramma verbinding met SQL Database met behulp van de .NET Framework-klasse **System.Data.SqlClient.SqlConnection**, gebruik .NET 4.6.1 of hoger (of .NET Core) zodat u kunt de functie van de verbinding opnieuw gebruiken. Zie voor meer informatie over de functie [deze webpagina](http://go.microsoft.com/fwlink/?linkid=393996).

<!--
2015-11-30, FwLink 393996 points to dn632678.aspx, which links to a downloadable .docx related to SqlClient and SQL Server 2014.
-->

Wanneer u bouwt het [verbindingsreeks](http://msdn.microsoft.com/library/System.Data.SqlClient.SqlConnection.connectionstring.aspx) voor uw **SqlConnection** object, coördinatie van de waarden onder de volgende parameters:

* **ConnectRetryCount**:&nbsp;&nbsp;standaardwaarde is 1. Bereik ligt tussen 0 en 255.
* **ConnectRetryInterval**:&nbsp;&nbsp;standaardwaarde is 1 seconde. Bereik ligt tussen 1 en 60.
* **Verbindingstime-out**:&nbsp;&nbsp;standaardwaarde is 15 seconden. Bereik is 0 en 2147483647.

In het bijzonder moet uw gekozen waarden de volgende gelijkheid true:

Verbindingstime-out ConnectRetryCount = * ConnectionRetryInterval

Bijvoorbeeld, als de telling is gelijk aan 3 en het interval is gelijk aan 10 seconden, een time-out van alleen 29 seconden geeft geen het systeem genoeg tijd voor de derde en laatste opnieuw verbinding maken: 29 < 3 * 10.

<a id="connection-versus-command" name="connection-versus-command"></a>

## <a name="connection-vs-command"></a>Verbinding versus opdracht
De **ConnectRetryCount** en **ConnectRetryInterval** parameters, kunnen uw **SqlConnection** object probeer het opnieuw verbinden zonder ontvangt of proberen alles uit uw programma, zoals het besturingselement wordt teruggezonden naar uw programma. De nieuwe pogingen kunnen optreden in de volgende situaties:

* de methodeaanroep mySqlConnection.Open
* de methodeaanroep mySqlConnection.Execute

Er is een geldt de volgende werkwijze. Als een tijdelijke fout optreedt terwijl uw *query* wordt uitgevoerd, uw **SqlConnection** object niet probeer het opnieuw verbinding maken. Dit wordt niet gewoon de query opnieuw. Echter, **SqlConnection** zeer snel controleert de verbinding voordat de query voor uitvoering worden verzonden. Als de snelle controle een verbindingsprobleem detecteert **SqlConnection** opnieuw probeert de bewerking verbinding maken. Als de nieuwe poging is gelukt, wordt uw query verzonden voor uitvoering.

### <a name="should-connectretrycount-be-combined-with-application-retry-logic"></a>Moet ConnectRetryCount worden gecombineerd met toepassingslogica opnieuw proberen?
Stel dat uw toepassing robuuste aangepaste Pogingslogica. Deze mogelijk opnieuw verbinding maken met vier keer. Als u **ConnectRetryInterval** en **ConnectRetryCount** = 3 op uw verbindingsreeks verhoogt u het maximale aantal pogingen tot en met 4 * 3 = 12 nieuwe pogingen. U kunt niet van plan bent een groot aantal pogingen.


<a id="a-connection-connection-string" name="a-connection-connection-string"></a>

## <a name="connections-to-sql-database"></a>Verbindingen met SQL Database
<a id="c-connection-string" name="c-connection-string"></a>

### <a name="connection-connection-string"></a>Verbinding: De verbindingsreeks
De verbindingsreeks dat nodig is om verbinding maken met SQL-Database is enigszins afwijken van de tekenreeks voor verbinding met SQL Server. U kunt de verbindingsreeks kopiëren voor uw database van de [Azure-portal](https://portal.azure.com/).

[!INCLUDE [sql-database-include-connection-string-20-portalshots](../../includes/sql-database-include-connection-string-20-portalshots.md)]

<a id="b-connection-ip-address" name="b-connection-ip-address"></a>

### <a name="connection-ip-address"></a>Verbinding: IP-adres
U moet de SQL-databaseserver voor het accepteren van communicatie van het IP-adres van de computer die als host fungeert voor uw clientprogramma configureren. Als u deze configuratie instelt, bewerkt u de firewall-instellingen via de [Azure-portal](https://portal.azure.com/).

Als u vergeet voor het configureren van het IP-adres, het programma is mislukt met een handige foutbericht dat aangeeft van de vereiste IP-adres.

[!INCLUDE [sql-database-include-ip-address-22-portal](../../includes/sql-database-include-ip-address-22-v12portal.md)]

Zie voor meer informatie [firewall-instellingen configureren op de SQL-Database](sql-database-configure-firewall-settings.md).
<a id="c-connection-ports" name="c-connection-ports"></a>

### <a name="connection-ports"></a>Verbinding: poorten
Normaal gesproken moet u ervoor zorgen dat alleen poort 1433 is geopend voor uitgaande communicatie op de computer die als host fungeert voor uw clientprogramma.

Bijvoorbeeld, wanneer uw clientprogramma wordt gehost op een Windows-computer, kunt u Windows Firewall op de host poort 1433 openen.

1. Open het Configuratiescherm.

2. Selecteer **alle onderdelen in het Configuratiescherm** > **Windows Firewall** > **geavanceerde instellingen** > **uitgaande regels**   >  **Acties** > **nieuwe regel**.

Als uw clientprogramma wordt gehost in Azure een virtuele machine (VM), leest u [poorten buiten 1433 voor ADO.NET 4.5 en SQL Database](sql-database-develop-direct-route-ports-adonet-v12.md).

Zie voor achtergrondinformatie over het configureren van poorten en IP-adressen, [Azure SQL Database-firewall](sql-database-firewall-configure.md).

<a id="d-connection-ado-net-4-5" name="d-connection-ado-net-4-5"></a>

### <a name="connection-adonet-461"></a>Verbinding: ADO.NET 4.6.1
Als het programma wordt gebruikt voor klassen als ADO.NET **System.Data.SqlClient.SqlConnection** voor verbinding met SQL-Database, het is raadzaam dat u .NET Framework versie 4.6.1 of hoger.

ADO.NET 4.6.1:

* Voor SQL-Database betrouwbaarheid is verbeterd wanneer u een verbinding met behulp van openen de **SqlConnection.Open** methode. De **Open** methode bevat nu best-effort opnieuw mechanismen in reactie op tijdelijke fouten voor bepaalde fouten binnen de time-outperiode.
* Verbindingsgroepering wordt ondersteund, waaronder een doeltreffende controle het verbindingsobject die dit biedt uw programma werkt.

Wanneer u een verbindingsobject van een verbindingsgroep gebruikt, wordt u aangeraden dat uw programma tijdelijk de verbinding niet sluiten wanneer het niet onmiddellijk in gebruik. Is geen dure om opnieuw een verbinding te openen, maar het is een nieuwe verbinding maken.

Als u ADO.NET 4.0 of eerder, we raden u een upgrade uitvoert naar de meest recente ADO.NET. U kunt vanaf November 2015 [downloaden ADO.NET 4.6.1](http://blogs.msdn.com/b/dotnet/archive/2015/11/30/net-framework-4-6-1-is-now-available.aspx).

<a id="e-diagnostics-test-utilities-connect" name="e-diagnostics-test-utilities-connect"></a>

## <a name="diagnostics"></a>Diagnostiek
<a id="d-test-whether-utilities-can-connect" name="d-test-whether-utilities-can-connect"></a>

### <a name="diagnostics-test-whether-utilities-can-connect"></a>Diagnostische gegevens: Testen of hulpprogramma's verbinding kunnen maken
Als uw programma verbinding maken met SQL-Database is mislukt, wordt een diagnostische mogelijkheid is om te proberen om te verbinden met een hulpprogramma's. In het ideale geval het hulpprogramma verbinding maakt met behulp van dezelfde bibliotheek dat het programma gebruikt.

Op elke Windows-computer probeert u deze hulpprogramma's:

* SQL Server Management Studio (ssms.exe) die is verbonden met ADO.NET
* Sqlcmd.exe die verbinding met behulp van maakt [ODBC](http://msdn.microsoft.com/library/jj730308.aspx)

Nadat het programma is aangesloten, test u of een korte SQL SELECT-query werkt.

<a id="f-diagnostics-check-open-ports" name="f-diagnostics-check-open-ports"></a>

### <a name="diagnostics-check-the-open-ports"></a>Diagnostische gegevens: Controleer de geopende poorten
Als u vermoedt dat er verbindingspogingen vanwege problemen met de poort mislukken, kunt u een hulpprogramma uitvoeren op de computer die over de poortconfiguraties rapporten.

Op Linux, kunnen het handig zijn de volgende hulpprogramma's:

* `netstat -nap`
* `nmap -sS -O 127.0.0.1`
  * Wijzig de Voorbeeldwaarde om uw IP-adres.

In Windows, de [PortQry.exe](http://www.microsoft.com/download/details.aspx?id=17148) hulpprogramma kan handig zijn. Hier volgt een voorbeeld van de uitvoering die de situatie poort op een SQL-Database-server een query uitgevoerd en die is uitgevoerd op een laptopcomputer:

```
[C:\Users\johndoe\]
>> portqry.exe -n johndoesvr9.database.windows.net -p tcp -e 1433

Querying target system called:
 johndoesvr9.database.windows.net

Attempting to resolve name to IP address...
Name resolved to 23.100.117.95

querying...
TCP port 1433 (ms-sql-s service): LISTENING

[C:\Users\johndoe\]
>>
```


<a id="g-diagnostics-log-your-errors" name="g-diagnostics-log-your-errors"></a>

### <a name="diagnostics-log-your-errors"></a>Diagnostische gegevens: Uw fouten in logboek registreren
Er is een onregelmatig probleem wordt soms beste gediagnosticeerd door detectie van een algemene patroon gedurende dagen of weken.

De client kan helpen bij het een diagnose door logboekregistratie van alle fouten die worden aangetroffen. U kunt mogelijk de logboekvermeldingen correleren met foutgegevens met SQL-Database intern zichzelf registreert.

Enterprise-bibliotheek 6 (EntLib60) biedt beheerde klassen om u te helpen bij het aanmelden. Zie voor meer informatie [5 - net zo eenvoudig als het een logboek vallen: Gebruik de Logging Application Block](http://msdn.microsoft.com/library/dn440731.aspx).

<a id="h-diagnostics-examine-logs-errors" name="h-diagnostics-examine-logs-errors"></a>

### <a name="diagnostics-examine-system-logs-for-errors"></a>Diagnostische gegevens: Raadpleeg het systeemlogboek in Logboeken op fouten
Hier volgen enkele Transact-SQL-instructies SELECT die query foutenlogboeken en andere informatie.

| Query van het logboek | Beschrijving |
|:--- |:--- |
| `SELECT e.*`<br/>`FROM sys.event_log AS e`<br/>`WHERE e.database_name = 'myDbName'`<br/>`AND e.event_category = 'connectivity'`<br/>`AND 2 >= DateDiff`<br/>&nbsp;&nbsp;`(hour, e.end_time, GetUtcDate())`<br/>`ORDER BY e.event_category,`<br/>&nbsp;&nbsp;`e.event_type, e.end_time;` |De [sys.event_log](http://msdn.microsoft.com/library/dn270018.aspx) weergave biedt informatie over afzonderlijke gebeurtenissen, waaronder die leiden tijdelijke fouten of verbindingsproblemen tot kunnen.<br/><br/>U kunt in het ideale geval correleren de **start_time** of **end_time** waarden met informatie over wanneer uw clientprogramma problemen ondervonden.<br/><br/>U moet verbinding maken met de *master* database deze query uit te voeren. |
| `SELECT c.*`<br/>`FROM sys.database_connection_stats AS c`<br/>`WHERE c.database_name = 'myDbName'`<br/>`AND 24 >= DateDiff`<br/>&nbsp;&nbsp;`(hour, c.end_time, GetUtcDate())`<br/>`ORDER BY c.end_time;` |De [sys.database_connection_stats](http://msdn.microsoft.com/library/dn269986.aspx) weergave biedt cumulatief aantal gebeurtenistypen voor aanvullende diagnostische gegevens.<br/><br/>U moet verbinding maken met de *master* database deze query uit te voeren. |

<a id="d-search-for-problem-events-in-the-sql-database-log" name="d-search-for-problem-events-in-the-sql-database-log"></a>

### <a name="diagnostics-search-for-problem-events-in-the-sql-database-log"></a>Diagnostische gegevens: Zoek naar gebeurtenissen probleem in de SQL-Database
U kunt zoeken naar vermeldingen over probleem gebeurtenissen in het logboek SQL-Database. Probeer de volgende Transact-SQL SELECT-instructie in de *master* database:

```
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


#### <a name="a-few-returned-rows-from-sysfnxetelemetryblobtargetreadfile"></a>Enkele geretourneerde rijen uit sys.fn_xe_telemetry_blob_target_read_file
Het volgende voorbeeld ziet wat een geretourneerde rij als volgt uitzien. De null-waarden weergegeven zijn vaak niet null zijn in andere rijen.

```
object_name                   timestamp                    error  state  is_success  database_name

database_xml_deadlock_report  2015-10-16 20:28:01.0090000  NULL   NULL   NULL        AdventureWorks
```


<a id="l-enterprise-library-6" name="l-enterprise-library-6"></a>

## <a name="enterprise-library-6"></a>Enterprise-bibliotheek 6
Enterprise-bibliotheek 6 (EntLib60) is een raamwerk van .NET-klassen waarmee u robuuste clients van cloud-services te implementeren waarbij een van de service SQL-Database is. Zie het zoeken van onderwerpen die zijn toegewezen aan elk gebied waarin EntLib60 kan helpen [Enterprise bibliotheek 6 April 2013](http://msdn.microsoft.com/library/dn169621%28v=pandp.60%29.aspx).

Pogingslogica voor tijdelijke foutafhandeling is een gebied waarin EntLib60 kan helpen. Zie voor meer informatie [4 - Perseverance, geheim van alle successen: gebruik van de tijdelijke fout afhandeling van Application Block](http://msdn.microsoft.com/library/dn440719%28v=pandp.60%29.aspx).

> [!NOTE]
> De broncode voor EntLib60 is gedownload van de [Downloadcentrum](http://go.microsoft.com/fwlink/p/?LinkID=290898). Microsoft heeft geen plannen verdere functie-updates en onderhoud om aan te brengen EntLib.
> 
> 

<a id="entlib60-classes-for-transient-errors-and-retry" name="entlib60-classes-for-transient-errors-and-retry"></a>

### <a name="entlib60-classes-for-transient-errors-and-retry"></a>EntLib60 klassen voor tijdelijke fouten en probeer het opnieuw
De volgende EntLib60 klassen zijn bijzonder nuttig voor Pogingslogica. Alle deze klassen zijn gevonden in of onder de naamruimte **Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling**.

In de naamruimte **Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling**:

* **Het RetryPolicy** klasse
  
  * **ExecuteAction** methode
* **ExponentialBackoff** klasse
* **SqlDatabaseTransientErrorDetectionStrategy** klasse
* **ReliableSqlConnection** klasse
  
  * **ExecuteCommand** methode

In de naamruimte **Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling.TestSupport**:

* **AlwaysTransientErrorDetectionStrategy** klasse
* **NeverTransientErrorDetectionStrategy** klasse

Hier volgen enkele koppelingen naar informatie over EntLib60:

* Gratis te downloaden: [Ontwikkelaarshandleiding voor bibliotheek van Microsoft Enterprise, 2e editie](http://www.microsoft.com/download/details.aspx?id=41145).
* Aanbevolen procedures: [Probeer algemene richtlijnen](../best-practices-retry-general.md) is een uitstekende gedetailleerdere beschrijving van Pogingslogica.
* NuGet downloaden: [Enterprise Library - tijdelijke fout afhandeling van toepassing blok 6.0](http://www.nuget.org/packages/EnterpriseLibrary.TransientFaultHandling/).

<a id="entlib60-the-logging-block" name="entlib60-the-logging-block"></a>

### <a name="entlib60-the-logging-block"></a>EntLib60: De logboekregistratie blokkeren
* De logboekregistratie voor scriptblokkering is een zeer flexibele en configureerbare oplossing die u kunt gebruiken voor:
  
  * Maak en logboekberichten opslaan in een groot aantal verschillende locaties.
  * Categoriseren en berichten filteren.
  * Contextuele informatie verzameld die handig voor foutopsporing en tracering, evenals voor controle en algemene logboekregistratie vereisten.
* Het blok logboekregistratie isoleert de logboekregistratie van de doel-logboek zodat de toepassingscode consistent, ongeacht de locatie en het type van het archief van de doel-logboekregistratie is.

Zie voor meer informatie [5 - net zo eenvoudig als het een logboek vallen: Gebruik de Logging Application Block](https://msdn.microsoft.com/library/dn440731%28v=pandp.60%29.aspx).

<a id="entlib60-istransient-method-source-code" name="entlib60-istransient-method-source-code"></a>

### <a name="entlib60-istransient-method-source-code"></a>EntLib60 IsTransient methode broncode
Vervolgens uit de **SqlDatabaseTransientErrorDetectionStrategy** klasse, is de C#-broncode voor de **IsTransient** methode. De broncode wordt uitleg gegeven over welke fouten zijn beschouwd als tijdelijke en leveringsopties opnieuw vanaf April 2013.

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
* Zie voor meer informatie over het oplossen van andere veelvoorkomende verbindingsproblemen voor SQL-Database [verbindingsproblemen met Azure SQL Database oplossen](sql-database-troubleshoot-common-connection-issues.md).
* [Verbindingsbibliotheken voor SQL-Database en SQL Server](sql-database-libraries.md)
* [SQL Server-verbinding (ADO.NET) groeperen](https://docs.microsoft.com/dotnet/framework/data/adonet/sql-server-connection-pooling)
* [*Opnieuw proberen* is een Apache 2.0 in licentie gegeven algemene bibliotheek, geschreven in Python, opnieuw proberen](https://pypi.python.org/pypi/retrying) gedrag voor het opnieuw toe te voegen om alles te vereenvoudigen.


<!-- Link references. -->

[step-4-connect-resiliently-to-sql-with-ado-net-a78n]: https://docs.microsoft.com/sql/connect/ado-net/step-4-connect-resiliently-to-sql-with-ado-net

[step-4-connect-resiliently-to-sql-with-php-p42h]: https://docs.microsoft.com/sql/connect/php/step-4-connect-resiliently-to-sql-with-php

