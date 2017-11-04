---
title: Een SQL-verbindingsfout, een tijdelijke fout oplossen | Microsoft Docs
description: 'Informatie over het oplossen van, diagnoses stellen en voorkomen dat een SQL-fout of een tijdelijke fout in Azure SQL Database. '
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
ms.date: 11/03/2017
ms.author: daleche
ms.openlocfilehash: dda284b45e2e8a35a7228d77afef0ad058c8ea42
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/04/2017
---
# <a name="troubleshoot-diagnose-and-prevent-sql-connection-errors-and-transient-errors-for-sql-database"></a>Oplossen, opsporen en voorkomen van SQL-verbindingsfouten en tijdelijke fouten voor SQL-database
In dit artikel wordt beschreven hoe voorkomen, oplossen, analyseren en beperken verbindingsfouten en tijdelijke fouten die uw clienttoepassing tegenkomt wanneer deze met Azure SQL Database samenwerkt. Informatie over het configureren van Pogingslogica, de verbindingsreeks en andere verbindingsinstellingen aanpassen.

<a id="i-transient-faults" name="i-transient-faults"></a>

## <a name="transient-errors-transient-faults"></a>Tijdelijke fouten (tijdelijke fouten)
Een tijdelijke fout - ook tijdelijke fout - heeft een oorzaak die wordt binnenkort automatisch opgelost. Een incidentele oorzaak van tijdelijke fouten is wanneer het Azure systeem snel hardwarebronnen betere taakverdeling verschillende workloads verschuift. De meeste van deze gebeurtenissen herconfiguratie vaak voltooid in minder dan 60 seconden. Tijdens deze tijdsspanne herconfiguratie mogelijk hebt u verbindingsproblemen met Azure SQL Database. Verbinding maken met Azure SQL Database toepassingen moeten worden samengesteld om verwacht dat deze tijdelijke fouten, wordt deze door het implementeren van Pogingslogica in de code in plaats van ze naar gebruikers als toepassingsfouten bepaalde te verwerken.

Als uw clientprogramma ADO.NET, het programma is adviseert over de tijdelijke fout door de throw van een **SqlException**. De **getal** eigenschap kan worden vergeleken in de lijst met tijdelijke fouten aan de bovenkant van het onderwerp: [SQL-foutcodes voor SQL-Database-clienttoepassingen](sql-database-develop-error-messages.md).

<a id="connection-versus-command" name="connection-versus-command"></a>

### <a name="connection-versus-command"></a>De verbinding ten opzichte van de opdracht
U probeert de SQL-verbinding of opnieuw instellen, afhankelijk van het volgende:

* **Een tijdelijke fout optreedt tijdens een verbindingspoging**: de verbinding moet opnieuw worden geprobeerd na enkele seconden vertraagd.
* **Een tijdelijke fout optreedt tijdens de opdracht van een SQL-query**: de opdracht mag niet onmiddellijk opnieuw uitgevoerd. In plaats daarvan na een vertraging de verbinding moet worden opnieuw worden gemaakt. Vervolgens de opdracht kan opnieuw worden geprobeerd.

<a id="j-retry-logic-transient-faults" name="j-retry-logic-transient-faults"></a>

### <a name="retry-logic-for-transient-errors"></a>Pogingslogica voor tijdelijke problemen
Client-programma's die soms treedt er een tijdelijke fout zijn krachtiger wanneer ze Pogingslogica bevatten.

Wanneer uw programma met Azure SQL Database via een 3e partij middleware communiceert, informeren met de leverancier of de middleware Pogingslogica voor tijdelijke fouten bevat.

<a id="principles-for-retry" name="principles-for-retry"></a>

#### <a name="principles-for-retry"></a>Principes voor een nieuwe poging
* Een poging om een verbinding te openen, moet opnieuw worden uitgevoerd als de fout tijdelijk is.
* Een SQL SELECT-instructie is mislukt met een tijdelijke fout moet niet opnieuw worden geprobeerd rechtstreeks.
  
  * In plaats daarvan een nieuwe verbinding tot stand brengen en probeer vervolgens de is geselecteerd.
