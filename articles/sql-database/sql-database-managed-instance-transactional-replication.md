---
title: Transactionele replicatie met Azure SQL Database | Microsoft-Docs'
description: Leer meer over SQL Server transactionele replicatie met één, gegroepeerd, en exemplaar-databases in Azure SQL Database.
services: sql-database
ms.service: sql-database
ms.subservice: data-movement
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
ms.reviewer: carlrab
manager: craigg
ms.date: 02/08/2019
ms.openlocfilehash: 409c1abd7e9f532bb243ecab00228b402215c77e
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57852754"
---
# <a name="transactional-replication-with-single-pooled-and-instance-databases-in-azure-sql-database"></a>Transactionele replicatie met één, gegroepeerd en databases in Azure SQL Database-exemplaar

Transactionele replicatie is een functie van Azure SQL Database en SQL-Server waarmee u voor het repliceren van gegevens uit een tabel in Azure SQL Database of SQL Server voor de tabellen die voor externe databases wordt geplaatst. Deze functie kunt u meerdere tabellen in verschillende databases synchroniseren.

## <a name="when-to-use-transactional-replication"></a>Wanneer u transactionele replicatie

Transactionele replicatie is handig in de volgende scenario's:
- Publiceren van wijzigingen die u in een of meer tabellen in een database en deze toewijzen aan een of meer SQL Server of Azure SQL-databases die voor de wijzigingen die zijn geabonneerd.
- Houd verschillende gedistribueerde databases gesynchroniseerde status.
- Databases van een SQL Server of Managed Instance migreren naar een andere database door continu de wijzigingen publiceert.

## <a name="overview"></a>Overzicht

De belangrijkste onderdelen van transactionele replicatie worden weergegeven in de volgende afbeelding:  

![replicatie met SQL Database](media/replication-to-sql-database/replication-to-sql-database.png)

De **Publisher** is een exemplaar of een server die wordt gepubliceerd op bepaalde tabellen (artikelen) wijzigingen door te sturen van de updates op de Distributor. Publiceren naar een Azure SQL wordt database van een on-premises SQL Server ondersteund door de volgende versies van SQL Server:

