---
title: Foutcodes voor SQL - fout in databaseverbinding | Microsoft Docs
description: 'Meer informatie over SQL-foutcodes voor SQL Database-clienttoepassingen, zoals algemene databasefouten verbinding, problemen met de database kopiëren en algemene fouten. '
keywords: SQL-foutcode, toegang tot sql, fout in databaseverbinding, sql-foutcodes
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
manager: craigg
ms.date: 10/31/2018
ms.openlocfilehash: 00fe4e109df2ac8954e657a1a567842ec5eb7d37
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53317454"
---
# <a name="sql-error-codes-for-sql-database-client-applications-database-connection-errors-and-other-issues"></a>SQL-foutcodes voor SQL Database-clienttoepassingen: Database-verbindingsfouten en andere problemen

In dit artikel geeft een lijst van de SQL-foutcodes voor SQL Database-clienttoepassingen, met inbegrip van database-verbindingsfouten optreden, tijdelijke fouten (ook wel tijdelijke fouten), resource governance fouten, problemen met de database kopiëren, elastische pool en andere fouten. De meeste categorieën zijn afgestemd op Azure SQL Database en niet van toepassing op Microsoft SQL Server. Zie ook [system foutberichten](https://technet.microsoft.com/library/cc645603(v=sql.105).aspx).

## <a name="database-connection-errors-transient-errors-and-other-temporary-errors"></a>Database-verbindingsfouten en tijdelijke fouten andere tijdelijke fouten
De volgende tabel bevat informatie over de SQL-foutcodes voor verlies-verbindingsfouten en andere tijdelijke fouten die mogelijk optreden wanneer uw toepassing probeert te krijgen tot SQL-Database. Zie voor het aan de slag-zelfstudies over hoe u verbinding maakt met Azure SQL Database, [verbinden met Azure SQL Database](sql-database-libraries.md).

### <a name="most-common-database-connection-errors-and-transient-fault-errors"></a>Meest voorkomende database-verbindingsfouten en tijdelijke fouten fouten
De Azure-infrastructuur heeft de mogelijkheid om servers dynamisch opnieuw te configureren wanneer zware werklasten in de service SQL Database optreden.  Dit dynamische gedrag kan ertoe leiden dat uw clientprogramma verliest de verbinding met SQL-Database. Dit type fout heet een *tijdelijke fouten*.

Het is raadzaam dat uw clientprogramma logica voor opnieuw proberen heeft, zodat deze kan opnieuw verbinding te maken na waardoor de tijd van de tijdelijke fouten te corrigeren zelf.  Het is raadzaam dat u gedurende vijf seconden voordat u uw eerste opnieuw proberen uitstellen. Opnieuw proberen na een vertraging van minder dan 5 seconden risico's overbelasting van de cloudservice. Voor elke volgende opnieuw proberen de vertraging exponentieel toeneemt, moet vergroten tot maximaal 60 seconden.

Fouten van tijdelijke fouten manifest doorgaans als een van de volgende foutberichten van uw client-programma's:

* Database &lt;db_name&gt; op server &lt;Azure_instance&gt; is momenteel niet beschikbaar. Probeer opnieuw verbinding later opnieuw. Als het probleem zich blijft voordoen, neem contact op met klantenondersteuning en geeft u de sessietracerings-ID van &lt;type session_id&gt;
* Database &lt;db_name&gt; op server &lt;Azure_instance&gt; is momenteel niet beschikbaar. Probeer opnieuw verbinding later opnieuw. Als het probleem zich blijft voordoen, neem contact op met klantenondersteuning en geeft u de sessietracerings-ID van &lt;type session_id&gt;. (Microsoft SQL Server, fout: 40613)
* Een bestaande verbinding is geforceerd gesloten door de externe host.
* System.Data.Entity.Core.EntityCommandExecutionException: Er is een fout opgetreden tijdens het uitvoeren van de opdrachtdefinitie. Zie de binnenste uitzondering voor meer informatie. ---> System.Data.SqlClient.SqlException: Er is een op transportniveau-fout opgetreden bij het ontvangen van de resultaten van de server. (provider: Sessieprovider, fout: 19 - fysieke verbinding kan niet worden gebruikt)
* Een verbindingspoging naar een secundaire database is mislukt omdat de database momenteel herconfiguratie wordt en is bezet toepassen van de nieuwe pagina's, terwijl in het midden van een actieve transactie op de primaire database. 

Zie voor voorbeelden van de programmacode van logica voor opnieuw proberen:

* [Verbindingsbibliotheken voor SQL-Database en SQL Server](sql-database-libraries.md) 
* [Bewerkingen voor het oplossen van verbindingsfouten en tijdelijke fouten in SQL-Database](sql-database-connectivity-issues.md)

Een bespreking van de *blokkerende periode* voor clients die gebruikmaken van ADO.NET is beschikbaar in [SQL Server-verbinding groeperen (ADO.NET)](https://msdn.microsoft.com/library/8xx3tyca.aspx).

### <a name="transient-fault-error-codes"></a>Foutcodes voor tijdelijke fouten
De volgende fouten tijdelijk zijn en opnieuw moeten worden uitgevoerd in de logica van toepassingen: 

| Foutcode | Severity | Description |
| ---:| ---:|:--- |
| 4060 |16 |Kan database niet openen "%.&#x2a;ls" aangevraagd door de aanmelding. De aanmelding is mislukt. |
| 40197 |17 |De service heeft een fout bij het verwerken van uw aanvraag aangetroffen. Probeer het opnieuw. Foutcode %d.<br/><br/>U ontvangt deze foutmelding wanneer de service niet beschikbaar vanwege de software of hardware-upgrades, problemen met de hardware of andere problemen met betrekking tot failover is. De foutcode (%d) dat is ingesloten in het bericht van fout 40197 bevat aanvullende informatie over het soort fout of een failover die is opgetreden. Enkele voorbeelden van de fout codes zijn ingesloten in het bericht van fout 40197 zijn 40020, 40143 40166 en 40540.<br/><br/>Automatisch opnieuw verbinding te maken met uw SQL Database-server, maakt u verbinding met een goede kopie van uw database. Uw toepassing moet 40197, foutenlogboek de ingesloten foutcode (%d) in het bericht voor het oplossen van problemen ontdekken en probeer het opnieuw verbinding maken met SQL-Database, totdat de resources beschikbaar zijn en de verbinding opnieuw wordt ingesteld. |
| 40501 |20 |De service is momenteel bezet. De aanvraag opnieuw proberen na tien seconden. Incident-ID: %ls. Code: %d.<br/><br/>Zie voor meer informatie:<br/>• [Azure SQL Database-resourcebeperkingen](sql-database-service-tiers-dtu.md). |
| 40613 |17 |Database '%.&#x2a;ls' op server '%.&#x2a;ls' is momenteel niet beschikbaar. Probeer opnieuw verbinding later opnieuw. Als het probleem zich blijft voordoen, neem contact op met klantenondersteuning en geeft u de sessietracerings-ID "%.&#x2a;ls". |
| 49918 |16 |Kan aanvraag niet verwerken. Er is onvoldoende resources om aanvraag te verwerken.<br/><br/>De service is momenteel bezet. Probeer de aanvraag later opnieuw. |
| 49919 |16 |Proces kan maken of bijwerken van de aanvraag. Te veel bewerkingen maken of bijwerken uitgevoerd voor abonnement "% ld '.<br/><br/>De service is bezet verwerken van meerdere maken of bijwerken van aanvragen voor uw abonnement of de server. Aanvragen worden momenteel geblokkeerd voor resource-optimalisatie. Query [sys.dm_operation_status](https://msdn.microsoft.com/library/dn270022.aspx) voor bewerkingen die in behandeling. Wacht totdat in behandeling zijnde maken of bijwerken aanvragen voltooid zijn of verwijder een van de aanvragen in behandeling en probeer de aanvraag later opnieuw. |
| 49920 |16 |Kan aanvraag niet verwerken. Te veel bewerkingen uitgevoerd voor abonnement "% ld '.<br/><br/>De service is bezig met het verwerken van meerdere aanvragen voor dit abonnement. Aanvragen worden momenteel geblokkeerd voor resource-optimalisatie. Query [sys.dm_operation_status](https://msdn.microsoft.com/library/dn270022.aspx) voor de bewerkingsstatus van de. Wacht totdat in behandeling zijnde aanvragen zijn voltooid of verwijder een van de aanvragen in behandeling en probeer de aanvraag later opnieuw. |
| 4221 |16 |Meld u aan bij het lezen en secundaire is mislukt vanwege een lange wachttijd op 'HADR_DATABASE_WAIT_FOR_TRANSITION_TO_VERSIONING'. De replica is niet beschikbaar voor aanmelding, omdat de versies van de rij ontbreken voor transacties die die onderweg zijn zijn als de replica gerecycled is. Het probleem kan worden opgelost door terug te draaien of vastleggen van de actieve transacties op de primaire replica. Instanties van dit probleem kunnen worden geminimaliseerd door te vermijden om lange schrijftransacties op de primaire. |

## <a name="database-copy-errors"></a>Fouten in de database kopiëren
De volgende fouten kunnen worden aangetroffen tijdens het kopiëren van een database in Azure SQL Database. Zie [Een Azure SQL Database kopiëren](sql-database-copy.md) voor meer informatie.

| Foutcode | Severity | Description |
| ---:| ---:|:--- |
| 40635 |16 |Client met IP-adres "%.&#x2a;ls" is tijdelijk uitgeschakeld. |
| 40637 |16 |Maken van database-exemplaar is momenteel uitgeschakeld. |
| 40561 |16 |Database kopiëren is mislukt. De bron-of doeldatabase bestaat niet. |
| 40562 |16 |Database kopiëren is mislukt. De brondatabase is verwijderd. |
| 40563 |16 |Database kopiëren is mislukt. De doeldatabase is verwijderd. |
| 40564 |16 |Kopiëren is mislukt vanwege een interne fout van de database. Verwijder de doeldatabase en probeer het opnieuw. |
| 40565 |16 |Database kopiëren is mislukt. Er is niet meer dan 1 gelijktijdige databasekopie van dezelfde bron toegestaan. Verwijder de doeldatabase en probeer het later opnieuw. |
| 40566 |16 |Kopiëren is mislukt vanwege een interne fout van de database. Verwijder de doeldatabase en probeer het opnieuw. |
| 40567 |16 |Kopiëren is mislukt vanwege een interne fout van de database. Verwijder de doeldatabase en probeer het opnieuw. |
| 40568 |16 |Database kopiëren is mislukt. Brondatabase is niet meer beschikbaar. Verwijder de doeldatabase en probeer het opnieuw. |
| 40569 |16 |Database kopiëren is mislukt. Doeldatabase is niet meer beschikbaar. Verwijder de doeldatabase en probeer het opnieuw. |
| 40570 |16 |Kopiëren is mislukt vanwege een interne fout van de database. Verwijder de doeldatabase en probeer het later opnieuw. |
| 40571 |16 |Kopiëren is mislukt vanwege een interne fout van de database. Verwijder de doeldatabase en probeer het later opnieuw. |

## <a name="resource-governance-errors"></a>Resource governance fouten
De volgende fouten zijn veroorzaakt door overmatig gebruik van bronnen tijdens het werken met Azure SQL Database. Bijvoorbeeld:

* Een transactie is te lang geopend.
* Een transactie is te veel vergrendelingen.
* Een toepassing is te veel geheugen verbruikt.
* Een toepassing al gebruikmaakt van te veel `TempDb` ruimte.

Verwante onderwerpen:

* Meer gedetailleerde informatie is hier beschikbaar: [Azure SQL Database-resourcebeperkingen](sql-database-service-tiers-dtu.md).

| Foutcode | Severity | Description |
| ---:| ---:|:--- |
| 10928 |20 |Resource-ID: %d. De limiet voor %s voor de database is %d en is bereikt. Zie voor meer informatie, [SQL Database-resourcebeperkingen voor één en gepoolde databases](https://docs.microsoft.com/azure/sql-database/sql-database-resource-limits-logical-server).<br/><br/>De Resource-ID geeft aan dat de resource die de limiet is bereikt. Voor werkthreads, de Resource-ID = 1. Voor de Resource-ID-sessies = 2.<br/><br/>Zie voor meer informatie over deze fout en hoe u deze kunt oplossen:<br/>• [Azure SQL Database-resourcebeperkingen](sql-database-service-tiers-dtu.md). |
| 10929 |20 |Resource-ID: %d. De minimumgarantie voor %s is %d, maximumlimiet is %d, en het huidige gebruik voor de database is %d. De server is momenteel echter te druk bezet ter ondersteuning van aanvragen die groter zijn dan %d voor deze database. Zie voor meer informatie, [SQL Database-resourcebeperkingen voor één en gepoolde databases](https://docs.microsoft.com/azure/sql-database/sql-database-resource-limits-logical-server). Anders probeer het later opnieuw.<br/><br/>De Resource-ID geeft aan dat de resource die de limiet is bereikt. Voor werkthreads, de Resource-ID = 1. Voor de Resource-ID-sessies = 2.<br/><br/>Zie voor meer informatie over deze fout en hoe u deze kunt oplossen:<br/>• [Azure SQL Database-resourcebeperkingen](sql-database-service-tiers-dtu.md). |
| 40544 |20 |De database heeft het groottequotum bereikt. Partitioneer of verwijder gegevens, verwijder indexen of Raadpleeg de documentatie voor mogelijke oplossingen. |
| 40549 |16 |Sessie is beëindigd omdat er een langlopende transactie. Probeer uw transactie te verkorten. |
| 40550 |16 |De sessie is beëindigd omdat er te veel vergrendelingen heeft verkregen. Probeer het lezen of minder rijen in één transactie te wijzigen. |
| 40551 |16 |De sessie is beëindigd vanwege het overmatige `TEMPDB` gebruik. Probeer uw zoekopdracht om te beperken van het gebruik van de tijdelijke tabel schijfruimte te wijzigen.<br/><br/>Als u van tijdelijke objecten gebruikmaakt, zo schijfruimte besparen in de `TEMPDB` database door tijdelijke objecten neer te zetten nadat ze niet meer nodig door de sessie hebt zijn. |
| 40552 |16 |De sessie is beëindigd vanwege het gebruik van ruimte overmatige transactielogboeken. Probeer minder rijen in één transactie te wijzigen.<br/><br/>Als u bulksgewijs invoegen met behulp van de `bcp.exe` hulpprogramma of de `System.Data.SqlClient.SqlBulkCopy` klasse, probeert u het `-b batchsize` of `BatchSize` opties voor het beperken van het aantal rijen gekopieerd naar de server in elke transactie. Als u opnieuw van een index met opbouwen de `ALTER INDEX` -instructie, probeert u het `REBUILD WITH ONLINE = ON` optie. |
| 40553 |16 |De sessie is beëindigd vanwege overmatige geheugengebruik. Probeer de query voor het verwerken van minder rijen te wijzigen.<br/><br/>Het aantal te verminderen `ORDER BY` en `GROUP BY` bewerkingen in de Transact-SQL-code vermindert de geheugenvereisten van de query. |

## <a name="elastic-pool-errors"></a>Elastische pool-fouten
De volgende fouten zijn met betrekking tot het maken en gebruiken van elastische pools:

| Foutcode | Severity | Description | Corrigerende maatregelen |
|:--- |:--- |:--- |:--- |
| 1132 | 17 |De opslag van de elastische pool is bereikt. Het opslaggebruik voor de elastische groep kan niet groter zijn dan (%d) MB/s. Er wordt geprobeerd gegevens te schrijven naar een database wanneer de limiet voor opslag van de elastische pool is bereikt. |Houd rekening met het aantal dtu's van toenemende en/of toe te voegen opslag aan de elastische pool indien mogelijk om te verhogen van de limiet voor opslag verminderen de opslag die wordt gebruikt door afzonderlijke databases binnen de elastische pool of databases verwijderen uit de elastische groep. |
| 10929 | 16 |De minimumgarantie voor %s is %d, maximumlimiet is %d, en het huidige gebruik voor de database is %d. De server is momenteel echter te druk bezet ter ondersteuning van aanvragen die groter zijn dan %d voor deze database. Zie [SQL Database-resourcebeperkingen voor één en gepoolde databases](https://docs.microsoft.com/azure/sql-database/sql-database-resource-limits-logical-server) voor hulp. Anders probeer het later opnieuw. DTU / vCore-minimum per database. DTU / vCore maximum per database. Het totale aantal gelijktijdige werknemers (aanvragen) voor alle databases in de elastische groep probeert te overschrijden de limiet voor groep van toepassingen. |Vergroot het aantal dtu's of vCores van de elastische pool indien mogelijk om de limiet van de werknemer verhogen of databases uit de elastische pool te verwijderen. |
| 40844 | 16 |Database '%ls' op Server '%ls' is een '%ls' edition-database in een elastische pool en een relatie doorlopend kopiëren kan hebben.  |N/A |
| 40857 | 16 |Elastische pool niet vinden voor de server: '%ls', naam elastische groep: '%ls'. Opgegeven elastische pool bestaat niet in de opgegeven server. | Geef een geldige elastische pool-naam. |
| 40858 | 16 |Elastische pool '%ls' bestaat al in de server: '%ls'. Opgegeven elastische pool bestaat al in de opgegeven logische server. | Geef de naam van een nieuwe elastische pool. |
| 40859 | 16 |Elastische groep biedt geen ondersteuning voor servicelaag '%ls'. Laag van de opgegeven service wordt niet ondersteund voor het inrichten van de elastische pool. |Geef de juiste editie of laat de servicelaag veld leeg als u de standaard-servicelaag. |
| 40860 | 16 |Combinatie van elastische groep '%ls' en service doelstelling '%ls' is ongeldig. Elastische pool- en service-laag kan samen worden opgegeven als resourcetype is opgegeven als 'ElasticPool'. |Juiste combinatie van elastische pool en servicelaag opgeven. |
| 40861 | 16 |De database-editie is %. *ls kunnen niet anders dan de elastische groepservicelaag is ' %.* ls'. De database-editie is anders dan de elastische pool-servicelaag. |Geef een database-editie die verschilt van de elastische pool-servicelaag.  Houd er rekening mee dat de database-editie niet hoeft te worden opgegeven. |
| 40862 | 16 |De naam van de elastische groep moet worden opgegeven als de servicedoelstelling voor de elastische pool is opgegeven. Elastische pool servicedoelstelling vormt geen unieke identificatie voor een elastische pool. |Geef de naam van de elastische groep bij het gebruik van de servicedoelstelling voor de elastische pool. |
| 40864 | 16 |Het aantal dtu's voor de elastische groep moet ten minste (%d) dtu's voor servicelaag ' % * ls'. Er wordt geprobeerd om in te stellen van het aantal dtu's voor de elastische groep onder de ondergrens. |Probeer het opnieuw instellen van het aantal dtu's voor de elastische groep aan ten minste het minimum. |
| 40865 | 16 |Het aantal dtu's voor de elastische groep kunnen niet groter zijn dan (%d) dtu's voor servicelaag ' % * ls'. Er wordt geprobeerd om in te stellen van het aantal dtu's voor de elastische groep boven de maximale limiet. |Probeer het opnieuw instellen van het aantal dtu's voor de elastische pool op niet langer zijn dan de maximale limiet. |
| 40867 | 16 |Het aantal DTU maximum per database moet ten minste (%d) voor servicelaag ' % * ls'. Er wordt geprobeerd om in te stellen het maximale aantal DTU's per database onder de ondersteunde limiet. | Overweeg het gebruik van de elastische groepservicelaag die ondersteuning biedt voor de gewenste instelling. |
| 40868 | 16 |Het maximum per database DTU niet langer zijn dan (%d) voor servicelaag ' % * ls'. Er wordt geprobeerd om in te stellen het maximale aantal DTU's per database dan de ondersteunde limiet. | Overweeg het gebruik van de elastische groepservicelaag die ondersteuning biedt voor de gewenste instelling. |
| 40870 | 16 |Het minimale aantal DTU's per database kan niet groter zijn dan (%d) voor servicelaag ' % * ls'. Er wordt geprobeerd om in te stellen het minimale aantal DTU's per database dan de ondersteunde limiet. | Overweeg het gebruik van de elastische groepservicelaag die ondersteuning biedt voor de gewenste instelling. |
| 40873 | 16 |Het aantal databases (%d) en het minimale aantal DTU's per database (%d) kan niet groter zijn dan het aantal dtu's van de elastische groep (%d). Er wordt geprobeerd om op te geven van minimale aantal DTU's voor databases in de elastische pool die langer is dan het aantal dtu's van de elastische pool. | Vergroot het aantal dtu's van de elastische pool of verkleinen van het minimale aantal DTU's per database, of verminder het aantal databases in de elastische pool. |
| 40877 | 16 |Een elastische pool kan niet worden verwijderd, tenzij deze geen databases bevat. De elastische groep bevat een of meer databases en kan niet worden verwijderd. |Databases verwijderen uit de elastische groep om te kunnen verwijderen. |
| 40881 | 16 |De elastische groep ' % * ls' heeft de limiet voor het aantal database bereikt.  De limiet voor het aantal database voor de elastische groep kan niet groter zijn dan (%d) voor een elastische pool met (%d) dtu's. Er wordt geprobeerd om te maken of database toevoegen aan de elastische pool wanneer de limiet voor het aantal database van de elastische pool is bereikt. | Houd rekening met het aantal dtu's van de elastische pool indien mogelijk om te verhogen van de databaselimiet te verhogen of databases verwijderen uit de elastische groep. |
| 40889 | 16 |Het aantal dtu's of de opslaglimiet voor de elastische groep ' % * ls kunnen niet worden verkleind aangezien die onvoldoende opslagruimte heeft voor de databases niet wilt opgeven. Er wordt geprobeerd de opslaglimiet van de elastische pool onder het gebruik van de opslag verminderen. | Verklein het opslaggebruik van afzonderlijke databases in de elastische pool of databases uit de pool verwijderen om te reduceren de dtu's of de limiet voor opslag. |
| 40891 | 16 |Het minimale aantal DTU's per database (%d) kan niet groter zijn dan het maximale aantal DTU's per database (%d). Er wordt geprobeerd om in te stellen het minimale aantal DTU's per database hoger is dan het maximale aantal DTU's per database. |Zorg ervoor dat het minimale aantal DTU's per databases niet groter zijn dan het maximale aantal DTU's per database. |
| NOG TE BEPALEN | 16 |De maximale grootte voor een individuele database in een elastische pool niet langer zijn dan de maximale grootte worden toegestaan door ' % * ls' service tier elastische pool. De maximale grootte voor de database is groter dan de maximale grootte worden toegestaan door de servicelaag van de elastische pool. |De maximale grootte van de database binnen de grenzen van de maximale grootte worden toegestaan door de servicelaag van de elastische pool instellen. |

Verwante onderwerpen:

* [Een elastische pool (C#) maken](sql-database-elastic-pool-manage-csharp.md)
* [Een elastische pool beheren (C#)](sql-database-elastic-pool-manage-csharp.md)
* [Een elastische pool (PowerShell) maken](sql-database-elastic-pool-manage-powershell.md)
* [Controleren en beheren van een elastische pool (PowerShell)](sql-database-elastic-pool-manage-powershell.md)

## <a name="general-errors"></a>Algemene fouten
De volgende fouten vallen niet in de vorige categorieën.

| Foutcode | Severity | Description |
| ---:| ---:|:--- |
| 15006 |16 |(AdministratorLogin) is niet een geldige naam omdat deze ongeldige tekens bevat. |
| 18452 |14 |Aanmelding mislukt. De aanmelding is afkomstig uit een niet-vertrouwd domein en kan niet worden gebruikt met Windows authentication.%.&#x2a;ls (Windows-aanmeldingen worden niet ondersteund in deze versie van SQL Server.) |
| 18456 |14 |Aanmelden is mislukt voor gebruiker ' %. &#x2a;ls'.%. &#x2a;ls %. &#x2a;ls (de aanmelding is mislukt voor gebruiker ' %.&#x2a; ls'.) |
| 18470 |14 |Aanmelding mislukt voor gebruiker '%.&#x2a;ls'. Reden: Het account is disabled.%. &#x2a;ls |
| 40014 |16 |Meerdere databases kunnen niet worden gebruikt in dezelfde transactie. |
| 40054 |16 |Tabellen zonder geclusterde index worden niet ondersteund in deze versie van SQL Server. Maak een geclusterde index en probeer het opnieuw. |
| 40133 |15 |Deze bewerking wordt niet ondersteund in deze versie van SQL Server. |
| 40506 |16 |De opgegeven SID is ongeldig voor deze versie van SQL Server. |
| 40507 |16 |'%.&#x2a;ls' kunnen niet worden aangeroepen met parameters in deze versie van SQL Server. |
| 40508 |16 |De instructie USE wordt niet ondersteund voor schakelen tussen databases. Gebruik een nieuwe verbinding om te verbinden met een andere database. |
| 40510 |16 |De instructie '%.&#x2a;ls' wordt niet ondersteund in deze versie van SQL Server |
| 40511 |16 |De ingebouwde functie '%.&#x2a;ls' wordt niet ondersteund in deze versie van SQL Server. |
| 40512 |16 |Afgeschafte functie '%ls' wordt niet ondersteund in deze versie van SQL Server. |
| 40513 |16 |Server variabele '%.&#x2a;ls' wordt niet ondersteund in deze versie van SQL Server. |
| 40514 |16 |'%ls' wordt niet ondersteund in deze versie van SQL Server. |
| 40515 |16 |Verwijzing naar de database en/of de server-naam in '%.&#x2a;ls' wordt niet ondersteund in deze versie van SQL Server. |
| 40516 |16 |Globale, tijdelijke objecten worden niet ondersteund in deze versie van SQL Server. |
| 40517 |16 |Trefwoord-of instructieoptie '%.&#x2a;ls' wordt niet ondersteund in deze versie van SQL Server. |
| 40518 |16 |DBCC-opdracht '%.&#x2a;ls' wordt niet ondersteund in deze versie van SQL Server. |
| 40520 |16 |De beveiligbare klasse '% S_MSG' wordt niet ondersteund in deze versie van SQL Server. |
| 40521 |16 |De beveiligbare klasse '% S_MSG' wordt niet ondersteund in het serverbereik in deze versie van SQL Server. |
| 40522 |16 |Database-principal '%.&#x2a;ls' type wordt niet ondersteund in deze versie van SQL Server. |
| 40523 |16 |Maken van het impliciete gebruiker '%.&#x2a;ls' wordt niet ondersteund in deze versie van SQL Server. Maak de gebruiker expliciet voordat u deze gebruikt. |
| 40524 |16 |Gegevenstype '%.&#x2a;ls' wordt niet ondersteund in deze versie van SQL Server. |
| 40525 |16 |MET '%.ls' wordt niet ondersteund in deze versie van SQL Server. |
| 40526 |16 |'%.&#x2a;ls' rijensetprovider ls niet ondersteund in deze versie van SQL Server. |
| 40527 |16 |Gekoppelde servers worden niet ondersteund in deze versie van SQL Server. |
| 40528 |16 |Gebruikers kunnen niet worden toegewezen aan certificaten, asymmetrische sleutels of Windows-aanmeldingen in deze versie van SQL Server. |
| 40529 |16 |De ingebouwde functie '%.&#x2a;ls' in imitatie context niet wordt ondersteund in deze versie van SQL Server. |
| 40532 |11 |Server kan niet worden geopend "%.&#x2a;ls" aangevraagd door de aanmelding. De aanmelding is mislukt. |
| 40553 |16 |De sessie is beëindigd vanwege overmatige geheugengebruik. Probeer de query voor het verwerken van minder rijen te wijzigen.<br/><br/> Het aantal te verminderen `ORDER BY` en `GROUP BY` bewerkingen in de Transact-SQL-code reduceert de geheugenvereisten van de query. |
| 40604 |16 |Kan geen CREATE/ALTER DATABASE, omdat deze wordt het quotum van de server. |
| 40606 |16 |Bezig met koppelen van databases wordt niet ondersteund in deze versie van SQL Server. |
| 40607 |16 |Windows-aanmeldingen worden niet ondersteund in deze versie van SQL Server. |
| 40611 |16 |Servers kunnen maximaal 128 firewallregels zijn gedefinieerd hebben. |
| 40614 |16 |IP-adres van de firewallregel niet langer zijn dan het laatste IP-adres. |
| 40615 |16 |Server kan niet worden geopend '{0}' aangevraagd door de aanmelding. De client met IP-adres{1}' is niet toegestaan voor toegang tot de server.<br /><br />Voor toegang, gebruik van de SQL Database-Portal of voer sp\_ingesteld\_firewall\_-regel op de hoofddatabase om een firewallregel voor dit IP-adres of adresbereik te maken. Het duurt maximaal vijf minuten voor deze wijziging door te voeren. |
| 40617 |16 |Naam van de firewallregel die begint met (regelnaam) is te lang. Maximale lengte is 128. |
| 40618 |16 |Naam van de firewallregel mag niet leeg zijn. |
| 40620 |16 |De aanmelding is mislukt voor gebruiker "%.&#x2a;ls". Wijzigen van het wachtwoord is mislukt. Wachtwoord wijzigen tijdens het aanmelden wordt niet ondersteund in deze versie van SQL Server. |
| 40627 |20 |Bewerking op server '{0}'en de database'{1}' wordt uitgevoerd. Wacht een paar minuten voordat u doorgaat. |
| 40630 |16 |Validatie van het wachtwoord is mislukt. Het wachtwoord voldoet niet aan de beleidseisen omdat het te kort is. |
| 40631 |16 |Het wachtwoord die u hebt opgegeven is te lang. Het wachtwoord moet bestaan niet meer dan 128 tekens. |
| 40632 |16 |Validatie van het wachtwoord is mislukt. Het wachtwoord voldoet niet aan de beleidseisen omdat het niet complex genoeg is. |
| 40636 |16 |Een gereserveerde databasenaam niet gebruiken '%.&#x2a;ls' in deze bewerking. |
| 40638 |16 |Ongeldige abonnements-id (abonnement-id). Abonnement bestaat niet. |
| 40639 |16 |Aanvraag voldoet niet aan schema: (schema fout). |
| 40640 |20 |Er is een onverwachte uitzondering opgetreden in de server. |
| 40641 |16 |De opgegeven locatie is ongeldig. |
| 40642 |17 |De server is momenteel bezet. Probeer het later opnieuw. |
| 40643 |16 |De opgegeven x-ms-version-kopwaarde is ongeldig. |
| 40644 |14 |Kan geen toegang tot het opgegeven abonnement verlenen. |
| 40645 |16 |Servernaam (servernaam) mag niet leeg of null zijn. Het kan alleen bestaan uit kleine letters 'a'-'z', de cijfers 0-9 en het afbreekstreepje staat. Het koppelteken mag niet- of achteraan in de naam. |
| 40646 |16 |Abonnements-ID mag niet leeg zijn. |
| 40647 |16 |Abonnement (abonnement-id) heeft geen server (servernaam). |
| 40648 |17 |Te veel aanvragen zijn uitgevoerd. Probeer het later opnieuw. |
| 40649 |16 |Ongeldige content-type is opgegeven. Alleen application/xml wordt ondersteund. |
| 40650 |16 |Abonnement (abonnement-id) bestaat niet of is niet gereed voor de bewerking. |
| 40651 |16 |Kan geen server maken omdat het abonnement (abonnement-id) is uitgeschakeld. |
| 40652 |16 |Kan verplaatsen of -server maken. Abonnement (abonnement-id) overschrijdt de serverquota. |
| 40671 |17 |Communicatiefout tussen de gateway en de management-service. Probeer het later opnieuw. |
| 40852 |16 |Kan de database openen ' %. \*ls op server "%. \*ls' aangevraagd door de aanmelding. Toegang tot de database is alleen toegestaan met behulp van een beveiligde verbindingsreeks. Voor toegang tot deze database, wijzigt u uw verbindingsreeksen bevat veilig op de server FQDN - naam van server.database.windows .net moet worden gewijzigd in 'servernaam'.database. `secure`. windows.net. |
| 40914 | 16 | Server kan niet worden geopend '*[servernaam]* is aangevraagd door de aanmelding. Client is niet toegestaan voor toegang tot de server.<br /><br />U kunt toevoegen om op te lossen, een [regel voor virtuele netwerken](sql-database-vnet-service-endpoint-rule-overview.md). |
| 45168 |16 |De SQL Azure-systeem wordt belast, en een bovengrens op gelijktijdige DB CRUD-bewerkingen voor één server is geplaatst (bijvoorbeeld database maken). De server die is opgegeven in het foutbericht heeft het maximum aantal gelijktijdige verbindingen overschreden. Probeer het later opnieuw. |
| 45169 |16 |De azure SQL-systeem wordt belast en een bovengrens wordt geplaatst op het aantal gelijktijdige server CRUD-bewerkingen voor één abonnement (bijvoorbeeld server maken). Het abonnement dat is opgegeven in het foutbericht overschrijdt het maximum aantal gelijktijdige verbindingen en de aanvraag is geweigerd. Probeer het later opnieuw. |

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [Azure SQL Database-functies](sql-database-features.md).
* Meer informatie over [DTU gebaseerde aankoopmodel](sql-database-service-tiers-dtu.md).
* Meer informatie over [vCore gebaseerde aankoopmodel](sql-database-service-tiers-vcore.md).

