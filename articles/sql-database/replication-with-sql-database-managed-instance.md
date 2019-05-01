---
title: Replicatie in een Azure SQL Database beheerde exemplaar in de database configureren | Microsoft Docs
description: Meer informatie over het configureren van transactionele replicatie in een Azure SQL Database beheerde Exemplaardatabase
services: sql-database
ms.service: sql-database
ms.subservice: data-movement
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: allenwux
ms.author: xiwu
ms.reviewer: mathoma
manager: craigg
ms.date: 02/07/2019
ms.openlocfilehash: c72c4d21f948d6d6c4d1d4598efa0e13de9705a6
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/30/2019
ms.locfileid: "64926201"
---
# <a name="configure-replication-in-an-azure-sql-database-managed-instance-database"></a>Replicatie in een Azure SQL Database beheerde Exemplaardatabase configureren

Transactionele replicatie kunt u voor het repliceren van gegevens in een Azure SQL Database beheerde exemplaar in de database uit een SQL Server-database of een ander exemplaar in de database. 

U kunt transactionele replicatie ook gebruiken om te wijzigingen aangebracht in een exemplaar in de database in Azure SQL-Database beheerd exemplaar te pushen:

- Een SQL Server-database.
- Een individuele database in Azure SQL Database.
- Een gegroepeerde-database in een Azure SQL Database elastische pool.
 
