---
title: Transactionele replicatie met logische Azure SQL-Server en Azure SQL Managed Instance | Microsoft-Docs'
description: Meer informatie over het gebruik van SQL Server transactionele replicatie met Azure SQL Database logische Servers en SQL Managed Instance.
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
ms.date: 01/08/2019
ms.openlocfilehash: 1839ca0d2495a07f6fc734501540cddcdcb28e18
ms.sourcegitcommit: dede0c5cbb2bd975349b6286c48456cfd270d6e9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/16/2019
ms.locfileid: "54332852"
---
# <a name="transactional-replication-with-azure-sql-logical-server-and-azure-sql-managed-instance"></a>Transactionele replicatie met logische Azure SQL-Server en Azure SQL Managed Instance

Transactionele replicatie is een functie van Azure SQL Database Managed Instance en SQL-Server waarmee u voor het repliceren van gegevens uit een tabel in Azure SQL Database of SQL Server voor de tabellen die voor externe databases wordt geplaatst. Deze functie kunt u meerdere tabellen in verschillende databases synchroniseren.

## <a name="overview"></a>Overzicht 
De belangrijkste onderdelen van transactionele replicatie worden weergegeven in de volgende afbeelding:  

![replicatie met SQL Database](media/replication-to-sql-database/replication-to-sql-database.png)