- SQL Server 2019 (preview)
- SQL Server 2016 SQL-2017
- SQL Server 2014 SP1 CU3 of hoger (12.00.4427)
- SQL Server 2014 RTM CU10 (12.00.2556)
- SQL Server 2012 SP3 of hoger (11.0.6020)
- SQL Server 2012 SP2 CU8 (11.0.5634.0)
- Voor andere versies van SQL Server die geen ondersteuning voor publicatie naar objecten in Azure, is het mogelijk gebruikmaken van de [gegevens opnieuw te publiceren](https://docs.microsoft.com/sql/relational-databases/replication/republish-data) methode om gegevens te verplaatsen naar nieuwere versies van SQL Server. 

De **Distributor** is een exemplaar of een server die wijzigingen in de artikelen van een uitgever verzamelt en verdeeld voor de abonnees. De Distributor kan Azure SQL Database Managed Instance of SQL Server (een willekeurige versie als het te lang is gelijk aan of hoger is dan de versie van de uitgever) zijn. 

De **abonnee** is een exemplaar of een server waarop de wijzigingen op de Publisher is ontvangen. Abonnees kunnen worden een enkele, samengevoegde en exemplaar-databases in Azure SQL Database of SQL Server-databases. Een abonnee op een enkele of gegroepeerde-database moet worden geconfigureerd als push-abonnement. 

| Rol | Één en gepoolde databases | Exemplaar-databases |
| :----| :------------- | :--------------- |
| **Publisher** | Nee | Ja | 
| **Distributor** | Nee | Ja|
| **Pull-abonnement** | Nee | Ja|
| **Push-abonnement**| Ja | Ja|
| &nbsp; | &nbsp; | &nbsp; |

  >[!NOTE]
  > Een pull-abonnement wordt niet ondersteund wanneer de distributor een exemplaar in de database is en de abonnee niet. 

Er zijn verschillende [replicatietypes](https://docs.microsoft.com/sql/relational-databases/replication/types-of-replication):


| Replicatie | Één en gepoolde databases | Exemplaar-databases|
| :----| :------------- | :--------------- |
| [**transactionele**](https://docs.microsoft.com/sql/relational-databases/replication/transactional/transactional-replication) | Ja (alleen als abonnee) | Ja | 
| [**momentopname**](https://docs.microsoft.com/sql/relational-databases/replication/snapshot-replication) | Ja (alleen als abonnee) | Ja|
| [**Samenvoegen van replicatie**](https://docs.microsoft.com/sql/relational-databases/replication/merge/merge-replication) | Nee | Nee|
| [**Peer-to-peer**](https://docs.microsoft.com/sql/relational-databases/replication/transactional/peer-to-peer-transactional-replication) | Nee | Nee|
| **One-way** | Ja | Ja|
| [**In twee richtingen**](https://docs.microsoft.com/sql/relational-databases/replication/transactional/bidirectional-transactional-replication) | Nee | Ja|
| [**Bij te werken abonnementen**](https://docs.microsoft.com/sql/relational-databases/replication/transactional/updatable-subscriptions-for-transactional-replication) | Nee | Nee|
| &nbsp; | &nbsp; | &nbsp; |

  >[!NOTE]
  > - -Replicatie configureren met een oudere versie willen kan resulteren in fout MSSQL_REPL20084 (het proces kan geen verbinding met abonneeserver.) en MSSQ_REPL40532 (server kan niet worden geopend \<naam > door de aanmelding is aangevraagd. De aanmelding is mislukt.)
  > - Voor het gebruik van alle functies van Azure SQL Database, moet u de nieuwste versies van [SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) en [SQL Server Data Tools (SSDT)](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt).
  
  ### <a name="supportabilty-matrix-for-instance-databases-and-on-premises-systems"></a>Supportabilty matrix voor exemplaar-Databases en On-premises systemen
  De replicatie supportability matrix bijvoorbeeld-databases zijn hetzelfde als de waarde voor on-premises SQL Server. 
  
  | **Publisher**   | **Distributor** | **Abonnee** |
| :------------   | :-------------- | :------------- |
| SQL Server 2017 | SQL Server 2017 | SQL Server 2017 <br/> SQL Server 2016 <br/> SQL Server 2014 |
| SQL Server 2016 | SQL Server 2017 <br/> SQL Server 2016 | SQL Server 2017 <br/>SQL Server 2016 <br/> SQL Server 2014 <br/> SQL Server 2012 |
| SQL Server 2014 | SQL Server 2017 <br/> SQL Server 2016 <br/> SQL Server 2014 <br/>| SQL Server 2017 <br/> SQL Server 2016 <br/> SQL Server 2014 <br/> SQL Server 2012 <br/> SQL Server 2008 R2 <br/> SQL Server 2008 |
| SQL Server 2012 | SQL Server 2017 <br/> SQL Server 2016 <br/> SQL Server 2014 <br/>SQL Server 2012 <br/> | SQL Server 2016 <br/> SQL Server 2014 <br/> SQL Server 2012 <br/> SQL Server 2008 R2 <br/> SQL Server 2008 | 
| SQL Server 2008 R2 <br/> SQL Server 2008 | SQL Server 2017 <br/> SQL Server 2016 <br/> SQL Server 2014 <br/>SQL Server 2012 <br/> SQL Server 2008 R2 <br/> SQL Server 2008 | SQL Server 2014 <br/> SQL Server 2012 <br/> SQL Server 2008 R2 <br/> SQL Server 2008 <br/>  |
| &nbsp; | &nbsp; | &nbsp; |

## <a name="requirements"></a>Vereisten

- Connectiviteit maakt gebruik van SQL-verificatie tussen replicatie deelnemers. 
- Een bestandsshare in Azure Storage-Account voor de werkmap die wordt gebruikt door middel van replicatie. 
- Poort 445 (TCP uitgaand) moet worden geopend in de regels van de Managed Instance-subnet voor toegang tot de Azure-bestandsshare. 
- Poort 1433 (TCP uitgaand) moet worden geopend als de uitgever/Distributor zich op een beheerd exemplaar en de abonnee on-premises wordt.

  >[!NOTE]
  > Fout 53 kunnen optreden bij het verbinden met een Azure Storage-bestand als de uitgaande network security group (NSG)-poort 445 is geblokkeerd wanneer de distributor een exemplaar in de database is en de abonnee on-premises wordt. [Werk het vNet NSG](/azure/storage/files/storage-troubleshoot-windows-file-connection-problems) om dit probleem te verhelpen. 

### <a name="compare-data-sync-with-transactional-replication"></a>Gegevens synchroniseren met transactionele replicatie vergelijken

| | Gegevens synchroniseren | Transactionele replicatie |
|---|---|---|
| Voordelen | -Actief / actief-ondersteuning<br/>Bi-directioneel tussen on-premises en Azure SQL Database | -Lagere latentie<br/>-Transactionele consistentie<br/>-Bestaande topologie gebruiken na de migratie |
| Nadelen | -5 min of meer latentie<br/>-Geen transactionele consistentie<br/>-Hogere invloed op de prestaties | -Uit Azure SQL Database single database of gegroepeerde database publiceren niet<br/>-Hoge onderhoudskosten |
| | | |

## <a name="common-configurations"></a>Algemene configuraties

In het algemeen de uitgever en de distributor moeten zich in de cloud of on-premises. De volgende configuraties worden ondersteund: 

### <a name="publisher-with-local-distributor-on-a-managed-instance"></a>Uitgever met lokale Distributor op een beheerd exemplaar

![Één exemplaar als de uitgever en de Distributor](media/replication-with-sql-database-managed-instance/01-single-instance-asdbmi-pubdist.png)

Uitgever en de distributor zijn geconfigureerd in een enkele Managed Instance en distributie wijzigingen naar andere beheerde exemplaar, individuele database, gepoolde database of SQL Server on-premises. In deze configuratie, de uitgever/distributor voor het beheerde exemplaar kan niet worden geconfigureerd met [Geo-replicatie en automatische failover-groepen](sql-database-auto-failover-group.md).

### <a name="publisher-with-remote-distributor-on-a-managed-instance"></a>Uitgever met externe distributor op een beheerd exemplaar

In deze configuratie is één Managed Instance publiceert wijzigingen aan distributor geplaatst op een andere Managed Instance die kunnen fungeren veel bron beheerde instanties en distribueren van wijzigingen in een of meer doelen die u in het beheerde exemplaar, individuele database, gegroepeerde database, of SQL Server.

![Afzonderlijke exemplaren voor de Publisher en de Distributor](media/replication-with-sql-database-managed-instance/02-separate-instances-asdbmi-pubdist.png)

Uitgever en de distributor worden geconfigureerd op twee exemplaren die worden beheerd. In deze configuratie

- Beide beheerde instanties zijn op hetzelfde vNet.
- Beide beheerde instanties zijn op dezelfde locatie.
- Beheerde exemplaren die als host gepubliceerd en distributor databases kunnen niet worden [geo-replicatie met behulp van automatische failover-groepen](sql-database-auto-failover-group.md).

### <a name="publisher-and-distributor-on-premises-with-a-subscriber-on-a-single-pooled-and-instance-database"></a>Uitgever en de distributor on-premises met een abonnee op een enkele, gegroepeerd en database-exemplaar 

![Azure SQL-database als abonnee](media/replication-with-sql-database-managed-instance/03-azure-sql-db-subscriber.png)
 
In deze configuratie is een Azure SQL-Database (één, gegroepeerd en database-exemplaar) een abonnee. Deze configuratie biedt ondersteuning voor migratie van on-premises naar Azure. Als een abonnee zich op een enkele of gegroepeerde-database, moet deze in de pushmodus gebruikt.  


## <a name="next-steps"></a>Volgende stappen

1. [Configureren van transactionele replicatie voor een beheerd exemplaar](replication-with-sql-database-managed-instance.md#configure-publishing-and-distribution-example). 
1. [Maken van een publicatie](https://docs.microsoft.com/sql/relational-databases/replication/publish/create-a-publication).
1. [Maken van een push-abonnement](https://docs.microsoft.com/sql/relational-databases/replication/create-a-push-subscription) met behulp van de naam van de Azure SQL Database-server als de abonnee (bijvoorbeeld `N'azuresqldbdns.database.windows.net` en de naam van de Azure SQL Database als de doeldatabase (bijvoorbeeld **Adventureworks**. )



## <a name="see-also"></a>Zie ook  

- [Replicatie naar SQL Database](replication-to-sql-database.md)
- [Replicatie naar beheerd exemplaar](replication-with-sql-database-managed-instance.md)
- [Maken van een publicatie](https://docs.microsoft.com/sql/relational-databases/replication/publish/create-a-publication)
- [Een Push-abonnement maken](https://docs.microsoft.com/sql/relational-databases/replication/create-a-push-subscription/)
- [Typen van de replicatie](https://docs.microsoft.com/sql/relational-databases/replication/types-of-replication)
- [Bewaking (replicatie)](https://docs.microsoft.com/sql/relational-databases/replication/monitor/monitoring-replication)
- [Initialiseren van een abonnement](https://docs.microsoft.com/sql/relational-databases/replication/initialize-a-subscription)  
