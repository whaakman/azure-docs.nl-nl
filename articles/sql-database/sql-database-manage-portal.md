---
title: Azure SQL-databases beheren met Azure Portal | Microsoft Docs
description: Naslaginformatie over het gebruik van Azure Portal voor het beheren van een relationele database in de cloud met behulp van Azure Portal.
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 3a56e9de-c21a-40ba-9a35-958172cb4e5b
ms.service: sql-database
ms.custom: overview
ms.devlang: NA
ms.workload: data-management
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.date: 01/10/2017
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: 420b2153f6115dd712d3033e30f11f79b18cd80f
ms.openlocfilehash: be89a2799af3bdc2938f73e3d54f00f81d9ab9cd


---
# <a name="manage-azure-sql-databases-using-the-azure-portal"></a>Azure SQL-databases beheren met Azure Portal
> [!div class="op_single_selector"]
> * [Azure Portal](sql-database-manage-portal.md)
> * [SSMS](sql-database-manage-azure-ssms.md)
> * [PowerShell](sql-database-manage-powershell.md)
> 
> 

Met [Azure Portal](https://portal.azure.com/) kunt u Azure SQL-databases en -servers maken, bewaken en beheren. Dit artikel bevat een snelle beschrijving en koppelingen naar de details van de meer algemene taken.

> [!TIP]
> Zie de [Zelfstudie Aan de slag](sql-database-get-started.md) voor een zelfstudie over het maken van een server, het maken van een op een server gebaseerde firewall, het weergeven van servereigenschappen, het maken van verbinding met behulp van SQL Server Management Studio, het uitvoeren van query's op de hoofddatabase, het maken van een voorbeelddatabase, het uitvoeren van query's op database-eigenschappen, het maken van verbinding met behulp van SQL Server Management Studio en het uitvoeren van query's op de voorbeelddatabase.

## <a name="view-your-azure-sql-databases-servers-and-pools"></a>Uw Azure SQL-databases, -servers en -pools weergeven
Als u de beschikbare SQL Database-services wilt weergeven, klikt u op **Meer services** en typt u **SQL** in het zoekvak:

![SQL Database](./media/sql-database-manage-portal/sql-services.png)

## <a name="how-do-i-create-or-view-azure-sql-databases"></a>Hoe kan ik Azure SQL-databases maken of weergeven?
Als u de blade **SQL-databases** wilt openen, klikt u op **SQL-databases** en vervolgens op de database waarmee u wilt werken. Of klik op **+ Toevoegen** om een SQL-database te maken. Zie [Maak in slechts enkele minuten een SQL-database met behulp van Azure Portal](sql-database-get-started.md) voor meer informatie.

![SQL-databases](./media/sql-database-manage-portal/sql-databases.png)

## <a name="how-do-i-create-or-view-azure-sql-servers"></a>Hoe kan ik Azure SQL-servers maken of weergeven?
Als u de blade **SQL-servers** wilt openen, klikt u op **SQL-servers** en vervolgens op de server waarmee u wilt werken. Of klik op **+ Toevoegen** om een SQL-server te maken. Zie [Maak in slechts enkele minuten een SQL-database met behulp van Azure Portal](sql-database-get-started.md) voor meer informatie.

![SQL-servers](./media/sql-database-manage-portal/sql-servers.png)

## <a name="how-do-i-create-or-view-sql-elastic-pools"></a>Hoe kan ik elastische SQL-pools maken of weergeven?
Als u de blade **Elastische SQL-pools** wilt openen, klikt u op **Elastische SQL-pools** en vervolgens op de pool waarmee u wilt werken. Of klik op **+ Toevoegen** om een pool te maken. Zie [Een elastische pool maken met Azure Portal](sql-database-elastic-pool-create-portal.md) voor meer informatie.

![Elastische SQL-pools](./media/sql-database-manage-portal/elastic-pools.png)

## <a name="how-do-i-update-or-view-sql-database-settings"></a>Hoe kan ik SQL Database-instellingen bijwerken of weergeven?
Als u de database-instellingen wilt weergeven of bijwerken, klikt u op de gewenste instelling op de blade SQL Database:

![SQL Database-instellingen](./media/sql-database-manage-portal/settings.png)

## <a name="how-do-i-find-a-sql-databases-fully-qualified-server-name"></a>Hoe vind ik de volledig gekwalificeerde servernaam van een SQL-database?
Als u de naam van uw databaseserver wilt weergeven, klikt u op de blade **SQL-database** op **Overzicht** en noteert u de servernaam:

![SQL Database-instellingen](./media/sql-database-manage-portal/server-name.png)

## <a name="how-do-i-manage-firewall-rules-to-control-access-to-my-sql-server-and-database"></a>Hoe kan ik firewallregels beheren om de toegang tot mijn SQL-server en -database te beheren?
Als u de firewallregels wilt weergeven, maken of bijwerken, klikt u op de blade **SQL-database** op **Firewallserver instellen**. Zie [Een firewallregel op serverniveau voor Azure SQL Database configureren via Azure Portal](sql-database-configure-firewall-settings.md) voor meer informatie.

![firewall-regels](./media/sql-database-manage-portal/sql-database-firewall.png)

## <a name="how-do-i-change-my-sql-database-service-tier-or-performance-level"></a>Hoe kan ik mijn SQL Database-servicelaag of -prestatieniveau wijzigen?
Om de servicelaag of het prestatieniveau van een SQL-database bij te werken, klikt u op de blade **SQL-database** op **Prijscategorie (schaal dtu's)**. Zie [De prijscategorie en het prestatieniveau (prijscategorie) van een SQL-database wijzigen](sql-database-scale-up.md).

![prijscategorieën](./media/sql-database-manage-portal/pricing-tier.png)

## <a name="how-do-i-configure-auditing-and-threat-detection-for-a-sql-database"></a>Hoe kan ik de controle en de bedreigingsdetectie configureren voor een SQL-database?
Als u de controle en bedreigingsdetectie wilt configureren voor een SQL-database, klikt u op de blade **SQL-database** op **Controle en detectie van bedreigingen**. Zie [Aan de slag met SQL Database Auditing](sql-database-auditing-get-started.md) en [Aan de slag met SQL Database-bedreigingsdetectie](sql-database-threat-detection-get-started.md) voor meer informatie.

## <a name="how-do-i-configure-dynamic-data-masking-for-a-sql-database"></a>Hoe kan ik de maskering van dynamische gegevens configureren voor een SQL-database?
Als u de maskering van dynamische gegevens wilt configureren voor een SQL-database, klikt u op de blade **SQL-database** op **Dynamische gegevensmaskering**. Zie [Aan de slag met SQL Database dynamische Gegevensmaskering](sql-database-dynamic-data-masking-get-started.md) voor meer informatie.

## <a name="how-do-i-configure-transparent-data-encryption-tde-for-a-sql-database"></a>Hoe kan ik transparante gegevensversleuteling (TDE) configureren voor een SQL-database?
Als u transparante gegevensversleuteling voor een SQL-database wilt configureren, klikt u op de blade **SQL-database** op **Transparante gegevensversleuteling**. Zie [TDE inschakelen op een database met behulp van de portal](https://msdn.microsoft.com/library/dn948096#Anchor_1) voor meer informatie.

## <a name="how-do-i-view-or-change-the-max-size-of-a-sql-database"></a>Hoe kan ik de maximale grootte van een SQL-database weergeven of wijzigen?
Als u de grootte van een SQL-database wilt weergeven of wijzigen, klikt u op de blade **SQL-database** op **Databasegrootte**. Werk de maximale grootte van een database bij door de servicelaag of het prestatieniveau te wijzigen. Zie [De prijscategorie en het prestatieniveau (prijscategorie) van een SQL-database wijzigen](sql-database-scale-up.md).

## <a name="how-do-i-monitor-and-improve-the-performance-of-a-sql-database"></a>Hoe kan ik de prestaties van een SQL-database bewaken en verbeteren?
Als u de prestatiekenmerken van een SQL-database wilt bewaken en verbeteren, klikt u op de blade **SQL-database** op **Prestatieoverzicht**. Zie [SQL Database Performance Insight](sql-database-performance.md) voor meer informatie.

## <a name="how-do-i-configure-geo-replication"></a>Hoe kan ik geo-replicatie configureren?
Als u geo-replicatie wilt instellen voor een SQL-database, klikt u op de blade **SQL-database** op **Geo-replicatie**. Zie [Geo-replicatie voor Azure SQL Database configureren via Azure Portal](sql-database-geo-replication-portal.md) voor meer informatie.

## <a name="how-do-i-fail-over-to-a-geo-replicated-sql-database"></a>Hoe kan ik een failover uitvoeren naar een via geo-replicatie gemaakte SQL-database?
Als u een failover wilt uitvoeren naar een via geo-replicatie gemaakte secundaire database, klikt u op de blade **SQL-database** op **Geo-replicatie** en vervolgens op **Failover**. Zie [Een geplande of niet-geplande failover voor Azure SQL Database initiëren via Azure Portal](sql-database-geo-replication-failover-portal.md) voor meer informatie.

## <a name="how-do-i-copy-a-sql-database"></a>Hoe kan ik een SQL-database kopiëren?
Als u een SQL-database wilt kopiëren, klikt u op de blade **SQL-database** op **Kopiëren**. Zie [Een Azure SQL-database kopiëren via Azure Portal](sql-database-copy-portal.md) voor meer informatie.

![SQL Database-instellingen](./media/sql-database-manage-portal/sql-database-copy.png)

## <a name="how-do-i-archive-an-azure-sql-database-to-a-bacpac-file"></a>Hoe kan ik een Azure SQL-database in een BACPAC-bestand archiveren?
Als u een BACPAC wilt maken van een SQL-database, klikt u op de blade **SQL-database** op **Exporteren**. Zie [Een Azure SQL-database archiveren in een BACPAC-bestand via Azure Portal](sql-database-export.md) voor meer informatie.

![SQL-database exporteren](./media/sql-database-manage-portal/sql-database-export.png)

## <a name="how-do-i-restore-a-sql-database-to-a-previous-point-in-time"></a>Hoe kan ik een SQL-database herstellen naar een eerder tijdstip?
Als u een SQL-database wilt herstellen, klikt u op de blade **SQL-database** op **Herstellen**. Zie [Een Azure SQL-database herstellen naar een vorig tijdstip via Azure Portal](sql-database-point-in-time-restore.md) voor meer informatie.

![SQL Database-instellingen](./media/sql-database-manage-portal/sql-database-restore.png)

## <a name="how-do-i-create-an-azure-sql-database-from-a-bacpac-file"></a>Hoe kan ik een Azure SQL-database maken vanuit een BACPAC-bestand?
Als u een SQL-database wilt maken vanuit een BACPAC-bestand, klikt u op de blade **SQL Server** op **Database importeren**. Zie [een BACPAC-bestand voor het maken van een Azure SQL-database importeren](sql-database-import.md) voor meer informatie.

![SQL Server](./media/sql-database-manage-portal/server-commands.png)

## <a name="how-do-i-restore-a-deleted-sql-database"></a>Hoe kan ik een verwijderde SQL-database herstellen?
Als u een verwijderde SQL-database wilt herstellen, klikt u op de blade **SQL-server** (de SQL-server waarop de verwijderde database stond) op **Verwijderde databases**. Zie [Een verwijderde Azure SQL-database herstellen via Azure Portal](sql-database-restore-deleted-database-portal.md) voor meer informatie.

## <a name="how-do-i-delete-a-sql-database"></a>Hoe verwijder ik een SQL-database?
Als u een SQL-database wilt verwijderen, klikt u op de blade **SQL-database** op **Verwijderen**. 

![SQL Database-instellingen](./media/sql-database-manage-portal/sql-database-delete.png)

## <a name="additional-resources"></a>Aanvullende bronnen
* [SQL Database](sql-database-technical-overview.md)
* [Een elastische pool bewaken en beheren met Azure Portal](sql-database-elastic-pool-manage-portal.md)




<!--HONumber=Dec16_HO3-->