De **Publisher** is een exemplaar of een server die wordt gepubliceerd op bepaalde tabellen (artikelen) wijzigingen door te sturen van de updates op de Distributor. Publiceren naar een Azure SQL Database van een on-premises SQL Server wordt ondersteund op de volgende versies van SQL Server:
    - SQL Server 2019 (preview)
    - SQL Server 2016 SQL-2017
    - SQL Server 2014 SP1 CU3 of hoger (12.00.4427)
    - SQL Server 2014 RTM CU10 (12.00.2556)
    - SQL Server 2012 SP3 of hoger (11.0.6020)
    - SQL Server 2012 SP2 CU8 (11.0.5634.0)
    - Voor andere versies van SQL Server die geen ondersteuning voor publicatie naar objecten in Azure, is het mogelijk gebruikmaken van de [gegevens opnieuw te publiceren](https://docs.microsoft.com/sql/relational-databases/replication/republish-data) methode om gegevens te verplaatsen naar nieuwere versies van SQL Server. 

De **Distributor** is een exemplaar of een server die wijzigingen in de artikelen van een uitgever verzamelt en verdeeld voor de abonnees. De Distributor kan Azure SQL Database Managed Instance of SQL Server (een willekeurige versie als het te lang is gelijk aan of hoger is dan de versie van de uitgever) zijn. 

De **abonnee** is een exemplaar of een server waarop de wijzigingen op de Publisher is ontvangen. Abonnees kunnen worden Azure SQL Database logische Server/Managed Instance of SQL Server. Een abonnee op een logische Server moet worden geconfigureerd als push-abonnement. 

| Rol | Logische Server | Beheerd exemplaar |
| :----| :------------- | :--------------- |
| **Publisher** | Nee | Ja | 
| **Distributor** | Nee | Ja|
| **Pull-abonnement** | Nee | Ja|
| **Push-abonnement**| Ja | Ja|
| &nbsp; | &nbsp; | &nbsp; |

Er zijn verschillende [replicatietypes](https://docs.microsoft.com/sql/relational-databases/replication/types-of-replication?view=sql-server-2017):


| Replicatie | Logische Server | Beheerd exemplaar |
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
  > - -Replicatie configureren met een oudere versie willen kan resulteren in fout nummer MSSQL_REPL20084 (het proces kan geen verbinding met abonneeserver.) en MSSQ_REPL40532 (server kan niet worden geopend <name> aangevraagd door de aanmelding. De aanmelding is mislukt.)
  > - Voor het gebruik van alle functies van Azure SQL Database, moet u de nieuwste versies van [SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-2017) en [SQL Server Data Tools (SSDT)](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt?view=sql-server-2017).

## <a name="requirements"></a>Vereisten
- Connectiviteit maakt gebruik van SQL-verificatie tussen replicatie deelnemers. 
- Een bestandsshare in Azure Storage-Account voor de werkmap die wordt gebruikt door middel van replicatie. 
- Poort 445 (TCP uitgaand) moet worden geopend in de regels van de Managed Instance-subnet voor toegang tot de Azure-bestandsshare. 
- Poort 1433 (TCP uitgaand) moet worden geopend als de uitgever/Distributor zich op een beheerd exemplaar en de abonnee on-premises wordt. 

## <a name="common-configurations"></a>Algemene configuraties
In het algemeen de uitgever en de distributor moeten zich in de cloud of on-premises. De volgende configuraties worden ondersteund: 

### <a name="publisher-with-local-distributor-on-a-managed-instance"></a>Uitgever met lokale Distributor op een beheerd exemplaar

![Één exemplaar als de uitgever en de Distributor ](media/replication-with-sql-database-managed-instance/01-single-instance-asdbmi-pubdist.png)

Uitgever en de distributor zijn in één beheerd exemplaar geconfigureerd. 

### <a name="publisher-with-remote-distributor-on-a-managed-instance"></a>Uitgever met externe distributor op een beheerd exemplaar

![Afzonderlijke exemplaren voor de Publisher en de Distributor](media/replication-with-sql-database-managed-instance/02-separate-instances-asdbmi-pubdist.png)

Uitgever en de distributor worden geconfigureerd op twee exemplaren die worden beheerd. In deze configuratie

- Beide beheerde instanties zijn op hetzelfde vNet.
- Beide beheerde instanties zijn op dezelfde locatie. 

### <a name="publisher-and-distributor-on-premises-with-a-subscriber-on-a-managed-instance-or-logical-server"></a>Uitgever en de distributor on-premises met een abonnee op een beheerd exemplaar of de logische Server 

![Azure SQL-database als abonnee](media/replication-with-sql-database-managed-instance/03-azure-sql-db-subscriber.png)
 
In deze configuratie is een Azure SQL-Database (Managed Instance of logische Server) een abonnee. Deze configuratie biedt ondersteuning voor migratie van on-premises naar Azure. Als een abonnee zich op logische Server moet deze in de pushmodus gebruikt.  

## <a name="next-steps"></a>Volgende stappen
1. [Configureren van transactionele replicatie voor een beheerd exemplaar](https://docs.microsoft.com/azure/sql-database/replication-with-sql-database-managed-instance#configure-publishing-and-distribution-example). 
1. [Maken van een publicatie](https://docs.microsoft.com/sql/relational-databases/replication/publish/create-a-publication).
1. [Maken van een push-abonnement](https://docs.microsoft.com/sql/relational-databases/replication/create-a-push-subscription) met behulp van de logische Azure SQL Database-servernaam als de abonnee (bijvoorbeeld `N'azuresqldbdns.database.windows.net` en de naam van de Azure SQL Database als de doeldatabase (bijvoorbeeld **Adventureworks**. )


## <a name="see-also"></a>Zie ook  

- [Replicatie naar SQL Database](replication-to-sql-database.md)
- [Replicatie naar beheerd exemplaar](replication-with-sql-database-managed-instance.md)
- [Maken van een publicatie](https://docs.microsoft.com/sql/relational-databases/replication/publish/create-a-publication)
- [Een Push-abonnement maken](https://docs.microsoft.com/sql/relational-databases/replication/create-a-push-subscription/)
- [Typen van de replicatie](https://docs.microsoft.com/sql/relational-databases/replication/types-of-replication)
- [Bewaking (replicatie)](https://docs.microsoft.com/sql/relational-databases/replication/monitor/monitoring-replication)
- [Initialiseren van een abonnement](https://docs.microsoft.com/sql/relational-databases/replication/initialize-a-subscription)  