* Wanneer een UPDATE van SQL-instructie is mislukt met een tijdelijke fout, moet een nieuwe verbinding worden ingesteld voordat de UPDATE wordt herhaald.
  
  * De Pogingslogica moet ervoor zorgen dat de gehele databasetransactie is voltooid of dat de hele transactie wordt teruggedraaid.

#### <a name="other-considerations-for-retry"></a>Andere overwegingen voor een nieuwe poging
* Een batch-programma die automatisch wordt gestart nadat de werkuren en die wordt voltooid voordat de ochtend toestaan van zeer geduld met lange tijdsintervallen tussen de nieuwe pogingen.
* Een gebruikersinterfaceprogramma moet rekening houden met de menselijke neiging te geven, na een wachttijd te lang.
  
  * De oplossing moet echter niet opnieuw proberen om de paar seconden worden omdat dit beleid kan het systeem met aanvragen overspoelen.

#### <a name="interval-increase-between-retries"></a>Verhoging van het interval tussen nieuwe pogingen
Het is raadzaam om u te stellen voor 5 seconden voordat de eerste poging. Opnieuw proberen na een vertraging die korter is dan 5 seconden risico's die zijn afgeleid van de cloudservice. Voor elke opeenvolgende pogingen de vertraging exponentieel, moet groeien maximaal 60 seconden.

