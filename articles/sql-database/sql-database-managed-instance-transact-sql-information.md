---
title: Verschillen tussen Azure SQL Database Managed Instance T-SQL | Microsoft Docs
description: In dit artikel worden besproken die de T-SQL-verschillen tussen een beheerd exemplaar in Azure SQL Database en SQL Server
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: sstein, carlrab, bonova
manager: craigg
ms.date: 03/13/2019
ms.custom: seoapril2019
ms.openlocfilehash: 2ca2e4e98f56f7df5e81217bcda00179f05ff69e
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67070359"
---
# <a name="azure-sql-database-managed-instance-t-sql-differences-from-sql-server"></a>Azure SQL Database Managed Instance T-SQL-verschillen van SQL Server

In dit artikel bevat een overzicht van en worden de verschillen in syntaxis en het gedrag tussen Azure SQL Database Managed Instance en on-premises SQL Server Database Engine uitgelegd. De volgende onderwerpen worden besproken: <a name="Differences"></a>

- [Beschikbaarheid](#availability) bevat de verschillen in [Always-On](#always-on-availability) en [back-ups](#backup).
- [Beveiliging](#security) bevat de verschillen in [controle](#auditing), [certificaten](#certificates), [referenties](#credential), [cryptografische providers](#cryptographic-providers), [aanmeldingen en gebruikers](#logins-and-users), en de [servicesleutel en servicehoofdsleutel](#service-key-and-service-master-key).
- [Configuratie](#configuration) bevat de verschillen in [extensie van de buffer](#buffer-pool-extension), [sortering](#collation), [compatibiliteitsniveaus](#compatibility-levels), [databasespiegeling ](#database-mirroring), [databaseopties](#database-options), [SQL Server Agent](#sql-server-agent), en [Tabelopties](#tables).
- [Functies](#functionalities) bevat [BULKSGEWIJS invoegen/OPENROWSET](#bulk-insert--openrowset), [CLR](#clr), [DBCC](#dbcc), [gedistribueerde transacties](#distributed-transactions), [uitgebreid gebeurtenissen](#extended-events), [externe bibliotheken](#external-libraries), [filestream en FileTable](#filestream-and-filetable), [semantische zoekopdrachten in volledige tekst](#full-text-semantic-search), [gekoppelde servers](#linked-servers), [PolyBase](#polybase), [replicatie](#replication), [herstellen](#restore-statement), [Service Broker](#service-broker), [opgeslagen procedures, functies en triggers](#stored-procedures-functions-and-triggers).
- [Omgevingsinstellingen](#Environment) , zoals configuraties van vnet's en subnet.
- [Functies waarvoor verschillend gedrag in beheerde instanties](#Changes).
- [Tijdelijke beperkingen en bekende problemen](#Issues).

De Implementatieoptie Managed Instance biedt extra compatibiliteit met on-premises SQL Server Database Engine. De meeste van de SQL Server database engine-functies worden ondersteund in een beheerd exemplaar.

![Migratie](./media/sql-database-managed-instance/migration.png)

## <a name="availability"></a>Beschikbaarheid

### <a name="always-on-availability"></a>Altijd ingeschakeld

[Hoge beschikbaarheid](sql-database-high-availability.md) is gratis ingebouwd in de Managed Instance en kan niet worden bepaald door gebruikers. De volgende instructies worden niet ondersteund:

- [EINDPUNT MAKEN... VOOR DATABASE_MIRRORING](https://docs.microsoft.com/sql/t-sql/statements/create-endpoint-transact-sql)
- [BESCHIKBAARHEIDSGROEP MAKEN](https://docs.microsoft.com/sql/t-sql/statements/create-availability-group-transact-sql)
- [ALTER AVAILABILITY GROUP](https://docs.microsoft.com/sql/t-sql/statements/alter-availability-group-transact-sql)
- [DROP AVAILABILITY GROUP](https://docs.microsoft.com/sql/t-sql/statements/drop-availability-group-transact-sql)
- De [SET HADR](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-set-hadr) -component van de [ALTER DATABASE](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql) instructie

### <a name="backup"></a>Backup

Beheerde exemplaren beschikken over automatische back-ups, zodat gebruikers van volledige database maken kunnen `COPY_ONLY` back-ups. Differentiële, het logboek en bestand momentopnameback-ups worden niet ondersteund.

- Met een beheerd exemplaar back-up van een exemplaar in de database alleen naar een Azure Blob storage-account:
  - Alleen `BACKUP TO URL` wordt ondersteund.
  - `FILE`, `TAPE`, en back-upapparaten worden niet ondersteund.
- De meeste van de algemene `WITH` opties worden ondersteund.
  - `COPY_ONLY` is verplicht.
  - `FILE_SNAPSHOT` wordt niet ondersteund.
  - Tapeopties: `REWIND`, `NOREWIND`, `UNLOAD`, en `NOUNLOAD` worden niet ondersteund.
  - Logboek-specifieke opties: `NORECOVERY`, `STANDBY`, en `NO_TRUNCATE` worden niet ondersteund.

Beperkingen: 

- Met een beheerd exemplaar, u kunt back-up een exemplaar in de database naar een back-up met maximaal 32 striping, wat voldoende is voor databases tot 4 TB als back-compressie wordt gebruikt.
- De maximale back-up Streepgrootte met behulp van de `BACKUP` opdracht in een beheerd exemplaar is 195 GB, waardoor de maximale blobgrootte is. Verhoog het aantal streepjes in de back-opdracht voor het verkleinen van de afzonderlijke stripe en binnen deze limiet blijven.

    > [!TIP]
    > Deze beperking omzeilen wanneer u back-up van een database van SQL Server in een on-premises-omgeving of in een virtuele machine, kunt u het volgende doen:
    >
    > - Reservekopie `DISK` in plaats van een back-up tot `URL`.
    > - De back-bestanden uploaden naar Blob-opslag.
    > - In het beheerde exemplaar herstellen.
    >
    > De `Restore` opdracht in een beheerd exemplaar biedt ondersteuning voor grotere blob-opslag in de back-upbestanden omdat een andere blob-type wordt gebruikt voor de opslag van de geüploade back-upbestanden.

Zie voor meer informatie over back-ups met behulp van T-SQL [back-up](https://docs.microsoft.com/sql/t-sql/statements/backup-transact-sql).

## <a name="security"></a>Beveiliging

### <a name="auditing"></a>Controleren

De belangrijkste verschillen tussen de controle van databases in Azure SQL Database en de databases in SQL Server zijn:

- Met de Managed Instance-Implementatieoptie in Azure SQL Database auditing werkt op het serverniveau van de. De `.xel` logbestanden worden opgeslagen in Azure Blob-opslag.
- Met de individuele databases en elastische pool implementatie-opties in Azure SQL Database auditing werkt op het databaseniveau van de.
- Controle werkt op het serverniveau van de in SQL Server on-premises of virtuele machines. Gebeurtenissen worden opgeslagen op het bestandssysteem of het Windows-gebeurtenislogboeken.
 
XEvent-controle in het beheerde exemplaar biedt ondersteuning voor Azure Blob storage-doelen. Bestands- en Windows Logboeken worden niet ondersteund.

De sleutel verschillen de `CREATE AUDIT` syntaxis voor het controleren van Azure Blob-opslag zijn:

- Een nieuwe syntaxis `TO URL` voorwaarde dat u kunt gebruiken om op te geven van de URL van de Azure Blob storage-container waarin de `.xel` bestanden worden geplaatst.
- De syntaxis van de `TO FILE` omdat een beheerd exemplaar heeft geen toegang Windows-bestandsshares tot wordt niet ondersteund.

Zie voor meer informatie: 

- [SERVERAUDIT MAKEN](https://docs.microsoft.com/sql/t-sql/statements/create-server-audit-transact-sql) 
- [ALTER SERVERAUDIT](https://docs.microsoft.com/sql/t-sql/statements/alter-server-audit-transact-sql)
- [Controle](https://docs.microsoft.com/sql/relational-databases/security/auditing/sql-server-audit-database-engine)

### <a name="certificates"></a>Certificaten

Een beheerd exemplaar geen toegang tot gedeelde bestanden en mappen, Windows, zodat de volgende beperkingen zijn van toepassing:

- De `CREATE FROM` / `BACKUP TO` bestand wordt niet ondersteund voor certificaten.
- De `CREATE` / `BACKUP` certificaat van `FILE` / `ASSEMBLY` wordt niet ondersteund. Bestanden met persoonlijke sleutel kunnen niet worden gebruikt. 

Zie [certificaat maken](https://docs.microsoft.com/sql/t-sql/statements/create-certificate-transact-sql) en [back-certificaat](https://docs.microsoft.com/sql/t-sql/statements/backup-certificate-transact-sql). 
 
**Tijdelijke oplossing**: Script voor het certificaat of de persoonlijke sleutel, als .sql-bestanden kunnen worden opgeslagen en maken op basis van het binaire bestand:

```sql
CREATE CERTIFICATE  
   FROM BINARY = asn_encoded_certificate
WITH PRIVATE KEY (<private_key_options>)
```

### <a name="credential"></a>Referentie

Alleen Azure Key Vault en `SHARED ACCESS SIGNATURE` identiteiten worden ondersteund. Windows-gebruikers worden niet ondersteund.

Zie [maken referentie](https://docs.microsoft.com/sql/t-sql/statements/create-credential-transact-sql) en [ALTER referentie](https://docs.microsoft.com/sql/t-sql/statements/alter-credential-transact-sql).

### <a name="cryptographic-providers"></a>Cryptografische providers

Een beheerd exemplaar heeft geen toegang tot bestanden, zodat de cryptografische providers kunnen niet worden gemaakt:

- `CREATE CRYPTOGRAPHIC PROVIDER` wordt niet ondersteund. Zie [maken CRYPTOGRAFISCHE PROVIDER](https://docs.microsoft.com/sql/t-sql/statements/create-cryptographic-provider-transact-sql).
- `ALTER CRYPTOGRAPHIC PROVIDER` wordt niet ondersteund. Zie [ALTER CRYPTOGRAFISCHE PROVIDER](https://docs.microsoft.com/sql/t-sql/statements/alter-cryptographic-provider-transact-sql).

### <a name="logins-and-users"></a>Aanmeldingen en gebruikers

- SQL-aanmeldingen die zijn gemaakt met behulp van `FROM CERTIFICATE`, `FROM ASYMMETRIC KEY`, en `FROM SID` worden ondersteund. Zie [maken aanmelding](https://docs.microsoft.com/sql/t-sql/statements/create-login-transact-sql).
- Azure Active Directory (Azure AD)-server-principals (aanmeldingen) die zijn gemaakt met de [CREATE LOGIN](https://docs.microsoft.com/sql/t-sql/statements/create-login-transact-sql?view=azuresqldb-mi-current) syntaxis of de [maken van GEBRUIKERSAANMELDING [Azure AD-aanmelding]](https://docs.microsoft.com/sql/t-sql/statements/create-user-transact-sql?view=azuresqldb-mi-current) syntaxis worden ondersteund (openbare preview). Deze aanmeldingen worden gemaakt op serverniveau.

    Beheerd exemplaar biedt ondersteuning voor Azure AD-database-principals met de syntaxis van de `CREATE USER [AADUser/AAD group] FROM EXTERNAL PROVIDER`. Deze functie wordt ook wel bekend als Azure AD ingesloten databasegebruikers.

- Windows-aanmeldingen die zijn gemaakt met de `CREATE LOGIN ... FROM WINDOWS` syntaxis worden niet ondersteund. Gebruik Azure Active Directory-aanmeldingen en gebruikers.
- De Azure AD-gebruiker die heeft gemaakt van het exemplaar heeft [onbeperkte beheerdersbevoegdheden](sql-database-manage-logins.md#unrestricted-administrative-accounts).
- Niet-Azure AD op databaseniveau beheerders kunnen worden gemaakt met behulp van de `CREATE USER ... FROM EXTERNAL PROVIDER` syntaxis. Zie [gebruiker maken... VAN DE EXTERNE PROVIDER](sql-database-manage-logins.md#non-administrator-users).
- Azure AD server-principals (aanmeldingen) ondersteuning voor SQL-functies binnen slechts één beheerd exemplaar. Functies waarvoor de interactie met de cross-exemplaar, ongeacht of ze nu binnen dezelfde Azure AD-tenant of verschillende tenants worden niet ondersteund voor Azure AD-gebruikers. Voorbeelden van dergelijke functies zijn:

  - Transactionele replicatie van SQL.
  - Link-server.

- Als u een Azure AD-aanmelding die is toegewezen aan een Azure AD-groep als eigenaar van de database wordt niet ondersteund.
- Imitatie van Azure AD-principals op serverniveau met behulp van andere Azure AD-principals wordt ondersteund, zoals de [EXECUTE AS](/sql/t-sql/statements/execute-as-transact-sql) component. EXECUTE AS beperkingen zijn:

  - EXECUTE AS USER wordt niet ondersteund voor Azure AD-gebruikers als de naam van de wijkt af van de aanmeldingsnaam. Een voorbeeld daarvan is wanneer de gebruiker is gemaakt via de syntaxis CREATE USER [myAadUser] van aanmelding [john@contoso.com] en imitatie wordt uitgevoerd via EXEC AS USER = _myAadUser_. Bij het maken van een **gebruiker** uit een Azure AD server-principal (aanmelden), geeft u de gebruikersnaam als het dezelfde login_name van **aanmelding**.
  - Alleen de SQL Server-level-principals (aanmeldingen) die deel van uitmaken de `sysadmin` rol van de volgende bewerkingen die zijn gericht op Azure AD-principals kunt uitvoeren:

    - EXECUTE AS USER
    - EXECUTE AS LOGIN

- Beperkingen voor openbare preview voor Azure AD server-principals (aanmeldingen):

  - Active Directory-beheerder beperkingen voor het beheerde exemplaar:

    - De Azure AD-beheerder gebruikt voor het instellen van het beheerde exemplaar kan niet worden gebruikt om te maken van een Azure AD-server principal (aanmelden) binnen het beheerde exemplaar. U moet de eerste Azure AD-server-principal (aanmelden) maken met behulp van een SQL Server-serviceaccount dat een `sysadmin` rol. Deze tijdelijke beperking worden verwijderd nadat Azure AD server-principals (aanmeldingen) algemeen beschikbaar. Als u een Azure AD-beheerdersaccount gebruiken om de aanmelding te maken probeert, ziet u de volgende fout: `Msg 15247, Level 16, State 1, Line 1 User does not have permission to perform this action.`
      - Op dit moment de eerste Azure AD-aanmelding gemaakt in de database master moet worden gemaakt met de standaard SQL Server-serviceaccount (niet-Azure AD) dat een `sysadmin` rol met behulp van [CREATE LOGIN](/sql/t-sql/statements/create-login-transact-sql?view=azuresqldb-mi-current) van externe PROVIDER. Na algemene beschikbaarheid, wordt deze beperking worden verwijderd. Vervolgens u een eerste maakt aanmelding met behulp van de Active Directory-beheerder voor beheerd exemplaar voor Azure AD.
    - DacFx (exporteren/importeren) gebruikt in combinatie met SQL Server Management Studio of SqlPackage wordt niet ondersteund voor Azure AD-aanmeldingen. Deze beperking wordt verwijderd nadat Azure AD server-principals (aanmeldingen) algemeen beschikbaar.
    - Met behulp van Azure AD server-principals (aanmeldingen) met SQL Server Management Studio:

      - Azure AD-aanmeldingen die gebruikmaken van elke geverifieerde aanmeldingsnaam Scripting wordt niet ondersteund.
      - IntelliSense niet wordt herkend door de aanmelding vanuit externe PROVIDER maken-instructie en ziet u een rood onderstreept.

- Alleen de principal-aanmelding op serverniveau, die is gemaakt met het beheerde exemplaar inrichtingsproces, leden van de server-functies, zoals `securityadmin` of `sysadmin`, of andere aanmeldingen met ALTER ANY LOGIN-machtigingen op serverniveau kunnen maken van Azure AD Server-principals (aanmeldingen) in de hoofddatabase voor Managed Instance.
- Als de aanmelding een SQL-principal is, alleen aanmeldingen die deel uitmaken van de `sysadmin` rol van de opdracht create aan aanmeldingen maken voor een Azure AD-account kunt gebruiken.
- De Azure AD-aanmelding moet een lid van een Azure AD in dezelfde map die wordt gebruikt voor Azure SQL Database Managed Instance.
- Azure AD server-principals (aanmeldingen) zijn zichtbaar in Object Explorer starten met SQL Server Management Studio 18.0 preview 5.
- Azure AD server-principals (aanmeldingen) met een Azure AD-beheerdersaccount overlappende is toegestaan. Azure AD server-principals (aanmeldingen) hebben voorrang op de Azure AD-beheerder bij het oplossen van de principal en machtigingen toepassen op het beheerde exemplaar.
- Tijdens de verificatie, wordt de volgende volgorde toegepast om op te lossen de verificatie-principal:

    1. Als de Azure AD-account als rechtstreeks bestaat toegewezen aan de Azure AD server-principal (aanmelden), die aanwezig is in sys.server_principals als het type 'E', toegang en machtigingen van de server-principal van Azure AD (aanmelden) toe te passen.
    2. Als de Azure AD-account is lid van een Azure AD-groep die gekoppeld aan de Azure AD server-principal (aanmelden), die aanwezig is in sys.server_principals als het type "X", toegang en machtigingen van de aanmelding van Azure AD-groep toe te passen.
    3. Als de Azure AD-account is een speciale portal geconfigureerd toepassen Azure AD-beheerder voor beheerd exemplaar, die niet in de Managed Instance systeemweergaven bestaat, speciale vaste machtigingen van de Azure AD-beheerder voor de Managed Instance (legacy-modus).
    4. Als de Azure AD-account bestaat als rechtstreeks toegewezen aan een Azure AD-gebruiker in een database, die aanwezig is in sys.database_principals als het type 'E', toegang en machtigingen van de gebruiker van Azure AD-database toe te passen.
    5. Als de Azure AD-account is lid van een Azure AD-groep die gekoppeld aan een Azure AD-gebruiker in een database, die aanwezig is in sys.database_principals als type "X", toegang en machtigingen van de aanmelding van Azure AD-groep toe te passen.
    6. Als er een Azure AD-aanmelding die is toegewezen aan een Azure AD-gebruikersaccount of een Azure AD-groep-account, die wordt omgezet aan de gebruiker die wordt geverifieerd, worden alle machtigingen van deze Azure AD-aanmelding toegepast.

### <a name="service-key-and-service-master-key"></a>De hoofdsleutel van sleutels en service-service

- [Back-up van de hoofdsleutel](https://docs.microsoft.com/sql/t-sql/statements/backup-master-key-transact-sql) (beheerd door SQL Database-service) wordt niet ondersteund.
- [Restore master key](https://docs.microsoft.com/sql/t-sql/statements/restore-master-key-transact-sql) (beheerd door SQL Database-service) wordt niet ondersteund.
- [Back-up van service master key](https://docs.microsoft.com/sql/t-sql/statements/backup-service-master-key-transact-sql) (beheerd door SQL Database-service) wordt niet ondersteund.
- [Service master key restore](https://docs.microsoft.com/sql/t-sql/statements/restore-service-master-key-transact-sql) (beheerd door SQL Database-service) wordt niet ondersteund.

## <a name="configuration"></a>Configuratie

### <a name="buffer-pool-extension"></a>De buffergroepuitbreiding

- [De extensie van de buffer](https://docs.microsoft.com/sql/database-engine/configure-windows/buffer-pool-extension) wordt niet ondersteund.
- `ALTER SERVER CONFIGURATION SET BUFFER POOL EXTENSION` wordt niet ondersteund. Zie [ALTER SERVER CONFIGURATION](https://docs.microsoft.com/sql/t-sql/statements/alter-server-configuration-transact-sql).

### <a name="collation"></a>Serverconfiguratie

De standaardsortering voor de instantie is `SQL_Latin1_General_CP1_CI_AS` en kunnen worden opgegeven als een parameter maken. Zie [sorteringen](https://docs.microsoft.com/sql/t-sql/statements/collations).

### <a name="compatibility-levels"></a>Compatibiliteitsniveaus

- Ondersteunde compatibiliteitsniveaus zijn 100, 110, 120, 130 en 140.
- Compatibiliteitsniveaus lager dan 100 worden niet ondersteund.
- Het standaardcompatibiliteitsniveau voor nieuwe databases is 140. Herstelde databases blijft het compatibiliteitsniveau ongewijzigd, als er 100 en hoger.

Zie [ALTER databasecompatibiliteitsniveau](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-compatibility-level).

### <a name="database-mirroring"></a>Databasespiegeling

Databasespiegeling wordt niet ondersteund.

- `ALTER DATABASE SET PARTNER` en `SET WITNESS` opties worden niet ondersteund.
- `CREATE ENDPOINT … FOR DATABASE_MIRRORING` wordt niet ondersteund.

Zie voor meer informatie, [ALTER DATABASE SET PARTNER en SET WITNESS](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-database-mirroring) en [EINDPUNT maken... VOOR DATABASE_MIRRORING](https://docs.microsoft.com/sql/t-sql/statements/create-endpoint-transact-sql).

### <a name="database-options"></a>Opties voor de database

- Meerdere logboekbestanden worden niet ondersteund.
- Objecten in het geheugen worden niet ondersteund in de categorie Algemeen gebruik-service. 
- Er is een limiet van 280 bestanden per exemplaar voor algemeen gebruik, wat betekent een maximum van 280 bestanden per database dat. Gegevens- en logboekbestanden bestanden in de categorie Algemeen gebruik worden geteld naar deze limiet. [De laag bedrijfskritiek 32.767 bestanden per database ondersteunt](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-resource-limits#service-tier-characteristics).
- De database mag geen bestandsgroepen met filestream-gegevens bevatten. Mislukt het herstellen van als .bak bevat `FILESTREAM` gegevens. 
- Elk bestand is in Azure Blob-opslag geplaatst. I/o- en doorvoer per bestand, is afhankelijk van de grootte van elk afzonderlijk bestand.

#### <a name="create-database-statement"></a>De instructie CREATE DATABASE

De volgende beperkingen gelden voor `CREATE DATABASE`:

- Bestanden en bestandsgroepen kan niet worden gedefinieerd. 
- De `CONTAINMENT` optie wordt niet ondersteund. 
- `WITH` opties worden niet ondersteund. 
   > [!TIP]
   > Als tijdelijke oplossing gebruiken `ALTER DATABASE` nadat `CREATE DATABASE` databaseopties bestanden toe te voegen of om in te stellen containment instellen. 

- De `FOR ATTACH` optie wordt niet ondersteund.
- De `AS SNAPSHOT OF` optie wordt niet ondersteund.

Zie voor meer informatie, [CREATE DATABASE](https://docs.microsoft.com/sql/t-sql/statements/create-database-sql-server-transact-sql).

#### <a name="alter-database-statement"></a>Instructie ALTER DATABASE

Sommige bestandseigenschappen kunnen niet worden ingesteld of gewijzigd:

- Pad naar een bestand kan niet worden opgegeven de `ALTER DATABASE ADD FILE (FILENAME='path')` T-SQL-instructie. Verwijder `FILENAME` van het script omdat de bestanden in een beheerd exemplaar automatisch wordt geplaatst. 
- Een bestandsnaam kan niet worden gewijzigd met behulp van de `ALTER DATABASE` instructie.

De volgende opties zijn standaard ingesteld en kunnen niet worden gewijzigd:

- `MULTI_USER`
- `ENABLE_BROKER ON`
- `AUTO_CLOSE OFF`

De volgende opties kunnen niet worden gewijzigd:

- `AUTO_CLOSE`
- `AUTOMATIC_TUNING(CREATE_INDEX=ON|OFF)`
- `AUTOMATIC_TUNING(DROP_INDEX=ON|OFF)`
- `DISABLE_BROKER`
- `EMERGENCY`
- `ENABLE_BROKER`
- `FILESTREAM`
- `HADR`
- `NEW_BROKER`
- `OFFLINE`
- `PAGE_VERIFY`
- `PARTNER`
- `READ_ONLY`
- `RECOVERY BULK_LOGGED`
- `RECOVERY_SIMPLE`
- `REMOTE_DATA_ARCHIVE` 
- `RESTRICTED_USER`
- `SINGLE_USER`
- `WITNESS`

Zie voor meer informatie, [ALTER DATABASE](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-file-and-filegroup-options).

### <a name="sql-server-agent"></a>SQL Server Agent

- Inschakelen en uitschakelen van SQL Server Agent wordt momenteel niet ondersteund in het beheerde exemplaar. SQL Agent wordt altijd uitgevoerd.
- SQL Server Agent-instellingen zijn alleen-lezen. De procedure `sp_set_agent_properties` wordt niet ondersteund in het beheerde exemplaar. 
- Taken
  - T-SQL-taakstappen worden ondersteund.
  - De volgende replicatietaken worden ondersteund:
    - Logboek voor databasetransacties lezer
    - Momentopname
    - Distributor
  - SSIS-taakstappen worden ondersteund.
  - Andere typen taakstappen worden momenteel niet ondersteund:
    - De taakstap voor samenvoegen replicatie wordt niet ondersteund. 
    - Lezer van de wachtrij wordt niet ondersteund. 
    - Opdrachtshell wordt nog niet ondersteund.
  - Beheerde exemplaren heeft geen toegang tot externe bronnen, bijvoorbeeld netwerkshares via robocopy. 
  - SQL Server Analysis Services worden niet ondersteund.
- Meldingen worden gedeeltelijk ondersteund.
- E-mailmelding wordt ondersteund, maar deze is vereist dat u een Database-e-mailprofiel configureert. SQL Server Agent kan slechts één Database-e-mailprofiel gebruiken, en moet worden aangeroepen `AzureManagedInstance_dbmail_profile`. 
  - Pager wordt niet ondersteund. 
  - Net Send wordt niet ondersteund.
  - Waarschuwingen worden nog niet ondersteund.
  - Proxy's worden niet ondersteund. 
- Gebeurtenislogboek wordt niet ondersteund.

De volgende functies worden niet ondersteund op dit moment maar in de toekomst worden ingeschakeld:

- Proxies
- Plannen van taken op een niet-actieve CPU
- In- of uitschakelen van een Agent
- Waarschuwingen

Zie [SQL Server Agent](https://docs.microsoft.com/sql/ssms/agent/sql-server-agent) voor meer informatie over SQL Server Agent.

### <a name="tables"></a>Tabellen

De volgende tabellen worden niet ondersteund:

- `FILESTREAM`
- `FILETABLE`
- `EXTERNAL TABLE`
- `MEMORY_OPTIMIZED` 

Zie voor meer informatie over het maken en wijzigen van tabellen [CREATE TABLE](https://docs.microsoft.com/sql/t-sql/statements/create-table-transact-sql) en [ALTER TABLE](https://docs.microsoft.com/sql/t-sql/statements/alter-table-transact-sql).

## <a name="functionalities"></a>Functies

### <a name="bulk-insert--openrowset"></a>Bulksgewijs invoegen / openrowset

Een beheerd exemplaar geen toegang tot gedeelde bestanden en mappen, Windows, zodat de bestanden moeten worden geïmporteerd uit Azure Blob-opslag:

- `DATASOURCE` is vereist in de `BULK INSERT` opdracht tijdens het importeren van bestanden vanuit Azure Blob storage. Zie [BULKSGEWIJS invoegen](https://docs.microsoft.com/sql/t-sql/statements/bulk-insert-transact-sql).
- `DATASOURCE` is vereist in de `OPENROWSET` wanneer u de inhoud van een bestand te lezen uit Azure Blob storage. Zie [OPENROWSET](https://docs.microsoft.com/sql/t-sql/functions/openrowset-transact-sql).

### <a name="clr"></a>CLR

Een beheerd exemplaar geen toegang tot gedeelde bestanden en mappen, Windows, zodat de volgende beperkingen zijn van toepassing:

- Alleen `CREATE ASSEMBLY FROM BINARY` wordt ondersteund. Zie [maken van de ASSEMBLY van binaire](https://docs.microsoft.com/sql/t-sql/statements/create-assembly-transact-sql). 
- `CREATE ASSEMBLY FROM FILE` wordt niet ondersteund. Zie [ASSEMBLY maken vanuit bestand](https://docs.microsoft.com/sql/t-sql/statements/create-assembly-transact-sql).
- `ALTER ASSEMBLY` kan niet verwijzen naar bestanden. Zie [ALTER ASSEMBLY](https://docs.microsoft.com/sql/t-sql/statements/alter-assembly-transact-sql).

### <a name="dbcc"></a>DBCC

Niet-gedocumenteerde DBCC-instructies die zijn ingeschakeld in SQL Server worden niet ondersteund in beheerde exemplaren.

- `Trace flags` worden niet ondersteund. Zie [traceermarkeringen](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql).
- `DBCC TRACEOFF` wordt niet ondersteund. Zie [DBCC TRACEOFF](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-traceoff-transact-sql).
- `DBCC TRACEON` wordt niet ondersteund. Zie [DBCC TRACEON](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-traceon-transact-sql).

### <a name="distributed-transactions"></a>Gedistribueerde transacties

MSDTC en [elastische transacties](sql-database-elastic-transactions-overview.md) op dit moment worden niet ondersteund in beheerde exemplaren.

### <a name="extended-events"></a>Uitgebreide gebeurtenissen

Sommige Windows-specifieke doelen voor Extended Events (XEvents) worden niet ondersteund:

- De `etw_classic_sync` doel wordt niet ondersteund. Store `.xel` bestanden in Azure Blob-opslag. Zie [etw_classic_sync doel](https://docs.microsoft.com/sql/relational-databases/extended-events/targets-for-extended-events-in-sql-server#etw_classic_sync_target-target).
- De `event_file` doel wordt niet ondersteund. Store `.xel` bestanden in Azure Blob-opslag. Zie [event_file doel](https://docs.microsoft.com/sql/relational-databases/extended-events/targets-for-extended-events-in-sql-server#event_file-target).

### <a name="external-libraries"></a>Externe bibliotheken

In de database R en Python, externe bibliotheken niet, maar wel ondersteund. Zie [SQL servermachine Learning-Services](https://docs.microsoft.com/sql/advanced-analytics/r/sql-server-r-services).

### <a name="filestream-and-filetable"></a>FILESTREAM en bestandstabel

- FILESTREAM-gegevens wordt niet ondersteund.
- De database geen bestandsgroepen met `FILESTREAM` gegevens.
- `FILETABLE` wordt niet ondersteund.
- Tabellen geen `FILESTREAM` typen.
- De volgende functies worden niet ondersteund:
  - `GetPathLocator()`
  - `GET_FILESTREAM_TRANSACTION_CONTEXT()`
  - `PathName()`
  - `GetFileNamespacePat)`
  - `FileTableRootPath()`

Zie voor meer informatie, [FILESTREAM](https://docs.microsoft.com/sql/relational-databases/blob/filestream-sql-server) en [Bestandstabellen](https://docs.microsoft.com/sql/relational-databases/blob/filetables-sql-server).

### <a name="full-text-semantic-search"></a>Semantische zoekopdrachten in volledige tekst

[Semantisch zoeken](https://docs.microsoft.com/sql/relational-databases/search/semantic-search-sql-server) wordt niet ondersteund.

### <a name="linked-servers"></a>Gekoppelde servers

Gekoppelde servers in beheerde instanties ondersteuning voor een beperkt aantal doelen:

- Ondersteunde doelen zijn SQL Server en SQL-Database.
- Doelen die niet worden ondersteund, worden bestanden, Analysis Services en andere RDBMS.

Bewerkingen

- Cross-instance schrijftransacties worden niet ondersteund.
- `sp_dropserver` wordt ondersteund voor het verwijderen van een gekoppelde server. Zie [sp_dropserver](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-dropserver-transact-sql).
- De `OPENROWSET` functie kan worden gebruikt voor het uitvoeren van query's alleen op SQL Server-exemplaren. Ze kunnen worden beheerd, on-premises, of in virtuele machines. Zie [OPENROWSET](https://docs.microsoft.com/sql/t-sql/functions/openrowset-transact-sql).
- De `OPENDATASOURCE` functie kan worden gebruikt voor het uitvoeren van query's alleen op SQL Server-exemplaren. Ze kunnen worden beheerd, on-premises, of in virtuele machines. Alleen de `SQLNCLI`, `SQLNCLI11`, en `SQLOLEDB` waarden worden ondersteund als een provider. Een voorbeeld is `SELECT * FROM OPENDATASOURCE('SQLNCLI', '...').AdventureWorks2012.HumanResources.Employee`. Zie [OPENDATASOURCE](https://docs.microsoft.com/sql/t-sql/functions/opendatasource-transact-sql).

### <a name="polybase"></a>PolyBase

Externe tabellen die verwijzen naar die de bestanden in HDFS of Azure Blob storage worden niet ondersteund. Zie voor meer informatie over PolyBase [PolyBase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide).

### <a name="replication"></a>Replicatie

Replicatie is beschikbaar voor openbare preview voor beheerd exemplaar. Zie voor meer informatie over replicatie [SQL Server-replicatie](https://docs.microsoft.com/sql/relational-databases/replication/replication-with-sql-database-managed-instance).

### <a name="restore-statement"></a>Instructie herstellen 

- Ondersteunde syntaxis:
  - `RESTORE DATABASE`
  - `RESTORE FILELISTONLY ONLY`
  - `RESTORE HEADER ONLY`
  - `RESTORE LABELONLY ONLY`
  - `RESTORE VERIFYONLY ONLY`
- Niet-ondersteunde syntaxis:
  - `RESTORE LOG ONLY`
  - `RESTORE REWINDONLY ONLY`
- Bron: 
  - `FROM URL` (Azure Blob storage) is de enige ondersteunde optie.
  - `FROM DISK`/`TAPE`/ back-upapparaat wordt niet ondersteund.
  - Back-upsets worden niet ondersteund.
- `WITH` opties worden niet ondersteund, zoals geen `DIFFERENTIAL` of `STATS`.
- `ASYNC RESTORE`: Herstel wordt voortgezet zelfs als de clientverbinding wordt verbroken. Als de verbinding wordt verbroken, kunt u controleren de `sys.dm_operation_status` weergeven voor de status van een herstelbewerking uit, en voor een database maken en -neerzetten. Zie [sys.dm_operation_status](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database). 

De volgende databaseopties zijn ingesteld of genegeerd en kan later worden gewijzigd: 

- `NEW_BROKER` Als de broker is niet ingeschakeld in het bestand met .bak. 
- `ENABLE_BROKER` Als de broker is niet ingeschakeld in het bestand met .bak. 
- `AUTO_CLOSE=OFF` Als een database in het bestand met .bak `AUTO_CLOSE=ON`. 
- `RECOVERY FULL` Als een database in het bestand met .bak `SIMPLE` of `BULK_LOGGED` herstelmodus.
- Een bestandsgroep geoptimaliseerd voor geheugen is toegevoegd en XTP aangeroepen als dit niet in het bronbestand .bak is. 
- Eventuele bestaande geoptimaliseerd voor geheugen bestandsgroep is gewijzigd in XTP. 
- `SINGLE_USER` en `RESTRICTED_USER` opties worden geconverteerd naar `MULTI_USER`.

Beperkingen: 

- `.BAK` bestanden met meerdere back-upsets kunnen niet worden hersteld. 
- `.BAK` bestanden met meerdere logboekbestanden kunnen niet worden hersteld.
- Mislukt het herstellen van als .bak bevat `FILESTREAM` gegevens.
- Back-ups die databases waarvoor actieve in-memory-objecten bevatten, kunnen niet worden hersteld op een algemeen gebruik-instantie. Zie voor meer informatie over de restore-instructies [RESTORE-instructies](https://docs.microsoft.com/sql/t-sql/statements/restore-statements-transact-sql).

### <a name="service-broker"></a>Service broker

Cross-exemplaar van service broker wordt niet ondersteund:

- `sys.routes`: U moet het adres van sys.routes selecteren als een vereiste. Het adres moet lokaal op elke route. Zie [sys.routes](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-routes-transact-sql).
- `CREATE ROUTE`: U kunt geen gebruiken `CREATE ROUTE` met `ADDRESS` dan `LOCAL`. Zie [maken ROUTE](https://docs.microsoft.com/sql/t-sql/statements/create-route-transact-sql).
- `ALTER ROUTE`: U kunt geen gebruiken `ALTER ROUTE` met `ADDRESS` dan `LOCAL`. Zie [ALTER ROUTE](https://docs.microsoft.com/sql/t-sql/statements/alter-route-transact-sql). 

### <a name="stored-procedures-functions-and-triggers"></a>Opgeslagen procedures, functies en triggers

- `NATIVE_COMPILATION` wordt niet ondersteund in de categorie Algemeen gebruik.
- De volgende [sp_configure](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-configure-transact-sql) opties worden niet ondersteund: 
  - `allow polybase export`
  - `allow updates`
  - `filestream_access_level`
  - `remote data archive`
  - `remote proc trans`
- `sp_execute_external_scripts` wordt niet ondersteund. See [sp_execute_external_scripts](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql#examples).
- `xp_cmdshell` wordt niet ondersteund. See [xp_cmdshell](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/xp-cmdshell-transact-sql).
- `Extended stored procedures` worden niet ondersteund, waaronder `sp_addextendedproc`  en `sp_dropextendedproc`. Zie [uitgebreide opgeslagen procedures](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/general-extended-stored-procedures-transact-sql).
- `sp_attach_db`, `sp_attach_single_file_db`, en `sp_detach_db` worden niet ondersteund. Zie [sp_attach_db](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-attach-db-transact-sql), [sp_attach_single_file_db](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-attach-single-file-db-transact-sql), en [sp_detach_db](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-detach-db-transact-sql).

## <a name="Environment"></a>Beperkingen van de omgeving

### <a name="subnet"></a>Subnet
- U plaatsen geen andere bronnen (bijvoorbeeld virtuele machines) in het subnet is gereserveerd voor uw beheerde exemplaar. Het plaatsen van deze resources in andere subnetten.
- Subnet moet voldoende beschikbare [IP-adressen](sql-database-managed-instance-connectivity-architecture.md#network-requirements). Minimaal is 16, terwijl het wordt aangeraden ten minste 32 IP-adressen in het subnet.
- [Service-eindpunten kunnen niet worden gekoppeld aan het beheerde exemplaar subnet](sql-database-managed-instance-connectivity-architecture.md#network-requirements). Zorg ervoor dat de service-eindpunten-optie is uitgeschakeld bij het maken van het virtuele netwerk.
- Het aantal vCores en typen van exemplaren die u in een regio implementeren kunt zijn sommige [beperkingen en limieten](sql-database-managed-instance-resource-limits.md#regional-resource-limitations).
- Er zijn een aantal [beveiligingsregels die moeten worden toegepast op het subnet](sql-database-managed-instance-connectivity-architecture.md#network-requirements).

### <a name="vnet"></a>VNET
- VNet kan worden geïmplementeerd met behulp van Resource-Model - klassieke Model voor het VNet wordt niet ondersteund.
- Sommige services zoals App Service-omgevingen, Logic apps en beheerde instanties (gebruikt voor Geo-replicatie, transactionele replicatie of via gekoppelde servers) geen toegang tot beheerde instanties in verschillende regio's als de vnet's zijn verbonden met behulp van [wereldwijde peering](../virtual-network/virtual-networks-faq.md#what-are-the-constraints-related-to-global-vnet-peering-and-load-balancers). U kunt verbinding maken met deze resource via ExpressRoute of VNet-naar-VNet via VNet-Gateways.

## <a name="Changes"></a> Gedragswijzigingen

De volgende variabelen, taken en weergaven kunt u verschillende resultaten retourneren:

- `SERVERPROPERTY('EngineEdition')` retourneert de waarde van 8. Deze eigenschap identificatie unieke van een beheerd exemplaar. Zie [SERVERPROPERTY](https://docs.microsoft.com/sql/t-sql/functions/serverproperty-transact-sql).
- `SERVERPROPERTY('InstanceName')` retourneert NULL omdat het concept van instantie als deze bestaat voor SQL Server niet van toepassing op een beheerd exemplaar. Zie [SERVERPROPERTY('InstanceName')](https://docs.microsoft.com/sql/t-sql/functions/serverproperty-transact-sql).
- `@@SERVERNAME` retourneert een volledige DNS 'koppelbaar'-naam, bijvoorbeeld Mijn-managed-instance.wcus17662feb9ce98.database.windows.net. Zie [@@SERVERNAME](https://docs.microsoft.com/sql/t-sql/functions/servername-transact-sql). 
- `SYS.SERVERS` retourneert een volledige DNS-"verbindingen" naam, zoals `myinstance.domain.database.windows.net` voor de eigenschappen 'name' en "data_source." Zie [SYS. SERVERS](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-servers-transact-sql).
- `@@SERVICENAME` retourneert NULL omdat het concept van de service die voor SQL Server niet van toepassing op een beheerd exemplaar bestaat. Zie [@@SERVICENAME](https://docs.microsoft.com/sql/t-sql/functions/servicename-transact-sql).
- `SUSER_ID` wordt ondersteund. Het resultaat NULL als de Azure AD-aanmelding niet in sys.syslogins. Zie [SUSER_ID](https://docs.microsoft.com/sql/t-sql/functions/suser-id-transact-sql). 
- `SUSER_SID` wordt niet ondersteund. De onjuiste gegevens wordt geretourneerd, die een tijdelijk probleem bekend is. Zie [SUSER_SID](https://docs.microsoft.com/sql/t-sql/functions/suser-sid-transact-sql). 

## <a name="Issues"></a> Bekende problemen en beperkingen

### <a name="tempdb-size"></a>Grootte van TEMPDB

De maximale bestandsgrootte van `tempdb` mag niet groter zijn dan 24 GB per kern op een categorie Algemeen gebruik. De maximale `tempdb` grootte op een laag bedrijfskritiek is beperkt met de grootte van de instantie. De `tempdb` database altijd is opgesplitst in 12 gegevensbestanden. Deze maximale grootte per bestand kan niet worden gewijzigd en nieuwe bestanden kunnen niet worden toegevoegd aan `tempdb`. Aantal query's mogelijk een fout geretourneerd als ze nodig hebben meer dan 24 GB per kern in `tempdb`. `tempdb` wordt altijd opnieuw als een lege database gemaakt wanneer de exemplaar-starten of failover- en een in-en-klare wijzigen `tempdb` blijft niet behouden. 

### <a name="cant-restore-contained-database"></a>Kan de ingesloten database niet herstellen.

Beheerd exemplaar kan niet worden hersteld [ingesloten databases](https://docs.microsoft.com/sql/relational-databases/databases/contained-databases). Point-in-time-terugzetten van de bestaande ingesloten databases werkt niet in het beheerde exemplaar. Dit probleem zal binnenkort worden omgezet. In de tussentijd is het raadzaam dat u de containment-optie verwijderen uit uw databases die worden geplaatst in het beheerde exemplaar. Gebruik niet de containment-optie voor de productiedatabases. 

### <a name="exceeding-storage-space-with-small-database-files"></a>Meer dan opslagruimte met kleine databasebestanden

`CREATE DATABASE`, `ALTER DATABASE ADD FILE`, en `RESTORE DATABASE` instructies mislukken omdat de instantie kan de Azure Storage-limiet is bereikt.

Elk beheerd exemplaar voor algemeen gebruik heeft tot wel 35 TB gereserveerd voor de schijfruimte van Azure Premium storage. Elk databasebestand is op een afzonderlijke fysieke schijf geplaatst. Schijfgrootten is 128 GB, 256 GB, 512 GB, 1 TB of 4 TB. Niet-gebruikte ruimte op de schijf is niet in rekening gebracht, maar de totale som van Azure Premium Disk-groottes mag niet meer dan 35 TB. In sommige gevallen kan een beheerd exemplaar van die 8 TB in totaal niet nodig de 35 TB Azure de limiet voor opslaggrootte vanwege een interne fragmentatie overschrijden.

Een beheerd exemplaar voor algemeen gebruik mogelijk bijvoorbeeld een bestand dat 1,2 TB in grootte geplaatst op een schijf met 4 TB. Dat er ook 248 bestanden die elke 1 GB groot zijn en die op afzonderlijke 128 GB schijven worden geplaatst. In dit voorbeeld:

- De grootte van de totale toegewezen schijfruimte is 1 x 4 TB + 248 x 128 GB = 35 TB.
- de totale gereserveerde ruimte voor databases op het exemplaar is 1 x-1,2 TB + 248 x 1 GB = 1,4 TB.

In dit voorbeeld ziet u dat onder bepaalde omstandigheden, vanwege een specifieke distributie van bestanden, een beheerd exemplaar van de 35 TB limiet die gereserveerd voor een gekoppelde Azure Premium-schijf wanneer u niet verwacht het dat mogelijk mogelijk is bereikt.

In dit voorbeeld bestaande databases blijven werken en zonder problemen kan groeien zolang er nieuwe bestanden worden niet toegevoegd. Nieuwe databases kunnen niet worden gemaakt of hersteld. Er is onvoldoende ruimte voor nieuwe schijfstations, zelfs als de totale grootte van alle databases de limiet van het exemplaar niet bereiken. De fout die wordt geretourneerd in dat geval niet wissen.

U kunt [identificeren van het aantal resterende bestanden](https://medium.com/azure-sqldb-managed-instance/how-many-files-you-can-create-in-general-purpose-azure-sql-managed-instance-e1c7c32886c1) via systeemweergaven. Als u deze limiet is bereikt, probeert te [leeg en verwijder enkele van de kleinere bestanden met behulp van de instructie DBCC SHRINKFILE](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-shrinkfile-transact-sql#d-emptying-a-file) of schakel over naar de [laag bedrijfskritiek, dat niet beschikt over deze limiet](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-resource-limits#service-tier-characteristics).

### <a name="incorrect-configuration-of-the-sas-key-during-database-restore"></a>Onjuiste configuratie van de SAS-sleutel tijdens de database herstellen

`RESTORE DATABASE` dat leesbewerkingen het bestand met .bak mogelijk worden steeds opnieuw wordt geprobeerd om te lezen van de .bak-bestand en een fout geretourneerd na een lange periode als de shared access signature in `CREDENTIAL` is onjuist. UPSETS uitvoeren voordat u een database om er zeker van te zijn dat de SAS-sleutel juist is herstelt.
Zorg ervoor dat u de voorloopspaties verwijderen `?` van de SAS-sleutel die wordt gegenereerd met behulp van de Azure-portal.

### <a name="tooling"></a>Hulpprogramma 's

SQL Server Management Studio en SQL Server Data Tools mogelijk enkele problemen hebben terwijl ze toegang krijgen een beheerd exemplaar tot.

- Met behulp van Azure AD server-principals (aanmeldingen) en gebruikers (openbare preview) met SQL Server Data Tools op dit moment wordt niet ondersteund.
- Scripts uitvoeren voor Azure AD server-principals (aanmeldingen) en gebruikers (openbare preview) wordt niet ondersteund in SQL Server Management Studio.

### <a name="incorrect-database-names-in-some-views-logs-and-messages"></a>Onjuiste databasenamen in bepaalde weergaven, logboeken en berichten

Verschillende systeemweergaven, prestatiemeteritems, foutberichten, XEvents en fouten in logboekvermeldingen weergegeven GUID database-id's in plaats van de werkelijke databasenamen. Vertrouw niet op deze GUID-id's omdat ze in de toekomst worden vervangen door de werkelijke databasenamen.

### <a name="database-mail"></a>Database-e-mail

De `@query` parameter in de [sp_send_db_mail](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-send-dbmail-transact-sql) procedure werkt niet.

### <a name="database-mail-profile"></a>Database-e-mailprofiel

Het Database-e-mailprofiel dat is gebruikt door de SQL Server Agent moet worden aangeroepen `AzureManagedInstance_dbmail_profile`. Er zijn geen beperkingen voor namen van andere Database Mail-profiel.

### <a name="error-logs-arent-persisted"></a>Foutenlogboeken worden niet opgeslagen

Foutenlogboeken die beschikbaar zijn in het beheerde exemplaar niet worden opgeslagen en de grootte niet is opgenomen in de limiet voor maximale opslag. Foutenlogboeken mogelijk automatisch worden gewist als failover is uitgevoerd.

### <a name="error-logs-are-verbose"></a>Foutenlogboeken zijn uitgebreide

Een beheerd exemplaar van uitgebreide informatie in de foutenlogboeken geplaatst en veel van deze is niet relevant. De hoeveelheid gegevens in foutenlogboeken verlaagd in de toekomst.

**Tijdelijke oplossing:** Een aangepaste procedure gebruiken om te lezen foutenlogboeken die sommige irrelevante gegevens filtert. Zie voor meer informatie, [Managed Instance-sp_readmierrorlog](https://blogs.msdn.microsoft.com/sqlcat/2018/05/04/azure-sql-db-managed-instance-sp_readmierrorlog/).

### <a name="transaction-scope-on-two-databases-within-the-same-instance-isnt-supported"></a>Transactie-scope op twee databases binnen dezelfde instantie wordt niet ondersteund

De `TransactionScope` klasse in .NET werkt niet als twee query naar de twee databases binnen hetzelfde exemplaar onder de dezelfde transactie verzonden's:

```C#
using (var scope = new TransactionScope())
{
    using (var conn1 = new SqlConnection("Server=quickstartbmi.neu15011648751ff.database.windows.net;Database=b;User ID=myuser;Password=mypassword;Encrypt=true"))
    {
        conn1.Open();
        SqlCommand cmd1 = conn1.CreateCommand();
        cmd1.CommandText = string.Format("insert into T1 values(1)");
        cmd1.ExecuteNonQuery();
    }

    using (var conn2 = new SqlConnection("Server=quickstartbmi.neu15011648751ff.database.windows.net;Database=b;User ID=myuser;Password=mypassword;Encrypt=true"))
    {
        conn2.Open();
        var cmd2 = conn2.CreateCommand();
        cmd2.CommandText = string.Format("insert into b.dbo.T2 values(2)");        cmd2.ExecuteNonQuery();
    }

    scope.Complete();
}

```

Maar deze code met gegevens in dezelfde instantie werkt, vereist het MSDTC.

**Tijdelijke oplossing:** Gebruik [SqlConnection.ChangeDatabase(String)](https://docs.microsoft.com/dotnet/api/system.data.sqlclient.sqlconnection.changedatabase) een andere database gebruiken in de verbindingscontext van een in plaats van twee verbindingen.

### <a name="clr-modules-and-linked-servers-sometimes-cant-reference-a-local-ip-address"></a>CLR-modules en soms gekoppelde servers kunnen niet verwijzen naar een lokaal IP-adres

CLR-modules geplaatst in een beheerd exemplaar en de gekoppelde servers of de gedistribueerde query's die een exemplaar soms verwijzen naar de IP-adres van een lokaal exemplaar kunnen niet worden omgezet. Deze fout is een tijdelijk probleem.

**Tijdelijke oplossing:** Context-verbindingen indien mogelijk in een CLR-module gebruiken.

### <a name="tde-encrypted-databases-with-a-service-managed-key-dont-support-user-initiated-backups"></a>Databases TDE is versleuteld met een door service beheerde sleutel bieden geen ondersteuning voor back-ups van de gebruiker geïnitieerde

Kan niet worden uitgevoerd `BACKUP DATABASE ... WITH COPY_ONLY` voor een database die versleuteld met service beheerde transparante gegevensversleuteling (TDE). Beheerde service TDE zorgt ervoor dat back-ups moeten worden versleuteld met een interne TDE-sleutel. De sleutel kan niet worden geëxporteerd, zodat u de back-up kan niet herstellen.

**Tijdelijke oplossing:** Gebruik automatische back-ups en terugzetten van de punt-in-time, of gebruik [klant beheerd (BYOK) TDE](https://docs.microsoft.com/azure/sql-database/transparent-data-encryption-azure-sql#customer-managed-transparent-data-encryption---bring-your-own-key) in plaats daarvan. U kunt versleuteling voor de database ook uitschakelen.

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over beheerde exemplaren [wat is er een beheerd exemplaar?](sql-database-managed-instance.md)
- Voor een functies en van de vergelijkingslijst, Zie [vergelijking van Azure SQL Database](sql-database-features.md).
- Zie voor een snelstartgids waarin wordt uitgelegd hoe u een nieuwe beheerd exemplaar maakt, [een beheerd exemplaar maken](sql-database-managed-instance-get-started.md).
