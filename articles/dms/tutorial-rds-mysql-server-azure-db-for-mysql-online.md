---
title: 'Zelfstudie: Azure Database Migration Service gebruiken voor een online migratie van extern bureaublad-services MySQL met Azure Database voor MySQL | Microsoft Docs'
description: Leer hoe u een online migratie uitvoeren van RDS MySQL met Azure Database for MySQL met behulp van de Azure Database Migration Service.
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
ms.openlocfilehash: 09b90f4b53750b94c0ecee7290d6b5405c984ff9
ms.sourcegitcommit: 1289f956f897786090166982a8b66f708c9deea1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/17/2019
ms.locfileid: "67154870"
---
# <a name="tutorial-migrate-rds-mysql-to-azure-database-for-mysql-online-using-dms"></a>Zelfstudie: RDS MySQL migreren naar Azure Database for MySQL online met behulp van DMS

U kunt Azure Database Migration Service gebruiken voor het migreren van databases van een RDS MySQL-exemplaar naar [Azure Database for MySQL](https://docs.microsoft.com/azure/mysql/) terwijl de brondatabase online tijdens de migratie blijft. Met andere woorden, kan migratie worden bereikt met minimale downtime voor de toepassing. In deze zelfstudie, migreert u de **werknemers** voorbeelddatabase van een exemplaar van RDS MySQL met Azure Database voor MySQL met behulp van de online migratie-activiteit in Azure Database Migration Service.

In deze zelfstudie leert u het volgende:
> [!div class="checklist"]
> * De voorbeeldschema migreren met behulp van de opdrachtregelprogramma's mysqldump en mysql.
> * Maak een instantie van Azure Database Migration Service.
> * Een migratieproject maken met behulp van Azure Database Migration Service.
> * De migratie uitvoeren.
> * De migratie controleren.

> [!NOTE]
> Als u Azure Database Migration Service gebruikt om een onlinemigratie uit te voeren, is het vereist dat u een exemplaar maakt op basis van de prijscategorie Premium. Zie de pagina met [prijzen](https://azure.microsoft.com/pricing/details/database-migration/) van Azure Database Migration Service voor meer informatie.

> [!IMPORTANT]
> Voor een optimale migratie-ervaring raadt Microsoft u aan een exemplaar van de Azure Database Migration Service te maken in dezelfde Azure-regio als de doeldatabase. Het verplaatsen van gegevens naar regio's of geografieën kan het migratieproces vertragen en fouten veroorzaken.

[!INCLUDE [online-offline](../../includes/database-migration-service-offline-online.md)]

In dit artikel wordt beschreven hoe u een online migratie uitvoeren van een exemplaar van RDS MySQL met Azure Database voor MySQL.

## <a name="prerequisites"></a>Vereisten

Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

* Zorg ervoor dat de bron-MySQL-server een ondersteunde versie van de MySQL-community wordt uitgevoerd. Als u wilt bepalen welke versie van uw MySQL-exemplaar in het hulpprogramma mysql of MySQL Workbench, voert u de opdracht uit:

    ```
    SELECT @@version;
    ```

    Zie voor meer informatie het artikel [Azure Database voor MySQL-versies ondersteund](https://docs.microsoft.com/azure/mysql/concepts-supported-versions).

* Download en installeer de [MySQL **werknemers** voorbeelddatabase](https://dev.mysql.com/doc/employee/en/employees-installation.html).
* Maak een instantie van [Azure Database for MySQL](https://docs.microsoft.com/azure/mysql/quickstart-create-mysql-server-database-using-azure-portal).
* Een Azure Virtual Network (VNet) maken voor Azure Database Migration Service met behulp van het Azure Resource Manager-implementatiemodel, waarmee u site-naar-site-verbinding met uw on-premises bronservers met behulp van [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) of [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways). Zie voor meer informatie over het maken van een VNet, de [documentatie voor Virtual Network](https://docs.microsoft.com/azure/virtual-network/), en met name de artikelen met vindt u meer details.
* Zorg ervoor dat uw VNet netwerkbeveiligingsgroepsregels de volgende poorten voor binnenkomende communicatie naar Azure Database Migration Service niet blokkeren: 443, 53, 9354, 445 en 12000. Zie het artikel voor meer informatie over Azure VNet NSG wordt verkeer gefilterd, [netwerkverkeer filteren met netwerkbeveiligingsgroepen](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg).
* Configureer uw [Windows Firewall voor toegang tot de database-engine](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access).
* Open uw Windows-firewall voor Azure Database Migration Service toegang tot de bron MySQL-server, die standaard TCP-3306 poort.
* Wanneer u een firewallapparaat gebruikt voor de brondatabase(s), moet u mogelijk firewallregels toevoegen om voor de Azure Database Migration Service toegang tot de brondatabase(s) voor de migratie toe te staan.
* Maken van een op serverniveau [firewallregel](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) voor de Azure Database for MySQL-server voor Azure Database Migration Service toegang tot de doeldatabase. Geef het subnetbereik van het VNet gebruikt voor Azure Database Migration Service.

> [!NOTE]
> Azure Database for MySQL ondersteunt alleen InnoDB-tabellen. Als u wilt converteren MyISAM-tabellen naar InnoDB, Zie het artikel [tabellen uit MyISAM converteren naar InnoDB](https://dev.mysql.com/doc/refman/5.7/en/converting-tables-to-innodb.html) .

### <a name="set-up-aws-rds-mysql-for-replication"></a>AWS RDS MySQL instellen voor replicatie

1. Voor het maken van een nieuwe parametergroep, volg de instructies die worden geleverd door AWS in het artikel [de logboekbestanden van de MySQL-Database](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_LogAccess.Concepts.MySQL.html), in de **binaire logboekindeling** sectie.
2. Maak een nieuwe parametergroep met de volgende configuratie:
    * binlog_format = rij
    * binlog_checksum = NONE
3. De nieuwe parametergroep opslaan.

## <a name="migrate-the-schema"></a>Het schema migreren

1. Het schema ophalen uit de brondatabase en van toepassing op de doeldatabase om de migratie van alle objecten in de database, zoals tabelschema, indexen en opgeslagen procedures te voltooien.

    De eenvoudigste manier voor het migreren van alleen het schema is mysqldump gebruiken met de--niet-data-parameter. De opdracht voor het migreren van het schema is:

    ```
    mysqldump -h [servername] -u [username] -p[password] --databases [db name] --no-data > [schema file path]
    ```
    
    Om bijvoorbeeld te dumpbestand van een schema voor de **werknemers** database, gebruikt u de volgende opdracht uit:
    
    ```
    mysqldump -h 10.10.123.123 -u root -p --databases employees --no-data > d:\employees.sql
    ```

2. Importeer het schema in target-service, Azure Database for MySQL. Als u wilt herstellen het dumpbestand schema, moet u de volgende opdracht uitvoeren:

    ```
    mysql.exe -h [servername] -u [username] -p[password] [database]< [schema file path]
    ```

    Bijvoorbeeld, voor het importeren van het schema voor de **werknemers** database:

    ```
    mysql.exe -h shausample.mysql.database.azure.com -u dms@shausample -p employees < d:\employees.sql
    ```

3. Als u refererende sleutels in uw schema hebt, mislukken de eerste lading en doorlopende synchronisatie van de migratie. Als u wilt ophalen van het script drop refererende sleutel en refererende sleutels script toevoegen op de bestemming (Azure Database voor MySQL), voer het volgende script in MySQL Workbench:

    ```
    SET group_concat_max_len = 8192;
        SELECT SchemaName, GROUP_CONCAT(DropQuery SEPARATOR ';\n') as DropQuery, GROUP_CONCAT(AddQuery SEPARATOR ';\n') as AddQuery
        FROM
        (SELECT
        KCU.REFERENCED_TABLE_SCHEMA as SchemaName,
        KCU.TABLE_NAME,
        KCU.COLUMN_NAME,
        CONCAT('ALTER TABLE ', KCU.TABLE_NAME, ' DROP FOREIGN KEY ', KCU.CONSTRAINT_NAME) AS DropQuery,
        CONCAT('ALTER TABLE ', KCU.TABLE_NAME, ' ADD CONSTRAINT ', KCU.CONSTRAINT_NAME, ' FOREIGN KEY (`', KCU.COLUMN_NAME, '`) REFERENCES `', KCU.REFERENCED_TABLE_NAME, '` (`', KCU.REFERENCED_COLUMN_NAME, '`) ON UPDATE ',RC.UPDATE_RULE, ' ON DELETE ',RC.DELETE_RULE) AS AddQuery
        FROM INFORMATION_SCHEMA.KEY_COLUMN_USAGE KCU, information_schema.REFERENTIAL_CONSTRAINTS RC
        WHERE
          KCU.CONSTRAINT_NAME = RC.CONSTRAINT_NAME
          AND KCU.REFERENCED_TABLE_SCHEMA = RC.UNIQUE_CONSTRAINT_SCHEMA
      AND KCU.REFERENCED_TABLE_SCHEMA = ('SchemaName') Queries
      GROUP BY SchemaName;
    ```

4. Voer de drop refererende sleutel (dit is de tweede kolom) in het queryresultaat verwijderen van de refererende sleutel.

5. Als u triggers (insert of update trigger) in de gegevens hebt, wordt deze de integriteit van gegevens in de doel-afdwingen voordat het repliceren van gegevens van de bron. De aanbeveling is om uit te schakelen van triggers in alle tabellen *op de doelopslaglocatie* tijdens de migratie en schakelt u vervolgens de triggers nadat de migratie is voltooid.

    Triggers in doeldatabase uitschakelen:

    ```
    select concat ('alter table ', event_object_table, ' disable trigger ', trigger_name)
    from information_schema.triggers;
    ```

6. Als er exemplaren van het type ENUM gegevens in tabellen, kunt u het beste tijdelijk bijwerken naar het gegevenstype 'teken uiteenlopende' in de doeltabel. Wanneer replicatie van gegevens voltooid is, klikt u vervolgens het type enum te herstellen.

## <a name="register-the-microsoftdatamigration-resource-provider"></a>Registreer de Microsoft.DataMigration-resourceprovider

1. Meld u aan bij de Azure-portal, selecteer **Alle services** en selecteer vervolgens **Abonnementen**.

   ![Portal-abonnementen weergeven](media/tutorial-rds-mysql-server-azure-db-for-mysql-online/portal-select-subscription1.png)

2. Selecteer het abonnement waarin u wilt maken van het exemplaar van Azure Database Migration Service, en selecteer vervolgens **resourceproviders**.

    ![Resourceproviders weergeven](media/tutorial-rds-mysql-server-azure-db-for-mysql-online/portal-select-resource-provider.png)

3. Zoek naar migratie en selecteer rechts van **Microsoft.DataMigration** de optie **Registreren**.

    ![Resourceprovider registreren](media/tutorial-rds-mysql-server-azure-db-for-mysql-online/portal-register-resource-provider.png)

## <a name="create-an-instance-of-azure-database-migration-service"></a>Maak een instantie van Azure Database Migration Service

1. Selecteer in de Azure-portal **Een resource maken**, zoek naar Azure Database Migration Service, en selecteer vervolgens **Azure Database Migration Service** uit de vervolgkeuzelijst.

    ![Azure Marketplace](media/tutorial-rds-mysql-server-azure-db-for-mysql-online/portal-marketplace.png)

2. Selecteer in het scherm **Azure Database Migration Service** **Maken**.

    ![Azure Database Migration Service-exemplaar maken](media/tutorial-rds-mysql-server-azure-db-for-mysql-online/dms-create1.png)
  
3. Geef in het scherm **Migratieservice maken** een naam op voor de service, het abonnement en een nieuwe of bestaande resourcegroep.

4. Selecteer de locatie waarin u wilt maken van het exemplaar van Azure Database Migration Service.

5. Selecteer een bestaand VNet of maak een nieuwe.

    Het VNet biedt Azure Database Migration Service toegang tot de bron-MySQL-exemplaar en de doel-Azure Database voor MySQL-exemplaar.

    Zie het artikel voor meer informatie over het maken van een VNet in Azure portal [een virtueel netwerk maken met de Azure-portal](https://aka.ms/DMSVnet).

6. Selecteer een prijscategorie; Zorg dat u selecteert u de Premium voor deze online migratie: 4vCores prijscategorie.

    ![Instellingen configureren van een Azure Database Migration Service-exemplaar](media/tutorial-rds-mysql-server-azure-db-for-mysql-online/dms-settings3.png)

7. Selecteer **Maken** om de dienst te maken.

## <a name="create-a-migration-project"></a>Maak een migratieproject

Nadat de service is gemaakt, zoek deze op in de Azure-portal, open hem en maak vervolgens een nieuw migratieproject.

1. Selecteer in de Azure-portal **Alle diensten**, zoek naar Azure Database Migration Service, en selecteer vervolgens **Azure Database Migration Service**.

      ![Zoek alle exemplaren van de Azure Database Migration Service](media/tutorial-rds-mysql-server-azure-db-for-mysql-online/dms-search.png)

2. Zoek in het scherm **Azure Database Migration service** naar de naam van de Azure Database Migration Service-instantie die u hebt gemaakt en selecteer vervolgens het exemplaar.

     ![Zoek uw exemplaar van de Azure Database Migration Service](media/tutorial-rds-mysql-server-azure-db-for-mysql-online/dms-instance-search.png)

3. Selecteer + **Nieuw migratieproject**.
4. Op de **nieuw migratieproject** scherm, Geef een naam voor het project in de **bronservertype** tekstvak, selecteer **MySQL**, en klik vervolgens in de **doel servertype** tekstvak, selecteer **AzureDbForMySQL**.
5. Selecteer in de sectie **Het type activiteit kiezen** de optie **Onlinegegevensmigratie**.

    > [!IMPORTANT]
    > Zorg ervoor dat u selecteert **Online gegevensmigratie**; offline migraties worden niet ondersteund voor dit scenario.

    ![Azure Database Migration Service-project maken](media/tutorial-rds-mysql-server-azure-db-for-mysql-online/dms-create-project6.png)

    > [!NOTE]
    > U kunt ook **alleen maken in project** nu het migratieproject maken en uitvoeren van de migratie later opnieuw.

6. Selecteer **Opslaan**.

7. Selecteer **Maken en uitvoeren van de activiteit** om het project te maken en de migratieactiviteit uit te voeren.

    > [!NOTE]
    > Controleer of u een notitie van de vereisten die nodig zijn voor het instellen van online migratie in de blade voor het maken van project.

## <a name="specify-source-details"></a>Geef brondetails op

* Op de **migratiebron** scherm, geef de details van de verbinding voor de bron-MySQL-exemplaar.

   ![Brondetails](media/tutorial-rds-mysql-server-azure-db-for-mysql-online/dms-source-details5.png)

## <a name="specify-target-details"></a>Doeldetails opgeven

1. Selecteer **opslaan**, en klik vervolgens op de **gericht op details** scherm, geef de details van de verbinding voor de doel-Azure Database voor MySQL-server, die vooraf is ingericht en is de **werknemers** geïmplementeerd met behulp van MySQLDump schema.

    ![Doel selecteren](media/tutorial-rds-mysql-server-azure-db-for-mysql-online/dms-select-target5.png)

2. Selecteer **Opslaan**, en klik vervolgens in het scherm **Toewijzen aan doeldatabases**, wijs de bron- en de doeldatabase voor de migratie toe.

    Als de doel-database de naam van de dezelfde database als de bron-database bevat, selecteert Azure Database Migration Service de doeldatabase standaard.

    ![Toewijzen aan doeldatabases](media/tutorial-rds-mysql-server-azure-db-for-mysql-online/dms-map-targets-activity5.png)

3. Selecteer **Opslaan**, geef in het scherm **Migratieoverzicht** in het tekstvak **Naam activiteit** een naam op voor de migratieactiviteit en controleer het overzicht om ervoor te zorgen dat de bron- en doeldetails overeenkomen met wat u eerder hebt opgegeven.

    ![Migratieoverzicht](media/tutorial-rds-mysql-server-azure-db-for-mysql-online/dms-migration-summary2.png)

## <a name="run-the-migration"></a>De migratie uitvoeren

* Selecteer **Migratie uitvoeren**.

    Het venster van de migratieactiviteit wordt weergegeven en de **Status** van de activiteit is **Initialiseren**.

## <a name="monitor-the-migration"></a>Bewaak de migratie

1. Selecteer in het scherm van de activiteitenmigratie **Vernieuwen** om de weergave bij te werken totdat de **Status** van de migratie als **Actief** wordt weergegeven.

    ![De Status van de activiteit - uitgevoerd](media/tutorial-rds-mysql-server-azure-db-for-mysql-online/dms-activity-status4.png)

2. Onder **DATABASENAAM**, selecteert u een specifieke database om te gaan naar de migratiestatus van voor **alle gegevens worden geladen** en **incrementele gegevenssynchronisatie** bewerkingen.

    **Alle gegevens worden geladen** toont de status van de migratie laden terwijl **incrementele gegevenssynchronisatie** toont gegevens vastleggen (CDC) status wijzigen.

    ![Scherm van de inventaris - volledige gegevens laden](media/tutorial-rds-mysql-server-azure-db-for-mysql-online/dms-inventory-full-load.png)

    ![Inventaris scherm - incrementele gegevenssynchronisatie](media/tutorial-rds-mysql-server-azure-db-for-mysql-online/dms-inventory-incremental.png)

## <a name="perform-migration-cutover"></a>Migratie-cutover uitvoeren

Nadat de initiële volledig laden is voltooid, de databases zijn gemarkeerd **gereed voor Cutover**.

1. Wanneer u klaar bent om de databasemigratie te voltooien, selecteert u **Cutover starten**.

    ![Knippen opnieuw beginnen](media/tutorial-rds-mysql-server-azure-db-for-mysql-online/dms-inventory-start-cutover.png)

2. Zorg dat u alle transacties stopt die bij de brondatabase binnenkomen; wacht totdat de teller van **Wijzigingen in behandeling** op **0** staat.
3. Selecteer **Bevestigen** en selecteer vervolgens **Toepassen**.
4. Wanneer de status van de database-migratie voor de status **voltooid**, verbinding maken met uw toepassingen naar de nieuwe doel-Azure Database voor MySQL-database.

Uw online migratie van een on-premises exemplaar van MySQL met Azure Database voor MySQL is nu voltooid.

## <a name="next-steps"></a>Volgende stappen

* Zie het artikel [Wat is de Azure Database Migration Service?](https://docs.microsoft.com/azure/dms/dms-overview) voor informatie over de Azure Database Migration Service.
* Zie het artikel voor informatie over Azure Database voor MySQL, [wat is Azure Database for MySQL?](https://docs.microsoft.com/azure/mysql/overview).
* Voor andere vragen, e-de [Azure Databasemigraties vragen](mailto:AskAzureDatabaseMigrations@service.microsoft.com) alias.