Een bespreking van de *blokkerende periode* voor clients die gebruikmaken van ADO.NET is beschikbaar in [SQL Server-verbinding groeperen (ADO.NET)](http://msdn.microsoft.com/library/8xx3tyca.aspx).

U kunt ook het maximale aantal nieuwe pogingen ingesteld voordat het programma zelf wordt beëindigd.

#### <a name="code-samples-with-retry-logic"></a>Codevoorbeelden met Pogingslogica
Codevoorbeelden met Pogingslogica in diverse programmeertalen, zijn beschikbaar op:

* [Verbindingsbibliotheken voor SQL-Database en SQL Server](sql-database-libraries.md)

<a id="k-test-retry-logic" name="k-test-retry-logic"></a>

#### <a name="test-your-retry-logic"></a>Uw Pogingslogica testen
Als u wilt testen uw Pogingslogica, moet u simuleren of een fout veroorzaken, dan kan worden gecorrigeerd terwijl uw programma nog steeds actief is.

##### <a name="test-by-disconnecting-from-the-network"></a>Testen door het netwerk verbreekt
Een manier kunt u uw Pogingslogica testen is op uw computer loskoppelen van het netwerk terwijl het programma wordt uitgevoerd. De fout is:

* **SqlException.Number** 11001 =
* Bericht: "Er is geen host is onbekend'

Als onderdeel van de eerste nieuwe poging, uw programma Corrigeer de onjuist gespeld, en vervolgens probeert te verbinden.

Om dit praktisch, loskoppelen u uw computer via het netwerk voordat u uw programma. Het programma herkent vervolgens een runtime-parameter zorgt ervoor dat het programma:

1. 11001 tijdelijk toevoegen aan de lijst met fouten in overweging moet nemen als tijdelijk.
2. De eerste verbinding gewoon proberen.
3. Nadat de fout is opgetreden, 11001 uit de lijst verwijderen.
4. Een bericht aan van de gebruiker op de computer op het netwerk aansluiten weergeven.
   * Onderbreken verder worden uitgevoerd met behulp van de **Console.ReadLine** methode of een dialoogvenster met de knop OK. De gebruiker op de Enter-toets drukt nadat de computer is aangesloten op het netwerk.
5. Opnieuw verbinding probeert te, succes verwacht.

##### <a name="test-by-misspelling-the-database-name-when-connecting"></a>Testen door de databasenaam bevat een typefout wanneer verbinding wordt gemaakt
Het programma kan de naam van de gebruiker voordat de eerste verbindingspoging opzettelijk Maak een typefout. De fout is:

* **SqlException.Number** 18456 =
* Bericht: 'aanmelding is mislukt voor gebruiker 'WRONG_MyUserName'.'

Als onderdeel van de eerste nieuwe poging, uw programma Corrigeer de onjuist gespeld, en vervolgens probeert te verbinden.

Om dit praktisch, kan uw programma herkend door een runtime-parameter zorgt ervoor dat het programma:

1. Tijdelijk 18456 toevoegen aan de lijst met fouten in overweging moet nemen als tijdelijk.
2. Opzettelijk 'WRONG_' toevoegen aan de gebruikersnaam.
3. Nadat de fout is opgetreden, 18456 uit de lijst verwijderen.
4. 'WRONG_' verwijderen uit de gebruikersnaam.
5. Opnieuw verbinding probeert te, succes verwacht.

<a id="net-sqlconnection-parameters-for-connection-retry" name="net-sqlconnection-parameters-for-connection-retry"></a>

### <a name="net-sqlconnection-parameters-for-connection-retry"></a>.NET-SqlConnection parameters voor een nieuwe poging verbinding
Als uw clientprogramma verbindt met naar Azure SQL Database met behulp van de .NET Framework-klasse **System.Data.SqlClient.SqlConnection**, moet u .NET 4.6.1 of hoger (of .NET Core) zodat u van de functie van de verbinding opnieuw gebruikmaken kunt. Details van de functie zijn [hier](http://go.microsoft.com/fwlink/?linkid=393996).

<!--
2015-11-30, FwLink 393996 points to dn632678.aspx, which links to a downloadable .docx related to SqlClient and SQL Server 2014.
-->


Wanneer u bouwt het [verbindingsreeks](http://msdn.microsoft.com/library/System.Data.SqlClient.SqlConnection.connectionstring.aspx) voor uw **SqlConnection** object, moet u de waarden onder de volgende parameters coördineren:

* ConnectRetryCount &nbsp; &nbsp; *(de standaardwaarde is 1. Bereik is 0 tot en met 255.)*
* ConnectRetryInterval &nbsp; &nbsp; *(de standaardwaarde is 1 seconde. Bereik is 1 tot en met 60).*
* Verbindingstime-out &nbsp; &nbsp; *(de standaardwaarde is 15 seconden. Bereik is 0 tot 2147483647)*

In het bijzonder moet uw gekozen waarden de volgende gelijkheid true:

* Verbindingstime-out ConnectRetryCount = * ConnectionRetryInterval

Bijvoorbeeld, als het aantal = 3, en interval = 10 seconden, een time-out van alleen 29 seconden niet erg, het systeem genoeg tijd voor de 3e en final opnieuw op verbinding te maken krijgt: 29 < 3 * 10.

<a id="connection-versus-command" name="connection-versus-command"></a>

### <a name="connection-versus-command"></a>De verbinding ten opzichte van de opdracht
De **ConnectRetryCount** en **ConnectRetryInterval** parameters, kunnen uw **SqlConnection** object probeer het opnieuw verbinden zonder ontvangt of proberen alles uit uw programma, zoals het besturingselement wordt teruggezonden naar uw programma. De nieuwe pogingen kunnen optreden in de volgende situaties:

* de methodeaanroep mySqlConnection.Open
* de methodeaanroep mySqlConnection.Execute

Er is een geldt de volgende werkwijze. Als een tijdelijke fout optreedt terwijl uw *query* wordt uitgevoerd, uw **SqlConnection** object komt niet opnieuw verbinding maken en deze gewoon probeert niet opnieuw uw query. Echter, **SqlConnection** zeer snel controleert de verbinding voordat de query voor uitvoering worden verzonden. Als de snelle controle een verbindingsprobleem detecteert **SqlConnection** opnieuw probeert de bewerking verbinding maken. Als de nieuwe poging is gelukt, wordt u query verzonden voor uitvoering.

#### <a name="should-connectretrycount-be-combined-with-application-retry-logic"></a>Moet ConnectRetryCount worden gecombineerd met toepassingslogica opnieuw proberen?
Stel dat uw toepassing robuuste aangepaste Pogingslogica. Deze mogelijk opnieuw verbinding maken met 4 keer. Als u **ConnectRetryInterval** en **ConnectRetryCount** = 3 op uw verbindingsreeks verhoogt u het maximale aantal pogingen tot en met 4 * 3 = 12 nieuwe pogingen. U kunt niet van plan bent een groot aantal pogingen.

<a id="a-connection-connection-string" name="a-connection-connection-string"></a>

## <a name="connections-to-azure-sql-database"></a>Verbindingen met Azure SQL Database
<a id="c-connection-string" name="c-connection-string"></a>

### <a name="connection-connection-string"></a>Verbinding: De verbindingsreeks
De verbindingsreeks nodig voor het verbinden met Azure SQL Database is enigszins afwijken van de tekenreeks voor het verbinden met Microsoft SQL Server. U kunt de verbindingsreeks kopiëren voor uw database van de [Azure-portal](https://portal.azure.com/).

[!INCLUDE [sql-database-include-connection-string-20-portalshots](../../includes/sql-database-include-connection-string-20-portalshots.md)]

<a id="b-connection-ip-address" name="b-connection-ip-address"></a>

### <a name="connection-ip-address"></a>Verbinding: IP-adres
U moet de SQL-databaseserver voor het accepteren van communicatie van het IP-adres van de computer die als host fungeert voor uw clientprogramma configureren. U dit doen door het bewerken van de firewallinstellingen via de [Azure-portal](https://portal.azure.com/).

Als u vergeet voor het configureren van het IP-adres, mislukt het programma met een handige foutbericht dat aangeeft van de vereiste IP-adres.

[!INCLUDE [sql-database-include-ip-address-22-portal](../../includes/sql-database-include-ip-address-22-v12portal.md)]

Zie voor meer informatie: [procedure: firewall-instellingen configureren op de SQL-Database](sql-database-configure-firewall-settings.md)

<a id="c-connection-ports" name="c-connection-ports"></a>

### <a name="connection-ports"></a>Verbinding: poorten
Normaal gesproken alleen moet u ervoor zorgen dat poort 1433 is geopend voor uitgaande communicatie, op de computer waarop u clientprogramma wordt gehost.

Bijvoorbeeld, wanneer uw clientprogramma wordt gehost op een Windows-computer, kunt de Windows Firewall op de host u poort 1433 openen:

1. Open het Configuratiescherm
2. &gt;Alle Configuratiescherm-onderdelen
3. &gt;Windows Firewall
4. &gt;Geavanceerde instellingen
5. &gt;Regels voor uitgaande verbindingen
6. &gt;Acties
7. &gt;Nieuwe regel

Als wordt uw clientprogramma wordt gehost in Azure een virtuele machine (VM), dient u te lezen:<br/>[Poorten buiten 1433 voor ADO.NET 4.5 en SQL Database](sql-database-develop-direct-route-ports-adonet-v12.md).

Zie voor achtergrondinformatie over cofiguration van poorten en IP-adres: [Azure SQL Database-firewall](sql-database-firewall-configure.md)

<a id="d-connection-ado-net-4-5" name="d-connection-ado-net-4-5"></a>

### <a name="connection-adonet-461"></a>Verbinding: ADO.NET 4.6.1
Als het programma wordt gebruikt voor klassen als ADO.NET **System.Data.SqlClient.SqlConnection** voor verbinding met Azure SQL Database, het is raadzaam dat u .NET Framework versie 4.6.1 of hoger.

ADO.NET 4.6.1:

* Voor Azure SQL Database bestaat verbeterde betrouwbaarheid bij het openen van een verbinding via de **SqlConnection.Open** methode. De **Open** methode bevat nu best effort opnieuw mechanismen in reactie op tijdelijke fouten voor bepaalde fouten binnen de time-out voor verbinding.
* Ondersteunt Groepsgewijze verbinding nodig. Dit omvat een doeltreffende controle het verbindingsobject die dit biedt uw programma werkt.

Wanneer u een verbindingsobject van een verbindingsgroep gebruikt, wordt u aangeraden dat uw programma tijdelijk de verbinding niet sluiten wanneer deze niet direct gebruikt. Opnieuw een verbinding te openen is niet duur is van de manier waarop een nieuwe verbinding maken.

Als u van ADO.NET 4.0 gebruikmaakt of eerder, we raden u een upgrade uitvoert naar de meest recente ADO.NET.

* U kunt vanaf November 2015 [downloaden ADO.NET 4.6.1](http://blogs.msdn.com/b/dotnet/archive/2015/11/30/net-framework-4-6-1-is-now-available.aspx).

<a id="e-diagnostics-test-utilities-connect" name="e-diagnostics-test-utilities-connect"></a>

## <a name="diagnostics"></a>Diagnostiek
<a id="d-test-whether-utilities-can-connect" name="d-test-whether-utilities-can-connect"></a>

### <a name="diagnostics-test-whether-utilities-can-connect"></a>Diagnostische gegevens: Testen of hulpprogramma's verbinding kunnen maken
Als het programma kan niet verbinden met Azure SQL Database, wordt een diagnostische mogelijkheid is om te proberen om te verbinden met een hulpprogramma's. In het ideale geval zou het hulpprogramma verbinding maken met behulp van dezelfde bibliotheek dat het programma gebruikt.

Op elke Windows-computer probeert u deze hulpprogramma's:

* SQL Server Management Studio (ssms.exe) die is verbonden met ADO.NET.
* Sqlcmd.exe die verbinding met behulp van maakt [ODBC](http://msdn.microsoft.com/library/jj730308.aspx).

Eenmaal zijn verbonden, test u of een korte SQL SELECT-query werkt.

<a id="f-diagnostics-check-open-ports" name="f-diagnostics-check-open-ports"></a>

### <a name="diagnostics-check-the-open-ports"></a>Diagnostische gegevens: Controleer de geopende poorten
Stel dat u vermoedt dat verbindingspogingen vanwege problemen met de poort mislukken. U kunt een hulpprogramma waarmee u over de poortconfiguraties rapporten uitvoeren op uw computer.

Op Linux kunnen het handig zijn de volgende hulpprogramma's:

* `netstat -nap`
* `nmap -sS -O 127.0.0.1`
  * (De Voorbeeldwaarde om uw IP-adres wijzigen).

In Windows de [PortQry.exe](http://www.microsoft.com/download/details.aspx?id=17148) hulpprogramma kan handig zijn. Hier volgt een voorbeeld van de uitvoering die de situatie poort op een Azure SQL Database-server een query uitgevoerd en die is uitgevoerd op een laptopcomputer:

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

De client kan helpen bij het een diagnose door logboekregistratie van alle fouten die worden aangetroffen. U kunt mogelijk de logboekvermeldingen correleren met foutgegevens met Azure SQL Database intern zichzelf registreert.

Enterprise-bibliotheek 6 (EntLib60) biedt beheerde klassen om te helpen bij logboekregistratie:

* [5 - zo eenvoudig als het een logboek vallen: met behulp van de logboekregistratie Application Block](http://msdn.microsoft.com/library/dn440731.aspx)

<a id="h-diagnostics-examine-logs-errors" name="h-diagnostics-examine-logs-errors"></a>

### <a name="diagnostics-examine-system-logs-for-errors"></a>Diagnostische gegevens: Raadpleeg het systeemlogboek in Logboeken op fouten
Hier volgen enkele Transact-SQL SELECT-instructies die querylogboeken van de fout en andere informatie.

| Query van het logboek | Beschrijving |
|:--- |:--- |
| `SELECT e.*`<br/>`FROM sys.event_log AS e`<br/>`WHERE e.database_name = 'myDbName'`<br/>`AND e.event_category = 'connectivity'`<br/>`AND 2 >= DateDiff`<br/>&nbsp;&nbsp;`(hour, e.end_time, GetUtcDate())`<br/>`ORDER BY e.event_category,`<br/>&nbsp;&nbsp;`e.event_type, e.end_time;` |De [sys.event_log](http://msdn.microsoft.com/library/dn270018.aspx) weergave biedt informatie over afzonderlijke gebeurtenissen, inclusief een aantal die leiden tijdelijke fouten of verbindingsproblemen tot kunnen.<br/><br/>U kunt in het ideale geval correleren de **start_time** of **end_time** waarden met informatie over wanneer uw clientprogramma problemen ondervonden.<br/><br/>**TIP:** u moet verbinding maken met de **master** database uitvoeren. |
| `SELECT c.*`<br/>`FROM sys.database_connection_stats AS c`<br/>`WHERE c.database_name = 'myDbName'`<br/>`AND 24 >= DateDiff`<br/>&nbsp;&nbsp;`(hour, c.end_time, GetUtcDate())`<br/>`ORDER BY c.end_time;` |De [sys.database_connection_stats](http://msdn.microsoft.com/library/dn269986.aspx) weergave biedt cumulatief aantal gebeurtenistypen, voor aanvullende diagnostische gegevens.<br/><br/>**TIP:** u moet verbinding maken met de **master** database uitvoeren. |

<a id="d-search-for-problem-events-in-the-sql-database-log" name="d-search-for-problem-events-in-the-sql-database-log"></a>

### <a name="diagnostics-search-for-problem-events-in-the-sql-database-log"></a>Diagnostische gegevens: Zoek naar gebeurtenissen probleem in de SQL-Database
U kunt zoeken naar vermeldingen over probleem gebeurtenissen in het logboek van Azure SQL Database. Probeer de volgende Transact-SQL SELECT-instructie in de **master** database:

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
Hierna volgt wat een geretourneerde rij als volgt uitzien. De null-waarden weergegeven zijn vaak niet null zijn in andere rijen.

```
object_name                   timestamp                    error  state  is_success  database_name

database_xml_deadlock_report  2015-10-16 20:28:01.0090000  NULL   NULL   NULL        AdventureWorks
```


<a id="l-enterprise-library-6" name="l-enterprise-library-6"></a>

## <a name="enterprise-library-6"></a>Enterprise-bibliotheek 6
Enterprise-bibliotheek 6 (EntLib60) is een raamwerk van .NET-klassen waarmee u het implementeren van krachtige clients van cloud-services, waarbij een van de Azure SQL Database-service is. U kunt de onderwerpen die zijn toegewezen aan elk gebied waarin EntLib60 kan helpen in eerste via vinden:

* [Enterprise-bibliotheek 6 April 2013](http://msdn.microsoft.com/library/dn169621%28v=pandp.60%29.aspx)

Pogingslogica voor tijdelijke foutafhandeling is een gebied waarin EntLib60 kan helpen:

* [4 - perseverance, geheim van alle successen: met behulp van de blokkering van de toepassing afhandeling van tijdelijke fout](http://msdn.microsoft.com/library/dn440719%28v=pandp.60%29.aspx)

> [!NOTE]
> De broncode voor EntLib60 is beschikbaar voor openbare [downloaden](http://go.microsoft.com/fwlink/p/?LinkID=290898). Microsoft heeft geen plannen verdere functie-updates en onderhoud om aan te brengen EntLib.
> 
> 

<a id="entlib60-classes-for-transient-errors-and-retry" name="entlib60-classes-for-transient-errors-and-retry"></a>

### <a name="entlib60-classes-for-transient-errors-and-retry"></a>EntLib60 klassen voor tijdelijke fouten en probeer het opnieuw
De volgende EntLib60 klassen zijn bijzonder nuttig voor Pogingslogica. Al deze, of worden verdere onder de naamruimte **Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling**:

*In de naamruimte **Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling**:*

* **Het RetryPolicy** klasse
  
  * **ExecuteAction** methode
* **ExponentialBackoff** klasse
* **SqlDatabaseTransientErrorDetectionStrategy** klasse
* **ReliableSqlConnection** klasse
  
  * **ExecuteCommand** methode

In de naamruimte **Microsoft.Practices.EnterpriseLibrary.TransientFaultHandling.TestSupport**:

* **AlwaysTransientErrorDetectionStrategy** klasse
* **NeverTransientErrorDetectionStrategy** klasse

Hier vindt u koppelingen naar informatie over EntLib60:

* Gratis [downloaden van het adresboek: Ontwikkelaarshandleiding voor Microsoft Enterprise-bibliotheek, 2e Edition](http://www.microsoft.com/download/details.aspx?id=41145)
* Aanbevolen procedures: [Probeer algemene richtlijnen](../best-practices-retry-general.md) is een uitstekende gedetailleerdere beschrijving van Pogingslogica.
* Downloaden van NuGet [Enterprise Library - toepassing blok 6.0 afhandeling van tijdelijke fout](http://www.nuget.org/packages/EnterpriseLibrary.TransientFaultHandling/)

<a id="entlib60-the-logging-block" name="entlib60-the-logging-block"></a>

### <a name="entlib60-the-logging-block"></a>EntLib60: De logboekregistratie blokkeren
* De logboekregistratie voor scriptblokkering is een zeer flexibele en configureerbare oplossing waarmee u kunt:
  
  * Maak en logboekberichten opslaan in een groot aantal verschillende locaties.
  * Categoriseren en berichten filteren.
  * Contextuele informatie verzameld die handig voor foutopsporing en tracering, evenals voor controle en algemene logboekregistratie vereisten.
* Het blok logboekregistratie isoleert de logboekregistratie van de doel-logboek zodat de toepassingscode consistent, ongeacht de locatie en het type van het archief van de doel-logboekregistratie is.

Zie voor meer informatie: [5 - als gemakkelijk als vallen buiten een logboek: met behulp van de Logging Application Block](https://msdn.microsoft.com/library/dn440731%28v=pandp.60%29.aspx)

<a id="entlib60-istransient-method-source-code" name="entlib60-istransient-method-source-code"></a>

### <a name="entlib60-istransient-method-source-code"></a>EntLib60 IsTransient methode broncode
Vervolgens uit de **SqlDatabaseTransientErrorDetectionStrategy** klasse, is de C#-broncode voor de **IsTransient** methode. De broncode wordt uitleg gegeven over welke fouten zijn aangemerkt als een tijdelijke en leveringsopties opnieuw vanaf April 2013.

Talrijke **//comment** regels zijn verwijderd uit deze kopie benadrukken leesbaarheid.

```
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
* Voor het oplossen van andere veelvoorkomende verbindingsproblemen van Azure SQL Database, gaat u naar [verbindingsproblemen met Azure SQL Database oplossen](sql-database-troubleshoot-common-connection-issues.md).
* [SQL Server-verbinding (ADO.NET) groeperen](http://msdn.microsoft.com/library/8xx3tyca.aspx)
* [*Opnieuw proberen* is een Apache 2.0 in licentie gegeven algemene bibliotheek, geschreven in opnieuw **Python**, voor het vereenvoudigen van de taak van het gedrag voor het opnieuw op vrijwel elk element toevoegen.](https://pypi.python.org/pypi/retrying)

