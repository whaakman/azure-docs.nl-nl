---
title: Foutcodes voor SQL - fout in databaseverbinding | Microsoft Docs
description: "Meer informatie over SQL-foutcodes voor SQL-Database-clienttoepassingen, zoals algemene databasefouten verbinding met de database kopiëren en algemene fouten. "
keywords: SQL-foutcode, toegang tot sql, fout in databaseverbinding, sql-foutcodes
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: 
ms.assetid: 2a23e4ca-ea93-4990-855a-1f9f05548202
ms.service: sql-database
ms.custom: develop apps
ms.workload: Active
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/28/2017
ms.author: sstein
ms.openlocfilehash: 34e7142b5ca13ad8de5a4dbd380377abdf055c04
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2017
---
# <a name="sql-error-codes-for-sql-database-client-applications-database-connection-errors-and-other-issues"></a>SQL-foutcodes voor SQL-Database-clienttoepassingen: verbindingsfouten en andere problemen van de Database

Dit artikel bevat een overzicht van de SQL-foutcodes voor SQL-Database-clienttoepassingen, waaronder verbinding databasefouten, tijdelijke fouten (ook wel tijdelijke fouten), resource governance fouten, problemen met de database kopiëren, elastische pool en andere fouten. De meeste categorieën specifiek zijn voor Azure SQL Database en niet van toepassing op Microsoft SQL Server.

## <a name="database-connection-errors-transient-errors-and-other-temporary-errors"></a>Database-verbindingsfouten en tijdelijke fouten andere tijdelijke fouten
De volgende tabel bevat informatie over de SQL-foutcodes voor verbindingsfouten gegevensverlies en andere tijdelijke fouten die mogelijk optreden wanneer uw toepassing probeert te krijgen tot SQL-Database. Zie voor het ophalen van gestarte zelfstudies over het verbinden met Azure SQL Database, [verbinding te maken met Azure SQL Database](sql-database-libraries.md).

### <a name="most-common-database-connection-errors-and-transient-fault-errors"></a>Meest voorkomende fouten in database-verbinding en tijdelijke fout
De Azure-infrastructuur biedt de mogelijkheid dynamisch servers opnieuw te configureren wanneer zware workloads in de service SQL-Database optreden.  Dit dynamische gedrag kan ertoe leiden dat uw clientprogramma verliest de verbinding met SQL-Database. Dit type fout heet een *tijdelijke fout*.

Het is raadzaam dat uw clientprogramma Pogingslogica heeft zodat deze kan opnieuw verbinding te maken nadat u de tijd tijdelijke fout corrigeren zelf hebt.  Het is raadzaam om u te stellen voor 5 seconden voordat de eerste poging. Opnieuw proberen na een vertraging die korter is dan 5 seconden risico's die zijn afgeleid van de cloudservice. Voor elke opeenvolgende pogingen de vertraging exponentieel, moet groeien maximaal 60 seconden.

Tijdelijke fout fouten worden gewoonlijk manifest als een van de volgende foutberichten van uw client-programma's:

* Database &lt;db_name&gt; op server &lt;Azure_instance&gt; is momenteel niet beschikbaar. Probeer het later opnieuw verbinding. Als het probleem zich blijft voordoen, neem contact op met klantenondersteuning en geeft u de sessietracerings-ID van &lt;session_id&gt;
* Database &lt;db_name&gt; op server &lt;Azure_instance&gt; is momenteel niet beschikbaar. Probeer het later opnieuw verbinding. Als het probleem zich blijft voordoen, neem contact op met klantenondersteuning en geeft u de sessietracerings-ID van &lt;session_id&gt;. (Microsoft SQL Server, fout: 40613)
* Een bestaande verbinding is verbroken door de externe host.
* System.Data.Entity.Core.EntityCommandExecutionException: Er is een fout opgetreden tijdens het uitvoeren van de opdrachtdefinitie. Zie de binnenste uitzondering voor meer informatie. ---> System.Data.SqlClient.SqlException: Er is een fout transportniveau opgetreden tijdens het ontvangen van resultaten van de server. (provider: sessieprovider, fout: 19 - fysieke verbinding is niet bruikbaar)
* Een verbinding met een secundaire database is mislukt omdat de database momenteel reconfguration wordt en is bezig het toepassen van nieuwe pagina's, terwijl in het midden van een actieve line transactie op de primaire database. 

Zie voor voorbeelden van de programmacode van Pogingslogica:

