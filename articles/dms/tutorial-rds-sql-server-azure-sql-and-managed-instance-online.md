---
title: 'Zelfstudie: Azure Database Migration Service gebruiken voor een online migratie van extern bureaublad-services SQL Server naar Azure SQL Database of naar een beheerd exemplaar voor Azure SQL Database | Microsoft Docs'
description: Meer informatie over het uitvoeren van een online migratie van extern bureaublad-services SQL Server naar Azure SQL Database of naar een Azure SQL-Database beheerd exemplaar met behulp van de Azure Database Migration Service.
services: dms
author: HJToland3
ms.author: jtoland
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: mvc, tutorial
ms.topic: article
ms.date: 05/08/2019
ms.openlocfilehash: 982ead69ba3d206e1aa2538597927dcbeaab70e9
ms.sourcegitcommit: 300cd05584101affac1060c2863200f1ebda76b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/08/2019
ms.locfileid: "65416102"
---
# <a name="tutorial-migrate-rds-sql-server-to-azure-sql-database-or-an-azure-sql-database-managed-instance-online-using-dms"></a>Zelfstudie: SQL-Server voor extern bureaublad-services migreren naar Azure SQL Database of een Azure SQL-Database beheerd exemplaar online met behulp van DMS
U kunt de Azure Database Migration Service gebruiken voor het migreren van de databases van een extern bureaublad-services SQL Server-exemplaar naar [Azure SQL Database](https://docs.microsoft.com/azure/sql-database/) of een [Azure SQL Database managed instance](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-index) met minimale downtime. In deze zelfstudie, migreert u de **Adventureworks2012** database hersteld naar een extern bureaublad-services SQL Server exemplaar van SQL Server 2012 (of hoger) naar Azure SQL Database of een Azure SQL-Database beheerd exemplaar met behulp van de Azure Database Migration De service.

In deze zelfstudie leert u het volgende:
> [!div class="checklist"]
> * Maak een exemplaar van Azure SQL Database of een beheerd exemplaar voor Azure SQL Database. 
> * Het voorbeeldschema migreren met behulp van de Data Migration Assistant.
> * De Azure-portal gebruiken om een Azure Database Migration Service-exemplaar te maken.
> * Een migratieproject maken met behulp van de Azure Database Migration Service.
> * De migratie uitvoeren.
> * De migratie controleren.
> * Een migratierapport downloaden.

> [!NOTE]
> Als u Azure Database Migration Service gebruikt om een onlinemigratie uit te voeren, is het vereist dat u een exemplaar maakt op basis van de prijscategorie Premium. Zie de pagina met [prijzen](https://azure.microsoft.com/pricing/details/database-migration/) van Azure Database Migration Service voor meer informatie.

> [!IMPORTANT]
> Voor een optimale migratie-ervaring raadt Microsoft u aan een exemplaar van de Azure Database Migration Service te maken in dezelfde Azure-regio als de doeldatabase. Het verplaatsen van gegevens naar regio's of geografieën kan het migratieproces vertragen en fouten veroorzaken.

[!INCLUDE [online-offline](../../includes/database-migration-service-offline-online.md)]

Dit artikel beschrijft een online migratie van extern bureaublad-services SQL Server naar Azure SQL Database of een beheerd exemplaar voor Azure SQL Database.

## <a name="prerequisites"></a>Vereisten
Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

* Maak een [RDS SQL Server-database](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_GettingStarted.CreatingConnecting.SQLServer.html).
* Maak een Azure SQL Database. Dit doet u door de details in het artikel [Een Azure SQL-database maken in de Microsoft Azure-portal](https://docs.microsoft.com/azure/sql-database/sql-database-get-started-portal) te volgen.

    > [!NOTE]
    > Als u naar een beheerd exemplaar voor Azure SQL Database migreert, volgt u de details in het artikel [maken van een beheerd exemplaar voor Azure SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-get-started), en maak vervolgens een lege database met de naam **AdventureWorks2012**. 
 
* Download en installeer de [Data Migration Assistant](https://www.microsoft.com/download/details.aspx?id=53595) (DMA) v3.3 of hoger.
* Maak een Azure Virtual Network (VNet) voor de Azure Database Migration Service met behulp van het Azure Resource Manager-implementatiemodel. Als u naar een beheerd exemplaar voor Azure SQL Database migreert bent, zorg ervoor dat de DMS-exemplaar maakt in hetzelfde VNet gebruikt voor het beheerde exemplaar van Azure SQL Database, maar in een ander subnet.  Als u een ander VNet voor DMS gebruikt, moet u ook een VNet-peering tussen de twee vnet's maken. Zie voor meer informatie over het maken van een VNet, de [documentatie voor Virtual Network](https://docs.microsoft.com/azure/virtual-network/), en met name de artikelen met vindt u meer details.

    > [!NOTE]
    > Tijdens de installatie van de VNet, als u ExpressRoute gebruikt met het naar Microsoft-netwerkpeering, voeg de volgende service [eindpunten](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview) aan het subnet waarin de service worden ingericht:
    >
    > * Doel-database-eindpunt (bijvoorbeeld SQL-eindpunt, Cosmos DB-eindpunt, enzovoort)
    > * Opslageindpunt
    > * Service bus-eindpunt
    >
    > Deze configuratie is nodig omdat de Azure Database Migration Service beschikt niet over de verbinding met internet. 

* Zorg ervoor dat uw VNet netwerkbeveiligingsgroepsregels de volgende poorten voor binnenkomende communicatie naar Azure Database Migration Service niet blokkeren: 443, 53, 9354, 445, 12000. Zie het artikel voor meer informatie over Azure VNet NSG wordt verkeer gefilterd, [netwerkverkeer filteren met netwerkbeveiligingsgroepen](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg).
* Configureer uw [Windows Firewall voor toegang tot de database-engine](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access).
* Open uw Windows-firewall voor toegang voor de Azure Database Migration Service tot de SQL Server-bron, die standaard TCP-poort 1433 gebruikt.
* Maak een [firewallregel](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) op serverniveau voor de Azure SQL Database-server om voor de Azure Database Migration Service toegang tot de doeldatabase toe te staan. Geef het subnetbereik van het VNET op dat wordt gebruikt voor de Azure Database Migration Service.
* Zorg ervoor dat de referenties waarmee verbinding wordt gemaakt met het bronexemplaar voor RDS SQL Server gekoppeld zijn aan een account dat lid is van de serverrol 'Processadmin' en van de databaserollen 'db_owner' op alle databases die moeten worden gemigreerd.
* Zorg ervoor dat de referenties waarmee verbinding maken met Azure SQL Database-doelexemplaar machtiging CONTROL DATABASE op de doel-Azure SQL-databases en lid van de rol sysadmin hebben als de migratie naar een Azure SQL-Database beheerd exemplaar.
* De versie van de RDS SQL-bronserver moet SQL Server 2012 of hoger zijn. Zie het artikel [Hoe de versie, de editie en het updateniveau van de SQL-server en de bijbehorende onderdelen worden bepaald](https://support.microsoft.com/help/321185/how-to-determine-the-version-edition-and-update-level-of-sql-server-an) om te bepalen welke versie van uw SQL Server-exemplaar wordt uitgevoerd.
* Inschakelen van Change Data Capture (CDC) op de RDS SQL Server-database en alle gebruikerstabellen die geselecteerd zijn voor migratie.
    > [!NOTE]
    > U kunt het onderstaande script gebruiken om CDC in te schakelen op een RDS SQL Server-database.
    ```
    exec msdb.dbo.rds_cdc_enable_db 'AdventureWorks2012'
    ```
    > U kunt het onderstaande script gebruiken om CDC in te schakelen op alle tabellen.
    ```
    use <Database name>
    go
    exec sys.sp_cdc_enable_table 
    @source_schema = N'Schema name', 
    @source_name = N'table name', 
    @role_name = NULL, 
    @supports_net_changes = 1 --for PK table 1, non PK tables 0
    GO
    ```
* Schakel databasetriggers voor de Azure SQL-doeldatabase uit.
    > [!NOTE]
    > U kunt de databasetriggers voor de Azure SQL-doeldatabase zoeken met behulp van de volgende query:
    ```
    Use <Database name>
    select * from sys.triggers
    DISABLE TRIGGER (Transact-SQL)
    ```
    Zie het artikel [TRIGGER UITSCHAKELEN (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/statements/disable-trigger-transact-sql?view=sql-server-2017) voor meer informatie.

## <a name="migrate-the-sample-schema"></a>Het voorbeeldschema migreren
DMA gebruiken voor het migreren van het schema naar Azure SQL Database.

> [!NOTE]
> Voordat u een migratieproject in DMA maakt, moet u ervoor zorgen dat u al een Azure SQL-database hebt ingericht zoals is vermeld in de vereisten. Voor deze zelfstudie wordt ervan uitgegaan dat de naam van de Azure SQL Database **AdventureWorks2012** is, maar u kunt elke naam die u wenst opgeven.

Voer voor het migreren van het schema **AdventureWorks2012** naar Azure SQL Database, de volgende stappen uit:

1. Selecteer het pictogram Nieuw (+) in de Data Migration Assistant, en selecteer vervolgens onder **Projecttype** **Migratie**.
2. Geef een projectnaam op, selecteer in het tekstvak **Bronservertype** **SQL Server**en in het tekstvak **Doelservertype** **Azure SQL Database**.
3. Selecteer onder **Migratiebereik**, selecteer **Alleen schema**.

    Na het uitvoeren van de vorige stappen, moet de DMA-interface worden weergegeven zoals in de volgende afbeelding:

    ![Data Migration Assistant-Project maken](media/tutorial-rds-sql-to-azure-sql-and-managed-instance/dma-create-project.png)

4. Selecteer **Maken** om het project te maken.
5. Geef in DMA de details van de bronverbinding voor uw SQL Server op, selecteer **Verbinden** en selecteer vervolgens de database **AdventureWorks2012**.

    ![Details Data Migration Assistant Bronverbinding](media/tutorial-rds-sql-to-azure-sql-and-managed-instance/dma-source-connect.png)

6. Selecteer **Volgende** onder **Verbinding maken met doelserver**, geef de details van de doelverbinding voor de Azure SQL Database op, selecteer **Verbinden** en selecteer vervolgens de database **AdventureWorksAzure** die u vooraf hebt ingericht in Azure SQL Database.

    ![Details Data Migration Assistant-doelverbinding](media/tutorial-rds-sql-to-azure-sql-and-managed-instance/dma-target-connect.png)

7. Selecteer **Volgende** om door te gaan naar het scherm **Objecten selecteren** waarin u de schemaobjecten in de **AdventureWorks2012**-database kunt opgeven, die moeten worden geïmplementeerd naar Azure SQL Database.

    Standaard worden alle objecten geselecteerd.

    ![SQL-scripts genereren](media/tutorial-rds-sql-to-azure-sql-and-managed-instance/dma-assessment-source.png)

8. Selecteer **SQL-script genereren** voor het maken van de SQL-scripts en controleer vervolgens de scripts op fouten.

    ![Schemascript](media/tutorial-rds-sql-to-azure-sql-and-managed-instance/dma-schema-script.png)

9. Selecteer **Schema implementeren** om het schema te implementeren naar Azure SQL Database, en nadat het schema is geïmplementeerd, controleer de doelserver op eventuele afwijkingen.

    ![Schema implementeren](media/tutorial-rds-sql-to-azure-sql-and-managed-instance/dma-schema-deploy.png)

## <a name="register-the-microsoftdatamigration-resource-provider"></a>Registreer de Microsoft.DataMigration-resourceprovider

1. Meld u aan bij de Azure-portal, selecteer **Alle services** en selecteer vervolgens **Abonnementen**.

   ![Portal-abonnementen weergeven](media/tutorial-rds-sql-to-azure-sql-and-managed-instance/portal-select-subscription1.png)

2. Selecteer het abonnement waarin u het Azure Database Migration Service-exemplaar wilt maken en selecteer vervolgens **Resourceproviders**.

    ![Resourceproviders weergeven](media/tutorial-sql-server-to-azure-sql-online/portal-select-resource-provider.png)

3. Zoek naar migratie en selecteer rechts van **Microsoft.DataMigration** de optie **Registreren**.

    ![Resourceprovider registreren](media/tutorial-rds-sql-to-azure-sql-and-managed-instance/portal-register-resource-provider.png)    

## <a name="create-an-instance"></a>Een instantie maken

1. Selecteer in de Azure-portal **Een resource maken**, zoek naar Azure Database Migration Service, en selecteer vervolgens **Azure Database Migration Service** uit de vervolgkeuzelijst.

    ![Azure Marketplace](media/tutorial-rds-sql-to-azure-sql-and-managed-instance/portal-marketplace.png)

2. Selecteer in het scherm **Azure Database Migration Service** **Maken**.

    ![Azure Database Migration Service-exemplaar maken](media/tutorial-rds-sql-to-azure-sql-and-managed-instance/dms-create1.png)
  
3. Geef in het scherm **Migratieservice maken** een naam op voor de service, het abonnement en een nieuwe of bestaande resourcegroep.

4. Selecteer het abonnement waarin u het Azure Database Migration Service-exemplaar wilt maken. 

5. Selecteer een bestaand VNet of maak een nieuwe.

    Het VNet biedt de Azure Database Migration Service toegang tot de bron-SQL Server en de doel-Azure SQL Database-exemplaar.

    Zie het artikel voor meer informatie over het maken van een VNet in Azure portal [een virtueel netwerk maken met de Azure-portal](https://aka.ms/DMSVnet).

6. Selecteer een prijscategorie. Zorg dat u de prijscategorie Premium selecteert voor deze onlinemigratie.

    Zie voor meer informatie over de kosten en prijscategorieën de [Pagina met prijzen](https://aka.ms/dms-pricing).

     ![Instellingen configureren van een Azure Database Migration Service-exemplaar](media/tutorial-rds-sql-to-azure-sql-and-managed-instance/dms-settings3.png)

7. Selecteer **Maken** om de dienst te maken.

## <a name="create-a-migration-project"></a>Maak een migratieproject

Nadat de service is gemaakt, zoek deze op in de Azure-portal, open hem en maak vervolgens een nieuw migratieproject.

1. Selecteer in de Azure-portal **Alle diensten**, zoek naar Azure Database Migration Service, en selecteer vervolgens **Azure Database Migration Service**.

      ![Zoek alle exemplaren van de Azure Database Migration Service](media/tutorial-rds-sql-to-azure-sql-and-managed-instance/dms-search.png)

2. Zoek in het scherm **Azure Database Migration service** naar de naam van de Azure Database Migration Service-instantie die u hebt gemaakt en selecteer vervolgens het exemplaar.

     ![Zoek uw exemplaar van de Azure Database Migration Service](media/tutorial-rds-sql-to-azure-sql-and-managed-instance/dms-instance-search.png)

3. Selecteer + **Nieuw migratieproject**.
4. Geef in het scherm **Nieuw migratieproject** een naam op voor het project, selecteer in het tekstvak **Bronservertype** de optie **AWS RDS voor SQL Server** en selecteer in het tekstvak **Doelservertype** de optie **Azure SQL Database**.

    > [!NOTE]
    > Type doelserver, selecteert u **Azure SQL Database** beheerd exemplaar voor het migreren naar zowel een Azure SQL Database singleton-database en ook over een Azure SQL Database.

5. Selecteer in de sectie **Het type activiteit kiezen** de optie **Onlinegegevensmigratie**.

    > [!IMPORTANT]
    > Zorg ervoor dat u selecteert **Online gegevensmigratie**; offline migraties worden niet ondersteund voor dit scenario.

    ![Azure Database Migration Service-project maken](media/tutorial-rds-sql-to-azure-sql-and-managed-instance/dms-create-project4.png)

    > [!NOTE]
    > U kunt ook **alleen maken in project** nu het migratieproject maken en uitvoeren van de migratie later opnieuw.

6. Selecteer **Opslaan**.

7. Selecteer **Maken en uitvoeren van de activiteit** om het project te maken en de migratieactiviteit uit te voeren.

    ![Een Azure Database Migration Service-activiteit maken en uitvoeren](media/tutorial-rds-sql-to-azure-sql-and-managed-instance/dms-create-and-run-activity1.png)

## <a name="specify-source-details"></a>Geef brondetails op

1. Geef in het scherm **Migratiebron** de details van de verbinding op voor het bron-SQL Server-exemplaar.

    Zorg ervoor dat u een Fully Qualified Domain Name (FQDN) gebruikt voor de naam van het bron-SQL Server-exemplaar.

2. Als u geen vertrouwd certificaat op de bronserver hebt geïnstalleerd, schakelt u het selectievakje **Servercertificaat vertrouwen** in.

    Wanneer er geen vertrouwd certificaat is geïnstalleerd, genereert SQL Server een zelfondertekend certificaat wanneer het exemplaar wordt gestart. Dit certificaat wordt gebruikt voor het versleutelen van de referenties voor clientverbindingen.

    > [!CAUTION]
    > SSL-verbindingen die zijn versleuteld met behulp van een zelfondertekend certificaat bieden geen sterke beveiliging. Ze zijn vatbaar voor man-in-the-middle-aanvallen. U moet niet vertrouwen op SSL met behulp van zelfondertekende certificaten in een productieomgeving of op servers die zijn verbonden met internet.

   ![Brondetails](media/tutorial-rds-sql-to-azure-sql-and-managed-instance/dms-source-details3.png)

## <a name="specify-target-details"></a>Doeldetails opgeven

1. Selecteer **Opslaan** en geef vervolgens in het scherm **Details migratiedoel** de details van de verbinding op voor de Azure SQL Database-doelserver. Dit is de vooraf ingerichte Azure SQL Database waarnaar het **AdventureWorks2012**-schema is geïmplementeerd met behulp van de DMA.

    ![Doel selecteren](media/tutorial-rds-sql-to-azure-sql-and-managed-instance/dms-select-target3.png)

2. Selecteer **Opslaan**, en klik vervolgens in het scherm **Toewijzen aan doeldatabases**, wijs de bron- en de doeldatabase voor de migratie toe.

    Als de doeldatabase de naam van de dezelfde database als de bron-database bevat, wordt in de Azure Database Migration Service de doeldatabase standaard geselecteerd.

    ![Toewijzen aan doeldatabases](media/tutorial-rds-sql-to-azure-sql-and-managed-instance/dms-map-targets-activity4.png)

3. Selecteer **Opslaan**in het scherm **Tabellen selecteren**, vouw de tabellenlijst uit en controleer de lijst met betrokken velden.

    De Azure Database Migration Service selecteert automatisch alle lege brontabellen die beschikbaar zijn op het Azure SQL Database-doelexemplaar. Als u tabellen die al gegevens bevatten terug wilt migreren, moet u expliciet de tabellen op dit scherm selecteren.

    ![Selecteer tabellen](media/tutorial-rds-sql-to-azure-sql-and-managed-instance/dms-configure-setting-activity4.png)

4. Selecteer **Opslaan** nadat u de volgende **Geavanceerde instellingen voor onlinemigratie** hebt ingesteld.

    | Instelling | Description |
    | ------------- | ------------- |
    | **Maximale aantal tabellen om parallel te laden** | Geeft het aantal tabellen aan dat DMS gelijktijdig tijdens de migratie uitvoert. De standaardwaarde is 5, maar deze kan worden ingesteld op een optimale waarde om te voldoen aan specifieke migratiebehoeften op basis van elke POC-migratie. |
    | **Wanneer de brontabel wordt afgekapt** | Geeft aan of DMS de doeltabel tijdens de migratie afkapt. Deze instelling kan nuttig zijn als een of meer tabellen zijn afgekapt als onderdeel van het migratieproces. |
    | **Instellingen voor LOB-gegevens (Large Objects, grote objecten) configureren** | Geeft aan of DMS onbeperkte LOB-gegevens migreert of de gemigreerde LOB-gegevens tot een specifieke grootte beperkt.  Wanneer er een limiet op de LOB-gegevens die zijn gemigreerd, wordt elke LOB-gegevens buiten deze limiet is afgekapt. Voor productiemigraties is het raadzaam om **Onbeperkte LOB-grootte toestaan** te selecteren om gegevensverlies te voorkomen. Selecteer om onbeperkte LOB-grootte toe te staan het selectievakje **Migrate LOB data in a single block when the LOB size is less than (KB) specified** (LOB-gegevens in één blok migreren wanneer de LOB-grootte kleiner is dan het opgegeven aantal KB) voor betere prestaties. |

    ![Geavanceerde instellingen voor onlinemigratie instellen](media/tutorial-rds-sql-to-azure-sql-and-managed-instance/dms-advanced-online-migration-settings.png)

5. Selecteer **Opslaan**, geef in het scherm **Migratieoverzicht** in het tekstvak **Naam activiteit** een naam op voor de migratieactiviteit en controleer het overzicht om ervoor te zorgen dat de bron- en doeldetails overeenkomen met wat u eerder hebt opgegeven.

    ![Migratieoverzicht](media/tutorial-rds-sql-to-azure-sql-and-managed-instance/dms-migration-summary.png)

## <a name="run-the-migration"></a>De migratie uitvoeren

* Selecteer **Migratie uitvoeren**.

    Het venster van de migratieactiviteit wordt weergegeven en de **Status** van de activiteit is **Initialiseren**.

    ![Activiteitsstatus - initialiseren](media/tutorial-rds-sql-to-azure-sql-and-managed-instance/dms-activity-status2.png)

## <a name="monitor-the-migration"></a>Bewaak de migratie

1. Selecteer in het scherm van de activiteitenmigratie **Vernieuwen** om de weergave bij te werken totdat de **Status** van de migratie als **Actief** wordt weergegeven.

2. Klik op een specifieke database om de migratiestatus voor de bewerkingen **Alle gegevens worden geladen** en **Incrementele gegevenssynchronisatie** te bekijken.

    ![Activiteitsstatus - actief](media/tutorial-rds-sql-to-azure-sql-and-managed-instance/dms-activity-in-progress.png)

## <a name="perform-migration-cutover"></a>Migratie-cutover uitvoeren

Nadat de eerste volledige lading is voltooid, worden de databases gemarkeerd als **Gereed voor cutover**.

1. Wanneer u klaar bent om de databasemigratie te voltooien, selecteert u **Cutover starten**.

    ![Cutover starten](media/tutorial-rds-sql-to-azure-sql-and-managed-instance/dms-start-cutover.png)

2. Zorg dat u alle transacties stopt die bij de brondatabase binnenkomen; wacht totdat de teller van **Wijzigingen in behandeling** op **0** staat.
3. Selecteer **Bevestigen** en selecteer vervolgens **Toepassen**.
4. Wanneer de databasemigratiestatus **Voltooid** toont, verbindt u uw toepassingen met de nieuwe doel-Azure SQL Database.

    ![Activiteitsstatus - voltooid](media/tutorial-rds-sql-to-azure-sql-and-managed-instance/dms-activity-completed.png)

## <a name="next-steps"></a>Volgende stappen

* Zie het artikel [Bekende problemen met en tijdelijke oplossingen voor onlinemigraties naar Azure SQL Database](known-issues-azure-sql-online.md) voor informatie over bekende problemen en beperkingen bij het uitvoeren van onlinemigraties naar Azure SQL Database.
* Zie het artikel [Wat is de Azure Database Migration Service?](https://docs.microsoft.com/azure/dms/dms-overview) voor informatie over de Azure Database Migration Service.
* Zie het artikel [Wat is de service Azure SQL Database?](https://docs.microsoft.com/azure/sql-database/sql-database-technical-overview) voor informatie over Azure SQL Database.
* Voor informatie over Azure SQL Database beheerde exemplaren, Zie de pagina [Azure SQL Database Managed Instance](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-index).