Transactionele replicatie is in openbare preview-versie op [Azure SQL Database managed instance](sql-database-managed-instance.md). Een beheerd exemplaar van kan uitgever, distributor en subscriber-databases hosten. Zie [transactionele replicatie configuraties](sql-database-managed-instance-transactional-replication.md#common-configurations) voor beschikbare configuraties.

  > [!NOTE]
  > Dit artikel is bedoeld als richtlijn voor een gebruiker bij het configureren van replicatie met een Azure-Database beheerd exemplaar van begin tot eind, beginnen met het maken van de resourcegroep. Als u al hebt instanties zijn geïmplementeerd wordt beheerd, gaat u verder met [stap 4](#4---create-a-publisher-database) te maken van de database publisher of [stap 6](#6---configure-distribution) als u al een publisher en de database hebt, en zijn klaar om te beginnen configureren van replicatie.  

## <a name="requirements"></a>Vereisten

Het configureren van een beheerd exemplaar als een uitgever en/of een distributor is vereist:

- Dat het beheerde exemplaar niet momenteel aan een geo-replicatie-relatie deelneemt.
- Op de distributor als de abonnee hetzelfde virtuele netwerk dat de uitgever beheerd exemplaar is of [vNet-peering](../virtual-network/tutorial-connect-virtual-networks-powershell.md) is tot stand is gebracht tussen de virtuele netwerken van alle drie entiteiten. 
- Connectiviteit maakt gebruik van SQL-verificatie tussen replicatie deelnemers.
- Een bestandsshare in Azure Storage-Account voor de replicatie-werkmap.
- Poort 445 (TCP uitgaand) is geopend in de beveiligingsregels van NSG voor de beheerde exemplaren voor toegang tot de Azure-bestandsshare. 


 > [!NOTE]
 > Individuele databases en databases in pools in Azure SQL Database kunnen alleen worden voor abonnees. 


## <a name="features"></a>Functies

Ondersteunt:

- De combinatie van de transactionele en momentopname replicatie van on-premises SQL Server en beheerde exemplaren in Azure SQL Database.
- Abonnees kunnen zich in de on-premises SQL Server-databases, enkele databases/beheerde instanties in Azure SQL Database, of gepoolde databases in Azure SQL Database elastische pools.
- Eenzijdige of replicatie in twee richtingen.

De volgende functies worden niet ondersteund in een beheerd exemplaar in Azure SQL Database:

- [Bij te werken abonnementen](/sql/relational-databases/replication/transactional/updatable-subscriptions-for-transactional-replication).
- [Actieve geo-replicatie](sql-database-active-geo-replication.md) en [automatische failovergroepen](sql-database-auto-failover-group.md) mag niet worden gebruikt als de transactionele replicatie is geconfigureerd.
 
## <a name="1---create-a-resource-group"></a>1 - een resourcegroep maken

Gebruik de [Azure-portal](https://portal.azure.com) te maken van een resourcegroep met de naam van de `SQLMI-Repl`.  

## <a name="2---create-managed-instances"></a>2 - een met het maken van beheerde exemplaren

Gebruik de [Azure-portal](https://portal.azure.com) maakt twee [instanties die worden beheerd](sql-database-managed-instance-create-tutorial-portal.md) in hetzelfde virtuele netwerk en subnet bevindt. De twee beheerde exemplaren moeten worden genoemd:

- `sql-mi-pub`
- `sql-mi-sub`

U moet ook [configureren van een Azure-VM verbinding](sql-database-managed-instance-configure-vm.md) met uw Azure SQL Database beheerde exemplaren. 

## <a name="3---create-azure-storage-account"></a>3 - Azure Storage-Account maken

[Maak een Azure Storage-Account](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account#create-a-storage-account) voor de werkmap en maak vervolgens een [bestandsshare](../storage/files/storage-how-to-create-file-share.md) binnen het opslagaccount. 

Kopieer het pad naar de bestandsshare in de indeling van: `\\storage-account-name.file.core.windows.net\file-share-name`

Kopieer de toegangssleutels voor opslag in de indeling van: `DefaultEndpointsProtocol=https;AccountName=<Storage-Account-Name>;AccountKey=****;EndpointSuffix=core.windows.net`

 Zie [Opslagtoegangssleutels bekijken en kopiëren](../storage/common/storage-account-manage.md#access-keys) voor meer informatie. 

## <a name="4---create-a-publisher-database"></a>4 - een publisher-database maken

Verbinding maken met uw `sql-mi-pub` beheerd exemplaar met behulp van SQL Server Management Studio en voer de volgende Transact-SQL (T-SQL)-code voor het maken van de publisher-database:

```sql
USE [master]
GO

CREATE DATABASE [ReplTran_PUB]
GO

USE [ReplTran_PUB]
GO
CREATE TABLE ReplTest (
    ID INT NOT NULL PRIMARY KEY,
    c1 VARCHAR(100) NOT NULL,
    dt1 DATETIME NOT NULL DEFAULT getdate()
)
GO


USE [ReplTran_PUB]
GO

INSERT INTO ReplTest (ID, c1) VALUES (6, 'pub')
INSERT INTO ReplTest (ID, c1) VALUES (2, 'pub')
INSERT INTO ReplTest (ID, c1) VALUES (3, 'pub')
INSERT INTO ReplTest (ID, c1) VALUES (4, 'pub')
INSERT INTO ReplTest (ID, c1) VALUES (5, 'pub')
GO
SELECT * FROM ReplTest
GO
```

## <a name="5---create-a-subscriber-database"></a>5 - een abonneedatabase maken

Verbinding maken met uw `sql-mi-sub` beheerd exemplaar met behulp van SQL Server Management Studio en voer de volgende T-SQL-code voor het maken van uw database leeg abonnee:

```sql
USE [master]
GO

CREATE DATABASE [ReplTran_SUB]
GO

USE [ReplTran_SUB]
GO
CREATE TABLE ReplTest (
    ID INT NOT NULL PRIMARY KEY,
    c1 VARCHAR(100) NOT NULL,
    dt1 DATETIME NOT NULL DEFAULT getdate()
)
GO
```

## <a name="6---configure-distribution"></a>6 - distributie configureren

Verbinding maken met uw `sql-mi-pub` beheerd exemplaar met behulp van SQL Server Management Studio en voer de volgende T-SQL-code voor het configureren van de distributiedatabase. 

```sql
USE [master]
GO

EXEC sp_adddistributor @distributor = @@ServerName;
EXEC sp_adddistributiondb @database = N'distribution';
GO
```

## <a name="7---configure-publisher-to-use-distributor"></a>7 - uitgever voor het gebruik van de distributor configureren 

Beheerd exemplaar van de uitgever `sql-mi-pub`, wijzigen van de uitvoering van de query naar [SQLCMD](/sql/ssms/scripting/edit-sqlcmd-scripts-with-query-editor) modus en voer de volgende code om de nieuwe distributor registreren bij de uitgever. 

```sql
:setvar username loginUsedToAccessSourceManagedInstance
:setvar password passwordUsedToAccessSourceManagedInstance
:setvar file_storage "\\storage-account-name.file.core.windows.net\file-share-name"
:setvar file_storage_key "DefaultEndpointsProtocol=https;AccountName=<Storage-Account-Name>;AccountKey=****;EndpointSuffix=core.windows.net"


USE [master]
EXEC sp_adddistpublisher
  @publisher = @@ServerName,
  @distribution_db = N'distribution',
  @security_mode = 0,
  @login = N'$(username)',
  @password = N'$(password)',
  @working_directory = N'$(file_storage)',
  @storage_connection_string = N'$(file_storage_key)';
```

Dit script wordt een lokale publisher geconfigureerd op het beheerde exemplaar, voegt u een gekoppelde server en maakt een set taken voor de SQL Server Agent. 

## <a name="8---create-publication-and-subscriber"></a>8 - publicatie en abonnee maken

Met behulp van [SQLCMD](/sql/ssms/scripting/edit-sqlcmd-scripts-with-query-editor) modus, voer het volgende T-SQL-script voor het inschakelen van replicatie voor uw database en replicatie tussen uw uitgever, distributor en subscriber configureren. 

```sql
-- Set variables
:setvar username sourceLogin
:setvar password sourcePassword
:setvar source_db ReplTran_PUB
:setvar publication_name PublishData
:setvar object ReplTest
:setvar schema dbo
:setvar target_server "sql-mi-sub.wdec33262scj9dr27.database.windows.net"
:setvar target_username targetLogin
:setvar target_password targetPassword
:setvar target_db ReplTran_SUB

-- Enable replication for your source database
USE [$(source_db)]
EXEC sp_replicationdboption
  @dbname = N'$(source_db)',
  @optname = N'publish',
  @value = N'true';

-- Create your publication
EXEC sp_addpublication
  @publication = N'$(publication_name)',
  @status = N'active';


-- Configure your log reaer agent
EXEC sp_changelogreader_agent
  @publisher_security_mode = 0,
  @publisher_login = N'$(username)',
  @publisher_password = N'$(password)',
  @job_login = N'$(username)',
  @job_password = N'$(password)';

-- Add the publication snapshot
EXEC sp_addpublication_snapshot
  @publication = N'$(publication_name)',
  @frequency_type = 1,
  @publisher_security_mode = 0,
  @publisher_login = N'$(username)',
  @publisher_password = N'$(password)',
  @job_login = N'$(username)',
  @job_password = N'$(password)';

-- Add the ReplTest table to the publication
EXEC sp_addarticle 
  @publication = N'$(publication_name)',
  @type = N'logbased',
  @article = N'$(object)',
  @source_object = N'$(object)',
  @source_owner = N'$(schema)';

-- Add the subscriber
EXEC sp_addsubscription
  @publication = N'$(publication_name)',
  @subscriber = N'$(target_server)',
  @destination_db = N'$(target_db)',
  @subscription_type = N'Push';

-- Create the push subscription agent
EXEC sp_addpushsubscription_agent
  @publication = N'$(publication_name)',
  @subscriber = N'$(target_server)',
  @subscriber_db = N'$(target_db)',
  @subscriber_security_mode = 0,
  @subscriber_login = N'$(target_username)',
  @subscriber_password = N'$(target_password)',
  @job_login = N'$(target_username)',
  @job_password = N'$(target_password)';

-- Initialize the snapshot
EXEC sp_startpublication_snapshot
  @publication = N'$(publication_name)';
```

## <a name="9---modify-agent-parameters"></a>9 - parameters van de agent wijzigen

Beheerd exemplaar voor Azure SQL-Database is momenteel enkele problemen met back-end met de connectiviteit met de replicatieagents. Als dit probleem is worden binnenkort aangepakt opgelost, wordt de tijdelijke oplossing voor het verhogen van de aanmelding time-outwaarde voor de replicatieagents. 

Voer de volgende T-SQL-opdracht op de publisher te verhogen van de time-out voor aanmelden: 

```sql
-- Increase login timeout to 150s
update msdb..sysjobsteps set command = command + N' -LoginTimeout 150' 
where subsystem in ('Distribution','LogReader','Snapshot') and command not like '%-LoginTimeout %'
```

Voer de volgende T-SQL-opdracht opnieuw uit om in te stellen de aanmeldingstime-out terug naar de standaardwaarde, als u wilt doen:

```sql
-- Increase login timeout to 30
update msdb..sysjobsteps set command = command + N' -LoginTimeout 30' 
where subsystem in ('Distribution','LogReader','Snapshot') and command not like '%-LoginTimeout %'
```

Start opnieuw op alle drie agenten om deze wijzigingen te laten. 

## <a name="10---test-replication"></a>10 - en replicatie

Wanneer replicatie is geconfigureerd, kunt u deze testen door het invoegen van nieuwe items op de publisher en volgt de wijzigingen doorgeven aan de abonnee. 

Voer het volgende codefragment T-SQL om de rijen op de abonnee weer te geven:

```sql
select * from dbo.ReplTest
```

Voer het volgende codefragment T-SQL voor het invoegen van extra rijen op de publisher en controleer vervolgens de rijen opnieuw op de abonnee. 

```sql
INSERT INTO ReplTest (ID, c1) VALUES (15, 'pub')
```

## <a name="clean-up-resources"></a>Resources opschonen

Als u wilt de publicatie verwijderen, moet u de volgende T-SQL-opdracht uitvoeren:

```sql
-- Drops the publication
USE [ReplTran_PUB]
EXEC sp_droppublication @publication = N'PublishData'
GO
```

Als u wilt de replicatie-optie verwijderen uit de database, moet u de volgende T-SQL-opdracht uitvoeren:

```sql
-- Disables publishing of the database
USE [ReplTran_PUB]
EXEC sp_removedbreplication
GO
```

Als u wilt publiceren en distribueren uitschakelen, moet u de volgende T-SQL-opdracht uitvoeren:

```sql
-- Drops the distributor
USE [master]
EXEC sp_dropdistributor @no_checks = 1
GO
```

U kunt uw Azure-resources door opschonen [de resources beheerd exemplaar verwijderen uit de resourcegroep](../azure-resource-manager/manage-resources-portal.md#delete-resources) en vervolgens de resourcegroep te verwijderen `SQLMI-Repl`. 

   
## <a name="see-also"></a>Zie ook

- [Transactionele replicatie](sql-database-managed-instance-transactional-replication.md)
- [Wat is een beheerd exemplaar?](sql-database-managed-instance.md)