* [Verbindingsbibliotheken voor SQL-Database en SQL Server](sql-database-libraries.md) 
* [Bewerkingen voor het oplossen van verbindingsfouten en tijdelijke fouten in SQL-Database](sql-database-connectivity-issues.md)

Een bespreking van de *blokkerende periode* voor clients die gebruikmaken van ADO.NET is beschikbaar in [SQL Server-verbinding groeperen (ADO.NET)](http://msdn.microsoft.com/library/8xx3tyca.aspx).

### <a name="transient-fault-error-codes"></a>Foutcodes voor tijdelijke fout
De volgende fouten zijn tijdelijk en moeten opnieuw worden geprobeerd in toepassingslogica: 

| Foutcode | Ernst | Beschrijving |
| ---:| ---:|:--- |
| 4060 |16 |Kan database niet openen '%. & #x2a; ls' aangevraagd door de aanmelding. De aanmelding is mislukt. |
| 40197 |17 |De service heeft een fout bij het verwerken van uw aanvraag aangetroffen. Probeer het opnieuw. Foutcode: %d.<br/><br/>U ontvangt deze foutmelding wanneer de service uitgeschakeld als gevolg van de software of hardware-upgrades, hardwarefouten of andere problemen met betrekking tot failover is. De foutcode (%d) dat is ingesloten in het bericht van fout 40197 bevat aanvullende informatie over het type fout of failover die is opgetreden. Enkele voorbeelden van de fout codes zijn ingesloten in het bericht van fout 40197 zijn 40020, 40143 40166 en 40540.<br/><br/>Automatisch opnieuw verbinding te maken met uw SQL Database-server, maakt u verbinding met een gezonde kopie van uw database. Uw toepassing moet catch 40197, foutenlogboek de ingesloten foutcode (%d) binnen het bericht voor het oplossen van problemen en probeer opnieuw verbinding te maken met SQL-Database tot de resources beschikbaar zijn en de verbinding tot stand is gebracht opnieuw. |
| 40501 |20 |De service is momenteel bezet. De aanvraag opnieuw proberen na tien seconden. Incident-ID: %ls. Code: %d.<br/><br/>Zie voor meer informatie:<br/>• [Limieten voor azure SQL Database](sql-database-service-tiers.md). |
| 40613 |17 |Database '%. & #x2a; ls' op server '%. & #x2a; ls' is momenteel niet beschikbaar. Probeer het later opnieuw verbinding. Als het probleem zich blijft voordoen, neem contact op met klantenondersteuning en geeft u de sessietracerings-ID '%. & #x2a; ls'. |
| 49918 |16 |Kan aanvraag niet verwerken. Onvoldoende resources om aanvraag te verwerken.<br/><br/>De service is momenteel bezet. Probeer de aanvraag later opnieuw. |
| 49919 |16 |Proces kan geen aanvraag maken of bijwerken. Te veel bewerkingen maken of bijwerken uitgevoerd voor abonnement ' % ld '.<br/><br/>De service is bezet verwerking van meerdere maken of bijwerken van aanvragen voor uw abonnement of de server. Aanvragen die momenteel is geblokkeerd voor optimalisatie van de resource. Query [sys.dm_operation_status bevat](https://msdn.microsoft.com/library/dn270022.aspx) voor bewerkingen die in behandeling. Wacht tot de in behandeling zijnde maken of bijwerken aanvragen zijn voltooid of verwijder een van de in behandeling genomen aanvragen en uw aanvraag later opnieuw. |
| 49920 |16 |Kan aanvraag niet verwerken. Te veel bewerkingen uitgevoerd voor abonnement ' % ld '.<br/><br/>De service is bezig met de verwerking van meerdere aanvragen voor dit abonnement. Aanvragen die momenteel is geblokkeerd voor optimalisatie van de resource. Query [sys.dm_operation_status bevat](https://msdn.microsoft.com/library/dn270022.aspx) voor de bewerkingsstatus van de. Wacht totdat aanvragen in behandeling zijn voltooid of verwijder een van de in behandeling zijnde aanvragen en uw aanvraag later opnieuw. |
| 4221 |16 |Aanmelden bij lezen secundaire is mislukt vanwege een lange wachttijd op 'HADR_DATABASE_WAIT_FOR_TRANSITION_TO_VERSIONING'. De replica is niet beschikbaar voor aanmelding, omdat de versies van de rij ontbreken voor transacties die onderweg zijn als de replica werd opnieuw gebruikt. Het probleem kan worden opgelost door terugdraaien of vastleggen van de actieve transacties op de primaire replica. Instanties van dit probleem kunnen worden geminimaliseerd door te lang schrijftransacties op de primaire vermijden. |

## <a name="database-copy-errors"></a>Fouten in de database kopiëren
De volgende fouten kunnen opgetreden bij het kopiëren van een database in Azure SQL Database. Zie [Een Azure SQL Database kopiëren](sql-database-copy.md) voor meer informatie.

| Foutcode | Ernst | Beschrijving |
| ---:| ---:|:--- |
| 40635 |16 |Client met IP-adres '%. & #x2a; ls' is tijdelijk uitgeschakeld. |
| 40637 |16 |Maken van database-exemplaar is momenteel uitgeschakeld. |
| 40561 |16 |Kopiëren van de database is mislukt. De bron-of doeldatabase bestaat niet. |
| 40562 |16 |Kopiëren van de database is mislukt. De brondatabase is verwijderd. |
| 40563 |16 |Kopiëren van de database is mislukt. De doeldatabase is verwijderd. |
| 40564 |16 |Database-exemplaar is mislukt vanwege een interne fout. Doeldatabase en probeer het opnieuw. |
| 40565 |16 |Kopiëren van de database is mislukt. Er is niet meer dan 1 gelijktijdige databasekopie van dezelfde bron toegestaan. Doeldatabase en probeer het later opnieuw. |
| 40566 |16 |Database-exemplaar is mislukt vanwege een interne fout. Doeldatabase en probeer het opnieuw. |
| 40567 |16 |Database-exemplaar is mislukt vanwege een interne fout. Doeldatabase en probeer het opnieuw. |
| 40568 |16 |Kopiëren van de database is mislukt. Brondatabase is niet meer beschikbaar. Doeldatabase en probeer het opnieuw. |
| 40569 |16 |Kopiëren van de database is mislukt. Doeldatabase is niet meer beschikbaar. Doeldatabase en probeer het opnieuw. |
| 40570 |16 |Database-exemplaar is mislukt vanwege een interne fout. Doeldatabase en probeer het later opnieuw. |
| 40571 |16 |Database-exemplaar is mislukt vanwege een interne fout. Doeldatabase en probeer het later opnieuw. |

## <a name="resource-governance-errors"></a>Resource governance fouten
De volgende fouten worden veroorzaakt door overmatig gebruik van bronnen tijdens het werken met Azure SQL Database. Bijvoorbeeld:

* Een transactie is te lang geopend.
* Een transactie is te veel vergrendelingen.
* Een toepassing is er te veel geheugen.
* Een toepassing te veel verbruikt `TempDb` ruimte.

Verwante onderwerpen:

* Meer gedetailleerde informatie is hier beschikbaar: [limieten voor Azure SQL Database](sql-database-service-tiers.md).

| Foutcode | Ernst | Beschrijving |
| ---:| ---:|:--- |
| 10928 |20 |Resource-ID: %d. De limiet voor %s voor de database %d is en is bereikt. Zie voor meer informatie [http://go.microsoft.com/fwlink/?LinkId=267637](http://go.microsoft.com/fwlink/?LinkId=267637).<br/><br/>De Resource-ID geeft aan dat de resource die de limiet is bereikt. Voor werkthreads, de Resource-ID = 1. Voor sessies, de Resource-ID = 2.<br/><br/>Zie voor meer informatie over deze fout en hoe het probleem te verhelpen:<br/>• [Limieten voor azure SQL Database](sql-database-service-tiers.md). |
| 10929 |20 |Resource-ID: %d. De minimumgarantie %s is %d, maximumlimiet is %d, en het huidige gebruik voor de database is %d. De server is echter momenteel te druk bezet voor de ondersteuning van aanvragen die groter zijn dan %d voor deze database. Zie voor meer informatie [http://go.microsoft.com/fwlink/?LinkId=267637](http://go.microsoft.com/fwlink/?LinkId=267637). Anders, probeer het later opnieuw.<br/><br/>De Resource-ID geeft aan dat de resource die de limiet is bereikt. Voor werkthreads, de Resource-ID = 1. Voor sessies, de Resource-ID = 2.<br/><br/>Zie voor meer informatie over deze fout en hoe het probleem te verhelpen:<br/>• [Limieten voor azure SQL Database](sql-database-service-tiers.md). |
| 40544 |20 |De database heeft het quotum grootte bereikt. Partitioneer of verwijder gegevens, verwijder indexen of Raadpleeg de documentatie voor mogelijke oplossingen. |
| 40549 |16 |Sessie is beëindigd omdat er een langlopende transactie. Probeer uw transactie te verkorten. |
| 40550 |16 |De sessie is beëindigd omdat er te veel vergrendelingen heeft verkregen. Probeer het lezen of minder rijen in één transactie te wijzigen. |
| 40551 |16 |De sessie is beëindigd vanwege het overmatige `TEMPDB` informatie over het gebruik. Probeer de query om te beperken van de tijdelijke tabelruimte te wijzigen.<br/><br/>Als u van tijdelijke objecten gebruikmaakt, schijfruimte besparen in de `TEMPDB` database door tijdelijke objecten neer te zetten nadat ze niet langer nodig zijn voor de sessie. |
| 40552 |16 |De sessie is beëindigd vanwege het gebruik van ruimte overmatige transactielogboeken. Probeer minder rijen in één transactie te wijzigen.<br/><br/>Als u uitvoert bulksgewijs invoegen met behulp van de `bcp.exe` hulpprogramma of de `System.Data.SqlClient.SqlBulkCopy` klasse, probeert u de `-b batchsize` of `BatchSize` opties voor het beperken van het aantal rijen wordt gekopieerd naar de server in elke transactie. Als u opnieuw van een index met opbouwen de `ALTER INDEX` instructie, probeer het opnieuw met de `REBUILD WITH ONLINE = ON` optie. |
| 40553 |16 |De sessie is beëindigd vanwege een uitzonderlijk groot geheugengebruik. Probeer de query voor het verwerken van minder rijen te wijzigen.<br/><br/>Het aantal te verminderen `ORDER BY` en `GROUP BY` bewerkingen in uw code Transact-SQL vermindert de geheugenvereisten van de query. |

## <a name="elastic-pool-errors"></a>Elastische pool fouten
De volgende fouten zijn gerelateerd aan het maken en gebruiken van elastische pools:

| Foutnummer | ErrorSeverity | ErrorFormat | ErrorInserts | ErrorCause | ErrorCorrectiveAction |
|:--- |:--- |:--- |:--- |:--- |:--- |
| 1132 |EX_RESOURCE |De elastische groep heeft de opslaglimiet bereikt. Het opslaggebruik voor de elastische groep kan niet groter zijn dan (%d) MB. |Limiet voor elastische pool schijfruimte in MB. |Poging om gegevens te schrijven naar een database als de opslaglimiet van de elastische groep is bereikt. |Vergroot het aantal dtu's van de elastische groep indien mogelijk om de opslaglimiet bereikt vergroten, verkleinen van de opslag van afzonderlijke databases binnen de elastische groep of databases uit de elastische groep verwijderd. |
| 10929 |EX_USER |De minimumgarantie %s is %d, maximumlimiet is %d, en het huidige gebruik voor de database is %d. De server is echter momenteel te druk bezet voor de ondersteuning van aanvragen die groter zijn dan %d voor deze database. Zie [http://go.microsoft.com/fwlink/?LinkId=267637](http://go.microsoft.com/fwlink/?LinkId=267637) voor hulp. Anders, probeer het later opnieuw. |Minimale aantal DTU's per database. Maximale aantal DTU's per database |Het totale aantal gelijktijdige werknemers (aanvragen) voor alle databases in de elastische groep heeft geprobeerd zijn dan de limiet van de. |Vergroot het aantal dtu's van de elastische groep indien mogelijk om de limiet van de werknemer verhogen of databases uit de elastische groep verwijderd. |
| 40844 |EX_USER |Database '%ls' op Server '%ls' is een '%ls'-database in een elastische pool en een relatie doorlopend kopiëren kan hebben. |databasenaam, de database-editie, de servernaam |Een opdracht StartDatabaseCopy is uitgegeven voor een niet-premium-database in een elastische pool. |Binnenkort beschikbaar |
| 40857 |EX_USER |Elastische groep niet vinden voor server: '%ls', naam elastische groep: '%ls'. |naam van de server. de naam van de elastische groep |Opgegeven elastische groep bestaat niet in de opgegeven server. |Geef een geldige elastische pool-naam. |
| 40858 |EX_USER |Elastische groep '%ls' bestaat al in de server: '%ls' |de naam van de elastische groep, servernaam |Opgegeven elastische groep bestaat al in de opgegeven logische server. |Geef de naam van een nieuwe elastische pool. |
| 40859 |EX_USER |Elastische groep biedt geen ondersteuning voor servicelaag '%ls'. |elastische groepservicelaag |Laag van de opgegeven service wordt niet ondersteund voor het inrichten van de elastische groep. |Geef de juiste editie of servicelaag de servicelaag standaard leeg laten. |
| 40860 |EX_USER |Combinatie van elastische groep '%ls' en de service doelstelling '%ls' is ongeldig. |de naam van de elastische groep; naam van serviceniveaudoelstelling |Elastische pool en service doelstelling tegelijk worden opgegeven als de servicedoelstelling is opgegeven als 'ElasticPool'. |Geef de juiste combinatie van elastische pool en servicedoelstelling. |
| 40861 |EX_USER |De database-editie is %. *ls kunnen niet anders dan de elastische groepservicelaag die is ' %.* ls'. |database-editie, elastische groepservicelaag |De database-editie is anders dan de elastische groepservicelaag. |Geef een database-editie die verschilt van de elastische groepservicelaag geen.  Houd er rekening mee dat de database-editie niet hoeft te worden opgegeven. |
| 40862 |EX_USER |De naam van de elastische groep moet worden opgegeven als de servicedoelstelling voor de elastische groep is opgegeven. |Geen |Elastische pool servicedoelstelling vormt geen unieke identificatie voor een elastische pool. |Geef de naam van de elastische groep bij het gebruik van de servicedoelstelling voor de elastische groep. |
| 40864 |EX_USER |Het aantal dtu's voor de elastische groep moet ten minste (%d) dtu's voor servicelaag ' % * ls'. |Aantal dtu's voor de elastische groep; elastische groepservicelaag. |Probeert in te stellen van het aantal dtu's voor de elastische groep onder het minimum. |Probeer het opnieuw instellen van het aantal dtu's voor de elastische groep naar ten minste het minimum. |
| 40865 |EX_USER |Het aantal dtu's voor de elastische groep niet langer zijn dan (%d) dtu's voor servicelaag ' % * ls'. |Aantal dtu's voor de elastische groep; elastische groepservicelaag. |Probeert in te stellen van het aantal dtu's voor de elastische groep is hoger dan de maximumlimiet. |Probeer het opnieuw instellen van het aantal dtu's voor de elastische groep op langer zijn dan de maximumlimiet. |
| 40867 |EX_USER |De DTU max per database moet ten minste (%d) voor servicelaag ' % * ls'. |Maximale aantal DTU's per database. elastische groepservicelaag |Wilt het maximale aantal DTU's per database onder de ondersteunde limiet ingesteld. | onsider met behulp van de elastische groepservicelaag die ondersteuning biedt voor de gewenste instelling. |
| 40868 |EX_USER |De DTU max per database kan niet groter zijn dan (%d) voor servicelaag ' % * ls'. |Maximale aantal DTU's per database. elastische groepservicelaag. |Wilt het maximale aantal DTU's per database voorbij de ondersteunde limiet ingesteld. | Overweeg het gebruik van de elastische groepservicelaag die ondersteuning biedt voor de gewenste instelling. |
| 40870 |EX_USER |Het minimale aantal DTU's per database kan niet groter zijn dan (%d) voor servicelaag ' % * ls'. |Minimale aantal DTU's per database. elastische groepservicelaag. |Probeert in te stellen het minimale aantal DTU's per database buiten het ondersteunde maximum. | Overweeg het gebruik van de elastische groepservicelaag die ondersteuning biedt voor de gewenste instelling. |
| 40873 |EX_USER |Het aantal databases (%d) en het minimale aantal DTU's per database (%d) kan niet groter zijn dan het aantal dtu's van de elastische groep (%d). |Aantal databases in elastische pool; Minimale aantal DTU's per database. Dtu's van elastische pool. |Bij het minimale aantal DTU's voor databases in de elastische groep die langer is dan het aantal dtu's van de elastische pool opgeven. | Vergroot het aantal dtu's van de elastische groep of het minimale aantal DTU's per database te verkleinen of verminder het aantal databases in de elastische pool. |
| 40877 |EX_USER |Een elastische groep kan niet worden verwijderd tenzij het bevat niet alle databases. |Geen |De elastische groep bevat een of meer databases en kan niet worden verwijderd. |Verwijderen databases uit de elastische groep om te verwijderen. |
| 40881 |EX_USER |De elastische groep ' % * ls heeft de database count-limiet bereikt.  De limiet voor het aantal database voor de elastische groep kan niet groter zijn dan (%d) voor een elastische groep met dtu's (%d). |Naam van de elastische pool; limiet voor het aantal database van de elastische pool; edtu's voor resourcegroep. |Probeert te maken of database toevoegen aan de elastische groep wanneer de limiet voor het aantal database van de elastische groep is bereikt. | Vergroot het aantal dtu's van de elastische groep indien mogelijk om de limiet van databases verhogen of databases uit de elastische groep verwijderd. |
| 40889 |EX_USER |Het aantal dtu's of de opslaglimiet voor de elastische groep ' % * ls kunnen niet worden verhoogd omdat die niet voldoende opslagruimte voor de databases wilt opgeven. |De naam van de elastische pool. |Probeert te verkleinen van de opslaglimiet van de elastische groep lager dan het gebruik van de opslag. | Beperk het gebruik van de opslag van de afzonderlijke databases in de elastische groep of databases uit de groep verwijderen om te reduceren de dtu's of de opslaglimiet bereikt. |
| 40891 |EX_USER |Het minimale aantal DTU's per database (%d) kan niet groter zijn dan het maximale aantal DTU's per database (%d). |Minimale aantal DTU's per database. DTU max per database. |Wilt het minimale aantal DTU's per database hoger is dan het maximale aantal DTU's per database instellen. |Zorg ervoor dat het minimale aantal DTU's per databases niet langer zijn dan het maximale aantal DTU's per database. |
| NOG TE BEPALEN |EX_USER |De maximale grootte voor een individuele database in een elastische groep kan niet groter zijn dan de maximale grootte die is toegestaan door ' % * ls' service tier elastische pool. |elastische groepservicelaag |De maximale grootte voor de database overschrijdt de maximale grootte die is toegestaan door de elastische groepservicelaag. |De maximale grootte van de database binnen de grenzen van de maximale grootte die is toegestaan door de elastische groepservicelaag ingesteld. |

Verwante onderwerpen:

* [Een elastische pool (C#) maken](sql-database-elastic-pool-manage-csharp.md) 
* [Een elastische pool (C#) beheren](sql-database-elastic-pool-manage-csharp.md). 
* [Maken van een elastische pool (PowerShell)](sql-database-elastic-pool-manage-powershell.md) 
* [Bewaken en beheren van een elastische pool (PowerShell)](sql-database-elastic-pool-manage-powershell.md).

## <a name="general-errors"></a>Algemene fouten
De volgende fouten vallen niet in de vorige categorieën.

| Foutcode | Ernst | Beschrijving |
| ---:| ---:|:--- |
| 15006 |16 |(AdministratorLogin) is niet een geldige naam omdat deze ongeldige tekens bevat. |
| 18452 |14 |Aanmelden is mislukt. De aanmelding is afkomstig uit een niet-vertrouwd domein en kan niet worden gebruikt met Windows authentication.%. & #x2a; ls (Windows-aanmeldingen worden niet ondersteund in deze versie van SQL Server.) |
| 18456 |14 |Aanmelding mislukt voor gebruiker ' % #x2a;ls'.%. & #x2a; % ls. & #x2a; ls (de aanmelding is mislukt voor gebruiker '%. & #x2a; ls'. Wijzigen van het wachtwoord is mislukt. Wijzigen van wachtwoorden tijdens het aanmelden wordt niet ondersteund in deze versie van SQL Server.) |
| 18470 |14 |Aanmelding mislukt voor gebruiker '%. & #x2a; ls'. Reden: Het account is disabled.%. & #x2a; ls |
| 40014 |16 |Meerdere databases kunnen niet worden gebruikt in dezelfde transactie. |
| 40054 |16 |Tabellen zonder geclusterde index worden niet ondersteund in deze versie van SQL Server. Een geclusterde index maken en probeer het opnieuw. |
| 40133 |15 |Deze bewerking wordt niet ondersteund in deze versie van SQL Server. |
| 40506 |16 |De opgegeven SID is ongeldig voor deze versie van SQL Server. |
| 40507 |16 |' %. & #x2a; ls kunnen niet worden aangeroepen met parameters in deze versie van SQL Server. |
| 40508 |16 |De instructie USE wordt niet ondersteund voor schakelen tussen databases. Een nieuwe verbinding om verbinding met een andere database te gebruiken. |
| 40510 |16 |De instructie '%. & #x2a; ls' wordt niet ondersteund in deze versie van SQL Server |
| 40511 |16 |De ingebouwde functie '%. & #x2a; ls' wordt niet ondersteund in deze versie van SQL Server. |
| 40512 |16 |Afgeschafte functie '%ls' wordt niet ondersteund in deze versie van SQL Server. |
| 40513 |16 |Server variabele '%. & #x2a; ls' wordt niet ondersteund in deze versie van SQL Server. |
| 40514 |16 |'%ls' wordt niet ondersteund in deze versie van SQL Server. |
| 40515 |16 |Verwijzing naar de database en/of de server-naam in '%. & #x2a; ls' wordt niet ondersteund in deze versie van SQL Server. |
| 40516 |16 |Globale, tijdelijke objecten worden niet ondersteund in deze versie van SQL Server. |
| 40517 |16 |Trefwoord-of instructieoptie '%. & #x2a; ls' wordt niet ondersteund in deze versie van SQL Server. |
| 40518 |16 |DBCC-opdracht '%. & #x2a; ls' wordt niet ondersteund in deze versie van SQL Server. |
| 40520 |16 |De beveiligbare klasse '% S_MSG' wordt niet ondersteund in deze versie van SQL Server. |
| 40521 |16 |De beveiligbare klasse '% S_MSG' wordt niet ondersteund in het serverbereik in deze versie van SQL Server. |
| 40522 |16 |Database-principal '%. & #x2a; ls'-type wordt niet ondersteund in deze versie van SQL Server. |
| 40523 |16 |Maken van het impliciete gebruiker '%. & #x2a; ls' wordt niet ondersteund in deze versie van SQL Server. Maak de gebruiker expliciet voordat u deze gebruikt. |
| 40524 |16 |Gegevenstype '%. & #x2a; ls' wordt niet ondersteund in deze versie van SQL Server. |
| 40525 |16 |MET '%.ls' wordt niet ondersteund in deze versie van SQL Server. |
| 40526 |16 |' %. & #x2a; rijensetprovider ls niet ondersteund in deze versie van SQL Server. |
| 40527 |16 |Gekoppelde servers worden niet ondersteund in deze versie van SQL Server. |
| 40528 |16 |Gebruikers kunnen niet worden toegewezen aan certificaten, asymmetrische sleutels of Windows-aanmeldingen in deze versie van SQL Server. |
| 40529 |16 |De ingebouwde functie '%. & #x2a; ls' in imitatie context niet wordt ondersteund in deze versie van SQL Server. |
| 40532 |11 |Server kan niet worden geopend '%. & #x2a; ls' aangevraagd door de aanmelding. De aanmelding is mislukt. |
| 40553 |16 |De sessie is beëindigd vanwege een uitzonderlijk groot geheugengebruik. Probeer de query voor het verwerken van minder rijen te wijzigen.<br/><br/> Het aantal te verminderen `ORDER BY` en `GROUP BY` bewerkingen in uw code Transact-SQL reduceert de geheugenvereisten van de query. |
| 40604 |16 |Kan geen CREATE/ALTER DATABASE omdat deze het quotum van de server overschreden. |
| 40606 |16 |Databases koppelen wordt niet ondersteund in deze versie van SQL Server. |
| 40607 |16 |Windows-aanmeldingen worden niet ondersteund in deze versie van SQL Server. |
| 40611 |16 |Servers kunnen maximaal 128 firewallregels zijn gedefinieerd hebben. |
| 40614 |16 |Eerste IP-adres van de firewallregel kan IP-eindadres niet overschrijden. |
| 40615 |16 |Kan de server '{0}' is aangevraagd door de aanmelding niet openen. Client met IP-adres '{1}' is niet toegestaan voor toegang tot de server.<br /><br />Voor toegang, gebruik van de SQL Database-Portal of voer sp\_ingesteld\_firewall\_-regel op de database master om een firewallregel voor dit IP-adres of -adresbereik te maken. Duurt maximaal vijf minuten om deze wijziging van kracht te laten worden. |
| 40617 |16 |Naam van de firewallregel die begint met (regelnaam) is te lang. Maximale lengte is 128. |
| 40618 |16 |Naam van de firewallregel mag niet leeg zijn. |
| 40620 |16 |De aanmelding is mislukt voor gebruiker '%. & #x2a; ls'. Wijzigen van het wachtwoord is mislukt. Wijzigen van wachtwoorden tijdens het aanmelden wordt niet ondersteund in deze versie van SQL Server. |
| 40627 |20 |Bewerking op server '{0}' en de database '{1}' wordt uitgevoerd. Wacht een paar minuten en probeer het opnieuw. |
| 40630 |16 |Wachtwoordvalidatie is mislukt. Het wachtwoord voldoet niet aan de beleidseisen omdat het te kort is. |
| 40631 |16 |Het wachtwoord die u hebt opgegeven, is te lang. Het wachtwoord moet langer zijn dan 128 tekens hebben. |
| 40632 |16 |Wachtwoordvalidatie is mislukt. Het wachtwoord voldoet niet aan de beleidseisen omdat het niet complex genoeg is. |
| 40636 |16 |Een gereserveerde databasenaam niet gebruiken '%. & #x2a; ls' in deze bewerking. |
| 40638 |16 |Ongeldige abonnements-id (abonnement-id). Abonnement bestaat niet. |
| 40639 |16 |Aanvraag voldoet niet aan schema: (Schemafout). |
| 40640 |20 |De server heeft een onverwachte uitzondering aangetroffen. |
| 40641 |16 |De opgegeven locatie is ongeldig. |
| 40642 |17 |De server is momenteel bezet. Probeer het later opnieuw. |
| 40643 |16 |De opgegeven x-ms-version-kopwaarde is ongeldig. |
| 40644 |14 |Kan geen toegang tot het opgegeven abonnement verlenen. |
| 40645 |16 |Servernaam (servername) mag niet leeg of null zijn. Deze kan alleen bestaan uit kleine letters 'a'-'z', de cijfers 0-9 en het afbreekstreepje. Het afbreekstreepje kan niet leiden of in de naam van de audittrail. |
| 40646 |16 |Abonnements-ID mag niet leeg zijn. |
| 40647 |16 |Abonnement (abonnement-id) is geen server (servername). |
| 40648 |17 |Zijn er te veel aanvragen uitgevoerd. Probeer het later opnieuw. |
| 40649 |16 |Ongeldig inhoudstype is opgegeven. Alleen application/xml wordt ondersteund. |
| 40650 |16 |Abonnement (abonnement-id) bestaat niet of is niet gereed voor werking. |
| 40651 |16 |Kan maken van de server omdat het abonnement (abonnement-id) is uitgeschakeld. |
| 40652 |16 |Kan niet worden verplaatst of gemaakt van de server. Abonnement (abonnement-id) zal de serverquota overschrijden. |
| 40671 |17 |Communicatiefout tussen de gateway en de management-service. Probeer het later opnieuw. |
| 40852 |16 |Kan database niet openen ' %. \*ls op server ' %. \*ls' aangevraagd door de aanmelding. Toegang tot de database is alleen toegestaan met een verbindingsreeks die door de beveiliging is ingeschakeld. Voor toegang tot deze database moet wijzigen verbindingsreeksen bevat beveiligde op de server FQDN - naam van server.database.windows .net moet worden gewijzigd naar 'servernaam'.database. `secure`. windows.net. |
| 40914 | 16 | Server kan niet worden geopend '*[servernaam]*is aangevraagd door de aanmelding. Client is niet toegestaan voor toegang tot de server.<br /><br />Als u wilt oplossen, overweeg een [virtueel netwerk regel](sql-database-vnet-service-endpoint-rule-overview.md). |
| 45168 |16 |Het SQL Azure-systeem wordt belast, en een bovenlimiet op gelijktijdige DB CRUD-bewerkingen voor één server is geplaatst (bijv, een database maken). De server die is opgegeven in het bericht heeft het maximum aantal gelijktijdige verbindingen overschreden. Probeer het later opnieuw. |
| 45169 |16 |Het SQL azure-systeem wordt belast en de bovengrens van het aantal gelijktijdige server CRUD-bewerkingen voor een abonnement van één wordt geplaatst (bijvoorbeeld een server maken). Het abonnement dat is opgegeven in het foutbericht is groter dan het maximum aantal gelijktijdige verbindingen en de aanvraag is geweigerd. Probeer het later opnieuw. |

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [functies van Azure SQL Database](sql-database-features.md).
* Meer informatie over [Servicelagen](sql-database-service-tiers.md).

